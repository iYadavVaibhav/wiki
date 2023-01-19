# iYadavVaibhav's Wiki Readme

*everything about this repo*

Vist at: <https://iYadavVaibhav.github.io>

## MKDocs notes

MKDocs is static site generator, more like `Pelican`. From markdown files, it can build static site.

Static site can be deployed on Github Pages and builds can be automated using Github Actions.

So one repo has your source markdown files and another repo has static site which is build and deployed using github actions.

## Basics

- `mkdocks.yml`
  - `site_name` and `site_url` are bare minimum to specify.
  - new docs are auto picked and added to navs. If you specify pages in nav, you will have to do for all the pages you add, any new ones too..

- `mkdocs serve` live preview server on dev+
- `mkdocs build` to create `site` folder

## Deployment

- `mkdocs gh-deploy` to deploy to github
  - creates `gh-pages` branch on local and adds site to it.
  - pushes this branch to remote as default.

- Issues
  - pushes static site, needs build. Can be resolved with GitHub actions.


## Github Actions

- GitHub Actions is yet another free option from GitHub, which is basically a build server in the cloud
- have a build server automatically pick up changes in Markdown source files and build the static website directly on the build server.
- More on git page

- Links
  - <https://blog.elmah.io/deploying-a-mkdocs-documentation-site-with-github-actions/>

## Material Theme for MKDocs

- add following to config file
  
  ```yaml
  theme:
    name: material
  ```

Decisions

- Dont need tabs
- need sidebar sections
- need blog
