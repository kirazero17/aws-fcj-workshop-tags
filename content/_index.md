---
title: "Tags and Tagging - Extended"
weight: 1
chapter: false
---

# Tags and Tagging - Extended

### What are tags ?

A **_tag_** is a **_key-value pair_** applied to a resource to **_hold metadata_** about that resource. Each tag is a label consisting of a key and an optional value. Not all services and resource types currently support tags, see [Services that support the Resource Groups Tagging API](https://docs.aws.amazon.com/resourcegroupstagging/latest/APIReference/supported-services.html). Other services may support tags via their own APIs. It should be noted that tags are not encrypted and should not be used to store sensitive data, such as personally identifiable information (PII).

Tags that a user creates and applies to AWS resources using the AWS Command Line Interface (AWS CLI), API, or the AWS Management Console are known as **_user-defined tags_**. Several AWS services, such as AWS CloudFormation, AWS Elastic Beanstalk, and AWS Auto Scaling, automatically assign tags to resources that they create and manage. These keys are known as **_AWS generated tags_** and are typically prefixed with `aws`. This prefix cannot be used in user-defined tag keys.

There are usage requirements and limits on the number of user-defined tags that can be added to an AWS resource. For more information, refer to [Tag naming limits and requirements](https://docs.aws.amazon.com/general/latest/gr/aws_tagging.html#tag-conventions) in the **AWS General Reference guide**. AWS generated tags do not count against these user-defined tag limits.

### Workshop agenda

The goal of this workshop is to walk through the various ways in which you can tag AWS resources & query said tags. By the end of this workshop you would have an understanding on how to tag, and govern the tags of different AWS resources.

### Infrastructure architecture

The infrastructure consists of one VPC, with one public and one private subnet created inside. One EC2 AutoScaling Group is created in each subnet. The public subnet can communicate with the world through an internet gateway. To make sure only outbound traffics from the private subnet are allowed, a NAT Gateway is placed in the public subnet. All private outbound traffics are routed to it.

![arch](images/0-home/0001-architecture.png)

### Workshop Structures
- Workshop Setup
- Managing tags using the console & AWS CLI
    - Managing tags using the console
    - Managing tags using the AWS CLI
- AWS Resource Groups
    - Create a stack-based resource group
    - Tag Editor
    - Create a tag-based resource group
- IaC & tagging
    - AWS CloudFormation
        - AWS CloudFormation Guard
        - AWS CloudFormation Hooks
- CI/CD & tagging
    - Creating the CodeCommit repository
    - Creating the CodeBuild project
    - Creating the CodePipeline
    - Validating the CI/CD pipeline operation
- Tag Governance with AWS Config
    - AWS Config Required Tags
    - AWS Config Advanced Queries
    - AWS Config Cleanup
- AWS Organizations
    - AWS Tag Policies
    - Tag Enforcement with SCPs
- Workshop Cleanup

### Further reading

1. [Workshop - Manage Resources Using Tags and Resource Groups](https://000027.awsstudygroup.com/)
2. [Workshop - Manage Access to EC2 Services With Resource Tags Through IAM Services](https://000028.awsstudygroup.com/)
3. [Tagging Whitepaper](https://docs.aws.amazon.com/whitepapers/latest/tagging-best-practices/tagging-best-practices.html)