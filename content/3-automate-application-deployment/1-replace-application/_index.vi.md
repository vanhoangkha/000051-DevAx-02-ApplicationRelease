+++
title = "Thay thế ứng dụng mẫu"
weight = 1
chapter = false
pre = "<b>3.1. </b>"
+++

Vì chúng ta sắp thực hiện thay đổi đối với mã nguồn, bạn nên sử dụng Git để tạo một nhánh mới. Bạn có thể thực hiện việc này thông qua dòng lệnh hoặc sử dụng IDE Eclipse. Đặt tên cho nhánh mới là **new-implementation**. Để tạo và chuyển sang nhánh mới trong Eclipse thực hiện các bước sau:
- Nhấp chuột phải vào project, chọn **Team** 
- Chọn **Switch To** , chọn **New Branch…**

![CreateNewBranch](/images/3/1.png?width=90pc)

- Đặt tên nhánh mới là **new-implementation** và chọn **Finish**.

![CreateNewBranch](/images/3/2.png?width=90pc)

Trong ví dụ này, chúng ta sẽ sử dụng dòng lệnh.
1. Mở terminal và truy cập thư mục: *C:\Users\Administrator\git\TravelBuddy* 
2. Chạy dòng lệnh ``git checkout -b new-implementation`` để tạo và chuyển sang nhánh mới.

![CreateNewBranch](/images/3/3.png?width=90pc)

3. Tải file **TravelBuddy.zip** và giải nén.

{{%attachments /%}}

{{%notice note%}}
Tải xuống tệp này bằng trình duyệt trong máy ảo bạn đang chạy.
{{%/notice%}}

4. Trong Eclipse IDE, nhấp chuột phải vào root element của project **TravelBuddy** và chọn **Show In -> System Explorer**.

![ShowInExplorer](/images/3/4.png?width=90pc)

5. Xóa thư mục **src** và **target**
6. Sao chép nội dung thư mục TravelBuddy đã giải nén vào thư mục vừa mở từ Eclip IDE. Bạn có thể xóa thư mục **.ebextensions**  ở thư mục đích trước khi thực hiện sao chép, hoặc chọn **Replace** để đảm bảo các tập tin và thư mục được ghi đè chính xác.\
Bạn sẽ thấy trong thư mục .ebextensions có 3 tập tin, một là các thông số bạn cần cập nhập với RDS endpoint, hai tập tin còn lại được tạo trước bằng cài đặt codecommit - set-instance-credit-unlimited.config sshd.config.
7. Mở tập tin **env_vars.config** trong thư mục **.ebextensions** và thay thế value bằng RDS endpoint:
```
option_settings:
  - namespace: aws:elasticbeanstalk:application:environment
    option_name: JDBC_CONNECTION_STRING
    value: <REPLACE YOUR DETAIILS HERE>
```
Bạn có thể lấy giá trị RDS của mình bằng cách đăng nhập vào AWS Console, truy cập RDS instance đang chạy và lấy thông tin endpoint từ đó và thêm nó vào tệp env_vars.

Bạn cũng có thể lấy thông số này từ cloudformation stack trong mục Output của Cloudformation stack được tạo từ đầu bài.

![CreateNewBranch](/images/3/5.png?width=90pc)

8. Cập nhật phiên bản Python.
- Mở tệp **.ebextensions/sshd.config**:
  - Chỉnh sửa **python27-devel** sang **python3-devel**.
  - Chỉnh sửa **python27-pip** sang **python3-pip**.
  - Chỉnh sửa **/etc/init.d/sshd restart** sang **sudo service sshd restart**.

![CreateNewBranch](/images/3/38.png?width=90pc)

- Mở tệp **.ebextensions/set-instance-credit-unlimited**:
  - Chỉnh sửa **pip install awscli –upgrade –user** sang **pip3 install awscli –upgrade –user**

![CreateNewBranch](/images/3/39.png?width=90pc)

9. Nhấp chuột phải vào thư mục gốc của project và chọn **Maven | Update Project | OK**

![CreateNewBranch](/images/3/6.png?width=90pc)

![CreateNewBranch](/images/3/7.png?width=90pc)

10. Khi cập nhật hoàn tất, bạn sẽ thấy cấu trúc thư mục của dự án tương tự như hình dưới đây:

![CreateNewBranch](/images/3/8.png?width=90pc)

11. Bạn vừa hoàn tất hoàn tất việc thay đổi mã nguồn, bây giờ bạn cần thêm các tập tin bạn vừa thay đổi vào nhánh **new-implementation** và commit các tập tin này.

12. Sử dụng terminal, truy cập vào thư mục **TravelBuddy**

- Nhập `git status` để xem các thay đổi của tập tin mã nguồn.

