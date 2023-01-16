+++
title = "Challenge - Deployment via Pipeline"
weight = 2
chapter = false
pre = "<b>3.2. </b>"
+++

In this part, you will make a change to your application, commit and push those changes to trigger CodePipeline process to rebuild and deploy your project to an Elastic Beanstalk environment. Here are the general steps you can follow to make a change to the index.jsp file and push that change to git, which will trigger the CI/CD pipeline to build and deplooy the session's new version of the application.

1. Create new a branch.

![DeployPipeline](/images/3/26.png?width=90pc)

2. Find **index.jsp**.
3. Change the text displayed.

![DeployPipeline](/images/3/27.png?width=90pc)

4. Commit the change.
5. Switch to master branch.
6. Merge the changes.

![DeployPipeline](/images/3/28.png?width=90pc)

7. Push the branch back to the origin.

![DeployPipeline](/images/3/29.png?width=90pc)

8. Re-access the application path when deploying changes to the Elastic Beanstalk environment. You should click on the various components in the CodeStar project dashboard and review the operations being performed by CodeBuild, CodePipeline, and Elastic Beanstalk.

9.  Check the application to confirm the changes have been implemented as expected.

![DeployPipeline](/images/3/30.png?width=90pc)