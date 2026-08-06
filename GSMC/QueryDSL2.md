## QueryDSL

JPQL(=JPA에서 쓰는 쿼리 언어)을 문자열로 직접 쓰면 오타가 나도 실행 전까지 모른다. QueryDSL은 이 JPQL을 자바 코드로 짜게 해주는 라이브러리라서, 코드 짜는 중에 바로 오류를 잡을 수 있고, 조건 붙었다 빠졌다 하는 동적 쿼리도 훨씬 깔끔하게 짤 수 있다.

```java
queryFactory
    .selectFrom(member)
    .where(member.name.eq("회원1"))
    .orderBy(member.name.desc())
    .fetch();
```

여기서 `member`는 엔티티 자체가 아니라 Q타입 객체(예: QMember)다. 컴파일할 때 엔티티 정보를 담아 자동으로 만들어지는 클래스라서, 이걸 통해 JPQL 생성에 필요한 필드 정보를 넘겨주는 거다.

## 검색 조건

where절에 조건을 여러 개 넣고 `.and()`, `.or()`로 이어붙이면 된다. `eq`, `gt`(초과), `contains`, `startsWith` 같은 메서드로 조건을 표현한다.

```java
queryFactory
    .selectFrom(item)
    .where(item.name.eq("좋은상품").and(item.price.gt(20000)))
    .fetch();
```

콤마로 조건을 나열해도 자동으로 and 처리된다. 그래서 실무에서는 조건마다 메서드로 빼서 콤마로 나열하는 방식을 많이 쓴다(null이면 그 조건은 무시되는 특징도 있어서 동적 쿼리에 유리하다).

## 결과 조회

쿼리를 다 짜고 마지막에 호출하는 메서드가 실제로 DB를 때리는 부분이다.

- `fetchOne()`: 결과가 딱 1개일 때. 없으면 null, 2개 이상이면 예외 터짐
- `fetchFirst()`: 결과가 여러 개여도 그냥 첫 번째 것만 가져옴
- `fetch()`: 결과를 리스트로 받음. 없으면 빈 리스트

## 페이징과 정렬

`offset`은 몇 개를 건너뛸지, `limit`은 몇 개를 가져올지를 정한다. 실무에서는 이 페이징 정보를 하나로 묶은 Pageable(몇 페이지째를, 몇 개씩, 어떤 순서로 달라는 요청 정보를 담은 객체)을 많이 쓴다.

```java
queryFactory
    .selectFrom(item)
    .where(item.price.gt(20000))
    .orderBy(item.price.desc(), item.stockQuantity.asc())
    .offset(10)
    .limit(20)
    .fetch();
```

## 그룹

`groupBy`로 묶고, 묶인 그룹 중에서 조건을 더 걸고 싶으면 `having`을 쓴다.

```java
queryFactory
    .selectFrom(item)
    .groupBy(item.price)
    .having(item.price.gt(1000))
    .fetch();
```

## 조인

`join`, `leftJoin`, `rightJoin` 다 쓸 수 있고, `on`으로 조인 조건을 추가로 걸 수 있다. 연관된 엔티티까지 한 번에 다 끌고 오고 싶으면 `fetchJoin()`을 붙인다.

```java
queryFactory
    .selectFrom(item)
    .leftJoin(item.category, category)
    .on(category.name.eq("전자제품"))
    .fetch();
```

## 서브쿼리

쿼리 안에 또 다른 질문(쿼리)을 넣는 것. 예를 들어 "가격이 제일 비싼 상품 하나"를 먼저 알아야 그 다음 조건을 걸 수 있을 때 쓴다.

```java
queryFactory
    .selectFrom(item)
    .where(item.price.eq(
        JPAExpressions.select(subItem.price.max()).from(subItem)
    ))
    .fetch();
```

## BooleanBuilder

조건을 if문으로 하나씩 만들어뒀다가, 마지막에 한 번에 where()로 던지는 조건 조립통이다. 검색 조건이 사람마다 다르게 들어올 때(이름만 검색, 나이만 검색, 둘 다 검색 등) 유용하다.

```java
BooleanBuilder builder = new BooleanBuilder();
if (name != null) builder.and(member.name.eq(name));
if (age != null) builder.and(member.age.eq(age));

queryFactory.selectFrom(member).where(builder).fetch();
```

## Projection

엔티티를 통째로 가져오지 않고, 필요한 컬럼만 골라서 바로 DTO에 담아오는 방식이다. 필요한 것만 가져오니 성능에도 유리하다.

```java
queryFactory
    .select(Projections.constructor(UserDto.class, user.name, user.email))
    .from(user)
    .fetch();
```