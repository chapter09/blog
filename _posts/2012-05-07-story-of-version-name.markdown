---
author: chapter09
comments: true
date: 2012-05-07 02:34:51+00:00
layout: post
slug: story-of-version-name
title: Story of version name
wordpress_id: 679
categories:
- Tech
tags:
- Python
- version
---

The paragraph below is quoted from [http://packages.python.org/distribute/setuptools.html](http://packages.python.org/distribute/setuptools.html)

A version consists of an alternating series of release numbers and pre-release or post-release tags. A release number is a series of digits punctuated by dots, such as 2.4 or 0.5. Each series of digits is treated numerically, so releases 2.1 and 2.1.0 are different ways to spell the same release number, denoting the first subrelease of release 2. But 2.10 is the _tenth_ subrelease of release 2, and so is a different and newer release from 2.1 or 2.1.0. Leading zeros within a series of digits are also ignored, so 2.01 is the same as 2.1, and different from 2.0.1.

Following a release number, you can have either a pre-release or post-release tag. Pre-release tags make a version be considered _older_ than the version they are appended to. So, revision 2.4 is _newer_than revision 2.4c1, which in turn is newer than 2.4b1 or 2.4a1. Postrelease tags make a version be considered _newer_ than the version they are appended to. So, revisions like 2.4-1 and 2.4pl3 are newer than 2.4, but are _older_ than 2.4.1 (which has a higher release number).

A pre-release tag is a series of letters that are alphabetically before “final”. Some examples of prerelease tags would include alpha, beta, a, c, dev, and so on. You do not have to place a dot before the prerelease tag if it’s immediately after a number, but it’s okay to do so if you prefer. Thus, 2.4c1 and 2.4.c1 both represent release candidate 1 of version 2.4, and are treated as identical by setuptools.

In addition, there are three special prerelease tags that are treated as if they were the letter c: pre, preview, and rc. So, version 2.4rc1, 2.4pre1 and 2.4preview1 are all the exact same version as 2.4c1, and are treated as identical by setuptools.

A post-release tag is either a series of letters that are alphabetically greater than or equal to “final”, or a dash (-). Post-release tags are generally used to separate patch numbers, port numbers, build numbers, revision numbers, or date stamps from the release number. For example, the version 2.4-r1263 might denote Subversion revision 1263 of a post-release patch of version 2.4. Or you might use2.4-20051127 to denote a date-stamped post-release.

Notice that after each pre or post-release tag, you are free to place another release number, followed again by more pre- or post-release tags. For example, 0.6a9.dev-r41475 could denote Subversion revision 41475 of the in- development version of the ninth alpha of release 0.6. Notice that dev is a pre-release tag, so this version is a _lower_ version number than 0.6a9, which would be the actual ninth alpha of release 0.6. But the -r41475 is a post-release tag, so this version is _newer_ than 0.6a9.dev.


