+++
title = "(Mis)adventures in Linux #1: Intro"
date = "2023-10-06"
author = "Ivy Duggan"
tags = [
    "linux",
    "git",
    "unix",
    "bash"
]
draft = true
+++

![image](/linux-penguin.jpg)

# The 'Just Work' Philosophy

Linux is awesome! It's free, configurable, versatile, and it runs on almost anything made in the last ~20 years. However, if you are a new user, Linux has showstopping usability issues that are apparent on first boot of a new system.

## Human Centric Design

I define a Usability Failure as the following: any event where a human's expectation of behaviour is not met by the software (in this case, the OS).

This very broad definiton means that many of the usability failures I point out may be "intended behaviour". 


1. Immediate unexpected outcome
   - inability to install *.deb files
1. Latent unexpected outcome - inability to update .deb files
1. 

## The Princess and the Pea

Before I end up cruxified on Linux forums, I understand that a massive part of the reason for the poor UX is due to the immense workload of developing & supporting an OS. The devs who work on open source software are overworked, underappreciated, and

### Why People Don't Use Linux
1. Lack of software support - hard to address
1. Things that actually can work don't **just work**
1. Things that just work can't be done without the commandline.

## Trying to Install *.deb files
1. Install Linux
1. Open built-in browser (Firefox)
1. Search for application (Chrome, Spotify, Slack, Discord)
1. Download \*.deb file
1. Click on downloaded file (in browser or in file explorer)
1. Archive Manager Opens
   - Default Ubuntu UX breaksdown at this point. Archive manager is the default file opener for \*.deb files.
1. Google "how to install deb file"
   - Find solution that requires command line - NOGO
   - Find solution that requires software installer
     - A popular option, however the way that software installer works, new .deb files cannot be installed over old packages. Users then must uninstall and reinstall.
1. Give up.

## To Deb, to Flatpak, to Snap, or to AppImage?
Ubuntu-based distros have 4 main kinds of system-wide application installers:
1. deb
1. flatpaks
1. snaps
1. appimage


### Linux Commandments

1. Do not break Windows.
   - While I'd love to force the world to use Linux exclusively, most people start their journeys with a dualbooted system. If a distro can't be setup to dual boot without fighting partitions & EFI & grub, then it isn't ready for mainstream users.
1. Clicking on Things Should Just Work.

## Fruits of my Frustration

Since I have done nothing but complain for the past few years, I have decided to begin compiling my normal configuration step for UNIX based machines. The following snippets are tested on Ubuntu 22.04LTS -- my preferred distribution.

