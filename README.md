# Hi, I'm Ivy (likethevine)

This is my github repo for my personal blog, [ivylikethevine.com](http://ivylikethevine.com), complete with continuous integration and deployment, staging & production environments, automatic file compression, and a variety of add-ons for rich content (such as [JupyterNotebook](https://jupyter.org/) integrations, equations, and diagrams).

![GitHub repo size](https://img.shields.io/github/repo-size/ivylikethevine/ivylikethevine) ![GitHub last commit](https://img.shields.io/github/last-commit/ivylikethevine/ivylikethevine)

## Tech Stack

- Built using
  - [Hugo](https://gohugo.io/) - A modern, static site framework built using [Go](https://go.dev/) with simple [markdown](https://www.markdownguide.org/) posts.
    - [Puppet](https://themes.gohugo.io/themes/hugo-theme-puppet/) - Theme used & extended.
  - [HugoMods](https://hugomods.com/) - Additional modules for Hugo.
    - [Images](https://github.com/hugomods/images) - .webp compression for low file size.
    - [KaTex](https://github.com/hugomods/katex) - LaTeX-like library to mathematical equations.
    - [Mermaid](https://github.com/hugomods/mermaid) - Easily created graphs including git flows, etc.
  - [Cloudflare Pages](https://developers.cloudflare.com/pages/framework-guides/deploy-a-hugo-site/) - CI/CD for deploying to cloudflare domains.
    - [Github Integration](https://developers.cloudflare.com/pages/configuration/git-integration/) - CI/CD integration as well as branch protection if an automatic deployment fails.
    - Utilizing multiple configs for `development`, `staging`, and `production` environments.
      - development - `hugo serve` for local work
      - staging - `hugo serve -e staging` with live preview of `origin/development` branch at [ivylikethevine.pages.dev](https://ivylikethevine.pages.dev)
      - production - `hugo serve -e production` with `origin/main` deployed at [ivylikethevine.com](https://ivylikethevine.com)
  - [Github workflows](https://docs.github.com/en/actions/using-workflows) - used to convert .ipynb Jupyter Notebooks to HTML for display
  - [Giscus](https://giscus.app/) - Github integrated comment system.
  - Documented on [my blog](https://ivylikethevine.com/projects/site-devops/)

### Installation

Requires: git, go, hugo-extended, dart-sass

```bash
hugo serve # development preview (drafts visible)
hugo serve -e staging # staging preview (drafts hidden)
hugo serve -e production # production preview (drafts hidden, giscus enabled)
```

#### Resources

https://learnxinyminutes.com/docs/markdown/

https://learnxinyminutes.com/docs/toml/