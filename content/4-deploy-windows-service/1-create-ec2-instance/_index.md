+++
title = "Create EC2 instance"
weight = 1
chapter = false
pre = "<b>4.1. </b>"
+++

In this section, you will use the AWS Console to create 2 Windows EC2 virtual machines and access them remotely.

1. Navigate to **EC2 console**, select **Instances** and click **Launch instance**.

![CreateEC2Instance](/images/4/1.png?width=90pc)

2. Enter instance name, such as: `idevelop`.
- Enter *2* for number of instances.
- Select **Windows** type for AMI.
- Select **Microsoft Windows Server 2022 Base** AMI

![CreateEC2Instance](/images/4/2.png?width=90pc)

3. Select **KPforDevAxInstances** for Keypair.
- Leave the default configuration for the **Network settings** section.

![CreateEC2Instance](/images/4/3.png?width=90pc)

4. In **Advanced details**:
- On IAM Role section, select **awscodestar-travelbuddy-xxx**

![CreateEC2Instance](/images/4/4.png?width=90pc)

5. Scroll to **User data** section, paste in the following content:
```bash
<powershell>
Set-ExecutionPolicy RemoteSigned -Force
Import-Module AWSPowerShell
$REGION = (ConvertFrom-Json (Invoke-WebRequest -Uri http://169.254.169.254/latest/dynamic/instance-identity/document -UseBasicParsing).Content).region
New-Item -Path c:\temp -ItemType "directory" -Force
powershell.exe -Command Read-S3Object -BucketName aws-codedeploy-$REGION -Key latest/codedeploy-agent-updater.msi -File c:\temp\codedeploy-agent-updater.msi
// Start-Sleep -Seconds 30 *optional
c:\temp\codedeploy-agent-updater.msi /quiet /l c:\temp\host-agent-updater-log.txt
</powershell>
```
Here is a Powershell script to automatically download and install CodeDeploy Agent for Windows.
- Click **Launch instance**.

![CreateEC2Instance](/images/4/5.png?width=90pc)

6. Instances have been created.

![CreateEC2Instance](/images/4/6.png?width=90pc)

You don't have to wait for instances to complete initialization - continue to the next sections.
