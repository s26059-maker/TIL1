# DI, IoC, PSA, AOP

## IoC (제어의 역전)

뜻 : 내가 하던 걸 Spring이 대신 한다

### 예)

기존에는 내가 직접 객체 생성함

<pre>UserService userService = new UserService();</pre>

Spring 방식은
<pre>@Autowired
UserService userService;</pre>

내가 만들던 걸 Spring이 대신 관리해줌


## DI (의존성 주입)

뜻 : 객체가 필요로 하는 다른 객체를 외부에서 넣어주는 것

###  예)
 기존
 <pre>class A {
    B b = new B(); 
}</pre>
위와 같이 직접 만듬 --> 삐삐 
- B를 바꾸면 A도 같이 수정해야 함
- 결합도가 높아짐

Spring 방식

<pre>class A {
    @Autowired
    B b;
}</pre>
B를 다른 걸로 바꿔도 A는 그대로 사용 가능


## AOP (관점 지향 프로그래밍)
 뜻 : 공통 기능을 따로 빼서 관리하는 것

### 설명

모든 메서드마다 이런 게 들어간다고 해보자
- 로그 찍기
- 시간 측정
- 권한 체크
<pre>메서드1 → 로그
메서드2 → 로그
메서드3 → 로그
</pre>

이러면 코드가 더러움<br>
<strong>but</strong><br>
AOP를 적용하면
공통 기능을 따로 빼버림

그게 뭔 뜻이냐

### 예시)

너는 지금 햄버거 만드는 직원이라 가정했을 때<br>
햄버거를 만드는건 <strong> 핵심 기능 </strong>

그런데 

- 주문 기록 남겨라 (로그)
- 위생 체크해라 (보안)
- 시간 측정해라 (성능)

같은걸 직접 계속 해야함

거기서 AOP 방식을 쓰면<br>
<strong>너는 그냥 햄버거만 만듦</strong> 나머지는 누가 하냐?<br>
---> 자동으로 처리됨














 ## PSA (기술 추상화)

뜻 : 기술을 바꿔도 코드 수정이 거의 없게 해주는 것

### 예)

기존에 

DB: MySQL 쓰다가 → Oracle로 바꾸면

보통은 코드 다 수정해야 함

<strong>but</strong>  PSA 적용<br>
---> Spring이 중간에서 추상화해줌<br>
그 말은
기술이 바뀌어도,
Spring만 바뀌고 내 코드는 그대로 
