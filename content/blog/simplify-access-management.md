+++ 
draft = false
date = 2023-11-18T16:48:17-07:00
title = "Simplify Access Management: A GitOps Approach for Efficiency and Security"
description = "Discover the power of GitOps in Simplifying access management, optimizing efficiency, and adhering security requirement for infrastructure teams."
slug = ""
authors = ["avdhoot"]
tags = ["security","GitOps","DevEx"]
categories = []
externalLink = ""
series = []
+++

## Introduction

In today's dynamic IT landscape, managing access to various components such as AWS accounts, databases, log management systems, APM tools, and internal applications is a critical responsibility for infrastructure teams. Balancing the need for accessibility with security considerations, especially when dealing with diverse components, poses a significant challenge. In this blog post, we explore a solution that leverages GitOps principles to Simplify the access management process, addressing common pain points and enhancing overall efficiency.

## The Problem

Infrastructure teams often find themselves caught in a manual access management process, involving multiple steps and dependencies. The typical workflow includes the engineering/support team initiating an access request ticket, waiting managerial approval for auditing purposes, manual provision of access by the DevOps team, and subsequent validation by the infrastructure team if they need access. This process not only lacks efficiency but also introduces delays, Sometimes leading to customer escalations and reduced developer productivity.

## The Solution

To address these challenges, we set out to design a solution with the following goals:

1. Zero infra team involvement in day-to-day access requests.
2. Implement least access for only for required duration.
3. Maintain a audit trail for each access request.
4. Minimize on/off boarding efforts.
5. Stay within the budget, avoiding unnecessary enterprise license expenses for Single Sign-On (SSO) solutions.

## Our Approach

Taking inspiration from GitOps methodologies, we come up with a streamlined workflow where users initiate access requests through a defined Git repository. Users submit pull requests (PRs) containing YAML specifications with basic information such as name, email, and required access details. After managerial approval, the PR is merged, triggering a CI workflow.

During the CI workflow, a Terraform template is generated for the respective provider(k8s, aws etc..), and the changes are applied automatically. This automated approach ensures that users get the required access promptly without manual intervention from the infrastructure team.

```yaml
name: Avdhoot Dendge
email: avdhoot@xxx.yy
type: human
k8s:
  dev-cluser:
    my-namespace:
      - role: exec-only
        expiresOn: 2024-02-04
      - role: db-readOnly
        expiresOn: 2023-12-04
aws:
  prod-account:
    - policy: rds-read-only
      expiresOn: 2024-01-23
  dev-account:
    - policy: mobile-team-role
      expiresOn: 2024-01-23
logs:
  roles:
    - dev-cluster-log
teleport: 
  apps:
    - dev-kibana
```
## Challenges and Lessons Learned

While our GitOps-based solution proved effective, we encountered challenges, especially with non-engineering teams unfamiliar with GitHub. Initial hand-holding was necessary to facilitate a smooth transition. Additionally, compared to Single Sign-On solutions, users had to manage more username/password combinations. However, this tradeoff was considered to be acceptable when considering the overall cost-effectiveness of the solution.


