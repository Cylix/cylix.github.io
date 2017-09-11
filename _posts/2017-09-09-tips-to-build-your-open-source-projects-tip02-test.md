---
layout: post
title: "Tips to successfully build your open-source projects<br/>#2. Test, test and test again"
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
* **[Tip 2: Test, test and test again](/2017/09/09/tips-to-build-your-open-source-projects-tip02-test.html)**
* [Tip 3: Have a sexy README](/2017/09/09/tips-to-build-your-open-source-projects-tip03-sexy-readme.html)
* [Tip 4: Document your project](/2017/09/09/tips-to-build-your-open-source-projects-tip04-document.html)
* [Tip 5: Release the right way](/2017/09/09/tips-to-build-your-open-source-projects-tip05-release.html)
* [Tip 6: Get feedbacks](/2017/09/09/tips-to-build-your-open-source-projects-tip06-feedbacks.html)
* [Tip 7: Promote](/2017/09/09/tips-to-build-your-open-source-projects-tip07-promote.html)
* [Tip 8: Handle the issues](/2017/09/09/tips-to-build-your-open-source-projects-tip08-handle-issues.html)
* [Tip 9: Listen to your issues](/2017/09/09/tips-to-build-your-open-source-projects-tip09-listen-issues.html)
* [Tip 10: Don't merge every pull requests](/2017/09/09/tips-to-build-your-open-source-projects-tip10-pull-requests.html)


# Test, test and test again
Testing is an important part of the process of software engineering.
And while it is already important in a professional environment, it is even more important in an open-source context.

First, it makes you feel confident that your library is working as it should.
In the same way you want to avoid bugs in production, you do want to avoid that for your open-source project.
It is especially true for that kind of project because people will not necessarily report it when they encounter a bug: they might think this is an expected behavior, an un-supported feature or simply stop using your project (slowing down your project adoption if you want it to be used).

<img src="/assets/tips_to_build_your_open_source_projects/commitstrip_tests.jpg" title="commitstrip unit tests"/>

Second, if you want people to use your project, they will look for alternatives, then choose what is the most appropriate solution.
They will base their choice on different factors: features, size, popularity, ...
But basically, for two libraries with equivalent features, what will make the difference is whether the person feel confident into including your project into their project. And to be confident, they are looking at whether your library is stable, widely used and tested.

Third, not providing tests decreases your maintainability or will undoubtedly lead to regressions at some points.

Last, but not least: you make it harder to contribute to your project.
When people want to contribute, they usually use part of your project and just want to improve a specific behavior or fix a bug.
Only you knows how your entire project works and you must not assume that others will.
Contributors will simply code the stuff they need and might break other existing components without noticing it. And you might not even notice it during the code review.

<img src="/assets/tips_to_build_your_open_source_projects/break_tests.jpg" title="break tests"/>

I know tests are boring and no one wants to code them, but do it to avoid the worst.
And again, you don't have any deadlines except the ones you defined, so take some time and don't rush for features: test them first.

Additionally, I advise you to setup an integration server that automatically compile you project (if necessary) and run you test whenever you push new code to the repository and whenever someone opens a pull request.

I personally use [Travis](travis-ci.org), but there are plenty of alternatives such as Jenkins, Circle or Shippable. Most of them are SaaS, entirely free if your project is open-sourced, easy to setup, with a nice UI and extremely useful: why not using them?

<img src="/assets/tips_to_build_your_open_source_projects/travis.png" title="travis"/>
<img src="/assets/tips_to_build_your_open_source_projects/travis_pr.png" title="travis"/>
<div style="width:100%;font-style:italic;text-align:center">Note the automatic Github integration of Travis once it is setup</div>
<br/>

### Read next tip: [Tip 3: Have a sexy README](/2017/09/09/tips-to-build-your-open-source-projects-tip03-sexy-readme.html)
