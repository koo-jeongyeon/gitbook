# SAM intellij 배포

상태: Done
생성일: 2023년 1월 10일 오후 5:09
유형: 세팅

# 배포방식

1. template.yaml 을 우클릭하여 Sync Serverless Application 선택

![스크린샷 2023-01-10 오후 5.11.51.png](SAM%20intellij%20%E1%84%87%E1%85%A2%E1%84%91%E1%85%A9%20916cdae6aae94ec8963cc62cf3947bc8/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-01-10_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_5.11.51.png)

1. 아래와 같이 Create Stack 에 스택의 이름을 정하고, 빌드 프로세스가 생성하는 배포 패키지를 호스팅할 S3 버킷을 선택하고 Sync를 누르면 CloudFormation을 사용하여 리소스를 구성하고 스택를 생성합니다

![스크린샷 2023-01-10 오후 5.13.31.png](SAM%20intellij%20%E1%84%87%E1%85%A2%E1%84%91%E1%85%A9%20916cdae6aae94ec8963cc62cf3947bc8/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-01-10_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_5.13.31.png)

1. 배포가 완료된 모습

![스크린샷 2023-01-10 오후 5.20.46.png](SAM%20intellij%20%E1%84%87%E1%85%A2%E1%84%91%E1%85%A9%20916cdae6aae94ec8963cc62cf3947bc8/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-01-10_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_5.20.46.png)

### 배포한 템플릿 코드

```jsx
AWSTemplateFormatVersion: '2010-09-09'
Transform: AWS::Serverless-2016-10-31
Description: >
  s3lambda

  Sample SAM Template for s3lambda
  
# More info about Globals: https://github.com/awslabs/serverless-application-model/blob/master/docs/globals.rst
Globals:
  Function:
    Timeout: 3
    MemorySize: 128

Resources:
  HelloWorldFunction:
    Type: AWS::Serverless::Function # More info about Function Resource: https://github.com/awslabs/serverless-application-model/blob/master/versions/2016-10-31.md#awsserverlessfunction
    Properties:
      CodeUri: hello-world/
      Handler: app.lambdaHandler
      Runtime: nodejs16.x
      Architectures:
        - arm64
      Policies:
        - S3ReadPolicy:
            BucketName: !Sub "olobo"
      Events:
        HelloWorld:
          Type: S3 # More info about API Event Source: https://github.com/awslabs/serverless-application-model/blob/master/versions/2016-10-31.md#api
          Properties:
            Bucket:
              Ref: Bucket1
            Events: s3:ObjectCreated:Put
  Bucket1:
    Type: AWS::S3::Bucket
```

# AWS Toolkit

AWS Toolkit 를 선택하면 왼쪽바에서 스택을 확인할 수도 있고

CloudWatch 의 로그를 확인할 수도 있습니다

![스크린샷 2023-01-10 오후 5.20.07.png](SAM%20intellij%20%E1%84%87%E1%85%A2%E1%84%91%E1%85%A9%20916cdae6aae94ec8963cc62cf3947bc8/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-01-10_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_5.20.07.png)

참고

[AWS Toolkit for IntelliJ를 통해 손쉽게 서버리스 앱 배포해 보기 | Amazon Web Services](https://aws.amazon.com/ko/blogs/korea/aws-toolkit-for-intellij-now-generally-available/)