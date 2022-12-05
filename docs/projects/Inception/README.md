---
layout: default
title: Inception
nav_order: 3
parent: Projects
has_children: true
permalink: /docs/projects/Inception
---

* [Inception](#inception)
* [Todos](#todos)



# Inception

이 프로젝트는 Docker를 사용하여 시스템 관리에 대한 지식을 넓히는 것을 목표로 한다. 여러 Docker 이미지를 가상화하여 새 개인용 가상 머신에 만든다.  

- 이 프로젝트는 가상 머신에서 완료되어야 한다.
- 구성에 필요한 모든 파일은 srcs 폴더에 있어야 한다.
- Makefile도 필요하며 디렉터리의 루트에 있어야 한다. 전체 애플리케이션을 설정해야 한다. 즉, docker-compose.yml을 사용하여 Docker 이미지를 빌드해야 한다.  

이 프로젝트는 특정 규칙에 따라 가양한 서비스로 구성된 소규모 인프라를 설정하는 것으로 구성된다. 전체 프로젝트는 가상 머신에서 수행되어야 한다. docker-compose를 사용해야 한다.  

각 Docker 이미지는 해당 서비스와 이름이 같아야 한다. 각 서비스는 전용 컨테이너에서 실행되어야 한다. 성능 문제의 경우 컨테이너는 Alpine Linux의 두 번째 안정 버전 또는 Debian Buster에서 빌드해야 한다.  
또한 서비스당 하나씩 자체 Dockerfile을 작성해야 한다. Dockerfile은 Makefile에 의해 docker-compose.yml에서 호출되어야 한다.  
즉, 프로젝트의 Docker 이미지를 직접 빌드해야 한다. 그런 다음 DockerHub(이 규칙에서 Alpine/Debian은 제외됨)와 같은 서비스를 사용하는 것뿐만 아니라 기성품 Docker 이미지를 가져오는 것이 금지된다.  

그리고 다음을 설정해야 한다.

- TLSv1.2 또는 TLSv1.3만 있는 NGINX를 포함하는 Docker 컨테이너
- NGINX 없이 WordPress + php-fpm만 포함하는 Docker 컨테이너(설치 및 구성해야함)
- NGINX 없이 MariaDB만 포함하는 Docker 컨테이너
- WordPress 데이터베이스가 포함된 볼륨
- WordPress 웹 사이트 파일이 포함된 두 번째 볼륨
- 컨테이너 간의 연결을 설정하는 Docker-network이며, 충돌이 발생할 경우 컨테이너를 다시 시작해야 한다.
  
> Docker 컨테이너는 가상 머신이 아니다. 따라서 실행하려고 할 때 'tail -f' 등을 기반으로 하는 해키 패치를 사용하지 않는 것이 좋다. 데몬이 어떻게 동작하는지 읽어보고, 데몬을 사용하는 것이 좋은지 생각해보아야 한다.  

> 네트워크 라인은 docker-compose.yml 파일에 있어야 합니다. host 또는 -link, links를 사용하는 것은 금지되어 있다. 무한 루프를 실행하는 명령으로 컨테이너를 시작하면 안된다. 따라서 이는 entrypoint로 사용되거나 entrypoint script에서 사용되는 모든 명령에도 적용된다. 다음은 금지된 해키 패치이다. tail -f, bash, sleep infinity, while true.

> PID 1 및 Dockerfile 작성 모범 사례에 대해 읽어보자.

WordPress 데이터베이스에는 두 명의 사용자가 있어야 하며, 그 중 한 명은 관리자이다. 관리자의 사용자 이름에는 admin/Admin 또는 administrator/Administrator가 포함될 수 없다.

> Docker를 사용하는 호스트 시스템의 /home/login/data 폴더에서 볼륨을 사용할 수 있다. 물론 로그인을 본인의 것으로 교체해야 한다.

작업을 더 간단하게 하려면 로컬 IP 주소를 가리키도록 도메인 이름을 구성해야 한다.  
이 도메인 이름은 login.42.ft이어야 한다. 반드시 자신의 로그인을 사용해야 한다. 예를 들어, 로그인인 wil인 경우에 wil.42.fr은 wil의 웹사이트를 가리키는 IP 주소로 리디렉션된다.  

> 최신 태그는 금지되어 있다. Dockerfile에 암호가 없어야 한다. 환경 변수를 사용하는 것은 필수이다. 또한, .env 파일을 사용하여 환경 변수를 저장하는 것이 좋다. .env 파일을 srcs 디렉토리의 루트에 있어야 한다. NGINX 컨테이너는 TLSv1.2 또는 TLSv1.3 프로토콜을 사용하여 포트 443가 인프라에 대한 유일한 진입점이어야 한다.

다음은 예상되는 결과의 다이어그램이다.  

![](../../src/projects/inception/inception01.png)  

아래는 예상되는 결과의 디렉터리 구조이다.  

![](../../src/projects/inception/inception02.png) . 


# Todos

- [ ] Dockerfile 작성
  - [ ] MariaDB
  - [ ] WordPress
  - [ ] NGINX
- [ ] docker.compose.yml 작성
- [ ] Docker-network 컨테이너 간의 연결 설정
- [ ] WordPress 데이터 베이스 사용자 이름 설정
- [ ] 호스트 시스템 로그인 설정
- [ ] NGINX 컨테이너 entrypoint port
- [ ] Makefile 작성



