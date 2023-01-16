+++
title = "Using Eclipse IDE"
weight = 1
chapter = false
pre = "<b>5.1. </b>"
+++

1. In AWS CodeDeploy Console, select **Deployments**
2. Select deployment name with *deploymentID* same as *deploymentID* returned from CLI in the privous section.

![OpenDeployment](/images/5/1.png?width=90pc)

3. Scroll down to **Deployment Lifecycle Events**, you will see 2 EC2 VMs tagged and belonging to the deployment group. One of the 2 will be **In Progress** and the other is **Pending**, because we used the **CodeDeployDefault.OneAtATime** configuration behavior in the CLI call. This forces CodeDeploy to only deploy updates to one server at a time.

![OpenDeployment](/images/5/2.png?width=90pc)

4. Shortly, the status will change and you will be able to see the events. Click **View Events** to display events for the virtual machine(s) as they are deployed.

![OpenDeployment](/images/5/3.png?width=90pc)