# API란?
### Application Programming Interface의 약자

쉽게 말하면 프로그램들끼리 서로 대화할 수 있게 해주는 약속된 창구라고 할 수 있다

API의 `핵심은` 내부가 어떻게 돌아가는지 몰라도 정해진 방식대로 요청하면 원하는 결과를 받을 수 있다 는 점이다
<br><br><br>

# API 명세서
### 뜻 : API 명세서는 API의 약속을 문서로 정리해놓은 설명서다

### 쓰는 이유
- 다른 개발자가 쓸 수 있게 하려고

- 말로 일일이 설명할 수 없으니까

- 나중에 나도 까먹으니까

- 팀원들과 협업하려고

- 실수를 줄이려고

- 버전 관리를 위해

### API 명세서 작성법

`기본 구성 요소`

* API 개요
    * 이 API가 뭘 하는 건지 한 줄 설명

* Endpoint (엔드포인트)
    * 요청을 보낼 주소(URL)

* HTTP Method (메서드)
    * GET (조회), 
    * POST (생성), 
    * PUT/PATCH (수정), 
    * DELETE (삭제)

* Request (요청)
    * Headers: 인증 정보 등
    * Parameters: URL에 붙는 값
    * Body: 보낼 데이터

* Response (응답)
    * 성공했을 때 어떤 데이터가 오는지
    * 실패했을 때 어떤 에러가 오는지

* Example (예시)
    * 실제 요청/응답 예시

### 작성 에시

### 설명
회원가입

### Endpoint
` GET /api/users/{userId} `

### HTTP Method

` POST `

### Request

<pre>
body:{
"name": "String",
"email": "String",
"password": "String",
"verify password": "String",
"verifiedToken" : "String"
"department": "소프트웨어개발과",
"grade": "10기"
}</pre>


### Response

<pre> {}
</pre>

### HTTP Status code

**201 Created -** 회원가입 성공

**400 Bad Request -** 입력값 형식 오류 (이메일 형식 틀림 등)

**409 Conflict -** 이미 존재하는 아이디
