# AWS IAM

AWS IAM(Identity and Access Management)

AWS 리소스에 대한 액세스 제어를 제공하는 서비스입니다. IAM은 사용자, 그룹, 권한, 정책 및 인증 정보를 관리하는데 사용됩니다. 이를 통해 리소스에 대한 액세스를 제한하고, 보안 취약점을 최소화할 수 있습니다. IAM를 사용하면 AWS 리소스에 대한 액세스를 정책에 따라 제어할 수 있으며, 보안 인증 기능을 제공하여 리소스를 보호할 수 있습니다.

- AWS 의 IAM은 사용자를 추가하고 쉽게 생성할 수 있는데 SECRET ACCESS KEY는 생성할 때 한번만 확인할 수 있다.

AWS_ACCESS_KEY_ID / AWS_SECRET_ACCESS_KEY 를 환경설정에 추가하여 리소스에 엑세스 할 수 있다. 

```jsx
vim ~/.zshrc
```

추가

```jsx
export AWS_ACCESS_KEY_ID=
export AWS_SECRET_ACCESS_KEY=
export AWS_DEFAULT_REGION=
```