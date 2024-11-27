# Hi, I'm Ivy (likethevine)

This is my [github repo](https://github.com/ivylikethevine/ivylikethevine) for my personal blog, [ivylikethevine.com](http://ivylikethevine.com), complete with continuous integration and deployment, staging & production environments.

![GitHub repo size](https://img.shields.io/github/repo-size/ivylikethevine/ivylikethevine) | ![GitHub last commit](https://img.shields.io/github/last-commit/ivylikethevine/ivylikethevine)

## Tech Stack

- Built using
  - [Hugo](https://gohugo.io/) - A modern, static site framework built using [Go](https://go.dev/) with simple [markdown](https://www.markdownguide.org/) posts.
    - [hello-friend-ng](https://themes.gohugo.io/themes/hugo-theme-hello-friend-ng/) - Theme used.
  - [HugoMods](https://hugomods.com/) - Additional modules for Hugo.
    - [Images](https://github.com/hugomods/images) - .webp compression for low file size.
    - [KaTex](https://github.com/hugomods/katex) - LaTeX-like library to mathematical equations.
    - [Mermaid](https://github.com/hugomods/mermaid) - Easily created graphs including git flows, etc.
    - [Icons](https://github.com/hugomods/icons/) - Enable use of many icons ([fontawesome](https://fontawesome.com/) in my case).
    - [Shortcodes](https://github.com/hugomods/shortcodes/) - Other miscellaneous nice-to-haves.
  - [Cloudflare Pages](https://developers.cloudflare.com/pages/framework-guides/deploy-a-hugo-site/) - CI/CD for deploying to cloudflare domains.
    - [Github Integration](https://developers.cloudflare.com/pages/configuration/git-integration/) - CI/CD integration as well as branch protection if an automatic deployment fails.
    - Utilizing multiple configs for `development`, `staging`, and `production` environments.
      - development - `hugo serve` for local work
      - staging - `hugo serve -e staging` with live preview of `origin/development` branch at [ivylikethevine.pages.dev](https://ivylikethevine.pages.dev)
      - production - `hugo serve -e production` with `origin/main` deployed at [ivylikethevine.com](https://ivylikethevine.com)
  - [Github workflows](https://docs.github.com/en/actions/using-workflows) - used to convert .ipynb Jupyter Notebooks to HTML for display
  - Documented on [my blog](https://ivylikethevine.com/projects/site-devops/)

### Installation

Requires: git, go, hugo-extended, dart-sass

```bash
git clone git@github.com:ivylikethevine/ivylikethevine.git # or git clone https://github.com/ivylikethevine/ivylikethevine.git
cd ivylikethevine
git submodule update --init --recursive
hugo mod clean && hugo mod tidy # optional, but nice
hugo mod get
hugo serve # development preview (drafts visible)
hugo serve -e staging # staging preview (drafts hidden)
hugo serve -e production # production preview (drafts hidden)
```

#### Resources

https://learnxinyminutes.com/docs/markdown/

https://learnxinyminutes.com/docs/toml/
