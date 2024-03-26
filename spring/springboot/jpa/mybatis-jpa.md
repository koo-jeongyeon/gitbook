# Mybatis / JPA 차이

JPA

* 페이징하기 쉬운 라이브러리가 있음 Pageable?
* CRUD 쿼리 안짜도됨 간편함 - 레파지토리 구현 간편
  * 복잡한 쿼리도 자바코드로 짤수있는 라이브러리도 있음

<pre><code><strong>//이런식으로 JpaRepository 상속받음
</strong><strong>public interface BoardContentRepository extends JpaRepository&#x3C;BoardContentEntity, Long>
</strong></code></pre>

<pre><code><strong>//CrudRepository 상속, JpaRepository랑 무슨 차이지?
</strong>//@Query 로 간단하게 mybatis 마냥 쿼리도 짤수있음
public interface WpUserRepository extends CrudRepository&#x3C;WpUserEntity, Long> {

@Query(nativeQuery = true,value = USER_QUERY
			+ " from wp_users a where user_login = :email or user_email = :email")
	public WpUserEntity findByEmail(@Param("email") String email);
</code></pre>

* 테이블을 객체로 만들기위해 중요한 클래스 Entity 임
  * 테이블 컬럼 그대로 구현해야됨
  * 보통 repository.entity 아래에 만듬
  * 다대다 일대다 이런 관계형 데이터베이스 관계를 구현해줄수있음 어노테이션으로&#x20;

```
//WpUserEntity안에 구현된 WpUserMetaEntity (일대다)
//JoinColumn 은 조인하는 컬럼 WpUserMetaEntity의 컬럼을 적어야함
@OneToMany(cascade = CascadeType.ALL, orphanRemoval = true)
@JoinColumn(name = "user_id")
private List<WpUserMetaEntity> wpUserMetaEntitys = new ArrayList<>();

```
