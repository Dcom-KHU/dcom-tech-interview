# HTTPS와 SSL/TLS

<p align="center"><img src="https://velog.velcdn.com/images/bambookim/post/fe705146-977d-41c8-b278-d6d6e15ee373/image.png" width="40%" height="40%"></p>


요즘 웬만한 사이트들은 접속할 때 HTTP가 아닌 HTTPS를 사용하고 있다.

HTTP와 HTTPS에는 어떤 차이점이 있는지, HTTPS 통신 연결에는 어떤 방법들이 쓰이는지 알아보도록 하자.

## HTTP와 HTTPS

**HTTP**(HyperText Transfer Protocol)는 어플리케이션 계층(7계층)에서 작동하는 프로토콜로, **TCP/IP**를 기반으로 한다. Well-Known Port로 80번을 사용한다. HTTP는 암호화되지 않은 평문을 전송하는 프로토콜이므로, 패킷 감청을 통해 송신자/수신자가 아닌 제3자가 정보를 들여다볼 수 있는 위험성이 존재한다. 이러한 문제를 해결하기 위해 등장한 것이 **HTTPS**이다.

**HTTPS**(HTTP Secure)는 **HTTP**에 데이터 암호화가 추가된 프로토콜이다. Well-Known Port로 443번을 사용한다. 암호화를 사용하기 때문에 제3자가 패킷을 감청해도 어떤 정보인지 알 수 없다. 또한 정보를 주고받는 서버가 신뢰할 수 있는 서버인지 판별할 수 있도록 도와준다.

> HTTPS = HTTP + SSL/TLS
> 

## SSL/TLS

- SSL : Secure Sockets Layer
- TLS : Transport Layer Security

**SSL/TLS**는 **HTTPS**를 사용하기 위해 안전한 보안 채널을 생성해주는 프로토콜이다. **SSL**은 1990년대 후반 등장한 프로토콜로, 여러 알려진 취약점으로 인해 현재는 사용되지 않는 프로토콜이다. **SSL**의 대안으로 등장한 것이 **TLS**로, **SSL**과 **TLS**를 흔히 혼용하여 부르고 있지만 실제 사용되는 프로토콜은 **TLS**이다.

<p align="center"><img src="https://velog.velcdn.com/images/bambookim/post/121fd97e-dab8-430c-a9e3-7f0486332978/image.png" width="200"></p>

**SSL/TLS**는 7계층의 **HTTP**와 4계층의 **TCP** 사이에 위치한다.

## CA(Certificate Authority)와 SSL 인증서

SSL **인증서**는 SSL 프로토콜에서 사용되는 인증서로, 공인된 **CA** 기관에서 발급한다. SSL **인증서**는 다음과 같은 역할을 하며, 아래의 정보를 담고 있다.

### 인증서의 역할

- Client가 접속한 사이트(Server)가 신뢰할 수 있는 Server임을 **보증**한다.
- 통신 과정에서 사용할 **Server의 공개 키**를 Client에 전달한다.

### 인증서에 담겨있는 정보

- CA 정보와 사이트의 도메인 등
- **Server의 공개 키**

대개 브라우저들은 신뢰할 수 있는 공인 CA들의 리스트와, **각 CA들의 공개 키**를 알고 있다. SSL 인증서가 구체적으로 어떻게 이용되는지는 아래의 SSL Handshake를 통해 알 수 있다.

## SSL/TLS에서의 암호화 방식

### 대칭키 방식

- 동일한 하나의 키, 즉 대칭키로 암호화와 복호화를 하는 방식이다.
- 공개키 방식에 비해 속도가 빠르지만, 암호 통신을 하기 위해 대칭키를 주고받는 문제에서 키가 유출되는 등의 **키 교환 문제**가 발생할 수 있다.

### 공개키(비대칭키) 방식

- 한 쌍의 {**공개키, 비밀키**}를 통해 암호화와 복호화를 하는 방식이다. 즉, 공개키로 암호화하면 비밀키로 복호화, 비밀키로 암호화하면 공개키로 복호화할 수 있다.
- 대칭키 방식에 비해 보안이 뛰어나지만, 상대적으로 속도가 느리다.

두 방식의 장단점을 고려하여, SSL/TLS에서는 실제 데이터를 주고받기 위해 **대칭키**를 사용하고, 이 대칭키를 주고받기 위해 **공개키** 방식을 이용한다.

## SSL Handshake (TLS 1.2 버전에서)

TLS Handshake는 TCP Handshake를 통해 연결을 생성한 뒤 실시한다.

<p align="center"><img src="https://velog.velcdn.com/images/bambookim/post/a76f8a6f-0b1d-42aa-9cc3-1f665e2b77fb/image.png" width="600"></p>

