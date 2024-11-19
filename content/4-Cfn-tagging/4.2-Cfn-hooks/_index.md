---
title: "AWS CloudFormation Hooks"
weight: 2
chapter: false
pre: "<b> 4.2 </b>"
---

### What are AWS CloudFormation Hooks?
Hooks proactively inspect the configuration of your AWS resources before provisioning. If non-compliant resources are found, AWS CloudFormation returns a failure status and either fails the operation or provides a warning and allows the operation to continue based on the hook failure mode. You can use pre-built hooks or build your own hooks using the CloudFormation Command Line Interface (CloudFormation CLI) .

For this lab you will use a hook example from the [aws-cloudformation](https://github.com/aws-cloudformation) github repository.

### Install CloudFormation Hooks in the AWS Cloud9 environment

1. Open the AWS Cloud9 terminal and run the following commands to install the Cloudformation CLI and plugins.

    ```bash
    pip install cloudformation-cli cloudformation-cli-java-plugin cloudformation-cli-go-plugin cloudformation-cli-python-plugin cloudformation-cli-typescript-plugin
    ```

1. You will use a pre-created hook for this lab. Run the following command in the terminal to create a folder called resource-tags in our AWS Cloud9 environment. This may take a few minutes to complete.

    ```bash
    cd ~/environment
    git clone https://github.com/aws-cloudformation/aws-cloudformation-samples.git 
    cd aws-cloudformation-samples/hooks/python-hooks/resource-tags/
    python update_hook.py
    ```

1. In the navigation pane, choose and expand the aws-cloudformation-samples/hooks/python-hooks/resource-tags folder and then open the file type_config.json to configure your required tags. In the file type_config.json you are going to edit the "TagKeys" value to match the following. Ensure to save after making the edits.

    ```json
    {
        "CloudFormationConfiguration": {
            "HookConfiguration": {
                "TargetStacks": "ALL",
                "FailureMode": "FAIL",
                "Properties": {
                    "TagKeys": "environment=dev|test|prod, service",
                    "ValidationStrategy": "resource"
                }
            }
        }
    }
    ```

1. Once step 2 and 3 have completed, your files will be updated and the build of your project will start after running the following command in the terminal. This may take a few minutes to complete.

    ```bash
    cfn generate -vv && cfn submit -vv --dry-run
    ```
    You should see the following return:

    ```
    Generated files for AWSSamples::ResourceTags::Hook
    Starting Docker build. This may take several minutes if the image 'public.ecr.aws/sam/build-python3.9' needs to be pulled first.
    Dry run complete: /home/ec2-user/environment/resource-tags/awssamples-resourcetags-hook.zip
    ```

### Register and activate your Hook

1. Once the previous step has completed, run the following command in the terminal to register your Hook.

    ```bash
    cfn submit -vv --set-default
    ```
    You should see the following return:

    ```
    Starting Docker build. This may take several minutes if the image 'public.ecr.aws/sam/build-python3.9' needs to be pulled first.
    Successfully submitted type. Waiting for registration with token 'a1b2c3d4-5678-90ab-cdef-EXAMPLE11111' to complete.
    Registration complete.
    {'ProgressStatus': 'COMPLETE', 'Description': 'Deployment is currently in DEPLOY_STAGE of status COMPLETED', 'TypeArn': 'arn:aws:cloudformation:eu-west-1:111122223333:type/hook/AWSSamples-ResourceTags-Hook', 'TypeVersionArn': 'arn:aws:cloudformation:eu-west-1:111122223333:type/hook/AWSSamples-ResourceTags-Hook/00000001', 'ResponseMetadata': {'RequestId': 'a1b2c3d4-5678-90ab-cdef-EXAMPLE11111', 'HTTPStatusCode': 200, 'HTTPHeaders': {'x-amzn-requestid': 'a1b2c3d4-5678-90ab-cdef-EXAMPLE11111', 'date': 'Tue, 08 Aug 2023 15:06:06 GMT', 'content-type': 'text/xml', 'content-length': '685', 'connection': 'keep-alive'}, 'RetryAttempts': 0}}
    Set default version to 'arn:aws:cloudformation:eu-west-1:111122223333:type/hook/AWSSamples-ResourceTags-Hook/00000001
    ```

    Let's look at the above return in json format:

    ```json
    {
    "TypeSummaries": [
            {
                "Description": "Example hook to validate resource tags you require.  This hook is designed to run on create and update pre-provision operations of resource types that support tags, and that are supported in this sample hook.  By default, the hook uses AWS resource types: you have the choice of using third-party, registered, and activated resource types as well.  Validations performed by this sample hook include: tag keys you require are specified; values for all tag keys, including non-required keys, are non-empty; tag values are included in allowed values you specify; tag propagation properties are set for supported resource types (resource-level tags only).",
                "LastUpdated": "2023-08-08T15:06:06.046Z",
                "TypeName": "AWSSamples::ResourceTags::Hook",
                "TypeArn": "arn:aws:cloudformation:eu-west-1:111122223333:type/hook/AWSSamples-ResourceTags-Hook",
                "DefaultVersionId": "00000001",
                "Type": "HOOK"
            }
        ]
    }
    ```
1. Copy your Hook type ARN on your local machine. Paste your Hook type ARN removing the version ID at the end so this ends in "AWSSamples-ResourceTags-Hook", then run the following command in the terminal.

    ```bash
    export HOOK_TYPE_ARN=arn:aws:cloudformation:eu-west-1:111122223333:type/hook/AWSSamples-ResourceTags-Hook
    ```
1. After you have developed and registered your Hook, you can activate the Hook in your AWS account by publishing it to the registry. Run the following command in the terminal.

    ```bash
    aws cloudformation set-type-configuration \
    --configuration file://type_config.json \
    --type-arn $HOOK_TYPE_ARN
    ```

### Test the Hook

1. Now you'll create a simple non-compliant stack. Run the following commands in the terminal to create the badtag-bucket.yaml.

    ```bash
    cd ~/environment/
    touch badtag-bucket.yaml
    ```
2. In the navigation pane, open your newly created badtag-bucket.yaml. You may have to scroll down the navigation pane to find the file. Once opened, paste the following in the file. Ensure to save after pasting.

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
            Value: "Develop"
    ```

1. You will now create a CloudFormation stack based on the file. You will use the stack name tag-hook-stack. Run the following command in the terminal.

    ```bash
    aws cloudformation create-stack --stack-name tag-hook-stack --template-body file://badtag-bucket.yaml
    ```

1. Review the status of the stack creation by running the following command in the terminal. You can enter q when you are done scrolling through the stack information.

    ```bash
    aws cloudformation describe-stack-events --stack-name tag-hook-stack
    ```
    You can see the failure here and that it was the hook that failed the creation:
    ```
    "StackId": "arn:aws:cloudformation:eu-west-1:111122223333:stack/tag-hook-stack/a1b2c3d4-5678-90ab-cdef-EXAMPLE11111",
    "EventId": "S3Bucket-CREATE_FAILED-2023-08-09T07:54:08.881Z",
    "ResourceStatus": "CREATE_FAILED",
    "ResourceType": "AWS::S3::Bucket",
    "Timestamp": "2023-08-09T07:54:08.881Z",
    "ResourceStatusReason": "The following hook(s) failed: [AWSSamples::ResourceTags::Hook]",
    "StackName": "tag-hook-stack",
    "ResourceProperties": "{\"PublicAccessBlockConfiguration\":{\"RestrictPublicBuckets\":\"true\",\"BlockPublicPolicy\":\"true\",\"BlockPublicAcls\":\"true\",\"IgnorePublicAcls\":\"true\"},\"BucketEncryption\":{\"ServerSideEncryptionConfiguration\":[{\"ServerSideEncryptionByDefault\":{\"SSEAlgorithm\":\"AES256\"}}]},\"Tags\":[{\"Value\":\"Develop\",\"Key\":\"environment\"}]}",
    "PhysicalResourceId": "",
    "LogicalResourceId": "S3Bucket"
    ```
    You can also review this from within the [CloudFormation console](https://console.aws.amazon.com/cloudformation/).

    ![](../../images/4/2/001-hook_failure_service.png)

1. Now you will add in the service tag. You will keep the value for environment incorrect to check the hook fails for an incorrect value as well. First, delete our original CloudFormation stack by running the following command in the terminal.

    ```bash
    aws cloudformation delete-stack --stack-name tag-hook-stack
    ```
1. Re-run the describe command to check its deleted by running the following command in the terminal.

    ```bash
    aws cloudformation describe-stack-events --stack-name tag-hook-stack
    ```
    You should receive the following error:
    ```
    An error occurred (ValidationError) when calling the DescribeStackEvents operation: Stack [tag-hook-stack] does not exist
    ```

1. You will now edit the content of badtag-bucket.yaml to add a second tag to the S3 bucket. Ensure to save after making the following edits to the file.

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
            Value: "Develop"
        - Key: "service"
            Value: "testvalue"
    ```

1. You will now re-create the stack by running the following command.

    ```bash
    aws cloudformation create-stack --stack-name tag-hook-stack --template-body file://badtag-bucket.yaml
    ```

1. Now run the following command to check the status of the stack creation.

    ```bash
    aws cloudformation describe-stack-events --stack-name tag-hook-stack
    ```
    You see again that the creation failed:
    ```
    "StackId": "arn:aws:cloudformation:eu-west-1:111122223333:stack/tag-hook-stack/a1b2c3d4-5678-90ab-cdef-EXAMPLE11111",
    "EventId": "S3Bucket-CREATE_FAILED-2023-08-09T08:15:41.811Z",
    "ResourceStatus": "CREATE_FAILED",
    "ResourceType": "AWS::S3::Bucket",
    "Timestamp": "2023-08-09T08:15:41.811Z",
    "ResourceStatusReason": "The following hook(s) failed: [AWSSamples::ResourceTags::Hook]",
    "StackName": "tag-hook-stack",
    "ResourceProperties": "{\"PublicAccessBlockConfiguration\":{\"RestrictPublicBuckets\":\"true\",\"BlockPublicPolicy\":\"true\",\"BlockPublicAcls\":\"true\",\"IgnorePublicAcls\":\"true\"},\"BucketEncryption\":{\"ServerSideEncryptionConfiguration\":[{\"ServerSideEncryptionByDefault\":{\"SSEAlgorithm\":\"AES256\"}}]},\"Tags\":[{\"Value\":\"Develop\",\"Key\":\"environment\"},{\"Value\":\"testvalue\",\"Key\":\"service\"}]}",
    "PhysicalResourceId": "",
    "LogicalResourceId": "S3Bucket"
    ```
    You can review the stack in the [CloudFormation console](https://console.aws.amazon.com/cloudformation/) to see what the failure message is this time.

    ![](../../images/4/2/002-hook_failure_value.png)

### Additional challenge
For an additional challenge, try setting the values in your stack to be correct so that the hook passes. Deploy the stack to create the bucket with the require tags. Remember to delete the failed stack first so you don't get conflicts.

### Conclusion
In this lab you were able to create a CloudFormation Hook to proactivly prevent CloudFormation stacks from deploying if the required tags were not present. You also walked through how developers can troubleshoot the stack to check for failure resons when failed by hooks.

### Cleanup
You can delete your stack and deregister your hook by running the following commands.

#### Delete Stack

```bash
aws cloudformation delete-stack --stack-name tag-hook-stack
```
Re-run the describe command to check its deleted
```bash
aws cloudformation describe-stack-events --stack-name tag-hook-stack
```
You should recieve this error
```
An error occurred (ValidationError) when calling the DescribeStackEvents operation: Stack [tag-hook-stack] does not exist
```

#### Deregister the hook

```bash
aws cloudformation deregister-type --arn $HOOK_TYPE_ARN
```
You can check this has been removed in the console under registry > Activated extensions. Be sure to select your Filter extension type to be Privately registered.

![](../../images/4/2/003-dereg_hook.png)