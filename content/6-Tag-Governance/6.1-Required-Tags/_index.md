---
title: "AWS Config Required Tags"
weight: 1
chapter: false
pre: "<b> 6.1 </b>"
---

The [required-tags](https://docs.aws.amazon.com/config/latest/developerguide/required-tags.html) rule checks if your resources have the tags that you specify. For example, you can check whether your Amazon EC2 instances have the CostCenter tag, while also checking if all your Amazon RDS instances have a tag. Separate multiple values with commas. You can check up to 6 tags at a time.

You can use this rule to find resources in your account that were not launched with your desired configurations by specifying which resources should have tags and the expected value for each tag. You can also run remediation actions to fix tagging mistakes. However, this rule does not prevent you from creating resources with incorrect tags.

### Ensure AWS Config recording is on

1. Navigate to the [AWS Config Dashboard](https://console.aws.amazon.com/config/), in the correct region.

1. Choose **Settings** in the navigation pane.

1. Under **Recorder** view that **Recording is on**.

### Create The Rule

1. In the navigation pane, choose **Rules** then choose **Add rule**.

1. Select **Add AWS Managed rule**.

1. Search for `required-tags` in the **AWS Managed Rules** search box.

1. Select the **required-tags** rule and choose Next.

1. Keep the defaults to include all resources.

1. Under Parameters, enter the following rule parameters:

    - tag1Key : component
    - tag2Key : resource
    - tag2Value : dedicated, shared
    - tag3Key : owner

1. Choose **Remove** to remove any additonal tag fields that are unused. Then choose **Next**.

    ![](../../images/6/1/001.png)

1. Review the details of your rule, ensuring to check your Parameter values to see what the rule will assess. Once you have finished reviewing, choose **Save**.

    ![](../../images/6/1/002.png)

### Review the dashboard

1. Navigate back to the [Config Dashboard](https://console.aws.amazon.com/config/) to review what has been found by the rule.

2. You can view the results of the rule you just created under Noncompliant rules by noncompliant resource count.

### Investigate The Rule

1. In the dashboard, under Compliance status, choose the **Noncompliant resource(s)*** link.

    ![](../../images/6/1/003.png)


1. Filter the resources viewed by choosing **AWS EC2 InternetGateway** under **Resource Type**.

1. Under **Resource Identifier** choose the Internet Gateway link to navigate to the resource.

1. Choose the **Tags** tab to see why this resource has flagged as _non-compliant_ according to **required-tags** rule.

1. Use the navigation pane and choose **Rules** and then choose the **required-tags** rule.

1. Scroll down to **Resources** in scope and choose **Compliant** from the the dropdown menu.

    ![](../../images/6/1/004.png)

1. Choose the Internet Gateway that you have that is compliant to navigate to the resource.

1. Choose the Tags tab to see why this resource has flagged as compliant according to required-tags rule.

    ![](../../images/6/1/005.png)

### Conclusion
In this lab, you have seen how you can use the required-tags AWS Config rule to check your existing account resources meet the requirments set out for the required tags that you have defined. The lab focuses on how to do this in an individual account, but it is also possible to do the same at an organization level if you have an aggregator in place.

### Cleaning this section up
To avoid incurring future charges, delete the resources that you have created during the walkthrough:

- **AWS Config rule**