---
title: "AWS Config Advanced Queries"
weight: 2
chapter: false
pre: "<b> 6.2 </b>"
---

You can use AWS Config to query the current configuration state of AWS resources based on configuration properties for a single account and Region or across multiple accounts and Regions. You can perform ad hoc, property-based queries against current AWS resource state metadata across a list of resources that AWS Config supports. For more information on the list of supported resource types, see [Supported Resource Types for Advanced Queries](https://github.com/awslabs/aws-config-resource-schema/tree/master/config/properties/resource-types).

The [advanced query feature](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) provides a single query endpoint and a powerful query language to get current resource state metadata without performing service-specific describe API calls. You can use configuration aggregators to run the same queries from a central account across multiple accounts and AWS Regions.

### Creating Advanced Queries

In this lab want to look at our tagged resources with not just required keys you have setup currently, but also recomended or optional keys as well. For our EC2 instances, you have decided that you want to make tier a required key within our environments. You know that some instances have this key already, and others don't. Before you go ahead, you will use Advanced Query in AWS Config to check some instance properties to get an idea of coverage.

In our environment, you know that you have four EC2 instances. You will use Advanced queries to check on them.

1. In the [Config Dashboard](https://console.aws.amazon.com/config/), from the navigation pane, choose Advanced queries.

1. Choose **New query** and enter this query in the **New query** box then choose **Run**. Running this query will enable you to see all the **EC2::Instance** resources with the specified tag.

    ```sql
    SELECT
        resourceId,
        resourceType,
        configuration.instanceType,
        configuration.imageId,
        availabilityZone
    WHERE
        resourceType = 'AWS::EC2::Instance'
        AND awsRegion = 'eu-west-1'
        AND tags.tag = 'component=compute'
    ```

1. Now you will alter the query slightly to make sure you don't have any instances without that tag. Enter the following query in the **New query** box then choose **Run**. Running this query will enable you to see all the **EC2::Instance** resources that do not have the specified tag.

    ```sql
    SELECT
        resourceId,
        resourceType,
        configuration.instanceType,
        configuration.imageId,
        availabilityZone
    WHERE
        resourceType = 'AWS::EC2::Instance'
        AND awsRegion = 'eu-west-1'
        AND NOT tags.tag = 'component=compute'
    ```

    ![](../../images/6/2/001.png)

    This shows that you have all your EC2 instances tagged with the required component tag.

1. Enter the following query in the New query box then choose Run. This will show all EC2 instances without the tag key of `tier`.

    ```sql
    SELECT
        resourceId,
        resourceType,
        configuration.instanceType,
        configuration.imageId,
        availabilityZone
    WHERE
        resourceType = 'AWS::EC2::Instance'
        AND awsRegion = 'eu-west-1'
        AND tags.tag = 'component=compute'
        AND NOT tags.key = 'tier'
    ```

1. Now you will select another aspect in the tags so you can see what other tag metadata you have within the output. Enter the following query in the **New query** box then choose **Run**. This query adds `tags.tag` to the `SELECT`.

    ```sql
    SELECT
        resourceId,
        resourceType,
        configuration.instanceType,
        configuration.imageId,
        availabilityZone,
        tags.tag
    WHERE
        resourceType = 'AWS::EC2::Instance'
        AND awsRegion = 'eu-west-1'
        AND tags.tag = 'component=compute'
        AND NOT tags.key = 'tier'
    ```

    ![](../../images/6/2/002.png)

1. In the **Output**, choose the **9 items** link under `tags.tag` link. This shows the tags associated with the EC2 instance. You can see `Name=appserver`. From this, you can then navigate to this instance and apply the required `tier` tag.

    ![](../../images/6/2/003.png)

1. Under **New query** choose **Save query** to save the queries you create for later use.

### Conclusion
AWS Config Advanced Queries allows us to create coplex queries and relationsips between our resources to validate our tagging requirements. The lab focuses on how to do this in an individual account, but it is also possible to do the same at an organization level if you have an aggregator in place.

### Cleaning up this section
To avoid incurring future charges, delete the resources that you have created during the walkthrough:

- **AWS Config rule**

- **AWS Config advanced queries**