---
title: "AWS CloudFormation Guard"
weight: 1
chapter: false
pre: "<b> 4.1 </b>"
---

### What is CloudFormation Guard?
AWS CloudFormation Guard is an open-source general-purpose policy-as-code evaluation tool. The Guard command line interface (CLI) provides developers with a simple-to-use, yet powerful and expressive domain-specific language (DSL) that you can use to express policy as code. In addition, you can use CLI commands to validate structured hierarchical JSON or YAML data against those rules. Guard also provides a built-in unit testing framework to verify that your rules work as intended.

Guard doesn't validate CloudFormation templates for valid syntax or allowed property values. You can use the cfn-lint  tool to perform a thorough inspection of template structure.

For full details on the options of CloudFormation Guard and example use cases outside of tagging, see the documentation AWS CloudFormation Guard 

### Install Guard in the AWS Cloud9 environment
1. Open the AWS Cloud9 terminal, and run the following commands.

    ```bash
    curl --proto '=https' --tlsv1.2 -sSf https://raw.githubusercontent.com/aws-cloudformation/cloudformation-guard/main/install-guard.sh | sh
    export PATH=~/.guard/bin:$PATH
    ```
1. Check if the install has properly completed

    ```bash
    cfn-guard --version
    ```

1. Create our folder and files for the Guard lab.

    ```bash
    cd ~/environment/
    mkdir cfnguard
    cd cfnguard
    touch example_bucket.yaml example_guard.guard
    ls -al
    ```

### Create our first rule

You have now created the files required to validate your template based on rules. In the following instructions, you will create an S3 bucket and test that the required tags have been applied.

1. In the navigation pane, choose and expand the cfnguard folder and then open the file example_bucket.yaml and add a bucket resource as per the below. Ensure to save after pasting.

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

1. Create the Guard rule. Open the file example_guard.guard and add rules as per the below. Ensure to save after pasting.

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

    Lets take a quick look over the rules. The some keyword tells Guard to ensure that one or more values from the resultant query match the check and you can use a list to check that allowed values are present also. Where you are free to allow teams to set their own values, you can still check directly that a required key exists. In the rule validate_env_tag you are running a statement that requires both the key to be a fixed value, and the value of that key to be fixed to a choice of strings held within the list.

1. Run the following command in the terminal:

    ```bash
    cfn-guard validate -S all -d example_bucket.yaml -r example_guard.guard
    ```
    You should see the following return:

    ```
    example_bucket.yaml Status = PASS
    PASS rules
    example_guard.guard/validate_env_tag        PASS
    example_guard.guard/validate_service_tag    PASS
    ---
    ```

1. Let's make some changes to our template to see what happens when there is a failure. In your example_buckeyt.yaml you are going to set the environment tag value to have used the incorrect case for the environment, see below. After making the change, save the file.

    ```
    Value: "Dev"
    ```

1. Re-run the command in the terminal.

    ```bash
    cfn-guard validate -S all -d example_bucket.yaml -r example_guard.guard
    ```
    You should see the following return:

    ```bash
    example_bucket.yaml Status = FAIL
    PASS rules
    example_guard.guard/validate_service_tag    PASS
    FAILED rules
    example_guard.guard/validate_env_tag        FAIL
    ---

    Evaluating data example_bucket.yaml against rules example_guard.guard
    Number of non-compliant resources 1
    Resource = S3Bucket {
    Type      = AWS::S3::Bucket
    Rule = validate_env_tag {
            ALL {
            Check =  Tags[*].Value IN  ["dev","test","prod"] {
                ComparisonError {
                Message          = Tag key 'environment' not set correctly. Check both the Key & Value
                Error            = Check was not compliant as property [/Resources/S3Bucket/Properties/Tags/0/Value[L:11,C:16]] was not present in [(resolved, Path=[L:0,C:0] Value=["dev","test","prod"])]
                }
                PropertyPath    = /Resources/S3Bucket/Properties/Tags/0/Value[L:11,C:16]
                Operator        = IN
                Value           = "Dev"
                ComparedWith    = [["dev","test","prod"]]
                Code:
                        9.            SSEAlgorithm: AES256
                    10.     Tags:
                    11.       - Key: "environment"
                    12.         Value: "Dev"
                    13.       - Key: "service"
                    14.         Value: "myservice"

            }
            Check =  Tags[*].Value IN  ["dev","test","prod"] {
                    ComparisonError {
                    Message          = Tag key 'environment' not set correctly. Check both the Key & Value
                    Error            = Check was not compliant as property [/Resources/S3Bucket/Properties/Tags/1/Value[L:13,C:16]] was not present in [(resolved, Path=[L:0,C:0] Value=["dev","test","prod"])]
                    }
                    PropertyPath    = /Resources/S3Bucket/Properties/Tags/1/Value[L:13,C:16]
                    Operator        = IN
                    Value           = "myservice"
                    ComparedWith    = [["dev","test","prod"]]
                    Code:
                        11.       - Key: "environment"
                        12.         Value: "Dev"
                        13.       - Key: "service"
                        14.         Value: "myservice"

                }
            }
        }
    }
    ```
