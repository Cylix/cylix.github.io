---
layout: post
title: "Quick Thoughts about ChatBots"
date: 2017-09-09 20:46:00 +0100
description: Quick thoughts about facebook messenger chatbots and facebook messenger for business
cover: /assets/header_image.jpg
tags: thoughts english
author: Simon Ninon
---

# What if we talk about the cons for once?

A few weeks ago, I had an interesting talk with some of my friends about the recent growth of chatbots on facebook, as well as the increasing number of companies opening communication channels on messenger.

<img src="/assets/quick_thoughts_about_chatbots/trend.png" title="Trend: FB for Messenger official page"/>

We are all recently graduated software engineers and quite had the same positive opinion concerning that trend: excitement!

I'm not going to detail all the possible pros coming with companies using messenger for direct communication and chatbots: there are loooots of blog posts about that already and for once this is not the purpose of that article!
Just to name a few: an easy way to reach out to companies, all in one place, support for wide variety of attachments, more companies switching to chat support such that you don't have to stay over the phone for hours, and so on...

Instead, I'm more gonna talk about some quick thoughts I had during that talk with my friends about the cons of these new tools.
I'm not gonna enter in any details and I don't pretend to speak the truth.
My only goal is to get other people thoughts on that topic, not just the typical "that's awesome".

Interestingly, while I often see some articles praising this trend on LinkedIn or Twitter, I never saw one post taking this approach.

<img src="/assets/quick_thoughts_about_chatbots/cartoon-lets-talk-about-jesus.jpg" title="Let's talk">

## Chatbots are the new Voicemails

I hate voicemails.

Most of the time I want to reach out to a company and have to do it by phone, I end up immediately on a voicemail throwing me a bunch of questions that I have to answer before being put on a waitlist to be connected to an operator.
And still, the experience is even worse than that:
* voicemails only accepting voice to answer the questions and do not understand what you say
* voicemails telling you to check the website that you did check before and hung up on you
* voicemails telling you the support is closed or too many people are calling and hung up on you
* communication dropped when you finally have an operator over the phone and you have to start all over again

<img src="/assets/quick_thoughts_about_chatbots/callcenter_commic.jpg" title="call center comic"/>

My first feeling is that chat is a way better alternative:
* you are connected directly to someone, so no more thousands of questions before being able to speak to someone
* no automated response to tell you to check the website
* if support is closed, you know it beforehand
* if support is too busy, you can still leave a message and wait
* if communication drops (internet drop or you close the tab), you can easily resume it

So when I first saw the trend of companies opening direct communications with customers using messenger or its API, I was pretty enthusiastic at the idea that voicemail would be dead soon!

But actually, after thinking a little bit, that excitement decreased a little bit: what if voicemails simply become chatbots.

Right now, chatbots are distinct from direct messaging (at least, I haven't met any case where both were mixed together): either you chat with someone, either you use a chatbot.
My personal believing is that it will merge sooner or later and we will end up with a chat window containing a chatbot throwing bunch of questions to be connected to someone and most of the time asking us to check the website out.

This is still gonna be an improvement as I don't believe we can't make as bad as the current voicemails.
Even with chatbots acting as voicemails, we won't have to say out loud multiple times "five-three-no-yes-my screen is broken-MY SCREEEN IS BROOOKEN" and once connected we still got all the advantages of chat messaging.

## Personal information

What concerns me the most is that now the barrier between your personal information and companies you are communicating with is getting thinner and thinner.

Even though I never considered Facebook as a private place but always as a public place (whatever you do on Facebook should be thought as something you do in public from my perspective), I'm really concerned with businesses being able to access my personal data without even telling me by using the messenger API.

Right now, after spending some time checking out the messenger API, it appears that a company using the messenger API can't access your facebook profile information: the user ID used for the messenger API is different from your facebook profile ID. Permissions should be asked explicitly.

<img src="/assets/quick_thoughts_about_chatbots/personal_info_access.png" title="messenger dev specifications"/>

<img src="/assets/quick_thoughts_about_chatbots/personal_info_available.png" title="available personal info"/>

But still. This is the current behavior, what about later? There is a huge lack of transparency and visibility for the user about what is shared to the company when you initiate a chat over messenger and even if you knew, it can change without you even notice.

I'm not even sure if it will ask your permission to share more information if you already agreed to share this information with that service in the past (simply to log in faster for example).

Would you send your facebook profile information to a car dealer before you come check out what he has to sell?
I believe you wouldn't, but that's maybe what we will indirectly do on messenger in the future, or maybe what will appear as the norm later on by giving more and more access to our personal information in contexts where it is irrelevant.

One would argue that it sounds paranoid. But things go one step at a time and we never really know in which direction and how far it will go.


## How will be used this information?

Directly linked to this point, I'm also wondering what could be done with data more easily accessible.

While I'm fine with facebook or google using my personal data to target me with specific ads, that's an entire story if personal data can be accessed when I chat from messenger.

The main reason is that in the first case, the company has never the hand on my data. It does not even know that I was targeted personally by their ad.

In the second case, you don't really know what the other company does: they might store it and allow random support people to access information that is irrelevant to what you are looking for while you are chatting with them on facebook.

<img src="/assets/quick_thoughts_about_chatbots/feed_personal_info.jpg" title="feed me more"/>

## Who am I talking to?

The last thought is probably not relevant in a close future.

With the development of chatbots but also AI, I feel it can be harder to know who you are talking to.
Is the person answering you a real person or just some AI?
Or simply, did the person dealing with your issue changed without you noticed it.

As long as I got what I'm looking for, I don't think this behavior is an issue.

But that's usually when something is not working at all that you are looking to talk to a real person and it is probably gonna harder and not as obvious to find out who is talking to you.

<img src="/assets/quick_thoughts_about_chatbots/learn_talk_human.jpg" title="learn to talk like a human"/>

Somehow, I feel it might also be more complicated to reach out to support people through the phone.

Even though I'm not a big fan of phone conversations, I feel phone has the advantage of being able to build something with the people you are talking to and probably help the person you are chatting with to have some empathy. That is really useful in some cases and we are probably going to lose that: way harder to transmit feelings through a chatbox, especially when the operator is chatting with 10 people at the same time.

## Conclusion

I'm still excited about that trend, but I feel it can be really interesting to put more thoughts on it.

Right now, there is nearly no transparency from a user point of view about how your data is used and transmitted through messenger for business.
I think it is important to get more information about it and probably think a bit more when using that kind of service, especially when we granted facebook profile access permission to so many services until now without putting that much thoughts on it most of the time, just because it is more convenient.
More convenient should never be tight to less privacy.

On the other hand, it is also interesting to think about what could be the evolution of communication in a near future, with chatbots mixed with direct communication and probably turning into voicemails.
Is it gonna be better? I believe so, even though it is probably not gonna be as ideal as we can picture it for now.

The goal of this article is to simply make people think about that trend too instead of keep praising these new tools.
I'm curious to know more about what are your thoughts on this, so do not hesitate to leave a comment, I'll read it :)
