+++
title = "Cấu hình tự động phát hành ứng dụng"
date = 2021
weight = 1
chapter = false
+++

# Cấu hình tự động phát hành ứng dụng

Trong bài thực hành này, bạn sẽ tự động hóa quy trình thủ công mà bạn đã sử dụng trong bài đầu tiên **DevAx::academy - Chuyển đổi ứng dụng Monoliths** để triển khai ứng dụng Java TravelBuddy tới Elastic Beanstalk - được triển khai trên môi trường Tomcat.
Bạn sẽ sử dụng dịch vụ AWS Developer Tools **CodeStar** để cung cấp một Continuous Integration/Continuous Delivery (CI/CD) pipeline. Bạn cũng sẽ được thử nghiệm triển khai một Windows Service tới máy ảo EC2 mà bạn quản lý trực tiếp bằng dịch vụ **AWS CodeDeploy**
Trong bài thực hành này, bạn sẽ sử dụng các dịch vụ AWS Developer Tools sau:

#### AWS CodeStar
AWS CodeStar cho phép bạn nhanh chóng phát triển, xây dựng và triển khai các ứng dụng trên AWS. AWS CodeStar cung cấp giao diện người dùng thống nhất, cho phép bạn dễ dàng quản lý các hoạt động phát triển phần mềm của mình tại cùng một nơi. Với AWS CodeStar, bạn có thể thiết lập toàn bộ chuỗi công cụ phân phối liên tục của mình trong vài phút, cho phép bạn bắt đầu phát hành mã nhanh hơn. AWS CodeStar giúp cả nhóm của bạn dễ dàng làm việc cùng nhau một cách an toàn, cho phép bạn dễ dàng quản lý quyền truy cập và thêm chủ sở hữu, cộng tác viên và người xem vào các dự án của mình. Mỗi dự án AWS CodeStar đều đi kèm với một bảng điều khiển quản lý dự án, bao gồm khả năng theo dõi sự cố tích hợp được cung cấp bởi Atlassian JIRA Software. Với bảng điều khiển dự án AWS CodeStar, bạn có thể dễ dàng theo dõi tiến độ trong toàn bộ quy trình phát triển phần mềm của mình, từ các hạng mục công việc tồn đọng cho đến việc triển khai mã gần đây của nhóm.

#### AWS CodeCommit

AWS CodeCommit là một dịch vụ kiểm soát mã nguồn được quản lý hoàn toàn giúp các công ty dễ dàng quản lý các kho lưu trữ Git riêng một cách an toàn và có khả năng mở rộng cao. CodeCommit loại bỏ nhu cầu vận hành hệ thống kiểm soát nguồn của riêng bạn hoặc lo lắng về việc mở rộng cơ sở hạ tầng của nó. Bạn có thể sử dụng CodeCommit để lưu trữ an toàn mọi thứ từ mã nguồn đến mã nhị phân, CodeCommit cũng hoạt động liền mạch với các công cụ Git hiện có của bạn.

#### AWS CodeBuild
AWS CodeBuild là một dịch vụ xây dựng được quản lý hoàn toàn nhằm biên dịch mã nguồn, chạy thử nghiệm và sản xuất các gói phần mềm sẵn sàng triển khai. Với CodeBuild, bạn không cần phải cung cấp, quản lý và mở rộng các máy chủ xây dựng của riêng mình. CodeBuild mở rộng quy mô liên tục và xử lý nhiều bản dựng đồng thời, do đó, các bản dựng của bạn không phải chờ đợi trong hàng đợi. Bạn có thể bắt đầu nhanh chóng bằng cách sử dụng môi trường xây dựng đóng gói sẵn hoặc bạn có thể tạo môi trường xây dựng tùy chỉnh sử dụng các công cụ xây dựng của riêng bạn. Với CodeBuild, bạn bị tính phí theo phút cho các tài nguyên máy tính mà bạn sử dụng.

#### AWS CodePipeline

