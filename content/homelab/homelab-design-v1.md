+++
title = 'My Homelab 1.0 - Design'
date = '2023-10-06'
subtitle = ''
author = 'Ivy Duggan'
draft = true
tags = [
    'linux',
    '3d-printing',
    'DNS',
    'networking',
    'docker',
    'homelab'
]
+++

I bought a 3D printer a few months ago, and the main lesson I learned was that I did *not* have a workflow for anything. Dealing with the dozens of files was a hassle by itself, but then, as all developers will encounter if you've ever dealt with Raspberry Pi's, it came: **SD card corruption.**

Repeat a few times with at least an hour of configuration per corruption and I finally bit the bullet and started designing my own server, according to my own principles.

#### Princple 1: Open

The software for the server should be free, preferably open source.

#### Principle 2: Simple

The simplest solution is the best. Avoid over complex software & configuration if simpler just works.

#### Principle 3: Congruent

Design patterns at all levels within the server should be similar.

#### Constraints

While I may have fond memories of running 64 core AWS compute instances 24/7, the reality of my situation is that I am a recent college graduate so I want to do this on the cheap. So I decided to retrofit my 2012 Macbook Pro into my homelab. Further, if the process was successful (and well documented) on the 2012 Macbook, I could repeat it on my 2015 Macbook Pro and add another node to my homelab!

## Laptop Benefits
Using a laptop as a home server has a few intrinsic benefits:

1. Power Efficiency - optimized for battery life
1. Reduce e-waste - reduce, **re-use**, recycle
1. On device debugging - for when I break something
1. Built in battery backup - for when my house breaks
1. Small footprint - my homelab is deployed right behind my tv

These benefits were crucial in my situation as someone who lives in a relatively small apartment with a roommate. 

## My Situation
1. I have a roommate.
    - The server can't mess with normal internet operation if it fails (no DNS server).
1. California
    - Electricity is moderately expensive.
1. Positioning
    - The server will be in my room
