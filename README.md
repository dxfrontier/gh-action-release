<h2> gh-action-release </h2>

This GitHub Action creates a release based on the SemVer (major.minor.patch) labels from Pull Requests and automatically updates the package.json version.

## Table of Contents

- [Table of Contents](#table-of-contents)
- [Prerequisites](#prerequisites)
- [Usage](#usage)
  - [Parameters](#parameters)
- [Example](#example)

## Prerequisites

Ensure you have the following `SemVer GitHub labels` created in your repository:

- `version: major`
- `version: minor`
- `version: patch`

> [NOTE]
> These labels will determine the version bump type for each release.

## Usage

Add the following as a job in your GitHub Actions workflow:

```yaml
- name: Bump version and create release
        uses: dxfrontier/gh-action-release@main
        with:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          BRANCH: main
```

### Parameters

| Parameter      | Description                                                                                                                                                                            |
| -------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `GITHUB_TOKEN` | GitHub token automatically provided by GitHub Actions to authenticate API requests for the repository. Ensure it has the necessary permissions to create releases and modify contents. |
| `BRANCH`       | The branch where the version bump and release will occur. Set this to the branch you want to release from, such as `main`, `dev`, or `qa`.                                             |

## Example

```yaml
permissions:
  contents: write
  pull-requests: write

jobs:
  version_release_job:
    runs-on: ubuntu-latest

    steps:
      - name: Bump version and create release
        uses: dxfrontier/gh-action-release@main
        with:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          BRANCH: main
```

> [!TIP]
> Is recommended to use on pull_request instead of on commit to master/main.
