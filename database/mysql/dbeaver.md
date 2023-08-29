# dbeaver 데이터 내보내기 불러오기

### 데이터 내보내기

* 데이터 추출 클릭

![](<../../.gitbook/assets/스크린샷 2023-08-29 오후 10.40.54.png>)

* 본인은 CSV, SQL 로 내보내기 실행함

<figure><img src="../../.gitbook/assets/스크린샷 2023-08-29 오후 10.42.37.png" alt=""><figcaption></figcaption></figure>

* SQL 내보내기
  * include generated columns 에 체크하면 PK까지 포함하여 쿼리 생성됨

<figure><img src="../../.gitbook/assets/스크린샷 2023-08-29 오후 10.49.12.png" alt=""><figcaption></figcaption></figure>

### 데이터 가져오기

* 데이터 가져오기 클릭

![](<../../.gitbook/assets/스크린샷 2023-08-29 오후 10.50.55.png>)

* 테이블 및 CSV만 가져오기 가능

<figure><img src="../../.gitbook/assets/스크린샷 2023-08-29 오후 10.51.43.png" alt=""><figcaption></figcaption></figure>

### 내보내기 도중 java heap space 부족 뜰때

1. 터미널을 열고 해당 경로로 이동합니다.\
   `cd /Applications/DBeaverEE.app/Contents/Eclipse`
2. vi로 아래 파일을 수정합니다.\
   `vi dbeaver.ini`
3. 해당 부분을 수정합니다.\
   `-Xms128m  >  -Xms512m`\
   `-Xmx2048m  >  -Xmx4096m`



참고

{% embed url="https://codingdog.tistory.com/entry/dbeaver-%ED%85%8C%EC%9D%B4%EB%B8%94-%EB%82%B4%EB%B3%B4%EB%82%B4%EA%B8%B0-%EB%B6%88%EB%9F%AC%EC%98%A4%EA%B8%B0-%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95%EC%9D%84-%EC%95%8C%EC%95%84%EB%B4%85%EC%8B%9C%EB%8B%A4" %}

{% embed url="https://dbeaver.com/docs/dbeaver/Data-Import-and-Replace/" %}

{% embed url="https://rastalion.me/dbeaver%EC%97%90%EC%84%9C-java-heap-space-%EB%B6%80%EC%A1%B1%EC%9D%B4%EB%9D%BC%EA%B3%A0-%EB%82%98%EC%98%AC%EB%95%8C/" %}
