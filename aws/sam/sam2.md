# SAM Lambda S3이벤트 트리거, MongoDB 접근코드

상태: Done
생성일: 2023년 1월 10일 오후 2:18
유형: 코드

[SAM 이벤트 처리 예시](https://github.com/amazon-archives/serverless-app-examples)

- nodejs 16 으로 세팅함
- bucket 버킷에 test 이미지 파일을 올려놓음

## AWS의 SDK를 사용

- AWS 서비스용 javascript API 를 제공한다.

[AWS SDK for JavaScript란 무엇인가요?](https://docs.aws.amazon.com/ko_kr/sdk-for-javascript/v2/developer-guide/welcome.html)

```jsx
const AWS = require('aws-sdk');
const s3 = new AWS.S3(); // https://docs.aws.amazon.com/AmazonS3/latest/API/API_Operations_Amazon_Simple_Storage_Service.html
```

## MongoDB 라이브러리 사용

- MongoClient 라이브러리를 사용해 mongodb 에 연결할수있다.
- 데이터를 검색하는 경우 ObjectId를 사용하기 위해선 라이브러리를 추가로 설치해야한다

```jsx
const MongoClient = require('mongodb').MongoClient;
const ObjectId = require('mongodb').ObjectId;
```

- mongoose 가 많이 사용되는 라이브러리 같은데 스키마를 사용할 수 있다는게 장점이라고 한다. 여기선 필요없으니 mongodb로 사용해도 충분한것같다.


## app.js

### lambdaHandler
- event에는 객체의 경로에 대한 정보가 포함되어있다.
    - key가 객체의 경로고 문자열을 split 으로 쪼개서 필요한 폴더의 이름을 추출해 mongodbConnect 함수로 넘겨주었다.
- s3.getObject 는 키값으로 버킷에서 해당 객체를 조회한다.
- [사용 JavaScript Promise](https://docs.aws.amazon.com/ko_kr/sdk-for-javascript/v2/developer-guide/using-promises.html)
```jsx
const AWS = require('aws-sdk');
const s3 = new AWS.S3();
const MongoClient = require('mongodb').MongoClient;
const ObjectId = require('mongodb').ObjectId;

exports.lambdaHandler = async (event, context) => {

    const bucket = event.Records[0].s3.bucket.name;
    const key = decodeURIComponent(event.Records[0].s3.object.key.replace(/\+/g, ' ')); //key path 경로
    const params = {
        Bucket: bucket,
        Key: key,
    };

    //키 경로의 중간부분에 디비에서 id값으로 쓰일 부분을 잘라 넘겨줌
    const parts = key.split('/');
    const id = parts[2];

    try {
        //await로 s3객체를 검사한뒤 (동기적으로) mongoDB관련 함수를 실행한다
        const { ContentType } = await s3.getObject(params).promise();
         await mongodbConnect(`${id}`);
        return ContentType;
    } catch (err) {
        const message = `Error getting object ${key} from bucket ${bucket}. Make sure they exist and your bucket is in the same region as this function.`;
        console.log(message);
        throw new Error(message);
    }
};
```

### mongodbConnect
- MongoDB를 연결하고 필요한 update처리를 한다
- [참고](https://www.mongodb.com/docs/atlas/manage-connections-aws-lambda/)

```jsx
async function mongodbConnect(id) {
    const url = '[MONGODB_URL]';
    const client = new MongoClient(url, { useNewUrlParser: true });

    // MongoDB 연결
    try {
        await client.connect();
        console.log('Connected database.');
    } catch (err) {
        console.log('Failed to connected database.');
    }

    try {
        const db = client.db('[DB]');
        const collection = db.collection('[COLLECTION]');

        //id 값으로 조회하여 업데이트 원하는 항목을 업데이트함
        // 조건
        const query = { _id: ObjectId(id)};
        // 업데이트
        const update = {$set: {status: true}};
        // 옵션
        const options = { returnOriginal: false };
        await collection.findOneAndUpdate(query, update, options);
        console.log(id+' Update succeeded');
    }catch (err) {
        console.log('Failed to update the state of the object. Check if the ObjectId exists in the database.');
    }finally {
        // MongoDB 연결 해제
        client.close();
    }
}
```

## template.yaml

- CloudFormation 방식으로, 스택이라는 리소스를 생성하며 aws 로 배포가능하게 해줌
- 템플릿은 자신에게 맞게 잘 작성해주어야함 아래는 그냥 예시임
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
            BucketName: !Sub "bucket"
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
#      BucketName: !Sub "bucket"
```

## event.json
- 로컬 테스트용도로 사용함
- 위와 같이 코드를 작성후 람다를 실행하였을때 content type 을 가져오는걸 확인할 수 있었음

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
          "key": "키경로",
          "size": 1024,
          "eTag": "0123456789abcdef0123456789abcdef",
          "sequencer": "0A1B2C3D4E5F678901"
        }
      }
    }
  ]
}
```


참고

[Amazon S3 이벤트 처리](https://docs.aws.amazon.com/ko_kr/serverless-application-model/latest/developerguide/serverless-example-s3.html)

[자습서: Amazon S3 트리거를 사용하여 Lambda 함수 호출](https://docs.aws.amazon.com/ko_kr/lambda/latest/dg/with-s3-example.html)