<h2> gh-action-release </h2>

This GitHub Action creates a release based on the SemVer (major.minor.patch) labels from Pull Requests and automatically updates the package.json version.

## Table of Contents

- [Table of Contents](#table-of-contents)
- [Prerequisites](#prerequisites)
- [Usage](#usage)
  - [Parameters](#parameters)
  - [Examples](#examples)
    - [Example 1](#example-1)
    - [Example 2](#example-2)
- [Contributing](#contributing)
- [License](#license)
- [Authors](#authors)

## Prerequisites

Ensure you have the following `SemVer GitHub labels` created in your repository:

- `version: major`
- `version: minor`
- `version: patch`

> [!NOTE]
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

### Examples

#### Example 1

```yaml
permissions:
  contents: write
  pull-requests: write

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - name: Bump version and create release
        uses: dxfrontier/gh-action-release@main
        with:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          BRANCH: main
```

> [!TIP]
> Is recommended to use on pull_request instead of on commit to master/main like in [Example 2](#example-2)

<p align="right">(<a href="#table-of-contents">back to top</a>)</p>

#### Example 2

Below job will be executed when a `pull request` is closed and merged into `dev` branch.

```yaml
name: Version and Release Workflow
on:
  pull_request:
    branches:
      - dev
    types:
      - closed

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

<p align="right">(<a href="#table-of-contents">back to top</a>)</p>

## Contributing

Pull requests are welcome. For major changes, please open an issue first
to discuss what you would like to change.

Please make sure to update tests as appropriate.

## License

![Licence](https://img.shields.io/github/license/Ileriayo/markdown-badges?style=for-the-badge)

Copyright (c) 2024 DXFrontier

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

## Authors

- [@mathiasvkaiz](https://github.com/mathiasvkaiz)
- [@dragolea](https://github.com/dragolea)
- [@sblessing](https://github.com/sblessing)
- [@ABS GmbH](https://www.abs-gmbh.de/) team

<p align="right">(<a href="#table-of-contents">back to top</a>)</p>
