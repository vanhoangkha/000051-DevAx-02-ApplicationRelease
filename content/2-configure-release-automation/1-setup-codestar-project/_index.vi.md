+++
title = "Thiết lập một AWS CodeStar project "
weight = 1
chapter = false
pre = "<b>2.1. </b>"
+++

#### Thiết lập một AWS CodeStar project

1. Truy cập AWS VPC dashboard. Chọn **VPCs**
![VPC](../../../images/2/1.png?width=90pc)
2. Bạn sẽ thấy có một VPC có tên **Module2/DevAxNetworkVPC**.
![VPC](../../../images/2/2.png?width=90pc)
3. Chọn **Subnets** ở menu bên trái và lọc theo VPC ID của VPC **Module2/DevAxNetworkVPC**
![VPC](../../../images/2/3.png?width=90pc)
4. Bạn sẽ thấy 4 subnet - 2 private và 2 public.
5. Truy cập CodeStar dashboard. 
![NewProjectCodeStar](../../../images/2/4.png?width=90pc)
6. Chọn **Create project**. Bạn sẽ thấy nhiều thẻ mẫu project.
![NewProjectCodeStar](../../../images/2/5.png?width=90pc)
{{%notice warning%}}
Nếu bạn thấy một thông báo liên quan vai trò dịch vụ bị thiếu. Chọn **Create service role**
{{%/notice%}}
7. Vì monolith của chúng ta được viết bằng ngôn ngữ lập trình Java và chúng ta sẽ triển khai một ứng dụng, trong bảng tìm kiếm, hãy chọn các tùy chọn:
- AWS Elastic Beanstalk
- Web Application
- Java
![NewProjectCodeStar](../../../images/2/6.png?width=90pc)
Thao tác này sẽ liệt kê các thẻ mẫu phù hợp với yêu cầu - chọn thẻ *Java Spring - Web Application*
8. Trong trang tiếp theo, nhập **TravelBuddy** cho mục **Project name**.
9. Với mục repository, đảm bảo rằng **CodeCommit** đã được chọn và tên repository (**Travel Buddy**) được tự động điền.
![NewProjectCodeStar](../../../images/2/7.png?width=90pc)
10. Trong mục **EC2 Configuration**, chọn Instance Type là **t3.medium**
11. Chọn **vpc** đúng với tên VPC của bài thực hành.
12. Chọn **Subnet** là public subnet id đã xem ở phần trên.
13. Chọn keypair **KPforDevAxInstances**
![NewProjectCodeStar](../../../images/2/8.png?width=90pc)
14. Chọn **Next**
15. Chọn **Create Project**
![NewProjectCodeStar](../../../images/2/9.png?width=90pc)
16. CodeStar sẽ bắt đầu tạo một pipeline. Sau một vài phút, màn hình sẽ hiển thị project của bạn.
![NewProjectCodeStar](../../../images/2/10.png?width=90pc)
17. Trong khi chờ quá trình tạo project được hoàn tất, hãy khám phá tab IDE và tìm hiểu cách truy cập mã nguồn project. 
![NewProjectCodeStar](../../../images/2/11.png?width=90pc)
18. Trong menu bên trái, chọn **Team**.
19. Chọn **Add team member**.
![Addmember](../../../images/2/12.png?width=90pc)
20. Chúng ta sẽ thêm một user đã có sẵn *awsstudent*. Tại mục **Select user**, chọn **awsstudent**
21. CodeStar yêu cầu các thành viên phải có một địa chỉ email. Do đó, hãy nhớ thêm địa chỉ email cho user.
22. Tại mục **Role Type**, chọn **Owner**.
23. Chọn **Remote Access**.
![Addmember](../../../images/2/13.png?width=90pc)
24. Tiếp theo, chúng ta sẽ tạo một user mới. Chọn **Add team member**.
25. Tại mục **User**, chọn **Create new IAM user**. Điều này sẽ chuyển hướng bạn tới IAM Management để tạo một user.
![CreateNewMember](../../../images/2/16.png?width=90pc)
26. Trong IAM Management Console nhập username, đánh dấu chọn cả 2 loại truy cập: Programmatic access và AWS Management Console access.
![CreateNewMember](../../../images/2/17.png?width=90pc)
27. Chọn **Next: Permissions**.
28. Chọn **Attach existing policies directly** và đánh dấu chọn **AdministratorAccess**.
![CreateNewMember](../../../images/2/18.png?width=90pc)
29. Chọn **Next: Tags**, gắn thẻ cho user nếu cần hoặc chọn **Next: Review** để xem lại cấu hình.
![CreateNewMember](../../../images/2/19.png?width=90pc)
30. Chọn **Create User** và chọn **Download .csv** để tải xuống tập tin chứa secret key và access key. Thông tin này sẽ được dùng trong các phần tiếp theo.
![CreateNewMember](../../../images/2/20.png?width=90pc)
31. Chọn **Close** và trở lại trang CodeStar
![CreateNewMember](../../../images/2/21.png?width=90pc)
32. Chọn biểu tượng *refesh* để cập nhật lại danh sách các user, chọn User user-lab02 vừa tạo.
![AddNewMember](../../../images/2/22.png?width=90pc)
33. Nhập địa chỉ email cho user, chọn **Owner** tại mục **Project role**, đánh dấu chọn **Remote Access** và chọn **Add team member**
34. Bây giờ chúng ta sẽ tạo credentials sử dụng cho Codecommit cho user vừa tạo.
35. Truy cập **IAM** và vào **Users**.
![CredentialforCodecommit](../../../images/2/23.png?width=90pc)
36. Chọn user vừa tạo và chọn tab Security Credentials.
![CredentialforCodecommit](../../../images/2/24.png?width=90pc)
37. Kéo xuống mục **HTTPS Git credentials for AWS CodeCommit** và chọn **Generate Credentials**
![CredentialforCodecommit](../../../images/2/25.png?width=90pc)
CodeStar bây giờ đã cấu hình xong một CI/CD pipeline để phân phối project artefact của bạn vào một máy chủ web Elastic Beanstalk PaaS công khai. Quá trình này sẽ mất một vài phút, hãy kiểm tra liên kết trong đường dẫn và nhấn vào liên kết đó và xem ứng dụng java HelloWorld mới của bạn đã được tạo.

31. Truy cập **CodeStar** và chọn **Projects**
32. Chọn **TravelBuddy**, chọn tab **Pipeline**. Bạn sẽ thấy 3 giai đoạn được tạo **Source, Build và Deploy.**
![CodeStarProject](../../../images/2/26.png?width=90pc)
33. Bạn có thể chọn **View application** tại góc trên bên phải để truy cập ứng dụng đang chạy của bạn.
![CodeStarProject](../../../images/2/27.png?width=90pc)