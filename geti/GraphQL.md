## GraphQL

클라이언트가 필요한 필드만 콕 집어 요청하는, REST 대신 쓰는 API 쿼리 언어

## Query

DB나 서버에 데이터를 달라고 던지는 요청문

## Mutation

GraphQL에서 데이터를 생성/수정/삭제하는 변경 요청문

## Resolver

GraphQL 주문서의 각 필드 값을 실제로 어디서 어떻게 가져올지 처리하는 담당 함수

## Schema

GraphQL 서버가 뭘 주문받을 수 있는지 정의해둔 메뉴판

## DataLoader

필드 요청들을 모아뒀다가 IN 쿼리 한 방으로 처리해서 N+1을 잡는 모아치기 도구