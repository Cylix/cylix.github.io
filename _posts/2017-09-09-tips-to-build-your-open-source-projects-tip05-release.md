---
layout: post
title: "Tips to successfully build your open-source projects<br/>#5. Release the right way"
date: 2017-09-09 20:46:00 +0100
description: Some tips based on my experience and failures about how to successfully build an open-source project
cover: /assets/header_image.jpg
categories: open-source english
author: Simon Ninon
---

# Summary
* [Intro](/2017/09/09/tips-to-build-your-open-source-projects-intro.html)
* [Tip 0: Why should you open-source your project?](/2017/09/09/tips-to-build-your-open-source-projects-tip00-why.html)
* [Tip 1: Don't rush features, Code quality matters](/2017/09/09/tips-to-build-your-open-source-projects-tip01-quality.html)
* [Tip 2: Test, test and test again](/2017/09/09/tips-to-build-your-open-source-projects-tip02-test.html)
* [Tip 3: Have a sexy README](/2017/09/09/tips-to-build-your-open-source-projects-tip03-sexy-readme.html)
* [Tip 4: Document your project](/2017/09/09/tips-to-build-your-open-source-projects-tip04-document.html)
* **[Tip 5: Release the right way](/2017/09/09/tips-to-build-your-open-source-projects-tip05-release.html)**
* [Tip 6: Get feedbacks](/2017/09/09/tips-to-build-your-open-source-projects-tip06-feedbacks.html)
* [Tip 7: Promote](/2017/09/09/tips-to-build-your-open-source-projects-tip07-promote.html)
* [Tip 8: Handle the issues](/2017/09/09/tips-to-build-your-open-source-projects-tip08-handle-issues.html)
* [Tip 9: Listen to your issues](/2017/09/09/tips-to-build-your-open-source-projects-tip09-listen-issues.html)
* [Tip 10: Don't merge every pull requests](/2017/09/09/tips-to-build-your-open-source-projects-tip10-pull-requests.html)


# Release the right way

#### Version
Your releases must be versioned: don't go custom and simply follow the standard [Semantic Versioning 2.0.0](http://semver.org/).
Basically, your versions should of the form x.y.z, where:
* x defines a major release
* y a minor one
* z a patch

When you want to release a new version, determine what is the purpose of your release: is it a bug-fix one, adding some new features, or changing a lot your project with some breaking changes?
Depending on that, increase the major, minor or patch part of your version.

#### Tag
Tag your release on your git repository. This will make it easy to navigate from one release to another and it will automatically create a new release on github, allowing people to download tarball of each release.

<img src="/assets/tips_to_build_your_open_source_projects/release_list.png" title="release list"/>
<img src="/assets/tips_to_build_your_open_source_projects/release_details.png" title="release details"/>

#### Update your CHANGELOG
The changelog is an important part and most people forget about it (including me for a long time).
But this file is really important: it is really useful for people upgrading your library and willing to know what has changed. These people are your project users, take care of them :)
It also helps you to know what has changed from one version to another without going through the git diff for hours when you are debugging.

<img src="/assets/tips_to_build_your_open_source_projects/changelog.png" title="changelog"/>

#### Branches
On my libraries, I usually have the main master branch on which I push all releases.
This is a good start, but this has some downsides I am currently facing: you can't fix older versions and you can't have multiple long time support versions at once.

From my observations, other libraries deal with it differently by having multiple branches, one per major release (or eventually minor release, though rarer).
Then, you can easily push patch on these versions by pushing on the corresponding branch and you can keep master as the latest available release.
I really like that approach and I plan to use it for my projects in the future. It is also very easy to apply that approach even though you did not use it at first: you can simply create branch from older commits and you are done.

### Read next tip: [Tip 6: Get feedbacks](/2017/09/09/tips-to-build-your-open-source-projects-tip06-feedbacks.html)
