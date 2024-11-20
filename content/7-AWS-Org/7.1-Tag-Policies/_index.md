---
title: "AWS Tag Policies"
weight: 1
chapter: false
pre: "<b> 7.1 </b>"
---

In this section, you’ll learn how to use AWS Tag Policies to help you standardize tags across resources in your environment. As an example, you can build policy to enforce naming convention for tag keys and values. This can prevent users from entering tag key or value that did not align with standard that you have established. Standardization is important in tag governance, with consistent tag key and values, you can perform analysis of usage and spend, perform automations and achieve predictable results.

### Overview
[AWS Organizations, Tag policies](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_tag-policies.html) are a type of policy that can help you standardize tags across resources in your organization's accounts. In a tag policy, you specify tagging rules applicable to resources when they are tagged. Each tag has two parts:

- A tag `key` (for example, CostCenter, Environment, or Project). Tag keys are case sensitive.
- An optional field known as a tag `value` (for example, 111122223333 or Production). Omitting the tag value is the same as using an empty string. Like tag keys, tag values are case sensitive.

For example, a tag policy can specify that when the CostCenter tag is attached to a resource, it must use the case treatment and tag values that the tag policy defines. A tag policy can also specify that noncompliant tagging operations on specified resource types are enforced. In other words, noncompliant tagging requests on specified resource types are prevented from completing. Untagged resources or tags that aren't defined in the tag policy aren't evaluated for compliance with the tag policy.

Tag policies are managed from AWS Organizations, you start by creating Tag policies and then apply them to target accounts or OUs.

In the following section, you will create a tag policy to standardize the `CostCenter` tag with predefined values, and then you will apply it to the target account and run a test. At the end of the lab, you will create a compliance report to evaluate the results.

**_Important :_**

- Tag policies **did not** mandate or enforce that certain tag key must be present when creating / updating AWS resources.

- Tag policies enforcement applies when certain tag key are present and the policy defines what tag values and case sensitivity will apply.

- Untagged resources or tags that aren't defined in the tag policy aren't evaluated for compliance with the tag policy

For example, a tag policy can specify that when the CostCenter tag is attached to a resource, it must use the case treatment and tag values that the tag policy defines. A tag policy can also specify that noncompliant tagging operations on specified resource types are enforced. In other words, noncompliant tagging requests on specified resource types are prevented from completing. Untagged resources or tags that aren't defined in the tag policy aren't evaluated for compliance with the tag policy.

Tag policies are managed from AWS Organizations, you start by creating Tag policies and then apply them to target accounts or OUs.

In the following section, you will create a tag policy to standardize the CostCenter tag with predefined values, and then you will apply it to the target account and run a test. At the end of the lab, you will create a compliance report to evaluate the results.

### Prerequisites

- This lab requires an account using AWS Organizations.

- An AWS account for the test, you can use preexisting non-production accounts.

### 1. Enable Tag Policies
First, you need to enable **Tag Policies** before you can start using it.

1. Sign in to **Organization** admistrator account using Administrator role.

