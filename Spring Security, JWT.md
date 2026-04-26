# Spring Security?

: 로그인/권한 관리 담당

예)
- 누가 들어오는지 확인
- 이 사람이 뭐 해도 되는지 확인

# JWT?

: 서버가 안 물어봐도 되는 로그인 증명서

예)
- 로그인 성공 시 → 토큰 하나 줌
- 이후 요청: 토큰으로 해결

서버가 DB 안 봐도 됨 = 빠름

## 구조

JWT = 3개 덩어리<br>
<strong> HEADER, PAYLOAD, SIGNATURE</strong>

### HEADER
- 어떤 방식으로 암호화했는지

### PAYLOAD
- 유저 정보 (id, role 등)

### SIGNATURE
- 위조 방지용 서명

## 서버만 이 서명을 만들 수 있음 = 위조 불가





## 전체 흐름

1.로그인 요청<br>
아이디 / 비밀번호 전송

2️. Spring Security가 인증<br>
맞으면 통과

3️. JWT 생성<br>
서버 → 토큰 발급

4️. 클라이언트 저장<br>
localStorage or 쿠키

5️. 이후 요청<br>
Authorization: Bearer 토큰

6️. 서버에서 검증<br>
JWT 서명 확인 → 통과하면 OK