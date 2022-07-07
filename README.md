# ltd-upload

A GitHub Action for uploading a website to Rubin Observatory's [LSST the Docs](https://sqr-006.lsst.io) documentation hosting platform.

**This actions requires you to set up Python,** either directly with [actions/setup-python](https://github.com/actions/setup-python) or indirectly through an action like [lsst-sqre/run-tox](https://github.com/lsst-sqre/run-tox).

## Example usage

```yaml
name: Python CI

"on":
  push:
    tags:
      - "*"
    branches:
      - "main"
  pull_request: {}

jobs:

  docs:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0 # full history for setuptools_scm

      - name: Build documentation
        uses: lsst-sqre/run-tox@v1
        with:
          python-version: "3.10"
          tox-envs: "docs"

      - name: Upload documentation
        uses: lsst-sqre/ltd-upload@v1
        with:
          project: "example"
          dir: "docs/_build/html"
          username: ${{ secrets.ltd_username }}
          password: ${{ secrets.ltd_password }}
```

## Inputs

- `project` (string, required) the name of the LTD project (e.g. "developer" for "developer.lsst.io").
- `dir` (string, required) the root directory path of the website build.
- `username` (string, required) the username for the LTD service account.
- `password` (string, required) the password for the LTD service account.

## Outputs

No outputs.

## Developer guide

This repository provides a **composite** GitHub Action, a type of action that packages multiple regular actions into a single step.
We do this to make the GitHub Actions workflows of all our software projects more consistent and easier to maintain.
[You can learn more about composite actions in the GitHub documentation.](https://docs.github.com/en/actions/creating-actions/creating-a-composite-action)

Create new releases using the GitHub Releases UI and assign a tag with a [semantic version](https://semver.org), including a `v` prefix. Choose the semantic version based on compatibility for users of this workflow. If backwards compatibility is broken, bump the major version.

When a release is made, a new major version tag (i.e. `v1`, `v2`) is also made or moved using [nowactions/update-majorver](https://github.com/marketplace/actions/update-major-version).
We generally expect that most users will track these major version tags.
