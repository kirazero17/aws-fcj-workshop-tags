---
title: "Managing tags using the AWS CLI"
weight: 2
chapter: false
pre: "<b> 2.2 </b>"
---

The AWS Command Line Interface ([AWS CLI](https://aws.amazon.com/cli/)) is a unified tool to manage your AWS services. With just one tool to download and configure, you can control multiple AWS services from the command line and automate them through scripts.

Instructions for configuring the AWS CLI can be found in the [AWS CLI documentation](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html). In this workshop, you will open the provisioned AWS Cloud9 instance to get started with the AWS CLI.

### AWS Cloud9 Access

1. Navigate to the [CloudFormation](https://console.aws.amazon.com/cloudformation/) console .

1. Choose the `cloud9-ws` stack and then choose the Outputs tab.

1. The Output Key will be `Cloud9DevEnvUrl`.

1. Open the URL Value in a new browser tab, this will open up the AWS Cloud9 environment to be used in the following steps.

### Get the Amazon Virtual Private Cloud (Amazon VPC) ID

1. Navigate to the CloudFormation  console and choose the tagging workload stack.

1. Choose the Outputs tab of the stack.

1. Copy the value of the Amazon VPC ID listed and and save to a local file.

### Select the Amazon VPC

Run the following command in the AWS Cloud9/VSCode Server terminal after making the following edits:

- a. Enter value of the **Amazon VPC ID** from the previous step in the place of `"VPC-ID"`

- b. Enter the name of your region, such as `eu-west-1`, in the place of `"REGION"`

- c. Ensure double quotations (" ") are removed from these placeholders in the command below

```bash
aws ec2 describe-vpcs --vpc-ids "VPC-ID" --region "REGION" --query 'Vpcs[*][Tags]'
```

This command lists the tags on the Amazon VPC you created earlier.The output should look similar to the below:

```json
[
    [
        [
            {
                "Key": "aws:cloudformation:stack-name",
                "Value": "tagging-workload"
            },
            {
                "Key": "aws:cloudformation:logical-id",
                "Value": "tagworkshopvpc"
            },
            {
                "Key": "Name",
                "Value": "tagworkshop"
            },
            {
                "Key": "environment",
                "Value": "dev"
            },
            {
                "Key": "aws:cloudformation:stack-id",
                "Value": "arn:aws:cloudformation:us-east-1:012345678901:stack/tagging-workload/a1b2c3d4-5678-90ab-cdef-EXAMPLE11111"
            }
        ]
    ]
]
```

Notice the tag that you added in the previous lab `environment:dev`.

### Managing individual resource tags

1. Run the following command after making the following edits:

    a. Enter value of the Amazon VPC ID from the previous step in the place of VPC-ID

    b. Enter the name of your region, such as eu-west-1, in the place of REGION

    c. Ensure double quotations (" ") are removed from these placeholders

This command will add another tag, owner:workshop in this case, to this workload via the command line.

```bash
aws ec2 create-tags --resources vpc-1234567890abcdef0 --tags Key=owner,Value=workshop --region eu-west-1
```

Then check to see if the tag has been correctly added:

```bash
aws ec2 describe-vpcs --vpc-ids vpc-1234567890abcdef0 --region us-east-1 --query 'Vpcs[*][Tags]'
```
**Output:**
```json
[
    [
        [
            {
                "Key": "aws:cloudformation:stack-name",
                "Value": "tagging-workload"
            },
            {
                "Key": "aws:cloudformation:logical-id",
                "Value": "tagworkshopvpc"
            },
            {
                "Key": "Name",
                "Value": "tagworkshop"
            },
            {
                "Key": "owner",
                "Value": "workshop"
            },
            {
                "Key": "environment",
                "Value": "dev"
            },
            {
                "Key": "aws:cloudformation:stack-id",
                "Value": "arn:aws:cloudformation:us-east-1:012345678901:stack/tagging-workload/tagging-workload/a1b2c3d4-5678-90ab-cdef-EXAMPLE11111"
            }
        ]
    ]
]
```

### Conclusion

You were able to create a tag directly through the AWS CLI on an AWS resource. This shows you how easy and simple tag additions can be when using the AWS CLI.