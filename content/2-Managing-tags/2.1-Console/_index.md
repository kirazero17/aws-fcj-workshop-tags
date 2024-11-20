---
title: "Managing tags using the console"
weight: 1
chapter: false
pre: "<b> 2.1 </b>"
---

### Get the Amazon Virtual Private Cloud (Amazon VPC) ID

1. Navigate to the [CloudFormation](https://console.aws.amazon.com/cloudformation/) console and choose the **tagging workload** stack.

1. Choose the **Outputs** tab of the stack.

1. Copy the value of the Amazon VPC ID listed.

### Select the Amazon VPC

1. Navigate to the [Amazon VPC dashboard](https://console.aws.amazon.com/vpc).

1. In the navigation pane, under Virtual private cloud, choose Your VPCs.

1. In the search bar, enter the Amazon VPC ID value you copied and press Enter.

1. Select the box to highlight the row of the Amazon VPC.

1. Choose the Tags tab.

![VPC Tags](../../images/2/1/001-vpc_default_tags.png)

Notice that the Amazon VPC already has a number of tags assigned to it. The tags were added to the Amazon VPC as they were created by the CloudFormation service. In addition to any tags you define, CloudFormation automatically creates stack-level tags with the prefix `aws:`. The `aws:` prefix is reserved for AWS use. This prefix is case-insensitive. If you use this prefix in the `Key` or `Value` property, you can't update or delete the tag. Tags with this prefix don't count toward the number of tags per resource.

Although `aws:` is used here, it's not the only example of AWS-generated tags. See table below for a non-exhaustive list of examples:

| **AWS-generated tag key** |	**Rationale**  |
|------------------------------|-------------------------------------------------------------------|
| aws:ec2spot:fleet-request-id	| Identifies the Amazon EC2 Spot Instance request that launched the instance  |
aws:cloudformation:stack-name	| Identifies the AWS CloudFormation stack that created the resource  |
| lambda-console:blueprint	| Identifies blueprint used as a template for an AWS Lambda function  |
| elasticbeanstalk:environment-name	| Identifies the application that created the resource  |
| aws:servicecatalog:provisionedProductArn	| The provisioned product Amazon Resource Name (ARN)  |
| aws:servicecatalog:productArn	| The ARN of the product from which the provisioned product was launched  |

Another tag assigned to the resource is `Name`. This is a predefined tag that can be used to name a resource in the console for easy identification. In this case you added this tag via CloudFormation. This will be covered in a later lab.

### Managing individual resource tags

1. Select the resource and choose the **Tags** tab, then choose **Manage tags**.

1. Notice that the tags presented with the option to edit the **Name** tag are only the user-defined tags.

1. Choose **Add new tag**.

1. Enter the key:value for the tag. In this case, enter `environment:dev`.

1. Choose **Add new tag** again for each additional tag to add. When you are finished adding tags, choose Save.

![Add tag](../../images/2/1/002-vpc_new_tag.png)

You have now added a new tag on the Amazon VPC resource.

![Added tag](../../images/2/1/003-vpc_final_look.png)

### Conclusion
You were able to add a new tag directly on an AWS resource using the console. This shows you how easy and simple tag additions can be when using the console.