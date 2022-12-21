---
layout: default
title: nginx conf
nav_order: 3
grand_parent: Projects
parent: Inception
permalink: /docs/projects/Inception/nginx_conf
---

- [nginx 구성 파일](#nginx-구성-파일)
- [기본 구조](#기본-구조)
  - [Core Module](#core-module)
  - [Event Block](#event-block)
  - [Http Block](#http-block)
  - [Server Block](#server-block)
  - [Location Block](#location-block)
- [Example : 정적 컨텐츠 제공](#example--정적-컨텐츠-제공)
- [Example : 프록시 서버 설정](#example--프록시-서버-설정)
  - [FastCGI 프록싱 설정](#fastcgi-프록싱-설정)

# nginx 구성 파일

nginx는 구성 파일에 명시된 지시문에 제어되는 모듈로 구성된다. 지시문은 단순 지시문과 블록 지시문으로 나뉜다. 단순 지시문은 공백으로 구분된 이름과 매개변수로 구성되며 세미콜론으로 끝난다. 블록 지시문은 중괄호로 둘러싸인 명령어 세트로 끝난다. 블록 지시문 안에 있는 다른 지시문은 컨텍스트라고 부른다.  

구성 파일에서 어떠한 컨텍스트 외부에 있는 지시문은 메인 컨텍스트에 있는 것으로 간주된다. events와 http 지시문은 메인 컨텍스트, server는 http, location은 server에 위치한다.  


# 기본 구조

```
# Core Module
user www-data;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

# event block
events {
    ...
}

# http block
http {
    ...
}
```

## Core Module

```
user www-data;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;
```

nginx의 기본 동작 방식을 설정하며, 파일의 최상단에 위치한다.  
- user : nginx 프로세스가 실행되는 권한
- worker_processes : 생성할 워커 프로세스의 개수
- error_log : 에러 발생 시, 로그가 생성될 경로
- pid : 마스터 프로세스 id 정보가 저장될 경로

## Event Block

```
events {
    worker_connections 768;
    # multi_accept on;
}
```

이벤트와 관련되어 설정하는 영역이다. 
- worker_connections : 하나의 프로세스가 처리할 수 있는 커넥션의 수(최대값은 768)

Core Module의 worker_processes와 worker_connections를 계산하여 최대 커낵션 수를 알 수 있다. (최대 접속자(커넥션) 수 = worker_processes * worker_connections)  

## Http Block

```
http {

    ## http 단에서 설정
    include /etc/nginx/mime.types;
    default_type application/actet-stream;
    
    ## server block
	server {
        ...
        
        ## location block
        location / {
            ...
        }
    }
}
```

- keepalive_timeout : 접속시 커넥션을 유지하는 시간
- server_tokens : 헤더에 nginx 버전을 숨기는 기능
- server_names_hash_bucket_size : nginx 웹 서버에서 처리할 수 있는 호스트의 최대 개수
- server_names_hash_max_size : 호스트의 도메인 이름 길이 제한
- include : 설정 파일을 불러와서 적용

## Server Block

```
server {
        listen         80 default_server;
        listen         [::]:80 default_server;
        server_name    example.com www.example.com;
        root           /var/www/example.com;
        index          index.html;
        try_files $uri /index.html;
        
        # location block
        location / {
            ...
        }
}
```

- listen : 서버가 사용할 포트
- root : 정적 파일들이 있는 경로
- server_name : 사용할 도메인 주소
- index : 기본적으로 렌더할 파일


## Location Block

```
location / {
  proxy_http_version      1.1;
  proxy_set_header        X-Forwarded-For $remote_addr;
  client_max_body_size    0;
  proxy_set_header        X-Forwarded-Proto $scheme;
  proxy_set_header        Host $http_host;
  proxy_pass              "http://localhost:3000";
}
```

헤더 세팅, proxy http version, 경로 등을 설정한다.


# Example : 정적 컨텐츠 제공

웹 서버 작업은 파일(이미지 또는 정적 페이지) 제공이 중요하다. 그러므로 요청에 따라 서로 다른 로컬 디렉터리에서 파일이 제공되는 예제를 구현해보기로 한다. 이를 위해서 구성 파일을 편집하고 두개의 location 블록이 있는 http 블록이 있는 server 블록을 설정한다.  

먼저, /data/www 디렉토리에 index.html 파일을 넣고, /data/images 디렉로리에 몇가지 이미지를 넣어 둔다. 그리고 구성 파일을 열어서 새로운 server 블록을 시작한다.  

```
http {
    server {
        
    }    
}
```

일반적으로 구성 파일에는 서버 이름과 포트로 구분되는 여러 server 블록이 포함될 수 있다. nginx가 요청을 처리하는 서버를 결정하면 서버 블록 내부에 정의된 location 지시문의 매개변수에 대해 요청 헤더에 지정된 URI를 테스트한다.  

```
http {
    server {
        location / {
            root /data/www;
        }
    }
}
```

location 블록은 요청의 URI와 비교하여 "/" 접두사를 지정한다. root 지시문에 지정된 경로가 추가되어 로컬 파일 경로를 형성한다. 만약에 일치되는 요청이 여러 개인 경우에는 접두사가 가장 긴 블록을 선택한다. 두번째 location 블록을 추가하면 다음과 같다.  

```
http {
    server {
        location / {
            root /data/www;
        }
        
        location /images/ {
            root /data;
        }
    }
}
```

이렇게 포트 80번에서 수신하는 서버가 구성되었으며, 로컬 시스템에서 액세스할 수 있다. 만약에 http://localhost/. 로 시작하고, /images/ 가 붙는 요청을 한다면, 응답으로 서버는 /data/images 디렉터리에서 파일을 보낸다. 예를 들어, http://localhost/images/example.png 요청에 응답으로 /data/images/example.png 파일을 보낸다. 해당 파일이 없으면 404 오류를 나타내는 응답을 보낸다.  


# Example : 프록시 서버 설정

nginx로 프록시 서버 설정을 자주한다. 즉, 수신한 요청을 프록시 서버에 전달하고, 응답을 받아서 클라이언트에 다시 보내는 서버로 자주 설정한다는 뜻이다.  

예시는 로컬 디렉터리의 파일로 이미지 요청을 처리하고, 다른 모든 요청은 프록시 서버로 보내는 기본 프록시 서버를 구성한다. 이 두 서버는 단일 nginx 인스턴스에 정의된다.  

먼저, 다음과 같이 server 블록을 구성 파일에 추가한다.  

```
server {
    listen 8080;
    root /data/up1;
    
    location / {
    }
}
```

포트 8080에서 수신 대기하고, 모든 요청을 로컬의 /data/up1 디렉터리에 매핑하는 간단한 서버가 될 것이다. 디렉터리를 생성하고 index.html 파일을 넣는다. server에 root 지시문이 있기 때문에 location 블록에는 root 지시문을 포함시키지 않는다.  

다음은 프록시 서버 구성을 만든다. 첫번째 위치 블록에 프록시 서버의 프로토콜, 이름 및 포트와 함께 proxy_pass 지시문을 넣는다.  

```
server {
    location / {
        proxy_pass http://localhost:8080;
    }
    
    location /image/ {
        root /data;
    }
}
```

일반적인 파일 확장자를 가진 이미지 요청과 일치하도록 두번째 location 블록을 수정한다.  

```
location ~ \.(gif|jpg|png)$ {
    root /data/images;
}
```

gif, jpg 또는 png 확장자로 끝나는 URI와 일치하는 정규식이다. 정규식은 ~ 으로 시작해야 한다. 해당 요청은 /data/images 디렉토리에 매핑된다.  

nginx는 요청을 처리할 location 블록을 선택할 때, 먼저 접두사를 지정하는 location 지시문을 확인한다. 그 중에 가장 긴 location을 기억한 다음에 정규식을 확인한다. 일치하는 정규식이 있는 경우 nginx는 이 location을 선택하거나 이전에 기억된 위치를 선택한다.  

프록시 서버 구성의 결과는 다음과 같다.  

```
server {
    location / {
        proxy_pass http://localhost:8080/;
    }
    
    location ~ \.(gif|jpg|png)$ {
        root /data/images;
    }
} 
```

이 서버는 gif, jpg, png 확장자로 끝나는 요청을 먼저 필터링한다. 그리고 /data/images 디렉터리에 매핑하고 위에 구성된 프록시 서버에 다른 모든 요청을 전달한다.  

## FastCGI 프록싱 설정

nginx는 PHP와 같은 다양한 프레임 워크 및 프로그래밍 언어로 구축된 애플리케이션을 실행하는 FastCGI 서버로 요청을 라우팅하는데 사용할 수 있다.  

FastCGI 서버와 함께 작동할때에는 proxy_pass 지시어 대신 fastcgi_pass 지시문을 사용하고, fastcgi_param 지시문으로 매개변수를 설정한다. 만약에 FastCGI 서버가 localhost:9000에 액세스할 수 있다고 가정하고, 이전에 구성한 프록시 서버 구성을 변경해본다. 먼저 proxy_pass를 fastcgi_pass로 바꾸고, 매개변수를 localhost:9000으로 변경한다. 그리고 fastcgi_param으로 SCRIPT_FILENAME과 QUERY_STRING을 설정한다. PHP에서 SCRIPT_FILENAME은 스크립트 이름을 결정하고, QUERY_STRING은 요청 매개변수를 전달한다.  

```
server {
    location / {
        fastcgi_pass  localhost:9000;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param QUERY_STRING    $query_string;
    }

    location ~ \.(gif|jpg|png)$ {
        root /data/images;
    }
}
```

정적 이미지 요청을 제외한 나머지 모든 요청을 FastCGI 프로토콜을 통해 localhost:9000에서 작동하는 프록시 서버로 설정되었다.  


참조 : https://nginx.org/en/docs/beginners_guide.html, https://prohannah.tistory.com/136, 
