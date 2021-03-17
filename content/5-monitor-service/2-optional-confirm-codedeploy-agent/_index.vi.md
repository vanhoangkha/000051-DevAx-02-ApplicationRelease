+++
title = "(Optional) Xác nhận cài đặt CodeDeploy agent "
weight = 2
chapter = false
pre = "<b>5.2. </b>"
+++

Đây là một bước tùy chọn. Nếu bạn gặp sự cố trong quá trình triển khai, bạn sẽ cần phải điều tra trạng thái của CodeDeploy agent trên các máy ảo. Hoặc ngay cả khi bạn không có bất kỳ vấn đề nào để theo dõi, bạn có thể tìm hiểu xem dịch vụ Heartbeat đang làm gì.
Để kết nối với các máy ảo Windows bạn phải sử dụng tập tin PEM chứa KeyPair được sinh ra trong quá trình cài đặt, để giải mã mật khẩu Windows, sau đó sử dụng Microsoft Remote Desktop client để kết nối tới máy ảo.

1. Truy cập EC2 Management Console. Chọn một trong số 2 máy ảo có gắn thẻ **idevelop**
![OpenEC2Instance](../../../images/5/4.png?width=90pc)
2. Chọn **Connect**
3. Chọn tab **RDP client** và chọn **Download remote desktop file** để tải xuống tập tin RDP.
4. Chọn keypair **KPforDevAxInstances** để decrypt mật khẩu của máy ảo Windows.
![ConnecttoEC2Instance](../../../images/5/5.png?width=90pc)
Sử dụng mật khẩu và tập tin RDP để truy cập vào máy ảo Windows.
5. Trong máy ảo Windows, mở tập tin **c:\temp\host-agent-install-log**
6. Kiểm tra và đảm bảo rằng không có dòng nào có báo lỗi, ví dụ như *Failed to connect to server* vì đây là một lỗi xảy ra trong quá trình cài đặt.
7. Nếu có lỗi xay ra trong quá trình cài đặt, bạn có thể thử tự chạy lại bằng các câu lệnh sau:
```bash
c:\temp\codedeploy-agent.msi /l*v c:\temp\host-agent-install-log.txt
```
8. Sau khi chạy lại cài đặt, kiểm tra lại tập tin **c:\temp\host-agent-install-log**. Nếu không có lỗi nào được hiển thị, quá trình tạo mới CodeDeploy có khả năng thành công. Bạn có thể tạo mới bằng lệnh AWS CLI:
```bash
aws deploy create-deployment --application-name idevelopDemo --deployment-group-name HeartbeatInstances --deployment-config-name CodeDeployDefault.AllAtOnce --description "Retrying Initial Deployment" --s3-location bucket=idevelop-codedeployartefacts-<yourinitials>,key=CodeDeployHearbeatDemo.zip,bundleType=zip
```
Thay thế <yourinitials> bằng tên của bạn sao cho giống với tên S3 bucket bạn đã tạo. Tập tin ZIP sẽ được sao chép tới S3 bucket.
#### Xác nhận cài đặt dịch vụ Heartbeat thành công

Giả sử CodeDeploy agent được cài đặt và chạy thành công, và bạn muốn xác nhận việc triển khai dichj vụ Heartbeat thành công, bạn có thể thực hiện các bước sau đây:

9. Sử dụng Service Control Manager để tìm **AWS Heartbeat Demo Service**. Nếu dịch vụ này hiển thị *Running* trong SCM, dịch vụ này đã cài đặt thành công.
10. Mở thư mục **c:\Logs**. Tập tin **HeartBeatService** trong thư mục sẽ chứa ouput như sau nếu nó khởi chạy thành công:
```
[INFO]09/14 08:33:18 - Initialising Heartbeat Service...
[INFO]09/14 08:33:18 - Heartbeat Service is now running...
[INFO]09/14 08:33:18 - Heartbeat - Deploy has Worked on Tuesday! Iteration 1
[INFO]09/14 08:33:19 - Heartbeat - Deploy has Worked on Tuesday! Iteration 2
...
...
```