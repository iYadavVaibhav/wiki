---
title: Material for MkDocs
description: Notes on material for MkDocs
---
# Material for MkDocs

*all about material theme for mkdocs*

## Admonitions

??? Collapsible

    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.

???+ "Collapsible Open"

    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.

More: <https://squidfunk.github.io/mkdocs-material/reference/admonitions/#collapsible-blocks>

## Code Blocks

Inline `code` and block.


```py title="Code Title: bubble_sort.py"  linenums="1"
    def bubble_sort(items):
        for i in range(len(items)):
            for j in range(len(items) - 1 - i):
                if items[j] > items[j + 1]:
                    items[j], items[j + 1] = items[j + 1], items[j]
```

More: <https://squidfunk.github.io/mkdocs-material/reference/code-blocks/>

## Content Tabs

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

## Diagrams Mermaid

``` mermaid
graph LR
  A[Start] --> B{Error?};
  B -->|Yes| C[Hmm...];
  C --> D[Debug];
  D --> B;
  B ---->|No| E[Yay!];
```

more: <https://squidfunk.github.io/mkdocs-material/reference/diagrams/>

## MathJax

Inline $\alpha$ math.

$$
\operatorname{ker} f=\{g\in G:f(g)=e_{H}\}{\mbox{.}}
$$

More: <https://squidfunk.github.io/mkdocs-material/reference/mathjax/>

## Lists as Tasks

- [x] Lorem ipsum dolor sit amet, consectetur adipiscing elit
- [ ] Vestibulum convallis sit amet nisi a tincidunt
    * [x] In hac habitasse platea dictumst
    * [x] In scelerisque nibh non dolor mollis congue sed et metus
    * [ ] Praesent sed risus massa
- [ ] Aenean pretium efficitur erat, donec pharetra, ligula non scelerisque

More: <https://squidfunk.github.io/mkdocs-material/reference/lists/>
