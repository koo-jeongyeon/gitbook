# gradle hidetake.ssh 키파일 설정

생성일: 2022년 7월 6일 오후 8:58

GCP 를 gradle에서 원격으로 접속하기 위해

아래와 같이 ssh정보를 작성함

- identity = file('/Users/jykoo/.ssh/google_rsb') 이부분에서 자꾸오류가 났었는데 키파일의 포멧이 맞지 않아서였음
- google_compute_engine 의 키파일을 복사해 google_rsb 만들고 포멧을 PEM으로 변경해줌

```java
war {
	    archiveName('aml.war')
	}

remotes {
	was1 {
			role('was')
			host = '34.64.223.72'
			user = 'jykoo'
			port = 22
			identity = file('/Users/jykoo/.ssh/google_rsb') //키파일 위치
			knownHosts = allowAnyHosts
		}
	}

 task deploy(dependsOn: war) {
        doLast {
            ssh.runInOrder {
                session(remotes.was1) {
                    put from: war.archivePath, into: "."
                    execute 'mv aml.war /usr/local/tomcat8-scheduler/webapps/aml/aml.war'
                    execute '/usr/local/tomcat8-scheduler/bin/shutdown.sh'
                    execute '/usr/local/tomcat8-scheduler/bin/startup.sh'
                }
            }
        }
    }
```

아래를 참고함

이것은 jsch의 문제입니다. 이것이 jsch 0.1.55에서 수정되었는지는 모르겠지만, 그렇지 않을 것 같지만 누군가 수정해 주었으면 합니다!

최신 버전의 OpenSSH(7.8 이상)는 기본적으로 다음으로 시작하는 새로운 OpenSSH 형식으로 키를 생성합니다.

----BEGIN OPENSSH 개인 키-----

JSch는 이 키 형식을 지원하지 않습니다.

ssh-keygen을 사용하여 키를 클래식 OpenSSH 형식으로 변환할 수 있습니다.

**ssh-keygen -p -f file -m pem -P passphrase -N passphrase**(키가 암호로 암호화되지 않은 경우 암호 대신 "" 사용)

Windows 사용자의 경우: ssh-keygen.exe는 이제 Windows 10에 내장되어 있습니다. 또한 이전 버전의 Windows용 Microsoft Win32-OpenSSH 프로젝트에서 다운로드할 수 있습니다.

Windows에서는 PuTTYgen(PuTTY 패키지에서)을 사용할 수도 있습니다.

PuTTYgen 시작키 로드변환 > OpenSSH 키 내보내기로 이동합니다.RSA 키의 경우 클래식 형식을 사용합니다.ssh-keygen을 사용하여 새 키를 생성하는 경우 -m PEM을 추가하여 클래식 형식으로 새 키를 생성합니다.

ssh-keygen -m PEM

- 내 경우 비번은 설정하지 않음 (비운채로 엔터치면됨)

```java
ssh-keygen -p -f google_rsb -m pem
```

Refo

[https://github.com/int128/gradle-ssh-plugin/issues/361](https://github.com/int128/gradle-ssh-plugin/issues/361)