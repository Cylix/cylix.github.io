---
layout: post
title: "Tips to successfully build your open-source projects<br/>#4. Document your project"
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
* **[Tip 4: Document your project](/2017/09/09/tips-to-build-your-open-source-projects-tip04-document.html)**
* [Tip 5: Release the right way](/2017/09/09/tips-to-build-your-open-source-projects-tip05-release.html)
* [Tip 6: Get feedbacks](/2017/09/09/tips-to-build-your-open-source-projects-tip06-feedbacks.html)
* [Tip 7: Promote](/2017/09/09/tips-to-build-your-open-source-projects-tip07-promote.html)
* [Tip 8: Handle the issues](/2017/09/09/tips-to-build-your-open-source-projects-tip08-handle-issues.html)
* [Tip 9: Listen to your issues](/2017/09/09/tips-to-build-your-open-source-projects-tip09-listen-issues.html)
* [Tip 10: Don't merge every pull requests](/2017/09/09/tips-to-build-your-open-source-projects-tip10-pull-requests.html)


# Document your project
There is nothing worst than an open-source project with no documentation.
Don't expect people to open your code to understand how to use your project or discover functions: no one wants to do that.

There are two main ways to provide documentation: a wiki on Github and automatically generated documentation.

Until now, I always preferred the wiki way because you can guide the user and only show him what you want.
I've always found that automatically generated documentation is big, confusing and lots of information are unnecessary.

But I now believe that is one of my mistakes: wiki is not enough and unmaintainable especially when you project tend to be bigger.

<img src="/assets/tips_to_build_your_open_source_projects/wiki_doc.png" title="wiki doc"/>
<div style="width:100%;font-style:italic;text-align:center">This is 100% NOT maintainable!</div>
<br/>

Instead you should mix both ways:

* Use the wiki to guide the user: how to install the library? How to use it? What are some detailed example? What are some important details to understand? What are the core features and functions of your project?

<img src="/assets/tips_to_build_your_open_source_projects/wiki_main.png" title="wiki main"/>

* Use the automatically generated documentation to allow people to find fine-grained information and discover your API and provide documentation for all released version of your project. It also force you to write functions and classes related documentation in your source files, making it easier to contribute to your project.

From my experience as a contributor, wiki alone is not maintainable for the project contributors and hard to version (for example, older versions of my projects are undocumented...).

From my experience as a developer, generated documentation alone is pointless. It does not help anyone to understand how to make it work and only force you to spend hours going through the documentation to find by myself what I was looking for.

That's why considering using both for your project sounds reasonable.

Of course, don't forget to keep both your wiki and your generated documentation up-to-date.


### Read next tip: [Tip 5: Release the right way](/2017/09/09/tips-to-build-your-open-source-projects-tip05-release.html)
