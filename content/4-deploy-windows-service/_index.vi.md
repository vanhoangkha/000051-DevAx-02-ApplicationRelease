+++
title = "Triển khai Windows Service sử dụng AWS CodeDeploy"
date = 2021
weight = 4
chapter = false
pre = "<b>4. </b>"
+++

Trong bài thực hành này, bạn sẽ sử dụng AWS CodeDeploy để đẩy một Windows Service lên một nhóm các EC2 instance và để nó tự động triển khai, đăng ký và khởi động.

Windows Service có tên là 'Heartbeat'. Nó ghi *heartbeat* message vào tệp nhật ký theo định kỳ và cũng viết chi tiết về trạng thái đang chạy của nó khi nó chuyển từ trạng thái dừng sang đang chạy và dừng lại để giúp bạn dễ dàng thấy AWS CodeDeploy đang hoạt động khi cập nhật phần mềm. Bạn sẽ không viết code cho bài tập này, tất cả mã biên dịch được cung cấp cho bạn. Thay vào đó, bạn sẽ tập trung vào việc sử dụng AWS CodeDeploy để quản lý việc triển khai cho nhóm.