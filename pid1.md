---
layout: default
title: PID 1
nav_order: 3
grand_parent: Projects
parent: Inception
permalink: /docs/projects/Inception/pid1
---

* [Docker Container PID 1](#docker-container-pid-1)
* [PID 1](#pid-1)
	* [PID 1, 신호 처리, 좀비 프로세스 올바르게 처리하기](#pid-1-신호-처리-좀비-프로세스-올바르게-처리하기)
	* [Solution 1 : PID 1으로 실행하고 신호 핸들러로 등록](#solution-1--pid-1으로-실행하고-신호-핸들러로-등록)
	* [Solution 2 : Kubernetes에서 프로세스 네임 스페이스 공유 사용 설정](#solution-2--kubernetes에서-프로세스-네임-스페이스-공유-사용-설정)
	* [Solution 3 : 특수한 init 시스템 사용](#solution-3--특수한-init-시스템-사용)
* [dumb-init](#dumb-init)
* [dumb-init 사용법](#dumb-init-사용법)


# Docker Container PID 1

# PID 1
https://cloud.google.com/architecture/best-practices-for-building-containers?hl=ko#signal-handling  

리눅스에서 PID 1은 부팅 시 커널에 의해 최초로 실행되는 init 프로세스이다. init 프로세스는 추가 하위 프로세스를 생성할 수 있다. 결국 모든 프로세스의 최종 부모 프로세스 역할을 한다. 현재 배포판들은 init 대신 systemd가 초기화 시스템의 역할을 대신하고 있다.  

만약에 어떠한 프로세스가 예기치 못한 상황으로 종료되면 좀비 프로세스로 변한다. 좀비 프로세스는 부모 프로세스가 waitpid 시스템 명령을 수행할 때까지 존재하며 이후에 제거된다. 일반적으로 자식 프로세스가 종료되면 운영 체제에서 SIGCHLD 신호를 보내어 부모 프로세스를 깨우고 자식 프로세스를 거두게 되므로 문제가 되지 않는다.  

그렇다면 부모 프로세스가 의도적으로 종료되거나 사용자가 프로세스를 종료시켰다고 가정한다면 자식 프로세스들은 고아 상태가 된다. init 프로세스는 고아 상태가 된 자식 프로세스를 거두는 역할을 한다. init 프로세스가 생성하지 않았지만 고아 프로세스가 좀비 프로세스가 되지 않도록 정리한다. 그러나 컨테이너 환경의 경우에는 다르다.

## PID 1, 신호 처리, 좀비 프로세스 올바르게 처리하기

Linux 신호는 컨테이너 내부의 프로세스 수명 주기를 제어하는 주요 방법이다. 앱의 수명 주기를 앱이 포함된 컨테이너와 긴밀하게 연결하려면 앱이 Linux 신호를 올바르게 처리하도록 해야 한다. 가장 중요한 Linux 신호는 프로세스를 종료하는 SIGTERM이다. 이외에도 SIGKILL과 SIGINT 신호를 수신할 수 있다.  

프로세스 식별자(PID)는 Linux 커널이 각 프로세스에 제공하는 고유한 식별자이다. PID는 네임 스페이스이므로 컨테이너에는 호스트 시스템의 PID에 매핑되는 고유한 PID 세트가 있다. Linux의 첫 번째 프로세스는 PID 1이며, init시스템이다. 컨테이너의 첫 번째 프로세스도 PID 1이며, Docker와 Kubernetes가 컨테이너 내부의 프로세스와 통신하거나 프로세스를 종료한다. Docker와 Kubernetes는 모두 컨테이너 내부에 PID 1이 있는 프로세스에만 신호를 보낼 수 있다.  

컨테이너의 측면에서 PID와 Linux 신호는 2가지 문제를 제기한다.  

1. Linux 커널이 신호를 처리하는 방법

Linux 커널이 신호를 처리하는 방법은 PID 1을 가진 프로세스와 그렇지 않은 프로세스에서 차이가 있다. 신호 핸들러가 이 프로세스에 자동으로 등록되지 않으므로 SIGTERM 또는 SIGINT 같은 신호는 기본적으로 아무런 영향을 미치지 않는다. 기본적으로, 단계적 종료를 방지하는 SIGKILL을 사용하여 프로세스를 강제 종료해야 한다. 앱에 따라 SIGKILL을 사용하면 모니터링 시스템에 사용자 표시 오류, 쓰기 중단(데이터 저장용), 원치 않는 알림이 발생할 수 있다.  

2. 기본 init 시스템이 분리된 프로세스를 처리하는 방법

systemd와 같은 기본 init 시스템은 분리된 좀비 프로세스를 제거(거둘 때)하는 데에도 사용된다. 분리된 프로세스(상위 요소가 사라진 프로세스)는 PID 1이 있는 프로세스에 다시 첨부된다. PID 1은 프로세스가 사라질 때 다시 거둬야 한다. 정상적인 init 시스템은 그렇게 작동한다. 그러나 컨테이너에서는 PID 1을 갖고 있는 프로세스가 이러한 책임을 갖게 된다. 이 프로세스에서 이러한 제거를 제대로 처리하지 못하면 메모리나 다른 리소스가 부족해질 수 있다.  

## Solution 1 : PID 1으로 실행하고 신호 핸들러로 등록

이 솔루션은 첫 번째 문제만 해결한다. 앱이 제어된 방식으로 하위 프로세스를 생성하면 두 번째 문제를 방지할 수 있다. 구현하는 방법은 Dockerfile에서 CMD 또는 ENTRYPOINT를 사용하여 프로세스를 실행하는 것이다.  

```
FROM debian:9

RUN apt-get update && \
    apt-get install -y nginx
    
EXPOSE 80

CMD [ "nginx", "-g", "daemon off;" ]
```

때로는 프로세스가 제대로 실행될 수 있도록 컨테이너에서 환경을 준비해야 할 수 있다. 이 경우 컨테이너를 시작할 때 셸 스크립트를 실행하여 환경을 준비하고 기본 프로세스를 실행한다. 이 방법을 사용하는 경우 셸 스크립트는 프로세스가 아닌 PID 1을 가지므로 기본 exec 명령어를 사용하여 셸 스크립트에서 프로세스를 실행해야 한다. exec 명령어로 스크립트를 원하는 프로그램으로 바꾼다. 그 다음 프로세스에서 PID 1을 상속한다.  

## Solution 2 : Kubernetes에서 프로세스 네임 스페이스 공유 사용 설정

pod에 프로세스 네임스페이스 공유를 사용 설정하면 Kubernetes는 해당 pod의 모든 컨테이너에 단일 프로세스 네임 스페이스를 사용한다. Kubernetes pod 인프라 컨테이너가 PID 1이 되고 분리된 프롯세스는 자동으로 다시 수거된다. 

## Solution 3 : 특수한 init 시스템 사용

기본적인 Linux 환경처럼 init 시스템을 사용하여 문제를 처리할 수 있다. 하지만 systemd 같은 일반 init 시스템을 사용하기에는 너무 복잡하고 크기 때문에 컨테이너용으로 제작된 init 시스템을 사용하는 것이 좋다.  

init 시스템을 사용하는 경우 init 프로세스는 PID 1을 가지며 다음을 수행한다.  
- 올바른 신호 핸들러를 등록한다.
- 앱에서 신호가 작동하는지 확인한다.
- 최종 모든 좀비 프로세스를 수거한다.

docker run 명령어의 --init 옵션을 사용하면 Docker 자체에서 init 시스템을 사용할 수 있다. Kubernetes에서 사용하려면 컨테이너 이미지에 init 시스템을 설치하고 컨테이너의 진입점으로 사용해야 한다.  

# dumb-init

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

# dumb-init 사용법

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
