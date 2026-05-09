# security로 인증과 인가 구현

## 인증

 회원가입으로 사용자 정보를 DB에 저장

![alt text](image-3.png)

그럼 로그인 창이 뜨는데

![alt text](<스크린샷 2026-05-10 020109.png>)

 여기서 로그인 성공 여부를 판단해
<br>맞으면 로그인을 성공 시키는게 <strong>인증</strong>이다

코드로는 <pre>loadUserByUsername(username)</pre>DB에서 사용자 가져와서 Spring Security에게 사용자 정보 전달

<pre>passwordEncoder.encode(password)</pre>
로 비교한다

## 인가

로그인 성공 후에 권한을 부여 받고 다음 페이지로 넘어 갈 수 있다<br>
이게 바로 <strong>인가</strong>이다.

![alt text](<스크린샷 2026-05-10 020144.png>)
위와 같이 로그인을 하면 저렇게 뜨는데

코드를 보면
<pre>.requestMatchers("/admin").hasRole("ADMIN")</pre>

 ### /admin 페이지는 ROLE_ADMIN만 들어갈 수 있다라고 되어있다

 그런데 지금 admin이 아니여서 <pre>.anyRequest().authenticated()</pre>이 코드에 써진 것 같이 main으로 갔다.

 ### 아이디가 admin@test.com면 admin 역할을 얻게 설정해 두었다

<pre>UPDATE member_table
SET role = 'ROLE_ADMIN'
WHERE member_email = 'admin@test.com';
</pre>

로그인 해보겠다

![alt text](image-4.png)

/admin으로 뜬다

![alt text](image-5.png)

코드는
<pre> User.builder()
        .username(member.getMemberEmail())
        .password(member.getMemberPassword())
        .authorities(member.getRole())
</pre> 이걸로 권한 부여

<pre>.requestMatchers("/admin/**").hasAuthority("ROLE_ADMIN")</pre>이걸로 검사


