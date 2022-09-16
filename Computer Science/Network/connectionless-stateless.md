# Connectionless와 Stateless

**HTTP**는 여러 가지 특징을 갖고 있는데, 그 중 Connectionless와 Stateless에 대해 알아보자.

## Connectionless (비연결성)

### HTTP와 TCP/IP

**HTTP**는 **7계층**의 프로토콜로, **TCP/IP**를 기반으로 한다. **TCP/IP**의 경우 기본적으로 연결을 종료하지 않으면 그 연결은 종료되지 않고 유지된다. 만약 하나의 서버에 여러 클라이언트가 접속해 있다고 하자.

<p align="center"><img src="https://velog.velcdn.com/images/bambookim/post/be00ef84-de1c-4529-9c74-1c57cb650abc/image.png" width="50%"></p>


모든 연결이 동시에 유의미한 통신을 하고 있을 확률은 적다. 즉, 놀고 있는 연결이 많을 가능성이 크고 이는 서버의 자원 낭비로 이어진다. 따라서 **HTTP**의 초기 모델은 각 클라이언트가 서버에 요청을 하고 응답을 받으면 바로 **TCP/IP** 연결을 끊도록 하는 방식이었는데, 이러한 **HTTP**의 특징을 **Connectionless**하다고 한다.

<p align="center"><img src="https://velog.velcdn.com/images/bambookim/post/749567dd-88b3-4da5-8442-cee5c0d5187c/image.png" width="50%"></p>

### HTTP 1.0 - 비지속 연결 (Non-Persistent Connection)

하지만 웹 페이지에는 HTML 문서뿐만 아니라 script나 이미지 파일 등 수많은 Request와 Response가 존재한다. HTTP 1.0 버전에서는 이러한 각 요청과 응답마다 TCP 연결을 새로 생성하는 과정에서 **매번** **3-way handshake**를 진행하여 연결을 establish해야 했으므로, 오버헤드에 의한 자원 낭비와 성능 저하가 심했다.

<p align="center"><img src="https://velog.velcdn.com/images/bambookim/post/95582e21-1c75-4548-9ef5-ef2858b1a97b/image.png" width="40%"></p>

### HTTP 1.1 - 지속 연결 (Persistent Connection)

HTTP 1.1부터는 비지속 연결의 단점을 보완하고자 **TCP 연결을 재사용**하도록 했는데, 이를 **지속 연결**이라고 하며 default로 지원한다. Request Header의 `Connection`에 `keep-alive`를 지정하면 지속 연결로, `close`를 지정하면 비지속 연결로 동작한다.

<p align="center"><img src="https://velog.velcdn.com/images/bambookim/post/6d4774cd-8cbb-419f-82df-c0415d1792b6/image.png"></p>

지속 연결을 구현하는 방법은 두 가지가 있다.

- 첫 번째 방법은 단일 TCP 연결을 이용한 **Pipelining**이다. 연결 생성-종료 사이에 response를 기다리지 않고 여러 request를 보내는 방식인데, response를 받는 통로인 TCP 연결이 딱 하나이므로 앞서 가는 Response가 모종의 이유로 지연된다면 뒤따르는 다른 Response들 또한 지연되는 **Head of Line Blocking (HoL Blocking)** 문제가 발생할 수 있다.
- 두 번째 방법은 여러 개의 TCP 연결(**Multiple Connections**)을 뚫어 놓고 병렬로 Request와 Response를 처리하는 것이다. 하지만 TCP 연결을 여러 개 유지하기 위한 **자원 소모**가 상당하다는 단점이 존재한다.

**HTTP 2.0**은 이러한 단점들을 보완하여 하나의 TCP 연결로 더 최적화된 통신을 지원한다.

## Stateless (무상태)

**Stateless**와 반대되는 표현은 Stateful인데, 서버가 이전 요청에서의 클라이언트의 상태를 보존한다는 것이다. **Stateless**는 통신이 끝나면 더 이상 상태를 유지하지 않는다는 **HTTP**의 또다른 특징이다.

하지만 상태를 유지하지 않으면 다음과 같은 문제점이 있다.

- 사이트에 로그인을 했는데, 페이지를 이동할 때마다 이전의 로그인이 유지되지 않아 매번 로그인을 해야 한다.
- 분명 사이트에 들어가서 팝업을 하루동안 보지 않기로 체크를 했는데, 페이지를 유지할 때마다 매번 팝업창이 떠서 팝업을 닫아야 한다.

## 쿠키와 세션

Connectionless와 Stateless를 보완하기 위해 우리는 **쿠키(Cookie)**와 **세션(Session)**을 이용한다.

[쿠키와 세션](https://velog.io/@bambookim/%EC%BF%A0%ED%82%A4%EC%99%80-%EC%84%B8%EC%85%98)

## 참고 자료

- [https://hirlawldo.tistory.com/106](https://hirlawldo.tistory.com/106)
- [https://velog.io/@duarufp06/HTTP-Stateless-Connectionless-HTTP-메시지-개념](https://velog.io/@duarufp06/HTTP-Stateless-Connectionless-HTTP-%EB%A9%94%EC%8B%9C%EC%A7%80-%EA%B0%9C%EB%85%90)
