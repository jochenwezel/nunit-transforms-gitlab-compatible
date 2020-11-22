# nunit-transforms
A collection of contributed XSLT transforms for use with the NUnit result file, organized in folders by contributor.

| Folder         | Purpose                                         | Author     |
|----------------|-------------------------------------------------|------------|
| [nunit3-junit](https://github.com/jochenwezel/nunit-transforms-to-junit-gitlab-compatible/tree/master/nunit3-junit) | Converts NUnit3 results to JUnit-style results. | Paul Hicks |
| [nunit3-summary](https://github.com/jochenwezel/nunit-transforms-to-junit-gitlab-compatible/tree/master/nunit3-summary) | Converts NUnit3 results to reports similar to those produced by the console runner. | Charlie Poole |
| [nunit2-summary](https://github.com/jochenwezel/nunit-transforms-to-junit-gitlab-compatible/tree/master/nunit2-summary) | Converts NUnit2 results to reports similar to those produced by the console runner. | Charlie Poole |

## What is the purpose / what has been solved by this fork?

The reason for this fork is the requirement for a workaround for GitLab support in [nunit3-junit].

The GitLab UI doesn't show the test results with failure message if there is a failure stack trace, see https://gitlab.com/gitlab-org/gitlab-foss/-/issues/53668.

This fork resolves this issue by concatenating failure message and stacktrace into 1 single field.
