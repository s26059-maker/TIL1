# SOLID?
### `SOLID` 원칙은 코드 구조를 유지보수 쉽고, 확장 가능하게 만들기 위한<br> 5가지 핵심 규칙이다.

## S : (SRP, Single Responsibility Principle)
 영어 번역 그대로 <strong>단일 책임 원칙<strong>이라는 뜻

 ### 단일 책임 원칙?
 <pre>class User {
    void saveUser() { }
    void sendEmail() { }
}</pre>

이런건 클래스 하나에 여러가지 기능이 있는데 이런건? 삐삐 틀림

<pre>class User { }

class UserRepository {
    void saveUser(User user) { }
}

class EmailService {
    void sendEmail(User user) { }
}</pre>

이렇게 클래스는 하나의 책임만 가져야 한다~

## 왜?
여러 기능이 하나의 클래스에 있으면 고치기 어려움
<br><br><br>

## O : (OCP, Open-Closed Principle) - 개방-폐쇄 원칙
확장에는 열려 있고, 수정에는 닫혀 있어야 한다라는 뜻<br>
ㅣ<br>
 -----> 기존 코드는 건드리지 말고 기능만 추가하라는 뜻이다.

 ### 예)

 <pre>class Discount {
    int getDiscount(String type) {
        if(type.equals("VIP")) return 1000;
        if(type.equals("NORMAL")) return 500;
    }
}</pre>
이건 새 타입 추가하면 계속 수정해야 함 but

<pre>interface Discount {
    int getDiscount();
}

class VIPDiscount implements Discount {
    public int getDiscount() { return 1000; }
}

class NormalDiscount implements Discount {
    public int getDiscount() { return 500; }
}</pre>
이렇게 하면 새로운 클래스 추가만 하면 됨 (기존 코드 수정 X)
<br><br><br>

## L : (LSP, Liskov Substitution Principle) - 리스코프 치환 원칙
뜻 : 부모 클래스 대신 자식 클래스를 넣어도 문제 없어야 한다

### 예)

<pre>class Bird {
    void fly() { }
}

class Penguin extends Bird {
    void fly() { throw new Error("못 날아"); }
}
</pre>
펭귄은 날 수 없는데 fly를 강제로 썼음

<pre>class Bird { }

class FlyingBird extends Bird {
    void fly() { }
}

class Penguin extends Bird { }</pre>

이렇게 상속은 진짜 같은 성질일 때만 써야 함

### 정리

- 부모(새 클래스): 새는 난다 → 자식도 반드시 날아야함<br>

새는 난다라고 했으면 날아야 하는데 팽귄은 못나니까 `날 수 있는 새` 와 `날 수없는 새` 로<br> 따로 부모님을 만들어야 함
<br><br><br>

## I : (ISP, Interface Segregation Principle) - 인터페이스 분리 원칙

뜻 : 필요 없는 기능을 강제로 구현하게 하지 말자

### 예)
<pre>interface Machine {
    void print();
    void scan();
    void fax();
}</pre>

기계라는 인터페이스에 기능이 다 들어있음<br>
그런데 어떤 클래스는 일부 기능만 필요하기 때문에 <br>
print만 구현하면? ---> 삐삐 컴파일 에러 발생!

<pre>interface Printer {
    void print();
}

interface Scanner {
    void scan();
}</pre>
이렇게 인터페이스를 쪼개서 필요한 것만 구현 ---> 딩동댕 
- 의미 없는 코드 생기지 않고
- 나중에 유지보수가 수월함

<br><br><br>

## D : (DIP, Dependency Inversion Principle) - 의존 역전 원칙

뜻 : 구체적인 것 말고, 추상적인 것에 의존하라

### 예)

DIP를 안 쓰면 
<pre>class MySQLDatabase {
    void connect() {
        System.out.println("MySQL 연결");
    }
}

class UserService {
    MySQLDatabase db = new MySQLDatabase();
}</pre>
와 같이 UserService가 MySQL에 딱 고정됨<br>
<strong>So</strong> 여러가지 클래스에서 db를 바꿀 때 하나하나 바꿔줘야함

<strong>하지만<strong> , DIP를 적용하면?
<pre>interface Database {
    void connect();
}</pre>
<pre>class UserService {
    Database db;

    UserService(Database db) {
        this.db = db;
    }
}</pre>

이게 대충 DB 종류 몰라도 그냥 Database면 된다는 뜻<br>
그래서 <pre>UserService service = new UserService(new MySQLDatabase());</pre>
이걸 <pre>UserService service = new UserService(new OracleDatabase());</pre>
이런 식으로만 바꿔 끼우면 된다는 뜻임


### 정리

살짝 코딩에서 쓰는 변수랑 비슷하게 변수 안에 값만 바꾸면 그 변수를 쓰고 있는 모든 것의 값이 바뀌는 것처럼 DIP 사용도 변수와 비슷하게 클래스가 모든 데이터베이스(예)를 가리키고 그 데이터베이스 종류가 뭔지 넣는 것에 따라 클래스의 데이터베이스가 달라지니까 변수라고 생각하면 쉽다.