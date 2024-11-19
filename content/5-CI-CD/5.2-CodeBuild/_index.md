---
title: "Creating the CodeBuild project"
weight: 2
chapter: false
pre: "<b> 5.2 </b>"
---

The AWS CodeBuild project orchestrates around CloudFormation Guard and runs validation checks of your CloudFormation templates as a phase of the CI process.

### Creating a CodeBuild project

1. From the [AWS Developer Tools console](https://console.aws.amazon.com/codesuite/), use the navigation pane to choose **Build** then choose **Build projects**.

1. Choose **Create project**.

1. For **Project name**, enter the name `cfn-guard-demo`.

1. For **Source provider**, choose **AWS CodeCommit**.

1. For **Repository**, choose the CodeCommit repository `cfn-guard-demo` that you created in the previous lab. Keep the rest of the configurations for **Source** as the default.

To set up the CodeBuild environment you will use managed image based on Ubuntu.

6. For **Provisioning model**, select **On-demand**.

1. For **Environment Image**, select **Managed image**.

1. For **Compute**, select **EC2**.

1. For **Operating system**, choose **Ubuntu**.

1. For **Service role**¸ select **New service role**.

1. For **Role name**, enter the service role name `codebuild-cfn-guard-demo-service-role`.

1. Leave the default settings for additional configurations.

1. For **Buildspec specifications**, select **Use a buildspec file**.

1. Use the buildspec file in your previously created repository. In this case `**buildspec.yml**`.

1. Leave the remaining defaults and choose **Create build project**.

![](../../images/5/2/001.png)

### Cleanup
To avoid incurring future charges, delete the resources that you have created during the walkthrough:

- **CodeBuild project**

- **CodeCommit repository**