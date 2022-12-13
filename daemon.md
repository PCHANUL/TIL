---
layout: default
title: Daemon
nav_order: 3
grand_parent: Projects
parent: Inception
permalink: /docs/projects/Inception/daemon
---

* [daemon](#daemon)

# daemon

https://blogger.pe.kr/770  

daemon은 서비스의 요청에 응답하기 위해 실행 중인 background 프로세스이다.  

프로세스는 foreground와 background 프로세스로 나뉜다. 표준 입출력 장치를 통해서 대화하는 프로세스는 foreground 프로세스이고, 적어도 입력장치에 대하여 연결되지 않는 프로세스를 background 프로세스라고 한다.  

예를 들어, 사용자가 어떠한 서버에 ssh로 접속한다고 가정해본다. 프롬프트에 ssh을 입력하면 명령이 실행되고, 입력받은 IP주소로 접속을 시도한다. 서버에서 실행 중인 ssh daemon과 세션을 맺은 다음에 ssh daemon이 사용자에게 보여주길 원하는 내용을 ssh 명령이 받아서 화면에 출력한다. 즉, ssh 명령은 입력받은 내용을 처리하고, 전달받은 내용을 표시하기 때문에 foreground 프로세스이다. 그 다음, 프롬프트를 보면 서버의 ssh daemon이 사용자의 입력을 기다리고 있는 것처럼 보인다. 화면에는 ssh daemon이 아니라 ssh daemon이 실행한 bash가 프롬프트에 보이는 것이다.  

daemon 프로세스는 background 프로세스이며, 부모 프로세스가 PID 1이거나 다른 deamon인 프로세스를 말한다. 이를 알 수 있는 방법은 background 프로세스가 자신을 실행한 bash가 종료되었을 때 같이 종료되는지 확인하면 된다.  

대표적인 daemon 프로세스는 웹서버 daemon이다. 웹서버 daemon 프로세스는 서버에서 터미널을 통해 실행될 수 있지만 사용자와 대화할 필요가 없기 때문에 background 프로세스로 생성되도록 만들어진다. 즉, 프로그램이 fork 함수를 통해 자식 프로세스를 생성하고 부모 프로세스는 죽는다. 그리고 생성된 자식은 부모 프로세스를 PID 1로 변경한 뒤 실제로 서비스를 수행할 자식 프로세스를 여러개 fork한다. 그리고 그 손자 프로세스들은 계정을 setuid 함수를 이용하여 웹서버가 실행되도록 설정된 계정을 바꾼다.  
