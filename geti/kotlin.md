## kotlin을 사용하는 이유

간결하고 표현력이 뛰어나 개발자의 생산성을 높혀주고, 자바와 100%호환되어 상호 운용성을 갖기 때문에 

## Java와 차이점

- 코드가 간결해짐(세미콜론 불필요, 타입 추론 기능 등)

## data class

데이터 클래스는 데이터 저장용 클래스로, `data class User(val name: String, val age: Int)` 이렇게 선언하면 toString, equals, hashCode, copy 같은 걸 컴파일러가 자동으로 만들어준다.

## object

인스턴스가 딱 하나만 존재하는 싱글톤 객체를 만드는 키워드

## null safety

null safety는 null이 들어갈 수 있는 타입(?)과 아닌 타입을 구분해서, NPE를 실행 중이 아니라 컴파일 때 미리 잡는 시스템

## scope function

스코프 펑션은 객체 이름을 반복하지 않게 중괄호 안은 이 객체 전용 공간을 열어주는 함수로, let/run/with/apply/also 5개가 있다

| 함수 | 객체 참조 | 주 용도 | 반환값 |
| --- | --- | --- | --- |
| let | it | null 체크 후 실행 | 람다 결과 |
| run | this | 객체 설정, 결과 계산 | 람다 결과 |
| with | this | 객체의 여러 멤버 접근 | 람다 결과 |
| apply | this | 객체 초기화/설정 | 객체 자신 |
| also | it | 부수 작업(로깅 등) | 객체 자신 |

## collection API

리스트 같은 데이터 뭉치를 for문 없이 한 줄로 가공하는 함수들

| 함수 | 하는 일 |
| --- | --- |
| `filter { }` | 조건 맞는 것만 골라냄 |
| `map { }` | 전부 다른 형태로 변환 |
| `forEach { }` | 하나씩 꺼내서 실행 (for문 대체) |
| `first()` / `firstOrNull()` | 첫 요소 꺼내기 (없으면 에러 vs null) |
| `find { }` | 조건 맞는 첫 요소 (없으면 null) |

## extension functions

남의 클래스에 내 함수를 밖에서 추가하는 기능

## coroutine 개념

스레드보다 훨씬 가벼운 비동기 작업 단위로, `suspend` 함수가 대기할 일(API 호출 등)이 생기면 그 자리서 일시정지하고 스레드는 다른 일을 하러 갔다가 결과가 오면 재개되는 방식

## sealed class

자식 클래스의 종류를 제한하는 클래스

예)

```jsx
sealed class Result {
data class Success(val data: String) : Result()
data class Error(val message: String) : Result()
object Loading : Result()
}
```
