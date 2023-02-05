# 환경설정

### MAC JAVA\_HOME 설정

자바 1.8, 자바 11 설치후

자바 1.8이 필요할때 아래 방법으로 변경함

1. 사용자 home 위치로 이동
2. `vim .bash_profile` 로 생성
3. 아래와 같이 입력

```python
export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_311.jdk/Contents/Home
export PATH=${PATH}:$JAVA_HOME/bin
```

4. `source ./bash_profile` 로 환경설정 적용가능함



### M1 JAVA\_HOME 설정

* homebrew 로 설치하는데 JavaVirtualMachines 경로에 jdk 가 존재하지 않았음
* 다시 재설치하고 뜬 문구를 읽어봄

For the system Java wrappers to find this JDK, symlink it with sudo ln -sfn /opt/homebrew/opt/openjdk@11/libexec/openjdk.jdk /Library/Java/JavaVirtualMachines/openjdk-11.jdk

openjdk@11 is keg-only, which means it was not symlinked into /opt/homebrew, because this is an alternate version of another formula.

If you need to have openjdk@11 first in your PATH, run: echo 'export PATH="/opt/homebrew/opt/openjdk@11/bin:$PATH"' >> \~/.zshrc

For compilers to find openjdk@11 you may need to set: export CPPFLAGS="-I/opt/homebrew/opt/openjdk@11/include"

* 위에 보면 심볼릭 링크 하라고 되어있음
* m1에서는 위의 경로로 jdk가 생성된다.
