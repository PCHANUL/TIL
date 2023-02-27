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




