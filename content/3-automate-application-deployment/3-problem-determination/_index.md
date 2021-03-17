+++
title = "Xác định lỗi"
weight = 3
chapter = false
pre = "<b>3.3. </b>"
+++

##### Kiểm tra tập tun buildspec.yml
Tập tin **buildspec.yml** là một phần quan trọng mà bạn cần phải đảm bảo tính chính xác của tập tin này. Tập tin này sẽ chạy qua các giai đoạn mà bạn có thể thấy trong bảng điều khiển bên dưới CodeBuild trong log của nó. 
Tập tin này được chia thành nhiều phần nhỏ

install - loại cài đặt mà bạn muốn chạy code. Trong trường hợp này, chúng ta sẽ sử dụng openjdk8.
```
phases:
  install:
    runtime-versions:
      java: openjdk8
    commands:
      - pip install --upgrade awscli
```
Tiếp theo là các lệnh bạn muốn chạy trước khi thực hiện build project. Đây là yêu cầu trước khi build project. Và trong ví dụ này, chúng ta sẽ chạy lệnh mvn để biên dịch tất cả các thiết lập cần thiết.
```
pre_build:
  commands:
    - mvn clean compile test
```
Kế tiếp là tạo build stage
```
build:
  commands:
    - mvn war:exploded
```
Sau khi build mvn, là cách thiết lập môi trường bắt buộc cho mỗi môi trường trong .ebextensions được đọc bởi các ứng dụng được yêu cầu
```
  post_build:
    commands:
      - cp -r .ebextensions/ target/travelbuddy/
      - aws cloudformation package --template template.yml --s3-bucket $S3_BUCKET --output-template-file template-export.yml

      # Do not remove this statement. This command is required for AWS CodeStar projects.
      # Update the AWS Partition, AWS Region, account ID and project ID in the project ARN on template-configuration.json file so AWS CloudFormation can tag project resources.
      - sed -i.bak 's/\$PARTITION\$/'${PARTITION}'/g;s/\$AWS_REGION\$/'${AWS_REGION}'/g;s/\$ACCOUNT_ID\$/'${ACCOUNT_ID}'/g;s/\$PROJECT_ID\$/'${PROJECT_ID}'/g' template-configuration.json
artifacts:
  type: zip
  files:
    - template-export.yml
    - template-configuration.json
```
2. Kiểm tra bảng điều khiển **CodeBuild** 
Kiểm tra bảng điều khiển CodeBuild và xem có lỗi nào xuất hiện trong quá trình triển khai hay không.
Nếu có, chọn xem chi tiết để biết lỗi xảy ra là gì.
![Error](../../../images/3/31.png?width=90pc)
Truy cập Buildlog tab và cuộn xuống phần log báo lỗi.
Trong ví dụ này, bạn có thể thấy địa chỉ truy cập của root trong tập tin template.yml không chính xác.
```
Unable to upload artifact target/ROOT referenced by SourceBundle parameter of EBApplicationVersion resource.
Parameter SourceBundle of resource EBApplicationVersion refers to a file or folder that does not exist /codebuild/output/src812543603/src/target/ROOT

[Container] 2020/06/05 02:16:17 Command did not exit successfully aws cloudformation package --template template.yml --s3-bucket $S3_BUCKET --output-template-file template-export.yml exit status 255
[Container] 2020/06/05 02:16:17 Phase complete: POST_BUILD State: FAILED
[Container] 2020/06/05 02:16:17 Phase context status code: COMMAND_EXECUTION_ERROR Message: Error while executing command: aws cloudformation package --template template.yml --s3-bucket $S3_BUCKET --output-template-file template-export.yml. Reason: exit status 255
```
Đây là lỗi trong tập tin template.yml cần được thay đổi:
```
EBApplicationVersion:
  Description: The version of the AWS Elastic Beanstalk application to be created for this project.
  Type: AWS::ElasticBeanstalk::ApplicationVersion
  Properties:
    ApplicationName: !Ref "EBApplication"
    Description: The application version number.
    SourceBundle: "target/ROOT"
```
thành
```
EBApplicationVersion:
  Description: The version of the AWS Elastic Beanstalk application to be created for this project.
  Type: AWS::ElasticBeanstalk::ApplicationVersion
  Properties:
    ApplicationName: !Ref "EBApplication"
    Description: The application version number.
    SourceBundle: "target/travelbuddy"
```
3. Cập nhật Elastic beanstalk có thành công hay không? 
Truy cập Elastic beanstalk, vào ứng dụng travelbuddy và chọn Request Logs.
![RequestLogs](../../../images/3/32.png?width=90pc)
Ở đây có 2 loại log là Last 100 Lines và Full Logs. Chọn loại phù hợp và tải về. 
![RequestLogs](../../../images/3/33.png?width=90pc)
Kiểm tra các tập tin log đã tải về để xem tại sao máy chủ lại không khởi động được.
Dưới đây là một vài vấn đề ví dụ:
- Ví dụ về việc giao tiếp giữa client và RDS server không hoạt động
```
05-Jun-2020 03:27:26.514 SEVERE [localhost-startStop-1] org.apache.tomcat.jdbc.pool.ConnectionPool.init Unable to create initial connections of pool.
	com.mysql.cj.jdbc.exceptions.CommunicationsException: Communications link failure

The last packet sent successfully to the server was 0 milliseconds ago. The driver has not received any packets from the server.
		at com.mysql.cj.jdbc.exceptions.SQLError.createCommunicationsException(SQLError.java:174)
		at com.mysql.cj.jdbc.exceptions.SQLExceptionsMapping.translateException(SQLExceptionsMapping.java:64)
		at com.mysql.cj.jdbc.ConnectionImpl.createNewIO(ConnectionImpl.java:836)
		at com.mysql.cj.jdbc.ConnectionImpl.<init>(ConnectionImpl.java:456)
		at com.mysql.cj.jdbc.ConnectionImpl.getInstance(ConnectionImpl.java:246)
```
Trong ví dụ này, RDS endpoint được khai báo trong .ebexentions sai. Sửa lại RDS endpoint sẽ khắc phục được lỗi này.
- Ví dụ về việc không tồn tại table trong RDS:
```
java.sql.SQLSyntaxErrorException: Table 'travelbuddy.flightspecial' doesn't exist
	com.mysql.cj.jdbc.exceptions.SQLError.createSQLException(SQLError.java:120)
	com.mysql.cj.jdbc.exceptions.SQLError.createSQLException(SQLError.java:97)
	com.mysql.cj.jdbc.exceptions.SQLExceptionsMapping.translateException(SQLExceptionsMapping.java:122)
	com.mysql.cj.jdbc.ClientPreparedStatement.executeInternal(ClientPreparedStatement.java:953)
	com.mysql.cj.jdbc.ClientPreparedStatement.executeQuery(ClientPreparedStatement.java:1003)
	sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
```
Để khắc phục lỗi này, đăng nhập vào mysql và truy cập vào database, thực hiện lại các phần tạo databse đã hướng dẫn ở bài lab đầu tiên.

