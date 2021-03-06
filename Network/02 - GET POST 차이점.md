# 02. GET/POST 차이점

> References <br> <a href="https://velog.io/@stampid/REST-API와-RESTful-API">REST API와 RESTful API</a> _.stampid_

## 1. GET

- 클라이언트에서 서버로 **데이터를 요청**하기 위해 사용되는 HTTP 메소드
- 보통 **데이터를 읽거나 가져올 때** 사용된다.
- 파라미터로 포함하여 전송하는 데이터는 **쿼리스트링**이라고 한다.

- GET의 특징
  - 요청이 **캐싱될 수 있다.**
  - 요청이 **브라우저 기록에 남는다.**
  - 요청을 **북마크에 추가할 수 있다.**
  - **데이터 길이에 제한**이 있다.
  - **멱등성**(여러 번 연산해도 결과가 같음)이 보장된다.

## 2. POST

- 클라이언트에서 서버로 **데이터를 전송**하기 위해 사용되는 HTTP 메소드
- 보통 **자원을 생성할 때** 사용된다.
- 데이터를 보낼 때 HTTP Body를 사용한다. <br>
  👉 눈에 보이지 않지만 브라우저 개발자 도구 등을 통해 볼 수 있다.

- POST의 특징
  - 요청이 **캐싱되지 않는다.**
  - 요청이 **브라우저 기록에 남아있지 않는다.**
  - 요청을 **북마크에 추가할 수 있다.**
  - **데이터 길이 제한이 없다.**
  - **멱등성이 보장되지 않는다.** <br>
    👉 요청할 때 마다 결과가 다를 수 있다.
