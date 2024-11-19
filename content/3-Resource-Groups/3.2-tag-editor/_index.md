---
title: "Tag Editor"
weight: 2
chapter: false
pre: "<b> 3.2 </b>"
---

### Using Tag Editor to add a tag to all supported resources

You first need to take a look at the tags on the resource that will be common to all resources. In the resource group you just created, you can take a look at the system tags to determine a tag common to all resources. This will show us all resources with that system tag applied, in this case from cloudformation.

1. In the navigation pane, under Tagging, choose [Tag Editor](https://console.aws.amazon.com/resource-groups/tag-editor).

1. (Optional) Choose the AWS Regions in which to search for resources to tag. Keep the default as your current region. In this case, you are using `eu-west-1`.

1. From the Resource types dropdown list, choose `AWS::EC2::VPC`.

1. In the **Tags** fields, you can enter a tag key, or a tag key and value pair, to limit the resources in the current region to only those that are tagged with your specified values. In this case, you will enter the following tag and choose Add or press Enter when finished adding the tag.

    ```
    key = Name
    value = tagworkshopvpcec2
    ```

5. When your query is ready, choose Search resources. Results are displayed as a table in the **Resource search results area**.

1. You should now see our Amazon VPC resource has been found with that tag. Select the check box for all resources, and then choose **Manage tags** of selected resources.

    ![Newly created resource group](../../images/3/2/001-f_res.png)

7. On the **Manage tags** page, shown below, view the tags on the resources that you selected. Although your original query returned more resources, you're adding tags to only those resources that you selected in step 1. Choose **Add tag**.

1. Enter a tag key and tag value. For this lab, you'll add the tag key team and the tag value platform.

1. When you're finished adding tags, choose **Review and apply changes**.

1. If you accept the changes, choose **Apply changes to all selected**.

    ![Edit tags](../../images/3/2/002-rev_apply.png)

As an optional step, you could follow this same process to edit or remove a tag. You can also add and remove tags on multiple resources witin the group, or for all group resources.

### Conclusion
In this lab you added a tag to the resource group using Tag Editor.