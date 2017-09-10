---
layout: post
title: "Tips to successfully build your open-source projects"
date: 2017-09-09 20:46:00 +0100
description: Some tips based on my experience and failures about how to successfully build an open-source project
cover: /assets/header_image.jpg
tags: open-source english
author: Simon Ninon
---



# Some background

I don't like open-source. **I love it!**

Open-source brought me a lot: not only it helped me to find tools I needed for some projects just like everyone, but I learned a lot from it, from both technical and social aspect.
The open-source world is an interesting world where everyone can learn a lot from peers and since I started to contribute 5 years ago, I couldn't stop and I now spend a consequent part of my free time contributing to the open-source community.

Like many people, I first started to contribute to small libraries, then bigger ones once you get more comfortable, and ended up bringing some new projects I needed and was willing to share to others. If you are interested, you can check my [github](https://github.com/cylix) and leave me some feedbacks on projects that caught your attention!

I spent a lot of time on these projects, I learned a lot from them and I think it could be interesting to share part of my experience to help some other folks to successfully built their own new projects or to improve their own!

<!-- more -->



# Before Reading

Please note that this article is based on my own experience, my observations, and of course some of my failures.

It does not aim to stand as a reference, but instead to provide some tips or observations I wanted to share with others.
Maybe some tips are missing or some people will not agree with hints listed here: feel free to start a discussion in the comments!

This article is not about how to contribute to existing projects. Lots of other blog posts already talk about that subject very well.
This post is focusing on projects of your own. I've never really found articles to help me take the appropriate decisions and had no choice than to learn by myself.



# Why should you open-source your project?

This might sounds like an off topic question, but I believe that it is not for many readers, and thus a great place to start.

I know that many of my friends or co-workers (past, present and probably future ones) use a lot open-source projects to achieve whatever task they have to complete. It can range from the small JS library to fine-tune some web UI, to bigger frameworks like Ruby on Rails, including famous softwares like Apache. I don't know any developer not using open-source at some point, and I strongly believe that you'd be a fool not to do so (re-inventing the wheel or paying for it while it is there for free with all possible options and people to fix it!).

Many of those friends or co-workers have lots of projects in their mind and eventually code some of these small or bigger projects on their free-time!

But when it comes to open-source it, most of them can't step in and these projects stay on their own computer forever, most of the time half-developed.

I'm not talking about start-up ideas or whatever project people might have to build a company with and are not willing to share.
I'm talking about small libraries, tools, websites, ... that they needed or wanted to create at some point and that definitely can be shared to the open-source.

One reason could be because they are afraid to show their own code publicly.
Some others would think their project is not interesting or not useful.

That's a mistake I believe.
Every project has a purpose, is useful for some tasks and will eventually be interesting for some people out there even though you don't know them!

Publishing to open-source can bring you some suprises, make you meet new people and most-of-all, it will make you learn a lot.
There is no reason to be afraid to share your code to the open-source environment: no-one will blame you for anything, but instead thank you or advise you.
You might also help some people that where looking for what you built, or some others by giving them some ideas for their own projects.
Finally, I believe open-source can push you to finish a first draft your project instead of starting to build it and then leave it there half developed.

<img src="/assets/tips_to_build_your_open_source_projects/commitstrip.jpg" title="commitstrip"/>

Even a small contribution is meaningful, and once you started, you won't be able to stop :)

So share what you built, whatever it is!



# Don't rush features, Code quality matters

Code is what might scare most people to contribute to open-source.
That's a legitimate source of fear: you don't want to share some code that other people could use to judge you because it is poorly designed.

But that's actually what makes the strength of open-source: everyone can see your code!
From my own experience, most people won't give a shit about it and are just looking for the features.
But some other people will spend some time going through your code, either to debug your project, to implement new features, or sometimes just for learning and getting inspiration from it.

So yes, everyone can see your code, most people won't look at it but some will.
And as scary as it is, this is a very important and positive point: yes, you will need to pay extra attention to what you commit, but that's what will lead you to learn!

Code quality is important in the open-source, so take time to think!
You are not working for a company, you don't have deadlines except the one you defined: so there is no urge.

* How are you gonna implement this feature?
* How will you design your project?
* What are the best practices to complete your task?
* How to make your code cleaner and more efficient?
* What do you want to learn?
* ...

Sure, this takes time, but that's the most important part.
This is how you are going to learn the most on the technical aspect.
Of course you know how to develop most of the features. But are you developing in the right way? Are there better ways out there? What are they and what is the most appropriate.
You need to be able to justify every decision you make: don't just code it that way because that's the way you learned it.

<img src="/assets/tips_to_build_your_open_source_projects/best_practice.png" title="best practice strip"/>

And you know what? Because you spend time building your code, quality will improve and you won't be afraid to make it public.

Sure, there might still be a better way to do it or some people arguing you could have done differently, but that's not the point.
If you spent enough time, you should have learned new knowledge and your code will have a good quality, and that's the point!

Don't rush for features.
Take advantage of open source to take time to learn, build better code and be proud of it.



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

Third, not providing tests decreases your maintainability or will undoubtly lead to regressions at some points.

Last, but not least: you make it harder to contribute to your project.
When people want to contribute, they usually use part of your project and just want to improve a specific behavior or fix a bug.
Only you knows how your entire project works and you must not assume that others will.
Contributors will simply code the stuff they need and might break other existing components without noticing it. And you might not even notice it during the code review.

I know tests are boring and no one wants to code them, but do it to avoid the worst.
And again, you don't have any deadlines except the ones you defined, so take some time and don't rush for features: test them first.



# Have a sexy README

README is crucial: this is the first thing people will see and read when they find your library.
Whatever features you provide, however elegant your code is, most people will only look at that.

It sounds easy to write a good REAMDE at first, but I strongly believe most projects out there got it wrong (even really popular one).
The README template I use for my projects evolved a lot over the time and I only tend to keep it shorter and shorter and make it more attractive. I probably still can make it better, but I think I'm heading to the right direction.

Having a sexy README is then strategic: it might push people to start using it and help you to get feedbacks!
Make your library attractive by making your README attractive, slim and providing what people are looking for.

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
We are not looking for a full list of all the features, but more a global description that tells us immediately if we are looking are on the right page or not.

Yes, you have tons of features and tons of things to say about your project, but nobody really cares.
People will see your features in the documentation when they will need it, now they simply want to understand if your library could be a good match.

<img src="/assets/tips_to_build_your_open_source_projects/readme_desc.png" title="readme description"/>

#### 2. What does it look like when your project is used?
Here we are looking at some results: we want to see what it looks like to use your library, not code.
This part is easier for UI projects, lot harder for non-UI related libraries.

I personnally don't have one right now on cpp_redis, but I believe there is always a way to show it but this might require some thinking.

In my opinion, for this part, an image (possibly a GIF) is better than words.
Just show, don't explain.

#### 3. How to use your project?
Now, we want to know how to use your project: what are the pre-requisites and some example.

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
Dont copy paste the content of your license here, that's overwhelming.

<img src="/assets/tips_to_build_your_open_source_projects/readme_license.png" title="readme license"/>

#### 5. Some links to your documentation or additional details
Simply add any useful and relevant links to go deeper in the use of your library.
Documentation is a must-have, but you can also include links to explanations about how to contribute for example.

<img src="/assets/tips_to_build_your_open_source_projects/readme_doc.png" title="readme documentation"/>
<img src="/assets/tips_to_build_your_open_source_projects/readme_contrib.png" title="readme contributing"/>

#### That's all.
Most other information information are usually irrelevant.
You may of course include some additional information like credits or author information.

But keep it short and at the bottom: people don't care most of the time, except if it makes sense for your project.

<img src="/assets/tips_to_build_your_open_source_projects/readme_extra.png" title="readme extra"/>

Keep in mind that a good README is small, easily readable and contains all the essential information we are looking for.
People want to understand quickly what your project does, not get lost by going deep.
And don't forget to update it when your project evolves!



# Document your project

There is nothing worst than an open-source project with no documentation.
Don't expect people to open your code to understand how to use your project or discover functions: no one wants to do that.

There are two main ways to provide documentation: a wiki on Github and automatically generated documentation.
Until now, I always prefered the wiki way because you can guide the user and only show him what you want.
I've always found that automatically generated documentation is big, confusing and lots of information are unnecessary.

But I now believe that is one of my mistake: wiki is not enough and unmaintainable especially when you project tend to be bigger.

Instead you should mix both ways:

* Use the wiki to guide the user: how to install the library? How to use it? What are some detailed example? What are some important details to understand? What are the core features and functions of your project?
* Use the automatically generated documentation to allow people to find fine-grained information and discover your API and provide documentation for all released version of your project. It also force you to write functions and classes related documentation in your source files, making it easier to contribute to your project.

From my experience as a contributor, wiki alone is not maintainable for the project contributors and hard to version (for example, older versions of my projects are undocumented...).
From my experience as a developer, generated documentation alone is pointless. It does not help anyone to understand how to make it work and only force you to spend hours going through the documentation to find by myself what I was looking for.

Of course, keep both your wiki and your generated documentation up-to-date.

<img src="/assets/tips_to_build_your_open_source_projects/wiki_main.png" title="wiki main"/>

The following screenshot shows what I did manually and is not maintainable: probably move that to generated documentation:
<img src="/assets/tips_to_build_your_open_source_projects/wiki_doc.png" title="wiki doc"/>

# Release the right way

Avoid releasing by simply pushing to the master branch, and always keep stable versions on master and development versions somewhere else.

Your releases must be versioned: don't go custom and simply follow the standard [Samentic Versioning 2.0.0](http://semver.org/).
Basically, your versions should of the form x.y.z, where x defines a major release, y a minor one and z a patch.
Additionally, your release might include some suffix, like x.y.z-beta or whatever.

When you want to release a new version, determine what is the purpose of your release: is it a bugfix one, adding some new features, or changing a lot your project with possible breaking changes?
Depending on that, increase the major, minor or patch part of your version.

When you submit a new release, two important steps are important:
* Tag it on your git repository. This will make it easy to navigate from one release to another and it will automatically create a new release on github, allowing people to download tarball of each release.
* Update your CHANGELOG

<img src="/assets/tips_to_build_your_open_source_projects/release_list.png" title="release list"/>
<img src="/assets/tips_to_build_your_open_source_projects/release_details.png" title="release details"/>

The changelog is an important part and most people forget about it (including me for a long time).
But this file is really important: it is really useful for people upgrading your library and willing to know what has changed. These people are your project users, take care of them :)
It also helps you to know what has changed from one version to another without going through the git diff for hours.

