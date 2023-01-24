---
layout: default
title: Today I Learned
nav_order: 1
has_children: true
child_nav_order: desc
permalink: /
---

# Today I Learned <!-- omit in toc -->

* [23.1.24](#23124)
  * [State diagram for server and client model](#state-diagram-for-server-and-client-model)
  * [Server](#server)
* [23.1.23](#23123)
  * [htons](#htons)
  * [inet\_addr](#inet_addr)
  * [inet\_ntoa](#inet_ntoa)
* [23.1.19](#23119)
  * [socket](#socket)
  * [socket programming](#socket-programming)
    * [socket](#socket-1)
    * [setsockopt](#setsockopt)
    * [bind](#bind)
    * [listen](#listen)
    * [accept](#accept)
    * [Client](#client)
* [23.1.12](#23112)
  * [bash 쉘의 환경 변수](#bash-쉘의-환경-변수)
* [23.1.11](#23111)
  * [VirtualBox 설정](#virtualbox-설정)
    * [복사, 붙여넣기 설정](#복사-붙여넣기-설정)
    * [docker, docker-compose](#docker-docker-compose)
* [23.1.10](#23110)
  * [env file](#env-file)
* [23.1.9](#2319)
  * [volume](#volume)
  * [bind mount](#bind-mount)
* [23.1.7](#2317)
  * [docker-compose volume configuration reference](#docker-compose-volume-configuration-reference)
    * [driver](#driver)
    * [driver\_opts](#driver_opts)
* [23.1.5](#2315)
  * [docker-compose depends\_on](#docker-compose-depends_on)
* [23.1.4](#2314)
  * [php extentions](#php-extentions)
  * [wordpress cli](#wordpress-cli)
* [23.1.3](#2313)
  * [wordpress 설정파일](#wordpress-설정파일)

---

## 23.1.24

### State diagram for server and client model

![](/TIL/docs/src/socketProgramming.png)  

### Server

- Creating socket file descriptor

```c
if ((server_fd = socket(AF_INET, SOCK_STREAM, 0)) < 0) {
  perror("socket failed");
  exit(EXIT_FAILURE);
}

// - int socket(int domain, int type, int protocol);
// - AF_INET은 해당 소켓을 IP version 4로 사용
// - SOCK_STREAM은 해당 소켓에 TCP 패킷을 받음
```

- Forcefully attaching socket to the port 8080

```c
if (setsockopt(server_fd, SOL_SOCKET, SO_REUSEPORT, &opt, sizeof(opt))) {
  perror("setsockopt");
  exit(EXIT_FAILURE);
}

// - int setsockopt(int sockfd, int level, int optname, const void *optval, socklen_t optlen);
// - SOL_SOCKET : 일반적인 설정에 관련된 레벨
// - SO_REUSEPORT : 동일한 포트에 여러 리스너를 바인드 할 수 있도록 설정
```

[소켓 옵션 변경](https://velog.io/@minji/%EC%86%8C%EC%BC%93%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-%EC%86%8C%EC%BC%93%EC%9D%98-%EC%98%B5%EC%85%98sockopt), [소켓 옵션](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=kbm0996&logNo=221041496905), [SO_REUSEPORT](https://jacking75.github.io/network_linux_reuseport/)  

```c
struct sockaddr_in address;
address.sin_family = AF_INET;
address.sin_addr.s_addr = INADDR_ANY;
address.sin_port = htons(PORT);

// - sockaddr_in : sa_family가 AF_INET인 경우에 사용하는 소켓 주소 저장 구조체  
// - sin_addr : 호스트 IP 주소 (inet_aton() 혹은 inet_addr() 값)
// - sin_port : 포트 번호 (network byte order)
```

[소켓 주소 정보 구조체](https://techlog.gurucat.net/292)  

```c
if (bind(server_fd, (struct sockaddr*)&address, sizeof(address)) < 0) {
  perror("bind failed");
  exit(EXIT_FAILURE);
}

// - int bind(int sockfd, const struct sockaddr *addr, socklen_t addrlen);
// - bind : 소켓을 addr(사용자 지정 데이터 구조)에 지정된 주소 및 포트 번호에 바인딩
```

```c
if (listen(server_fd, 3) < 0) {
  perror("listen");
  exit(EXIT_FAILURE);
}

// - int listen(int sockfd, int backlog);
// - listen : 서버 소켓을 수동 모드로 전환하여 클라이언트가 서버에 접근할 때까지 기다림
```

```c
if ((new_socket = accept(server_fd, (struct sockaddr*)&address, (socklen_t*)&addrlen)) < 0) {
  perror("accept");
  exit(EXIT_FAILURE);
}

// - int accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen);
// - accept : listen 소켓에 대하여 대기 중인 연결 큐에서 첫 번째 연결을 추출하고 연결된 새 소켓을 만들어 해당 소켓을 참조하는 새 file descriptor를 반환
```

- Connected

```c
valread = read(new_socket, buffer, 1024);
printf("%s\n", buffer);
send(new_socket, hello, strlen(hello), 0);
printf("Hello message sent\n");

// - ssize_t send(int sockfd, const void *buf, size_t len, int flags);
// - 연결된 socket으로 상대 시스템에 데이터를 전송
```

- Closing the socket

```c
// closing the connected socket
close(new_socket);

// closing the listening socket
shutdown(server_fd, SHUT_RDWR);
```







## 23.1.23

### htons

htons 함수는 데이터를 네트워크 바이트 순서로 변환한다.  
데이터는 바이트 단위로 저장되지만 저장되는 방식에 따라서 cpu마다 차이가 발생된다.  
데이터를 저장할 때, 가장 낮은 바이트부터 저장을 하면 Little Endian 방식이고, 높은 바이트부터 저장을 하면 Big Endian 방식이라고 한다.  

서로 다른 데이터 저장 방식을 사용하는 시스템끼리 통신하면 문제가 발생된다. 전혀 다른 값을 주고 받을 수 있으므로 네트워크 byte order를 따르도록 데이터의 byte order를 변경한다. 데이터의 byte order는 Big Endian을 따른다.  

### inet_addr

Dotte-Decimal Notation 형식을 Big Endian 32비트 값으로 변환  

```c
unsigned long inet_addr(const char *string);

// 성공시 32비트 값 반환
// 실패시 INADDR_NONE 반환(-1)
```

### inet_ntoa

네트워크 바이트 순서의 32비트 값을 Dotte-Decimal Notation으로 변환

```c
char * inet_ntoa(struct in_addr addr);

// 성공시 문자열의 포인터 반환
// 실패시 -1 반환
```


## 23.1.19

### socket

서버와 클라이언트가 통신을 하기 위해서 소켓 라이브러리를 사용한다. socket 함수는 소켓을 생성하여 반환한다.  

socket 함수는 통신을 위한 endpoint를 만들고 endpoint를 참조하는 file descriptor를 반환한다.  

```c
#include <sys/socket.h>

int socket(int domain, int type, int protocol);
```

성공적이라면 현재 열리지 않은 fd의 가장 낮은 번호를 반환한다. 이 fd는 생성된 소켓을 가리키는 소켓 디스크립터이다. 만약에 생성에 실패했다면 -1을 반환한다. 

- domain : 통신 도메인을 지정한다. 이는 통신에 사용될 프로토콜 제품군을 선택한다. 제품군은 <sys/socket.h>에 정의되어 있다. (AF_UNIX(프로토콜 내부에서), AF_INET(IPv4), AF_INET6(IPv6))  
- type : 사용할 프로토콜을 지정한다. (SOCK_STREAM(TCP), SOCK_DGRAM(UDP), SOCK_RAW(사용자 정의))
- protocol : 프로토콜의 값을 결정한다. (IPPROTO_TCP(TCP 일때), IPPROTO_UDP(UDP 일때))


참조 : [socket man page](https://man7.org/linux/man-pages/man2/socket.2.html), [c언어 소켓 생성 함수 socket()](https://badayak.com/entry/C%EC%96%B8%EC%96%B4-%EC%86%8C%EC%BC%93-%EC%83%9D%EC%84%B1-%ED%95%A8%EC%88%98-socket)

### socket programming

https://www.geeksforgeeks.org/socket-programming-cc/  

소켓 프로그래밍은 네트워크의 두 노드를 연결하여 서로 통신하는 방법입니다. 하나의 소켓(노드)은 IP의 특정 포트에서 수신 대기하고 다른 소켓은 연결을 형성하기 위해 다른 소켓에 도달합니다. 클라이언트가 서버에 도달하는 동안 서버는 리스너 소켓을 형성합니다.  

#### socket

```c
int sockfd = socket(domain, type, protocol)
```

- sockfd : socket descriptor, 정수이다. 
- domain : integer, 통신 도메인을 지정한다. 동일한 호스트의 프로세스 간 통신을 위해 POSIX 표준에 정의된 대로 AF_LOCAL을 사용한다.  
- type : 통신 방식
  - SOCK_STREAM: TCP(reliable, connection oriented)
  - SOCK_DGRAM: UDP(unreliable, connectionless)
- protocol : 인터넷 프로토콜(IP)에 대한 프로토콜 값으로 0이다. 패킷의 IP 헤더에 있는 protocol 필드에 나타나는 것과 동일한 숫자이다.(자세한 내용은 man protocols 참조)

#### setsockopt

fd인 sockfd가 참조하는 소켓에 대한 옵션을 조작한다. 주소와 포트를 재사용하는데 도움이 된다. "이미 사용 중인 주소"와 같은 오류를 방지한다.  

```c
int setsockopt(int sockfd, int level, int optname, const void *optval, socklen_t optlen);
```

#### bind

소켓 생성 후 bind 함수는 소켓을 addr(사용자 지정 데이터 구조)에 지정된 주소 및 포트 번호에 바인딩한다.  

```c
int bind(int sockfd, const struct sockaddr *addr, socklen_t addrlen);
```  

#### listen

서버 소켓을 수동 모드로 전환하여 클라이언트가 연결을 위해 서버에 접근할 때까지 기다린다. backlog는 sockfd에 대한 대기 중인 연결 대기열이 커질 수 있는 최대 길이를 정의한다. 대기열이 가득 찼을 때 연결 요청이 도착하면 클라이언트는 ECONNREFUSED 표시와 함께 오류를 수신할 수 있다.  

```c
int listen(int sockfd, int backlog);
```

#### accept

listen 소켓인 sockfd에 대하여 대기 중인 연결 큐에서 첫 번째 연결 요청을 추출하고 연결된 새 소켓을 만들고 해당 소켓을 참조하는 새 fd를 반환한다. 이 시점에서 클라이언트와 서버 간에 연결이 설정되고 데이터를 전송할 준비가 된다. 

```c
int new_socket = accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen);
```

#### Client

- Socket connection : 서버의 소켓 생성과 같다.
- Connect : connect 시스템 호출은 sockfd가 참조하는 소켓을 addr가 지정한 주소에 연결한다. 서버의 주소와 포트는 addr에 지정된다.  

```c
int connect(int sockfd, const struct sockaddr *addr, socklen_t addrlen);
```





## 23.1.12

### bash 쉘의 환경 변수

bash 쉘에는 전역 변수와 지역 변수가 있다. 전역 환경 변수는 쉘 세션 및 그로부터 파생된 자식 서브쉘에서 볼 수 있다. 하지만 지역 변수는 이를 만든 쉘에서만 사용할 수 있다.  

https://aroundck.tistory.com/6920  

## 23.1.11

### VirtualBox 설정

#### 복사, 붙여넣기 설정

[Install the VirtualBox Guest Additions in Debian 11 bullseye](https://www.pragmaticlinux.com/2021/09/install-the-virtualbox-guest-additions-in-debian-11-bullseye/)  
[Unable to insert the virtual optical disk 해결 방법](https://kldp.org/node/156897)  

shared folder를 설정하는 도중에 `usermod: command not found`가 발생되면 `/usr/sbin/usermod`를 사용하면 된다.  

#### docker, docker-compose 

[Install docker-compose](https://developer-eun-diary.tistory.com/138)  



## 23.1.10

### env file

https://seongjin.me/environment-variables-in-docker-compose/  

환경변수를 별도의 파일로 구성할 수 있다. 이는 간단하게 docker-compose.yaml 파일이 위치한 경로에 `.env` 파일을 추가하면 된다. `.env`는 다음과 같은 문법을 따라서 작성해야 한다.  

- `변수명=값` 형태로 한 줄씩 작성
- `#` 문자는 주석 
- 빈 줄은 무시된다.
- 따옴표 처리는 불필요

`.env` 파일은 다른 설정없이 자동으로 docker-compose 파일에 반영된다.  

```
# .env
MYSQL_PASSWORD=1234
```

```
# docker-compose.yaml
version: '3'
services:
  mysql:
    image: mysql
    environment:
      MYSQL_PASSWORD: ${MYSQL_PASSWORD}
```

`.env` 파일을 이용하는 방식은 간편하지만 `docker-compose up` 명령어를 수행하는 경우에만 적용된다.  



## 23.1.9

https://medium.com/dtevangelist/docker-%EA%B8%B0%EB%B3%B8-5-8-volume%EC%9D%84-%ED%99%9C%EC%9A%A9%ED%95%9C-data-%EA%B4%80%EB%A6%AC-9a9ac1db978c  

### volume

Docker가 volume을 생성하고 관리하는 방식이다. docker volume create 명령으로 volume을 생성하거나, container 생성 중에 volume을 생성할 수 있다.  

- volume이 생성되면 host의 디렉토리에 저장된다. 
- 생성된 volume을 container에 mount하면, host의 디렉토리가 mount된다.
- volume은 Docker에 의해 관리되며, host 시스템과 분리된다.
- docker volume 명령어로 볼륨을 생성하거나 제거할 수 있다. 

### bind mount

host 시스템의 파일 또는 디렉토리가 container에 mount된다. 

- 파일 또는 디렉토리가 host 시스템의 전체 경로로 참조된다.
- 만약에 파일 또는 디렉토리가 존재하지 않는 경우에는 참조된 경로로 생성된다. 
- bind mount는 host의 파일 시스템 디렉토리 구조에 의존적이다.
- Docker CLI 명령어로 관리할 수 없다.

> bind mount는 container에서 실행 중인 프로세스가 host 파일 시스템의 중요한 시스템 파일이나 디렉토리를 생성, 수정, 삭제 명령을 통해 변경할 수 있다. 이는 host 시스템에서 Non-Docker 프로세스와 충돌을 일으키거나, 보안에 큰 영향을 줄 수 있다. 그러므로 bind mount보다 volume을 사용해야 한다.  




## 23.1.7

### docker-compose volume configuration reference

https://docs.docker.com/compose/compose-file/compose-file-v3/#volume-configuration-reference  

서비스 선언의 일부로 볼륨을 선언할 수 있지만, 여러 서비스에서 재사용할 수 있고, docker 명령을 사용하여 쉽게 검색할 수 있는 named 볼륨을 생성할 수 있다. 다음은 데이터 디렉터리를 볼륨으로 두개의 서비스가 공유하는 설정의 예이다.  

```
version: "3.9"

services:
  db:
    image: db
    volumes:
      - data-volume:/var/lib/db
  backup:
    image: backup-service
    volumes:
      - data-volume:/var/lib/backup/data

volumes:
  data-volume:
```

볼륨 키 아래의 항목은 비어있을 수 있으며, 이 경우 기본 드라이버를 사용한다. 선택적으로 다음 키를 사용하여 볼륨을 구성할 수 있다.  

#### driver

볼륨에 사용할 볼륨 드라이버를 지정한다. 기본적으로 Docker 엔진이 사용하도록 구성된 모든 드라이버가 사용되며 대부분의 경우 로컬이다. 드라이버를 사용할 수 없는 경우 엔진이 오류를 반환한다.

```
driver: foobar
```

#### driver_opts

볼륨의 드라이버에 전달할 키-값 쌍으로 옵션 목록을 지정한다. 옵션은 드라이버에 따라 다르다. 자세한 내용은 드라이버 설명서를 참조하면 된다.  

```
volumes:
  example:
    driver_opts:
      type: "nfs"
      o: "addr=10.40.0.199,nolock,soft,rw"
      device: ":/docker/example"
```


## 23.1.5

### docker-compose depends_on

[Docker Compose를 사용한 여러 컨테이너의 구성 관리](https://nomad-programmer.tistory.com/317), [Docker Compose에서 컨테이너 startup 순서 컨트롤하기](https://jupiny.com/2016/11/13/conrtrol-container-startup-order-in-compose/)  

depends_on은 서비스간의 의존관계를 지정하여 순서대로 서비스를 시작하기 위해 사용된다. 하지만 depends_on은 순서만 제어할 뿐, 컨테이너의 어플리케이션이 이용 가능할 때까지 기다리지 않는다. 그렇기 때문에 이에 대한 조치가 필요하다면 대책이 필요하다. 만약에 다른 서비스가 실행 가능한 상태가 될 때까지 기다리길 원한하면 dockerize라는 툴을 사용하면 된다.  


## 23.1.4

### php extentions

https://make.wordpress.org/hosting/handbook/server-environment/#php-extensions  

### wordpress cli

[wp-cli 공식 페이지](https://wp-cli.org/)  
[wp-cli wordpress 페이지](https://make.wordpress.org/cli/handbook/guides/installing)

https://www.lesstif.com/system-admin/wp-cli-22643947.html  

```
curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar
chmod +x wp-cli.phar
mv wp-cli.phar /usr/local/bin/wp

cd /var/www/html
wp core download --locale=ko_KR
wp core install --url=$WP_URL  --title=$WP_TITLE --admin_user=$WP_ADMIN_USER --admin_password=$WP_ADMIN_PWD --admin_email=$WP_ADMIN_EMAIL
```


## 23.1.3

### wordpress 설정파일

wordpress의 설정 파일은 wp-config.php이다. 그러나 wordpress를 설치한 직후에는 파일이 존재하지 않고, wp-config-sample.php 파일을 제공한다. 이를 복사하여 wp-config.php 파일을 만들고, 파일을 수정하면 된다. 수정할 사항은 다음과 같다.   

- Database 정보 값 수정
- Auth key, salts 값 입력

```
# wp-config.php 생성
cp -p /var/www/html/wordpress/wp-config-sample.php /var/www/html/wordpress/wp-config.php
```

```
# Database 정보 값 수정
sed -i "s|define( 'DB_NAME', 'database_name_here' );|define( 'DB_NAME', '$DB_NAME' );|g" /var/www/html/wp-config.php
sed -i "s|define( 'DB_USER', 'username_here' );|define( 'DB_USER', '$DB_USER' );|g" /var/www/html/wp-config.php
sed -i "s|define( 'DB_PASSWORD', 'password_here' );|define( 'DB_PASSWORD', '$DB_PWD' );|g" /var/www/html/wp-config.php
sed -i "s|define( 'DB_HOST', 'localhost' );|define( 'DB_HOST', '$DB_HOST' );|g" /var/www/html/wp-config.php
```

```
# wordpress 랜덤 문자열 파일 생성
curl https://api.wordpress.org/secret-key/1.1/salt/ > salt
```

salt 파일의 문자열을 sed로 치환하기 어렵다. sed 명령어 치환 문자열의 구분자를 정하기 어렵기 때문이다. salt 문자열에 없는 문자를 구분자로 사용해야 하지만 거의 모든 문자를 사용하여 문자열을 만들어 준다. [sed: bad option in substitution expression](https://superuser.com/questions/1015234/sed-command-returning-sed-bad-option-in-substitution-expression)  

그러므로 wp-config.php 파일을 미리 생성하여 /var/www/html/wordpress에 복사하는 방법을 사용하기로 한다.  

참조 : [wordpress wp-config.php](https://swiftcoding.org/wp-config-file)  
