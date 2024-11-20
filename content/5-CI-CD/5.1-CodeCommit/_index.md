---
title: "Creating the CodeCommit repository"
weight: 1
chapter: false
pre: "<b> 5.1 </b>"
---

This four part lab walks through integrating AWS CloudFormation Guard into an AWS CodePipeline  to automate and simplify pre-deployment compliance checks of your AWS CloudFormation  templates.

This lab has been created based around the blog: Integrating AWS CloudFormation Guard into CI/CD pipelines.

In these labs we will walk through a number of stages:

1. Creation of the AWS Codecommit repository

1. Creation of the AWS CodeBuild project

1. Creation of the AWS CodePipeline pipeline

1. Validating that the pipeline fails as expected if the correct tags aren't in place

1. Correcting the tags again so the pipeline succeeds

### AWS CodeCommit repository creation

To create the CodeCommit repository, run the following command in the AWS Cloud9/VSCode Server terminal:

    ```bash
    cd ~/environment/
    aws codecommit create-repository --repository-name cfn-guard-demo --repository-description "CloudFormation Guard Demo"
    ```

    You should see the below in the CodeCommit console :

    ![](../../images/5/1/001.png)

### Clone the repository into the AWS Cloud9/VSCode Server environment

Clone your repository into your AWS Cloud9/VSCode Server environment so that you can easily push up your changes. Change the below command to meet your region requirments and run in the terminal:

    ```bash
    git clone https://git-codecommit.eu-west-1.amazonaws.com/v1/repos/cfn-guard-demo && cd cfn-guard-demo && touch buildspec.yml cfn_guard_ruleset_example.guard cfn_template_file_example.yaml && ls -l
    ```
    You should see the following output:

    ```
    Cloning into 'cfn-guard-demo'...
    warning: You appear to have cloned an empty repository.
    total 0
    -rw-rw-r-- 1 ec2-user ec2-user 0 Aug 10 09:18 buildspec.yml
    -rw-rw-r-- 1 ec2-user ec2-user 0 Aug 10 09:18 cfn_guard_ruleset_example.guard
    -rw-rw-r-- 1 ec2-user ec2-user 0 Aug 10 09:18 cfn_template_file_example.yaml
    ```

### Populating the CodeCommit repository

You will now need to populate your repository artifacts.

- buildspec.yml

- cfn_guard_ruleset_example.guard

- cfn_template_file_example.yaml

1. Use the AWS Cloud9/VSCode Server IDE to update the above files. Use the navigation pane to find the folder cfn-guard-demo and open each file. Once opened, modify each file according to the following artifacts. Ensure that you save each file.

    **a. The `buildspec.yml` file:**

    ```yaml
    version: 0.2
    env:
    variables:
        # Definining CloudFormation Teamplate and Ruleset as variables - part of the code repo
        CF_TEMPLATE: "cfn_template_file_example.yaml"
        CF_ORG_RULESET:  "cfn_guard_ruleset_example.guard"
    phases:
    install:
        commands:
        - apt-get update
        - apt-get install build-essential -y
        - apt-get install cargo -y
        - apt-get install git -y
    pre_build:
        commands:
        - echo "Setting up the environment for AWS CloudFormation Guard"
        - echo "More info https://github.com/aws-cloudformation/cloudformation-guard"
        - echo "Install Rust"
        - curl https://sh.rustup.rs -sSf | sh -s -- -y
    build:
        commands:
        - echo "Pull GA release from github"
        - echo "More info https://github.com/aws-cloudformation/cloudformation-guard/releases"
        - echo "Install with Cargo"
        - curl --proto '=https' --tlsv1.2 -sSf https://raw.githubusercontent.com/aws-cloudformation/cloudformation-guard/main/install-guard.sh | sh
        - export PATH=~/.guard/bin:$PATH
    post_build:
        commands:
        - echo "Validate CloudFormation template with cfn-guard tool"
        - echo "More information https://github.com/aws-cloudformation/cloudformation-guard"
        - cfn-guard validate -d $CF_TEMPLATE -r $CF_ORG_RULESET
    artifacts:
    files:
        - cfn_template_file_example.yaml
    name: guard_templates
    ```

    **b. The `cfn_guard_ruleset_example.guard` file which describes the rule set:**

    ```bash
    let my_buckets = Resources.*[ Type == 'AWS::S3::Bucket' ]

    rule validate_env_tag when %my_buckets !empty {
        %my_buckets.Properties {
            some Tags[*].Key == 'environment'
            some Tags[*].Value in ['dev', 'test', 'prod']
                <<Tag key 'environment' not set correctly. Check both the Key & Value>>
        }
    }

    rule validate_service_tag when %my_buckets !empty {
        %my_buckets.Properties {
            some Tags[*].Key == 'service'
                <<Tag key 'service' not set>>
        }
    }
    ```

    **c. The `cfn_template_file_example.yaml` file:**
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
            Value: "deeev"
        - Key: "service"
            Value: "myservice"
    ```

2. Once you have populated and saved all 3 files, you wil push your changes directly to the CodeCommit repository by running the following commands in the terminal:

    ```bash
    git add -A
    git commit -m "Upadating repo with our artifacts"
    git push 
    ```
    You can now see your repository is populated with your artifacts in the CodeCommit console:
    
    ![](../../images/5/1/002.png)

### Section cleanup
To avoid incurring future charges, delete the resources that you have created during the walkthrough:

- CodeCommit repository