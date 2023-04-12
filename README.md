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

## 23.04.11

- 서버와 클라이언트의 연결 상태
- ping 간격 3초
- 서버가 ping을 클라이언트에게 보내면 응답으로 pong을 받는다.
- 서버가 응답을 받지 못하는 경우 처리 방법이 필요하다.
- 여러 상황이 존재한다.
  - 게임 대기열에 있는 경우
    - 큐 제거
  - 게임 설정방에 있는 경우
    - 플레이어가 나감으로 처리
  - 게임 플레이하고 있는 경우
    - 플레이어가 나감으로 처리

- notice
  - code를 추가
  - 대상에게 해야하는지
  - 예를 들어, mute
- mute를 데이터 베이스에서 제거
- socket data 정리
- 



## 23.04.10

### 유저의 게임 대기열에 추가 플로우

- 유저가 대기열에 들어가겠다는 요청을 보낸다.
- 대기열을 확인하여 들어갈 수 있는 방이 있는지 확인한다.
- 방에 들어갈 수 없는 조건은 다음과 같다.
  - 유저가 서로 차단한 상태
  - 방의 모드가 다른 경우
- 만약에 방이 없다면 새로운 방을 만들고, 있다면 들어간다.
- 초대방인 경우에는 방이 대기열에 존재하지 않는다.

- 대기열에 유저가 들어간 후에는 인터벌이 시작된다.
  - 네트워크 연결 상태 확인, 방이 생성된 시간 체크
- 네트워크 연결 상태를 확인하는 것은 게임이 종료되기 전까지 지속된다.
- 방이 생성된 시간을 체크하는 것은 player2가 들어오기 전까지 지속된다.

- 클라이언트의 네트워크 상태 확인
- ping 메세지를 socketio로 보낸다.
- 클라이언트는 pong 메세지를 서버에 보내어 이상이 없음을 알린다.
- 서버는 클라이언트들의 pong 메세지를 저장한다.
- 다음 ping 메세지를 보내기 전에 해당 클라이언트가 pong을 보냈는지 확인한다.
- 만약에 pong을 보내지 않았다면 네트워크에 문제가 생긴 것이다.


### prisma migration
- 스키마가 변경되어도 이전 데이터를 유지해야한다.
- prisma로 postgresql DB를 조작하고 있다.
- prisma schema에서 정의한 모델을 바탕으로 실제 DB의 테이블을 생성할 수 있다.

- prisma migrate 동작 원리
  1. schema.prisma 생성
  2. draft migration file 생성
  3. draft migration을 DB schema에 적용

### 1. schema.prisma 생성

schema.prisma에 수정사항이 발생되었다.

```
id    Int 
age   Int 
name  String
```

### 2. draft migration file 생성

migration을 실제로 DB schema에 적용하지 않고 초안을 작성한다.  
다음 명령어를 실행하면 생성된다.  

```
prisma migrate dev --create-only --preview-feature
```

migration 이름을 입력하면 prisma/migrate/에 sql파일이 생성된다.  
prisma가 알아서 sql 파일을 생성해준다.

### 3. draft migration file을 DB schema에 적용

DB schema를 다음 명령어를 사용하여 적용할 수 있다.

```
npx prisma migrate deploy --preview-feature
```



## 23.04.05

- 데이터의 흐름
- DB -> 서버 -> 클라이언트
- 데이터는 다른 레이어로 이동될 때, 형태가 변한다.
- 변형되는 형태에 따라서 정의된 객체가 필요하다.

- 문제는 각각의 타입에서 다른 타입의 메서드를 사용해야하는 경우가 있다.
- 만약에 channel에서 user의 객체를 확인하려면 userId를 받아서 직접 찾고 가져와서 객체를 확인했다.
- 이제는 Channel의 메소드에서 user 객체를 직접받아야 한다.


## 23.04.04

