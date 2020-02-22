This is an example repo using the version-docs github action

# How to install version-docs in your repository

## Setup your doc generation utility

Within this example repository we are using typedoc, however there are many other tools that might fit your needs better. Some of the other popular ones include:

- https://www.mkdocs.org/
- http://www.sphinx-doc.org/en/master/index.html
- https://vuepress.vuejs.org/
- https://docusaurus.io/

## Create the following github actions workflow file

```
name: Version Docs
on: push
jobs:
  CI:
    runs-on: ubuntu-latest
    env:
      CI: true # this is needed to ignore husky hooks
    steps:
    - uses: actions/checkout@master
    - uses: actions/setup-node@master
      with:
        node-version: 13.5
    - name: Install dependencies
      run: yarn install
    - name: Generate docs
      run: yarn docs # or your repo-specific command to generate documentation in a folder named 'docs' at the root of your repo
    - name: Version docs
      uses: bobrown101/version-docs@v2.1.2
      with:
        github-token: ${{ secrets.GITHUB_TOKEN }}
        doc-location: docs
        doc-branch: gh-pages
```

# FAQ

## `sh: 1: cannot open /dev/tty: No such device or address`

This will occur if you use a tool like husky to install git hooks, and commitizen to help create conventional commit messages. These are helpful for local development, but on CI servers they sometimes get in the way. In our case it prevents version-docs from committing your docs to the docs-branch. The solution? Set the enviornment variable CI=true for your jobs

```
name: Version Docs
on: push
jobs:
  CI:
    runs-on: ubuntu-latest
    env:
      CI: true # this is needed to ignore husky hooks
```

Husky will check for the enviornment variable CI, and if present it will skip installing git hooks