AWS CodePipeline là một dịch vụ tích hợp liên tục và phân phối liên tục để cập nhật ứng dụng và cơ sở hạ tầng nhanh chóng và đáng tin cậy. CodePipeline xây dựng, kiểm tra và triển khai mã của bạn mỗi khi có sự thay đổi mã nguồn, dựa trên các mô hình quy trình phát hành mà bạn xác định. Điều này cho phép bạn cung cấp các tính năng và bản cập nhật nhanh chóng và đáng tin cậy. Bạn có thể dễ dàng xây dựng giải pháp end-to-end bằng cách sử dụng các plugin được tạo sẵn cho các dịch vụ bên thứ ba phổ biến như GitHub hoặc tích hợp các plugin tùy chỉnh của riêng bạn vào bất kỳ giai đoạn nào trong quá trình phát hành của bạn. Với AWS CodePipeline, bạn chỉ trả tiền cho những gì bạn sử dụng. Không có phí trả trước hoặc cam kết dài hạn.

#### AWS CodeDeploy

AWS CodeDeploy là dịch vụ triển khai giúp tự động hóa việc triển khai ứng dụng cho các máy ảo Amazon EC2 hoặc các máy ảo on-premise trong cơ sở của riêng bạn. Bạn có thể triển khai nhiều loại nội dung ứng dụng gần như không giới hạn, chẳng hạn như mã nguồn, web và tệp cấu hình, tệp thực thi, gói, tập lệnh, tệp đa phương tiện, v.v. AWS CodeDeploy có thể triển khai nội dung ứng dụng được lưu trữ trong Amazon S3 bucket, kho GitHub hoặc kho Bitbucket. Bạn không cần thực hiện thay đổi đối với mã nguồn hiện có của mình trước khi có thể sử dụng AWS CodeDeploy. AWS CodeDeploy giúp bạn phát hành nhanh chóng các tính năng mới dễ dàng hơn, giúp bạn tránh thời gian chết trong quá trình triển khai ứng dụng và xử lý sự phức tạp của việc cập nhật ứng dụng của bạn mà không gặp nhiều rủi ro liên quan đến triển khai thủ công dễ xảy ra lỗi.

#### Nội dung
Sau khi hoàn thành bài thực hành, bạn sẽ có thể:

- Sử dụng một ứng dụng monoliths Java chạy Spring trên Tomcat trên một môi trường tính toán chung và thực hiện 'lift-and-shift' lên Amazon Elastic Beanstalk PaaS với CI/CD pipeline để tự động cập nhật, sử dụng AWS CodeStar
- Sử dụng Eclipse IDE để kết nối và kiểm tra mã nguồn dự án từ một kho lưu trữ AWS CodeCommmit do AWS CodeStar thiết lập.
- Thực hiện các thay đổi đối với mã nguồn, cam kết (commit) và đẩy (push) những thay đổi đó vào AWS CodeCommit để kích hoạt quy trình tạo tự động thông qua AWS CodePipeline.
- Spin-up EC2 chạy Windows theo cách thủ công và sử dụng AWS CodeDeploy để triển khai Windows Service cho các máy ảo.
- Hiểu cách lấy thông tin đăng nhập Quản trị viên cho máy ảo EC2 chạy Windows.

#### Kiến thức kỹ thuật cần có
Để có thể hoàn thành bài thực hành này, bạn nên làm quen với:
- Những điều hướng cơ bản của AWS Management Console.
- Chỉnh sửa tập lệnh bằng trình soạn thảo.
- Sử dụng trình soạn thảo Eclipse và ngôn ngữ lập trình Java

#### Môi trường
Tất cả tài nguyên cần thiết để bắt đầu bài thực hành được cung cấp trong CloudFomation template, sử dụng template này để khởi tạo các tài nguyên cho bài thực hành.
Sơ đồ sau mô tả các tài nguyên được triển khai trong bài thực hành.

![Diagram](../../../images/1/0.png?width=50pc)
