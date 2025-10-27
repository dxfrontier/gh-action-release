<h2> gh-action-release </h2>

This GitHub Action creates a release based on the SemVer (major.minor.patch) labels from Pull Requests and automatically updates the package.json version.

## Table of Contents

- [Table of Contents](#table-of-contents)
- [Prerequisites](#prerequisites)
  - [GitHub Labels](#github-labels)
  - [GitHub Token Requirements](#github-token-requirements)
    - [Using a Custom Token with Write Access (Recommended)](#using-a-custom-token-with-write-access-recommended)
- [Usage](#usage)
  - [Parameters](#parameters)
  - [Examples](#examples)
    - [Example 1: Basic Setup with Default GITHUB_TOKEN](#example-1-basic-setup-with-default-github_token)
    - [Example 2: Using Custom PAT Token with Auto-merge](#example-2-using-custom-pat-token-with-auto-merge)
- [Contributing](#contributing)
- [License](#license)
- [Authors](#authors)

## Prerequisites

### GitHub Labels

Ensure you have the following `SemVer GitHub labels` created in your repository:

- `version: major`
- `version: minor`
- `version: patch`

> [!NOTE]
> These labels will determine the version bump type for each release.

### GitHub Token Requirements

This action requires a GitHub token with **write access** to your repository.

#### Using a Custom Token with Write Access (Recommended)

For this action to work properly, you need a GitHub token with **write access**. Create a Personal Access Token (PAT) with the following scopes and store it as a secret (e.g., `GITHUB_WRITE_ACCESS`):

**Required Token Scopes:**

- `repo` - Full control of private repositories
- `workflow` - Update GitHub Action workflows
- `write:packages` - Upload packages to GitHub Package Registry

**Setup Instructions:**

1. Go to GitHub Settings → Developer settings → Personal access tokens → Tokens (classic)
2. Click "Generate new token (classic)"
3. Select the scopes listed above
4. Copy the token and add it to your repository secrets as `GITHUB_WRITE_ACCESS`

**Workflow Permissions:**

Ensure your workflow also has the required permissions:

```yaml
permissions:
  contents: write
  pull-requests: write
```

`Example`:

```yaml
permissions:
  contents: write
  pull-requests: write

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - uses: dxfrontier/gh-action-release@main
        with:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          BRANCH: main
          ENABLE_AUTO_MERGE: 'false' # Disable auto-merge with default token
```

## Usage

Add the following as a job in your GitHub Actions workflow:

```yaml
- name: Bump version and create release
  uses: dxfrontier/gh-action-release@main
  with:
    GITHUB_TOKEN: ${{ secrets.GITHUB_WRITE_ACCESS }}
    BRANCH: main
```

> [!NOTE]
> Replace `GITHUB_WRITE_ACCESS` with your custom token secret name if you named it differently. See [GitHub Token Requirements](#github-token-requirements) for setup instructions.

### Parameters

| Parameter            | Type    | Required | Description                                                                                                                                                                      |
| -------------------- | ------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `GITHUB_TOKEN`       | string  | ✅ Yes   | GitHub token used for tagging, releases, and pushes. Recommended: `${{ secrets.GITHUB_WRITE_ACCESS }}` (must have write access and scopes `repo`, `workflow`, `write:packages`). |
| `BRANCH`             | string  | ❌ No    | Target branch for version bump and PR base. Default: `main`.                                                                                                                     |
| `WRITE_ACCESS_TOKEN` | string  | ❌ No    | Personal Access Token with write access for PR operations and auto-merge. Falls back to `GITHUB_TOKEN` if not provided.                                                          |
| `ENABLE_AUTO_MERGE`  | boolean | ❌ No    | Enable automatic PR merge after version bump. Default: `true`.                                                                                                                   |
| `ENABLE_SYNC_MTA`    | boolean | ❌ No    | Enable syncing version to mta.yaml and manifest.json files. Default: `true`.                                                                                                     |

### Examples

#### Example 1: Basic Setup with Default GITHUB_TOKEN

```yaml
permissions:
  contents: write
  pull-requests: write

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Bump version and create release
        uses: dxfrontier/gh-action-release@main
        with:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          BRANCH: main
```

> [!NOTE]
> This example uses the default `GITHUB_TOKEN` which is automatically provided by GitHub Actions.

> [!TIP]
> Is recommended to use on pull_request instead of on commit to master/main like in [Example 2](#example-2-using-custom-pat-token-with-auto-merge)

<p align="right">(<a href="#table-of-contents">back to top</a>)</p>

#### Example 2: Using Custom PAT Token with Auto-merge

Below job will be executed when a `pull request` is closed and merged into `dev` branch. This example uses a custom Personal Access Token for enhanced permissions.

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
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Bump version and create release
        uses: dxfrontier/gh-action-release@main
        with:
          GITHUB_TOKEN: ${{ secrets.GITHUB_WRITE_ACCESS }}
          WRITE_ACCESS_TOKEN: ${{ secrets.GITHUB_WRITE_ACCESS }}
          BRANCH: dev
          ENABLE_AUTO_MERGE: 'true'
```

> [!IMPORTANT]
> When using a custom token (e.g., `GITHUB_WRITE_ACCESS`), ensure it has the following scopes:
>
> - `repo` - Full control of private repositories
> - `workflow` - Update GitHub Action workflows
> - `write:packages` - Upload packages to GitHub Package Registry

<p align="right">(<a href="#table-of-contents">back to top</a>)</p>

## Contributing

Pull requests are welcome. For major changes, please open an issue first
to discuss what you would like to change.

Please make sure to update tests as appropriate.

## License

![Licence](https://img.shields.io/github/license/Ileriayo/markdown-badges?style=for-the-badge)

Copyright (c) 2025 DXFrontier

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
