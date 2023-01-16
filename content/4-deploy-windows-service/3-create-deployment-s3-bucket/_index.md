+++
title = "Create S3 Bucket Deployment"
weight = 3
chapter = false
pre = "<b>4.3. </b>"
+++

AWS CodeDeploy requires deployment artefacts to be stored in an S3 bucket. In this section, we will create a bucket for that purpose, using the AWS CLI.

1. Open terminal and use the following command:
```bash
aws s3 mb s3://idevelop-codedeployartefacts-<yourinitials>
```
![CreateDeploymentS3](../../../images/4/17.png?width=90pc)
Replace <yourinitials> with your name so that the bucket name is unique.
