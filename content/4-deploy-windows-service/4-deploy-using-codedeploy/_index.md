+++
title = "Deploy application using CodeDeploy"
weight = 4
chapter = false
pre = "<b>4.4. </b>"
+++

#### Download sample Heartbeat Windows Service

1. Right-click and save the file **CodeDeployHeartbeatDemo.zip**. Unzip the file.

{{%attachments /%}}

Please take a moment to examine the contents of the **appspec.yml** file. It has the following contents:
```
os: windows
files:
  - source: Heartbeat.dll
    destination: c:\HeartbeatService
  - source: HeartbeatService.exe
    destination: c:\HeartbeatService
  - source: HeartbeatService.exe.config
    destination: c:\HeartbeatService
  - source: log4net.dll
    destination: c:\HeartbeatService
  - source: Logger.dll
    destination: c:\HeartbeatService
  - source: wintail.exe
    destination: c:\temp

hooks:
  ApplicationStop:
    - location: uninstall.ps1
      timeout: 30
  AfterInstall:
    - location: install.ps1
      timeout: 30
    - location: copywintail.ps1
      timeout: 30

```

The appspec.yaml file outlines each source file contained in the unzipped ZIP archive and specifies the destination on the destination EC2 virtual machine where the files will be stored. This implementation uses only two lifecycle hooks - ApplicationStop and AfterInstall but you can use more if your case requires it. In this case, the powershell scripts (also included in the ZIP file archive) are used to stop the Window service and unregister the service (if it is already installed) and then after all files are copied, to register the service and start it, and also move the *wintail* helper in place to assist in viewing log files.

#### Push deployment packages to S3 ready to deploy by CodeDeploy
We are now ready to deploy the Heartbeat service to the EC2 instance. We have the deployment files on our machine and need to package them up and push them to S3. The AWS CLI CodeDeploy tool will help us do this.

2. In terminal, run `cd CodeDeployHeartbeatDemo` command. 
3. Use the following commands to pack files and push them to S3.
```bash
aws deploy push --application-name idevelopDemo --source CodeDeployHeartbeatDemo --profile default --s3-location s3://idevelop-codedeployartefacts-<yourinitials>/CodeDeployHeartbeatDemo.zip 
```
Replace <yourinitials> with your name the same as the name of the bucket created in the previous step.
If successful, CLI will return *To deploy with this revision, run:…*, but you should not use this command line, instead use the command provided in the following step.

![UsingCodeDeploy](/images/4/18.png?width=90pc)

4.  Enter the following command to terminal.
```bash
aws deploy create-deployment --application-name idevelopDemo --deployment-group-name HeartbeatInstances --deployment-config-name CodeDeployDefault.OneAtATime --description "Initial Deployment" --s3-location bucket=idevelop-codedeployartefacts-<yourinitials>,key=CodeDeployHeartbeatDemo.zip,bundleType=zip
```
Replace <yourinitials> with your name the same as the name of the bucket created in the previous step.
If successful, CLI will return **deploymentId**

![UsingCodeDeploy](/images/4/19.png?width=90pc)
