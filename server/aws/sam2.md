# SAM S3이벤트 테스트

상태: Done
생성일: 2023년 1월 10일 오후 2:18
유형: 코드

SAM 이벤트 처리 예시 https://github.com/amazon-archives/serverless-app-examples

- nodejs 16 으로 세팅함
- olobo 버킷에 test 이미지 파일을 올려놓음

app.js

- 람다 함수

```jsx
const AWS = require('aws-sdk');
const s3 = new AWS.S3();

exports.lambdaHandler = (event, context, callback) => {
    // Get the object from the event and show its content type
    const bucket = event.Records[0].s3.bucket.name;
    const key = decodeURIComponent(event.Records[0].s3.object.key.replace(/\+/g, ' '));
    s3.getObject({ Bucket: bucket, Key: key }, (error, data) => {
        if (error) {
            console.log(error);
            console.log(`Error getting object ${key} from bucket ${bucket}. Make sure they exist and your bucket is in the same region as this function.`);
            callback(error);
        } else {
            console.log(`CONTENT TYPE: ${data.ContentType}`);
            callback(null, data.ContentType);
        }
    });
};
```

template.yaml

- CloudFormation 방식으로, 스택이라는 리소스를 생성하며 aws 로 배포가능하게 해줌

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
        - S3CrudPolicy:
            BucketName: !Sub "olobo"
      Events:
        HelloWorld:
          Type: S3 # More info about API Event Source: https://github.com/awslabs/serverless-application-model/blob/master/versions/2016-10-31.md#api
          Properties:
            Bucket:
              Ref: Bucket1
            Events: s3:ObjectCreated:*
  Bucket1:
    Type: AWS::S3::Bucket
# 아래 부분을 추가하여 배포하면 버킷을 새로 생성함
#    Properties: 
#      BucketName: !Sub "olobo"
```

event.json

- S3 에서 객체가 생성되었을때 넘어오는 값

```jsx
{
  "Records": [
    {
      "eventVersion": "2.0",
      "eventSource": "aws:s3",
      "awsRegion": "us-west-2",
      "eventTime": "1970-01-01T00:00:00.000Z",
      "eventName": "ObjectCreated:Put",
      "userIdentity": {
        "principalId": "EXAMPLE"
      },
      "requestParameters": {
        "sourceIPAddress": "127.0.0.1"
      },
      "responseElements": {
        "x-amz-request-id": "EXAMPLE123456789",
        "x-amz-id-2": "EXAMPLE123/5678abcdefghijklambdaisawesome/mnopqrstuvwxyzABCDEFGH"
      },
      "s3": {
        "s3SchemaVersion": "1.0",
        "configurationId": "testConfigRule",
        "bucket": {
          "name": "olobo",
          "ownerIdentity": {
            "principalId": "EXAMPLE"
          },
          "arn": "arn:aws:s3:::olobo"
        },
        "object": {
          "key": "test.png",
          "size": 1024,
          "eTag": "0123456789abcdef0123456789abcdef",
          "sequencer": "0A1B2C3D4E5F678901"
        }
      }
    }
  ]
}
```

위와 같이 코드를 작성후 람다를 실행하였을때 content type 을 가져오는걸 확인할 수 있었음

참고

[Amazon S3 이벤트 처리](https://docs.aws.amazon.com/ko_kr/serverless-application-model/latest/developerguide/serverless-example-s3.html)

[자습서: Amazon S3 트리거를 사용하여 Lambda 함수 호출](https://docs.aws.amazon.com/ko_kr/lambda/latest/dg/with-s3-example.html)