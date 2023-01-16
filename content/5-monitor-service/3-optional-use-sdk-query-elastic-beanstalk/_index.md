+++
title = "(Optional) Using the SDK to query Elastic Beanstalk"
weight = 3
chapter = false
pre = "<b>5.3. </b>"
+++

Trong phần này, bạn sẽ sử dụng Java SDK để kiểm tra môi trường AWS của mình và truy vấn thông tin về Elastic Beanstalk.
1. Download **BeanstalkManager.zip** file. BeanstalkManager is a sample Java application that uses the AWS Elastic Beanstalk API SDK.

{{%attachments /%}}

2. Unzip and open the project in Eclipse IDE.

![UseSDKQueryEB](/images/5/8.png?width=90pc)

3. Run Unit Test for this application.

![UseSDKQueryEB](/images/5/9.png?width=90pc)

4. Unit Test will return the following information:
- List applications registered with Elastic Beanstalk in the default region.
- List registered applications and Application Versions.

![UseSDKQueryEB](/images/5/10.png?width=90pc)

{{% notice tip %}}
If you don't see any results, you have selected the wrong region. Check the file **src/main/java/idevelop.samples/AmazonElasticBeanstalkClientFactory.java** to see if you have selected the correct region.
{{% /notice %}}

This sample application gives you an idea of how you can use the SDK to retrieve information about your Elastic Beanstalk environment.