1. Navigate to [AWS Organizations](https://console.aws.amazon.com/organizations/v2/home/).

1. Choose **Policies** from the side panel.

1. Choose **Tag policies**.

1. If required, choose **Enable tag policies** (it's normal if you dont see this selection, it means that Tag policies has been enabled previously).

1. Next you want to grant Tag policies as trusted service in your AWS Organizations.

1. Choose **Services** from the side panel.

1. Locate and choose **Tag Policies**.

1. Choose **Enable trusted access**.

1. Choose the checkbox _"Show the option to enable trusted access for Tag policies without performing additional setup tasks."_.

1. Type enable on the text box and choose **Enable trusted access**.

### 2. Create Tag Policies
Next, let's create simple tag policy for CostCenter. For this demonstration purpose, you want to enforce capitalization of CostCenter and set two predefined value `A001` and `B001`.

1. Return back to the tag policies console, use this [AWS Organizations, Tag Policies](https://console.aws.amazon.com/organizations/v2/home/policies/tag-policy) if needed.

1. Choose **Create policy**.

1. Set **Policy name** for example: `CostCenter`.

1. Choose the **Visual editor**.

1. Enter the **Tag key** value : `CostCenter`.

1. To enforce capitalization compliance, choose the check box _"Use the capitalization that you've specified above for the tag key"_.

1. To restrict the predefined tag values, choose check box **"Specify allowed values for this tag key"**.

1. Choose Specify values and then enter two predefiend value `A001` and `B001`.

1. Choose **Save changes** to confirm.

1. To define the scope, choose check box _"Prevent noncompliant operations for this tag"_.

1. Choose **Specify resource types**, expand the tree selection and choose `ec2:volume` & `ec2:instance`.

1. Choose **Save changes** to confirm.

1. Your configuration should be similar to example screenshot below.

    ![](../../images/7/1/001.png)

1. Choose **Create policy** to finish this step.

### 3. Understanding Tag policies syntax

Now its a good time to review the tag policies that you just created earlier.

1. Return back to the tag policies console, use this [AWS Organizations, Tag Policies](https://console.aws.amazon.com/organizations/v2/home/policies/tag-policy) if needed.

1. Choose the **CostCenter** policies that you just created earlier.

1. Under the **Content** section, you can see the actual policy document:

    ```json
    {
        "tags": {
            "CostCenter": {
                "tag_key": {
                    "@@assign": "CostCenter"
                },
                "tag_value": {
                    "@@assign": [
                        "A001",
                        "B001"
                    ]
                },
                "enforced_for": {
                    "@@assign": [
                        "ec2:volume"
                    ]
                }
            }
        }
    }
    ```
1. Let's dive deeper to this tag policy.

    - This policy has a single **tag_key** set to `CostCenter`. It then set **tag_value** with two possible options `A001` and `B001`. Keep in mind that you can create a different policy with the same **tag_key** set to `CostCenter` and assign a different **tag_value**. For example, each OUs will have an unique tag policies.

    - When defining the **tag_value** you could utilize wildcard to keep it simpler. As an example you could re-write the policy to `A00*` to allow any values starting with **_"A00"_**

    - This policy is enforced to specific AWS resources only, in this case `ec2:volume`. Keep in mind at the moment you can't use wildcard * to select all resources. However, some services allows you to use a wildcard such as `kms:*` on your policy statement. Refer to [supported resource enforcement](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_supported-resources-enforcement.html) for the full list.

    - The operator `@@assign` will overwrites or set the value. When dealing with only a single policy, the overwrites operator didn't bring much value. However, you can utilize it when you are dealing with multiple policies in a nested OU structure but it is outside the scope of this labs.

    - For detailed example, use the link to refer to [types of operators](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_inheritance_mgmt.html#policy-operators)

    - For complete reference documentation, please check out the [Tag policy syntax and examples](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_example-tag-policies.html)

### 4. Attach Tag policies to target OU

Now let us apply the tag policies to target OU.

1. Return back to the tag policies console, use this AWS Organizations, Tag Policies  if needed.

1. Choose the CostCenter policies that you just created earlier.

1. Under the Targets section, choose Attach.

1. On the OU selection, choose one account as the target.

- Choose a non-production test account as per your preferences.
- **Important :** Please be mindful of the effect of this policy on the target account.
- Navigate to your chosen OU.
- Select **Attach policy** to confirm.

In practice, you could attach the policy at the OU or account level.

### 5. Testing the tag policies enforcement

1. Sign in to your chosen account using Administrator role.

1. Navigate to EC2 console, you can use this [EC2 console](https://us-west-2.console.aws.amazon.com/ec2/v2/home#) as a shortcut.

1. Tag policies apply at account level, thus it doesn't matter which region that you choose.

1. From EC2 console, navigate to Volumes by selecting from the side-panel.

1. Go ahead and create a new volume, the volume type or size does not matter in this case.

    - As guidance, choose volume type **gp2** and set the size to **1 GiB**.
    - Choose **Add Tag**.
    - You will intentionally set it wrong to test the policies:
        - Enter key : `costcenter`
        - Enter value : `C001`
    - Select **Create Volume**.

1. You should receive error about wrong capitalization for **_CostCenter_**.

    ![](../../images/7/1/002.png)

1. Choose **Back** to return to the create volume section.

1. Go ahead and rename the key to `CostCenter` to fix the capitalization. Choose **Create Volume** again to proceed.

1. You should receive new error stating that the specified value is not allowed.

    ![](../../images/7/1/003.png)

1. Choose **Back** to return to the create volume section.

1. Let's do the final fix, rename the value from C001 to A001. Choose Create Volume again to proceed.

1. You should see confirmation that the volume created successfully.

1. To avoid further charges, go ahead and delete the volume.

We have successfully demonstrated how to use Tag policies to enforce standard capitalization for the tag keys and to setup predefined list of tag values.

In the following section, we'll modify the tag policies and explore how to generate report for tag compliance.

### 6. Using Tag policies without enforcement

Earlier we set the tag policies with enforcement applied to **s** and you succesfully tested the enforcement. Now let's go back to AWS Organization management account to modify the tag policies.

1. Sign in to AWS Organization management account using Administrator role.

1. Return back to the tag policies console, use this [AWS Organizations, Tag Policies](https://console.aws.amazon.com/organizations/v2/home/policies/tag-policy) if needed.

1. Choose the **CostCenter** tag policies.

1. Choose **Edit policy**.

1. Uncheck the check box _"Prevent noncompliant operations for this tag"_.

1. Choose **Save Changes**.

Notice how the tag policies document now changed and there are no enforced_for section in the policy document.

### 7. Testing the tag policies report at account level

In this section, you will navigate back to chosen account, create EBS volume that did not match the policy and then view the report.

1. Sign in to chosen account using Administrator role.

1. Navigate to EC2 console, you can use [this](https://us-west-2.console.aws.amazon.com/ec2/v2/home#) as a shortcut.

1. Tag policies apply at account level, thus it doesn't matter which region that you choose.

1. From EC2 console, navigate to **Volumes** by selecting from the side-panel.

1. Go ahead and create a new volume, the volume type or size does not matter in this case.

    - As guidance, choose volume type *gp2* and set the size to *1 GiB*.
    - Select **Add Tag**.
    - You will intentionally set the value wrong to test the report:
        - Enter key : `CostCenter`
        - Enter value : `C001`
    - Choose **Create Volume**.

1. Notice that you are able to successfully created the volume since the enforcement was not in place.

1. Navigate to AWS Resource Groups  as shortcut

1. Select **Tag Policies** from the side panel.

1. To search for non-compliant resources, filter by the region where you create the volume. Choose **Search resources**.

1. You can find the volume that you created earlier listed as non-compliant.

    ![](../../images/7/1/004.png)

1. Clicking on the noncompliant status on the table will pop up further details about the non-compliant item.

    ![](../../images/7/1/005.png)

1. Let's go ahead and delete the EBS volume again to avoid further charges.

As bonus item, you can explore the [Tag Editor](https://console.aws.amazon.com/resource-groups/tag-editor/find-resources) to search any resources based on certain tag criteria and do bulk update as necessary. To read more about Tag Editor, please check out the [documentation link](https://docs.aws.amazon.com/ARG/latest/userguide/tag-editor.html).

### 8. Managing tag compliance at scale

Earlier you saw how to check tag compliance at individual account. In this last section, you will explore how to review and generate report for tag compliance across all accounts in your AWS Organization environment.

1. Sign in to AWS Organization management account using Administrator role.

1. Navigate to [AWS Resource Groups](https://console.aws.amazon.com/resource-groups/home)

1. Choose **Tag Policies** from the side panel.

1. Notice the UI selection is different compared at the individual account.

1. You can choose on each individual account to retrieve the compliance status. Please keep in mind that it will take up to 48 hours to propagate the results at AWS Organization level.

1. You can also generate report by specifying the S3 bucket as target. Please check out example report below for your references

    ![](../../images/7/1/006.png)

Further detail about the content of the report can be found in the [documentation](https://docs.aws.amazon.com/ARG/latest/userguide/tag-policies-arg-evaluating-org-wide-compliance.html).

Before you end the lab, please check the last section below to clean up any resources that you deployed in this lab.

### 9. Deleting AWS resources deployed in this lab
To decommission the lab, let's delete the tag policies that you created earlier.

1. Sign in to AWS Organization management account using Administrator role.

1. Return back to the **AWS Organizations, tag policies**.

1. Choose the **CostCenter** tag policies.

1. Choose **Targets** section.

1. Choose the target which should be the chosen account and choose **Detach**.

1. Choose **Delete** from the top menu.

1. Confirm it by entering the policy name `CostCenter` and choose **Delete**.

Congratulation for completing the lab, if you created additional EBS volumes as part of this lab, don't forget to delete those as well.

### Best Practices

It is highly recommended that you review the collection of Tag policies best practices before you continue with implementation on your production environment.
