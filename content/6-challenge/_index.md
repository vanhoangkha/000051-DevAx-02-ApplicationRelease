+++
title = "Thử thách - Thay đổi mã nguồn ứng dụng"
date = 2021
weight = 6
chapter = false
pre = "<b>6. </b>"
+++

Trong thử thách này, bạn sẽ thực hiện một thay đổi đối với ứng dụng BeanstalkManager để tạo tự động một ứng dụng Elastic Beanstalk, Môi trường và Vesion. Đây là những tác vụ được CodePipeline hoàn thành khi bạn chọn Elastic Beanstalk để triển khai. Điều này sẽ cung cấp cho bạn một số hiểu biết về lợi ích của việc sử dụng Công cụ dành cho nhà phát triển AWS để cung cấp và triển khai các ứng dụng của bạn cũng như các tính năng mạnh mẽ mà chúng cung cấp.
Để bắt đầu, bạn có thể tham khảo tài liệu Java SDK tại đường dẫn http://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/elasticbeanstalk/AWSElasticBeanstalkClient.html

Bạn sẽ cần tạo một ứng dụng 
``` java
AWSElasticBeanstalk client = new AWSElasticBeanstalkClient();
CreateApplicationRequest request = new CreateApplicationRequest()
        .withApplicationName("my-app")
        .withDescription("my application");
CreateApplicationResult response = client.createApplication(request);
```
Tiếp theo, bạn sẽ cần phải đóng gói artefact triển khai của mình (có lẽ đó là trang TravelBuddy mà chúng tôi đang sử dụng hoặc bạn có thể tạo ứng dụng web java của riêng mình) và tải tệp WAR đó lên S3. Bạn đã học cách tải tệp lên bộ chứa S3 theo chương trình trong bài thực hành đầu tiên của khóa học này. Sau đó, để tạo một phiên bản trong Elastic Beanstalk:
```java
AWSElasticBeanstalk client = new AWSElasticBeanstalkClient();
CreateApplicationVersionRequest request = new CreateApplicationVersionRequest()
        .withApplicationName("my-app")
        .withAutoCreateApplication(true)
        .withDescription("my-app-v1")
        .withProcess(true)
        .withSourceBundle(
                new S3Location().withS3Bucket("my-bucket").withS3Key(
                        "sample.war")).withVersionLabel("v1");
CreateApplicationVersionResult response = client
        .createApplicationVersion(request);

```
![Challenge](../../../images/6/1.png?width=90pc)
Ta có ứng dụng được trong Elastic Beanstalk
![Challenge](../../../images/6/2.png?width=90pc)
Tiếp theo, ta cần tạo môi trường cho ứng dụng này.
``` java
ConfigurationOptionSetting cos = new ConfigurationOptionSetting("aws:autoscaling:launchconfiguration","IamInstanceProfile","aws-elasticbeanstalk-ec2-role");
		CreateEnvironmentRequest request = new CreateEnvironmentRequest()
				.withApplicationName("my-app")
				.withEnvironmentName("env-my-app")
				.withSolutionStackName("64bit Amazon Linux 2018.03 v3.4.3 running Tomcat 8.5 Java 8")
				.withOptionSettings(cos);
		CreateEnvironmentResult response = eb.createEnvironment(request);
		System.out.print(response);
```
![Challenge](../../../images/6/3.png?width=90pc)
Dùng lệnh **mvn install** để khởi chạy chương trình. Sau khi chạy thành công, ta được một ứng dụng my-app được triển khai trên môi trường env-my-app
![Challenge](../../../images/6/4.png?width=90pc)
![Challenge](../../../images/6/5.png?width=90pc)