---
title: "Tag Enforcement with SCPs"
weight: 2
chapter: false
pre: "<b> 7.2 </b>"
---

This lab is based on the following blog post: [Implement AWS resource tagging strategy using AWS Tag Policies and Service Control Policies (SCPs)](https://aws.amazon.com/blogs/mt/implement-aws-resource-tagging-strategy-using-aws-tag-policies-and-service-control-policies-scps/)

Tag Policy only enforces the accepted value of a tag, and not its presence. Therefore, users (with appropriate IAM permissions) would still be able to create untagged resources. To restrict the creation of an AWS resource without the appropriate tags, you will utilize SCPs to set guardrails around resource creation requests.

Sign in to the [organization’s management account](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_getting-started_concepts.html) and enable SCPs for your organization.

![](../../images/7/2/001.png)

Now, let’s create an SCP that denies Amazon EC2 instance creation if the tag keys CostCenter and team and their allowed values in the Tag Policy (including how the values are capitalized) are missing.

1. You will apply the following SCP policy in the JSON editor.

    ```json
    {
    "Version": "2012-10-17",
    "Statement": [
        {
        "Sid": "DenyEC2CreationSCP1",
        "Effect": "Deny",
        "Action": [
            "ec2:RunInstances"
        ],
        "Resource": [
            "arn:aws:ec2:*:*:instance/*",
            "arn:aws:ec2:*:*:volume/*"
        ],
        "Condition": {
            "Null": {
            "aws:RequestTag/CostCenter": "true"
            }
        }
        },
        {
        "Sid": "DenyEC2CreationSCP2",
        "Effect": "Deny",
        "Action": [
            "ec2:RunInstances"
        ],
        "Resource": [
            "arn:aws:ec2:*:*:instance/*",
            "arn:aws:ec2:*:*:volume/*"
        ],
        "Condition": {
            "Null": {
            "aws:RequestTag/team": "true"
            }
        }
        }
    ]
    }
    ```

    ![](../../images/7/2/002.png)
    
1. Then create policy. You will then see your created policy below.

    ![](../../images/7/2/003.png)

1. Now, let’s create another SCP that denies users from deleting tag key CostCenter and the tag key team after it has been created. Create this SCP by simply copying the following JSON template and pasting it in the SCP –> JSON editor.

    ```json
    {
    "Version": "2012-10-17",
    "Statement": [
        {
        "Sid": "DenyDeleteTag1",
        "Effect": "Deny",
        "Action": [
            "ec2:DeleteTags"
        ],
        "Resource": [
            "arn:aws:ec2:*:*:instance/*",
            "arn:aws:ec2:*:*:volume/*"
        ],
        "Condition": {
            "Null": {
            "aws:RequestTag/CostCenter": "false"
            }
        }
        },
        {
        "Sid": "DenyDeleteTag2",
        "Effect": "Deny",
        "Action": [
            "ec2:DeleteTags"
        ],
        "Resource": [
            "arn:aws:ec2:*:*:instance/*",
            "arn:aws:ec2:*:*:volume/*"
        ],
        "Condition": {
            "Null": {
            "aws:RequestTag/team": "false"
            }
        }
        }
    ]
    }
    ```
    ![](../../images/7/2/004.png)

    For this lab I have used a dedicated account that i have called testscp in an OU that I have called **scptest**.

    ![](../../images/7/2/005.png)

1. Once the SCPs are created you can attach them to the OU with our test account within.

    ![](../../images/7/2/006.png)

    ![](../../images/7/2/007.png)

1. Now you have applied the SCPs to our OU, log into the account in the OU and lets validate the expected outcomes of our SCPs. Lets forllow the below table to see what happens when launching and EC2 instance.


    | **Tag Enforcement Test** |	**Outcome**	| **Is this the Expected Result** |
    |---------------------|-----------------|------------------------------|
    | without tags	| launch failed	| Yes |
    | with random tag key and value	| launch failed	| Yes |
    | with tag key CostCenter and wrong tag value	| launch failed	| Yes |
    | with tag key team only and correct tag value	| launch failed	| Yes |
    | with both tag keys (CostCenter & team) and correct tag value	| launch success	| Yes |

    Failed launches will drop an error in the AWS console that looks like the picture below when the SCP has been breached. In the below screenshot you tried to launch an instance without any tags:

    ![](../../images/7/2/008.png)

    Tag policy enforcement will also show up here, in this case, when CostCenter tag is incorrectly configured.

    ![](../../images/7/2/009.png)

    When you have both the keys of CostCenter & team configured on the instance and volumes with the correct values, you will be able to launch the instance correctly.

    ![](../../images/7/2/010.png)

1. Try the same validations for the delete tag SCP. Use the table below:

    |	**Tag Enforcement Test**	|	**Outcome** |	**Is this the Expected Result** |
    |-----------------------|-----------------|---------------------------|
    |	add a new random tag key/value	|	success |	Yes |
    |	remove the random tag key / value	|	success |	Yes |
    |	remove the tag CostCenter	|	error |	Yes |
    |	remove the tag team |	error	|	Yes |

### Conclusion
By utilizing SCPs alongside AWS Tag Policies explained in this post, customers can achieve consistency in coverage, discoverability, and enforcement of resource tags by using a centralized tagging governance framework. Companies of any size can adopt this proactive approach to resource tagging enforcement as part of the broader cloud governance framework.