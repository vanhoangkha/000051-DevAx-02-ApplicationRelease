+++
title = "Challenge - Triển khai thông qua Pipeline"
weight = 2
chapter = false
pre = "<b>3.2. </b>"
+++

Trong bài tập này, bạn sẽ thực hiện một thay đổi đối với ứng dụng, commit và push những thay đổi đó để kích hoạt quy trình CodePipeline để xây dựng lại và triển khai dự án của bạn vào môi trường Elastic Beanstalk. Dưới đây là các bước hướng dẫn chung mà bạn có thể làm theo để thực hiện thay đổi đối với tệp index.jsp và push thay đổi đó sang git, thao tác này sẽ kích hoạt pipeline CI/CD để xây dựng và triển khai phiên bản mới của ứng dụng.

1. Tạo một nhánh mới.
![DeployPipeline](../../../images/3/26.png?width=90pc)
2. Tìm trang index.jsp.
3. Thay đổi đoạn văn bản hiển thị trên băng chuyền. 
![DeployPipeline](../../../images/3/27.png?width=90pc)
4. Commit thay đổi
5. Chuyển tới nhánh master
6. Merge các thay đổi
![DeployPipeline](../../../images/3/28.png?width=90pc)
7. Đẩy nhánh về điểm gốc
![DeployPipeline](../../../images/3/29.png?width=90pc)
8. Truy cập vào lại đường dẫn của ứng dụng khi triển khai những thay đổi đối với môi trường Elastic Beanstalk. Bạn nên nhấp vào các thành phần khác nhau trong bảng điều khiển dự án CodeStar và xem lại các hoạt động đang được thực hiện bởi CodeBuild, CodePipeline và Elastic Beanstalk. 
9.  Kiểm tra ứng dụng để xác nhận các thay đổi đã được triển khai như mong đợi
![DeployPipeline](../../../images/3/30.png?width=90pc)