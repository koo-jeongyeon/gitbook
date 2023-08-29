# GROUP BY 정리

* GROUP BY 정리
* 특정컬럼을 그룹화 합니다.

조건처리\[WHERE] / 컬럼그룹화\[GROUP BY] / 그룹화한것조건처리\[HAVING] / 정렬\[ORDER BY]

```
SELECT 컬럼 FROM 테이블 [WHERE 조건식]
GROUP BY 그룹화할 컬럼 [HAVING 조건식] ORDER BY 컬럼1 [, 컬럼2, 컬럼3 ...];
```

예제 테이블 : INVEST

| 상품번호 | 구매자   | 구매금액 |
| ---- | ----- | ---- |
| R01  | kang  | 1000 |
| R02  | kang  | 2000 |
| R02  | Jeong | 1000 |
| R03  | Jeong | 3000 |
| R03  | Pan   | 2000 |

```
SELECT 상품번호,구매자,구매금액,COUNT(구매자) AS CNT FROM INVEST GROUP BY 구매자
```

결과 테이블

| 상품번호 | 구매자   | 구매금액 | CNT |
| ---- | ----- | ---- | --- |
| R01  | kang  | 1000 | 2   |
| R02  | Jeong | 1000 | 2   |
| R03  | Pan   | 2000 | 1   |

* 구매자로 그룹지어서 테이블이 생성된걸 확인할수있음
* 구매자와 집계함수(CNT) 외에는 맨 최상위 컬럼이 결과값으로 나옴 → ORDER BY로 내림차순이든 오름차순이든 해서 그룹지어도 반영안됨

***

**GROUP BY후 집계함수가 아닌 컬럼의 최신값을 가져오는법**

* MAX 를 사용할수있는 컬럼이 있어야함. 날짜나 IDX(인덱스)
* MAX 와 WHERE IN 활용
* 예제 테이블의 상품번호 앞에 IDX 가 있다고 가정하면

```
SELECT
	*
FROM INVEST
WHERE(IDX,구매자)
in (
		SELECT 
		MAX(IDX) AS IDX,
		구매자
		FROM INVEST GROUP BY 구매자
)
```

결과 테이블

| IDX | 상품번호 | 구매자   | 구매금액 |
| --- | ---- | ----- | ---- |
| 2   | R02  | kang  | 2000 |
| 4   | R03  | Jeong | 3000 |
| 5   | R03  | Pan   | 2000 |

**Refer**

[https://extbrain.tistory.com/56](https://extbrain.tistory.com/56)
