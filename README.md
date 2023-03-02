---
layout: default
title: Today I Learned
nav_order: 1
has_children: true
child_nav_order: desc
permalink: /
---

# Today I Learned <!-- omit in toc -->


---

## 23.03.02

### OAuth

OAuth는 인터넷 사용자들이 비밀번호를 제공하지 않고 다른 웹사이트 상의 자신들의 정보에 대해 웹사이트나 애플리케이션의 접근 권한을 부여할 수 있는 공통적인 수단으로서 사용되는 개방형 표준이다. 예를 들어, 앱에서 사용자 인증을 위해 Google이나 Facebook에서 제공하는 사용자 인증 방식을 사용할 수 있다. 그리고 OAuth를 바탕으로 앱은 외부 서비스(Google, Facebook 등)의 특정 자원에 접근하고 사용할 수 있는 권한을 인가받는다.  

### OAuth 참여자

- Resource Server : Client가 제어하고자 하는 자원을 보유하고 있는 서버
- Resource Owner : 자원의 소유자
- Client : Resource Server에 접속해서 정보를 가져오고자 하는 클라이언트

### OAuth Flow

1. Client 등록

Client가 Resource Server를 이용하기 위해서는 자신의 서비스를 등록하여 승인을 받아야 한다. 등록 절차를 통해 세가지 정보를 부여받는다.  

- Client ID : 클라이언트 웹 앱을 구별할 수 있는 식별자
- Client Secret : ClientID에 대한 비밀키
- Authorized redirect URL : Authorization Code를 전달받을 리다이렉트 주소

Client가 인증을 마치면 명시된 url로 리다이렉트시킨다. 이때 Query String으로 Code가 함께 전달된다. Client는 이 Code와 Client ID, Client Secret을 Resource Server에 보내서 자원을 사용할 수 있는 Access Token을 발급받는다.  

2. Resource Owner의 승인

Resource Owner는 로그인을 위해 특정 주소로 연결되는 소셜 로그인 버튼을 클릭한다. Resource Owner는 Resource Server에 접속하여 로그인을 수행한다. 로그인이 완료되면 Resource Server는 Query String으로 넘어온 파라미터들을 통해 Client를 검사한다. 검증이 마무리되면 권한 부여를 승인받은 후에 Resource Server는 Resource Owner에게 Client의 접근을 승인하게 된다.  

3. Resource Server의 승인

Resource Owner의 승인이 마무리 되면 명시된 Redirect URL로 클라이언트를 리다이렉트 시킨다. 이 때 Resource Server는 Client가 자신의 자원을 사용할 수 있는 Access Token을 발급하기 전에, 임시 암호인 Authorization Code를 함께 발급한다.  

Query String으로 들어온 code가 바로 Authorization Code이다.  

Client는 ID와 비밀키 및 code를 Resource Owner를 거치지 않고 Resource Server에 직접 전달한다. Resource Server는 정보를 검사한 다음, 유효한 요청이라면 Access Token을 발급하게 된다.  

Client는 해당 토큰을 서버에 저장해두고, Resource Server의 자원을 사용하기 위한 API 호출시 해당 토큰을 헤더에 담아 보낸다.  

4. API 호출




## 23.03.01

### Signalling Server

피어를 다음과 같은 순서로 연결할 수 있다.

1. 브라우저에서 미디어 스트림을 받는다.
2. 스트림을 넣어준다.
3. local sdp를 설정한다.
4. 연결할 Peer에 Offer을 제안한다.

연결할 Peer에서는 offer을 받으면

1. remote sdp를 설정한다
2. 브라우저 미디어 스트림을 받는다.
3.  스트림을 넣어준다.
4. local sdp 설정
5. create answer 후 answer을 보낸다.
6. 받는 Peer에서는 remote sdp를 설정한다.

create-answer 과정이 끝나면 icecandidate로 네트워크 정보를 교환한다.

1. 요청자에서 candidate를 보낸다.
2. 연결할 peer에서 받은 정보를 저장하고 자신의 candidate를 보내고
3. 받는 쪽에서 해당 candidate를 저장한다.




## 23.02.28

### WebRTC 동작 원리

WebRTC가 동작하기 위해서 P2P 연결이 필요하고, 연결을 위해서 서버가 필요하다. 피어가 서로의 정보를 교환해야 하기 때문이다. 서버로 피어간에 정보를 전달시키면 되지만 서버에서 공유하는 공인IP를 얻기위해서 STUN서버가 필요하다. 만약에 STUN 서버로 해결하지 못하는 경우에는 TURN 서버가 필요하다.  

### 서버 종류

#### Mesh (Signalling Server)

- 피어간의 offer, answer라는 session 정보 signal만을 중계한다. 
- 그러므로 처음 WebRTC가 피어간의 정보를 중계할 때만 서버에 부하가 발생한다.
- 주로 1:1 연결에서 사용된다.
- 서버에 부하가 없다.

#### SFU (Selective Forwarding Unit Server)