<img src="/assets/tips_to_build_your_open_source_projects/changelog.png" title="changelog"/>

On my libraries, I usually have the main master branch on which I push all releases.
This is a good start, but this has some downsides I am currently facing: you can't fix older versions and you can't have multiple long time support versions at once.

From my observations, other libraries deal with it differently by having multiple branches, one per major release (or eventually minor release, though rarer).
Then, you can easily push patch on these versions by pushing on the corresponding branch and you can keep master as the latest available release.
I really like that approach and I plan to use it for my project in the future. It is also very easy to apply that approach even though you did not use it at first: you can simply create branch from older commits and you are done.




# Get feedbacks

Ok, great, you coded your project, did a perfect README and provided a great documentation. Your code is awesome and your idea looks cool!

But what do other people think about your project?
Try to collect some feedbacks to know what they think is interesting, but also what they dislike or think can be improved.
Some people will give you some hints about how to make your project even better or report some needs that your project could fulfill!

There are many ways to collect feedbacks:
* Your friends and co-workers. That's the easiest way, but it has some limits: your friends tend to be nice and won't necessarily be objective. They also might have no need in your project, so they might not understand what matters. But it is a good start, and you should do it.
* Specialized forum: this is sometimes harder to find because you might not necessarily know any forum for which your project is relevant. So it might require extra time to find some, but I believe the result must be rewarding. To be honest, I never did that one, but I plan to go that way for future projects
* Big websites such as reddit or Hacker News (Show HN). The main advantage of this one is that you get a huge audience, so more feedback easily. However, it might be a bit harder for someone to post on it because of potentially big exposure.