- 채팅을 구현하며 클라이언트의 데이터를 서버에 저장하고 있다.
- 서버에서 클라이언트의 상태 데이터를 가지고 있다.
- 만약에 서버가 종료된다면 데이터가 사라진다.
- 그러므로 서버가 종료되어도 데이터를 보존하려면 다른 곳에 저장해야 한다.
- 보통 캐시 서버를 사용하지만 이번에는 DB 서버에 저장하는 방식을 사용한다.

- 서버의 데이터와 데이터 베이스의 데이터의 형태는 다르다. 
- 차이를 처리하는 방법이 필요하다.
- 서버에서 사용하는 데이터의 인터페이스를 만들면 간극을 메울 수 있다.
- 지금은 Map에 데이터를 저장하여 관리하고 있다. 
- Map의 메소드를 가진 클래스를 만들면 코드의 수정없이 데이터 베이스와 연결할 수 있다.
- 

## 23.04.03

- 서버에서 소켓 테스트하는 방법
- jest에서 제공하는 it메서드를 사용하여 테스트 케이스를 작성한다.
- it 메서드에 title과 callback 함수를 보낸다.
- callback 함수에서 done 파라미터를 받을 수 있다. 
- done 파라미터는 함수로써 테스트 케이스를 마치는 위치에서 호출한다.
- 테스트 케이스의 전과 후로 호출되는 메소드가 제공된다.
- afterAll, beforeAll은 describe 스코프 안에서 전후로 한번씩 호출된다.
- afterEach, beforeEach는 it이 호출되기 전후로 한번씩 호출된다.

- describe로 테스트 카테고리를 나눌 수 있다.
- 카테고리는 전후 처리가 같은 테스트로 묶는다.



## 23.03.30

- 클라이언트와 서버간의 통신을 상태를 유지해야하는가?
- socketio로 채팅 서비스를 구현하고 있다. 
- 채팅 서비스에서 필요한 유저와 채널의 상태를 서버에 저장하고 있다.
- 클라이언트의 상태를 서버에 보존하는 방식은 문제점을 가지고 있다. 
- 해당 서버가 멈추거나 이상이 생겨서 다른 서버를 사용해야할 때 문제가 발생한다.
- 새로운 서버에는 이전 서버에서 가지고 있는 상태값이 없기 때문이다. 
- 그래서 이러한 클라이언트의 상태 데이터를 캐시 서버에 저장한다. (Redis)
- 서버에서 클라이언트의 상태를 저장하지 않으면 서버는 단순히 요청에 응답하는 역할을 수행한다.
- 이로 인해 다른 서버를 사용하여도 같은 응답을 받게되어 서버 확장을 수월하게 한다.

- 서비스가 커질수록 테스트하기 어려워진다.
- 채팅 서비스를 구현하다가 메세지 보내기를 테스트하려면 여러 과정을 거쳐야한다.
- 사용자 인증, 토큰 발급, 유저 생성, 채팅방 생성, 다른 유저 초대하는 과정을 거쳐야 메세지를 보낼 수 있다.
- 그리고 추가로 서버를 빌드하는 시간까지 포함한다면 개발 시간이 비약적으로 늘어난다.
- 비효율적으로 사용되는 시간을 줄이기 위해서 테스트 케이스를 작성해야 한다.


## 23.03.28

### user 재접속시 socket 관리
- 어떤 유저가 나가는 경우
  - 로그아웃
  - 채팅방 나가기
  - 브라우저 종료
  - 회원 탈퇴
- 어느 채팅방에 유저가 있는지 알아야 한다.
- 유저가 재접속하면 다른 socket으로 통신한다.
- user객체에서 socket만 변경하여 사용한다.

### user 식별 방법
- REST API를 구현할 떄는 유저를 db 상의 id로 구별했다.
- socket에는 id가 주어진다.
- rest와 socket을 함께 사용하고 있으므로 한가지로 통일해야 한다.
- db의 userId와 username을 socket에서도 사용해야 한다.


## 23.03.27

