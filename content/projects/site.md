+++
title = 'Deploying a Modern Website'
date = '2023-10-13'
subtitle = 'A lightweight, professional workflow.'
author = 'Ivy Duggan'
draft = true
tags = [
    'CI/CD',
    'hugo',
    'cloudflare',
    'github',
    'devops',
]
+++

![image](/parked1.jpg "text Source: <a href=''>source</a>")

# The Background

As a contractor, I saw many projects that were built to be temporary, from architecture to technology choice to deployment. On smaller projects, its appealing to bootstrap and run fast & loose with rules, but a production-style architecture is not hard to implement, especially with modern tools.

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

[^1]: test
