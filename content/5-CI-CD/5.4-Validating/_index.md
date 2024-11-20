---
title: "Under Construction"
weight: 4
chapter: false
pre: "<b> 5.4 </b>"
---

Our CodePipeline has two basic flows and outcomes. If the CloudFormation template complies with our CloudFormation Guard rule set file, the resources in the template deploy successfully.

If our CloudFormation template doesn’t comply with the policies specified in our CloudFormation Guard rule set file, our CodePipeline stops at the CodeBuild step and you see an error in the build job log indicating the resources that are non-compliant:

### Validation

1. From the AWS Developer Tools console , use the navigation pane to choose Pipeline then choose Pipelines.

1. Choose the pipeline you created in the previous lab. In this case, choose `cloudformationguarddemo`.

1. Review the **Build stage**. Choose **View details** to review the CodeBuild logs. You will be able to see the entire failure reason in the log from the cfn-guard output. After you have finished reviewing the logs, choose **Done**.

1. Now you will correct the tag in the AWS Cloud9/VSCode Server IDE. Open the `cfn_template_file_example.yaml` file and add the below with the correct values. After you have finished editing the file, ensure you save the file after making the update:

    ```yaml
    Resources:
    S3Bucket:
    Type: "AWS::S3::Bucket"
    Properties:
        PublicAccessBlockConfiguration:
            BlockPublicAcls: true
            BlockPublicPolicy: true
            IgnorePublicAcls: true
            RestrictPublicBuckets: true
        BucketEncryption:
        ServerSideEncryptionConfiguration:
            - ServerSideEncryptionByDefault:
                SSEAlgorithm: AES256
        Tags:
        - Key: "environment"
            Value: "dev"
        - Key: "service"
            Value: "myservice"
    ```

1. You will now push the changes to the CodeCommit repository by running the following commands in the AWS Cloud9/VSCode Server terminal:

    ```bash
    cd ~/environment/cfn-guard-demo/
    git add -A
    git commit -m "Fix tags on bucket"
    git push 
    ```
    If you check the CodeBuild logs for the latest build, you will now see that the validation passes, and the bucket is created:

    ```
    [Container] 2023/08/15 13:31:53 Running command cfn-guard validate -d $CF_TEMPLATE -r $CF_ORG_RULESET

    [Container] 2023/08/15 13:31:53 Phase complete: POST_BUILD State: SUCCEEDED
    ```

1. You should see that you have successfully created the bucket through creation of the stack in the **CloudFormation** console. Choose the `cfn-guard-deployment-example` stack and choose Resources to view the S3 bucket:

    ![](../../images/5/4/001.png)

7. In the S3 console, you should see the expected tags on your cfn-guard-deployment-example bucket. Choose your bucket then choose Properties to view the tags that have been applied to the bucket.

    ![](../../images/5/4/002.png)

### Cleaning up this section

To avoid incurring future charges, delete the resources that you have created during the walkthrough:

- **CloudFormation stack resources that were deployed by the CodePipeline**

- **CodePipeline that you have created**

- **CodeBuild project**

- **CodeCommit repository**