# 도커 컨테이너 중단시 슬랙 리포팅 및 재실행

## 상황

* 젠킨스를 올린 도커 컨테이너가 중단됨

## 해결

* 슬랙이 권장하는 방식인 앱생성 방식으로 생성하고 웹훅 URL을 SLACK\_WEBHOOK\_URL에 세팅
* 슬랙 리포팅과 컨테이너 재실행함
* 크론탭으로 5분마다 확인

```jsx
#!/bin/bash

DOCKER_CONTAINER_NAME=""
SLACK_WEBHOOK_URL=""
JENKINS_URL=""

if docker inspect -f '{{.State.Running}}' "$DOCKER_CONTAINER_NAME" 2>/dev/null | grep -q "false"; then
    echo "Jenkins container is not running. Sending notification to Slack..."

    # 슬랙으로 알림 보내기
    curl -X POST -H 'Content-type: application/json' \\
        --data "{\\"text\\":\\"Jenkins container is not running! Restarting Jenkins... <$JENKINS_URL|Jenkins Page>\\"}" \\
        "$SLACK_WEBHOOK_URL"

    # Jenkins 컨테이너 다시 실행
    dccd up
else
    echo "Jenkins container is running."
fi
```



