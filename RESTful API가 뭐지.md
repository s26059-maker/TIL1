# RESTful API가 뭐지?

## 의미
RESTful API는 **REST(Representational State Transfer) 원칙을 따르는 웹 API 설계 방식**이다.  
HTTP 프로토콜을 사용해 **클라이언트와 서버가 자원(Resource)을 주고받도록 하는 인터페이스**를 의미한다.

## 표현 방식
REST에서는 모든 데이터를 **자원(Resource)** 으로 보고 URI로 표현하며, 자원에 대한 동작은 **HTTP Method**로 표현한다.  
예를 들어 `/users`, `/users/1` 같은 URI로 자원을 식별하고 `GET`, `POST`, `PUT(PATCH)`, `DELETE` 같은 메서드로 조회·생성·수정·삭제를 수행한다.

예시:<br><br>
GET /users<br>
GET /users/1<br>
POST /users<br>
DELETE /users/1<br>


## 특징
RESTful API는 **Stateless 구조(서버가 클라이언트 상태를 저장하지 않음)**, **클라이언트-서버 구조 분리**, **일관된 URI와 HTTP Method 사용** 등의 특징을 가진다.

## 요약 정리
정리하면 RESTful API는 **URI로 자원을 표현하고 HTTP Method로 행위를 정의하는 API 설계 방식**이다.