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

Initially as I researched how to carry my configs with me, I found [sshrc](https://github.com/cdown/sshrc), which was a great intro to what I wanted in this type of system. `sshrc` is the base of my project, `hi`, with some heavy modifications. `sshrc` was originally a project by Russell Stewart (Russell91), but I found the cdown fork linked above. Even this fork was still 6 years old, so I ended up changing almost all of it. `sshrc` still remains a relatively straightforward solution and is simpler than what I ended up building, so I recommend it. :)

## hi.d

The design goal of hi.d was to make it so I never had to do any real per-device configuration. I switch between many Linux devices, and I wanted installing, syncing, and carrying my configs via ssh as easy and simple as possible. I further wanted to build a shell configuration for the (as I see them) 3 main shell languages: bash, zsh, and fish.

I personally use fish on my machines, but I hardly ever install packages for my comfort on a bare metal host as a security precaution, so bash is a must. Zsh is a nice inbetween, as a lot of modern Linux installs will have it, and it is a lot more useful than bare bash. 

As a sub-point to the above, I also required my aliases to properly work in all shells, in addition to enabling terminal colors. If my aliases aren't *always* working, I will *never* use them.

### Initial Design

I knew from the start that my ideal system to maintain my configuration would be to house hi.d as a git repository. I could easily just `git push` any changes and `git pull` to update my other machines. (I would later add a header that notifies me if I have any un-pushed changes to the installed `hi.d` repo). This has a few drawbacks/concerns:

1. Security - Your shell configurations shouldn't ever be used to store secrets, but even some shell variables are best left out of the git history. An aggressive .gitignore was required, and I continually add to it as I find other kinds of files that are security-adjacent. 
2. Installer - If we have a git repo, we can't (easily) use the default location for most configuration files (typically something like `~/.gitconfig || /$HOME/$USER/.gitconfig || /home/ivy/.gitconfig ` ), so we'll need a script to configure our default configurations to reference our `hi.d` git-tracked files. This will also be uesful later, when we carry our configs to a new host.
3. Per Machine Config - Sometimes it is inevitable that a single machine will need "some goddamned fix for some goddamned reason". I've been lucky that almost all of my machines have nothing in the local configuration outside of the above mentioned chainloader, but we can modify the local configurations if we have to.


#### Other Considerations

1. Fail safety
2. Conditional loading
3. Compatibility
4. Speed
5. Unifying behaviors


## dotfiles

## shell configs
