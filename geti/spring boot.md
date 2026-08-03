## Spring Boot란?

**Spring Boot** 는 웹 애플리케이션을 쉽고 빠르게 만들 수 있도록 도와주는 자바의 웹 프레임워크이다. 

## IOC

뜻 : 내가 하던 걸 Spring이 대신 한다

### **예)**

기존에는 내가 직접 객체 생성함

```
UserService userService = new UserService();
```

Spring 방식은

```
@Autowired
UserService userService;
```

내가 만들던 걸 Spring이 대신 관리해줌

## DI

뜻 : 객체가 필요로 하는 다른 객체를 외부에서 넣어주는 것

### **예)**

기존

```
class A {
    B b = new B();
}
```

위와 같이 직접 만듬 

- B를 바꾸면 A도 같이 수정해야 함
- 결합도가 높아짐

Spring 방식

```
class A {
    @Autowired
    B b;
}
```

B를 다른 걸로 바꿔도 A는 그대로 사용 가능

## Bean

스프링 컨테이너가 관리하는 자바 객체를 뜻한다

## Component

재사용이 가능한 각각의 독립된 모듈

앱 켜질 때 스프링이 빈으로 만들어 컨테이너에 보관한

## Controller

사용자 요청을 받는 곳이다.(제일 먼저 받음)

## Service

프로그램의 실제 로직을 처리하는 곳이다

Controller와 Repository 사이에서 기능 처리

## Repository

DB와 직접 통신하는 곳이다

프로그램에서

> 데이터 조회
> 

> 데이터 저장
> 

> 데이터 수정
> 

> 데이터 삭제
> 

같은 일을 담당함.

## Configuration

@Bean 메서드로 외부 라이브러리 클래스 등을 수동으로 빈 등록하는 설정 클래스 표시

## application.yml

스프링 앱의 설정값 적어두는 파일

## Profile

개발/운영 같은 환경별로 설정 파일 나눠두고 골라서 켜는 기능

## Auto Configuration

의존성(라이브러리)을 보고 스프링 부트가 필요한 빈 설정을 자동으로 등록해주는 기능

내가 같은 빈을 직접 등록하면 내 빈이 우선

## Bean Lifecycle

빈이 생성 → 주입 → 초기화(@PostConstruct) → 사용 → 소멸(@PreDestroy) 순서로 관리되는 생애주기