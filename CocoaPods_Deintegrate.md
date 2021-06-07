---
title: Get Rid of CocoaPods
---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Get Rid of CocoaPods](#get-rid-of-cocoapods)
  - [Preparation](#preparation)
  - [Deintegrate](#deintegrate)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Get Rid of CocoaPods

## Preparation

Install 2 gems

```bash
gem install cocoapods-deintegrate cocoapods-clean --user-install
```

## Deintegrate

```bash
pod deintegrate
pod clean
```

`Podfile` is only file left in workspaces.