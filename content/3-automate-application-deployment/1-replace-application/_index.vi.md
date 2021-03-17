+++
title = "Thay thế ứng dụng mẫu"
weight = 1
chapter = false
pre = "<b>3.1. </b>"
+++

Vì chúng ta sắp thực hiện thay đổi đối với mã nguồn, bạn nên sử dụng Git để tạo một nhánh mới. Bạn có thể thực hiện việc này thông qua dòng lệnh hoặc sử dụng IDE Eclipse. Đặt tên cho nhánh mới là **new-implementation**. Để tạo và chuyển sang nhánh mới trong Eclipse, nhấp chuột phải vào project, chọn **Team** , chọn **Switch To** , chọn **New Branch…** , đặt tên nhánh mới là **new-implementation** và chọn **Finish**.
![CreateNewBranch](../../../images/3/1.png?width=90pc)
Trong ví dụ này, chúng ta sẽ sử dụng dòng lệnh.
1. Mở terminal và truy cập thư mục *C:\Users\Administrator\git\TravelBuddy* 
2. Chạy dòng lệnh ***git checkout -b new-implementation*** để tạo và chuyển sang nhánh mới.
![CreateNewBranch](../../../images/3/2.png?width=90pc)
3. Tải file **TravelBuddy.zip** và giải nén.
{{%attachments /%}}
4. Trong Eclipse IDE, nhấp chuột phải vào root element của project **TravelBuddy** trong và chọn **Show In -> System Explorer**.
![ShowInExplorer](../../../images/3/3.png?width=90pc)
5. Xóa thư mục **src** và **target**
6. Sao chép nội dung thư mục TravelBuddy đã giải nén vào thư mục vừa mở từ Eclip IDE. Bạn có thể xóa thư mục **.ebextensions**  ở thư mục đích trước khi thực hiện sao chép, hoặc chọn **Replace** để đảm bảo các tập tin và thư mục được ghi đè chính xác.\
Bạn sẽ thấy trong thư mục .ebextensions có 3 tập tin, một là các thông số bạn cần cập nhập với RDS endpoint, hai tập tin còn lại được tạo trước bằng cài đặt codecommit - set-instance-credit-unlimited.config sshd.config.
7. Mở tập tin **env_vars.config** trong thư mục **.ebextensions** và thêm các thêm các mục sau:
```
option_settings:
  - namespace: aws:elasticbeanstalk:application:environment
    option_name: JDBC_CONNECTION_STRING
    value: <REPLACE YOUR DETAIILS HERE>
```
Bạn có thể lấy giá trị RDS của mình bằng cách đăng nhập vào AWS Console, truy cập RDS instance đang chạy và lấy thông tin endpoint từ đó và thêm nó vào tệp env_vars.

Bạn cũng có thể lấy thông số này từ cloudformation stack trong mục Output của Cloudformation stack được tạo từ đầu bài.
![CreateNewBranch](../../../images/3/4.png?width=90pc)
8. Nhấp chuột phải vào thư mục gốc của project và chọn **Maven | Update Project | OK**
![CreateNewBranch](../../../images/3/5.png?width=90pc)
![CreateNewBranch](../../../images/3/6.png?width=90pc)
9.  Khi cập nhật hoàn tất, bạn sẽ thấy cấu trúc thư mục của dự án tương tự như hình dưới đây:
![CreateNewBranch](../../../images/3/7.png?width=90pc)
10. Bạn vừa hoàn tất hoàn tất việc thay đổi mã nguồn, bây giờ bạn cần thêm các tập tin bạn vừa thay đổi vào nhánh **new-implementation** và commit các tập tin này.\
11. Sử dụng terminal, truy cập vào thư mục **TravelBuddy**

- Nhập *git status* để xem các thay đổi của tập tin mã nguồn.
![CreateNewBranch](../../../images/3/8.png?width=90pc)
- Nhập *git config --global user.email "you@example.com"*.
- Nhập *git add .* để thêm các tập tin đã thay đổi.
- Nhập *git commit -m "Baseline implementation"* để commit các thay đổi.
![CreateNewBranch](../../../images/3/9.png?width=90pc)
- Nhập *git checkout master* để chuyển tới nhánh master.
- Merge các thay đổi của nhánh new-implementation tới nhánh master bằng lệnh *git merge new-implementation*
![CreateNewBranch](../../../images/3/10.png?width=90pc)
- Kiểm tra lại bằng lệnh *git status*.
![CreateNewBranch](../../../images/3/11.png?width=90pc)
122.  Để đẩy các thay đổi này tới CodeCommit, trong Eclipse, nhấp chuột phải và project root và chọn **Team | Push to Origin…**
![CreateNewBranch](../../../images/3/12.png?width=90pc)
13.  Chọn **Close**.\
![CreateNewBranch](../../../images/3/13.png?width=90pc)
Sẽ mất một vài phút để đẩy mã nguồn lên và tiến hành triển khai.
![ReplaceApplication](../../../images/3/14.png?width=90pc)
#### Caching
Trong bài thực hành này, chúng ta sẽ tăng tốc việc build trong tương lại bằng cách lưu trữ các artifacts (ví dụ như java packages) trong S3 bucket có thể sử dụng trên việc build tiếp theo.

14. Mở tập tin buildspec.yml và thêm các dòng sau:
```
cache:
  paths:
    - '/root/.m2/**/*'
```
![ReplaceApplication](../../../images/3/15.png?width=90pc)
15. Mở terminal.\
16. Tạo một caching bucket mới
```
aws s3 mb s3://cachingbucket-<YOURNAME-NO-SPACES>
```
![ReplaceApplication](../../../images/3/16.png?width=90pc)
17. AWS CodeBuild cần truy cập vào cache bucket, do đó chúng ta cần têm IAM permissions. Mở AWS Console và truy cập IAM.\
18. Truy cập tab Policies và tìm policy **CodeStar_travelbuddy_PermissionsBoundary**, chọn **Edit policy**.
![ReplaceApplication](../../../images/3/17.png?width=90pc)
19. Trong trình soạn thảo, chọn JSON và thêm policy sau:
``` JSON
        ...
        {
            "Sid": "CBCachePolicy",
            "Effect": "Allow",
            "Action": "s3:*",
            "Resource": [
                "arn:aws:s3:::cachingbucket-<YOURNAME-NO-SPACES>",
                "arn:aws:s3:::cachingbucket-<YOURNAME-NO-SPACES>/*"
            ]
        },
        ...
```
![ReplaceApplication](../../../images/3/18.png?width=90pc)
20. Chọn **Review policy** và chọn **Save changes**.\
21. Truy cập AWS Codestar và đi tới tab Pipeline của project.
![ReplaceApplication](../../../images/3/19.png?width=90pc)
22. Chọn **AWS CodeBuild** trong Buil section của Pipeline
23. Chọn **Edit**, và chọn **Artifacts**
![ReplaceApplication](../../../images/3/20.png?width=90pc)
24. Dưới mục **Additional Configuration**, chọn **Caching** và chọn **S3**
![ReplaceApplication](../../../images/3/21.png?width=90pc)
25. Chọn bucket bạn đã tạo ở trên.\
26. Sau khi triển khai hoàn tất, chọn **Application Endpoint** tại trang CodeStar dashboard. Bạn sẽ thấy website TravelBuddy được triển khai trên môi trường Elastic Beanstalk.
![ReplaceApplication](../../../images/3/25.png?width=90pc)
