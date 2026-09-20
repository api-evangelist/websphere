---
title: "Configuration drift- why it’s bad and how to solve it with GitOps and ArgoCD"
url: "https://openliberty.io/_i18n/en/2026/09/18/2024-04-26-argocd-drift-pt1.html"
date: "2026-09-18"
author: "Daniel Guinan"
feed_url: "https://openliberty.io/feed.xml"
---
Configuration drift refers to the disparity between configurations as coded in repositories like Git and what’s actively deployed. Despite embracing continuous integration and continuous deployment (CI/CD) methodologies, modern software organizations still grapple with this disparity, which not only complicates deployments but can also jeopardize the stability of production environments. For example, imagine a scenario where a Continuous Deployment (CD) pipeline deploys an application to a Kubernetes cluster, with all configurations codified in a Git repository, as shown in the following diagr
