+++
title = "Tạo ứng dụng CodeDeploy và Deployment Group"
weight = 1
chapter = false
pre = "<b>4.1. </b>"
+++


Trong phần này, bạn sẽ sử dụng bảng điều khiển CodeDeploy để tạo một ứng dụng và Deployment Group làm mục tiêu để CodeDeploy triển khai phần mềm vào.

1. Truy cập **CodeDeploy** và chọn **Getting Started**
![CreateCodeDeployApp](../../../images/4/11.png?width=90pc)
2. Chọn **Create Application**
3. Trong trang **Create application**, nhập **idevelopDemo** cho **Application name**
4. Chọn **EC2/On-premises** cho mục **Compute Platform**
5. Chọn **Create Application**
![CreateCodeDeployApp](../../../images/4/12.png?width=90pc)
6. Chọn **Create deployment group**
![CreateCodeDeployApp](../../../images/4/13.png?width=90pc)
7. Nhập **HeartbeatInstances** cho **Deployment group name**
8. Tại mục **Service role ARN**, nhập **CodeDeployServiceRole** và chọn full ARN của quyền từ danh sách.
![CreateCodeDeployApp](../../../images/4/14.png?width=90pc)
9.  Cuộn xuống phần **Environment configuration**, chọn tab **Amazon EC2 Instances**. Trong **Tab group 1** section, chọn **Name** cho mục **Key**
10. Chọn **idevelop** cho mục **Value**
![CreateCodeDeployApp](../../../images/4/15.png?width=90pc)
11. Trong **Matching instances** section, bạn sẽ thấy 2 instance, 2 instance này đủ điều kiện để là một target cho Deployment Group này, vì có *Name* tag là *idevelop*.
12. Đặt các mục khác như mặc định.
13. Bỏ chọn Load Balancer
14. Chọn **Create Deployment Group**
![CreateCodeDeployApp](../../../images/4/16.png?width=90pc)
Ứng dụng CodeDeploy và Deployment Group sẽ được tạo. Bây giờ bạn đã sẵn sàng triển khai phần mềm cho các máy ảo.