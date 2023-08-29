# AWS CLI 설치 & 명령어


[최신 버전의 AWS CLI 설치 또는 업데이트](https://docs.aws.amazon.com/ko_kr/cli/latest/userguide/getting-started-install.html)

AWS CLI 설치

- MAC 기준

```jsx
brew install awscli
```

설치후 IAM설정

```jsx
aws configure
```

아래 경로에서 설정 확인가능함

```jsx
~/.aws
```

S3 버킷 다른 버킷으로 복사

- 다른 리전일때

```jsx
aws s3 sync --source-region SOURCE-REGION-NAME --region DESTINATION-REGION-NAME s3://BUCKET-SOURCE s3://BUCKET-TARGET
```

- 같은 리전일때

```jsx
aws s3 sync --region REGION-NAME s3://BUCKET-SOURCE s3://BUCKET-TARGET
```