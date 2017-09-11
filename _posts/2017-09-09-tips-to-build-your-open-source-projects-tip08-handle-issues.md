---
layout: post
title: "Tips to successfully build your open-source projects<br/>#8. Handle the issues"
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
* **[Tip 8: Handle the issues](/2017/09/09/tips-to-build-your-open-source-projects-tip08-handle-issues.html)**
* [Tip 9: Listen to your issues](/2017/09/09/tips-to-build-your-open-source-projects-tip09-listen-issues.html)
* [Tip 10: Don't merge every pull requests](/2017/09/09/tips-to-build-your-open-source-projects-tip10-pull-requests.html)


# Handle the issues

#### Stay calm
If people start to use your project, you will end up having some people opening issues: stay calm, everything is fine!
As I heard once, "if your project don't have any bug, then maybe it's because no one is using it". So having bugs is, on some way, a good sign ;)

#### Categorize, prioritize and collect information
First, categorize the issue and its priority:
* is it a bug?
* is it a misuse of your project?
* is it some questions related to your project?

Prioritize issues: I tend to respond immediately for questions or misuse of project because they are pretty fast to handle.
Concerning bugs, depending on your project, some will appear more critical and you might handle them first.

For reported bugs, this can be a bit tricky: sometimes the issue is coming from the user code itself, not from your project.
The right way to handle it in my opinion is to make sure you have all the information you need:
* Which version of your project is used?
* What is the error?
* Is there some steps to reproduce the error or a small code snippet?

#### Take your time
When I first started to get issues, I usually started to dig into it in the few minutes after it was opened to be as much reactive as possible and because I was stressed of having a bug.

But somehow, most of the time, the reporter of the issue was coming back after a few hours to say it was coming from its code and I wasted a few hours solving it.
Right now, I usually ask for more details and never dig into it until the next day except if it immediately appears as something wrong coming from my library. Trust me, many of my issues where closed by the reporter the same day and it saved me some time.

<img src="/assets/tips_to_build_your_open_source_projects/solved.png" title="solved"/>

Being reactive is important, but you don't need to be available 24/7: you are not paid, you are using your free time to build this project, don't be a slave of your own project.
On the contrary, don't let the issues getting accumulated, that would appear as a pretty bad sign to new-comers. If you really don't have time, simply put at the top of your README that you don't maintain it anymore or are looking for the help of other contributors.

#### Stay polite
Always stay polite.
That sounds straightforward advice, but  if you project start to be used, you will start to receive issues you don't really want to address.
The type I personally hate the most are people opening issues with only the title filled, no description, no hi or thanks. I always feel that kind of people disrespectful, but that's not a reason to act the same.

<img src="/assets/tips_to_build_your_open_source_projects/gross.png" title="gross"/>

#### Keep it clean
Always close your issues when you are done or if no user replied to you for too long.

There is no point to keep your issues opened if you addressed them or can't address them due to lack of details.
Closing might sound rude and I had some difficulties to do it at first, especially for questions where I simply replied with the answer: I'm technically done, but I was waiting for a day or two before closing. Now I simply close immediately when it is done and simply notify the user that he can re-open if he feels the need.

<img src="/assets/tips_to_build_your_open_source_projects/close.png" title="close"/>

### Read next tip: [Tip 9: Listen to your issues](/2017/09/09/tips-to-build-your-open-source-projects-tip09-listen-issues.html)