### gateway와 service의 의존성 문제
- gateway에서 로직을 service로 분리하려한다.
- 채널에 메세지를 보내려면 `Server`객체가 필요하다.
- `@WebSocketServer` 데코레이터는 `@WebSocketGateway` 로 정의된 클래스 안에서 정의된다. 
- service에서 server를 사용하려면 gateway를 연결해야 한다.
- 이미 gateway가 service를 의존하고 있으므로 service가 gateway를 의존하면 에러가 발생된다.
- gateway에는 `afterInit()`이 있다.
- `this.chatService.server = this.server;`를 추가하여 해결

### Socket 객체
- socket event 부분을 gateway와 service로 나눈다.
- chatGateway, chatService
- chat.controller를 만들어서 chatService를 주입한다.
- REST API에서는 Socket 객체를 받을 수 없다.
- chatService에서 Socket 객체를 관리한다.
- user의 nickname으로 Socket 객체를 가져온다.



## 23.03.05

### NestJS 데코레이터

데코레이터를 사용하면 횡단 관심사를 분리하여 관점 지향 프로그래밍을 적용한 코드를 작성할 수 있다. 데코레이터는 클래스, 메서드, 접근자, 프로퍼티, 매개변수에 적용가능하다. 각 요소의 선언부 앞에 데코레이터를 선언하면 데코레이터로 구현된 코드를 함께 실행한다.  

데코레이터는 `@expression` 형식으로 사용한다. expression은 데코레이팅 된 선언에 대한 정보와 함께 런타임에 호출되는 함수이어야 한다.  

데코레이터 함수에는 세가지 인자가 전달된다.  
1. target : 현재 인스턴스 객체의 클래스
2. propertyKey : 데코레이터를 적용할 속성 이름
3. descriptor : 해당 속성의 설명자 객체

```ts
function deco(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
  console.log('데코레이터가 평가됨');
}

class TestClass {
  @deco
  test() {
    console.log('함수 호출됨');
  }
}

const t = new TestClass();
t.test();
```

#### 데코레이터 팩토리

만약에 데코레이터에 인자를 넘겨서 동작을 변경하고 싶다면 데코레이터 팩토리를 만들면 된다. 아래의 예시는 value라는 인자를 받는다.  

```ts

function deco(value: string) {
  console.log('데코레이터가 평가됨');
  return function (target: any, propertykey: string, descriptor: PropertyDescriptor) {
   console.log(value); 
  }
}

class TestClass {
  @deco('HELLO')
  test() {
    console.log('함수 호출됨');
  }
}

/*

데코레이터가 평가됨
HELLO
함수 호출됨

*/
```

#### 데코레이터 합성

여러개의 데코레이터를 사용한다면 다음 단계가 수행된다.  

1. 각 데코레이터의 표현은 `위에서 아래로 평가`
2. 그 다음에 결과는 `아래에서 위로 호출`

```ts

function first() {
  console.log("first(): factory evaluated");
  return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    console.log("first(): called");
  }
}

function second() {
  console.log("second(): factory evaluated");
  return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    console.log("second(): called");
  }
}

class ExampleClass {
  @first()
  @second()
  method() {
    console.log('method is called');
  }
}

/*

first(): factory evaluated
second(): factory evaluated
second(): called
first(): called
method is called

*/

```


#### Class 데코레이터

클래스 데코레이터는 클래스의 생성자에 적용되어 클래스 정의를 읽거나 수정할 수 있다. 선언 파일이나 선언 클래스내에서는 사용할 수 없다.  

```ts

// 클래스 데코레이터는 생성자를 반환하는 함수이어야 한다.
functionn reportableClass<T extends { new ( ... args: any[]): {} }>(constructor: T) {
  return class extends constructor {
    reportingURL = 'http://www.example.com';
  }
}

@reportableClass
class BugReport {
  type = 'report';
  title: string;
  contructor(t: string) {
    this.title = t;
  }
}

const bug = new BugReport('Needs dark mode');
console.log(bug);

/*

BugReport {
  type: 'report',
  title: 'Needs dark mode',
  reportingURL: 'http://www.example.com'
}

*/

```


