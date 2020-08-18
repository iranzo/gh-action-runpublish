# GitHub RUN&Publish Action

This action runs `build.sh` and pushes the generated content to Git Hub Pages site.

## Environment variables

- `GH_PAGES_BRANCH` (optional): override the default `gh-pages` deployment branch
- `SOURCE_FOLDER` (optional): Set path to use as document root `content` is the default configuration file

## Setup

Create a `.github/workflow/runpublish.yml` like:

```yml
name: Run build.sh and publish to GHPages

on:
  push:
    branches:
      - master
  schedule:
    - cron: "0 * * * *"

jobs:
  runpublish:
    runs-on: ubuntu-16.04
    steps:
      - uses: actions/checkout@v2

      # Use GitHub Actions' cache to shorten build times and decrease load on servers
      - uses: actions/cache@v2.1.0
        with:
          path: ~/.cache/pip
          key: ${{ runner.os }}-pip-${{ hashFiles('**/requirements/*') }}
          restore-keys: |
            ${{ runner.os }}-pip-

      - uses: iranzo/gh-action-runpublish@0.0.2
        env:
          GITHUB_TOKEN: ${{secrets.GITHUB_TOKEN}}
          SOURCE_FOLDER: content
```

Adjust your `build.sh` for additional commands:

```sh
#!/bin/bash
mkdir -p output
echo "Whatever" >  content/mypage.html
```

Commit and let the GitHub Actions take care of it!
