---
title: "AWS CloudFormation Hooks"
weight: 2
chapter: false
pre: "<b> 4.2 </b>"
---

### What are AWS CloudFormation Hooks?
Hooks proactively inspect the configuration of your AWS resources before provisioning. If non-compliant resources are found, AWS CloudFormation returns a failure status and either fails the operation or provides a warning and allows the operation to continue based on the hook failure mode. You can use pre-built hooks or build your own hooks using the CloudFormation Command Line Interface (CloudFormation CLI) .

For this lab you will use a hook example from the [aws-cloudformation](https://github.com/aws-cloudformation) github repository.