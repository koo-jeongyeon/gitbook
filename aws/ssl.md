# SSL 인증서 수동 갱신

Let’s Encrypt SSL은 자동으로 인증서가 갱신되지만 자동갱신에 오류가 생기는 경우가 있어 수동으로 갱신해줄 필요가 있을 때 수동갱신을 사용한다.

→ 현재는 자동갱신 불가한것으로 파악, 수동갱신 진행

### DEV\_WP\_WEB

* 비트나미 Let’s Encrypt SSL 사용, Certbot은 설치되지 않음
* 비트나미의 SSL 인증서 파일 경로
  * `/opt/bitnami/letsencrypt/certificates/`
  * `/home/bitnami/.ssl/gitt.co/live/`
* nginx
  * 설정 파일 경로
    * `/opt/bitnami/nginx/conf/nginx.conf`
    * `/opt/bitnami/apps/*/conf/nginx-vhosts.conf`
  * 로그 경로
    * `/opt/bitnami/nginx/logs/access.log`
    * `/opt/bitnami/nginx/logs/error.log`

현재 `nginx`의 설정 파일을 보면 아래와 같이 다른 `conf`파일을 만들어 포함하고 있는걸 알 수 있었음

```bash
include "/opt/bitnami/apps/wp-dev/conf/nginx-vhosts.conf"; 
include "/opt/bitnami/apps/wp-qa/conf/nginx-vhosts.conf";
include "/opt/bitnami/apps/wordpress/conf/nginx-vhosts.conf";
```

`conf`파일 내부 `ssl` 설정

* 원래 경로의 키를 갱신해줄 명령어 모름, 각 도메인 별로 새로 수동으로 받아주고 경로 변경하였음
* 나중에 `.pem`키가 `DEV_JAVA_WEB`에서 복사되어 온다는걸 확인하고 **원래 경로로 변경해줌**

```bash
#원래 이 경로의 파일로 갱신하고 있었음
ssl_certificate         /home/bitnami/.ssl/gitt.co/live/fullchain.pem;
ssl_certificate_key     /home/bitnami/.ssl/gitt.co/live/privkey.pem;

#현재 이 경로의 파일로 갱신해줌
ssl_certificate         /opt/bitnami/letsencrypt/certificates/dev-wp-bacon.gitt.co.crt;
ssl_certificate_key     /opt/bitnami/letsencrypt/certificates/dev-wp-bacon.gitt.co.key;
```

*   각 도메인별로 수동으로 갱신하는 방법

    * 만약 이메일이 에러나면 끝에 `renew --days 90` 대신 `run` 실행후 다시 실행

    ```bash
    sudo su
    /opt/bitnami/ctlscript.sh stop #사이트를 멈춤
    ...
    /opt/bitnami/nginx/scripts/ctl.sh : Nginx stopped
    /opt/bitnami/php/scripts/ctl.sh : php-fpm stopped
    /opt/bitnami/mysql/scripts/ctl.sh : mysql not running
    ...
    #인증서 갱신
    #이메일 - 해당 이메일로 인증서 갱신 안내 메일이 발송됨
    sudo /opt/bitnami/letsencrypt/lego --tls --email="이메일" --domains="dev-wp-bacon.gitt.co" --path="/opt/bitnami/letsencrypt" renew --days 90
    sudo /opt/bitnami/letsencrypt/lego --tls --email="이메일" --domains="dev-wp-collins.gitt.co" --path="/opt/bitnami/letsencrypt" renew --days 90

    /opt/bitnami/ctlscript.sh start #사이트 구동
    ```

### DEV\_JAVA\_WEB

* Let’s Encrypt SSL 사용, Certbot은 설치됨
* nginx
  * 주 설정 파일 경로
    * `/etc/nginx/nginx.conf`
  * 사이트별 설정 파일 경로
    * `/etc/nginx/conf.d/*.conf`
