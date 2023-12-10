+++
title = 'Overcoming Jank with Design'
date = '2023-12-06'
subtitle = 'A realistic approach to software development.'
author = 'Ivy Duggan'
draft = true
tags = [
  'dev ops',
  'op-ed'
]
header_img = ''
description = ''
toc = true
categories = []
series = []
+++

# Code Doesn't Matter

In an ideal world, all code would be 100% unit tested and analyzed perfectly optimized C++, but in reality, most code is bad.

>Most Code Is Bad.

My favorite phrase I've ever heard in my work as a software developer was 'almost certainly suboptimal'. It's so perfect. It describes every single solution except for the Optimal One(s)[^1].

## Suboptimal Software

I'll say it. I deliver suboptimal software, we all do. There will be inherent flaws, limitations, expectations, and assumptions in every single system built. That is not why software development is hard.

Software development is hard because the skill is to mitigate risks before they happen, and to plan for features no one has thought of.

Here's it another way:

### No One Cares About Your Tech Stack

If you ask any website developer what they think about PHP, they'll probably tell you that we have better options now.

#### 42% of the entire internet is PHP[^2]

Customers and users have zero concern for what makes the internet tick. Bad tech choices exist everywhere, will continue to exist everywhere, and entirely new classes of bad tech choices have yet to be invented.

Working as a software developer means to work within and around all of the tech choices that all other developers have made and will make, ad infinitum. In a cognitive science perspective, when programmers work, we work with all other programmers who have touched the code we use. In a literal sense, we are using API's, toolchains, algorithms, drivers, and compilers that someone else created and saved, which was then duplicated onto our machines.

##### Mitigation

In software development, there's already the idea of defensive programming, which has been a personal lifesaver many times. I propose that this concept applies even more broadly, to the entire design and architecture of a system. From language, toolchain, and framework selection all the way to the naming of a file, every choice made should be made with as 'little faith' in them as possible.

###### Ye of Little Faith

I love JavaScript & TypeScript. It's my confession. But most of the time, it is a bad option. I also don't like Python, but many of my projects ARE in it. These choices are made because the needs of the project are more important than what I personally want.

'Little faith' means that I have little faith in any choice being perfect. It is indescribably important to be nimble, especially in the early stages of a project, and almost no project is made of a single technical choice. Computer Science classes love to talk about abstraction, but abstraction between the different layers of a project allows for easier replacement when different requirements arise. If we design our entire project as a system of parts, each of which can be described and build (more or less) independently of each other, then we can mitigate poor technical decisions by quite literally ripping that part out and replacing it.

Fully integrated systems with poor decisions are the downfall of companies. Technical debt is an exponential monster, but it is often somewhat bounded by the size of the system it inhabits. Here's an example from my work.

A client brought in a graphical planning/organizing application, and upon code inspection, the previous 'team' had written an entirely bespoke C++ web server as well as the business logic of the application. Ignoring for a moment the fact that Apache web server has been out since 1996, and nginx has been out since 2004, since the (horrible) web server was intimitately intertwined with the business logic, none of the previous work was salvagable.

##### In Upside Down Land

Let's imagine that instead of making a tragic decision to write a custom webserver in the 2010s, the previous team had made the slightly better but still aggravating decision to write the server in a theoretical Hell Language (imagine your most hated language, or [watch this](https://www.youtube.com/watch?v=vcFBwt1nu2U)). At least in that case, if the API was even marginally functional, we could derive some use from it, especially if the business logic was some magic formula.

Bad decisions will happen and we have to deal with them. When we can plan around them, not only do we gain the ability to compensate for issues, but also to push out every single drop of performance possible out of what we have.

The startup world, with its many flaws, was very good at proposing the idea of MVP: Minimal Viable Product. The concept is simple, if you're making a product, make it the MOST of THE quality that differentiates you from the competition. The most pickle-y pickle, or the most fast car.[sic]

##### Tech (& I) <3 Jank

Technology is one of the only industries that can thrive off of unconceivable jank. No one cares about the tech stack, and no one cares about the hardware stack. I've written about this before, in my LINK TO NAS POST, but I endorse using janky hardware and software WHEN it is properly architected. Datacenters are magic because they have 2 things:

1. Money (which you don't)
1. Well Configured Software and Hardware (which you can do yourself)

All systems in a data center are designed for when they fail. Apply that to your system and you can reach acceptable levels of uptime. I personally know someone who became a millionaire from a stack of bitcoin mining milk crates. I've seen terrible developers

learnxinyminutes.com

[^1]: Most problems likely have multiple solutions that are around equal in efficacy, but the point is the same: most solutions are suboptimal by definition.
[^2]: <http://colorlib.com/wp/wordpress-statistics/>
