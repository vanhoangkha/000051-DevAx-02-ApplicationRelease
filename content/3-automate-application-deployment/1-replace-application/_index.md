+++
title = "Replace sample application"
weight = 1
chapter = false
pre = "<b>3.1. </b>"
+++

Since we are about to make changes to the source code, you should use Git to create a new branch. You can do this via the command line or using the Eclipse IDE. Name the new branch **new-implementation**. To create and switch to a new branch in Eclipse perform the following steps:
- Right click on the project, select **Team** 
- Select **Switch To** , select **New Branch…**

![CreateNewBranch](/images/3/1.png?width=90pc)

- Enter new branch name is **new-implementation** and click **Finish**.

![CreateNewBranch](/images/3/2.png?width=90pc)

In this case, we will use the command line.
1. Open terminal and access the directory: *C:\Users\Administrator\git\TravelBuddy* 
2. Run the command line: ``git checkout -b new-implementation`` to create and switch to the new branch.

![CreateNewBranch](/images/3/3.png?width=90pc)

3. Download **TravelBuddy.zip** file and extract it.

{{%attachments /%}}

{{%notice note%}}
Download this file by browser in the virtual machine you are running.
{{%/notice%}}

4. In the Eclipse IDE, right click on the root element of the **TravelBuddy** project and select **Show In -> System Explorer**.

![ShowInExplorer](/images/3/4.png?width=90pc)

5. Delete **src** and **target** folders.

6. Copy the contents of the extracted TravelBuddy folder (source folder) to the folder you just opened from the Eclip IDE (destination folder). You can delete the **.ebextensions** folder in the destination folder before performing the copy, or select **Replace** to ensure that the files and folders are overwritten correctly.\
You will see in the .ebextensions folder there are 3 files, one is the parameters you need to update with the RDS endpoint, the other two are pre-generated with the codecommit setting - set-instance-credit-unlimited.config sshd. config.
7. Open **env_vars.config** file in **.ebextensions** folder and replace value to RDS endpoint:
```
option_settings:
  - namespace: aws:elasticbeanstalk:application:environment
    option_name: JDBC_CONNECTION_STRING
    value: <REPLACE YOUR DETAIILS HERE>
```
You can get your RDS value by logging into the AWS Console, accessing the running RDS instance and getting the endpoint information from there, and adding it to the env_vars.

You can also get this parameter from the cloudformation stack in the Output section of the Cloudformation stack created at the beginning of the workshop.

![CreateNewBranch](/images/3/5.png?width=90pc)

8. Update Python version.
- Open **.ebextensions/sshd.config** file:
  - Edit **python27-devel** to **python3-devel**.
  - Edit **python27-pip** to **python3-pip**.
  - Edit **/etc/init.d/sshd restart** to **sudo service sshd restart**.

![CreateNewBranch](/images/3/38.png?width=90pc)

- Open **.ebextensions/set-instance-credit-unlimited** file:
  - Edit **pip install awscli –upgrade –user** to **pip3 install awscli –upgrade –user**

![CreateNewBranch](/images/3/39.png?width=90pc)

9. Right click on the root project and select **Maven | Update Project | OK**

![CreateNewBranch](/images/3/6.png?width=90pc)

![CreateNewBranch](/images/3/7.png?width=90pc)

10. When the update is complete, you will see project folder structure similar to the below image:  

![CreateNewBranch](/images/3/8.png?width=90pc)

11. You have just changing the source code, now you need to add the files you just changed to the **new-implementation** branch and commit it.

12. Use the terminal, access to **TravelBuddy** folder.
- Enter `git status` to list changes of source code.

![CreateNewBranch](/images/3/9.png?width=90pc)

- Enter `git config --global user.email "you@example.com"`
- Nhap `git config --global user.name awsstudent`
- Enter `git add .` to add changed files.
- Enter `git commit -m "Baseline implementation"` to commit the changes.

![CreateNewBranch](/images/3/10.png?width=90pc)

- Enter `git checkout master` to switch to master branch.
- Merge the changes of the **new-implementation** branch to the **master** branch with the command - `git merge new-implementation`

![CreateNewBranch](/images/3/11.png?width=90pc)

- Check again with `git status` command.

![CreateNewBranch](/images/3/12.png?width=90pc)

13. To push these changes to CodeCommit, in Eclipse, right click on the project root và select **Team | Push to Origin…**

![CreateNewBranch](/images/3/13.png?width=90pc)

14. Click **Close**.

![CreateNewBranch](/images/3/14.png?width=90pc)

It will take a few minutes to push code and running.

![ReplaceApplication](/images/3/15.png?width=90pc)

15. To make the application accessible to the database, update the security rule.

16. Open EC2 console.

17. Click **Security Groups** on the left menu.
- Select **DBSecurityGroup**

![ReplaceApplication](/images/3/35.png?width=90pc)

18. Click **Edit inbound rules**

![ReplaceApplication](/images/3/36.png?width=90pc)

19. Click **Add rule**.
- Select **MySQL/Aurora** service.
- Select SecurityGroup ID from **travelbuddyapp**.
- Select **Save rules**.

![ReplaceApplication](/images/3/37.png?width=90pc)


#### Caching
In this part, we will speed up building by storing artifacts (e.g. java packages) in an S3 bucket that can be used on the next build.
1. Open **buildspec.yml** file, you will see the below lines:
```
cache:
  paths:
    - '/root/.m2/**/*'
```

2. Open the terminal.

3. Execute the below commnad to create a new caching bucket. Replace **<YOURNAME-NO-SPACES>** with the name you want.
```
aws s3 mb s3://cachingbucket-<YOURNAME-NO-SPACES>
```

![ReplaceApplication](/images/3/16.png?width=90pc)

4. The AWS CodeBuild needs to access to the cache bucket, so we need IAM permissions name. Open AWS Console and access IAM.

5. Select Policies tab and find **CodeStar_travelbuddy_PermissionsBoundary** policy.

![ReplaceApplication](/images/3/17.png?width=90pc)

6. Select **Edit policy**.

![ReplaceApplication](/images/3/18.png?width=90pc)

7. In the editor, select JSON and add the following policy:
``` JSON
        {
            "Sid": "CBCachePolicy",
            "Effect": "Allow",
            "Action": "s3:*",
            "Resource": [
                "arn:aws:s3:::cachingbucket-<YOURNAME-NO-SPACES>",
                "arn:aws:s3:::cachingbucket-<YOURNAME-NO-SPACES>/*"
            ]
        },
```
- Select **Review policy** 
![ReplaceApplication](/images/3/19.png?width=90pc)

8. Select **Save changes**.

9. Open AWS Codestar and select your project's Pipeline tab.
- Select **AWS CodeBuild** in Build section of Pipeline

![ReplaceApplication](/images/3/20.png?width=90pc)

10.  Select **Edit**, and select **Artifacts**

![ReplaceApplication](/images/3/21.png?width=90pc)

11. After **Additional Configuration** section, unchecked **Allow AWS CodeBuild to modify this service role so it can be used with this build project**
- Select **Amazon S3** for Cache type.
- Select the bucket you created to store cache.
- Click **Update artifacts**

![ReplaceApplication](/images/3/22.png?width=90pc)

12. After deployment, select **Application Endpoint** on the CodeStar dashboard page. You will see the TravelBuddy website deployed on an Elastic Beanstalk environment.

![ReplaceApplication](/images/3/25.png?width=90pc)


