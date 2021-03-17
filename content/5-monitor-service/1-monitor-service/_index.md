+++
title = "Sử dụng Eclipse IDE"
weight = 1
chapter = false
pre = "<b>5.1. </b>"
+++

1. Trong AWS CodeDeploy Console, chọn **Deployments**
2. Chọn tên deployment có *deploymentID* giống với *deploymentID* được trả về từ CLI ở phần trước
![OpenDeployment](../../../images/5/1.png?width=90pc)
3. Kéo xuống **Deployment Lifecycle Events**, bạn sẽ thấy 2 máy ảo EC2 được gắn thẻ và thuộc deployment group. Một trong 2 sẽ có trạng thái là **In Progress** và cái còn lại là **Pending**, bởi vì chúng ta đã sử dụng behaviour cấu hình **CodeDeployDefault.OneAtATime** trong lời gọi CLI. Điều này buộc CodeDeploy chỉ triển khai bản cập nhật cho một máy chủ tại một thời điểm.
![OpenDeployment](../../../images/5/2.png?width=90pc)
4. Trong thời gian ngắn, trạng thái sẽ thay đổi và bạn sẽ có thể xem các sự kiện. Nhấp vào **View Events** để hiển thị các sự kiện cho (các) máy ảo khi chúng được triển khai.
![OpenDeployment](../../../images/5/3.png?width=90pc)