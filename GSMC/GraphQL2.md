## 1. GraphQL이란

페이스북에서 만든 **API 쿼리 언어**. 단일 엔드포인트(`/graphql`)를 가지며, 클라이언트가 필요한 데이터를 정확히 지정해서 요청하는게 핵심 목적이다.

## 2. Over/Under-fetching

- Over-fetching : 필요한 것보다 많은 데이터가 옴 -> 안 쓰는 데이터 때문에 통신이 무거워짐
- Under-fetching : 필요한 것보다 적은 데이터가 옴 -> API를 두 번 호출해야 해서 느려짐

## 3. REST와 비교

### REST vs GraphQL

| 구분 | REST | GraphQL |
|---|---|---|
| 엔드포인트 | URL+METHOD로 여러 개 (`/library`, `/library/book/{id}`) | 단 하나 (`/graphql`) |
| 응답 데이터 | 서버가 정한 대로 옴 (DTO 다 만들어야 함) | 클라이언트가 원하는 필드만 받음 |

## 4. 기본 문법

### Query / Mutation

- **query**: 조회
- **mutation**: 생성/수정/삭제

```graphql
query {
  hero {
    name
  }
}
```
```graphql
{
  "data": {
    "hero": { "name": "R2-D2" }
  }
}
```
→ **요청한 모양 그대로** 응답이 온다. (`query` 키워드는 생략 가능)

### 필드 (Field)

원하는 필드만 나열해서 요청. 하위 객체도 중첩해서 요청 가능.

```graphql
{
  hero {
    name
    friends {
      name
    }
  }
}
```

### 인자 (Argument)

REST는 URL 파라미터 하나로 때우지만, GraphQL은 **모든 필드**가 인자를 가질 수 있다.

```graphql
{
  human(id: "1000") {
    name
    height(unit: FOOT)
  }
}
```

### 별칭 (Alias)

같은 필드를 다른 조건으로 두 번 요청할 때, JSON 키 중복을 피하려고 이름을 바꿔 붙인다.

```graphql
{
  empireHero: hero(episode: EMPIRE) { name }
  jediHero: hero(episode: JEDI) { name }
}
```

### 프래그먼트 (Fragment)

반복되는 필드 묶음을 재사용 가능한 조각으로 캡슐화.

```graphql
fragment comparisonFields on Character {
  name
  appearsIn
  friends { name }
}
```

### 변수 (Variable)

쿼리 안의 값을 하드코딩하지 않고 `$변수명`으로 분리해서 외부에서 주입.

```graphql
query HeroNameAndFriends($episode: Episode) {
  hero(episode: $episode) {
    name
  }
}
```
전달값: `{ "episode": "JEDI" }`

### 지시어 (Directive)

변수 값에 따라 쿼리 구조 자체를 동적으로 바꿈.

- `@include(if: Boolean)` : true일 때만 필드 포함
- `@skip(if: Boolean)` : true일 때 필드 건너뜀

### 인라인 프래그먼트

인터페이스/유니언처럼 **여러 타입 중 하나**가 반환될 때, 타입별로 다른 필드를 요청.

```graphql
hero(episode: $ep) {
  name
  ... on Droid { primaryFunction }
  ... on Human { height }
}
```

## 5. 스키마와 타입

**스키마 = 어떤 데이터를 주고받을 수 있는지 정의해놓은 규칙(메뉴판)**

### 객체 타입

```graphql
type Character {
  name: String!
  appearsIn: [Episode]!
}
```
- `!` = Non-nullable (null이면 에러)
- `[]` = 리스트

### 스칼라 타입

더 이상 쪼갤 수 없는 값. 기본 제공: `Int`, `Float`, `String`, `Boolean`, `ID`
- `ID`: 문자열처럼 직렬화되지만 "사람이 읽기 위한 값"이 아니라 **고유 식별자** 의미
- 커스텀도 가능: `scalar Date`

### 열거형 (Enum)

허용된 값 중 하나만 가능하도록 제한.

```graphql
enum Episode {
  NEWHOPE
  EMPIRE
  JEDI
}
```

### 인터페이스

여러 타입이 공통으로 가져야 할 필드를 강제하는 추상 타입.

```graphql
interface Character {
  id: ID!
  name: String!
}
type Human implements Character { ... }
type Droid implements Character { ... }
```
→ 공통 필드 외에 타입별 고유 필드를 조회하려면 **인라인 프래그먼트** 필요.

### 입력 타입 (Input)

뮤테이션에서 복잡한 객체를 인자로 넘길 때 사용.

```graphql
input ReviewInput {
  stars: Int!
  commentary: String
}
```

## 6. Resolver — 실제로 데이터를 가져오는 함수

**Query 타입(Root)** = 진입점 목록

```graphql
type Query {
  hero(id: ID!): Hero
  users: [User]
}
```

- **기본 Resolver**: `obj.필드명`을 자동으로 반환
- **커스텀 Resolver**: DB 조회 등 비동기 로직 수행 가능

```js
human(obj, args, context) {
  return context.db.loadHumanByID(args.id)
    .then(userData => new Human(userData));
}
```
> 모든 resolver가 끝난 후 **최종 응답을 한 번에** 반환한다.

## 7. 사용 시 주의사항

| 위험 | 원인 | 대응 |
|---|---|---|
| 서버 과부하 | 복잡하거나 깊게 중첩된 쿼리 | 쿼리 깊이/복잡도 제한, 쿼리 분석 도구 사용 |
| 정보 유출 | 제한 없는 필드 접근 | 필드 단위 접근 제어, 사용자 권한 검증 |
| DoS 공격 | 악의적인 대량 쿼리 요청 | IP/계정별 요청 수 제한, 트래픽 모니터링 |
