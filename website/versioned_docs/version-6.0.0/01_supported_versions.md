---
id: version-6.0.0-supported_versions
title: Supported Versions and Compatibility
sidebar_label: Supported Versions and Compatibility
copyright: (C) 2007-2021 GoodData Corporation
original_id: supported_versions
---

This article explains how different versions of GoodData.UI go through the life cycle phases and what level of support you can expect when using a specific version.

As a general recommendation, we encourage you to always use the latest available version of GoodData.UI to make the user experience with integrating GoodData.UI as smooth and secure as possible and to ensure that GoodData.UI always uses the latest features of the GoodData platform.

## Life cycle phases

Each version of GoodData.UI goes through the following phases:

1. **General Availability** (GA)
    * **When it starts:** When a major version is publicly released. To get notified about a new version, subscribe to the [Release Notes](https://support.gooddata.com/hc/en-us/sections/203564877).
    * **What it means:** A version in GA is going through active development, receives all new features and bug fixes, which are applied on top of the last minor version.

2. **End-of-Development** (EOD)
    * **When it starts:** When a newer major version is publicly released.
    * **What it means:** A version in EOD receives only security fixes (unless they can be resolved by a SemVer-compatible upgrade) and fixes for critical issues. Only production dependencies receive the security fixes. These fixes are applied on top of the last minor version. No new features are added.

3. **End-of-Support** (EOS)
    * **When it starts:** The date is defined by GoodData.
    * **What it means:** A version in EOS receives neither new features nor bug fixes. No technical support is provided. Although the version is still available on NPM, we do not recommend that you use it.

## Status of GoodData.UI versions

The following table provides the lifecycle phases of GoodData.UI versions:

| Major Version | Status              | GA               | EOD               | EOS               | Last Minor Version |
|:--------------|:--------------------|:-----------------|:------------------|:------------------|:-------------------|
| 8             | Generally available | October 8, 2020  | _Not yet defined_ | _Not yet defined_ | 8.12               |
| 7             | End-of-Support      | May 21, 2019     | October 8, 2020   | March 31, 2022    | 7.9                |
| 6             | End-of-Support      | November 1, 2018 | May 21, 2019      | October 8, 2020   | 6.3                |

## Compatibility with GoodData Cloud

The first version of GoodData.UI compatible with GoodData Cloud is 8.10.0, previous versions are not compatible with GoodData Cloud.

## Compatibility with GoodData.CN

The versions of GoodData.UI older than 8.3.0 are not compatible with GoodData.CN.
