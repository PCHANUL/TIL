---
layout: default
title: Dockerfile reference
nav_order: 3
grand_parent: Projects
parent: Inception
permalink: /docs/projects/Inception/dockerfile_ref
---

* [Dockerfile reference](#dockerfile-reference)
	* [FROM](#from)
	* [RUN](#run)
	* [ADD](#add)
	* [VOLUME](#volume)
	* [EXPOSE](#expose)
	* [ENTRYPOINT](#entrypoint)

# Dockerfile reference

https://docs.docker.com/engine/reference/builder/  

## FROM

```
FROM [--platform=<platform>] <image> [AS <name>]
FROM [--platform=<platform>] <image>[:<tag>] [AS <name>]
FROM [--platform=<platform>] <image>[@<digest>] [AS <name>]
```

FROM 명령어는 새 빌드 단계를 초기화하고 후속 명령어에 대한 Base Image를 설정한다. 

## RUN

RUN은 두가지 형식이 있다.  

- `RUN <command>` (shell 형식, 명령은 셸에서 실행되며 기본적으로 Linux에서는 /bin/sh -c, Windows에서는 cmd /S /C 이다.)
- `RUN ["executable", "param1", "param2"]` (exec 형식)

RUN 명령은 현재 이미지 위에 있는 새 레이어에서 모든 명령을 실행하고 결과를 커밋한다. 커밋된 결과 이미지는 Dockerfile의 다음 단계에 사용된다.  

exec 형식을 사용하면 셸 문자열 변경을 방지하고 지정된 셸 실행 파일이 포함되지 않은 기본 이미지를 사용하여 명령을 실행할 수 있다.  

shell 형식에서는 `\`(백슬래시)를 사용하여 단일 RUN 명령을 다음 줄로 계속 실행할 수 있습니다.  

## ADD

```
ADD [--chown=<user>:<group>] [--checksum=<checksum>] <src>... <dest>
ADD [--chown=<user>:<group>] ["<src>",... "<dest>"]
```

ADD 명령은 <src>에서 새 파일, 디렉터리 또는 원격 파일 URL을 복사하여 <dest> 경로에 있는 이미지의 파일 시스템에 추가한다.  

## VOLUME

```
VOLUME ["/data"]
```

VOLUME에 지정된 이름으로 마운트 지점을 생성하고 기본 호스트 또는 다른 컨테이너에서 외부 마운트된 볼륨을 보유하는 것으로 표시한다. docker run 명령은 기본 이미지 내의 지정된 위치에 있는 데이터로 새로 생성된 불륨을 초기화한다.  

[docker volume의 사용 방법](https://darkrasid.github.io/docker/container/volume/2017/05/10/docker-volumes.html)  

container의 데이터 휘발성 때문에 volume을 사용하게 된다. Dockerfile에서 volume을 다음과 같이 선언할 수 있다.  

```
FROM ubuntu:latest

VOLUME ["/volume/path"]
```

volume이 선언된 이미지는 container로 올라갈 때 자동으로 해당 경로를 host에 연결한다. 경로는 `/var/lib/docker/volumes/{volume_name}`에 생성된다. volume_name으로는 docker가 생성한 hash 값이 들어가기 때문에 어떤 container의 volume인지 알기 어렵다. 

## EXPOSE

```
EXPOSE <port> [<port>/<protocol>...]
```

EXPOSE 명령은 컨테이너가 런타임에 지정된 네트워크 포트에서 수신 대기함을 Docker에 알린다. 포트가 TCP 또는 UDP에서 수신하는지 여부를 지정할 수 있으며 프로토콜이 지정되지 않은 경우 기본값은 TCP이다.  

EXPOSE 명령이 실제로 포트를 게시하지 않지만 이미지를 빌드하는 사람과 컨테이너를 실행하는 사람 사이에서 게시할 포트에 대한 일종의 문서 역할을 한다. 실제로 포트를 게시하려면 docker run 명령어에 -p 플래그를 사용한다.  

## ENTRYPOINT

```
ENTRYPOINT ["executable", "param1", "param2"]
ENTRYPOINT command param1 param2
```

ENTRYPOINT를 사용하면 해당 컨테이너가 수행하게 될 실행 명령을 정의한다. 컨테이너 실행 시 받은 인자와 상관없이 실행 명령을 수행한다.   

[ENTRYPOINT와 CMD의 차이점](https://bluese05.tistory.com/77)  
