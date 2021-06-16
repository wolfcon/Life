---
title: Auto Push to CocoaPod Trunk
---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Auto Push to CocoaPod Trunk](#auto-push-to-cocoapod-trunk)
  - [Get Token](#get-token)
  - [Set Secret in Github Repository](#set-secret-in-github-repository)
  - [Setup Github Action](#setup-github-action)
  - [Trigger Action](#trigger-action)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->


# Auto Push to CocoaPod Trunk

## Get Token

Register a new token account

```sh
pod trunk register your@email.com -- description "GitHub Actions Token"
```

Once you confirm email verification you can obtain it from private files. `~/.netrc` or use following command:

```sh
grep -A2 'trunk.cocoapods.org' ~/.netrc
```

## Set Secret in Github Repository

Copy  the token you've got and go `Settings->Secrets` of your repo. Add token to `Repository secrets` with `COCOAPODS_TRUNK_TOKEN` (can be other name paired with action `env`)

## Setup Github Action

this is an example:

```yml
name: Auto Push Trunk

on:
  release:
    types:
      - created

jobs:
  validation:
    name: Upload pods to Specs
    runs-on: macos-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v2
      - name: push trunk
        env:
          COCOAPODS_TRUNK_TOKEN: ${{ secrets.COCOAPODS_TRUNK_TOKEN }}
        run: |
          pod trunk push
          pod trunk me clean-sessions --all
```

Enjoy it🎉.

## Trigger Action

Just modify `on` section.

[Documents is here.](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#on)



