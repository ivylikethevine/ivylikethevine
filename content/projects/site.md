+++
title = 'Deploying a Modern Website'
date = '2023-10-14'
subtitle = 'A lightweight, professional workflow.'
author = 'Ivy Duggan'
draft = false
tags = [
    'CI/CD',
    'hugo',
    'cloudflare',
    'github',
    'devops',
]
+++

![image](/img/parked1.jpg "text Source: <a href=''>source</a>")
<https://pages.github.com/>
<https://gohugo.io/>
<https://jekyllrb.com/>
<https://pages.cloudflare.com/>
<https://mermaid.live/>

# Sustainable Software

As a contractor, I saw many projects that were built to be temporary. On smaller projects, it is appealing to bootstrap, to run fast & loose with rules, but a production-style architecture is not hard to implement, and worth it when designing a lasting solution.

## The Fallacy of Saving Time

Let me give an example. At one company I did work for, it took at minimum, 2 developers 4 hours each to deploy our application (at a *generous* minimum). Here's how that math works out over just a single year:

{{< math >}}
<p>
Inline math: \(\varphi = \dfrac{1+\sqrt5}{2}= 1.6180339887…\)
</p>
{{</ math >}}

![image](/dev-hours.png)

That's over a month of dev hours lost per year! Does any dev want to waste a month every single year running the same deployments?[^1] It doesn't even come close to stopping there, here are some other downsides I have seen firsthand:

- Issues in production cannot be immediately rolled back or hotfixed.
  - Picture the immense stress of both the main production site having a major error, fixing the major error, and effectively deploying under all of that pressure -- its not fun!
  - Exploits in production may be open for longer, increasing exposure time.
- Onboarding new devs is much slower than normal.
  - I once spent 2-3 weeks on configuring my dev system with a project, and this was not abnormal.
- If a system is already hard to deploy with current setup, migration of any technology will likely require immense work.

## Building Software

Alternatively, a well architected software system (including not just the "code", but the systems that hold, deploy, test, update, and document that entire product), is worth the initial setup if the product will exist for more than, say a month. For something like my website, here is a basic list of dev ops problems we need to solve:

1. Source Version Management
1. Continuous Integration
1. Continuous Deployment
1. Staging Site
1. Production Site
1. Easy rollbacks
1. Built-in Safeguards

### i. Source Version Mangement (svm/git)

`git` is very powerful and very complex, but we only need a few commands to set up a useable CI/CD environment.

{{< mermaid >}}
gitGraph
    commit
    branch develop
    checkout develop
    branch add-feature
    checkout add-feature
    commit
    commit
    checkout develop
    branch fix-bug
    checkout fix-bug
    commit
    checkout develop
    merge add-feature
    checkout main
    merge develop
    checkout develop
    branch new-page
    commit
    checkout develop
    merge fix-bug
    merge new-page
    checkout main
    merge develop
{{< /mermaid >}}

This website is made with Hugo, a static site generator written in Go. It has a theme called Puppet, and it is deployed to a custom domain for production, a password protected staging domain, and follows git flow with CI/CD for staging and production.

This system only costs the price of my domain registration (~10USD per year), and takes only a few hours to setup. Some real world benefits:

- Private staging site with access control for previewing changes
- Cloudflare analytics, DDoS protection, automatic SSL/HTTPS
- Built-in checks at the commit & merge level
- One command deployment to all servers
- Ability to use cloudflare tunnels to securely access homelab resources without exposing ports

## Github vs Cloudflare Pages

- Migration tip: rename & drop `github.io` beforehand

### Custom Domains & DNS Pains

- Route 53
- Cloudflare[^1]
- Github Pages

Nameservers  & CNAME & A/AAAA

#### Staging & Production

# Simple git flow

# Lock dev site behind zero trust

###### Where in the world is my workflow?

##### Footnotes

[^1]: Even assuming the [median software developer salary in the US](https://www.payscale.com/research/US/Job=Software_Developer/Salary), that is almost 7,000USD lost entirely to deployments every single year, as a generous underestimate.
