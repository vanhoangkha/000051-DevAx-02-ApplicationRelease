+++
title = "Tạo máy ảo EC2"
weight = 1
chapter = false
pre = "<b>4.1. </b>"
+++

Trong phần này, bạn sẽ sử dụng AWS Console để tạo 2 máy ảo Windows EC2 và truy cập từ xa vào chúng.

1. Truy cập **EC2 console**, chọn **Instances** và chọn **Launch instance**

![CreateEC2Instance](/images/4/1.png?width=90pc)

2. Nhập tên cho instance, ví dụ: `idevelop`.
- Nhập *2* cho số lượng instance.
- Chọn kiểu **Windows** cho AMI.
- Chọn **Microsoft Windows Server 2022 Base** AMI

![CreateEC2Instance](/images/4/2.png?width=90pc)

3. Chọn **KPforDevAxInstances** cho Keypair.
- Để mặc định cấu hình cho mục **Network settings**.

![CreateEC2Instance](/images/4/3.png?width=90pc)

4. Trong **Advanced details**:
- Tại mục IAM Role, chọn **awscodestar-travelbuddy-xxx**

![CreateEC2Instance](/images/4/4.png?width=90pc)

5. Kéo xuống mục **User data**, dán vào nội dung sau:
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
Đây là đoạn script Powershell để tự động tải và cài đặt CodeDeploy Agent cho Windows.
- Ấn **Launch instance**.

![CreateEC2Instance](/images/4/5.png?width=90pc)

6. Các instance đã được tạo.

![CreateEC2Instance](/images/4/6.png?width=90pc)


Bạn không cần phải đợi các phiên bản hoàn tất quá trình khởi tạo - hãy tiếp tục các phần sau.
