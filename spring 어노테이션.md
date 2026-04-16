# 어노테이션
## 짧고 굵게 의미 정리
### 코드 위에 붙이는 “표식”
예) `@Controller`
### 쉽게 말하면..!
이 코드는 이런 역할이야! 라고 알려주는 표식

## 이유
왜 쓸까? 어노테이션을? 바로바로<br>

Spring이 이걸 보고 이렇게 생각함 

- @Controller : 요청 처리
- @Service : 로직 처리
- @Repository : DB 담당

  그래서 자동으로 처리해줌

  예)
 <pre> @Service
public class UserService {
}
</pre>


Spring이 자동으로:

- 객체 생성하고
- 저장하고
- 필요하면 가져다 씀

### 없다? = 직접 만들어야함 = 귀찮음