You can now see that you have a flexible set of rules that can be applied to tags for our resources in a modular fashion.

### Test our first Guard test
Guard gives you the ability to write tests for your rules, to validate that your rules work as you expect.

1. Create a test file for the rules you created earlier by running the following command in the AWS Cloud9 terminal.

    ```bash
    cd ~/environment/cfnguard
    touch example_bucket_tests.yaml
    ```

1. In the navigation pane, choose and expand the cfnguard folder and then open the file example_bucket_tests.yaml. Append the following content that contains tests for one of the named rules you used earlier. Ensure to save after pasting.

    ```yaml
    - input:
        Resources:
        MyExampleBucket:
            Type: AWS::S3::Bucket
            Properties:
            Tags:
                - Key: "environment"
                Value: "dev"
    expectations:
        rules:
        validate_env_tag: PASS
    - input:
        Resources:
        MyExampleBucket:
            Type: AWS::S3::Bucket
            Properties:
            Tags:
                - Key: "environment"
                Value: "Prod"
    expectations:
        rules:
        validate_env_tag: FAIL
    ```

3. Run the following command in the terminal to initiate the test for our Guard rule:

    ```bash
    cfn-guard test -t example_bucket_tests.yaml -r example_guard.guard
    ```

    You should see the following return:

    ```
    Test Case #1
    No Test expectation was set for Rule validate_service_tag
    PASS Rules:
        validate_env_tag: Expected = PASS

    Test Case #2
    No Test expectation was set for Rule validate_service_tag
    PASS Rules:
        validate_env_tag: Expected = FAIL
    ```

    If you set the test to show something other than the expectation, you will see an expected vs evaluated result in the test, see example here:

    ```
    Test Case #1
    No Test expectation was set for Rule validate_service_tag
    FAIL Rules:
        validate_env_tag: Expected = FAIL, Evaluated = [PASS]

    Test Case #2
    No Test expectation was set for Rule validate_service_tag
    PASS Rules:
        validate_env_tag: Expected = FAIL
    ```

1. In the file example_bucket_tests.yaml, add the following at the end of the file. This adds a final test for the validate_service_tag rule. Ensure to save after pasting.

    ```yaml
    - input:
        Resources:
        MyExampleBucket:
            Type: AWS::S3::Bucket
            Properties:
            Tags:
                - Key: "service"
    expectations:
        rules:
        validate_service_tag: PASS
    ```

1. Run the following command in the terminal to include the test added in the previous step.

    ```
    cfn-guard test -t example_bucket_tests.yaml -r example_guard.guard
    ```

    You should see that our validate_service_tag rule also has a test associated with it.

    ```
    Test Case #1
    No Test expectation was set for Rule validate_service_tag
    PASS Rules:
        validate_env_tag: Expected = PASS

    Test Case #2
    No Test expectation was set for Rule validate_service_tag
    PASS Rules:
        validate_env_tag: Expected = FAIL

    Test Case #3
    No Test expectation was set for Rule validate_env_tag
    PASS Rules:
        validate_service_tag: Expected = PASS
    ```

### Conclusion
AWS CloudFormation Guard allows you to write Policy-as-Code to help you validate your cloud environments. Here you have seen how to write guards specificaly for tagging and also how to write tests for those guards.