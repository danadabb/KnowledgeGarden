---
id: jeybygpftmwnk69ylywov78
title: Solutions Architect Associate
desc: "Notes for the SAA certification"
updated: 1734582975000
created: 1734484581601
---

Notes attributed to [this course](https://learn.cantrill.io/courses/enrolled/1820301)

## AWS Accounts

- AWS account is a container for identities (users) and resources
- Every AWS account has a root users
- The account root user can't be restricted it has full access to everything within this account.
- The credit card used with the root account will be the Account Payment method, everything will be billed to that card
- AWS is a pay-as-you-go/consume platform
- Certain resources have a free-tier
- IAM - every AWS account comes with it's own IAM DB
- IAM lets you create 3 different IAM profiles - Users, groups and roles
  - Users represent humans or applications that need access to your account - this is for individual purposes
  - Groups are a collection of related users
  - Roles can be used for granting external access to your account
- IAM policy - used to allow/deny access to AWS services attached to other identities
- IAM is an identity provider, which also authenticates and authorizes
- IAM Access Keys are long term credentials with up to 2 available per IAM user typically used in CLIs or applications
- Access Keys are made up of Access Key ID and the Secret Access Key
-
