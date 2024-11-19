---
title: "CI/CD & tagging"
weight: 5
chapter: false
pre: "<b> 5. </b>"
---

### Continuous Integration & Continuous Delivery (CI/CD)

As the maturity of a workload increases, it’s likely that techniques such as [continuous integration and continuous deployment (CI/CD)](https://aws.amazon.com/solutions/app-development/ci-cd/) are adopted. These techniques help to reduce deployment risk by making it easier to deploy small changes more frequently with increased automation of testing. An observability strategy that detects unexpected behavior introduced by a deployment can automatically roll back the deployment with minimal user impact. As you get to the stage of deploying tens of times a day, applying tags retroactively is simply no longer practical. Everything needs to be expressed as code or configuration, version controlled, and, wherever possible, tested and evaluated before deployment into production.

In the combined [develop and operations (DevOps)](https://aws.amazon.com/devops/what-is-devops/) model , many of the practices address operational considerations as code and validate them early in the deployment lifecycle. Ideally, you want to push these checks as early in the process as you can (as shown with AWS CloudFormation hooks), so that you can be confident that your AWS CloudFormation template meets your policies before they leave the developer's machine. AWS CloudFormation Guard 2.0  provides the means to write preventative compliance rules for anything you can define with CloudFormation. The template is validated against the rules in the development environment. Clearly, this feature has a range of applications, but in this lab, you will look at a tagging usecase.

### What's in this section

- Creating the CodeCommit repository

- Creating the CodeBuild project

- Creating the CodePipeline

- Validating the CI/CD pipeline operation