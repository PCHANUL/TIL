---
layout: default
title: Today I Learned
nav_order: 1
has_children: true
child_nav_order: desc
permalink: /
---

# Today I Learned <!-- omit in toc -->

- [22.12.20](#221220)
  - [nginx.conf](#nginxconf)
    - [event 블록](#event-블록)
    - [http 블록](#http-블록)
  - [nginx 사용자별 가상 호스트 도메인 기본 설정 방법](#nginx-사용자별-가상-호스트-도메인-기본-설정-방법)
    - [가상 호스트 루트 디렉토리 생성과 퍼미션](#가상-호스트-루트-디렉토리-생성과-퍼미션)
    - [가상 호스트 추가](#가상-호스트-추가)
    - [IP 및 기타 도메인 접근 불가 설정](#ip-및-기타-도메인-접근-불가-설정)
- [22.12.19](#221219)
  - [Install Nginx](#install-nginx)
  - [nginx.conf](#nginxconf-1)
  - [Nginx systemd](#nginx-systemd)
  - [Nginx SSL](#nginx-ssl)
    - [openssl 인증서 발급](#openssl-인증서-발급)
- [22.12.18](#221218)
  - [Dockerfile ENTRYPOINT와 CMD의 차이](#dockerfile-entrypoint와-cmd의-차이)
  - [nginx](#nginx)
    - [nginx 구조](#nginx-구조)
- [22.12.16](#221216)
  - [mysql 원격 접속](#mysql-원격-접속)
    - [서버에서 로컬 접속만 허용](#서버에서-로컬-접속만-허용)
- [22.12.15](#221215)
  - [mysqld\_safe 실행 오류](#mysqld_safe-실행-오류)
    - [chown 명령어](#chown-명령어)
  - [데몬 기초 : 개념과 구현 방법](#데몬-기초--개념과-구현-방법)
  - [Docker Compose volume](#docker-compose-volume)
- [22.12.14](#221214)
  - [Docker Compose network](#docker-compose-network)
    - [사용자 정의 네트워크 지정](#사용자-정의-네트워크-지정)
    - [Docker Compose service ports](#docker-compose-service-ports)
    - [Docker Compose service expose](#docker-compose-service-expose)
  - [mysql\_install\_db](#mysql_install_db)
    - [Options](#options)
  - [mysqld\_safe](#mysqld_safe)
    - [Options](#options-1)
- [22.12.13](#221213)
  - [docker network](#docker-network)
    - [네트워크 조회](#네트워크-조회)
    - [네트워크 종류](#네트워크-종류)
    - [네트워크 생성](#네트워크-생성)
    - [네트워크 상세 정보](#네트워크-상세-정보)
    - [네트워크에 컨테이너 연결](#네트워크에-컨테이너-연결)
- [22.12.12](#221212)
  - [docker rmi 사용법](#docker-rmi-사용법)
  - [Can't connect to local MySQL server through socket '/var/run/mysqld/mysqld.sock' 에러](#cant-connect-to-local-mysql-server-through-socket-varrunmysqldmysqldsock-에러)
- [22.12.11](#221211)
  - [daemon](#daemon)
- [22.12.09](#221209)
  - [Docker Container PID 1](#docker-container-pid-1)
    - [Solution 1 : PID 1으로 실행하고 신호 핸들러로 등록](#solution-1--pid-1으로-실행하고-신호-핸들러로-등록)
    - [Solution 2 : Kubernetes에서 프로세스 네임 스페이스 공유 사용 설정](#solution-2--kubernetes에서-프로세스-네임-스페이스-공유-사용-설정)
    - [Solution 3 : 특수한 init 시스템 사용](#solution-3--특수한-init-시스템-사용)
  - [컨테이너 환경을 위한 초기화 시스템](#컨테이너-환경을-위한-초기화-시스템)
    - [컨테이너 내부에서의 프로세스 동작](#컨테이너-내부에서의-프로세스-동작)
      - [PID 1의 Signal 문제](#pid-1의-signal-문제)
    - [dumb-init](#dumb-init)
    - [dumb-init 사용법](#dumb-init-사용법)
- [22.12.08](#221208)
  - [Docker Container 백그라운드 실행](#docker-container-백그라운드-실행)
  - [컨테이너 환경을 위한 초기화 시스템](#컨테이너-환경을-위한-초기화-시스템-1)
    - [PID 1](#pid-1)
  - [PID 1, 신호 처리, 좀비 프로세스 올바르게 처리하기](#pid-1-신호-처리-좀비-프로세스-올바르게-처리하기)
- [22.12.07](#221207)
  - [MariaDB Dockerfile](#mariadb-dockerfile)
- [22.12.06](#221206)
  - [container의 OS, Virtual Machine의 OS](#container의-os-virtual-machine의-os)
  - [MariaDB](#mariadb)
- [22.12.05](#221205)
  - [The Compose application model](#the-compose-application-model)
    - [example](#example)
- [22.12.04](#221204)
  - [Docker file](#docker-file)
- [22.12.03](#221203)
  - [Docker Compose](#docker-compose)
  - [Key features of Docker Compose](#key-features-of-docker-compose)
    - [단일 호스트에 여러 개의 격리된 환경](#단일-호스트에-여러-개의-격리된-환경)
    - [컨테이너 생성 시 볼륨 데이터 보존](#컨테이너-생성-시-볼륨-데이터-보존)
    - [변경된 컨테이너만 재생성](#변경된-컨테이너만-재생성)
    - [변수 지원 및 환경 간 컴포지션 이동](#변수-지원-및-환경-간-컴포지션-이동)
- [22.12.02](#221202)
  - [Docker](#docker)
    - [Docker Architecture](#docker-architecture)
  - [alpine linux](#alpine-linux)
- [22.12.01](#221201)
  - [Container](#container)
    - [Container란](#container란)
    - [Container Image란](#container-image란)
    - [Kernel Space](#kernel-space)
      - [chroot](#chroot)
      - [Linux namespace](#linux-namespace)
      - [namespace API](#namespace-api)
        - [clone](#clone)
        - [unshare](#unshare)
        - [setns](#setns)
        - [proc](#proc)
      - [namespace](#namespace)
        - [mnt](#mnt)
        - [uts](#uts)
        - [ipc](#ipc)
        - [pid](#pid)
        - [net](#net)
        - [user](#user)
        - [cgroup](#cgroup)

---
## 22.12.20

### nginx.conf 

#### event 블록

- worker_connections : 하나의 프로세스가 처리할 수 있는 커넥션의 수이다.  

#### http 블록

- default_type : 옥텟 스트림 기반의 http를 사용한다.
- server_tokens : 헤더에 nginx버전을 숨기는 기능이다.
- keepalive_timeout : 접속시 커넥션 유지 시간을 지정한다. 값이 높으면 불필요한 커넥션을 유지하기 때문에 낮은 값으로 설정하는 것이 좋다.  
- access_log : 접속 로그를 저장한다. http 블록에서 로그를 저장하면 관리가 불편하기 때문에 각 가상 호스트마다 로그를 배분하는 것이 좋다.
- include : 옵션 항목을 설정해둔 파일의 경로를 지정한다. 가상 호스트 설정이나 반복되는 옵션 항목을 불러오는 방식으로 활용한다. 예를 들어서, 리버스 프록시를 각 도메인에 설정할 때 헤더 처리같은 옵션을 include로 깔끔하게 처리할 수 있다. 기본적으로 conf.d 디렉토리에 .conf 파일로 설정을 저장하여 관리한다.

- upstream 블록 : origin 서버를 지정하는데 사용된다. origin 서버는 WAS, 웹 어플리케이션 서버를 의미하며, nginx는 downstream에 해당된다. 
  - server : 연결할 웹 어플리케이션 서버의 주소와 포트를 지정한다.
- server 블록 : 하나의 웹 사이트를 선언하는데 사용된다. 만약에 server 블록이 여러 개이면 한 개의 머신에서 여러 웹 사이트를 서빙할 수 있다. 
  - listen : 웹 사이트가 바라보는 포트이다.
  - server_name : 클라이언트가 접속하는 서버이다. 실제로 들어온 request의 header에 명시된 값이 일치하는지 확인해서 server를 분기한다.
- location 블록 : server 블록 안에서 특정 웹 사이트의 url를 처리하는데 사용된다.


### nginx 사용자별 가상 호스트 도메인 기본 설정 방법

https://extrememanual.net/10008  

하나의 IP로 웹서버에서 여러 도메인을 연결하여 서비스하기 위해서는 가상 호스트를 설정해야 한다.  
nginx의 설정 파일인 nginx.conf 파일에 가상 호스트 설정을 추가할 수 있다. 가상 호스트 설정은 가독성을 위해 별도의 파일로 만들어서 include 하는 것이 좋다. 가상화 호스트를 관리하기 쉽게 다음과 같은 구문을 추가해준다.  

```
http {
  include /etc/nginx/sites-enabled/*;
} 
```

위와 같은 구문을 추가한 후, 가상 호스트 설정 파일이 위치한 /etc/nginx/sites-available 에서 /etc/nginx/sites-enabled/* 로 심볼릭 링크를 걸어주면 사용하는 가상 호스트와 사용하지 않는 가상 호스트를 관리할 수 있다.  

#### 가상 호스트 루트 디렉토리 생성과 퍼미션

```
# mkdir /home/계정명/www
# chown -R www-data:www-data /home/계정명/www
# usermod -G www-data 계정명
```

웹 문서가 게시되고, 로그 파일이 위치할 디렉토리를 생성한다. 홈페이지를 구성할 때 자동으로 생성되는 설정이나 캐시같은 파일이 www-data 계정 권한으로 동작한다. 디렉토리 권한이 없다면 루트 디렉토리의 권한을 변경하거나 사용자가 FTP, SSH로 파일을 다룰 때에 권한 문제가 발생된다. 그러므로 웹서버에서 사용할 계정을 www-data 그룹에 추가해야 한다.  

#### 가상 호스트 추가

```
server {
    listen 80;
    root /home/계정명/www;
    server_name domain.com www.domain.com;
    index index.html index.htm index.php;
    access_log /var/log/nginx/$host-access.log main;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.1-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }
}
```

가상 호스트에 적용될 도메인 주소와 루트 디렉토리, 로그, PHP 구동 설정이 담긴 설정 파일을 만든다. 이 파일은 /etc/nginx/sites-available 디렉토리 안에 위치 시킨다. 그 다음 설정 파일을 /etc/nginx/sites-enabled 디렉토리에 심볼릭 링크를 걸어서 nginx가 설정을 사용할 수 있게 한다.  

```
ln -s /etc/nginx/sites-available/가상호스트파일명 /etc/nginx/sites-enabled/가상호스트파일명
```


#### IP 및 기타 도메인 접근 불가 설정

```
server {
    listen 80 default_server;
    listen 443 default_server;
    server_name _;
    return 403;
}
```

위와 같이 기본 가상 호스트 설정 파일을 만든다. 가상 호스트 도메인을 명시하지 않으면 server_name _; 구문으로 인해서 웹서버 IP에 연결되어 있는 모든 도메인 및 IP로 접근하는 메인 페이지가 된다. 가상 호스트로 명시하지 않은 도메인에 접근하지 못하도록 설정하고 싶은 경우에 기본 호스트 설정 파일을 위와 같이 수정한다.  







## 22.12.19

### Install Nginx

https://wiki.alpinelinux.org/wiki/Nginx  

### nginx.conf

https://wonit.tistory.com/335, https://prohannah.tistory.com/136  

최소의 nginx 설정 파일은 다음과 같다.  

```
# /etc/nginx/nginx.conf                                                           
                                                                                   
user www;                                                                          
                                                                                   
# CPU 코어 수에 따라 자동으로 작업자 프로세스 수를 설정합니다.  
worker_processes auto;                                                            
                                                                                  
# 정규 표현식에 JIT를 사용하여 처리 속도를 높입니다.  
pcre_jit on;                                                                      
                                                                                   
# 기본 오류 로그를 구성합니다.
error_log /var/log/nginx/error.log warn;

# 동적 모듈을 로드하기 위한 지시문이 있는 파일을 포함합니다.
include /etc/nginx/modules/*.conf;                                                 
                                                                                   
# 구성 스니펫이 있는 파일을 루트 컨텍스트에 포함하려면 주석을 제거하십시오.
# 참고: 이것은 Alpine 3.15에서 기본적으로 활성화됩니다.                           
#include /etc/nginx/conf.d/*.conf;                                                
                                                                                   
events {                                                                          
        # 작업자 프로세스에서 열 수 있는 최대 동시 연결 수입니다.
        worker_connections 1024;                                                   
}                                                                                  
                                                                                  
http {                                                                             
        # 응답의 MIME 유형에 대한 파일 이름 확장명 매핑을 포함하고 기본 유형을 정의합니다.
        include /etc/nginx/mime.types;                                             
        default_type application/octet-stream;                                     
                                                                                   
        # 업스트림 서버의 이름을 주소로 확인하는 데 사용되는 이름 서버.
        # Lua 모듈에서 tcpsocket 및 udpsocket을 사용할 때도 필요합니다.
        #resolver 1.1.1.1 1.0.0.1 2606:4700:4700::1111 2606:4700:4700::1001;       
                                                                                   
        # nginx 버전을 클라이언트에 알리지 않습니다. 기본값은 '켜짐'입니다.
        server_tokens off;                                                         
                                                                                   
        # 요청 헤더 Content-Length에 표시된 대로 클라이언트 요청의 최대 허용 본문 크기를 지정합니다. 명시된 콘텐츠 길이가 이 크기보다 크면 클라이언트는 HTTP 오류 코드 413을 수신합니다. 비활성화하려면 0으로 설정하십시오. 기본값은 '1m'입니다.                    
        client_max_body_size 1m;                                                   
                                                                                   
        # Sendfile은 커널 내에서 하나의 FD와 다른 FD 간에 데이터를 복사하며 이는 read() + write()보다 효율적입니다. 기본값은 꺼져 있습니다.           
        sendfile on;                                                               
                                                                                   
        # nginx가 부분 프레임을 사용하는 대신 하나의 패킷으로 HTTP 응답 헤드를 보내도록 합니다. 기본값은 '꺼짐'입니다.                       
        tcp_nopush on;                                                             
                                                                    
        # 지정된 프로토콜을 활성화합니다. 기본값은 TLSv1 TLSv1.1 TLSv1.2입니다.
        # 팁: 고대 클라이언트를 지원할 의무가 없다면 TLSv1.1을 제거하십시오.
        ssl_protocols TLSv1.1 TLSv1.2 TLSv1.3;                                     
                                                                                   
        # EDH 암호에 대한 Diffie-Hellman 매개변수가 있는 파일의 경로입니다.
        # 팁: 다음을 사용하여 생성: `openssl dhparam -out /etc/ssl/nginx/dh2048.pem 2048`
        #ssl_dhparam /etc/ssl/nginx/dh2048.pem;
        
        # 우리의 암호 슈트가 클라이언트 암호보다 선호되어야 함을 지정합니다. 기본값은 '꺼짐'입니다.                                                     
        ssl_prefer_server_ciphers on;                                           
                                                                                   
        # 약 8000개의 세션을 저장할 수 있는 크기의 공유 SSL 캐시를 활성화합니다. 기본값은 'none'입니다.
        ssl_session_cache shared:SSL:2m;                                          
                                                                                   
        # 클라이언트가 세션 매개변수를 재사용할 수 있는 시간을 지정합니다. 기본값은 '5m'입니다.
        ssl_session_timeout 1h;                                                    
                                                                                  
        # TLS 세션 티켓을 비활성화합니다(보안되지 않음). 기본값은 '켜짐'입니다.
        ssl_session_tickets off;                                                   
                                                                
        # 응답의 gzipping을 활성화합니다.
        #gzip on;                                                                  
                                                                                   
        # RFC 2616에 정의된 대로 Vary HTTP 헤더를 설정합니다. 기본값은 'off'입니다.
        gzip_vary on;                                                              
                                                                                   
                                                                                   
        # websocket을 프록싱하기 위한 도우미 변수입니다.
        map $http_upgrade $connection_upgrade {                                    
                default upgrade;                                                   
                '' close;                                                          
        }                                                                          
                                                                                   
                                                                                   
        # 기본 로그 형식을 지정합니다.
        log_format main '$remote_addr - $remote_user [$time_local] "$request" '    
                        '$status $body_bytes_sent "$http_referer" '                
                        '"$http_user_agent" "$http_x_forwarded_for"';              
                                                                                   
        # 버퍼링된 로그 쓰기에 대한 경로, 형식 및 구성을 설정합니다.
        access_log /var/log/nginx/access.log main;                                 
                                                                                   
                                                                                   
        # 가상 호스트 구성을 포함합니다.
        include /etc/nginx/http.d/*.conf;                                          
}                                                                                  
                                                                                   
# 팁: 스트림 모듈을 사용하는 경우 주석을 제거하십시오.
#include /etc/nginx/stream.conf;
```

### Nginx systemd

https://www.nginx.com/resources/wiki/start/topics/examples/systemd/  

```
[Unit]
Description=The NGINX HTTP and reverse proxy server
After=syslog.target network-online.target remote-fs.target nss-lookup.target
Wants=network-online.target

[Service]
Type=forking
PIDFile=/run/nginx.pid
ExecStartPre=/usr/sbin/nginx -t
ExecStart=/usr/sbin/nginx
ExecReload=/usr/sbin/nginx -s reload
ExecStop=/bin/kill -s QUIT $MAINPID
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

### Nginx SSL 

- certbot : https://geko.cloud/en/nginx-letsencrypt-certbot-docker-alpine/
- openssl : https://couplewith.tistory.com/entry/SSLTLS%EC%9D%B8%EC%A6%9D%EC%9D%84-%EC%9C%84%ED%95%9C-OpenSSL%EA%B3%BC-CA-%EC%9D%B8%EC%A6%9D%EC%84%9C-%EB%A7%8C%EB%93%A4%EA%B8%B0  

SSL(Secure Socket Layer) / TLS(Transport Layer Security)는 네트워크 전송 계층의 암호화를 통해 통신하는 규약이다. 통신을 위해서는 상호간의 인증서를 통해서 데이터를 암/복호화 할 수 있다. 웹 서비스와 같이 불특정 다수의 사용자를 위한 서비스는 공인 SSL 인증서를 발급받아 웹서버에 적용하고, 특정 서비스나 지정된 IP간의 통신은 사설 인증서를 만들어서 적용한다.  

#### openssl 인증서 발급

https://setyourmindpark.github.io/2017/05/04/nginx/nginx-1/  

1. 개인 키와 인증 요청서 생성

```
$ openssl req -new -newkey rsa:2048 -nodes -keyout <개인키이름>.key -out <인증요청서이름>.csr
```

2. 인증서 생성

```
$ openssl x509 -req -days 365 -in <인증요청서이름>.csr -signkey <개인키이름>.key -out <생성할인증서이름>.crt
```

3. 개인 키의 비밀번호 제거

```
$ cp <생성된개인키이름>.key <생성할개인키복사본이름>.key.secure
$ openssl rsa -in <생성된개인키복사본이름>.key.secure -out <재생성할개인키이름>.key
```

4. ssl 적용

```
$ vi /etc/nginx/conf.d/<서비스명>.conf
```

```
# Load Balancing
upstream target-server {
  least_conn;
  server 10.10.200.3:4000 max_fails=3 fail_timeout=10s;
  server 10.10.200.4:4000 max_fails=3 fail_timeout=10s;
}
server {
        listen 443;
        server_name 10.10.200.2;
        charset utf-8;
        access_log /etc/nginx/log/access.log;
        error_log /etc/nginx/log/error.log;
        ssl     on;                                               #ssl사용
        ssl_certificate /etc/nginx/ssl/jaehunpark-ssl.crt;        #생성된 인증서경로
        ssl_certificate_key /etc/nginx/ssl/jaehunpark-ssl.key;    #생성된 개인키
        location / {
                proxy_redirect  off;
                proxy_set_header Host $http_host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Scheme $scheme;
                proxy_pass http://target-server;
        }
}
```


## 22.12.18

### Dockerfile ENTRYPOINT와 CMD의 차이

https://www.bmc.com/blogs/docker-cmd-vs-entrypoint/  

### nginx

nginx는 웹 서버 소프트웨어이다. 비동기 이벤트 기반으로 트래픽이 많은 웹사이트의 서버를 도와준다.  

웹 서버인 아파치 서버는 커넥션을 형성하기 위해 프로세스를 생성한다. 확장성이 좋다는 장점을 활용하여 하나의 서버에서 요청을 받고 응답을 처리할 수 있었다. 하지만 서버 트래픽량이 높아지며 서버에 동시 연결된 커넥션을 감당하지 못하는 문제가 발생된다. 이를 Connection 10000 Problem (C10K problem) 이라고 한다. 수많은 동시 커넥션을 감당할 수 없었던 아파치 서버는 nginx로 문제를 해결할 수 있었다.  

#### nginx 구조

nginx는 Event-Driven Model(이벤트 기반 구조)이다.  

nginx에는 master process가 있다. master process는 설정 파일을 읽고 worker process를 생성한다. worker process는 지정된 소켓을 배정받으며 생성된다. 소켓에 새로운 클라이언트의 요청이 들어오면 worker process는 connection을 형성하고 처리한다. connection은 정해진 시간만큼 유지되며, 형성된 connection에 아무런 요청이 없으면 새로운 connection을 형성하거나 다른 connection에 들어온 요청을 처리한다. connection 형성과 제거, 요청 처리를 event라고 부른다.  

OS 커널이 event를 큐 형식으로 worker process에 전달한다. 큐에 담긴 event는 비동기 방식으로 처리되기 전까지 대기한다. worker process는 하나의 스레드로 event를 꺼내어 처리한다. 만약에 꺼낸 event의 처리가 오래걸린다면 thread pool에 위임하고 다른 event를 처리한다.  

참조 : https://ssdragon.tistory.com/60  




## 22.12.16

### mysql 원격 접속

어떠한 컨테이너에 mysql 서버를 실행시키고, 다른 컨테이너에서 클라이언트로 접속을 시도하면 다음과 같은 문제를 만난다.  

#### 서버에서 로컬 접속만 허용

mysql은 설치시 기본으로 로컬 접근만 허용한다. host 목록을 보면 알 수 있다. 그렇기 때문에 외부에서 접근 가능한 새로운 유저를 생성해야 한다. 다음과 같은 명령어로 모든 IP에서 접속 가능한 유저가 생성된다.   

```
mysql> CREATE USER '[USER_NAME]'@'%' IDENTIFIED BY '[USER_PWD]';
mysql> GRANT ALL PRIVILEGES ON *.* TO '[USER_NAME]'@'%' WITH GRANT OPTION;
mysql> FLUSH PRIVILEGES;
```

다음은 mysql 설정 파일을 수정한다. my.cnf 파일에서 skip-networking이 있는 경우에 주석 처리를 해주어야 한다. skip-networking으로 mysql 서버가 로컬의 유닉스 소켓 접속만 혀용하도록 설정되기 때문이다.  

모든 설정 후에는 mysql 서버를 재시작하여 수정된 설정 사항을 적용시켜주어야 한다.  


참조 : https://velog.io/@wpdlzhf159/MySql-%EC%9B%90%EA%B2%A9%EC%A0%91%EC%86%8D-%ED%95%98%EA%B8%B0, https://jordy-torvalds.tistory.com/entry/%ED%8D%BC%EC%98%A8-%EA%B8%80-MySql-%EC%9B%90%EA%B2%A9-%EC%A0%91%EC%86%8D-%EB%B0%A9%EB%B2%95-1  


## 22.12.15

### mysqld_safe 실행 오류

mysqld_safe를 실행하면 로그 파일이 생성된다. 로그 파일에서 발견한 이번에 발생된 에러는 다음과 같다.  

```
Cannot open datafile for read-only: './mysql/gtid_slave_pos.ibd' OS error: 81  
```  

이 에러는 폴더 권한 문제로 인해 발생되었다. mysqld_safe를 실행하며 --datadir 옵션으로 지정된 폴더에 권한이 없는 사용자이기 때문에 파일을 읽을 수 없었다. 그래서 지정된 폴더와 모든 하위 폴더의 소유자를 변경하여 문제를 해결할 수 있었다.  

참조 : https://bbs.archlinux.org/viewtopic.php?id=247385, https://hmjkor.tistory.com/325  

#### chown 명령어

하위 폴더 소유자를 모두 변경하려면 -R 옵션을 추가한다.  

```
$ chown -R user:user folder
```  

참조 : https://codechacha.com/ko/linux-chown/  

### 데몬 기초 : 개념과 구현 방법

https://reakwon.tistory.com/118  


### Docker Compose volume

Docker 컨테이너가 종료되면 안에 저장된 데이터가 사라진다. 컨테이너에서 사용된 데이터를 보존하려면 호스트에 데이터를 저장해야 한다. 호스트에 데이터를 저장한다면 여러 컨테이너가 데이터를 공유할 수 있다. 호스트의 파일 시스템 안에 데이터를 저장하는 방식에 따라서 볼륨과 바인드로 나뉜다.  

1. bind mount : 컨테이너의 데이터를 호스트 경로에 연결
2. volume : 컨테이너의 데이터를 호스트의 /var/lib/volume 경로에 저장

참조 : https://nerd-mix.tistory.com/47, https://monkeydeveloper.tistory.com/entry/Docker-volume-compose, https://jjeong.tistory.com/1435


## 22.12.14

### Docker Compose network

Docker Compose는 여러 개의 컨테이너로 구성된 애플리케이션을 관리하기 위한 오케스트레이션 도구이다. 기본적으로 Docker Compose는 하나의 기본 네트워크에 모든 컨테이너를 연결한다. 기본 네트워크 이름은 docker-compose.yml 파일이 위치한 디렉토리 이름 뒤에 _default가 붙는다. `docker network ls` 명령어로 기본 네트워크를 확인할 수 있다.  

#### 사용자 정의 네트워크 지정

docker-compose.yml 파일에 networks 항목에 새로운 네트워크를 명시하여 컨테이너를 연결할 수 있다.  

```
services:
  web:
    build: .
    ports:
      - "8000:8000"
    networks:
      - new-net
  db:
    image: postgres
    ports:
      - "8001:5432"
networks:
  new-net:
    driver: bridge
```

#### Docker Compose service ports

service의 ports에 "host:container" 또는 "container"로 명시하여 호스트 포트와 컨테이너 포트를 구분하는 것이 중요하다. container 포트는 서비스 네트워크를 위해 사용되고, host 포트가 정의되면 외부에서도 서비스에 접근할 수 있다.  

#### Docker Compose service expose

expose는 host OS에 포트를 공개하지 않고, 컨테이너에만 포트를 공개한다. host OS와 직접 연결되지 않고, 링크 등으로 연결된 컨테이너 간의 통신이 필요한 경우에 사용된다.  

참조 : https://www.daleseo.com/docker-compose-networks/, https://docs.docker.com/compose/networking/, https://nirsa.tistory.com/80  

### mysql_install_db

mysql_install_db는 MariaDB 데이터 디렉토리를 초기화하고 시스템 테이블이 존재하지 않는 경우 mysql 데이터베이스에 시스템 테이블을 생성한다. MariaDB는 시스템 테이블을 사용하여 권한, 역할 및 플러그인을 관리한다.  

mysql_install_db는 --bootstrap 모드에서 MariaDB 서버의 mysqld 프로세스를 시작하고 명령을 전송하여 시스템 테이블과 그 내용을 생성하는 방식으로 작동한다.  

MariaDB 서버인 mysqld는 나중에 실행될 때 데이터 디렉터리에 액세스해야 하므로 mysqld를 실행하는 데 사용할 동일한 계정에서 mysql_install_db를 실행하고 --user 옵션을 사용하여 사용자를 지정해야 한다. 또한 --basedir 또는 --datadir과 같은 옵션으로 설치 디렉터리 또는 데이터 디렉터리의 위치를 지정할 수 있다.  

```
$ scripts/mysql_install_db --user=mysql \
   --basedir=/opt/mysql/mysql \
   --datadir=/opt/mysql/mysql/data
```  

#### Options

- `--auth-root-authentication-method={normal | socket}` : normal로 설정하면 mysql_native_password 인증 플러그인으로 인증하고 초기 비밀번호가 설정되지 않은 root@localhost 계정을 생성하므로 안전하지 않을 수 있다. socket으로 설정하면 unix_socket 인증 플러그인으로 인증하는 root@localhost 계정을 생성한다.  
- `--basedir=path` : MariaDB 설치 디렉터리의 경로이다.
- `--builddir=path` : 디렉터리 외부 빌드와 함께 --srcdir을 사용하는 경우 이를 빌드된 파일이 있는 빌드 디렉터리의 위치로 설정해야 한다.
- `--datadir=path`, `--ldata=path` : MariaDB 데이터 디렉터리의 경로이다.

참조 : https://mariadb.com/kb/en/mysql_install_db/  

### mysqld_safe

mysqld_safe는 몇 가지 추가 안전 기능으로 mysqld를 시작하는 래퍼이다. 예를 들어, mysqld_safe는 mysqld가 충돌했음을 알게 되면 mysqld_safe는 자동으로 mysqld를 다시 시작한다.  

mysqld_safe는 systemd를 지원하지 않는 Linux 및 Unix 배포판에서 mysqld를 시작하는데 권장되는 방법이다.  

mysqld_safe 명령어를 사용하는 구문은 다음과 같다.  

```
mysqld_safe [ --no-defaults | --defaults-file | --defaults-extra-file | --defaults-group-suffix | --print-defaults ] <options> <mysqld_options>
```  

#### Options

- `--basedir=path` : 	MariaDB 설치 디렉토리의 경로이다.
- `--datadir=path` : 데이터 디렉토리의 경로이다.
- `--user={user_name or user_id}` : 이름이 user_name이거나 숫자 사용자 ID인 user_id가 있는 사용자로 mysqld 서버를 실행한다.


참조 : https://runebook.dev/ko/docs/mariadb/mysqld_safe/index  




## 22.12.13

### docker network

컨테이너는 기본적으로 다른 컨테이너와의 통신이 불가능하다. 하지만 여러 컨테이너를 하나의 Docker Network에 연결시키면 통신이 가능해진다.  

#### 네트워크 조회

`docker network ls` 명령어를 사용하면 현재 생성되어 있는 네트워크 목록을 볼 수 있다. bridge, host, none은 Docker 데몬이 실행되며 생성된 네트워크이다.  

```bash
$ docker network ls
NETWORK ID     NAME                    DRIVER    SCOPE
0e5017a27ba9   bridge                  bridge    local
5e7e20144697   host                    host      local
07e13e6481ff   none                    null      local
```  

#### 네트워크 종류

네트워크는 bridge, host, overlay 등 목적에 따라 다양한 종류의 네트워크 드라이버를 지원한다.  

- bridge : 하나의 호스트 내에서 여러 컨테이너들의 네트워크
- host : 호스트와 동일한 네트워크를 컨테이너가 사용
- overlay : 여러 호스트의 컨테이너들 간의 네트워크

#### 네트워크 생성

`docker network create` 명령어를 사용하여 새로운 네트워크를 생성할 수 있다. 아래의 예시에는 -d 옵션을 사용하지 않아서 기본값인 bridge 네트워크가 생성된다.   

```
$ docker network create new-net
$ docker network ls
NETWORK ID     NAME                    DRIVER    SCOPE
0e5017a27ba9   bridge                  bridge    local
5e7e20144697   host                    host      local
77461af7ee7e   new-net                 bridge    local
07e13e6481ff   none                    null      local
```

#### 네트워크 상세 정보

`docker network inspect` 명령어러 네트워크의 상세 정보를 볼 수 있다. Container 값을 보면 어떠한 컨테이너도 연결되어 있지 않음을 알 수 있다.  

```
[
    {
        "Name": "new-net",
        "Id": "77461af7ee7e07d3454222a1b53e0148fdae10f88218a2f27b80b2a52f23fd00",
        "Created": "2022-12-13T14:11:47.193672325Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.20.0.0/16",
                    "Gateway": "172.20.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {},
        "Labels": {}
    }
]
```

#### 네트워크에 컨테이너 연결

`docker network connect` 명령어로 컨테이너를 네트워크에 연결할 수 있다. 컨테이너를 실행할 때 --network 옵션을 사용하지 않으면 기본값인 bridge 네트워크에 연결된다.  

```
$ docker run -itd --name two busybox
$ docker network inspect bridge

(...)
"Containers": {
            "4318dc9251f0f2c4be95056ab1c248491939202850601c569554850b85aae27c": {
                "Name": "two",
                "EndpointID": "5c171aa3eabb9f308926fe2f6a3271eaaf7dc0dedd482eb54939e7128f601380",
                "MacAddress": "02:42:ac:11:00:02",
                "IPv4Address": "172.17.0.2/16",
                "IPv6Address": ""
            }
        },
(...)
```

```
$ docker network connect new-net two
$ docker network inspect new-net

(...)
"Containers": {
            "4318dc9251f0f2c4be95056ab1c248491939202850601c569554850b85aae27c": {
                "Name": "two",
                "EndpointID": "4d89fd17ac05f188b73dada5dc96aacd975e6b6a8f2214c4c02cdec1c63d2411",
                "MacAddress": "02:42:ac:14:00:02",
                "IPv4Address": "172.20.0.2/16",
                "IPv6Address": ""
            }
        },
(...)
```

두번째 컨테이너도 연결해본다.  

```
$ docker run -itd --name four --network new-net busybox
$ docker network inspect new-net

(...)
"Containers": {
            "4318dc9251f0f2c4be95056ab1c248491939202850601c569554850b85aae27c": {
                "Name": "two",
                "EndpointID": "4d89fd17ac05f188b73dada5dc96aacd975e6b6a8f2214c4c02cdec1c63d2411",
                "MacAddress": "02:42:ac:14:00:02",
                "IPv4Address": "172.20.0.2/16",
                "IPv6Address": ""
            },
            "706849116fb8932986908a9a523315d28fa47c2a820ce50808bac209134c324b": {
                "Name": "four",
                "EndpointID": "4c3f080ca1b073ef960f8c3c2797eb524d9d594f4800e583bc46b4544601bd30",
                "MacAddress": "02:42:ac:14:00:03",
                "IPv4Address": "172.20.0.3/16",
                "IPv6Address": ""
            }
        },
(...)
```

참조 : https://www.daleseo.com/docker-networks/

## 22.12.12

### docker rmi 사용법

https://www.lainyzine.com/ko/article/docker-rmi-removing-docker-images/  

### Can't connect to local MySQL server through socket '/var/run/mysqld/mysqld.sock' 에러 

alpinelinux wiki : https://wiki.alpinelinux.org/wiki/Mysql#Installation  
mysql.sock 에러 해결 : https://velog.io/@tok1324/MySQL-%EC%BD%94%EB%94%A9%EC%9D%91%EC%95%A0%EC%9D%98-mysql.sock-%EC%97%90%EB%9F%AC-%ED%95%B4%EA%B2%B0  
mysql 일반적인 오류를 해결하는 유용한 정보들 : https://blog.naver.com/islove8587/221970366883  

## 22.12.11

### daemon

https://blogger.pe.kr/770  

daemon은 서비스의 요청에 응답하기 위해 실행 중인 background 프로세스이다.  

프로세스는 foreground와 background 프로세스로 나뉜다. 표준 입출력 장치를 통해서 대화하는 프로세스는 foreground 프로세스이고, 적어도 입력장치에 대하여 연결되지 않는 프로세스를 background 프로세스라고 한다.  

예를 들어, 사용자가 어떠한 서버에 ssh로 접속한다고 가정해본다. 프롬프트에 ssh을 입력하면 명령이 실행되고, 입력받은 IP주소로 접속을 시도한다. 서버에서 실행 중인 ssh daemon과 세션을 맺은 다음에 ssh daemon이 사용자에게 보여주길 원하는 내용을 ssh 명령이 받아서 화면에 출력한다. 즉, ssh 명령은 입력받은 내용을 처리하고, 전달받은 내용을 표시하기 때문에 foreground 프로세스이다. 그 다음, 프롬프트를 보면 서버의 ssh daemon이 사용자의 입력을 기다리고 있는 것처럼 보인다. 화면에는 ssh daemon이 아니라 ssh daemon이 실행한 bash가 프롬프트에 보이는 것이다.  

daemon 프로세스는 background 프로세스이며, 부모 프로세스가 PID 1이거나 다른 deamon인 프로세스를 말한다. 이를 알 수 있는 방법은 background 프로세스가 자신을 실행한 bash가 종료되었을 때 같이 종료되는지 확인하면 된다.  

대표적인 daemon 프로세스는 웹서버 daemon이다. 웹서버 daemon 프로세스는 서버에서 터미널을 통해 실행될 수 있지만 사용자와 대화할 필요가 없기 때문에 background 프로세스로 생성되도록 만들어진다. 즉, 프로그램이 fork 함수를 통해 자식 프로세스를 생성하고 부모 프로세스는 죽는다. 그리고 생성된 자식은 부모 프로세스를 PID 1로 변경한 뒤 실제로 서비스를 수행할 자식 프로세스를 여러개 fork한다. 그리고 그 손자 프로세스들은 계정을 setuid 함수를 이용하여 웹서버가 실행되도록 설정된 계정을 바꾼다.  



## 22.12.09

### Docker Container PID 1

#### Solution 1 : PID 1으로 실행하고 신호 핸들러로 등록

이 솔루션은 첫 번째 문제만 해결한다. 앱이 제어된 방식으로 하위 프로세스를 생성하면 두 번째 문제를 방지할 수 있다. 구현하는 방법은 Dockerfile에서 CMD 또는 ENTRYPOINT를 사용하여 프로세스를 실행하는 것이다.  

```
FROM debian:9

RUN apt-get update && \
    apt-get install -y nginx
    
EXPOSE 80

CMD [ "nginx", "-g", "daemon off;" ]
```

때로는 프로세스가 제대로 실행될 수 있도록 컨테이너에서 환경을 준비해야 할 수 있다. 이 경우 컨테이너를 시작할 때 셸 스크립트를 실행하여 환경을 준비하고 기본 프로세스를 실행한다. 이 방법을 사용하는 경우 셸 스크립트는 프로세스가 아닌 PID 1을 가지므로 기본 exec 명령어를 사용하여 셸 스크립트에서 프로세스를 실행해야 한다. exec 명령어로 스크립트를 원하는 프로그램으로 바꾼다. 그 다음 프로세스에서 PID 1을 상속한다.  

#### Solution 2 : Kubernetes에서 프로세스 네임 스페이스 공유 사용 설정

pod에 프로세스 네임스페이스 공유를 사용 설정하면 Kubernetes는 해당 pod의 모든 컨테이너에 단일 프로세스 네임 스페이스를 사용한다. Kubernetes pod 인프라 컨테이너가 PID 1이 되고 분리된 프롯세스는 자동으로 다시 수거된다. 

#### Solution 3 : 특수한 init 시스템 사용

기본적인 Linux 환경처럼 init 시스템을 사용하여 문제를 처리할 수 있다. 하지만 systemd 같은 일반 init 시스템을 사용하기에는 너무 복잡하고 크기 때문에 컨테이너용으로 제작된 init 시스템을 사용하는 것이 좋다.  

init 시스템을 사용하는 경우 init 프로세스는 PID 1을 가지며 다음을 수행한다.  
- 올바른 신호 핸들러를 등록한다.
- 앱에서 신호가 작동하는지 확인한다.
- 최종 모든 좀비 프로세스를 수거한다.

docker run 명령어의 --init 옵션을 사용하면 Docker 자체에서 init 시스템을 사용할 수 있다. Kubernetes에서 사용하려면 컨테이너 이미지에 init 시스템을 설치하고 컨테이너의 진입점으로 사용해야 한다.  


### 컨테이너 환경을 위한 초기화 시스템

https://swalloow.github.io/container-tini-dumb-init/  

#### 컨테이너 내부에서의 프로세스 동작

https://engineeringblog.yelp.com/2016/01/dumb-init-an-init-for-docker.html  

docker는 ENTRYPOINT나 CMD에 명시된 프로세스를 PID 1로써 새로운 PID 네임 스페이스에 정의한다. 그리고 컨테이너 내부에 있는 PID 1 프로세스에만 신호를 보내 종료할 수 있다. 그렇기 때문에 컨테이너는 경량화 이미지를 기반으로 단일 프로세스만 실행하는 경우가 많다. 다음과 같은 두가지 상황이 있다.   

1. PID 1인 shell

Dockerfile은 컨테이너의 명령을 지정하면 실행을 위해 명령을 shell에 공급한다. 그 결과 가음과 같은 프로세스 트리가 생성된다.  

```
- docker run (on the host machine)
  - /bin/sh (PID 1, inside container)
    - python my_server.py (PID 2, inside container)
```

shell을 PID 1로 사용하면 2번 프로세스에 신호를 보낼 수 없다. shell로 보낸 신호는 하위 프로세스로 전달되지 않으며, 프로세스가 완료될 때까지 shell이 종료되지 않는다. 이 경우 컨테이너를 종료하기 위해 SIGKILL을 보내야 한다.  

2. PID 1인 프로세스

Dockerfile 구문을 사용하면 프로세스가 즉시 시작되고, 컨테이너의 초기화 시스템 역할을 하여 다음과 같은 프로세스 트리가 생성된다.  

```
- docker run (on the host machine)
  - python my_server.py (PID 1, inside container)
```

프로세스가 신호를 수신하지만 PID 1이므로 예상대로 응답하지 않을 수 있다.  

##### PID 1의 Signal 문제

일반적인 프로세스는 TERM에 대한 자체 핸들러를 등록하여 종료하기 전 cleanup을 수행한다.  프로세스가 핸들러를 등록하지 않는 경우에 커널은 TERM 신호의 기본 동작인 프로세스 종료를 수행한다.  

그러나 PID 1은 TERM 신호에 대해 기본 동작으로 실행되지 않는다. 핸들러를 등록하지 않은 경우, TERM 신호는 프로세스에 아무런 영향을 미치지 못한다. 만약에 자식 프로세스가 하위 프로세스를 생성하고 먼저 죽는다면 컨테이너에 좀비 프로세스가 계속 쌓일 수 있다.  

docker run은 SIGTERM을 수신하면 컨테이너가 죽지 않더라도 신호를 컨테이너에 전달하고 종료된다. docker stop 명령을 사용하면 TERM 신호를 보내고 기다린 다음에 프로세스가 중지되지 않으면 KILL이 전송되어 즉시 중지된다.  

#### dumb-init

dumb-init은 경량화된 init 시스템이다. 서버 프로세스를 직접 실행하는 대신 Dockerfile에서 dumb-init을 사용하면 다음과 같은 프로세스 트리가 생성된다.  

```
CMD ["dumb-init", "python", "my_server.py"] 
```

```
- docker run (on the host machine)
  - dumb-init (PID 1, inside container)
    - python my_server.py (PID 2, inside container)
```

dumb-init은 모든 신호에 대해 핸들러를 등록하고 해당 신호를 프로세스 세션으로 전달한다. python 프로세스는 PID 1으로 실행되지 않기 때문에 핸들러를 등록하지 않은 경우에도 dumb-init이 보내는 신호에 기본 동작을 적용한다.  

dumb-init은 신호 처리를 할 뿐만 아니라 고아, 좀비 프로세스를 처리하는 init 시스템의 기능도 수행한다.  

#### dumb-init 사용법

dumb-init은 apline 패키지 레포에서 설치할 수 있다. [링크](https://pkgs.alpinelinux.org/package/edge/community/x86/dumb-init) Dockerfile에서 `apk add dumb-init`을 추가하여 패키지를 설치한다.  

Docker 컨테이너 내부에 설치되면 간단하게 명령에 앞에 dumb-init을 붙이면 된다. Dockerfile 내에서 컨테이너의 진입점으로 dumb-init을 사용하는 것이 좋다. ENTRYPOINT는 CMD 명령 앞에 추가되는 부분 명령으로 dumb-init에 매우 적합하다.  

```
# Runs "/usr/bin/dumb-init -- /my/script --with --args"
ENTRYPOINT ["/usr/bin/dumb-init", "--"]

# or if you use --rewrite or other cli flags
# ENTRYPOINT ["dumb-init", "--rewrite", "2:3", "--"]

CMD ["/my/script", "--with", "--args"]
```

CMD 또는 ENTRYPOINT 같은 JSON 구문으로 사용하는 것이 중요하다. 그렇지 않으면 Docker가 shell을 호출하여 shell이 PID 1이 되기 때문이다.  


## 22.12.08

### Docker Container 백그라운드 실행

https://www.popit.kr/%EA%B0%9C%EB%B0%9C%EC%9E%90%EA%B0%80-%EC%B2%98%EC%9D%8C-docker-%EC%A0%91%ED%95%A0%EB%95%8C-%EC%98%A4%EB%8A%94-%EB%A9%98%EB%B6%95-%EB%AA%87%EA%B0%80%EC%A7%80/  

Docker 컨테이너는 hostOS의 입장에서 보면 하나의 프로세스이다. 그러므로 프로세스가 종료되면 컨테이너가 종료된다. 컨테이너를 종료하지 않고 유지하는 방법을 찾아본다.   

컨테이너를 실행할 때 -d 옵션을 주면 백그라운드 프로세스로 실행한다.  
- 하지만 컨테이너는 모든 명령이 종료됨과 동시에 종료된다.  
명령어에 -it 옵션을 추가하여 shell을 백그라운드로 실행하면 컨테이너가 종료되지 않음을 볼 수 있다.  
- 그러나 shell에서 exit 명령을 이용하여 나온다면 컨테이너가 종료된다.  

### 컨테이너 환경을 위한 초기화 시스템

https://swalloow.github.io/container-tini-dumb-init/  

#### PID 1

리눅스에서 PID 1은 부팅 시 커널에 의해 최초로 실행되는 init 프로세스이다. init 프로세스는 추가 하위 프로세스를 생성할 수 있다. 결국 모든 프로세스의 최종 부모 프로세스 역할을 한다. 현재 배포판들은 init 대신 systemd가 초기화 시스템의 역할을 대신하고 있다.  

만약에 어떠한 프로세스가 예기치 못한 상황으로 종료되면 좀비 프로세스로 변한다. 좀비 프로세스는 부모 프로세스가 waitpid 시스템 명령을 수행할 때까지 존재하며 이후에 제거된다. 일반적으로 자식 프로세스가 종료되면 운영 체제에서 SIGCHLD 신호를 보내어 부모 프로세스를 깨우고 자식 프로세스를 거두게 되므로 문제가 되지 않는다.  

그렇다면 부모 프로세스가 의도적으로 종료되거나 사용자가 프로세스를 종료시켰다고 가정한다면 자식 프로세스들은 고아 상태가 된다. init 프로세스는 고아 상태가 된 자식 프로세스를 거두는 역할을 한다. init 프로세스가 생성하지 않았지만 고아 프로세스가 좀비 프로세스가 되지 않도록 정리한다. 그러나 컨테이너 환경의 경우에는 다르다.

### PID 1, 신호 처리, 좀비 프로세스 올바르게 처리하기

https://cloud.google.com/architecture/best-practices-for-building-containers?hl=ko#signal-handling  

Linux 신호는 컨테이너 내부의 프로세스 수명 주기를 제어하는 주요 방법이다. 앱의 수명 주기를 앱이 포함된 컨테이너와 긴밀하게 연결하려면 앱이 Linux 신호를 올바르게 처리하도록 해야 한다. 가장 중요한 Linux 신호는 프로세스를 종료하는 SIGTERM이다. 이외에도 SIGKILL과 SIGINT 신호를 수신할 수 있다.  

프로세스 식별자(PID)는 Linux 커널이 각 프로세스에 제공하는 고유한 식별자이다. PID는 네임 스페이스이므로 컨테이너에는 호스트 시스템의 PID에 매핑되는 고유한 PID 세트가 있다. Linux의 첫 번째 프로세스는 PID 1이며, init시스템이다. 컨테이너의 첫 번째 프로세스도 PID 1이며, Docker와 Kubernetes가 컨테이너 내부의 프로세스와 통신하거나 프로세스를 종료한다. Docker와 Kubernetes는 모두 컨테이너 내부에 PID 1이 있는 프로세스에만 신호를 보낼 수 있다.  

컨테이너의 측면에서 PID와 Linux 신호는 2가지 문제를 제기한다.  

1. Linux 커널이 신호를 처리하는 방법

Linux 커널이 신호를 처리하는 방법은 PID 1을 가진 프로세스와 그렇지 않은 프로세스에서 차이가 있다. 신호 핸들러가 이 프로세스에 자동으로 등록되지 않으므로 SIGTERM 또는 SIGINT 같은 신호는 기본적으로 아무런 영향을 미치지 않는다. 기본적으로, 단계적 종료를 방지하는 SIGKILL을 사용하여 프로세스를 강제 종료해야 한다. 앱에 따라 SIGKILL을 사용하면 모니터링 시스템에 사용자 표시 오류, 쓰기 중단(데이터 저장용), 원치 않는 알림이 발생할 수 있다.  

2. 기본 init 시스템이 분리된 프로세스를 처리하는 방법

systemd와 같은 기본 init 시스템은 분리된 좀비 프로세스를 제거(거둘 때)하는 데에도 사용된다. 분리된 프로세스(상위 요소가 사라진 프로세스)는 PID 1이 있는 프로세스에 다시 첨부된다. PID 1은 프로세스가 사라질 때 다시 거둬야 한다. 정상적인 init 시스템은 그렇게 작동한다. 그러나 컨테이너에서는 PID 1을 갖고 있는 프로세스가 이러한 책임을 갖게 된다. 이 프로세스에서 이러한 제거를 제대로 처리하지 못하면 메모리나 다른 리소스가 부족해질 수 있다.  



## 22.12.07

### MariaDB Dockerfile

[alpine-mariadb](https://github.com/yobasystems/alpine-mariadb/blob/d01b296b1d63153011fee28cb1e0f9626afe9c38/alpine-mariadb-amd64/Dockerfile)  

Docker hub에서 alpine을 베이스로한 MariaDB 이미지를 찾았다. 여기있는 Dockerfile을 참조하여 어떤 설정을 해야하는지 알아내려한다.  

```
FROM yobasystems/alpine:3.16.3-amd64

ARG BUILD_DATE
ARG VCS_REF

LABEL (...)

# MariaDB 설치
RUN apk add --no-cache mariadb mariadb-client mariadb-server-utils pwgen && \
    rm -f /var/cache/apk/*

# run.sh 파일 복사
ADD files/run.sh /scripts/run.sh

# run.sh를 위한 폴더 생성
RUN mkdir /docker-entrypoint-initdb.d && \
    mkdir /scripts/pre-exec.d && \
    mkdir /scripts/pre-init.d && \
    chmod -R 755 /scripts

# 네트워크 포트
EXPOSE 3306

# 볼륨 선언
VOLUME ["/var/lib/mysql"]

# run.sh 실행
ENTRYPOINT ["/scripts/run.sh"]
```

- [pwgen](https://linuxconfig.org/how-to-use-a-command-line-random-password-generator-pwgen-on-linux) : 임의의 패스워드를 생성하기 위해 설치
- [--no-cache](https://stackoverflow.com/questions/49118579/alpine-dockerfile-advantages-of-no-cache-vs-rm-var-cache-apk) : 캐시하지 않도록 하여 컨테이너를 작게 유지


## 22.12.06

### container의 OS, Virtual Machine의 OS

docker는 linux 위에서 구동된다. 만약에 Window와 MacOS docker를 설치하면 경량화된 linux 머신 위에서 docker가 구동된다. 여기에서 VM가 아닌 docker를 사용하는 이유에 대해 의문을 가질 수 있다. VM과 Docker Container의 차이를 살펴본다.  

Virtual Machine은 x86 하드웨어를 가상화했다고 볼 수 있다. 그렇기 때문에 VM에는 다양한 OS를 설치할 수 있다. Docker Container는 Linux기반의 OS만 지원하며 Container 자체에는 Kernel 등의 OS 이미지가 들어있지 않다. Kernel은 Host OS를 그대로 사용하고, Host OS와 Container OS의 다른 부분만 Container 내에 같이 Packing된다. Container 내에서 명령어를 수행하면 실제로는 Host OS에서 명령어가 수행된다. 즉, Host OS의 Process 공간을 공유한다.  

참조 : https://mosei.tistory.com/entry/Docker-Container%EC%9D%98-OS-vs-VM%EC%9D%98-OS, https://bcho.tistory.com/805  

### MariaDB

MariaDB는 관계형 데이터베이스 관리 시스템(RDBMS) 중 하나이다. MySQL을 제작한 AB사의 개발자들이 나와서 만든 오픈소스 RDBMS 소프트웨어이다. MySQL을 인수한 오라클의 정책에 반발하여 만들게 되었다고 한다.  

관계형 데이터베이스란 최소한 두 여건을 만족하는 데이터베이스 시스템이다.  
- 사용자에게 데이터를 관계로서 표현한다. 즉, 행과 열의 집합으로 구성된 테이블의 묶음 형식으로 데이터를 제공한다. 
- 테이블 형식의 데이터를 조작할 수 있는 관계 연산자를 제공한다. 


## 22.12.05

### The Compose application model

참조 : https://docs.docker.com/compose/compose-file/  

Compose 사양를 사용하면 플랫폼에 구애받지 않는 컨테이너 기반 애플리케이션을 정의할 수 있다. 애플리케이션은 적절한 공유 리소스 및 통신 채널과 함께 실행되어야 하는 컨테이너 집합으로 설계된다.  

애플리케이션의 구성 요소는 services로 정의된다. services는 동일한 컨테이너 이미지를 한 번 이상 실행하여 플랫폼에서 구현되는 추상적인 개념이다.  

services는 networks를 통해 서로 통신한다. networks는 함께 연결된 services 내의 컨테이너 간에 IP 경로를 설정하기 위한 플랫폼 기능 추상화이다. 

services는 영구 데이터를 volumes에 저장하고 공유한다. volumes은 전역 옵션이 있는 상위 수준 파일 시스템 마운트와 같은 영구 데이터를 설명한다.  

#### example

다음 예제는 프런트엔드 웹 애플리케이션과 백엔드 서비스로 분할된 애플리케이션이다.  
프런트엔드는 런타임에 인프라에서 관리하는 HTTP configuration 파일로 구성되어 외부 도메인 이름과 플랫폼의 보안 비밀 저장소에 삽입된 HTTPS server certificate를 제공한다.  
백엔드는 persistent volume에 데이터를 저장한다.  
두 서비스 모두 격리된 백티어 네트워크에서 서로 통신하지만 프런트엔드는 프런트 네트워크에 연결되어 외부 사용을 위해 포트 443을 노출한다.  

```
(External user) --> 443 [frontend network]
                            |
                  +--------------------+
                  |  frontend service  |...ro...<HTTP configuration>
                  |      "webapp"      |...ro...<server certificate> #secured
                  +--------------------+
                            |
                        [backend network]
                            |
                  +--------------------+
                  |  backend service   |  r+w   ___________________
                  |     "database"     |=======( persistent volume )
                  +--------------------+        \_________________/
```

예제 애플리케이션은 다음으로 구성된다.
- Docker Image로 지원되는 2개의 services: webapp, database
- 프런트엔트에 주입된 1개의 secured
- 프런트엔트에 삽입된 1개의 configs
- 백엔드에 연결된 1개의 영구 volumes
- 2개의 networks

```
services:
  frontend:
    image: awesome/webapp
    ports:
      - "443:8043"
    networks:
      - front-tier
      - back-tier
    configs:
      - httpd-config
    secrets:
      - server-certificate

  backend:
    image: awesome/database
    volumes:
      - db-data:/etc/data
    networks:
      - back-tier

volumes:
  db-data:
    driver: flocker
    driver_opts:
      size: "10GiB"

configs:
  httpd-config:
    external: true

secrets:
  server-certificate:
    external: true

networks:
  # The presence of these objects is sufficient to define them
  front-tier: {}
  back-tier: {}
```


## 22.12.04

### Docker file

컨테이너 이미지를 만들기 위해서 Dockerfile를 사용해야 한다. Dockerfile은 파일 확장자가 없는 단순한 텍스트 기반 파일이다. Dockerfile에는 Docker가 컨테이너 이미지를 생성하는 데 사용하는 명령 스크립트가 포함되어 있다.  

1. app의 package.json 파일이 있는 위치와 같은 디렉터리에 Dockerfile이라는 이름의 파일을 생성한다.  

```bash
> cd /path/to/app
> touch Dockerfile
```

2. 텍스트 편집기를 사용하여 Dockerfile에 다음과 같은 내용을 추가한다.  

```
FROM node:18-alpine
WORKDIR /app
COPY . .
RUN yarn install --production
CMD ["node", "src/index.js"]
EXPOSE 3000
```

[Dockerfile reference](https://docs.docker.com/engine/reference/builder/)  
- FROM : 베이스 이미지이다. node:18-alpine은 노드 18에 설치된 alpine 기반 이미지를 뜻한다.
- WORKDIR : Dockerfile에 뒤따라 나오는 명령어의 작업 디렉터리를 정한다.
- RUN : Docker image가 생성되기 전에 수행할 쉘 명령어이다.
- COPY : Docker client의 현재 디렉토리에서 파일을 추가한다. (COPY (source) (dist))
- CMD : 컨테이너 내에서 실행할 명령을 지정하며, Dockerfile 내에서 한 번만 사용할 수 있다. 
- EXPOSE : 컨테이너가 런타임 시 지정된 네트워크 포트에서 수신을 대기하고 있음을 Docker에 알린다. 

3. 다음과 같은 명령어로 컨테이너 이미지를 만든다. 

/path/to/app으로 디렉터리 위치를 이동하고, 컨테이너 이미지를 만든다.  

```bash
> cd /path/to/app
> docker build -t getting-started .
```

docker build 명령은 Dockerfile을 사용하여 새 컨테이너 이미지를 빌드한다. node:18-alpine 이미지에서 시작하겠다고 빌더에 지시했지만 머신에 해당 파일이 없기 때문에 Docker는 많은 이미지 레이어를 다운로드한다.  

Docker가 이미지를 다운로드한 후에 Dockerfile의 지침이 애플리케이션에 복사되고, yarn으로 애플리케이션의 종속성을 설치한다. CMD 지시문은 이미지에서 컨테이너를 시작할 때 실행할 기본 명령을 지정한다.  

-t 플래그는 이미지에 태그를 지정한다. 읽기 쉬운 이름으로 정하는 것이 좋다. 이미지 이름을 getting-started로 지정하였으므로 컨테이너를 실행할 때 해당 이미지를 참조할 수 있다.  

docker build 명령의 마지막에 있는 . 은 Dockerfile을 현재 디렉터리에서 찾음을 뜻한다. 

## 22.12.03

### Docker Compose

Docker Compose란, 다중 컨테이너 Docker 애플리케이션을 정의하고 실행하기 위한 도구이다. Compose에서는 YAML 파일을 사용하여 애플리케이션 서비스를 구성한다. 그런 다음에 단일 명령으로 모든 서비스를 만들고 시작한다.  

Compose를 사용하기 위해 기본적으로 3단계를 거친다.  
1. 어디에서나 재현할 수 있도록 Dockerfile으로 애플리케이션의 환경을 정의한다.  
2. 격리된 환경에서 함께 실행할 수 있도록 docker-compose.yml에서 앱을 구성하는 서비스를 정의한다.
3. docker compose up을 실행하면 Docker compose command가 전체 앱을 시작하고 실행한다. 

docker-compose.yml 파일은 다음과 같다.  

```
version: "3.9"  # optional since v1.27.0
services:
  web:
    build: .
    ports:
      - "8000:5000"
    volumes:
      - .:/code
      - logvolume01:/var/log
    depends_on:
      - redis
  redis:
    image: redis
volumes:
  logvolume01: {}
```

Compose 파일에 대한 자세한 내용은 다음을 참조하면 된다.  
[Compose file reference](https://docs.docker.com/compose/compose-file/)  

### Key features of Docker Compose

#### 단일 호스트에 여러 개의 격리된 환경

Compose는 프로젝트 이름을 사용하여 환경을 서로 격리한다. 여러 다른 컨텍스트에서 이 프로젝트 이름을 사용할 수 있다.  
- 프로젝트의 각 기능 분기에 대한 안정적인 복사본을 실행하려는 경우처럼 개발 호스트에서 단일 환경의 여러 복사본을 만들기 위해 사용한다.
- CI 서버에서 빌드가 서로 간섭하지 않도록 프로젝트 이름을 고유한 빌드 번호로 설정할 수 있다.
- 공유 호스트 또는 개발 호스트에서 동일한 서비스 이름을 사용할 수 있는 서로 다른 프로젝트가 서로 간섭하지 않도록 한다. 

#### 컨테이너 생성 시 볼륨 데이터 보존

Compose는 서비스에서 사용하는 모든 볼륨을 보존한다. 실행 시 이전 컨테이너에서 새 컨테이너로 볼륨을 복사하여 볼륨에서 만든 모든 데이터가 손실되지 않도록 한다.  

#### 변경된 컨테이너만 재생성

Compose는 컨테이너를 만드는 데 사용되는 구성을 캐시한다. 변경되지 않은 서비스를 다시 시작하면 Compose는 기존 컨테이너를 재사용한다.  

#### 변수 지원 및 환경 간 컴포지션 이동

Compose는 Compose 파일의 변수를 지원한다. 이러한 변수를 사용하여 다양한 환경 또는 다양한 사용자에 맞게 구성을 사용자 지정할 수 있다.  



## 22.12.02

### Docker

Docker는 컨테이너라고 하는 격리된 환경에서 애플리케이션을 패키징하고 실행할 수 있는 기능을 제공한다. 격리 및 보안을 통해 지정된 호스트에서 여러 컨테이너를 동시에 실행할 수 있다. 컨테이너는 가볍지만 애플리케이션을 실행하는 데 필요한 모든 것을 포함하므로 호스트에 현재 설치된 항목에 의존할 필요가 없다. 작업하는 동안 컨테이너를 쉽게 공유할 수 있으며 공유하는 모든 사람이 동일한 방식으로 작동하는 동일한 컨테이너를 갖게 된다.  

Docker는 다음과 같은 상황에서 사용할 수 있다. 
- 로컬에서 코드를 작성하고 Docker 컨테이너를 사용하여 동료와 작업을 공유한다.
- Docker를 사용하여 애플리케이션을 테스츠 환경에 푸시하고 자동 및 수동 테스트를 실행한다.
- 개발자가 버그를 발견하면 개발 환경에서 버그를 수정하고 테스트 및 검증을 위해 테스트 환경에 재배보할 수 있다.
- 테스트가 완료되면 고객에게 수정 사항을 제공하는 것은 업데이트된 이미지를 프로덕션 환경에 푸시하는 것만큼 간단하다.

#### Docker Architecture

Docker는 클라이언트-서버 아키텍처를 사용한다.  
Docker 클라이언트는 컨테이너 빌드. 실행 및 배포하는 Docker 데몬과 통신한다. 이들은 동일한 시스템에서 실행되거나 클라이언트를 원격 데몬에 연결할 수 있다.  
Docker 클라이언트와 데몬은 UNIX 소켓 또는 네트워크 인터페이스를 통해 REST API를 사용하여 통신한다.  
또 다른 Docker 클라이언트는 컨테이너 세트로 구성된 애플리케이션으로 작업할 수 있는 Docker Compose이다.  

![](./docs/src/projects/inception/docker01.png)  


### alpine linux

리눅스 커널을 기반으로 한 리눅스 배포판 가운데 하나이다. Musl과 BusyBox를 기반하고 있다. alpine linux는 작고, 보안이 뛰어나고, 간단함을 염두하여 만들어졌다. 이 장점이 두드러져서 배포판의 용량이 커널을 제외하고 8MB 밖에 되지 않으며, 수많은 패키지들을 설치할 수 있다.  

기본적으로 다른 리눅스 배포판보다 훨씬 가볍고 깔끔한 것이 장점이기 때문에 Docker 컨테이너에 사용되는 예시가 많고 유명하다. 주로 호스트 환경보다 특정 애플리케이션을 서비스하는 컨테이너 환경에서 사용할 수 있으면 되기 대문에 미러 서버의 규모가 다른 배포판에 비해 크지는 않다. 


## 22.12.01

### Container

#### Container란

컨테이너는 호스트 시스템의 다른 모든 프로세스와 격리된 시스템의 샌드박스 프로세스이다. 오랫동안 Linux에 있던 기능인 kernel namespace와 cgroups를 활용하여 프로세스를 격리시킨다.  

다음과 같이 컨테이너를 요약할 수 있다.  
- 이미지의 실행 가능한 인스턴스이다.
- DockerAPI 또는 CLI를 사용하여 컨테이너를 생성, 시작, 중지, 이동 또는 삭제할 수 있다.
- 로컬 머신, 가상 머신에서 실행하거나 클라우드에 배포할 수 있다.
- 모든 OS에서 실행할 수 있다. 
- 다른 컨테이너와 격리되며 자체 소프트웨어, 바이너리 및 구성을 실행한다. 

#### Container Image란

컨테이너는 실행할 때 격리된 파일 시스템을 사용한다. 이 사용자 지정 파일 시스템은 컨테이너 이미지에서 제공된다. 이미지에는 컨테이너의 파일 시스템이 포함되어 있으므로 애플리케이션을 실행하는 데 필요한 모든 것이 포함되어야 한다. 이미지에는 환경 변수, 실행할 기본 명령 및 기타 메타 데이터와 같은 컨테이너에 대한 기타 구성도 포함된다. chroot는 2000년대 초반에 마이크로서비스라고 부르는 애플리케이션을 실행했으며, 현재에는 다양한 응용 프로그램에서 사용된다.  

#### Kernel Space

##### chroot

대부분의 모든 UNIX 운영 체제는 현재 실행 중인 프로세스의 루트 디렉터리를 변경할 수 있다. UNIX 버전 7에서 chroot가 처음 등장한 것에서 비롯되었으며, Linux에서는 시스템 호출 또는 해당 독립 실행형 래퍼 프로그램으로 chroot를 사용할 수 있다. chroot는 1991년에 어떤 사람이 보안 해커를 감시하기 위해 허니팟으로 사용했기 때문에 "감옥"이라고도 한다.  

```bash
> mkdir -p new-root/{bin,lib64}
> cp /bin/bash new-root/bin
> cp /lib64/{ld-linux-x86-64.so*,libc.so*,libdl.so.2,libreadline.so*,libtinfo.so*} new-root/lib64
> sudo chroot new-root
```

자체 chroot 환경을 실행하려면 새로운 루트 디렉터리를 생성하고, bash 쉘과 해당 의존 항목을 복사하고 chroot를 실행한다. 빌트인 함수만을 가지고 있기 때문에 아직 쓸모가 없는 bash이다.  

현재 작업 디렉토리는 chroot가 호출된 때에서 변경되지 않지만, 상대 경록는 여전히 새 루트 외부의 파일을 참조할 수 있다. 이 호출이 루트 경로만 변경하고 다른 것은 변경하지 않기 때문이다. 그래서 루트 사용자는 다음과 같은 프로그램을 실행하여 감옥에서 쉽게 탈출할 수 있다.  

```c
#include <sys/stat.h> 
#include <unistd.h> 

int main(void) 
{ 
    mkdir(".out", 0755); 
    chroot(".out"); 
    chdir("../../../../../"); 
    chroot("."); 
    return execl("/bin/bash", "-i", NULL); 
}
```

현재 감옥을 덮어써서 새로운 감옥을 만들고 작업을 chroot 외부 환경에 상대 경로로 직접 변경한다. chroot 호출은 다른 bash 쉘을 생성하여 감옥 외부로 이동된다.  

그렇기 때문에 유용한 감옥을 사용하려면 적절한 루트 파일 시스템이 필요하다. 여기에는 모든 바이너리, 라이브러리 및 필요한 파일 구조가 포함된다. 다음 코드는 루트 파일 시스템을 skopeo와 umoci를 사용하여 가져온다.  

```bash
> skopeo copy docker://opensuse/tumbleweed:latest oci:tumbleweed:latest 
[output removed] 
> sudo umoci unpack --image tumbleweed:latest bundle 
[output removed]
```

새로 다운로드하고 추출한 rootfs에 chroot를 사용하여 감옥을 설정할 수 있다.  

```bash
> sudo chroot 번들/rootfs 
#
```

그러나 아직 프로세스 관점에서 감옥을 외부에서 몰래 들여다볼 수 있다. 심지어 감옥에서 실행되는 프로그램을 외부에서 죽일 수도 있다. 

```bash
> mkdir /proc 
> mount -t proc proc /proc 
> ps aux 
[출력 제거됨]
```

네트워크 격리도 없다. 감옥에 누락된 격리는 많은 보안 관련 문제로 이어진다. 

```bash
> mkdir /sys 
> 마운트 -t sysfs sys /sys 
> ls /sys/class/net 
eth0 lo
```

이 문제를 해결하기 위해 Linux namespace가 사용된다.  

##### Linux namespace

namespace는 2002년 Linux2.4.19와 함께 도입된 커널 기능이다. namespace는 추상화 계층에서 특정 글로벌 시스템 리소스를 래핑한다. 이는 namespace 내의 프로세스가 자체적으로 격리된 리소스 인스턴스를 갖는 것처럼 보인다. 커널 namespace 추상화를 통해 서로 다른 프로세스 그룹이 시스템에 대해 서로 다른 뷰를 가지게 된다.  

##### namespace API

namespace API는 세가지 주요 시스템 호출로 구성된다. 

###### clone

clone API 함수는 fork와 비슷하게 새로운 자식 프로세스를 생성한다. 그러나 fork와 다르게 clone API는 자식 프로세스에게 메모리 공간, 파일 디스크립트 테이블, 시그널 핸들러 같은 실행 컨텍스트의 일부를 호출 프로세스와 공유한다. 그리고 다른 namespace 플레그를 넘겨주어 자식 프로세스에 새로운 namespace를 생성할 수 있다.  

###### unshare

unshare 함수는 현재 프로세스가 다른 프로세스와 공유 중인 실행 컨텍스트의 일부를 연결 해제할 수 있다.  

###### setns

setns 함수는 호출 thread를 제공된 namespace 파일 디스크립터와 다시 연결한다. 기존 namespace를 합치는데 사용할 수 있다.  

###### proc

proc 파일 시스템은 추가 namespace 관련 파일을 채운다. Linux3.8 이후로, /proc/$PID/ns 파일은 "매직" 링크이다. 참조된 namespacee에 대한 작업을 수행학기 위한 핸들로 사용할 수 있다.  

```bash
> ls -Gg /proc/self/ns/
total 0
lrwxrwxrwx 1 0 Feb  6 18:32 cgroup -> 'cgroup:[4026531835]'
lrwxrwxrwx 1 0 Feb  6 18:32 ipc -> 'ipc:[4026531839]'
lrwxrwxrwx 1 0 Feb  6 18:32 mnt -> 'mnt:[4026531840]'
lrwxrwxrwx 1 0 Feb  6 18:32 net -> 'net:[4026532008]'
lrwxrwxrwx 1 0 Feb  6 18:32 pid -> 'pid:[4026531836]'
lrwxrwxrwx 1 0 Feb  6 18:32 pid_for_children -> 'pid:[4026531836]'
lrwxrwxrwx 1 0 Feb  6 18:32 user -> 'user:[4026531837]'
lrwxrwxrwx 1 0 Feb  6 18:32 uts -> 'uts:[4026531838]'
```

이를 통해 특정 프로세스가 상주하는 namespace를 추적할 수 있다. 여기에는 시스템 호출에 대한 전용 래퍼 프로그램이 포함되어 있다. 현재 액세스 가능한 모든 namespace 또는 주어진 단일 namespace에 대한 유용한 정보를 나열한다.  

##### namespace

###### mnt

mnt namespace를 사용하여 Linux는 일련의 마운트 지점을 프로세스 그룹별로 분리할 수 있다. 감옥과 유사하지만 더 안전한 방식으로 환경을 만들 수 있다. 다음과 같이 API 시스템 호출이나 unshare 명령 라인 툴로 쉽게 수행할 수 있다.  

```bash
> sudo unshare -m
# mkdir mount-dir
# mount -n -o size=10m -t tmpfs tmpfs mount-dir
# df mount-dir
Filesystem     1K-blocks  Used Available Use% Mounted on
tmpfs              10240     0     10240   0% <PATH>/mount-dir
# touch mount-dir/{0,1,2}
```

```bash
> ls mount-dir
> grep mount-dir /proc/mounts
>
```

호스트 시스템 레벨에서 사용할 수 없는 tmpfs이 성공적으로 마운트된 것을 볼 수 있다.  

마운트 지점에 사용되는 실제 메모리는 가상 파일 시스템(VFS)이라는 추상화 계층에 있다. 이 계층은 커널의 일부이며 다른 모든 파일 시스템의 기반이 된다. namespace가 파괴되면 마운트 메모리는 복구할 수 없게 손실된다. mount namespace 추상화는 루트 권한 없이도 루트 사용자인 전체 가상 환경을 생성할 수 있는 가능성을 제공한다.  

호스트 시스템에서 proc 파일 시스템 내부의 mountinfo 파일을 통해 마운트 지점을 볼 수 있다.  

```bash
> grep mount-dir /proc/$(pgrep -u root bash)/mountinfo
349 399 0:84 / /mount-dir rw,relatime - tmpfs tmpfs rw,size=1024k
```

프로그램은 사용된 namespace를 참조하는 해당 /proc/$PID/ns/mntt 파일에 파일 핸들을 유지하는 경향이 있다. mount namespace 관련 구현 시나리오는 복잡할 수 있지만 유연한 컨테이너 파일 시스템 트리를 생성할 수 있는 기능을 제공한다. 마운트는 다양한 특징을 가질 수 있다. 이는 Linux 커널의 [공유 하위 트리 문서](https://www.kernel.org/doc/Documentation/filesystems/sharedsubtree.txt)에서 잘 설명된다. 

###### uts

uts namespace는 현재 호스트 시스템에서 도메인 및 호스트 이름의 공유를 해제할 수 있다. 다음과 같이 사용한다.  

```bash
> sudo unshare -u
# hostname
nb
# hostname new-hostname
# hostname
new-hostname
```

```bash
> hostname
nb
```

시스템 레벨에서는 아무것도 변경되지 않았다. uts namespace는 특히 컨테이너 네트워킹과 관련하여 컨테이너화의 추가 기능이다. 

###### ipc

ipc namespace는 프로세스 간 통신 리소스를 격리한다. 특히 System V IPC 객체와 POSIX 메시지 대기열이다. 한 가지 예시로 오용을 방지하기 위해 두 프로세스 간의 공유 메모리(SHM)를 분리한다. 대신에 각 프로세스는 공유 메모리 세그먼트에 대해 동일한 식별자를 사용하고 두 개의 개별 영역을 생성할 수 있다. ipc namespace가 소멸되면 namespace의 ipc 객체도 자동으로 소멸된다.  

###### pid

pid namespace는 프로세스에 독립적인 프로세스 식별자(PID) 세트를 제공한다. 즉, 서로 다른 namespace에 있는 프로세스가 동일한 PID를 소유할 수 있다. 결국 프로세스에는 두가지 PID, namespace 내부의 PID와 호스트 시스템의 PID가 있다. pid namespace는 중첩될 수 있으므로 새 프로세스가 생성되면 현재 namespace에서 초기 PID namespace까지 각 namespace에 대한 PID를 가지게 된다.  

PID namespace에서 생성된 첫 번째 프로세스는 숫자 1을 얻고 일반 초기화 프로세스와 동일한 특수 처리를 모두 얻는다. 예를 들어 namespace 내의 모든 프로세스는 호스트 PID 1이 아닌 namespace의 PID 1로 부모가 변경된다. 또한 이 프로세스를 종료하면 PID namespace의 모든 프로세스와 모든 하위 프로세스가 즉시 종료된다. 다음은 새로운 PID namespace를 만든다.  

```bash
> sudo unshare -fp --mount-proc
# ps aux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.4  0.6  18688  6608 pts/0    S    23:15   0:00 -bash
root        39  0.0  0.1  35480  1768 pts/0    R+   23:15   0:00 ps aux
```

잘 분리된 것을 볼 수 있다. 새 namespace에서 proc 파일 시스템을 다시 마운트하려면 --mount-proc 플래그가 필요하다. 그렇지 않으면 namespace에 해당하는 PID 하위 트리를 볼 수 없다. 

###### net

net namespace는 네트워크 스택을 가상화하는데 사용할 수 있다. 각 네트워크 namespace는 /proc/net에 자체 리소스 속성을 포함한다. 또한 네트워크 namespace는 초기 생성 시 루프백 인터페이스만 포함한다.  

```bash
> sudo unshare -n
# ip l
1: lo: <LOOPBACK> mtu 65536 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
```

모든 네트워크 인터페이스(물리적 또는 가상)는 namespace당 정확히 한 번만 존재한다. namespace 간에 인터페이스를 이동할 수 있다. 각 namespace에는 비공개 IP 주소 집합, 자체 라우팅 테이블, 소켓 목록, 연결 추적 테이블, 방화벽 및 기타 네트워크 관련 리소스가 포함되어 있다.  

네트워크 namespace를 제거하면 모든 가상이 제거되고 그 안에 있는 모든 물리적 인터페이스가 다시 초기 네트워크 namespace로 이동된다.  

net namespace의 예시로는 가상 이더넷(veth) 인터페이스 쌍을 통해 소프트웨어 정의 네트워크(SDN)를 생성하는 것이다. 네트워크 쌍의 한 쪽은 브리지 인터페이스에 연결되고, 다른 쪽은 대상 컨테이너에 할당된다.  

###### user

user namespace는 사용자 및 그룹 ID를 격리한다. Linux 3.8에서는 실제로 권한이 없어도 user namespace를 생성할 수 있다. user namespace를 사용하면 프로세스의 사용자 및 그룹 ID가 namespace 외부와 다를 수 있다.  

namespace 생성 후 /proc/$PID/{u, g}id_map 파일은 PID의 사용자 및 그룹 ID에 대한 매핑을 노출한다. 일반적으로 이러한 파일 내의 각 중에는 두 사용자 namespacee 간의 연속적인 사용자 ID 범위에 대한 일대일 매핑이 포함되며 다음과 같이 표시될 수 있다.  

```bash
> cat /proc/$PID/uid_map
0 1000 1
```

시작 사용자 ID가 0인 namespace는 ID 1000에서 시작하는 범위에 매핑된다. 정의된 길이가 1이므로 ID가 1000인 사용자에게만 적용된다.  

이제 프로세스가 파일에 액세스하려고 하면 권한 확인을 위해 해당 사용자 및 그룹 ID가 초기 사용자 namespace에 매핑된다. 프로세스가 파일 사용자 및 그룹 ID를 검색할 때(stat(2)를 통해) ID는 반대 방향으로 매핑된다.  

/proc/$PID/setgroups 파일에는 user namespace 내에서 setgroups을 시스템 호출할 수 있는 권한을 활성화 또는 비활성화하는 허용 또는 거부가 포함되어 있다. 이 파일은 user namespace에 도입된 추가 보안 문제를 해결하기 위해 추가되었다. 권한이 없는 프로세스가 사용자에게 모든 권한이 있는 새 namespace를 생성할 수 있다. 이전에 권한이 없었던 이 사용자는 이전에 가지고 있지 않았던 파일에 접근하기 위해 setgroups를 통해 그룹을 삭제할 수 있다.  

###### cgroup

cgroup namespace는 리소스 제한, 우선순위 지정 및 제어를 지원한다. 호스트 정보가 namespace로 유출되는 것을 방지하기 위해 추가되었다. 기본적으로 커널은 /sys/fs/cgroup에 cgroup을 노출한다. 새 cgroup을 생성하려면 해당 위치에 새 하위 디렉터리를 생성하면 된다.  

```bash
> sudo mkdir /sys/fs/cgroup/memory/demo
> ls /sys/fs/cgroup/memory/demo
cgroup.clone_children
cgroup.event_control
cgroup.procs
memory.failcnt
memory.force_empty
memory.kmem.failcnt
memory.kmem.limit_in_bytes
memory.kmem.max_usage_in_bytes
memory.kmem.slabinfo
memory.kmem.tcp.failcnt
memory.kmem.tcp.limit_in_bytes
memory.kmem.tcp.max_usage_in_bytes
memory.kmem.tcp.usage_in_bytes
memory.kmem.usage_in_bytes
memory.limit_in_bytes
memory.max_usage_in_bytes
memory.move_charge_at_immigrate
memory.numa_stat
memory.oom_control
memory.pressure_level
memory.soft_limit_in_bytes
memory.stat
memory.swappiness
memory.usage_in_bytes
memory.use_hierarchy
notify_on_release
tasks
```

이미 몇가지 기본값이 있음을 알 수 있다. 여기에서 해당 cgroup에 대한 메모리 제한을 설정할 수 있다.  

```bash
> sudo su
# echo 100000000 > /sys/fs/cgroup/memory/demo/memory.limit_in_bytes
# echo 0 > /sys/fs/cgroup/memory/demo/memory.swappiness
```

cgroup에 프로세스를 지정하기 위해 cgroup.procs 파일에 해당 PID를 작성할 수 있다.  

```bash
# echo $$ > /sys/fs/cgroup/memory/demo/cgroup.procs
```

이제 애플리케이션을 실행하여 허용된 100MB 이상의 메모리를 사용해볼 수 있다.  



참조 : https://docs.docker.com/get-started/, https://medium.com/@saschagrunert/demystifying-containers-part-i-kernel-space-2c53d6979504  
