# RestAPI 란?

REST는 Representational State Transfer를 의미한다.<br>
웹에 존재하는 모든 자원(이미지, 동영상, DB 자원)에 고유한 URI를 부여해 활용하는 것으로 자원을 정의하고 자원에 대한 주소를 지정하는 방법을 의미한다.

## 장점

- 서버와 클라이언트의 역할을 명확하게 분리한다.
- 여러가지 서비스 디자인에서 생길 수 있는 문제를 최소화한다.

~~뭔 개소리야~~

## 단점

- 표준이 존재하지 않는다.
- 사용할 수 있는 메소드가 4가지 밖에 없다.HTTP Method 형태가 제한적이다.
- 구형 브라우저가 아직 제대로 지원해주지 못하는 부분이 존재한다.(아래 2가지)
    - PUT, DELETE를 사용하지 못하는 점
    - [pushState](https://developer.mozilla.org/ko/docs/Web/API/History/pushState)를 지원하지 않는 점