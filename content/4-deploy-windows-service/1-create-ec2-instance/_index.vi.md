+++
title = "Tạo máy ảo EC2"
weight = 1
chapter = false
pre = "<b>4.1. </b>"
+++

Trong phần này, bạn sẽ sử dụng AWS Console để tạo 2 máy ảo Windows EC2 và truy cập từ xa vào chúng.

1. Truy cập **EC2**, chọn **Instances** và chọn **Launch instance**
![CreateEC2Instance](../../../images/4/1.png?width=90pc)
2. Trong *Step 1: Choose an Amazon Machine Image (AMI)*, chọn **Microsoft Windows Server 2019 Base** AMI
![CreateEC2Instance](../../../images/4/2.png?width=90pc)
3. Trong *Step 2: Choose an Instance Type*, để các thông số như mặc định và chọn **Next: Configure Instance Details**
![CreateEC2Instance](../../../images/4/3.png?width=90pc)
4. Trong *Step 3: Configure Instance Details*, chọn **2** cho mục **Number of instances**
![CreateEC2Instance](../../../images/4/4.png?width=90pc)
5. Đặt VPC và Subnet như mặc định và chọn **Auto-assign Public IP**
6. Tại mục **IAM Role**, chọn *awscodestar-travelbuddy-xxx*. Vai trò này được tạo tự động cho bạn trong quá trình cài đặt bài thực hành.
7. Đặt các thông số khac như mặc định và cuộn xuống mục **Advanced details**.
8. Tại mục **User data**, dán vào nội dung sau
```bash
<powershell>
Set-ExecutionPolicy RemoteSigned -Force
Import-Module AWSPowerShell
$REGION = (ConvertFrom-Json (Invoke-WebRequest -Uri http://169.254.169.254/latest/dynamic/instance-identity/document -UseBasicParsing).Content).region
New-Item -Path c:\temp -ItemType "directory" -Force
powershell.exe -Command Read-S3Object -BucketName aws-codedeploy-$REGION -Key latest/codedeploy-agent-updater.msi -File c:\temp\codedeploy-agent-updater.msi
// Start-Sleep -Seconds 30 *optional
c:\temp\codedeploy-agent-updater.msi /quiet /l c:\temp\host-agent-updater-log.txt
</powershell>
```
![CreateEC2Instance](../../../images/4/5.png?width=90pc)
Đây là đoạn script Powershell để tự động tải và cài đặt CodeDeploy Agent cho Windows.\
9. Chọn **Next: Add storage**.\
10. Trong *Step 4: Add Storage*, đặt các thông số như mặc định và chọn **Next: Add Tags**
![CreateEC2Instance](../../../images/4/6.png?width=90pc)
11. Trong *Step 5: Add Tags*, chọn **Add Tag**
12. Nhập **Name** cho **Key** và nhập **idevelop** cho **Value**
![CreateEC2Instance](../../../images/4/7.png?width=90pc)
13. Chọn **Next: Configure Security Group**\
14. Trong *Step 6: Configure Security Group*, đặt các thông số như mặc định.
![CreateEC2Instance](../../../images/4/8.png?width=90pc)
15. Chọn **Review and Launch**. Chọn **Launch**\
![CreateEC2Instance](../../../images/4/9.png?width=90pc)
16. Một hộp thoại xuất hiện, yêu cầu chọn một KeyPair. KeyPair này sử dụng để mã hóa mật khẩu Administrator được tạo ngẫu nhiêu trong quá trình khởi tạo. Chọn KeyPair và đánh dấu chọn **I acknowledge that I have access to the selected private key file…**, chọn **Launch Instances**
![CreateEC2Instance](../../../images/4/10.png?width=90pc)
Bạn sẽ thấy trang Launch Status, cho biết rằng các máy ảo đang được khởi chạy. Quá trình này sẽ mất vài phút để hoàn thành. Nhấp vào nút **View Instances** ở cuối trang để hiển thị trạng thái của quá trình khởi chạy.
![CreateEC2Instance](../../../images/4/10.png?width=90pc)
Bạn không cần phải đợi các phiên bản hoàn tất quá trình khởi tạo - hãy tiếp tục các phần sau.
