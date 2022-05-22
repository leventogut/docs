---
title: Private Clusters
summary: Private Clusters
authors:
    - Levent Ogut
date: 2022-05-22-17:00
tags:
    - GCP
    - private clusters
    - Kubernetes
---
Good for security however there are issues that aren't resolved yet, one of them is Google Cloud Build not being able to reach your cluster directly. And there is no IP list for Google Cloud Build egress hence you can't add it to allowed networks.

## References and further reading

- [Google Cloud Build deploy to GKE Private Cluster](https://stackoverflow.com/questions/51944817/google-cloud-build-deploy-to-gke-private-cluster)
