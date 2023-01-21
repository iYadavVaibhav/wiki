---
description: Notes on Material Theme for MkDocs
date: 2018-07-25
revision_date: 2024-08-12
---

# Material for MkDocs

*all about MkDocs and Material theme*

## MkDocs

MkDocs is static site generator, more like `Pelican`. From markdown files, it can build static site.

Static site can be deployed on Github Pages and builds can be automated using Github Actions.

So one repo has your source markdown files and another repo has static site which is build and deployed using github actions.

### Basics

- `mkdocks.yml`
  - `site_name` and `site_url` are bare minimum to specify.
  - new docs are auto picked and added to navs. If you specify pages in nav, you will have to do for all the pages you add, any new ones too..

- `mkdocs serve` live preview server on dev+
- `mkdocs build` to create `site` folder

### Deployment

- `mkdocs gh-deploy` to deploy to github
  - creates `gh-pages` branch on local and adds site to it.
  - pushes this branch to remote as default.

- Issues
  - pushes static site, needs build. Can be resolved with GitHub actions.


### Github Actions

- GitHub Actions is yet another free option from GitHub, which is basically a build server in the cloud
- have a build server automatically pick up changes in Markdown source files and build the static website directly on the build server.
- More on git page

- Links
  - <https://blog.elmah.io/deploying-a-mkdocs-documentation-site-with-github-actions/>

## Material Theme

- Requirements
  - `pip install mkdocs-material`
  - `pip install mkdocs-git-revision-date-localized-plugin`

- add following to config file
  
  ```yaml
  theme:
    name: material
  ```

- For Good Looks
  - Keep `title: Two-Three words`
  - Can have H1-H5 as proper english sentences, keep small to a few words.


## MarkDown Extensions

Extend markdown and do configurations

### Admonitions

??? Collapsible

    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.

???+ "Collapsible Open"

    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.

More: <https://squidfunk.github.io/mkdocs-material/reference/admonitions/#collapsible-blocks>

### Code Blocks

Inline `code` and block.

```py title="Code Title: bubble_sort.py"  linenums="1"
def bubble_sort(items):
    for i in range(len(items)):
        for j in range(len(items) - 1 - i):
            if items[j] > items[j + 1]:
                items[j], items[j + 1] = items[j + 1], items[j]
```

More: <https://squidfunk.github.io/mkdocs-material/reference/code-blocks/>

### Content Tabs

=== "C"

    ``` c
    #include <stdio.h>

    int main(void) {
      printf("Hello world!\n");
      return 0;
    }
    ```

=== "C++"

    ``` c++
    #include <iostream>

    int main(void) {
      std::cout << "Hello world!" << std::endl;
      return 0;
    }
    ```

More: <https://squidfunk.github.io/mkdocs-material/reference/content-tabs/>

### Diagrams Mermaid

``` mermaid
graph LR
  A[Start] --> B{Error?};
  B -->|Yes| C[Hmm...];
  C --> D[Debug];
  D --> B;
  B ---->|No| E[Yay!];
```

more: <https://squidfunk.github.io/mkdocs-material/reference/diagrams/>

### MathJax

Inline $\alpha$ math.

$$
\operatorname{ker} f=\{g\in G:f(g)=e_{H}\}{\mbox{.}}
$$

More: <https://squidfunk.github.io/mkdocs-material/reference/mathjax/>

### Lists as Tasks

- [x] Lorem ipsum dolor sit amet, consectetur adipiscing elit
- [ ] Vestibulum convallis sit amet nisi a tincidunt
  - [x] In hac habitasse platea dictumst
  - [x] In scelerisque nibh non dolor mollis congue sed et metus
  - [ ] Praesent sed risus massa
- [ ] Aenean pretium efficitur erat, donec pharetra, ligula non scelerisque

More: <https://squidfunk.github.io/mkdocs-material/reference/lists/>

## Third Party Tweaks

Modify theme and interact with core Python Plugins

### Last 10 updated pages

Link <https://timvink.github.io/mkdocs-git-revision-date-localized-plugin/howto/override-a-theme/#example-list-last-updated-pages>

## iYV Wiki Specifics

Decisions

- Dont need tabs
- need sidebar sections
- need blog

Example repo wiki - <https://github.com/barnumbirr/wiki>

view all page items

```py

# all page items
{% for key,value in page.__dict__.items() %}
    {{ key,value }}
{% endfor %}


{% for key,value in page.meta.items() %}
    {{ key,value }}
{% endfor %}
```
