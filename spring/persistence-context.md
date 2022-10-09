# 영속성 컨텍스트

영속성 컨텍스트란 **엔티티를 영구 저장하는 환경**이라는 뜻이다<br>
어플리케이션과 데이터베이스 사이에서 객체를 보관하는 **가상의 데이터베이스** 같은 역할을 한다

### 엔티티의 생명주기

- 비영속<br>
  영속성 컨텍스트와 아무런 관계가 없는 상태

```java
Member member = new Member(1, "사람");
```

- 영속<br>
  엔티티가 영속성 컨텍스트에 저장되어 관리되고 있는 상태

```java
em.persist(member)
```

- 준영속<br>
  영속성 컨텍스트에서 관리되다 분리된 상태<br>
  아래 3가지 방법을 통해 준영속 상태를 만들 수 있다

```java
// 엔티티를 영속성 컨택스트에서 분리
em.detach(entity);
// 영속성 컨텍스트를 비우기
em.clear();
// 영속성 컨택스트를 종료
em.close();
```

- 삭제<br>
  말 그대로 삭제된 상태

### 장점

- 1차 캐시<br>
  엔티티 매니저가 조회를 할 때 1차 캐시에서 해당 엔티티를 찾고 존재할 경우 DB에 접근하지 않고 반환한다<br>
  엔티티가 존재하지 않는 경우에는 DB에 접근해서 엔티티를 찾아오고 해당 엔티티는 1차 캐시에 저장한다

- 동일성 보장<br>
  동일성은 값 뿐만 아니라 실제 인스턴스 자체가 같다는 뜻이다<br>
  아래와 같이 2번 조회한 결과 `member1`과 `member2`가 동일하다는 것을 알 수 있다

```java
Member member1 = entityManager.find(Member.class, "1L");
Member member2 = entityManager.find(Member.class, "1L");
System.out.print(member1 == member2) // Result: true
```

- 트랜잭션을 지원하는 쓰기 지연<br>
  `persit`가 일어날 때 엔티티들은 1차 캐시에 저장하고 **쓰기 지연 SQL 저장소** 라는 곳에 INSERT 쿼리들을 쌓아놓는다. commit또는 flush를 할 때 **쓰기 지연 SQL 저장소**에 저장되어 있는 SQL들을 DB에 보낸다

- 변경감지<br>
  엔티티 매니저가 엔티티를 1차 캐시에 저장할 때 스냅샷도 같이 저장되는데 커밋 시점에 엔티티와 스냅샷을 비교해 변경사항이 있으면 update 쿼리를 알아서 생성해 쓰기 지연 SQL 저장소에 저장한다

- 플러시<br>
  영속성 컨텍스트의 변경내용을 데이터베이스에 반영한다

  #### 영속성 컨텍스트를 플러시하는 방법

  1. em.flush()
  2. 트랜잭션 커밋
  3. jpql 쿼리 실행