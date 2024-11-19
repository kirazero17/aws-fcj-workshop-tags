---
title: "Create a stack-based resource group"
weight: 1
chapter: false
pre: "<b> 3.1 </b>"
---

### Create the CloudFormation stack-based resource group

The following procedure shows how to build a stack-based query and use it to create a resource group.

1. Navigate to the [AWS Resource Groups](https://eu-west-1.console.aws.amazon.com/resource-groups/) console and choose **Create a resource group**.

1. On **Create query-based group**, under **Group type**, choose the **CloudFormation stack based** group type.

1. From the **CloudFormation stack** dropdown menu, choose the tagging-workload stack.

1. Choose resource types in the stack that you want to include in the group. For this lab, keep the default, **All supported resource types**. For more information about which resource types are supported and can be in the group, see [Resource types you can use with AWS Resource Groups and Tag Editor](https://docs.aws.amazon.com/ARG/latest/userguide/supported-resources.html).

1. Choose **Preview group resources** to return the list of resources shown under **Group resources**. These are resources in the CloudFormation stack that match your selected resource types.

1. After you have the results that you want, create a group based on this query. Under **Group details**, for **Group name**, type a name for your resource group. In this case, you will enter tagging-workshop-cf as the **Group name**.

1. (Optional) In **Group tags**, add tag key and value pairs that apply only to the resource group, not the member resources in the group. Group tags are useful if you plan to make this group a member of a larger group. Because specifying at least a tag key is required to create a group, be sure to add at least a tag key in Group tags to groups that you plan to nest into larger groups.

1. Choose **Create group**.

    ![Newly created resource group](../../images/3/1/001-resource_group_created.png)

### Conclusion
In this lab you created a CloudFormation based Resource Group.

### Clean this section up

1. Navigate to Saved Resource Groups.

1. Choose the group under Group name.

1. Choose Delete and confirm.

    ![Delete the newly created resource group](../../images/3/1/002-delete_resource_group.png)

**_Do not cleanup any further resources yet if you will be moving onto further sections._**