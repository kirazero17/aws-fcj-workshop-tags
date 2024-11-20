---
title: "Prepare for this workshop (New)"
weight: 1
chapter: false
pre: "<b> 1.1 </b>"
---

{{% notice info %}}
As Cloud9 is being deprecated, we suggest you use this instruction to set your environment up instead.\
\
If you still wish to use Cloud9, you can [go back](../).
{{% /notice %}}

### Deploy The Template

1. Download the [tagging-workload.yaml](../../workload/tagging-workload.yaml) CloudFormation template to your local machine.

1. Navigate to the [CloudFormation console](https://console.aws.amazon.com/cloudformation/home) and choose Create Stack > With new resources (standard).

![](../../images/1/old/001-CFNCreateStackButton.png)

1. For **Prepare template** select **Template is ready**, and select **Upload a template** file under **Template source**.

1. Choose Choose File to select the template file that you want to upload. Choose the CloudFormation template that you downloaded at the beginning of this section and choose Next.

1. Enter `tagging-workload` for the Stack name.

![](../../images/1/old/002-deploystack.png)

1. Choose **Next** till you get to the Review page and choose Submit. Wait for the stack creation to reach CREATE_COMPLETE once completed move on to the workshop labs.

1. Repeat steps 1-6 with the VSCode Server stack [code-server-stack.yaml](../../workload/code-server-stack.yaml). Enter `vscodeserver` for the Stack name.

### VSCode Server Access

1. Navigate to the [CloudFormation console](https://console.aws.amazon.com/cloudformation/home).

1. Choose the _VSCode IDE Server_ stack and then choose the **Outputs** tab.

1. The Output key will be **VSCodeServerCloudFrontDomainName**

1. Open the URL Value in a new browser tab, this will open up the AWS Cloud9 environment to be used in the following labs.

![](../../images/1/new/001.png)

You will now see the created AWS Cloud9 environment in a new tab.

![](../../images/1/new/002.png)

### AWS Config

For the AWS Config labs you will need a configuration recorder enabled in your account. If AWS Config is setup in your account already, you can move onto the labs. If AWS Config is not setup in your account already, complete the following instructions, or, skip the config labs if you do not wish to enable AWS Config in your own account.

1. Log into your AWS Cloud9 environment, then create ands access the environment directory.

```bash
mkdir ~/environment
cd ~/environment
mkdir config
cd config
curl 'https://static.us-east-1.prod.workshops.aws/public/bc005028-76d7-42ac-9cb2-fed686ce81e0/static/templates/config.yaml' --output config.yaml
aws cloudformation create-stack --stack-name config-deployment --template-body file://config.yaml --capabilities CAPABILITY_NAMED_IAM
```

At the end of the CloudFormation role out, you should see the configuration recorder is now enabled.

1. Navigate to the [AWS Config console](https://console.aws.amazon.com/config/).

1. Choose **Settings** in the navigation pane.

1. Under **Recorder** view that **Recording** is on.

![](../../images/1/old/005-recorder.png)