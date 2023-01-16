+++
title = "Configure app auto-release"
date = 2023
weight = 1
chapter = false
+++

# Configure app auto-release

In this workshop, you'll automate the manual process you used in the first lesson **DevAx::academy - Converting a Monoliths** Application to Deploy a TravelBuddy Java App to Elastic Beanstalk - Deploy on tomcat environment.
You will use the AWS Developer Tools **CodeStar** service to provide a Continuous Integration/Continuous Delivery (CI/CD) pipeline. You will also get to test deploying a Windows Service to an EC2 virtual machine that you manage directly using the **AWS CodeDeploy** service
In this lab, you will use the following AWS Developer Tools services:

#### AWS CodeStar
AWS CodeStar enables you to quickly develop, build, and deploy applications on AWS. AWS CodeStar provides a unified user interface, enabling you to easily manage your software development activities in one place. With AWS CodeStar, you can set up your entire continuous delivery toolchain in minutes, allowing you to start releasing code faster. AWS CodeStar makes it easy for your whole team to work together securely, allowing you to easily manage access and add owners, contributors, and viewers to your projects. Each AWS CodeStar project comes with a project management dashboard, including an integrated issue tracking capability powered by Atlassian JIRA Software. With the AWS CodeStar project dashboard, you can easily track progress across your entire software development process, from your backlog of work items to teams’ recent code deployments.

#### AWS CodeCommit

AWS CodeCommit is a fully-managed source control service that makes it easy for companies to host secure and highly scalable private Git repositories. CodeCommit eliminates the need to operate your own source control system or worry about scaling its infrastructure. You can use CodeCommit to securely store anything from source code to binaries, and it works seamlessly with your existing Git tools.

#### AWS CodeBuild

AWS CodeBuild is a fully managed continuous integration service that compiles source code, runs tests, and produces software packages that are ready to deploy. With CodeBuild, you don’t need to provision, manage, and scale your own build servers. CodeBuild scales continuously and processes multiple builds concurrently, so your builds are not left waiting in a queue. You can get started quickly by using prepackaged build environments, or you can create custom build environments that use your own build tools. With CodeBuild, you are charged by the minute for the compute resources you use

#### AWS CodePipeline

AWS CodePipeline is a continuous integration and continuous delivery service for fast and reliable application and infrastructure updates. CodePipeline then builds, tests, and deploys your application according to the defined workflow every time there is a code change. This allows you to deliver features and updates quickly and reliably. You can easily build an end-to-end solution using pre-made plugins for popular third-party services like GitHub, or integrate your own custom plugins at any stage. during your release. With AWS CodePipeline, you only pay for what you use. No upfront fees or long-term commitments.

#### AWS CodeDeploy

CodeDeploy is a deployment service that automates application deployments to Amazon EC2 instances, on-premises instances, serverless Lambda functions, or Amazon ECS services. You can deploy a nearly unlimited variety of application content, such as Code, Web and configuration files, Packages, Scripts, Multimedia files, v.v. CodeDeploy can deploy application content that runs on a server and is stored in Amazon S3 buckets, GitHub repositories, or Bitbucket repositories. You do not need to make changes to your existing code before you can use CodeDeploy. AWS CodeDeploy makes it easier for you to release new features, avoid downtime during application deployment and handle the complexity of updating your applications, without many of the risks associated with error-prone manual deployments.


#### Content
After completing the workshop, you will be able to:

- Use a Java monoliths application running Spring on Tomcat on a common compute environment and do a 'lift-and-shift' to Amazon Elastic Beanstalk PaaS with CI/CD pipeline for automatic updates, using AWS CodeStar
- Use Eclipse IDE to connect and test project source code from an AWS CodeCommmit repository set up by AWS CodeStar.
- Make changes to the source code, commit and push those changes to AWS CodeCommit to trigger automatic generation through AWS CodePipeline.
- Spin-up EC2 manually runs Windows and uses AWS CodeDeploy to deploy Windows Service to virtual machines.
- Understand how to obtain Administrator credentials for EC2 VMs running Windows.

#### Technical knowledge required
To be able to complete this lab, you should be familiar with:
- AWS Management Console basics.
- Edit the script with the editor.
- Using Eclipse editor and Java programming language

#### Environment
All the resources needed to start the practice are provided in the CloudFormation template, use this template to initialize the resources for the practice.
The following diagram depicts the resources deployed in the lab.

![Diagram](/images/1/0.png?width=50pc)
