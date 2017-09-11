---
layout: post
title: "Tips to successfully build your open-source projects<br/>#10. Don't merge every pull requests"
date: 2017-09-09 20:46:00 +0100
description: Some tips based on my experience and failures about how to successfully build an open-source project
cover: /assets/header_image.jpg
tags: open-source english
author: Simon Ninon
---

# Summary
* [Intro](/2017/09/09/tips-to-build-your-open-source-projects-intro.html)
* [Tip 0: Why should you open-source your project?](/2017/09/09/tips-to-build-your-open-source-projects-tip00-why.html)
* [Tip 1: Don't rush features, Code quality matters](/2017/09/09/tips-to-build-your-open-source-projects-tip01-quality.html)
* [Tip 2: Test, test and test again](/2017/09/09/tips-to-build-your-open-source-projects-tip02-test.html)
* [Tip 3: Have a sexy README](/2017/09/09/tips-to-build-your-open-source-projects-tip03-sexy-readme.html)
* [Tip 4: Document your project](/2017/09/09/tips-to-build-your-open-source-projects-tip04-document.html)
* [Tip 5: Release the right way](/2017/09/09/tips-to-build-your-open-source-projects-tip05-release.html)
* [Tip 6: Get feedbacks](/2017/09/09/tips-to-build-your-open-source-projects-tip06-feedbacks.html)
* [Tip 7: Promote](/2017/09/09/tips-to-build-your-open-source-projects-tip07-promote.html)
* [Tip 8: Handle the issues](/2017/09/09/tips-to-build-your-open-source-projects-tip08-handle-issues.html)
* [Tip 9: Listen to your issues](/2017/09/09/tips-to-build-your-open-source-projects-tip09-listen-issues.html)
* **[Tip 10: Don't merge every pull requests](/2017/09/09/tips-to-build-your-open-source-projects-tip10-pull-requests.html)**


# Don't merge every pull requests
The most rewarding thing when you built an open-source project is when you start having people contributing to it! It is truly exciting and it always make me happy, even tiny contributions!

But be careful, when you are new, you might tend to accept all incoming pull requests. Why? Because people coded something that work and improve your project, so why not merging it now?
I did that mistake for a long time and I regret it.

When you receive a new pull requests, you have to think about different aspect:

#### 1. Does it work?
This is straightforward: does the change work without breaking everything? Having tests help ;)

#### 2. Documentation
This thing is mostly for feature addition: does the contributor explain in the pull request how to use it and what are all the details to understand about it.
That would help you to do the code review having everything in mind, but also to save time when updating the documentation

#### 3. Code quality
Is the code meeting your quality expectations?

This is an important point and that is not easy to handle: someone is contributing and it is cool, but the code might not be super OK. Problem: you don't want to piss off the person that spent time improving the project.

This does not mean you should merge shitty code.
Instead, you can guide the contributor by telling him how to change the code to make it match your expectations.
If you are not willing to tell the contributor or want to merge it ASAP, then you can simply improve the given change by yourself

<img src="/assets/tips_to_build_your_open_source_projects/commitstrip_maintainability.jpg" title="commitstrip maintainability"/>

#### 4. Code consistency
This is a really important point: multiple people will contribute, but everyone has its own coding style.
You should not let anyone submit something with a different coding style than the one you defined for your library or you will end up with weird code mixing different styles.
This is fine at the beginning, but very disgusting after a while.

I personally use an automatic formatter like clang-format. It is not exactly reflecting how I code, but I can tune it and it helps me to ensure a sane consistent code base.

<img src="/assets/tips_to_build_your_open_source_projects/inconsistency.jpg" title="code inconsistency"/>

#### 5. What is good for others
Great, someone coded a new feature that solved some needs for him.
But is it relevant for other users?
Is the design that has been chosen relevant or not?

If it needs some improvements or if it has nothing to do in the project, don't merge it or re-work it.
I made the mistake of merging a bit everything including irrelevant stuff or poorly designed one (the design might solve the needs of one but confuse the others for example), and it only leads to degrading your project quality.

So yes, it is great to receive pull requests.
But you need to spend an appropriate time to review them, test them and eventually re-work them.
This is a real art to do such a thing while not disappointing the contributor, but that's important if you don't want to end-up with a weird project that does everything and nothing at the same time.

I personally like the idea of having a project where the maintainer has a clear idea of where the project should go.
Of course, that does not mean the maintainer should be closed-minded and not open to any feedback or discussion.
But it means that at the end, there is one person that take the decision about how it should be done and whether it is relevant or not.

<img src="/assets/tips_to_build_your_open_source_projects/zuck.jpg" title="I'm CEO bitch"/>

Yes, it is open source, but you are the project owner and you define the rules, don't forget it!