* Certbot의 설정파일
  * `/etc/letsencrypt/renewal/gitt.co.conf`
  * 기본적으로 Certbot은 `/etc/letsencrypt/renewal` 디렉터리 내의 설정 파일을 자동으로 감지하고 해당 설정 파일에 정의된 도메인과 갱신 옵션을 사용하여 인증서를 갱신함

#### 자동갱신

* 여기서 `gitt.co`로 갱신을 하고 그걸 `DEV_WP_WEB`으로 복사하는거였음
* 매월 1일 한번만 실행하기 때문에 한번 실패하면 갱신이 안되었던것

```bash
sudo crontab -l
#매월 1일 0시 0분 인증서가 갱신됨
0 0 1 * * /bin/bash -l -c 'certbot renew --quiet'
#매월 1일 1시 0분 인증서가 DEV_WP_WEB로 복사됨
0 1 1 * * /bin/bash -l -c 'scp -i /home/ec2-user/pem/bacon.pem *.pem [bitnami@172.31.30.2](<mailto:bitnami@172.31.30.2>):/home/bitnami/.ssl/gitt.co/archive/'
```

#### 문제

9월달에 갱신이 안되어서 확인해보니 `certbot renew --quiet` 명령어 실행시 오류를 확인함

* 지금까지 어떻게 자동 갱신이 잘 됬는지는 모르겠으나, 수동모드로 인증서 갱신을 해야하는걸로 나옴
* `/etc/letsencrypt/renewal/gitt.co.conf` 확인시에도 수동갱신 설정이 되어있음

```bash
Failed to renew certificate [gitt.co](<http://gitt.co/>) with error: The manual plugin is not working; there may be problems with your existing configuration.
The error was: PluginError('An authentication script must be provided with --manual-auth-hook when using the manual plugin non-interactively.',)
All renewals failed. The following certificates could not be renewed:
/etc/letsencrypt/live/gitt.co/fullchain.pem (failure)
1 renew failure(s), 0 parse failure(s)
```

이런저런 시도한 결과 아래와 같이 갱신됨

* 명령어를 실행하면 `_acme-challenge.gitt.co`레코드 설정(DNS TXT)변경 해줘야한다고 뜸 AWS에서 해당 레코드 값 저 내용으로 변경 해준다음 계속 진행하면 갱신됨

```bash
sudo service nginx stop #nginx 꺼야됨 
sudo certbot certonly --manual -d *.gitt.co #수동갱신 명령어

Saving debug log to /var/log/letsencrypt/letsencrypt.log
Plugins selected: Authenticator manual, Installer None
Cert is due for renewal, auto-renewing...
Renewing an existing certificate for *.gitt.co
Performing the following challenges:
dns-01 challenge for gitt.co

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Please deploy a DNS TXT record under the name
_acme-challenge.gitt.co with the following value:

1K5ZWAVqgUxnbvisTjQbjort-_K40nZmRJaZpawMU5M

Before continuing, verify the record is deployed.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Press Enter to Continue
Waiting for verification...
Cleaning up challenges

IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/gitt.co/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/gitt.co/privkey.pem
   Your certificate will expire on 2023-12-24. To obtain a new or
   tweaked version of this certificate in the future, simply run
   certbot again. To non-interactively renew *all* of your
   certificates, run "certbot renew"
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   <https://letsencrypt.org/donate>
   Donating to EFF:                    <https://eff.org/donate-le>

sudo service nginx start #nginx 시작
```

인증서의 유효기간 확인

```bash
openssl x509 -in /경로/서버인증서.crt -noout -dates
```

* 참고

[Lightsail의 Bitnami 스택에서 SSL 인증서 갱신하기](https://repost.aws/ko/knowledge-center/lightsail-bitnami-renew-ssl-certificate)

[Let's Encrypt 갱신시 Cert not yet due for renewal 문제 해결 방법 - 익스트림 매뉴얼](https://extrememanual.net/27348)

[About Let's Encrypt Renewal](https://community.letsencrypt.org/t/about-lets-encrypt-renewal/145646/4)
