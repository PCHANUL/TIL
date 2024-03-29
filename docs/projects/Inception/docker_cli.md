---
layout: default
title: Docker CLI
nav_order: 3
grand_parent: Projects
parent: Inception
permalink: /docs/projects/Inception/docker_cli
---

* [Docker CLI](#docker-cli)
	* [run](#run)
		* [-d : 컨테이너 백그라운드 실행](#-d--컨테이너-백그라운드-실행)
		* [-it : 컨테이너를 종료하지 않고, 터미널 입력을 컨테이너에 전달](#-it--컨테이너를-종료하지-않고-터미널-입력을-컨테이너에-전달)
		* [-name : 컨테이너 이름](#-name--컨테이너-이름)
		* [-e : 환경변수 설정](#-e--환경변수-설정)
		* [-p : 호스트와 컨테이너 간의 포트 설정](#-p--호스트와-컨테이너-간의-포트-설정)
		* [-v : 호스트와 컨테이너 간의 볼륨 설정](#-v--호스트와-컨테이너-간의-볼륨-설정)
		* [-w : 컨테이너의 작업 디렉터리 설정](#-w--컨테이너의-작업-디렉터리-설정)

# Docker CLI

https://docs.docker.com/engine/reference/commandline/docker/   

## run 

https://www.daleseo.com/docker-run/  

```
$ docker run (<옵션>) <이미지 식별자> (<명령어>) (<인자>)
```

컨테이너를 실행하기 위한 명령어이다. 이미지 식별자는 필수이며 이미지 ID나 레파지토리, 태그를 사용할 수 있다.  

### -d : 컨테이너 백그라운드 실행

컨테이너를 백그라운드에서 실행하는 옵션이다. -d 옵션을 사용하면 컨테이너가 detached 모드에서 실행된다.  

### -it : 컨테이너를 종료하지 않고, 터미널 입력을 컨테이너에 전달

-i 옵션과 -t 옵션을 같이 사용하여 컨테이너를 종료하지 않고, 터미널의 입력을 컨테이너로 전달한다. 컨테이너의 shell이나 CLI 도구를 사용할 때 유용하게 사용된다.  

### -name : 컨테이너 이름

--name 옵션으로 이름을 부여하여 컨테이너를 제어하기 쉽게한다. docker kill이나 docker rm 명령어를 사용할 때 컨테이너 이름을 사용할 수 있다.  

### -e : 환경변수 설정

-e 옵션으로 컨테이너 환경변수를 설정한다. Docker ENV 설정도 덮어써지게 된다.  

### -p : 호스트와 컨테이너 간의 포트 설정

-p 옵션은 호스트에서 컨테이너에서 리스닝하고 있는 포트로 접속할 수 있도록 설정해준다. 포트의 배포, 바인드를 위해서 사용된다.  

### -v : 호스트와 컨테이너 간의 볼륨 설정

-v 옵션은 호스트의 파일 시스템의 특정 경로를 컨테이너의 파일 시스템의 특정 경로로 마운트해준다.  

### -w : 컨테이너의 작업 디렉터리 설정

-w 옵션은 작업 디렉터리를 설정한다. Dockerfiledml WORKDIR 설정을 덮어쓴다.  


