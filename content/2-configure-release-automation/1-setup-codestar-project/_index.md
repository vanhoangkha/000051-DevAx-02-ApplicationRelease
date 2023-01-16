+++
title = "Configure AWS CodeStar project"
weight = 1
chapter = false
pre = "<b>2.1. </b>"
+++

#### Setting up an AWS CodeStar project

1. Access to AWS VPC dashboard.

![VPC](/images/2/1.png?width=90pc)

2.  Select **Your VPCs**. We should see a VPC named **Module2/DevAxNetworkVPC**.

![VPC](/images/2/2.png?width=90pc)

3. Select **Subnets** on the left menu và filter by VPC ID of **Module2/DevAxNetworkVPC** VPC ở menu bên trái và lọc theo VPC ID của VPC **Module2/DevAxNetworkVPC**. We should see 4 subnets - 2 privates and 2 public.

![VPC](/images/2/3.png?width=90pc)

4. Open CodeStar dashboard. Select **Create project**.

![NewProjectCodeStar](/images/2/4.png?width=90pc)

5. We should see many project template.

![NewProjectCodeStar](/images/2/5.png?width=90pc)

{{%notice warning%}}
If you see a message related to the missing service role. Select **Create service role**
{{%/notice%}}
6. Cause our monolith is written by Java and we are going to deploy an application, in the search panel, select the options:
- AWS Elastic Beanstalk
- Web Application
- Java

This will list the template tags that match the request - select the tag **Java Spring - Web Application**

![NewProjectCodeStar](/images/2/6.png?width=90pc)

7. In the next page, enter **TravelBuddy** for **Project name**.
8. In repository section, make sure that **CodeCommit** is selected and repository name (**Travel Buddy**) is auto filled.

![NewProjectCodeStar](/images/2/7.png?width=90pc)

9. In **EC2 Configuration** section, select **t3.medium** for Instance Type
10. Select **vpc** match the VPC name of the exercise.
11. Select **Subnet** is public subnet id seen above.
12. Select **KPforDevAxInstances** keypair.
13. Select **Next**

![NewProjectCodeStar](/images/2/8.png?width=90pc)

14. Select **Create Project**

![NewProjectCodeStar](/images/2/9.png?width=90pc)

15. The CodeStar will start creating a pipeline. After a few minutes, the new project is displayed.

![NewProjectCodeStar](/images/2/10.png?width=90pc)

16. While waiting for project creation completed, explore the IDE tab and learn how to access the project source code. 

![NewProjectCodeStar](/images/2/11.png?width=90pc)

17. On the left menu, select **Team**.
18. Select **Add team member**.

![Addmember](/images/2/12.png?width=90pc)

19. We will add an existing user **awsstudent**. In the **Select user** section, select **awsstudent**
20. CodeStar requires members to have an email address. Therefore, remember to add an email address for the user.
21. In **Role Type** section, select **Owner**.
22. Select **Remote Access**.
23. Click **Add team member**.

![Addmember](/images/2/13.png?width=90pc)

24. Next, we will create a new user. Click **Add team member**.
25. In **User** section, select **Create new IAM user**. This will redirect you to IAM Management to create a user.

![CreateNewMember](/images/2/16.png?width=90pc)

26. In IAM Management Console, enter `user-lab02` for username, select both access types: **Programmatic access** and **AWS Management Console access**.
27. Click **Next: Permissions**.

![CreateNewMember](/images/2/17.png?width=90pc)

28. Select **Attach existing policies directly** and select **AdministratorAccess**.
- Chọn **Next: Tags**

![CreateNewMember](/images/2/18.png?width=90pc)

29. Click **Next: Tags**, tag the user if needed or click **Next: Review** to review the configure.

![CreateNewMember](/images/2/19.png?width=90pc)

30. Click **Create User** and 

![CreateNewMember](/images/2/20.png?width=90pc)


31. Click **Download .csv file** to download file that contains a secret key and an access key.This information will be used in the following part.
- Click **Close** and back to CodeStar page.

![CreateNewMember](/images/2/21.png?width=90pc)

32. Click *refresh* symbol to refresh user list, select **user-lab02** just created
33. Enter email for user, select **Owner** in **Project role** section, tick **Remote Access** and click **Add team member** .

![AddNewMember](/images/2/22.png?width=90pc)

34. Now, we will create credentials to use for Codecommit for the newly created user. Access to **IAM** console. Select **Users** on the left menu.
35. Click newly created user

![CredentialforCodecommit](/images/2/23.png?width=90pc)

36. Select **Security Credentials** tab.

![CredentialforCodecommit](/images/2/24.png?width=90pc)

37. Scroll down to **HTTPS Git credentials for AWS CodeCommit** section and click **Generate Credentials**

![CredentialforCodecommit](/images/2/25.png?width=90pc)

38. Click **Download credentials**, then click **Close**.

![CredentialforCodecommit](/images/2/25.png?width=90pc)

CodeStar has now configured a CI/CD pipeline to deliver our project artefact to a public Elastic Beanstalk PaaS web server. This will take a few minutes, check the link in the path and click this link and check the your new java HelloWorld application has been created.

39. Access **CodeStar** and select **Projects**
40. Select **TravelBuddy**, then select **Pipeline** tab. Bạn sẽ thấy 3 giai đoạn được tạo **Source, Build và Deploy.**

![CodeStarProject](/images/2/26.png?width=90pc)

41. You can click **View application** in the upper right corner to access to your running application.

![CodeStarProject](/images/2/27.png?width=90pc)