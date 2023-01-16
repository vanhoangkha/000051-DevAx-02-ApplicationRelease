+++
title = "Connect to Eclipse IDE with AWS Codecommit"
weight = 2
chapter = false
pre = "<b>2.2. </b>"
+++

This the section, you will connect to Eclipse IDE with AWS CodeCommit.
1. In AWS CodeStar dashboard, select **IDE** tab.
2. Select **Eclipse** and select **View Instructions**.

![OpenIDE](/images/2/28.png?width=90pc)

![OpenIDE](/images/2/29.png?width=90pc)

3. You will use the IAM user you created earlier to configure AWS Codecommit. Make sure you have the user IAM secret key and access key that you downloaded earlier.
4. Access to the Windows virtual machine, open **Eclipse**. On first startup, Eclipse will ask you to configure the workspace.
5. Tick **Use this as the default and do not ask again** and click **Launch**.

![OpenIDE](/images/2/30.png?width=90pc)

6. If the AWS Toolkit is pre-installed on Eclipse, a dialog box appears - **Welcome to the AWS Toolkit for Eclipse**.
- Enter **Access Key** and **Secret Key** at this dialog.
- Click **Finish**

![ConnectIDE](/images/2/31.png?width=50pc)

7. If not already installed, you can install AWS Toolkit by selecting **Help**, selecting **Install new software**

![ConnectIDE](/images/2/32.png?width=90pc)

8. In the drop-down menu, find *https://aws.amazon.com/eclipse* and select the below tools:
- AWS Developer Tools
- AWS Deployment Tools
Then, click **Next**.

![ConnectIDE](/images/2/33.png?width=90pc)

{{%notice note%}}
If the **Next** button cannot be pressed, you do not need to install the tools.
{{%/notice%}}

9. Click **Next**.

![ConnectIDE](/images/2/34.png?width=50pc)

10. In **Review** window, click **Next**.

![ConnectIDE](/images/2/35.png?width=50pc)

11. Select **I accept the terms of the license agreements** and click **Finish**

![ConnectIDE](/images/2/36.png?width=50pc)

12. After the installation is done, click **Restart Now**
13. If you have already set the information in step 6, skip steps 13 to 16.
- Open **Eclipse IDE**, find **AWS** symbol, click the drop-down arrow and select **Preferences…**

![ConnectIDE](/images/2/37.png?width=90pc)

14. Setting **Default Profile** to *default*.
15. Enter **Profile credentials** information to **Profile Details** dialog box.
16. Click **Apply and Close**.

![ConnectIDE](/images/2/38.png?width=90pc)


17. Select **AWS** symbol and select **Import AWS CodeStar Project…**.

![ConnectIDE](/images/2/39.png?width=90pc)

18. Select the region where the practice is being conducted in **Account and region** section. Select **TravelBuddy** and **TravelBuddy** repository.
19. Enter downloaded git credentials in the previous part to **Configure Git credentials** section.
20. Click **Next**.

![ConnectIDE](/images/2/40.png?width=90pc)

21. An error dialog may appear, which you can dismiss by selecting **OK**.
22. Click **Next**.

![ConnectIDE](/images/2/41.png?width=90pc)


23. Leave other information as default and press **Finish**.

![ConnectIDE](/images/2/43.png?width=90pc)

The IDE will pull the files from the CodeCommit repository and prepare them for you to start coding. There may be an error with this downloaded source code, however you can ignore it as you will not be using it other than check it out in the next step.

24. Take a moment to look at the project structure before you can continue. This Hello World sample application presents a web page to the user and connects to a standard Spring controller.

![ConnectIDE](/images/2/44.png?width=90pc)

So you have finished connecting and pulling source code from CodeCommit to Developer machine.
In the next step, we will change code and release the new version of the appliction to Elastic Beanstalk.