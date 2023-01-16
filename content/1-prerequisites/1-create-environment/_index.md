+++
title = "Prepare Environment"
weight = 1
chapter = false
pre = "<b>1.1. </b>"
+++

#### Prepare environment
In this part, we will use the CloudFormation template that provided and pre-installed the necessary resources. 

1. We will reuse the **KPforDevAxInstances.PEM** Keypair created in the previous workshop for virtual machines. Or you can review the previous workshop to see how to create a keypair.

2. Download the CloudFormation template file to install the necessary components for the workshop.

{{%attachments title="Template" pattern="Module2.template.yaml"/%}}


3. Access the **AWS CloudFormation**

![CloudFormation](/images/1/1.png?width=90pc)

4. Select **Create stack**, select **With new resources (standard)**

![CloudFormation](/images/1/2.png?width=90pc)

5. In **Prerequisite - Prepare template** section, select **Template is ready**
6. In **Template source** section, select **Upload a template file**, click **Choose file** and choose the downloaded template file. Click **Next**

![CloudFormation](/images/1/3.png?width=90pc)

7. Enter stack name at **Stack name** section.
8. Select **KPforDevAxInstances** keypair for **EEKeyPair** section and click **Next**

![CloudFormation](/images/1/4.png?width=90pc)

9. Click **Next** in **Configure stack options** page.

![CloudFormation](/images/1/5.png?width=90pc)

10.  Click **Create stack**

![CloudFormation](/images/1/6.png?width=90pc)

11. We need to wait about more than 30 minutes for the resources to be initialized and configured.
