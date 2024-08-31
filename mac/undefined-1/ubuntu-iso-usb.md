# ubuntu iso 설치 usb 만들기

우분투 [https://parkaparka.tistory.com/31](https://parkaparka.tistory.com/31)

우분투 서버 [https://ubuntu.com/download/server](https://ubuntu.com/download/server)

* 우분투 iso 파일 다운받기

[Download Ubuntu Desktop | Download | Ubuntu](https://ubuntu.com/download/desktop)

* 다운된 경로 가서 명령어 실행

```java
hdiutil convert -format UDRW -o ubuntu.iso [다운된파일]
```

* 파일명 변경

```java
mv ubuntu.iso.dmg ubuntu.iso
```

* diskutil list 명령어 사용해서 usb가 몇번 디스크인지 찾기
  * usb 없이 디스크 확인하고 usb 꽃고 몇번 디스크가 생겼는지 보면됨
  * 내경우 disk2임
* usb 디스크 언마운트 시켜주기

```java
diskutil unmountDisk /dev/disk2
```

* 다음 실행

```java
sudo dd if=./ubuntu.iso of=/dev/disk2 bs=1m
```

* 추출

```java
diskutil eject /dev/disk2
```
