---
title: A template document
summary: Good place to start, copy and write away.
authors:
    - Levent Ogut
date: 2022-05-12-12:06
tags:
    - template
    - document
    - markdown
    - example
---
## Admonitions [REF](https://squidfunk.github.io/mkdocs-material/reference/admonitions/)

!!! example

    === "Preview"

        ???+ important
            Square brackets "[]" mean the option/parameter is optional. They might include a description of the parameter.

    === "Markdown"

        ```markdown
        ???+ important
            Square brackets "[]" mean the option/parameter is optional. 
            They might include a description of the parameter.
        ```


## Code blocks [REF](https://squidfunk.github.io/mkdocs-material/reference/code-blocks/#usage)

!!! example

    === "Preview"

        ``` python title="bubble_sort.py" hl_lines="2 3"
        def bubble_sort(items):
            for i in range(len(items)):
                for j in range(len(items) - 1 - i):
                    if items[j] > items[j + 1]:
                        items[j], items[j + 1] = items[j + 1], items[j]
        ```

    === "Markdown"

        ````
        ``` python title="bubble_sort.py" hl_lines="3 4"
        def bubble_sort(items):
            for i in range(len(items)):
                for j in range(len(items) - 1 - i):
                    if items[j] > items[j + 1]:
                        items[j], items[j + 1] = items[j + 1], items[j]
        ```
        ````

## Code block with annotation [REF](https://squidfunk.github.io/mkdocs-material/reference/code-blocks/#adding-annotations)

!!! example

    === "Preview"

        ``` yaml title="Enabling annotation feature."
        theme:
        features:
            - content.code.annotate # (1)
        ```

        1.  :man_raising_hand: I'm a code annotation! I can contain `code`, __formatted
            text__, images, ... basically anything that can be written in Markdown.

    === "Markdown"

        ````
        ``` yaml title="Enabling annotation feature."
        theme:
        features:
            - content.code.annotate # (1)
        ```
        ````

        ````
        ```markdown
        1.  :man_raising_hand: I'm a code annotation! I can contain `code`, __formatted
            text__, images, ... basically anything that can be written in Markdown.
        ```
        ````

    === "Configure"

        ``` yaml title="Enabling annotation feature."
        theme:
        features:
            - content.code.annotate
        ```

## Including external files [REF](https://facelessuser.github.io/pymdown-extensions/extensions/snippets/#snippets-notation)

!!! example

    === "Preview"

        ``` title=".gitignore"
        --8<-- ".gitignore"
        ```

    === "Markdown"

        ````markdown
        ``` title=".gitignore"
        \--8<-- ".gitignore"  # (1)
        ```
        ````

        1.  "\" is included so that the expression doesn't include the file.
