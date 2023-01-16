+++
title = "Kết nối Eclipse IDE với AWS Codecommit"
weight = 2
chapter = false
pre = "<b>2.2. </b>"
+++


Trong bài thực hành này, bạn sẽ kết nối Eclipse IDE tới AWS CodeCommit
1. Trong AWS CodeStar dashboard, chọn tab **IDE**
2. Chọn **Eclipse** và chọn **View Instructions**

![OpenIDE](/images/2/28.png?width=90pc)

![OpenIDE](/images/2/29.png?width=90pc)

3. Bạn sẽ sử dụng IAM user mà bạn đã tạo trước đó để cấu hình AWS Codecommit. Đảm bảo rằng bạn có user IAM secret key và access key mà bạn đã tải xuống trước đó.
4. Truy cập máy ảo Windows, mở **Eclipse**. Trong lần đầu tiên khởi động, Eclipse sẽ hỏi bạn cấu hình workspace.
5. Chọn **Use this as the default and do not ask again** và chọn **Launch**

![OpenIDE](/images/2/30.png?width=90pc)

6. Nếu AWS Toolkit đã được cài đặt sẵn trên Eclipse, Eclipse sẽ xuất hiện một hộp thoại **Welcome to the AWS Toolkit for Eclipse**.
- Bạn nhập **Access Key** và **Secret Key** tại hộp thoại này.
- Ấn **Finish**

![ConnectIDE](/images/2/31.png?width=50pc)

7. Nếu chưa được cài đặt, bạn có thể cài đặt AWS Toolkit bằng cách chọn **Help**, chọn **Install new software**

![ConnectIDE](/images/2/32.png?width=90pc)

8. Trong menu thả xuống, tìm *https://aws.amazon.com/eclipse* và chọn các công cụ sau đây:
- AWS Developer Tools
- AWS Deployment Tools
Sau đó, ấn **Next**

![ConnectIDE](/images/2/33.png?width=90pc)

{{%notice note%}}
Nếu nút **Next** không thể ấn thì bạn không cần cài các công cụ đó.
{{%/notice%}}

9. Chọn **Next**.

![ConnectIDE](/images/2/34.png?width=50pc)

10. Trong cửa sổ **Review**, chọn **Next**

![ConnectIDE](/images/2/35.png?width=50pc)

11. Chọn **I accept the terms of the license agreements** và chọn **Finish**

![ConnectIDE](/images/2/36.png?width=50pc)

12. Sau khi cài đặt xong, chọn **Restart Now**
13. Nếu bạn đã cài đặt thông tin ở bước 6 thì bỏ qua bước 13 đến 16.
- Mở **Eclipse IDE**, tìm biểu tượng **AWS**, nhấp vào mũi tên thả xuống và chọn **Preferences…**

![ConnectIDE](/images/2/37.png?width=90pc)

14. Đặt **Default Profile** thành *default*
15. Nhập thông tin **Profile credentials** vào hộp thoại **Profile Details**
16. Chọn **Apply and Close**

![ConnectIDE](/images/2/38.png?width=90pc)


17. Chọn biểu tượng **AWS** và chọn **Import AWS CodeStar Project…**.

![ConnectIDE](/images/2/39.png?width=90pc)

18. Chọn region đang tiến hành bài thực hành tại mục **Account and region**. Chọn **TravelBuddy** và **TravelBuddy** repository.
19. Nhập thông tin git credentials đã tải xuống ở phần trước vào mục **Configure Git credentials**
20. Chọn **Next**.

![ConnectIDE](/images/2/40.png?width=90pc)

21. Có thể xuất hiện một hội thoại báo lỗi, bạn có thể bỏ qua hộp thoại này bằng cách chọn **OK**.
22. Ấn **Next**.

![ConnectIDE](/images/2/41.png?width=90pc)


23. Để các thông tin khác như mặc định và ấn **Finish**.

![ConnectIDE](/images/2/43.png?width=90pc)

IDE sẽ lấy các tập tin từ kho lưu trữ CodeCommit và chuẩn bị để bạn có thể bắt đầu viết mã nguồn. Có thể có lỗi xảy ra với mã nguồn vừa tải xuống này, tuy nhiên bạn có thể bỏ qua nó vì bạn sẽ không sử dụng mã này ngoài việc kiểm tra nó trong bước tiếp theo.

24. Hãy dành chút thời gian để xem cấu trúc project trước khi bạn có thể tiếp tục. Ứng dụng mẫu Hello World này trình bày một trang web cho người dùng và kết nối với một Spring controller chuẩn.

![ConnectIDE](/images/2/44.png?width=90pc)

Vậy là bạn đã hoàn thành việc kết nối và pull source code từ code commit về máy của developer.
Ở bước tiếp theo chúng ta sẽ thay đổi code và release phiên bản mới của ứng dụng lên Elastic Beanstalk.