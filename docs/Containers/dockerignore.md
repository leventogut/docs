---
title: .dockerignore best practices
summary: .dockerignore best practices
authors:
    - Levent Ogut
tags:
    - .dockerignore
    - docker
    - container
    - best-practices
---

## A sample `.dockerignore` file

!!! examples

    === "General"

        ``` title="General.dockerignore"
        --8<-- "src/containers/dockerignore/General.dockerignore"
        ```
    === "Node.js"

        ``` title="NodeJS.dockerignore"
        --8<-- "src/containers/dockerignore/NodeJS.dockerignore"
        ```

## References & further reading

- [.dockerignore file reference](https://docs.docker.com/engine/reference/builder/#dockerignore-file)
- [Go filepath.Match](https://golang.org/pkg/path/filepath#Match)
