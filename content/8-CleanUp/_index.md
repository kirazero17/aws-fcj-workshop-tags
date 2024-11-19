---
title: "Cleaning Up"
weight: 8
chapter: false
pre: "<b> 8. </b>"
---

### Cleanup Workshop Resources

1. Navigate to the Cloudformation console .

1. Choose the tagging-workload stack you created at the beggining of the workshop.

1. Choose Delete and confirm.

![](../images/8/001.png)

### Cleanup AWS Cloud9 Resources

1. Navigate to the Cloudformation console .

1. Choose the cloud9 stack you created at the beggining of the workshop.

1. Choose Delete and confirm.

![](../images/8/002.png)

### Cleanup AWS Config Resources From Account

1. Navigate to the Cloudformation console .

1. Choose the S3 config bucket under the Resources tab and delete everything in this bucket.

1. Go back to the Cloudformation console .

1. Choose the config stack you created at the beggining of the workshop.

1. Choose Delete and confirm.

![](../images/8/003.png)

{{% notice note %}}
Some labs use additional resources. Please make sure you have reviewed the cleanup guide for each lab you have completed. Failure to cleanup all resources will incur costs.
{{% /notice %}}