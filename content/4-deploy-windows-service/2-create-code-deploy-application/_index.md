+++
title = "Create CodeDeploy application and Deployment Group"
weight = 2
chapter = false
pre = "<b>4.2. </b>"
+++

In this section, you will use the CodeDeploy console to create an application and the Deployment Group as the target for CodeDeploy to deploy the software into.

1. Access to **CodeDeploy**.
2. Select **Create application**.

![CreateCodeDeployApp](/images/4/11.png?width=90pc)

3. In **Create application** page, enter **idevelopDemo** for **Application name**
4. Select **EC2/On-premises** for **Compute Platform** section.
5. Select **Create application**

![CreateCodeDeployApp](/images/4/12.png?width=90pc)

6. Select **Create deployment group**

![CreateCodeDeployApp](/images/4/13.png?width=90pc)

7. Enter **HeartbeatInstances** for **Deployment group name**
8. In **Service role ARN** section, enter **CodeDeployServiceRole** and select full ARN of role from list.

![CreateCodeDeployApp](/images/4/14.png?width=90pc)

9.  Scroll down to **Environment configuration** section.
- Select **Amazon EC2 Instances** tab.
- In **Tab group 1** section, select **Name** for **Key**.
- Chọn **idevelop** cho mục **Value**

![CreateCodeDeployApp](/images/4/15.png?width=90pc)

10. In **Matching instances** section, you will see 2 instance, those instances are eligible to be a target for this Deployment Group, because the *Name* tag is *idevelop*.
11. Select **Create Deployment Group**

![CreateCodeDeployApp](/images/4/16.png?width=90pc)

CodeDeploy application and Deployment Group will be created. You are now ready to deploy the software to the virtual machines.