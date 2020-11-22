### nunit3-junit -- Jochen Wezel

#### What is the purpose / what has been solved by this fork?

The reason for this fork is the requirement for a workaround for GitLab support in [nunit3-junit](https://github.com/jochenwezel/nunit-transforms-to-junit-gitlab-compatible/tree/master/nunit3-junit).

The GitLab UI doesn't show the test results with failure message if there is a failure stack trace, see https://gitlab.com/gitlab-org/gitlab-foss/-/issues/53668.

This fork resolves this issue by concatenating failure message and stacktrace into 1 single field.

#### Sample for use in .gitlab-ci.yml
```yml

test_job:
  before_script:
    - 'curl https://raw.githubusercontent.com/jochenwezel/nunit-transforms-to-junit-gitlab-compatible/master/nunit3-junit/nunit3-junit.xslt > nunit3-junit.xslt'
  script:
    - 'nuget install NUnit.Runners -Version 3.11.1 -OutputDirectory testrunner'
    - 'mono ./testrunner/NUnit.ConsoleRunner.3.11.1/tools/nunit3-console.exe "./My-Test/bin/Debug/My.Test.dll" --result=TestResult.JUnit.xml\;transform=nunit3-junit.xslt --result=TestResult.xml'
  artifacts:
    when: always  # save test results even when the task fails
    expire_in: 1 month  # save gitlab server space, we copy the files we need to deploy folder later on
    paths:
      - './TestResult.xml'  # saving NUnit results
      - './TestResult.JUnit.xml'  # saving NUnit results as Jenkins JUnit XML
    reports:
      junit:
        - './TestResult.JUnit.xml'  # saving NUnit results as Jenkins JUnit XML
```

### nunit3-xunit -- Paul Hicks

Converts NUnit3 results to JUnit-style results. It deliberately drops some information: the intention is to produce a results file suitable for publishing to Jenkins via the JUnit publisher.

The Jenkins NUnit publisher ("NUnit plugin" for Jenkins) requires NUnit2-style results and isn't keeping up with the snazziness of the JUnit plugin. Of particular interest to me, the JUnit plugin allows for claiming of individual test failures. XML files produced by transforming NUnit3 results with the attached XLST file are suitable for publishing via the JUnit plugin.

The transform is usually used via nunit-console's --result option:

    nunit3-console.exe YourTestAssembly.dll --result=junit-results.xml;transform=nunit3-junit.xslt

This transform is XSLT 1.0 compliant. It would be simpler if it used XSL 2.0; for example, the for-each loop in the test-suite template could be reduced to a simple string-join. XSLT 1.0 was chosen so that it can be used with Powershell, which currently (February 2016) supports only XSLT 1.0.

If you would like to run this using Powershell, here is a minimal Powershell script which you can run from Jenkins via the Powershell plugin. It uses the default NUnit output file name TestResults.xml and transforms it to a new file junit-results.xml, which you can then publish to Jenkins using the JUnit plugin.

    $xml = Resolve-Path TestResult.xml
    $output = Join-Path ($pwd) junit-results.xml
    $xslt = New-Object System.Xml.Xsl.XslCompiledTransform;
    $xslt.Load("nunit3-junit.xslt");
    $xslt.Transform($xml, $output);
    
