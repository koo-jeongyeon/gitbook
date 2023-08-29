# 정규표현식을 사용해 대소문자 구분없이 검색

쿼리를 아래처럼 web을 대소문자 구문없이 검색하고 싶을때 이렇게 짜면됨

```jsx
db.myCollection.find({'sitename': {'$regex': 'web', '$options': 'i' }})
```

스프링 코드에선 Document에 Document 객체를 넣는 방식으로 객체를 반환해 find로 검색함

```java
public Document getAliveAvatarDocument(String avatarId) {
        var regex = new Document("$regex", avatarId);
        regex.put("$options","i");
        var doc = new Document("avatar_id", regex);
        doc.put("deleted", null);

        return doc;
    }
```

[MongoDB에서 대소문자 구분하지 않고 정규표현식 찾는 방법](https://webisfree.com/2017-08-10/mongodb%EC%97%90%EC%84%9C-%EB%8C%80%EC%86%8C%EB%AC%B8%EC%9E%90-%EA%B5%AC%EB%B6%84%ED%95%98%EC%A7%80-%EC%95%8A%EA%B3%A0-%EC%A0%95%EA%B7%9C%ED%91%9C%ED%98%84%EC%8B%9D-%EC%B0%BE%EB%8A%94-%EB%B0%A9%EB%B2%95)
