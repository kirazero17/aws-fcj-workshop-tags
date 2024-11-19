---
title: "AWS Resource Groups"
weight: 3
chapter: false
pre: "<b> 3. </b>"
---

### What are AWS Resource Groups?
In AWS, a resource is an entity that you can work with. Examples include an Amazon EC2 instance, an AWS CloudFormation stack, or an Amazon S3 bucket. If you work with multiple resources, you might find it useful to manage them as a group rather than move from one AWS service to another for each task. If you manage large numbers of related resources, such as EC2 instances that make up an application layer, you likely need to perform bulk actions on these resources at one time. Examples of bulk actions include:

- Applying updates or security patches.

- Upgrading applications.

- Opening or closing ports to network traffic.

- Collecting specific log and monitoring data from your fleet of instances.

A _resource group_ is a collection of AWS resources that are all in the same AWS Region, and that match the criteria specified in the group's query. In Resource Groups, there are two types of queries you can use to build a group. Both query types include resources that are specified in the format `AWS::service::resource`:

#### Tag-based
A tag-based resource group bases its membership on a query that specifies a list of resource types and tags. Tags are keys that help identify and sort your resources within your organization. Optionally, tags include values for keys.

{{% notice note %}}
Do not store personally identifiable information (PII) or other confidential or sensitive information in tags. You use tags to provide you with billing and administration services. Tags are not intended to be used for private or sensitive data.
{{% /notice %}}

#### AWS CloudFormation stack-based
An AWS CloudFormation stack-based resource group bases its membership on a query that specifies an AWS CloudFormation stack in your account in the current region. You can optionally choose resource types within the stack that you want to be in the group. You can base your query on only one AWS CloudFormation stack.

### Section Objectives
You will create query-based groups in AWS Resource Groups and use Tag Editor to add tags to the resource group based on the CloudFormation stack you created earlier.

### Further reading

- [AWS Resource Groups documentation](https://docs.aws.amazon.com/ARG/latest/userguide/resource-groups.html)