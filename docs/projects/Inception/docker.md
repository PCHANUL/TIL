---
layout: default
title: Docker
nav_order: 3
grand_parent: Projects
parent: Inception
permalink: /docs/projects/Inception/docker
---

* [Docker](#docker)
	* [Container](#container)
	* [Image](#image)
	* [Layer](#layer)

# Docker
도커는 컨테이너 기반의 오픈소스 가상화 플랫폼이다.  

다양한 프로그램, 실행 환경을 컨테이너로 추상화하고 동일한 인터페이스를 제공하여 프로그램의 배포 및 관리를 단순하게 해준다. 어떤 프로그램이든지 컨테이너로 추상화할 수 있고, 어디에서든 실행할 수 있다.  

## Container
컨테이너는 격리된 공간에 프로세스가 동작하는 기술이다.  

기존에는 주로 OS를 가상화했다. 이 방식은 비교적 사용법이 간단하지만 무겁고, 느려서 운영환경에서는 사용할 수 없었다. 상황을 개선하기 위해 CPU 가상화 기술을 이용하거나 반가상화 방식이 등장한다. 이 방식은 전체 OS를 가상화하지 않고 게스트 OS를 사용하여 호스트형 가상화 방식에 비해 성능이 향상된다. 그러나 반가상화도 추가적인 OS를 설치하여 가상화하기 때문에 성능문제가 있었다. 그래서 프로세스를 격리하는 방식이 등장한다.  

리눅스에서는 단순히 프로세스를 격리시키기 때문에 가볍고 빠르게 동작한다. 프로세스가 필요한 만큼만 CPU나 메모리를 사용하며 성능적으로 손실이 거의 없다. 하나의 서버에 여러개의 컨테이너를 실행하여도 독립적으로 실행되기 때문에 가벼운 가상머신을 사용하는 듯하다.  

컨테이너는 오랫동안 Linux에 있던 기능인 kernel namespace와 cgroup을 활용하여 프로세스를 격리시킨다. [Kernel Space](/TIL/docs/projects/Inception/kernel_space)  

## Image
이미지는 컨테이너 실행에 필요한 파일과 설정값 등을 포함하고 있다.  

이미지는 상태값을 가지지 않고, 변하지 않는다. 컨테이너는 이미지를 실행한 상태라고 볼 수 있다. 추가되거나 변하는 값은 컨테이너에 저장된다. 같은 이미지에서 여러개의 컨테이너를 생성할 수 있고 컨테이너의 상태가 바뀌거나 컨테이너가 삭제되더라도 이미지는 변하지 않고 그대로 남아있다.  

이미지는 컨테이너를 실행하기 위한 모든 정보를 가지고 있다. 예를 들어 MySQL 이미지는 MySQL을 실행하는데 필요한 파일과 실행 명령어, 포트 정보 등을 가지고 있다. 만약에 새로운 서버가 추가된다면 미리 만들어 놓은 이미지를 다운받고 컨테이너를 생성하면 된다.  

## Layer
만약에 도커 이미지가 수정된다면 이미지를 다시 다운받아야 한다. 이 문제를 위해 도커는 레이어라는 개념을 사용한다. 유니온 파일 시스템을 이용하여 여러개의 레이어를 하나의 파일 시스템으로 사용할 수 있게 해준다. 이미지는 여러개의 읽기 전용 레이어로 구성되고 파일이 추가되거나 수정되면 새로운 레이어가 생성된다.  

예를 들어, ubuntu 이미지가 A + B + C의 집합이라면 ubuntu 이미지를 베이스로 만든 nginx 이미지는 A + B + C + nginx 가 된다. webapp 이미지를 nginx 이미지 기반으로 만들었다면 A + B + C + nginx + source 레이어로 구성된다. 만약에 webapp 소스를 수정한다면 A, B, C, nginx 레이어를 제외한 레이어를 다운받으면 된다.  

컨테이너는 생성될 때 기존의 이미지 레이어 위에 read-write 레이어를 추가한다. 이미지 레이어를 그대로 사용하고, 변경된 내용은 read-write 레이어에 저장되기 때문에 여러개의 컨테이너를 생성해도 최소한의 용량만을 사용한다.  