![CreateNewBranch](/images/3/9.png?width=90pc)

- Nhập `git config --global user.email "you@example.com"`
- Nhap `git config --global user.name awsstudent`
- Nhập `git add .` để thêm các tập tin đã thay đổi
- Nhập `git commit -m "Baseline implementation"` để commit các thay đổi.

![CreateNewBranch](/images/3/10.png?width=90pc)

- Nhập `git checkout master` để chuyển tới nhánh master.
- Merge các thay đổi của nhánh **new-implementation** tới nhánh **master** bằng lệnh `git merge new-implementation`

![CreateNewBranch](/images/3/11.png?width=90pc)

- Kiểm tra lại bằng lệnh `git status`.

![CreateNewBranch](/images/3/12.png?width=90pc)

13. Để đẩy các thay đổi này tới CodeCommit, trong Eclipse, nhấp chuột phải và project root và chọn **Team | Push to Origin…**

![CreateNewBranch](/images/3/13.png?width=90pc)

14. Chọn **Close**.

![CreateNewBranch](/images/3/14.png?width=90pc)

Sẽ mất một vài phút để đẩy mã nguồn lên và tiến hành triển khai.

![ReplaceApplication](/images/3/15.png?width=90pc)

15. Để ứng dụng có thể truy cập được vào database, cập nhật security rule.

16. Mở bảng điều khiển của EC2.

17. Ấn **Security Groups** ở menu phía bên trái.
- Chọn **DBSecurityGroup**

![ReplaceApplication](/images/3/35.png?width=90pc)

18. Ấn **Edit inbound rules**

![ReplaceApplication](/images/3/36.png?width=90pc)

19. Ấn **Add rule**.
- Chọn dịch vụ **MySQL/Aurora** 
- Chọn SecurityGroup ID từ **travelbuddyapp**.
- Chọn **Save rules**.

![ReplaceApplication](/images/3/37.png?width=90pc)


#### Caching
Trong bài thực hành này, chúng ta sẽ tăng tốc việc build trong tương lại bằng cách lưu trữ các artifacts (ví dụ như java packages) trong S3 bucket có thể sử dụng trên việc build tiếp theo.

1. Mở tập tin buildspec.yml, bạn sẽ thấy các dòng sau:
```
cache:
  paths:
    - '/root/.m2/**/*'
```

2. Mở terminal.

3. Chạy dòng lệnh dưới đây để tạo một caching bucket mới. Thay **<YOURNAME-NO-SPACES>** bằng tên bạn muốn.
```
aws s3 mb s3://cachingbucket-<YOURNAME-NO-SPACES>
```

![ReplaceApplication](/images/3/16.png?width=90pc)


4. AWS CodeBuild cần truy cập vào cache bucket, do đó chúng ta cần tên IAM permissions. Mở AWS Console và truy cập IAM.

5. Truy cập tab Policies và tìm policy **CodeStar_travelbuddy_PermissionsBoundary**

![ReplaceApplication](/images/3/17.png?width=90pc)

6. Chọn **Edit policy**.

![ReplaceApplication](/images/3/18.png?width=90pc)

7. Trong trình soạn thảo, chọn JSON và thêm policy sau:
``` JSON
        {
            "Sid": "CBCachePolicy",
            "Effect": "Allow",
            "Action": "s3:*",
            "Resource": [
                "arn:aws:s3:::cachingbucket-<YOURNAME-NO-SPACES>",
                "arn:aws:s3:::cachingbucket-<YOURNAME-NO-SPACES>/*"
            ]
        },
```
- Chọn **Review policy** 
![ReplaceApplication](/images/3/19.png?width=90pc)

8. Chọn **Save changes**.

9. Truy cập AWS Codestar và đi tới tab Pipeline của project.
- Chọn **AWS CodeBuild** trong Build section của Pipeline

![ReplaceApplication](/images/3/20.png?width=90pc)

10.  Chọn **Edit**, và chọn **Artifacts**

![ReplaceApplication](/images/3/21.png?width=90pc)

11. Dưới mục **Additional Configuration**, bỏ chọn **Allow AWS CodeBuild to modify this service role so it can be used with this build project**
- Chọn **Amazon S3** cho kiểu Cache.
- Chọn bucket bạn vừa tạo để lưu cache.
- Ấn **Update artifacts**

![ReplaceApplication](/images/3/22.png?width=90pc)

12. Sau khi triển khai hoàn tất, chọn **Application Endpoint** tại trang CodeStar dashboard. Bạn sẽ thấy website TravelBuddy được triển khai trên môi trường Elastic Beanstalk.

![ReplaceApplication](/images/3/25.png?width=90pc)


