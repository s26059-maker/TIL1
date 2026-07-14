# ?? Controller, Service, Repository, Entity, DTO ??

## Controller?
**사용자 요청을 받는 곳이다**

**사용자가 웹에서 뭔가 요청하면 Controller가 제일 먼저 받는다.**

## Service?
**프로그램의 실제 기능(로직)을 처리하는 곳이다**
<br>- Controller와 Repository 사이에서 기능 처리

### 하는 일

- 데이터 처리<br>
- 비즈니스 로직<br>
- DTO → Entity 변환<br>
- 여러 Repository 사용<br>
- 검증

## Repository?
**Repository = DB와 직접 통신하는 곳이다**

프로그램에서

- 데이터 조회

- 데이터 저장

- 데이터 수정
<br><br>

데이터 삭제같은 일을 담당함.

## Entity?
**Entity = DB 테이블을 자바 클래스로 만든 것이다**

### 풀이
데이터베이스에 표(table) 형태로 데이터가 저장되어 있음
<br>**JAVA**는 데이터를 **객체**로 다룸<br>
즉 자바는 객체로 데이터를 표현함.
<br>그래서 DB 테이블을 자바 클래스 형태로 만들어서 연결함.<br>
그게 바로 **Entity**.

### 예시
DB 테이블

<pre>id name age
1  철수 17</pre>

이걸 JAVA에선?
<pre>Student student = new Student();

student.id = 1;
student.name = "철수";
student.age = 17;</pre>
이렇게 객체로 변함.

## DTO?
**DTO = 데이터를 전달하기 위한 객체(Data Transfer Object)이다**

### 사용 이유
데이터를 그냥 변수 하나씩 보내면 복잡함.<br>
그래서 **하나의 객체에 데이터를 묶어서 보냄.**

**예**<br>
<pre>
signup(name, email, password)</pre>

### 활용 예시)

사용자가 입력
<pre>
이름 : 철수
이메일 : chulsoo@email
비밀번호 : 1234</pre>

DTO에 클래스에선?
<pre>public class UserDTO {

    private String name;
    private String email;
    private String password;

}</pre>
이렇게 묶음, 그리고 자바에 

<pre>UserDTO dto = new UserDTO();

dto.setName("철수");
dto.setEmail("chulsoo@email");
dto.setPassword("1234");</pre>
이렇게 들어감

## Entity vs DTO ?
| | Entity    | DTO                     |
| --- | --------- | ----------------------- |
| 목적  | DB 데이터 표현 | 데이터 전달                  |
| 위치  | DB와 연결    | Controller / Service 사이 |
| 데이터 | DB 구조 그대로 | 필요한 데이터만                |
