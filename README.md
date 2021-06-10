# Maven Version Bump Action

A simple GitHub Actions to bump the version of Maven projects.

When triggered, this action will look at the commit message of HEAD~1 and determine if it contains one of `#major`, `#minor`, or `#patch` (in that order of precedence).
If true, it will use Maven to bump your pom's version.

For example, a `#minor` update to version `1.3.9` will result in the version changing to `1.4.0`.
The change will then be committed.

## CHANGES

* POMPATH should include pom filename
*  -DgenerateBackupPoms=false -DprocessAllModules --file "${POMPATH}" added to mvn
* support prefix for version TAG
## Sample Usage

```yaml
name: Maven Version Bump

on:
  push:
    branches:
      - "release/**"

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
    - name: Checkout Latest Commit
      uses: actions/checkout@v2

    - name: Bump Version
      id: bump
      uses: E-Health/maven-version-bump-action@v4.3
      with:
        github-token: ${{ secrets.github_token }}

    - name: Print Version
      run: "echo 'New Version: ${{steps.bump.outputs.version}}'"
```

## Supported Arguments

* `github-token`: The only required argument. Can either be the default token, as seen above, or a personal access token with write access to the repository
* `git-email`: The email address each commit should be associated with. Defaults to a github provided noreply address
* `git-username`: The GitHub username each commit should be associated with. Defaults to `github-actions[bot]`
* `pom-path`: The full path to the root pom.xml.
* `version-prefix`: The prefix for the version tag.

## Outputs

* `version` - The after-bump version. Will return the old version if bump was not necessary.