#### Method 데코레이터

메서드 바로 앞에 선언되어 속성 디스크립터에 적용되고 메서드의 정의를 읽거나 수정할 수 있다. 선언 파일, 오버로드 메서드, 선언 클래스에 사용할 수 없다.  

```ts

function HandleError() {
  return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    const method = descriptor.value;
    descriptor.value = function() {
      try {
        method();
      } catch(e) {
        console.log(e);
      }
    }
  }
}

class Greeter {
  @HandleError()
  hello() {
    throw new Error('텍스트 에러');
  }
}

const methodDeco = new Greeter();
methodDeco.hello();

/*

{}
hello
{
  value: function,
  writable: true,
  ...
}
Error: 테스트 에러

*/

```


#### NestJS - module

App module은 root module과 같다. provider와 controller를 추가하여 module를 구성하고 App module에 import하여 사용한다.  

Controller나 Provider에 배열을 입력할 때 순서를 신경써야한다. 예를 들어, `/todos`와 `/todos/name` URL가 있는 경우에 `/todos/name`을 담당하는 Controller를 먼저 작성해야 한다. 클라이언트의 요청은 배열에 작성된 순서대로 컨트롤러가 처리된다.  

#### NestJS - controller

express의 라우터처럼 URL을 가져오고 함수를 실행한다. URL을 매핑하고, 요청을 받고, 쿼리를 넘기는 역할을 한다. 비즈니스 로직은 provider에서 구현된다.  

```ts
@Controller('movies')
export class MoviesController {
  @Get()
  getAll() {
    return 'movies';
  }
  @Get('/:id')
  getOne(@Param('id') movieId: string) {
    return `one movie id: ${movieId}`;
  }
  @Post()
  create() {
    return 'This will create a movie';
  }
  @Delete('/:id')
  remove(@Param('id') movieId: string) {
    return `this will delete a movie with the id : ${movieId}`;
  }
  @Patch('/:id')
  path(@Param('id') movieId: string) {
    return `this will patch a movie with the id : ${movieId}`;
  }
}
```

#### NestJS - Provider

종속성으로 주입할 수 있다. controller가 HTTP 요청을 처리하고 더 복잡한 적업을 Provider를 위임해야 한다. HTTP요청을 처리하여 필요한 Service를 호출한다.

모든 Provider들 위에 반드시 선언되어야하는 데코레이터이다. 이 데코레이터가 있어야 Modele을 통해 Inject된다.  









## 23.03.04

### passport-oauth2-refresh

https://github.com/fiznool/passport-oauth2-refresh  

## 23.03.03

### Refreshing Access Tokens

https://www.oauth.com/oauth2-servers/access-tokens/refreshing-access-tokens/  

- Request Parameters
  - grant_type(required) : "refresh_token"
  - refresh_token(required) : 이전에 받은 refresh token
  - scope (optional) : 원래 액세스 토큰에서 발급되지 않은 추가 범위를 추가하지 말아야 한다. 생략된 경우에는 이전에 발급된 것과 동일한 범위의 액세스 토큰이 발급된다.  
  - Client Authenticaiton (만약에 secret을 발급했다면 required이다)

### OAuth와 JWT

OAuth와 JWT는 둘 다 인증 및 권한 부여를 위한 프로토콜 및 토큰이지만 목적과 동작 방식에 차이가 있다.  

- OAuth : 사용자가 서드파티 애플리케이션에 대한 접근 권한을 제어하는 프로토콜
- JWT : JSON Web Token의 약자로, 서비스 간에 정보를 안전하게 전송하기 위한 토큰

따라서, OAuth는 사용자가 서드파티 애플리케이션에 대한 접근 권한을 관리하는 반면에, JWT는 서비스 간에 정보를 안전하게 전송하고 인증 및 권한 부여를 처리한다.


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




