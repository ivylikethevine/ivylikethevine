+++
title = 'hi (or how I learned to love SSH)'
slug = 'hi (or how I learned to love SSH)'
date = '2026-07-22'
subtitle = 'Carrying your configs everywhere you go'
description = 'Carrying your configs everywhere you go'
comments = true
draft = true
toc = true
tags = [
    'dev-ops',
    'ssh',
    'config',
    'homelab'
]
+++

![image](images/hackerman.jpg?width=750#center "Hackerman, 'Kung Fury'")
## Alias Anger

Like a lot of developers (and a great many System Admins), I have to connect to a variety of machines for my job. Luckily, almost all of them are Linux or Mac, but even just amongst Linux machines, often I find myself in a strange new land with nothing but the muscle memory in my fingers.

Since I can't rely on my local configuration on my remote machines, this resulted in me just never using any aliases, helper functions, or configurations on any machine to reduce any chance of confusion. This is highly inefficient, and I sought a way to improve it.

### Pain Points

When you ssh to a host, you don't really know what the environment you'll arrive in will have. In my experience, the further your local environment is from a stock install, the more likely you will encounter the following paint points:

#### 1. The shell(s) themselves

This is the first problem I encountered. For a few years now, I've been using [`fish` shell](https://fishshell.com/), which is great **but** it is not technically POSIX-compliant. [POSIX](https://en.wikipedia.org/wiki/POSIX) is an IEEE standard, and the specifics are not that important. The consideration when using `fish` is *scripts that would execute fine in sh/bash/zsh may not run in fish*. 

Another consideration is that most stock systems will not have it installed by default.

#### 2. Aliases

Even with the perfect mechanical keyboard, an ideal chair, and all of the wrist wrests and braces in the world, typing on a keyboard all day is bad for your hands and wrists. The name of the game is harm reduction, and when a good 50% of your CLI work involves `git ...` or `docker ...`, etc., then aliases can be a lifesaver. 

However, I found that *an alias that only exists sometimes results in me using the full command almost all of the time*. Aliases are part of a local configuration, and there is no way I can be bothered to (remember to) set up every single machine and keep the aliases up-to-date.

#### 3. Packages

Similar to the above, a major issue encountered on a variety of machines is the lack of "standard" utilities. What's "standard" is itself a massive debate, but here are a few examples.

- Text editors - `nano, vim, vi, pico, micro, emacs`
- Miscellaneous - `gpg, curl, git, ping, rsync, cat`
- Package managers - `apt, apk, dnf, flatpak, snap, pacman`
- Languages/Systems - `docker, dotnet, rust, node, python, asdf, just`
- Improved basics - `bat, prettyping, exa/eza`

Whether or not those packages are installed immediately can be incredibly helpful to knowing the capabilities of a system without first running into a bunch of errors.

#### 4. Colors & Configurations

Related to the above pain points are general configurations, which typically fall into the following categories:

1. Color configurations
2. Command configurations
3. Shell prompts/autocomplete configurations

## sshrc

## hi.d

## dotfiles

## shell configs
