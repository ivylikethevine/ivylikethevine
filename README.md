# ivylikethevine - Hi, that's me

Welcome to my github profile/personal site. Here you'll find my latest projects, blog posts, and any resources I want to share. I love to program, and have recently started to contribute to OSS projects, as well as my own.

My main current projects are as follows below:

## My Projects

### Saffron

An open source, docker compose based homelab configuration system.

[Repo](https://github.com/ivylikethevine/saffron) | [Documentation](https://ivylikethevine.github.io/saffron/#/)

### say-hi

A single command to connect to ssh hosts and docker containers while bringing all of your essential configs along.

[Repo](https://github.com/ivylikethevine/say-hi) | [Homebrew tap](https://github.com/ivylikethevine/homebrew-tap) | [Documentation](https://ivylikethevine.github.io/say-hi/)

### sharerr-rs

A system to cross-seed existing catalogued media libraries between friends, with as little setup as possible.

[Repo](https://github.com/ivylikethevine/sharerr-rs) | [Documentation](https://ivylikethevine.github.io/sharerr-rs/)

### instadroid

Early alpha of a project to convert instagram feeds into RSS.

[Repo](https://github.com/ivylikethevine/instadroid) | [Documentation](https://ivylikethevine.github.io/instadroid/)

### python-constricter

Python linting plugin that enforces strict typing of **all** variables, at **all** levels, in **all** python code.

[Repo](https://github.com/ivylikethevine/python-constricter) | [PyPi Package](https://pypi.org/project/python-constricter/)

---

### ivylikethevine - My Personal Site

This is my [github repo](https://github.com/ivylikethevine/ivylikethevine) for my personal blog, [ivylikethevine.com](http://ivylikethevine.com), complete with continuous integration and deployment, staging & production environments.

#### Tech Stack

- Built using
  - [Hugo](https://gohugo.io/) - A modern, static site framework built using [Go](https://go.dev/) with simple [markdown](https://www.markdownguide.org/) posts.
  - [Cloudflare Pages](https://developers.cloudflare.com/pages/framework-guides/deploy-a-hugo-site/) - CI/CD for deploying to cloudflare domains.
    - [Github Integration](https://developers.cloudflare.com/pages/configuration/git-integration/) - CI/CD integration as well as branch protection if an automatic deployment fails.
  - Documented on [my blog](https://ivylikethevine.com/projects/site-devops/)

##### Installation

Requires: git, go, hugo-extended, dart-sass

```bash
git clone git@github.com:ivylikethevine/ivylikethevine.git # or git clone https://github.com/ivylikethevine/ivylikethevine.git
cd ivylikethevine
git submodule update --init --recursive
hugo mod clean && hugo mod tidy # optional, but nice
hugo mod get
hugo serve # development preview (drafts visible) -> localhost:1313
hugo serve -e staging # staging preview (drafts hidden) -> https://ivylikethevine.pages.dev
hugo serve -e production # production preview (drafts hidden) -> https://ivylikethevine.com
```
