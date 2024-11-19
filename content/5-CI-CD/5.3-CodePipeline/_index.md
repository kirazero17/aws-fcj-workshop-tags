---
title: "Creating the CodePipeline"
weight: 3
chapter: false
pre: "<b> 5.3 </b>"
---

This lab walks through integrating AWS CloudFormation Guard into an AWS CodePipeline  to automate and simplify pre-deployment compliance checks of your AWS CloudFormation templates.

This lab has been created based around the blog: [Integrating AWS CloudFormation Guard into CI/CD pipelines](https://aws.amazon.com/blogs/devops/integrating-aws-cloudformation-guard/).

### Create the IAM Resources

The CI/CD pipeline needs two AWS Identity and Access Management (IAM) roles to run properly: one role for CodePipeline to work with other resources and services, and one role for AWS CloudFormation to run the deployments that passed the validation check in the CodeBuild phase.

For this section you will create a CloudFormation stack to build your IAM resources. See IAM Documentation for more information on IAM.

1. In your AWS Cloud9 environment, runn the following commands in the terminal to pull down the CloudFormation YAML file into a newly created directory:

    ```bash
    cd ~/environment
    mkdir iamcfn
    cd iamcfn
    curl 'https://static.us-east-1.prod.workshops.aws/public/bc005028-76d7-42ac-9cb2-fed686ce81e0/static/templates/pipeline_iam.yaml' --output pipeline_iam.yaml
    ```

2. Create the CloudFormation stack to create your IAM roles required by running the following commands in the terminal:

    ```bash
    aws cloudformation create-stack --stack-name guard-demo-iam-stack --template-body file://pipeline_iam.yaml --capabilities CAPABILITY_NAMED_IAM
    ```

You can view the created stack in the [CloudFormation console](https://console.aws.amazon.com/cloudformation/).

### Creating a pipeline

You can now create your pipeline to assemble all the components into one managed, continuous mechanism.

1. From the **AWS Developer Tools console**, use the navigation pane to choose Pipeline then choose Pipelines.

1. Choose **Create pipeline**.

1. For **Pipeline name**, enter `**cloudformationguarddemo**`.

1. Leave the remaining defaults up until **Service role**.

1. For **Service role**, select **Existing service role** and select `**codepipeline-cfnguard-demo-role**`.

1. Leave the remaining defaults and choose **Next**.

1. In the **Source section**, for **Source provider**, choose **AWS CodeCommit**.

1. For **Repository name**, choose your repository name. In this case, choose `**cfn-guard-demo**`.

1. For **Branch name**, choose `master`.

1. For **Change detection options**, select **Amazon CloudWatch Events**.

1. For **Output artifact format**, keep the default. In this case, **CodePipeline default**.

1. Choose **Next**.

    ![](../../images/5/3/001.png)

1. In the **Build section**, for **Build provider**, choose **AWS CodeBuild**.

1. For **Project name**, choose the CodeBuild project you created. In this case, choose `**cfn-guard-demo**`.

1. For **Build type**, select **Single build**.

1. Choose **Next**.

    Now you will create a deploy stage in your CodePipeline to deploy CloudFormation templates that passed the CloudFormation Guard inspection in the CI stage.

1. In the **Deploy section**, for **Deploy provider**, choose **AWS CloudFormation**.

1. For **Action mode**, choose **Create or update stack**.

1. For **Stack name**, enter `**cfn-guard-deployment-example**`.

1. For **Artifact name**, choose `**BuildArtifact**`.

1. For **File name**, enter the CloudFormation template name in your CodeCommit repository. In this case it is `**cfn_template_file_example.yaml**`.

1. For **Role name**, choose the role you created earlier for CloudFormation cf-cfn-guard-demo-role.

1. Leave the remaining defaults and choose **Next**.

    ![](../../images/5/3/002.png)

1. In the next step review your selections for the pipeline to be created. The stages and action providers in each stage are shown in the order that they will be created. After you have reviewed your selections, choose **Create pipeline**.

    ```
    cfn_template_file_example.yaml Status = FAIL
    FAILED rules
    cfn_guard_ruleset_example.guard/validate_env_tag        FAIL
    ---

    Evaluating data cfn_template_file_example.yaml against rules cfn_guard_ruleset_example.guard
    Number of non-compliant resources 1
    Resource = S3Bucket {
    Type      = AWS::S3::Bucket
    Rule = validate_env_tag {
        ALL {
        Check =  Tags[*].Value IN  ["dev","test","prod"] {
            ComparisonError {
            Message          = Tag key 'environment' not set correctly. Check both the Key & Value
            Error            = Check was not compliant as property [/Resources/S3Bucket/Properties/Tags/0/Value[L:15,C:16]] was not present in [(resolved, Path=[L:0,C:0] Value=["dev","test","prod"])]
            }
            PropertyPath    = /Resources/S3Bucket/Properties/Tags/0/Value[L:15,C:16]
            Operator        = IN
            Value           = "deeev"
            ComparedWith    = [["dev","test","prod"]]
            Code:
                13.            SSEAlgorithm: AES256
                14.     Tags:
                15.       - Key: "environment"
                16.         Value: "deeev"
                17.       - Key: "service"
                18.         Value: "myservice"

        }
        Check =  Tags[*].Value IN  ["dev","test","prod"] {
                ComparisonError {
                Message          = Tag key 'environment' not set correctly. Check both the Key & Value
                Error            = Check was not compliant as property [/Resources/S3Bucket/Properties/Tags/1/Value[L:17,C:16]] was not present in [(resolved, Path=[L:0,C:0] Value=["dev","test","prod"])]
                }
                PropertyPath    = /Resources/S3Bucket/Properties/Tags/1/Value[L:17,C:16]
                Operator        = IN
                Value           = "myservice"
                ComparedWith    = [["dev","test","prod"]]
                Code:
                    15.       - Key: "environment"
                    16.         Value: "deeev"
                    17.       - Key: "service"
                    18.         Value: "myservice"

                }
            }
        }
    }
    ```

### Cleaning up this section

To avoid incurring future charges, delete the resources that you have created during the walkthrough:

- **CloudFormation stack resources that were deployed by the CodePipeline**

- **CodePipeline that you have created**

- **CodeBuild project**

- **CodeCommit repository**