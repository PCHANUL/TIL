---
layout: default
title: Docker compose
nav_order: 3
grand_parent: Projects
parent: Inception
permalink: /docs/projects/Inception/docker_compose
---

* [Docker Compose](#docker-compose)
* [Key features of Docker Compose](#key-features-of-docker-compose)
  * [단일 호스트에 여러 개의 격리된 환경](#단일-호스트에-여러-개의-격리된-환경)
  * [컨테이너 생성 시 볼륨 데이터 보존](#컨테이너-생성-시-볼륨-데이터-보존)
  * [변경된 컨테이너만 재생성](#변경된-컨테이너만-재생성)
  * [변수 지원 및 환경 간 컴포지션 이동](#변수-지원-및-환경-간-컴포지션-이동)
* [The Compose application model](#the-compose-application-model)
  * [example](#example)


# Docker Compose

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

# Key features of Docker Compose

## 단일 호스트에 여러 개의 격리된 환경

Compose는 프로젝트 이름을 사용하여 환경을 서로 격리한다. 여러 다른 컨텍스트에서 이 프로젝트 이름을 사용할 수 있다.  
- 프로젝트의 각 기능 분기에 대한 안정적인 복사본을 실행하려는 경우처럼 개발 호스트에서 단일 환경의 여러 복사본을 만들기 위해 사용한다.
- CI 서버에서 빌드가 서로 간섭하지 않도록 프로젝트 이름을 고유한 빌드 번호로 설정할 수 있다.
- 공유 호스트 또는 개발 호스트에서 동일한 서비스 이름을 사용할 수 있는 서로 다른 프로젝트가 서로 간섭하지 않도록 한다. 

## 컨테이너 생성 시 볼륨 데이터 보존

Compose는 서비스에서 사용하는 모든 볼륨을 보존한다. 실행 시 이전 컨테이너에서 새 컨테이너로 볼륨을 복사하여 볼륨에서 만든 모든 데이터가 손실되지 않도록 한다.  

## 변경된 컨테이너만 재생성

Compose는 컨테이너를 만드는 데 사용되는 구성을 캐시한다. 변경되지 않은 서비스를 다시 시작하면 Compose는 기존 컨테이너를 재사용한다.  

## 변수 지원 및 환경 간 컴포지션 이동

Compose는 Compose 파일의 변수를 지원한다. 이러한 변수를 사용하여 다양한 환경 또는 다양한 사용자에 맞게 구성을 사용자 지정할 수 있다.  

# The Compose application model

참조 : https://docs.docker.com/compose/compose-file/  

Compose 사양를 사용하면 플랫폼에 구애받지 않는 컨테이너 기반 애플리케이션을 정의할 수 있다. 애플리케이션은 적절한 공유 리소스 및 통신 채널과 함께 실행되어야 하는 컨테이너 집합으로 설계된다.  

애플리케이션의 구성 요소는 services로 정의된다. services는 동일한 컨테이너 이미지를 한 번 이상 실행하여 플랫폼에서 구현되는 추상적인 개념이다.  

services는 networks를 통해 서로 통신한다. networks는 함께 연결된 services 내의 컨테이너 간에 IP 경로를 설정하기 위한 플랫폼 기능 추상화이다. 

services는 영구 데이터를 volumes에 저장하고 공유한다. volumes은 전역 옵션이 있는 상위 수준 파일 시스템 마운트와 같은 영구 데이터를 설명한다.  

## example

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
