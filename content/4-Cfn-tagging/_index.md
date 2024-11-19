---
title: "AWS CloudFormation & Tagging"
weight: 4
chapter: false
pre: "<b> 4. </b>"
---

### Infrastructure as Code (IaC)
Infrastructure was traditionally provisioned using a combination of scripts and manual processes. Sometimes these scripts were stored in version control systems or documented step by step in text files or run-books. Often the person writing the run books is not the same person executing these scripts or following through the run-books. If these scripts or runbooks are not updated frequently, they can potentially become a show-stopper in deployments. This results in the creation of new environments not always being repeatable, reliable, or consistent.

### Why use IaC ?
Practicing infrastructure as code means applying the same rigor of application code development to infrastructure provisioning. All configurations should be defined in a declarative way and stored in a source control system such as [AWS CodeCommit](https://aws.amazon.com/codecommit), the same as application code. Infrastructure provisioning, orchestration, and deployment should also support the use of the infrastructure as code.

### What is AWS CloudFormation?
AWS CloudFormation is a service that helps you model and set up your AWS resources so that you can spend less time managing those resources and more time focusing on your applications that run in AWS. You create a template that describes all the AWS resources that you want (like Amazon EC2 instances or Amazon RDS instances), and CloudFormation takes care of provisioning and configuring those resources for you. You don't need to individually create and configure AWS resources and figure out what's dependent on what; CloudFormation handles that.

### Section Objectives
The objective of this section is to take a look at CloudFormation tooling and see how it can be used to enhance the adoption, and ease the operational overhead of your tagging strategy.