### 0. Server는 CA를 통해 SSL 인증서를 생성한다.

1) 사이트(Server)는 CA에 사이트 정보와 사이트 공개 키를 보낸다.

<p align="center"><img src="https://velog.velcdn.com/images/bambookim/post/99a3cecf-0721-4944-9c57-67f4389a3191/image.png" width="500"></p>

2) CA는 CA의 비밀 키를 이용해 사이트 정보와 사이트 공개 키를 암호화하여 인증서를 생성해 사이트에 전달한다.

<p align="center"><img src="https://velog.velcdn.com/images/bambookim/post/0499d1d7-8f66-4fee-b04c-d27b397c64c8/image.png" width="500"></p>

### 1. ClientHello (Client → Server)

Client가 Server에 TLS 연결을 시도하며 패킷을 보낸다. 보내는 패킷에는 아래의 정보 등이 담겨있다.

- 사용 가능한 Cipher Suite 목록
- Session ID
- TLS Protocol Version
- Client Random Byte (무작위 키)

아래는 Cipher Suite의 한 예이다. (이미지 출처 [https://steady-coding.tistory.com/512](https://steady-coding.tistory.com/512))

<p align="center"><img src="https://velog.velcdn.com/images/bambookim/post/8d8d2abb-f834-4242-ac2e-e14d2ba9a85f/image.png" width="500"></p>

### 2. ServerHello (Server → Client)

Server는 Client로부터 ClientHello 패킷을 받고, CipherSuite 목록에서 하나를 선택한다.

아래의 정보 등을 담아 ServerHello 패킷을 Client로 보낸다.

- 선택한 Cipher Suite
- TLS Protocol Version
- Server Random Byte (무작위 키)

### 3. Certificate (Server → Client)

Server의 TLS 인증서를 Client에 전달한다. 이때 TLS 인증서는 CA의 비밀 키로 암호화된 상태이며, CA의 공개 키는 누구에게나 공개되어 있다. 

<p align="center"><img src="https://velog.velcdn.com/images/bambookim/post/ae61b6e2-3150-408a-aa8e-764578dc4fb2/image.png" width="500"></p>

Client는 CA의 공개 키를 이용해 TLS 인증서를 복호화한다. 복호화에 성공하면 해당 인증서는 CA가 서명한 진짜 인증서임을 검증한 것으로 볼 수 있다. (Authentication)

<p align="center"><img src="https://velog.velcdn.com/images/bambookim/post/90e2e68b-8f09-435f-8c20-069ea3f0536e/image.png" width="500"></p>

### 4. Client Key Exchange (Client → Server)

Client는 Random한 Pre-master key를 생성한 뒤, 3)에서 TLS 인증서 복호화를 통해 얻어낸 Server의 공개 키로 암호화하여 서버로 전송한다. 

<p align="center"><img src="https://velog.velcdn.com/images/bambookim/post/6ae81d4b-a11d-4576-919e-d148e287c75e/image.png" width="500"></p>

Server는 자신의 비밀 키로 이것을 복호화하여 Pre-master key를 얻어낸다.

<p align="center"><img src="https://velog.velcdn.com/images/bambookim/post/751e4cc4-8908-46c2-b10f-c744e016468d/image.png" width="500"></p>

Server와 Client는 주고받은 Client Random Byte, Server Random Byte, Pre-master key를 이용해 각자 독립적으로 master key를 만들어내고, 일련의 과정을 통해 이를 session key로 만들어낸다. Client와 Server가 만든 세션 키는 동일하고, 이를 앞으로 데이터를 주고받을 때 암호화할 대칭 키로 사용한다.

<p align="center"><img src="https://velog.velcdn.com/images/bambookim/post/7bda037c-34d2-44e5-b819-c63b96ad89d2/image.png" width="500"></p>

> 자료마다 해당 단계에서 대칭 키를 생성하는 방식이 조금씩 상이하나, Client가 Server로 보낸 어떤 ‘무엇인가'를 이용해 결과적으로 대칭 키를 얻는다는 점은 같다.
> 

### 5. ChangeCipherSpec / Finished (Client ↔ Server)

교환할 정보를 모두 교환하여 통신할 준비가 다 되었음을 알린 뒤(ChangeCipherSpec), SSL Handshake를 종료한다. (Finished)

SSL Handshake가 끝나면 Client와 Server는 각자 갖고 있는 대칭키를 이용해 데이터를 암호화하여 주고받는다.

<p align="center"><img src="https://velog.velcdn.com/images/bambookim/post/4616c49c-b76a-4bf0-8d10-750c209a1aad/image.png" width="500"></p>
