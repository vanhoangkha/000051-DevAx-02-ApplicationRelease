+++
title = "(Optional) Sử dụng SDK để truy vấn Elastic Beanstalk"
weight = 3
chapter = false
pre = "<b>5.3. </b>"
+++

Trong phần này, bạn sẽ sử dụng Java SDK để kiểm tra môi trường AWS của mình và truy vấn thông tin về Elastic Beanstalk.
1. Tải tập tin **BeanstalkManager.zip**. BeanstalkManager là một ứng dụng Java mẫu sử dụng SDK AWS Elastic Beanstalk API.
{{%attachments /%}}
2. Giải nén và mở project trong Eclipse IDE.
![UseSDKQueryEB](../../../images/5/7.png?width=90pc)
3. Chạy Unit Test cho ứng dụng này
4. Unit Test sẽ trả về các thông tin:
- Liệt kê các ứng dụng đăng ký với Elastic Beanstalk ở region mặc định.
- Liệt kê các ứng dụng và Application Versions đã được đăng ký.
![UseSDKQueryEB](../../../images/5/8.png?width=90pc)
{{% notice tip %}}
Nếu bạn không thấy bất kỳ kết quả trả về nào, có nghĩa rằng bạn đã chọn sai region. Kiểm tra tập tin **src/main/java/idevelop.samples/AmazonElasticBeanstalkClientFactory.java** để xem bạn đã chọn đúng region chưa.
{{% /notice %}}

Ứng dụng mẫu này cung cấp cho bạn ý tưởng về cách bạn có thể sử dụng SDK để truy xuất thông tin về môi trường Elastic Beanstalk.