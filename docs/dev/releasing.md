# Release Process

This document guides maintainers through the process of creating an Envoy Gateway release.

## Creating a Minor Release

### Prerequisites

- Permissions to push to the Envoy Gateway repository.

### Set Environment Variables

Set environment variables for use in subsequent steps:

```shell
export MAJOR_VERSION=0
export MINOR_VERSION=3
export GITHUB_REMOTE=origin
```

1. Clone the repo, checkout the `main` branch, ensure it’s up-to-date, and your local branch is clean.
2. Create a topic branch to create the release notes. Reference previous [release notes][] for additional details.
3. Sign, commit, and push your changes to your fork and submit a [Pull Request][] to merge the release notes into the
   `main` branch.
4. Create a topic branch for the release announcement. Reference previous [release announcements][] for additional
   details.
5. Sign, commit, and push your changes to your fork and submit a [Pull Request][] to merge the release announcement
   into the `main` branch.
6. Create a topic branch and update the release tag references in the [Quickstart Guide][]. This should be the last
   commit to main before cutting the release.

   ```shell
   make update-quickstart TAG=${MAJOR_VERSION}.${MINOR_VERSION}.0
   ```

7. Sign, commit, and push your changes to your fork. Send a PR to get your changes merged into main. Do not proceed
   until all your PRs have merged and the Build and Test [release GitHub action][] has completed for your final PR.

   __Note:__ The Quickstart update should occur in the release branch after [Issue #632][] is resolved.

8. Pull the latest changes from the `main` branch that include commits from all the above PRs:

   ```shell
   git pull ${GITHUB_REMOTE} main
   ```

9. Create a new release branch from `main`. The release branch should be named
   `release/v${MAJOR_VERSION}.${MINOR_VERSION}`, e.g. `release/v0.3`.

   ```shell
   git checkout -b release/v${MAJOR_VERSION}.${MINOR_VERSION}
   ```

10. Push the branch to the Envoy Gateway repo.

    ```shell
    git push ${GITHUB_REMOTE} release/v${MAJOR_VERSION}.${MINOR_VERSION}
    ```

11. Tag the head of your release branch with the release tag. For example:

    ```shell
    git tag -a v${MAJOR_VERSION}.${MINOR_VERSION}.0 -m 'Envoy Gateway v${MAJOR_VERSION}.${MINOR_VERSION}.0 Release'
    ```

    __Note:__ The tag version differs from the release branch by including the `.0` patch version.

12. Push the tag to the Envoy Gateway repository.

     ```shell
     git push origin v${MAJOR_VERSION}.${MINOR_VERSION}.0
     ```

13. This will trigger the [release GitHub action][] that generates the release, release artifacts, etc.
14. Confirm that the [release workflow][] completed successfully.
15. Confirm that the Envoy Gateway [image][] with the correct release tag was published to Docker Hub.
16. Confirm that the [release][] was created.
17. Confirm that the steps in the [Quickstart Guide][] work as expected.
18. [Generate][] the GitHub changelog and include the following text at the beginning of the release page:

   ```console
   # Release Announcement

   Check out the [v${MAJOR_VERSION}.${MINOR_VERSION} release announcement]
   (https://gateway.envoyproxy.io/releases/v${MAJOR_VERSION}.${MINOR_VERSION}.html) to learn more about the release.
   ```

19. If you find any bugs in this process, please create an issue.

## Creating a Release Candidate

### Prerequisites

- Permissions to push to the Envoy Gateway repository.

### Set Environment Variables

```shell
export MAJOR_VERSION=0
export MINOR_VERSION=3
export RELEASE_CANDIDATE_NUMBER=1
export GITHUB_REMOTE=origin
```

1. Clone the repo, checkout the `main` branch, ensure it’s up-to-date, and your local branch is clean.
5. Tag the head of the main branch with the release candidate number.

   ```shell
   git tag -a v${MAJOR_VERSION}.${MINOR_VERSION}.0-rc.${RELEASE_CANDIDATE_NUMBER} -m 'Envoy Gateway v${MAJOR_VERSION}.${MINOR_VERSION}.0-rc.${RELEASE_CANDIDATE_NUMBER} Release Candidate'
   ```

6. Push the tag to the Envoy Gateway repository.

   ```shell
   git push v${MAJOR_VERSION}.${MINOR_VERSION}.0-rc.${RELEASE_CANDIDATE_NUMBER}
   ```

7. This will trigger the [release GitHub action][] that generates the release, release artifacts, etc.
8. Confirm that the [release workflow][] completed successfully.
9. Confirm that the Envoy Gateway [image][] with the correct release tag was published to Docker Hub.
10. Confirm that the [release][] was created.
11. Note that the [Quickstart Guide][] references are __not__ updated for release candidates. However, test
    the quickstart steps using the release candidate by manually updating the links.
12. [Generate][] the GitHub changelog.
13. Ensure you check the "This is a pre-release" checkbox when editing the GitHub release.
14. If you find any bugs in this process, please create an issue.

## Announcing the Release

It's important that the world knows about the release. Use the following steps to announce the release.

1. Set the release information in the Envoy Gateway Slack channel. For example:

   ```shell
   Envoy Gateway v${MAJOR_VERSION}.${MINOR_VERSION} has been released: https://github.com/envoyproxy/gateway/releases/tag/v${MAJOR_VERSION}.${MINOR_VERSION}.0
   ```

2. Send a message to the Envoy Gateway Slack channel. For example:

   ```shell
   On behalf of the entire Envoy Gateway community, I am pleased to announce the release of Envoy Gateway
   v${MAJOR_VERSION}.${MINOR_VERSION}. A big thank you to all the contributors that made this release possible.
   Refer to the official v${MAJOR_VERSION}.${MINOR_VERSION} announcement for release details and the project docs
   to start using Envoy Gateway.
   ...
   ```

   Link to the GitHub release and release announcement page that highlights the release.

[release notes]: https://github.com/envoyproxy/gateway/tree/main/release-notes
[Pull Request]: https://github.com/envoyproxy/gateway/pulls
[Quickstart Guide]: https://github.com/envoyproxy/gateway/blob/main/docs/user/quickstart.md
[release GitHub action]: https://github.com/envoyproxy/gateway/blob/main/.github/workflows/release.yaml
[release workflow]: https://github.com/envoyproxy/gateway/actions/workflows/release.yaml
[image]: https://hub.docker.com/r/envoyproxy/gateway/tags
[release]: https://github.com/envoyproxy/gateway/releases
[Generate]: https://docs.github.com/en/repositories/releasing-projects-on-github/automatically-generated-release-notes
[release announcements]: https://github.com/envoyproxy/gateway/blob/main/docs/releases/v0.2.md
[Issue #632]: https://github.com/envoyproxy/gateway/issues/632