You have to filter feedbacks: which one are relevant, which one aren't (some people might have misunderstood what you did for example).
Don't apply every feedback in your roadmap: think and argue if it makes sense or not.
Discard the comments of grumpy people that believe they can do better than anyone and only bring non-constructive comments.

Feedback is great and help you to take a step back after spending so much time coding on your side for your own purpose. Be open-minded!

<img src="/assets/tips_to_build_your_open_source_projects/feedbacks.jpg" title="feedbacks"/>

# Promote

Most developers, including myself, always believe that a project itself is enough and people will rush to it without any promotion because everyone was waiting for it.
That's bullshit and you know it: if you don't talk about your project, in most cases, no one will notice it.
And it is too bad: you spent so much time putting everything you have on it, why would you want it to stay in the dark?

Spend some time promote it: talk about it to your friends, post it on some websites or even design a small logo for it.
I did that for cpp_redis for example: I checked where redis client are listed and discovered that there is an official list of all redis client available on redis.io, so I added my project there: people looking for a redis client might find mine by looking at that list while they would probably have never found it otherwise!

<img src="/assets/tips_to_build_your_open_source_projects/promote.png" title="promote"/>

Don't spend your time doing it, but still allocate a small amount to make it easier to find your project. There is no shame to promote your project!



# Handle the issues

