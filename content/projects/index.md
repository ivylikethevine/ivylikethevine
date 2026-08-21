+++
title = 'Project Links'
slug = 'Projects'
date = '2026-07-25'
subtitle = 'Project Links'
description = 'Project Links'
comments = false
draft = false
toc = false
tags = [
    'projects',
]
+++

## ivylikethevine.com

![image](images/personal-site.png?width=750#center 'Woah, that looks just like this website...')

This entire site is statically built via hugo, with a set of github actions to automatically publish to a staging server as well as deploy this live version! To learn more, [read here](https://github.com/ivylikethevine/ivylikethevine).

## Saffron

![image](images/saffron-heimdall.png?width=750#center 'My current personal homelab dashboard, powered by Saffron.')

An open source, *S*erver as *a* *f*ile *f*older *r*unning *o*n *n*etwork built from docker compose files. I use this as the basis of my personal homelab, and have been developing it since I only had a stack of macbooks for a server rack! For details, [read my full post on it here](/posts/saffron-server-as-a-file-folder-running-on-network/) and the project is available here: [saffron](https://github.com/ivylikethevine/saffron)

## say-hi (nearing alpha release)

![image](images/say-hi-tab-complete.png?width=750#center 'Say hi to any host, anywhere, and have the same configuration.')

My solution to running a million different machines and environment, [say-hi](https://github.com/ivylikethevine/say-hi) is a project that unifies and git tracks configurations for all of the packages, systems, and commands that I use daily. It supports bash, zsh, and fish, and carries all of the configurations with it when you ssh to a new host. Instead of saying "shush", say "hi" :). I wrote about my process and the considerations I had [during the development here](/posts/say-hi)

## sharerr-rs (in active development)

![image](images/sharerr-rs.png?width=750#center 'Alpha UI.')

A less developed project, but much larger in scope. Sharerr-rs aims to allow users with homelab libraries to share their libraries with friends as easily and quickly as possible. [sharerr-rs](https://github.com/ivylikethevine/sharerr-rs) accomplishes this by interacting with existing *-arr apps, cataloging, and cross-seeding those files on a private tracker swarm.
