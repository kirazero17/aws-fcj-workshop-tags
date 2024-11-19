---
title: "Create a tag-based resource group"
weight: 3
chapter: false
pre: "<b> 3.3 </b>"
---

In much the same way as you created a CloudFormation stack-based resource group, you can create one that is tag-based. This is useful as you may have tags that span multiple CloudFormation templates that you want to group so you can view resources spread across multiple components within an account. In this example, you will setup a resource group based on the tag you created around operations ownership.

### Create the tag-based resource group

The following procedure shows how to build a tag-based query and use it to create a resource group:

1. Navigate to the [AWS Resource Groups](https://eu-west-1.console.aws.amazon.com/resource-groups/) console and choose Create a resource group.

1. On **Create query-based group**, under ***Group type***, choose the **Tag based** group type.

1. Choose resource types in the stack that you want to include in the group. For this lab, keep the default, All supported resource types. For more information about which resource types are supported and can be in the group, see [Resource types you can use with AWS Resource Groups and Tag Editor](https://docs.aws.amazon.com/ARG/latest/userguide/supported-resources.html).

1. Still under **Grouping criteria**, for **Tags**, specify a tag key, or a tag key and value pair, to limit the matching resources in the current region to include only those that are tagged with your specified values. In this case, you will enter the following tag and choose Add or press Enter when finished adding the tag.

    ```
    key = owner
    value = workshop
    ```

1. Choose **Preview group resources** to return the list of resources shown under Group resources. These are resources in the account that match your selected resource types and tag.

1. After you have the results that you want, create a group based on this query. Under **Group details**, for **Group name**, type a name for your resource group. In this case, you will enter tagging-workshop-tag as the **Group name**.

1. In **Group tags**, enter the tag key owner and value workshop to apply only to the resource group. Group tags are useful if you plan to make this group a member of a larger group. Because specifying at least a tag key is required to create a group, be sure to add at least a tag key in Group tags to groups that you plan to nest into larger groups.

1. Choose **Create group**.

![Create Resource Group](../../images/3/3/001-tag_the_group.png)

After the group is created, you should notice a new resource identifier has appeared.

![New Resource Group Created](../../images/3/3/002-res_group.png)

### Clean this section up

1. Navigate to **Saved Resource Groups**.

1. Choose the group under **Group name**.

1. Choose **Delete** and confirm.

![Delete RG](../../images/3/3/003-delete_resource_group.png)