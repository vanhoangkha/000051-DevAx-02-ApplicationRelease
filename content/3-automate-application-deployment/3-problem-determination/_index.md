+++
title = "Determine error"
weight = 3
chapter = false
pre = "<b>3.3. </b>"
+++

##### Check buildspec.yml file
The **buildspec.yml** file is an important part that you need to ensure the accuracy of this file. This file will run through the stages which you can see in the console under CodeBuild in its log.
This file is divided into several parts

install - type of installation in which you want to run the code. In this case we will use openjdk8.
```
phases:
  install:
    runtime-versions:
      java: openjdk8
    commands:
      - pip install --upgrade awscli
```
Next are the commands you want to run before executing the build project. This is required before building the project. And in this example, we will run the mvn command to compile all the necessary settings.
```
pre_build:
  commands:
    - mvn clean compile test
```
Next is to create the build stage
```
build:
  commands:
    - mvn war:exploded
```
After mvn build, is how to set required environment for each environment in .ebextensions to be read by required applications
```
  post_build:
    commands:
      - cp -r .ebextensions/ target/travelbuddy/
      - aws cloudformation package --template template.yml --s3-bucket $S3_BUCKET --output-template-file template-export.yml

      # Do not remove this statement. This command is required for AWS CodeStar projects.
      # Update the AWS Partition, AWS Region, account ID and project ID in the project ARN on template-configuration.json file so AWS CloudFormation can tag project resources.
      - sed -i.bak 's/\$PARTITION\$/'${PARTITION}'/g;s/\$AWS_REGION\$/'${AWS_REGION}'/g;s/\$ACCOUNT_ID\$/'${ACCOUNT_ID}'/g;s/\$PROJECT_ID\$/'${PROJECT_ID}'/g' template-configuration.json
artifacts:
  type: zip
  files:
    - template-export.yml
    - template-configuration.json
```
2. Check **CodeBuild** console
Check CodeBuild console and check if any errors appear during deployment.
If yes, select view details to see what error occurred.

![Error](/images/3/31.png?width=90pc)

Select Buildlog tab and scroll down to error log.
In this example, you may find the root access address in the template.yml file is incorrect.
```
Unable to upload artifact target/ROOT referenced by SourceBundle parameter of EBApplicationVersion resource.
Parameter SourceBundle of resource EBApplicationVersion refers to a file or folder that does not exist /codebuild/output/src812543603/src/target/ROOT

[Container] 2020/06/05 02:16:17 Command did not exit successfully aws cloudformation package --template template.yml --s3-bucket $S3_BUCKET --output-template-file template-export.yml exit status 255
[Container] 2020/06/05 02:16:17 Phase complete: POST_BUILD State: FAILED
[Container] 2020/06/05 02:16:17 Phase context status code: COMMAND_EXECUTION_ERROR Message: Error while executing command: aws cloudformation package --template template.yml --s3-bucket $S3_BUCKET --output-template-file template-export.yml. Reason: exit status 255
```
Here is the error in the template.yml file that needs to be changed:
```
EBApplicationVersion:
  Description: The version of the AWS Elastic Beanstalk application to be created for this project.
  Type: AWS::ElasticBeanstalk::ApplicationVersion
  Properties:
    ApplicationName: !Ref "EBApplication"
    Description: The application version number.
    SourceBundle: "target/ROOT"
```
to
```
EBApplicationVersion:
  Description: The version of the AWS Elastic Beanstalk application to be created for this project.
  Type: AWS::ElasticBeanstalk::ApplicationVersion
  Properties:
    ApplicationName: !Ref "EBApplication"
    Description: The application version number.
    SourceBundle: "target/travelbuddy"
```
3. Elastic beanstalk update successful or not?
Go to Elastic beanstalk, go to the travelbuddy app, and select Request Logs.

![RequestLogs](/images/3/32.png?width=90pc)

Here there are 2 types of logs: Last 100 Lines and Full Logs. Select the appropriate type and download.

![RequestLogs](/images/3/33.png?width=90pc)

Check the downloaded log files to see why the server failed to start.
Here are a few example problems:
- Example of communication between client and RDS server not working
```
05-Jun-2020 03:27:26.514 SEVERE [localhost-startStop-1] org.apache.tomcat.jdbc.pool.ConnectionPool.init Unable to create initial connections of pool.
	com.mysql.cj.jdbc.exceptions.CommunicationsException: Communications link failure

The last packet sent successfully to the server was 0 milliseconds ago. The driver has not received any packets from the server.
		at com.mysql.cj.jdbc.exceptions.SQLError.createCommunicationsException(SQLError.java:174)
		at com.mysql.cj.jdbc.exceptions.SQLExceptionsMapping.translateException(SQLExceptionsMapping.java:64)
		at com.mysql.cj.jdbc.ConnectionImpl.createNewIO(ConnectionImpl.java:836)
		at com.mysql.cj.jdbc.ConnectionImpl.<init>(ConnectionImpl.java:456)
		at com.mysql.cj.jdbc.ConnectionImpl.getInstance(ConnectionImpl.java:246)
```
In this example, the RDS endpoint declared in .eexentions is wrong. Repairing the RDS endpoint should fix this error.
- Example of a not existing table in RDS:
```
java.sql.SQLSyntaxErrorException: Table 'travelbuddy.flightspecial' doesn't exist
	com.mysql.cj.jdbc.exceptions.SQLError.createSQLException(SQLError.java:120)
	com.mysql.cj.jdbc.exceptions.SQLError.createSQLException(SQLError.java:97)
	com.mysql.cj.jdbc.exceptions.SQLExceptionsMapping.translateException(SQLExceptionsMapping.java:122)
	com.mysql.cj.jdbc.ClientPreparedStatement.executeInternal(ClientPreparedStatement.java:953)
	com.mysql.cj.jdbc.ClientPreparedStatement.executeQuery(ClientPreparedStatement.java:1003)
	sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
```
To fix this error, log in to MySQL and access the database, repeat the database creation instructions in the first workshop.