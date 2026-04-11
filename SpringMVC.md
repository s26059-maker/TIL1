# Spring MVC
### MVC = 기본 이론<br>
### Spring MVC = 실제 Spring 구조<br>
**Model을 그냥 안 쓰고 더 잘게 나눔.**

## MVC
MVC 패턴은 애플리케이션을 개발할 때 사용하는 디자인 패턴이다.<br>
애플리케이션의 개발 영역을 <strong>MVC(Model, View, Controller)</strong>로 구분하여 각 역할에 <br>맞게 코드를 작성하는 개발 방식이다.

### Model(모델)
Spring MVC 기반의 웹 애플리케이션이 클라이언트의 요청을 전달받으면<br> 요청 사항을 처리하기 위한 작업을 한다.<br>
처리한 작업의 결과 데이터를 클라이언트에게 응답을 돌려주어야 하는데,<br> 이때 클라이언트에게 응답으로 돌려주는 작업의 처리 결과 데이터를 **Model**이라 한다.

### View(뷰)

**View**는 Model을 이용하여 웹 브라우저와 같은 애플리케이션의 화면에 보이는 리소스(Resource)를 제공하는 역할을 한다.<br>

 
Spring MVC에는 다양한 View 기술이 포함되어 있다.

* HTML 페이지 출력
* PDF, Excel 등의 문서 형태로 출력
* XML, JSON 등 특정 형식의 포맷으로 변환

### Controller(컨트롤러)

컨트롤러는 클라이언트 측의 요청을 직접적으로 전달받는 엔드포인트(Endpoint)로써 Model과 View의 중간에서 상호작용을 해주는 역할을 한다.<br>

 
클라이언트 측의 요청을 전달받아 비즈니스 로직을 거친 후, Model 데이터가 만들어지면, 이 Model 데이터를 View로 전달하는 역할을 한다.


### 전체적 흐름

사용자<br>
 ↓<br>
Controller (요청 받음)<br>
 ↓<br>
Model (데이터 준비)<br>
 ↓<br>
View (화면 보여줌)<br>
 ↓<br>
사용자

## Spring MVC
**Spring Framework**에서 웹사이트를 만들 때 쓰는 **구조(방법)**

사용자<br>
 ↓<br>
Controller<br>
 ↓<br>
Service<br>
 ↓<br>
Repository<br>
 ↓<br>
Database<br>
 ↑<br>
Model (DTO / Entity)<br>
 ↓<br>
View<br>
 ↓<br>
사용자<br>

**위에 Service, DTO, Entity 같은건 다음편에 낋여 올게요**
