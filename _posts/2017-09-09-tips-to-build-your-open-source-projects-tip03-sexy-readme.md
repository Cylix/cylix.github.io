---
layout: post
title: "Tips to successfully build your open-source projects<br/>#3. Have a sexy README"
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
* **[Tip 3: Have a sexy README](/2017/09/09/tips-to-build-your-open-source-projects-tip03-sexy-readme.html)**
* [Tip 4: Document your project](/2017/09/09/tips-to-build-your-open-source-projects-tip04-document.html)
* [Tip 5: Release the right way](/2017/09/09/tips-to-build-your-open-source-projects-tip05-release.html)
* [Tip 6: Get feedbacks](/2017/09/09/tips-to-build-your-open-source-projects-tip06-feedbacks.html)
* [Tip 7: Promote](/2017/09/09/tips-to-build-your-open-source-projects-tip07-promote.html)
* [Tip 8: Handle the issues](/2017/09/09/tips-to-build-your-open-source-projects-tip08-handle-issues.html)
* [Tip 9: Listen to your issues](/2017/09/09/tips-to-build-your-open-source-projects-tip09-listen-issues.html)
* [Tip 10: Don't merge every pull requests](/2017/09/09/tips-to-build-your-open-source-projects-tip10-pull-requests.html)


# Have a sexy README
The README is crucial: this is the first thing people will see and read when they find your library.
Whatever features you provide, however elegant your code is, most people will only look at that.

It sounds easy to write a good README at first, but I strongly believe most projects out there got it wrong (even really popular one).
The README template I use for my projects evolved a lot over the time and I only tend to keep it shorter and shorter and make it more attractive. I probably still can make it better, but I think I'm heading to the right direction.

<img src="/assets/tips_to_build_your_open_source_projects/add_readme.png" title="add readme"/>

Having a sexy README is then strategic: it might push people to start using your project and help you to get feedbacks!
Make your library attractive by making your README sexy and including information people are looking for.

That's why it is important to understand what people are looking for when they are having a quick look on your project.

That's not super hard to find out and you can simply take yourself as an example: what are you looking for when you are searching for a library for your projects?

1. You first want to be sure you are at the right place: does it solve your needs?
2. You then want to check how it looks like in action
3. You want to have an idea of how to use it
4. If it seems it meets your needs, you want to make sure you can use it based on the license.
5. Finally, if everything looks good, you might consider to learn more by looking at some documentation somewhere else.

Here is a list of questions your README need to answer in a brief way. I'm gonna use my library [cpp_redis](https://github.com/cylix/cpp_redis) as an example (feel free to let me know some ideas of improvement!).

#### 1. What is the goal of your project and what can it be used for?
We want to understand what does your project do in 2 or 3 lines.
We are not looking for a full list of all the features, but more a global description that tells us immediately if we are looking the right page or not.

Yes, you have tons of features and tons of things to say about your project, but nobody really cares.
People will see your features in the documentation when they will need it, now they simply want to understand if your library could be a good match.

<img src="/assets/tips_to_build_your_open_source_projects/readme_desc.png" title="readme description"/>

#### 2. What does it look like when your project is used?
Here we are looking at some results: we want to see what it looks like to use your library without seeing any code.
This part is easier for UI projects, lot harder for non-UI related libraries.

I personally don't have one right now on cpp_redis, but I believe there is always a way to show it but this might require some thinking.

In my opinion, for this part, an image (possibly a GIF) is better than words.
Just show, don't explain.

#### 3. How to use your project?
Now, we want to know how to use your project: what are the pre-requisites and some examples.

The pre-requisites is a simple list of what is necessary to use your project.
This can be other libraries, languages, compilers, tools, ...
Usually, specifying the versions is important.

<img src="/assets/tips_to_build_your_open_source_projects/readme_prereq.png" title="readme prerequisites"/>

Example is important but be cautious: we only want a quick example that shows how to use the core features.
You don't want to show a long and complex example that takes minutes to go through and understand.
No! Instead, you want to provide 20 lines of codes that show only the important stuff, possibly with some short comments.
It should be easy to understand, and at the same time give a good idea about how this project can be used.
Again, put detailed examples somewhere else and simply put a link to it.

<img src="/assets/tips_to_build_your_open_source_projects/readme_examples.png" title="readme examples"/>

#### 4. Under which license is your project packaged?
Simply name your license and put a link to it.
Don't copy paste the content of your license here, that's overwhelming.

<img src="/assets/tips_to_build_your_open_source_projects/readme_license.png" title="readme license"/>

#### 5. Some links to your documentation or additional details
Simply add any useful and relevant links to go deeper in the use of your library.
Documentation is a must-have, but you can also include links to explanations about how to contribute for example.

<img src="/assets/tips_to_build_your_open_source_projects/readme_doc.png" title="readme documentation"/>
<img src="/assets/tips_to_build_your_open_source_projects/readme_contrib.png" title="readme contributing"/>

#### That's all.
Any other information is usually irrelevant for most projects.
You may of course include some additional information like credits or author.

But keep it short and at the bottom: people don't care most of the time, except if it makes sense for your project.

<img src="/assets/tips_to_build_your_open_source_projects/readme_extra.png" title="readme extra"/>

Keep in mind that a good README is small, easily readable and contains all the essential information we are looking for.
People want to understand quickly what your project does, not get lost by going deep.
And don't forget to update it when your project evolves!

Don't make the mistake to underestimate the README of your project: this is really the front page of your work and it deserves much more work than people tend to think.

### Read next tip: [Tip 4: Document your project](/2017/09/09/tips-to-build-your-open-source-projects-tip04-document.html)
