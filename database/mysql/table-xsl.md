# 테이블 명세서 빠르게 생성

```jsx
SELECT
    a.TABLE_NAME '테이블명',
    b.ORDINAL_POSITION '순번',
    b.COLUMN_NAME '필드명',
    b.DATA_TYPE 'DATA TYPE',
    b.COLUMN_TYPE '데이터길이',
    b.COLUMN_KEY 'KEY',
    b.IS_NULLABLE 'NULL값여부',
    b.EXTRA '자동여부',
    b.COLUMN_DEFAULT '디폴트값',
    b.COLUMN_COMMENT '필드설명'
FROM information_schema.TABLES a
JOIN information_schema.COLUMNS b ON a.TABLE_NAME = b.TABLE_NAME AND a.TABLE_SCHEMA = b.TABLE_SCHEMA
WHERE a.TABLE_SCHEMA = 'DB_NAME'
ORDER BY
    a.TABLE_NAME, b.ORDINAL_POSITION
```

파일 > 기본설정 > 리본 및 도구 모음 에서 개발도구 오픈함

엑셀 메크로

[https://ttend.tistory.com/595](https://ttend.tistory.com/595)

목차만들기 메크로

[https://stat-and-news-by-daragon9.tistory.com/241](https://stat-and-news-by-daragon9.tistory.com/241)
