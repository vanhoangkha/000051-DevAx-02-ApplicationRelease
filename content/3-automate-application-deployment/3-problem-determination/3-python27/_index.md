+++
title = "Python 2.7 EOS"
weight = 3
chapter = false
pre = "<b>3.3.1 </b>"
+++

#### Cập nhật cấu hình ebextensions chuyển sang sử dụng python 3

Do hiện tại Elastic Beanstalk sử dụng Python 3 nên chúng ta sẽ cần cập nhật lại cấu hình trong thư mục .ebextensions
nằm trong Project của chúng ta.

Cập nhật nội dung  file **sshd.config** như hướng dẫn bên dưới.
+ Chỉnh sửa **python27-devel** sang **python3-devel**
+ Chỉnh sửa **python27-pip** sang **python3-pip**
+ Chỉnh sửa **/etc/init.d/sshd restart** sang **sudo service sshd restart**

```
packages:
  yum:
    python3-devel: []
    python3-pip: []
    gcc: []
  python:
    pycryptodome: []
    boto3: []
files:
  "/usr/local/bin/get_authorized_keys" :
    mode: "000755"
    owner: root
    group: root
    source: https://awscodestar-templates-common.s3.amazonaws.com/ap-southeast-2/get_authorized_keys
commands:
  01_update_ssh_access:
    command: >
      sed -i '/AuthorizedKeysCommand /s/.*/AuthorizedKeysCommand \/usr\/local\/bin\/get_authorized_keys/g' /etc/ssh/sshd_config &&
      sed -i '/AuthorizedKeysCommandUser /s/.*/AuthorizedKeysCommandUser root/g' /etc/ssh/sshd_config &&
      sudo service sshd restart

```


Cập nhật nội dung  file **set-instance-credit-unlimited** như hướng dẫn bên dưới.
+ Chỉnh sửa **pip install awscli --upgrade --user** sang **pip3 install awscli --upgrade --user**

```
commands:
  upgrade-aws-cli:
    command: |
      pip3 install awscli --upgrade --user
  set-instance-credit-unlimited:
    command: |
      aws ec2 modify-instance-credit-specification --region ap-southeast-1 --instance-credit-specification '[{"InstanceId": "'"$(wget -q -O - http://169.254.169.254/latest/meta-data/instance-id)"'","CpuCredits": "unlimited"}]'
```
Sau khi chỉnh sửa xong , thực hiện commit và push code lên Code Commit để kích hoạt pipeline.