- 미디어 트래픽을 중계하는 중앙 서버 방식
- 서버와 클라이언트 간의 피어를 연결시킨다.
- 소규모 실시간 스트리밍에 적합하다.
- 클라이언트가 받ㄷ는 부하가 줄어든다.

#### MCU (Multi-point Control Unit Server)

- 다수의 송출 미디어를 중앙 서버에서 혼합 또는 가공하여 수신측으로 전달하는 중앙 서버 방식이다.
- 과부화로 레이턴시가 증가할 수 있다.
- 클라이언트 부하가 현저히 줄어들지만 서버 부담이 커진다.





## 23.02.27

### WebRTC

WebRTC는 리얼 타임 음성, 영상, 데이터 교환을 할 수 있는 P2P 기술이다. 서로 다른 네트워크에 있는 2개의 디바이스들을 서로 위치시키기 위해서는 각 디바이스들의 위치를 발견하는 방법과 미디어 포맷 협의가 필요하다. 이 프로세스를 시그널링이라 부르고 각 디바이스들을 상호간에 동의된 서버에 연결시킨다. 이 서버는 각 디바이스들이 협의 메세지들을 교환할 수 있도록 한다.  

#### The signaling server

두 디바이스 사이에 WebRTC 커넥션을 만들기 위해, 인터넷 네트워크에서 이 둘을 연결시키는 작업을 해줄 signaling server가 필요하다.  

가장 먼저, signaling server가 필요하다. 두 피어들 사이에서 시그널링에 관련된 정보들을 전달해줄 수 있다면 WebSocket이든 XMLHttpRequest이든 상관없다. 중요한 점은 시그널링 서버가 데이터 내용을 몰라도 된다. 메세지 내용들이 그저 시그널링 서버를 통해 상대편으로 가기만 하면된다.  




## 23.02.26

### Node.js

Node.js는 Chrome V8 JS 엔진으로 빌드된 JS 런타임이다.  
- 확장성이 있는 네트워크 애플리케이션 개발에 사용되는 소프트웨어 플랫폼이다.  
- JavaScript로 Non-blocking I/O와 단일 스레드 이벤트 루프를 통해 높은 처리 성능을 가지고 있다.
- 내장 HTTP 서버 라이브러리를 포함하기 때문에 별도의 소프트웨어 없이 동작할 수 있다.

JavaScript는 웹 브라우저 프로그램에서 사용되는 스크립트 언어이기 때문에 웹 브라우저가 없다면 사용할 수 없다. 
그래서 Node.js가 개발되었고, Node.js를 사용하여 JavaScript로 웹 브라우저에서 분리된 프로그램을 만들 수 있게 되었다.  

Node.js는 싱글 스레드, 논 블로킹 모델이다. 
싱글 스레드가 모든 작업을 처리하지만 논 블로킹 방식으로 이전 작업의 완료를 기다리지 않고 다음 작업을 처리한다. 
그래서 크기는 작지만 개수가 많은 데이터를 주고 받는 작업에 적합하다.  


[Node.js 개념 이해하기](https://hanamon.kr/nodejs-%EA%B0%9C%EB%85%90-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0/)  
[블로킹/논블로킹, 동기/비동기](https://velog.io/@nittre/%EB%B8%94%EB%A1%9C%ED%82%B9-Vs.-%EB%85%BC%EB%B8%94%EB%A1%9C%ED%82%B9-%EB%8F%99%EA%B8%B0-Vs.-%EB%B9%84%EB%8F%99%EA%B8%B0)  



## 23.02.24

### PostgreSQL

PostgreSQL은 객체-관계형 데이터베이스 시스템(OPDBMS)이다.  
관계형 DBMS의 기본적인 기능인 트랜잭션과 ACID(Atomicity, Consistency, Isolation, Durability)를 준수하여 데이터의 일관성과 무결성을 보장한다.  

PostgreSQL은 매우 안정적이고, 확장성이 뛰어나며, 다양한 운영 체제와 프로그래밍 언어를 지원한다. 매우 강력한 기능과 성능을 제고하며, 고급 데이터 유형을 지원하고, 풍부한 확장 기능을 가지고 있어서 많은 기업들이 데이터베이스 관리 시스템으로 선택하고 있다.  

### NestJS

NestJS는 Node.js를 위한 프레임워크이다. TypeScript로 작성된 서버 사이드 애플리케이션을 빠르고 효율적으로 구축할 수 있도록 한다.  

Angular에 영감을 받아 만들어져서 비슷한 구조와 패턴을 사용한다. 코드를 모듈화하고 레이어를 분리하여 코드의 유지 보수를 쉽게하여 확장성을 높인다. 그리고 Express, Fastify 등의 Node.js HTTP 프레임워크 위에서 작동하여 개발자들에게 친숙한 인터페이스를 제공한다.  

NestJS는 강력한 의존성 주입 시스템을 가지고 있어서 서비스를 쉽게 분리하고 모듈화할 수 있다. 또한, 지원되는 데이터베이스 시스템들과 함께 사용할 수 있는 ORM(Object-Relational Mapping)도 포함하고 있다.  