##### Khắc phục sự cố ứng dụng sau khi thay thế mã nguồn.
Việc khởi tạo thành công, tuy nhiên bạn không thể truy cập ứng dụng của mình. Làm theo các bước sau để điều tra và giải quyết vấn đề.
4. Kiểm tra CodeStar Pipeline xem có lỗi nào hay không
![Pipeline](../../../images/3/34.png?width=90pc)
5. Kiểm tra phần Deploy và chọn ExecuteChangeSet Details
6. Kiểm tra tại ElasticBeanstalk 
7. Xem logs của ứng dụng **travelbuddy**
```
23-Sep-2020 06:16:13.732 SEVERE [localhost-startStop-1] org.apache.tomcat.jdbc.pool.ConnectionPool.init Unable to create initial connections of pool.
	com.mysql.cj.jdbc.exceptions.CommunicationsException: Communications link failure

The last packet sent successfully to the server was 0 milliseconds ago. The driver has not received any packets from the server.
		at com.mysql.cj.jdbc.exceptions.SQLError.createCommunicationsException(SQLError.java:174)
```
Dường như ứng dụng không được phép kết nối tới Database.
Để ứng dụng có thể giao tiếp được với Database, ta cần thêm SecurotyGroup từ ElasticBeanstalk Enviroment tới SecurityGroup từ RDS.
1. Truy cập EC2 và chọn Security Groups
![SecurityGroup]](../../../images/3/35.png?width=90pc)
2. Chọn **DBSecurityGroup** và chọn tab **Inbound rules**
3. Chọn **Edit inbound rules**
![SecurityGroup](../../../images/3/36.png?width=90pc)
4. Chọn dịch vụ MySQL/Aurora và chọn SecurityGroup ID từ travelbuddyapp. Chọn **Save rules**
![SecurityGroup](../../../images/3/37.png?width=90pc)
5. Kiểm tra lại ứng dụng và thấy rằng có thể truy cập được ứng dụng một cách bình thường.