If people start to use your project, you will end up having some people opening issues: stay calm, everything is fine!
As I heard once, "if your project don't have any bug, then maybe it's because no one is using it". So having bugs is, on some way, a good sign ;)

First, categorize the issue and its priority:
* is it a bug?
* is it a misuse of your project?
* is it some questions related to your project?

For the reported bug, this is a bit tricky: sometimes the issue is coming from the user code itself, not from your project.
The right way to handle it in my opinion is to make sure you have all the information you need:
* Which version of your project is used?
* What is the error?
* Is there some steps to reproduce the error or a small code snippet?

When I first started to get issues, I usually started to dig into it in the few minutes after it was opened to be as much reactive as possible and because I was stressed of having a bug.
But somehow, most of the time, the reporter of the issue was coming back after a few hours to say it was coming from its code and I wasted a few hours solving it.
Right now, I usually ask for more details and never dig into it until the next day except if it immediately appears as something wrong coming from my library. Trust me, many of my issues where closed by the reporter the same day and it saved me some time.

<img src="/assets/tips_to_build_your_open_source_projects/solved.png" title="solved"/>

Being reactive is important, but you don't need to be available 24/7: you are not paid, you are using your free time to build this project, don't be a slave of your own project.
On the contrary, don't let the issues getting accumulated, that would appear as a pretty bad sign to new-comers. If you really don't have time, simply put at the top of your README that you don't maintain it anymore or are looking for the help of other contributors.

Prioritize issues: I tend to respond immediately for questions or misuse of project because they are pretty fast to handle.
Concerning bugs, depending on your project, some will appear more critical and you might handle them first.

Always stay polite.
That sounds straightforward advice, but  if you project start to be used, you will start to receive issues you don't really want to address.
The type I personnally hate the most are people opening issues with only the title filled, no description, no hi or thanks. I always feel that kind of people disrespectful, but that's not a reason to act the same.

<img src="/assets/tips_to_build_your_open_source_projects/gross.png" title="gross"/>

Always close your issues when you are done or if no user replied to you for too long.
There is no point to keep your issues opened if you addressed them or can't address them due to lack of details.
Closing might sound rude and I had some difficulties to do it at first, especially for questions where you reply: you are technically done, but I was waiting for a day or two before closing. Now I simply close immediately when it is done and simply notify the user that he can re-open if he feels the need.

<img src="/assets/tips_to_build_your_open_source_projects/close.png" title="close"/>


# Listen to your issues.

As I said previously, you have different types of issues: bugs, misuse and questions.

You need to pay more attention than you think to the misuse and questions one.

If you see that one question come over and over again (how to use xxx? does your library support xxx?), it might mean that your documentation is not clear enough or an important feature is missing and would contitue a huge improvement of your library.

<img src="/assets/tips_to_build_your_open_source_projects/listen.png" title="listen"/>

If you see people misusing a particular part of your library often, maybe you should improve the documentation or adjust the design of your library to make it less error-prone.

Don't forget that only a few users will report issues that they encounters, so even if an issue is reported two or three times, it is already a sign and you should think about why people keep reporting it.
Even though the error might coming from their side at first sight, if it keep coming up, it might actually be shared responsabilities.

Listening to your issues means that you listen to your users and stay aware about what is going wrong with your project.



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
Is the code meeting your quality expectations.
This is an important point and that is not easy to handle: someone is contributing and it is cool, but the code might not be super OK. Problem: you don't want to piss off the person that spent time improving the project.
This does not mean you should merge shitty code.
Instead, you can guide the contributor by telling him how to change the code to make it match your expectations.
If you are not willing to tell the contributor or want to merge it ASAP, then you can simply improve the given change by yourself

<img src="/assets/tips_to_build_your_open_source_projects/commitstrip_maintainability.jpg" title="commitstrip maintainability"/>

#### 4. Code consistency
This is a really important point: multiple people will contribute, but everyone has its own coding style.
You should not let anyone submit something with a different coding style than the one you defined for your library or you will end up with weird code mixing different styles.
This is fine at the beginning, but very disguting after a while.
I personnally use an automatic formatter like clang-format. It is not exactly how I would personnally code, but you can tune it and you ensure a sane code base.

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
Yes, it is open source, but you are the project owner and you define the rules, don't forget it!


# Conclusion

Let me know what you think about these tips, I would be glad to read your opinion and discuss with you!
Also leave me a comment if you fill some tips are missing but worth knowing :)

I tried to cover everything based on my own experience but I might have forget some stuff or still lack of experience for some parts, let's talk about it!

If you want to learn more about my contributions, simply checkout my [github](https://github.com/cylix)
