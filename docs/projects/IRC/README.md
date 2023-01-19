---
layout: default
title: IRC
nav_order: 4
parent: Projects
has_children: true
permalink: /docs/projects/irc
---

* [IRC](#irc)
  * [Requirements](#requirements)
    * [Allowed functions](#allowed-functions)
  * [Test example](#test-example)
* [Summary](#summary)
* [Functions](#functions)
  * [socket](#socket)
  * [setsockopt](#setsockopt)
    * [SO\_REUSEADDR](#so_reuseaddr)
  * [getsockname](#getsockname)
  * [poll](#poll)
    * [입출력 다중화 모델](#입출력-다중화-모델)
    * [Definition](#definition)
      * [ufds](#ufds)
      * [nfds](#nfds)
      * [timeout](#timeout)
* [TCP/IP](#tcpip)
  * [TCP/IP 통신 함수 사용 순서](#tcpip-통신-함수-사용-순서)


# IRC

이 프로젝트는 자신의 IRC 서버를 만드는 것입니다. 실제 IRC 클라이언트를 사용하여 서버에 연결하고 테스트합니다. 인터넷은 연결된 컴퓨터가 서로 상호 작용할 수 있도록 하는 견고한 표준 프로토콜에 의해 지배됩니다. 항상 알아두는 것이 좋습니다.  

Internet Relay Chat 또는 IRC는 인터넷의 텍스트 기반 통신 프로토콜입니다. 공개 또는 비공개가 될 수 있는 실시간 메시징을 제공합니다. 사용자는 다이렉트 메시지를 교환하고 그룹 채널에 참여할 수 있습니다. IRC 클라이언트는 채널에 참여하기 위해 IRC 서버에 연결합니다. IRC 서버는 함께 연결되어 네트워크를 형성합니다.  

C++ 98에서 IRC 서버를 개발해야 합니다.  
클라이언트를 개발하면 안 됩니다.  
서버간 통신을 처리하면 안 됩니다.  

실행 파일은 다음과 같이 실행됩니다.  
./ircserv <port> <password>

- port : IRC 서버가 들어오는 IRC 연결을 수신할 포트 번호입니다.
- password : 연결 암호입니다. 서버에 연결을 시도하는 모든 IRC 클라이언트에 필요합니다.

## Requirements

- 서버는 동시에 여러 클라이언트를 처리할 수 있어야 하며 중단되지 않아야 합니다.
- 포크는 허용되지 않습니다. 모든 I/O 작업은 비차단이어야 합니다.
- 이러한 모든 작업(읽기, 쓰기, 듣기 등)을 처리하는 데 하나의 poll()(또는 이와 동등한 것)만 사용할 수 있습니다.
- 여러 IRC 클라이언트가 존재합니다. 그 중 하나를 참조로 선택해야 합니다. 참조 클라이언트는 평가 프로세스 중에 사용됩니다.
- 참조 클라이언트는 오류 없이 서버에 연결할 수 있어야 합니다.
- 클라이언트와 서버 간의 통신은 TCP/IP(v4 또는 v6)를 통해 이루어져야 합니다.
- 서버에서 참조 클라이언트를 사용하는 것은 공식 IRC 서버에서 사용하는 것과 유사해야 합니다. 그러나 다음 기능만 구현하면 됩니다.
  - 참조 클라이언트를 사용하여 인증하고, 닉네임, 사용자 이름을 설정하고, 채널에 참여하고, 개인 메시지를 보내고 받을 수 있어야 합니다.
  - 한 클라이언트에서 채널로 보낸 모든 메시지는 채널에 가입한 다른 모든 클라이언트에게 전달되어야 합니다.
  - 운영자와 일반 사용자가 있어야 합니다.
  - 그런 다음 연산자에 특정한 명령을 구현해야 합니다.

### Allowed functions

- socket
- setsockopt
- getsockname
- getprotobyname
- gethostbyname
- getaddrinfo
- freeaddrinfo
- bind
- connect
- listen
- accept
- htons
- htonl
- ntohs
- ntohl
- inet_addr
- inet_ntoa
- send
- recv
- signal
- lseek
- fstat
- fcntl
- poll



## Test example

가능한 모든 오류 및 문제(부분 데이터 수신, 낮은 대역폭 등)를 절대적으로 확인하십시오. 서버가 보내는 모든 것을 올바르게 처리하는지 확인하려면 nc를 사용하여 다음과 같은 간단한 테스트를 수행할 수 있습니다.  

```
\$> nc 127.0.0.1 6667
com^Dman^Dd
\$>
```

ctrl+D를 사용하여 'com', 'man', 'd\n'의 여러 부분으로 명령을 보냅니다. 명령을 처리하려면 먼저 수신된 패킷을 집계하여 재구성해야 합니다.


# Summary

- 여러 클라이언트를 처리하는 irc 서버를 구현한다.
- 모든 작업(읽기, 쓰기, 대기)을 처리하는데 하나의 poll()만 사용할 수 있다.
  - poll 함수는 여러 fd에 대하여 이벤트가 발생하기를 기다리고, 발생된 이벤트의 정보를 제공한다.
  - poll와 같은 함수를 사용하지 않고 구현할 수 있지만 시스템 리소스가 많이 사용될 것이다.
- 이미 존재하는 IRC 클라이언트를 참조하고, 평가 중에도 이를 사용한다.
  - 공식 IRC 서버에서 사용하는 것과 유사하게 사용할 수 있어야 한다.


# Functions

## socket

socket 함수는 통신을 위한 endpoint를 만들고 endpoint를 참조하는 file descriptor를 반환한다.  

```c
#include <sys/socket.h>

int socket(int domain, int type, int protocol);
```

- domain : 통신 도메인을 지정 (AF_UNIX(프로토콜 내부), AF_INET(IPv4), AF_INET6(IPv6))
- type : 사용할 프로토콜 지정 (SOCK_STREAM(TCP), SOCK_DGRAM(UDP), SOCK_RAW(사용자 정의))
- protocol : 프로토콜 값 지정 (IPPROTO_TCP(TCP), IPPROTO_UDP(UDP))


## setsockopt

fd인 sockfd가 참조하는 소켓에 대한 옵션을 조작하여 세부사항을 조절한다. 주소와 포트를 재사용하는데 도움이 된다. "이미 사용 중인 주소"와 같은 오류를 방지한다.  

```c
int setsockopt(int sockfd, int level, int optname, const void *optval, socklen_t optlen);
```

- sockfd : 소켓지정번호
- level : 소켓의 레벨로 어떤 레벨의 소켓정보를 가져오거나 변경할 것인지 명시한다. 보통 SOL_SOCKET과 IPPROTO_TCP 중 하나를 사용한다.
- optname : 설정을 위한 소켓옵션의 번호
- optval : 설정값을 저장하기 위한 버퍼의 포인터
- optlen : optval 버퍼의 크기

optval이 `void *`인 이유는 설정하려는 옵션에 따라서 다양한 크기를 가지는 데이터형이 사용되기 때문이다. SOL_SOCKET 레벨에서 사용할 수 있는 옵션과 데이터형은 다음과 같다.  

- SO_BROADCAST : (BOOL) 브로드캐스트 메시지 전달이 가능하도록 한다.
- SO_DEBUG : (BOOL) 디버깅 정보를 레코딩 한다.
- SO_DONTLINGER : (BOOL) 소켓을 닫을때 보내지 않은 데이터를 보내기 위해서 블럭되지 않도록 한다.
- SO_DONTROUTE : (BOOL) 라우팅 하지 않고 직접 인터페이스로 보낸다.
- SO_OOBINLINE : (BOOL) OOB 데이터 전송을 설정할때, 일반 입력 큐에서 데이터를 읽을 수 있게 한다. 이 플래그를 켜면 recv(:12)나 send(:12)에서 MSG_OOB 플래그를 사용할 필요 없이 OOB 데이터를 읽을 수 있다.
- SO_GROUP_PRIORITY : (int) 사용하지 않음
- SO_KEEPALIVE : (BOOL) Keepalives를 전달한다.
- SO_LINGER : (struct) LINGER	소켓을 닫을 때 전송되지 않은 데이터의 처리 규칙
- SO_RCVBUF : (int) 데이터를 수신하기 위한 버퍼공간의 명시
- SO_REUSEADDR : (BOOL) 이미 사용된 주소를 재사용 (bind) 하도록 한다.
- SO_SNDBUF : (int) 데이터 전송을 위한 버퍼공간 명시

### SO_REUSEADDR

소켓을 이용한 서버 프로그램을 운용하면 프로그램을 다시 실행시켜야하는 경우가 생긴다. 이 경우에 커널이 bind 정보를 유지하고 있다면 다음과 같은 에러가 발생된다.  

```
bind error : Address already in use
```

이러한 문제를 해결하기 위해서 SO_REUSEADDR로 소켓을 설정할 수 있다. 설정하면 기존에 bind로 할당된 소켓자원을 프로세스가 재사용할 수 있다.   

```c
int sock = socket(...);
setsockopt(sock, SOL_SOCKET, SO_REUSEADDR, (char *)&bf, (int)sizeof(bf));
```


참조 : [setsockopt - 소켓옵션](https://www.joinc.co.kr/w/Site/Network_Programing/AdvancedComm/SocketOption)  


## getsockname

getsockname은 지정한 소켓 지정자에 대한 정보를 가져온다.  

```c
#include <sys/socket.h>

int getsockname(int sockfd, struct sockaddr *name, socklen_t *namelen);
```



## poll

poll은 다중입출력을 구현하는 방법으로 사용된다. 여러 file descriptor에 대해서 event가 발생하기를 기다렸다가, event가 발생하면 그에 대한 정보를 제공한다.  

어떠한 server가 여러 client의 연결을 가진 상태에서 입력을 처리하는 경우가 있다. 이 경우에 client의 요청을 하나의 thread로 처리한다면 문제가 발생된다. client의 요청이 많아지면 그만큼 thread를 생성해야하기 때문이다.  

### 입출력 다중화 모델

입출력 다중화는 단일 프로세스에서 여러 개의 파일을 제어한다. 입출력 다중화는 "비동기/봉쇄 입출력 모델"의 응용이다.  

입출력 다중화는 fd를 배열로 관리하여 여러 개의 파일을 다룬다. 데이터 변경을 감시할 fd를 배열에 포함시키고, 데이터 변경이 발생되면 fd에 대응되는 배열에 표시한다. fd 배열의 값을 검사하는 방식으로 여러 개의 파일을 처리할 수 있게 된다.  

```c++
[0, 0, 1, 0, 1, 0, 0, 0, 1]

// 0 : 데이터 변화가 없음
// 1 : 데이터 변화가 있음
```

이 모델은 다음과 같은 제한을 가지고 있다.  

- 처리할 수 있는 파일의 최대 크기는 프로세스가 열 수 있는 파일의 최대 개수와 fd 테이블의 크기의 영향을 받는다. 일반적으로 fd 테이블의 크기는 1024이다.  
- 데이터 변경이 일어난 경우 배열의 값을 검사해야 한다. 최악의 경우에는 1024개의 필드를 모두 검사해야 한다.  
- 병렬 처리가 아니다. 그러므로 어떠한 파일이 처리되는 동안에 다른 파일은 대기해야 한다. 그러므로 데이터 처리 과정이 긴 서비스에 적용하기에 적당한 모델은 아니다. 데이터 처리 과정이 짧은 메시지 전달 서비스에 적합한 모델이다.  

### Definition

```c
#include <poll.h>

int poll(struct pollfd *ufds, unsigned int nfds, int timeout);
```

#### ufds

다음은 첫번째 인자인 ufds의 구조체이다.  

```c
struct  pollfd
{
  int   fd;
  short events;
  short revents;
}
```

- fd : 점검할 file descriptor
- event : fd에 대하여 기다릴 event(사용자 지정)
- revent : fd에 대하여 발생한 event(커널 지정)

pollfd 구조체로 fd와 기다릴 fd의 event를 지정한다. 이를 지정하면 poll은 해당 fd에서 event가 발생하는지 검사하게 되고, 발생되면 revent에 값을 반환한다. revent는 발생된 event에 반응한 커널의 반응 값이다. 그러므로 revent 값을 확인하여 event가 어떻게 처리되었는지 알 수 있다.  

다음은 events에 지정할 수 있는 값이다.  

```c
// <sys/poll.h>

#define POLLIN      0x0001  // 읽을 데이타가 있다.
#define POLLPRI     0x0002  // 긴급한 읽을 데이타가 있다.
#define POLLOUT     0x0004  // 쓰기가 봉쇄(block)가 아니다. 
#define POLLERR     0x0008  // 에러발생
#define POLLHUP     0x0010  // 연결이 끊겼음
#define POLLNVAL    0x0020  // 파일지시자가 열리지 않은것같은
                            // Invalid request (잘못된 요청)
```

#### nfds

poll의 두번째 인자인 nfds는 ufds의 크기이다. 조사할 fd의 크기로서 클라이언트의 크기라고 할 수 있다.  

#### timeout

poll의 마지막 인자인 timeout은 대기 시간이다. 

- 값을 지정하지 않거나 -1인 경우 event를 영원히 기다린다. 
- 0인 경우에는 기다리지 않고 다음 루틴을 진행한다.
- 0보다 큰 양의 정수인 경우에는 해당 시간만큼 기다린다. 


참조 : [poll man page](https://man7.org/linux/man-pages/man2/poll.2.html), [[리눅스] 입출력다중화 - POLL 함수의 개념과 응용 코드 예제](https://reakwon.tistory.com/219), [poll을 이용한 입출력 다중화](https://www.joinc.co.kr/w/Site/Network_Programing/Documents/Poll), [select를 이용한 입출력 다중화](https://www.joinc.co.kr/w/Site/system_programing/File/select), [select를 이용한 다중연결 처리 서버 작성](https://www.joinc.co.kr/w/Site/Network_Programing/Documents/select)  




# TCP/IP



## TCP/IP 통신 함수 사용 순서

![](/TIL/docs/src/projects/irc/tcpip1.png)  

![](/TIL/docs/src/projects/irc/tcpip2.png)  

출처 : [c언어 소켓 생성 함수 socket()](https://badayak.com/entry/C%EC%96%B8%EC%96%B4-%EC%86%8C%EC%BC%93-%EC%83%9D%EC%84%B1-%ED%95%A8%EC%88%98-socket)  





