+++
title = "Triển khai ứng dụng sử dụng CodeDeploy"
weight = 4
chapter = false
pre = "<b>4.4. </b>"
+++

#### Tải xuống Heartbeat Windows Service mẫu

1. Nhấp chuột phải và lưu tập tin **CodeDeployHeartbeatDemo.zip**. Giải nén tập tin.
Hãy dành một chút thời gian để kiểm tra nội dung của tệp **appspec.yml**. Nó có các nội dung sau:
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
Tệp appspec.yml phác thảo từng tập tin nguồn có trong tập tin nén ZIP vừa giải nén và chỉ định đích trên máy ảo EC2 đích nơi tập tin sẽ được lưu trữ. Việc triển khai này chỉ sử dụng hai lifecycle hooks - ApplicationStop và AfterInstall nhưng bạn có thể sử dụng nhiều hơn nếu trường hợp của bạn yêu cầu. Trong trường hợp này, các tập lệnh powershell (cũng được bao gồm trong kho lưu trữ tệp ZIP) được sử dụng để dừng dịch vụ Windows và hủy đăng ký dịch vụ (nếu nó đã được cài đặt) và sau đó sau khi tất cả các tệp được sao chép, để đăng ký dịch vụ và khởi động nó, và cũng di chuyển trình trợ giúp *wintail* vào vị trí để hỗ trợ xem tệp nhật ký.

#### Đẩy gói triển khai lên S3 sẵn sàng để triển khai bởi CodeDeploy
Chúng ta đã sẵn sàng để triển khai dịch vụ Heartbeat tới EC2 instance. Chúng ta có các tập tin triển khai trong máy và cần đóng gói chúng lại và đẩy chúng lên S3. Công cụ AWS CLI CodeDeploy sẽ giúp chúng ta hiện việc này.

2. Trong terminal, *cd* tới thư mục giải nén **CodeDeployHeartbeatDemo**. 
3. Dùng các lệnh sau để đóng gói các tập tin và đẩy chúng lên S3.
```bash
aws deploy push --application-name idevelopDemo --source CodeDeployHeartbeatDemo --profile aws-lab-env --s3-location s3://idevelop-codedeployartefacts-<yourinitials>/CodeDeployHeartbeatDemo.zip 
```
Thay <yourinitials> bằng tên của bạn giống như tên của bucket đã tạo ở bước trước.
Nếu thành công, CLI sẽ trả về kết quả *To deploy with this revision, run:…*, nhưng bạn không nên sử dụng dòng lệnh này, thay vào đó hãy sử dụng câu lệnh được cung cấp ở bước sau.
![UsingCodeDeploy](../../../images/4/18.png?width=90pc)
4.  Nhập các lệnh sau vào terminal
```bash
aws deploy create-deployment --application-name idevelopDemo --deployment-group-name HeartbeatInstances --deployment-config-name CodeDeployDefault.OneAtATime --description "Initial Deployment" --s3-location bucket=idevelop-codedeployartefacts-<yourinitials>,key=CodeDeployHeartbeatDemo.zip,bundleType=zip
```
Thay <yourinitials> bằng tên của bạn giống như tên của bucket đã tạo ở bước trước.
Nếu thành công, CLI sẽ trả về **deploymentId**
![UsingCodeDeploy](../../../images/4/19.png?width=90pc)
