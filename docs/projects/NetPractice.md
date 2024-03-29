---
layout: default
title: NetPractice
parent: Projects
nav_order: 2
permalink: /docs/projects/NetPractice
---

- [NetPractice](#netpractice)
  - [네트워크란](#네트워크란)
  - [네트워크 종류](#네트워크-종류)
  - [네트워크 회선 구성 방식](#네트워크-회선-구성-방식)
  - [네트워크 데이터 교환 방식](#네트워크-데이터-교환-방식)
  - [OSI 7계층](#osi-7계층)
    - [데이터 캡슐화](#데이터-캡슐화)
    - [1계층 물리 계층(Physical Layer)](#1계층-물리-계층physical-layer)
    - [2계층 데이터 링크 계층(Data Link Layer)](#2계층-데이터-링크-계층data-link-layer)
    - [3계층 네트워크 계층(Network Layer)](#3계층-네트워크-계층network-layer)
    - [4계층 전송 계층(Transport Layer)](#4계층-전송-계층transport-layer)
    - [5계층 세션 계층(Session Layer)](#5계층-세션-계층session-layer)
    - [6계층 표현 계층(Presentation Layer)](#6계층-표현-계층presentation-layer)
    - [7계층 응용 프로그램 계층(Application Layer)](#7계층-응용-프로그램-계층application-layer)
  - [네트워크 장비](#네트워크-장비)
    - [1. 스위치](#1-스위치)
      - [패킷 전송 방식](#패킷-전송-방식)
    - [2. 라우터](#2-라우터)
      - [패킷 라우팅 방식](#패킷-라우팅-방식)
      - [- 경로 지정](#--경로-지정)
      - [- 브로드캐스트 컨트롤](#--브로드캐스트-컨트롤)
      - [- 프로토콜 변환](#--프로토콜-변환)
  - [TCP/IP (Transmission Control Protocol/Internet Protocal)](#tcpip-transmission-control-protocolinternet-protocal)
    - [TCP/IP 용어](#tcpip-용어)
    - [TCP/IP 네트워크 계획](#tcpip-네트워크-계획)
    - [TCP와 UDP의 차이점](#tcp와-udp의-차이점)
    - [TCP/IP 프로토콜](#tcpip-프로토콜)
    - [TCP/IP 4 계층](#tcpip-4-계층)
      - [1계층 네트워크 엑세스 계층(Network Access Layer)](#1계층-네트워크-엑세스-계층network-access-layer)
      - [2계층 인터넷 계층(Internet Layer)](#2계층-인터넷-계층internet-layer)
      - [3계층 전송 계층(Trasport Layer)](#3계층-전송-계층trasport-layer)
      - [4계층 응용 계층(Application Layer)](#4계층-응용-계층application-layer)
    - [TCP/IP가 정보를 발신자에서 수신자로 이동하는 방법](#tcpip가-정보를-발신자에서-수신자로-이동하는-방법)
    - [TCP/IP 주소 지정](#tcpip-주소-지정)
    - [인터넷 주소](#인터넷-주소)
    - [서브넷 주소](#서브넷-주소)
    - [서브넷 마스크](#서브넷-마스크)
    - [서브넷 마스크 예시](#서브넷-마스크-예시)
    - [주소 비교](#주소-비교)
    - [특수 IP 주소](#특수-ip-주소)
  - [소규모 네트워크 구성](#소규모-네트워크-구성)
  - [정리](#정리)
    - [왜 TCP/IP 주소를 사용하는가?](#왜-tcpip-주소를-사용하는가)
    - [TCP/IP는 무엇인가?](#tcpip는-무엇인가)
    - [인터넷 주소](#인터넷-주소-1)
    - [네트워크 주소와 호스트 주소를 어떻게 구별하는가?](#네트워크-주소와-호스트-주소를-어떻게-구별하는가)
    - [라우팅 테이블](#라우팅-테이블)


# NetPractice 

TCP/IP 주소 지정이 작동하는 방식을 이해하고, 소규모 네트워크를 구성한다.  

## 네트워크란

- 네트워크는 둘 이상의 컴퓨터와 이들을 연결하는 링크의 조합이다.  
- 네트워크는 몇 개의 독립적인 장치가 적절한 영역내에서 적당히 빠른 속도의 물리적 통신 채널을 통하여 서로가 직접 통신할 수 있도록 지원해 주는 데이터 통신 체계이다.   

현대식 컴퓨터 네트워크의 복잡도로 인해 네트워크의 작업 방법을 설명하기 위한 여러 개념적 모델이 등장했다. 가장 일반적인 모델이 ISO(International Organization for Standardization)의 개방 시스템 연결 규약 참조 모델이다. OSI 7계층 모델(Open Systems Interconnection model)이라고도 한다. OSI 참조 모델이 네트워킹 개념을 논의하기에 유용하지만 많은 네트워킹 프로토콜이 OSI 모델을 그대로 따르지는 않는다. TCP/IP에서는 애플리케이션 및 프리젠테이션 계층 기능이 결합되며 세션 및 전송 계층 그리고 데이터 링크 및 물리적 계층도 마찬가지이다.  

네트워크를 사용함으로써 파일을 공유하거나 다른 네트워크에 있는 컴퓨터의 파일에 접근할 수 있다. 또한, 사진과 음악 또는 비디오같은 디지털 미디어를 네트워크를 통해서 스트리밍하거나 인터넷에서 다른 사람과 만나서 네트워크 게임을 즐길 수 있게 되었다. 이렇게 큰 장점을 가진 네트워크이지만 이로 인해서 바이러스나 악성코드, 해킹으로 인한 개인 정보 유출 등 보안상의 문제점이 생긴다.  

## 네트워크 종류

- 개인 통신망(Personal Area Network, PAN) : 가장 작은 규모의 네트워크
- 근거리 영역의 네트워크(Local Area Network, LAN)
- 도시권 통신망(Metropolitan Area Network, MAN) : 대도시 영역의 네트워크
- 원거리 통신망(Wide Area Network, WAN) : 광대역 네트워크
- 부가 가치 통신망(Value Added Network, VAN), 광대역 종합정보통신망(BISDN) : 정보의 축적과 제공, 통신 속도와 형식의 변화, 통신 경로의 선택 등 여러 종류의 정보 서비스가 부가된 통신망
- 종합정보통신망(Integrated Service Digital Network, ISDN) : 전화, 팩스, 데이터 통신, 비디오텍스 등 통신 관련 서비스를 종합하여 다루는 통합 서비스 디지털 통신망

## 네트워크 회선 구성 방식

- 투 포인트 방식 : 중앙 컴퓨터와 단말기를 일대일로 연결하여 언제든지 데이터 전송이 가능하게 하는 방식
- 멀티 드롭 방식 : 다수의 단말기를 한 개의 통신 회선에 연결하여 사용하는 방식
- 회선 다중 방식 : 여러 대의 단말기를 다중화 장치를 활용하여 중앙 컴퓨터와 연결하여 사용하는 방식

## 네트워크 데이터 교환 방식

- 회선 교환 : 통신을 원하는 두 지점의 교환기를 이용하여 물리적으로 접속시키는 방법으로써 송신자의 모든 데이터는 동일한 경로로 전송된다. 대표적으로 음성 전화망이 있으며 포인트 투 포인트 방식으로 연결된다.  
- 패킷 교환 : 회선 교환과 다르게 전용선의 개녕이 없다. 데이터를 일괄적으로 한 번에 보내지 않고, 여러 개로 분할해서 송신하는 방법을 말한다. 분할된 데이터를 패킷이라고 한다. 패킷에는 데이터와 최종 목적지에 대한 정보가 들어있어서 라우터가 이를 보고 패킷을 최적 경로를 향해 전달한다.  
- 공간 분할 교환 : 기계식 접점과 전자 교환기의 전자식 접점 등을 이용하여 교환을 수행한다. 기존의 음성용 전화 회선망을 이용할 수 있어서 간단한 저속 데이터 전송에 매우 효과적이다.  
- 시분할 교환 : 전자 부품이 갖는 고속성과 디지털 교환 기술을 이용하여 다수의 디지털 신호를 시분할적으로 동작시켜 다중화하는 방식이다. 이 방식은 데이터 전용 회선 교환 방식에 이용된다.  

## OSI 7계층

OSI 7 계층은 통신이 일어나는 과정을 7단계로 크게 구분하여 정의해놓은 것이다. 컴퓨터 통신 구조의 모델과 앞으로 개발될 프로토콜의 표준적인 뼈대를 제공하기 위해 개발된 참조 모델이므로 알고 있으면 네트워크 구성을 예측하고 이해할 수 있다.  

### 데이터 캡슐화

데이터 캡슐화는 사용자 데이터가 각 계층을 지나면서 하위 계층은 상위 계층으로 부터 온 정보를 데이터로 취급하며, 자신의 계층 특성을 담은 제어정보를 헤더화 시켜서 붙이는 일련의 과정을 말한다. 데이터를 보낼 때는 응용 계층에서 시작되어 OSI 계층을 차례로 내려오며 물리 계층으로 간다. 이 과정에서 캡슐화를 하게 되는데 각 계층은 다른 계층과 통신할 때 데이터에 특정 정보가 들어 있는 머리말과 꼬리말을 추가한 후 다른 계층으로 전달한다.  

![](/TIL/docs/src/projects/network/network_25.jpeg)  

### 1계층 물리 계층(Physical Layer)

물리 계층은 실제 장치를 연결하기 위한 전기적 및 물리적 세부 사항을 정의한 계층이다. 인터넷 케이블, 라우터 스위치 등의 전기적 신호가 물리적인 장치에 의해 통신하는 계층이다. 이 계층은 데이터가 무엇인지 전혀 신경쓰지 않으며 데이터를 전기적인 신호로 변환하여 주고받는 기능만 한다.  

### 2계층 데이터 링크 계층(Data Link Layer)

데이터 링크 계층은 장치 간 신호를 전달하는 물리계층을 이용하여 네트워크 상의 주변 장치들 간의 데이터를 전송하는 역할을 한다. 물리 계층을 통해 송수신되는 정보의 오류와 흐름을 관리하여 안전한 정보의 전달을 수행할 수 있도록 도와준다. 통신에서의 오류를 찾고 재전송하는 기능을 가지고 있으며, 맥 주소를 가지고 통신한다. 데이터 링크 계층에서 전송되는 단위를 프레임이라고 한다. 대표적인 2계층 장비는 브리지, 스위치가 있다.  

### 3계층 네트워크 계층(Network Layer)

네트워크 계층은 경로를 선택하고, 주소를 정하며 경로에 따라 패킷을 전달하는 역할을 한다. 데이터를 다른 네트워크를 통해 전달하기 때문에 인터넷이 가능하게 만드는 계층이다. 가장 중요한 기능은 데이터를 목적지까지 안전하고 빠르게 전달하는 기능이다. 네트워크 관리자가 직접 주소를 할당하는 구조인 논리적인 주소 구조(IP)를 가지며, 계층적이다.  

![](/TIL/docs/src/projects/network/network_22.png)  

### 4계층 전송 계층(Transport Layer)

전송 계층은 양 끝단의 통신을 다루는 최하위 계층이다. 신뢰성있는 데이터를 주고받게하여 상위 계층이 데이터 전달의 유효성이나 효율성을 신경쓰지 않게 해준다. 주로 TCP 프로토콜을 사용하며 포트를 열고 응용프로그램들이 전송할 수 있게 한다. 데이터가 들어왔다면 4계층에서 데이터를 하나로 통합하여 5계층으로 전달한다. 전송 계층은 특정 연결의 유효성을 제어한다. 이는 전송 계층에서 패킷을 확인하고 전송 실패한 패킷을 다시 전송한다는 것을 뜻한다. 대표적인 전송 계층으로 TCP가 있으며, TCP는 3방향 핸드셰이크로 유효성을 확인한다.

|   3-웨이 핸드셰이킹 과정   |     TCP 연결 해제 과정     |
| :------------------------: | :------------------------: |
| ![](/TIL/docs/src/projects/network/network_23.png) | ![](/TIL/docs/src/projects/network/network_24.png) |

### 5계층 세션 계층(Session Layer)

세션 계층은 양 끝단의 응용 프로세스가 통신을 관리하는 방법을 제공한다. 통신 연결이 손실되는 경우에 연결 복구 시도가 가능하며 연결 시도 중에 장시간 연결되지 않는다면 연결을 닫고 다시 시도한다. 세션 계층의 중요한 기능에는 동기화가 있다.  

- 전이중 통신(Full Duplex) : 두 단말기가 각각 독립된 회선을 사용하여 송수신하는 통신 방식
- 반이중 통신(Half Duplex) : 한쪽이 송신하는 동안에 다른쪽에서 수신하며 전송 방향을 바꾸는 통신 방식

### 6계층 표현 계층(Presentation Layer)

표현 계층은 코드 간의 번역을 담당하며 응용프로그램이나 네트워크를 위해 데이터를 표현한다. 데이터 표현에서 독립적인 부분을 나타내고, 일반적으로 응용프로그램 형식을 준비 또는 네트워크 형식으로 변환하거나 네트워크 형식을 응용프로그램 형식으로 변환한다. 예를 들어서 EBCDIC(확장 이진화 십진법 교환 부호)로 인코딩된 문서 파일을 ASCII로 인코딩된 파일로 바꾸어 준다. 해당 데이터가 TEXT인지, GIF인지 JPG인지 구분한다.  

### 7계층 응용 프로그램 계층(Application Layer)

응용 프로그램 계층은 일반적인 응용 서비스를 수행하는 계층이다. 응용 계층은 최상위 계층으로 사용자에게 직접적으로 보이는 부분이며, 최종 사용자에게 가장 가까운 계층이다. 예를 들어서 Chrome, Firefox 같은 웹 브라우저나 Skype, Outlook 같은 응용 프로그램이 있다. 

## 네트워크 장비

### 1. 스위치

스위치는 OSI 2계층(Data Link Layer)의 장치로 패킷을 효율적으로 전송하여 원활한 네트워크 통신이 이루어지도록 한다. 3계층(Network Layer)에서 각 세그먼트에 IP주소를 달아서 2계층으로 넘겨주는데, 2계층에서는 각 세그먼트에 출발지 / 목적지 MAC 주소 헤더를 추가하여 프레임을 구성한다.  

#### 패킷 전송 방식

스위치는 MAC 테이블에 존재하는 도착지 주소를 가진 패킷이 들어오면 매핑된 포트로만 패킷을 전송하고, 테이블에 존재하지 않는 도착지 주소를 가진 패킷이 들어오면 전체 포트로 패킷을 전송한다. 스위치의 동작 방식은 3가지로 정리된다.  

- 플러딩 : MAC 주소 테이블이 완전히 비어있는 경우, 패킷이 들어온 포트를 제외한 모든 포트로 패킷을 전달하는 작업
- 어드레스 러닝 : MAC 테이블을 만들고, 유지하는 작업
- 포워딩 : 들어온 패킷에서 MAC 주소를 확인하고 자신이 가진 MAC 테이블과 비교해 맞는 정보가 있으면 매칭되는 해당 포트로 패킷을 보내는 작업
- 필터링 : 패킷이 들어왔을 때, 주소가 매칭되지 않는 나머지 포트로는 패킷을 보내지 않는 작업

### 2. 라우터

라우터는 OSI 3계층(Network Layer)의 장치로 패킷의 목적지 IP 주소를 확인하고, 해당 주소의 경로를 지정해준다. 라우터는 여러 경로 정보를 가지고 있는데, 들어오는 패킷을 최적의 경로로 목적지까지 도달시키는 방법을 파악하여 포워딩한다. 이를 위해 라우터는 다양한 경로 정보를 수집하며, 라우팅 테이블에 저장한다.  

#### 패킷 라우팅 방식

라우터가 패킷을 라우팅하는 방식은 경로 지정, 브로드캐스트 컨트롤, 프로토콜 변환으로 나뉜다.

#### - 경로 지정

![](/TIL/docs/src/projects/network/network_14.png)

라우터에 패킷이 들어오면 패킷의 도착지 IP를 확인하여 경로를 지정한다. 패킷의 경로 정보를 얻고, 패킷을 포워딩하는 방법은 다음과 같이 세분화할 수 있다. 

- 경로 정보 얻는 방법
  1. 다이렉트 커넥티드 : IP 주소를 입력할 때 인접 네트워크 정보를 자동으로 얻는 방법
  2. 스태틱 라우팅 : 관리자가 직접 정보를 입력하는 방법
  3. 다이나믹 라우팅 : 라우터끼리 정보를 교환하는 방법

- 패킷 포워딩
  - 라우팅 테이블을 확인하여 패킷의 IP 주소에 맞는 넥스트 홉을 찾고, 포워딩한다. 
  - 패킷은 이동하며 여러 개의 라우터를 거치기 때문에 라우터는 다음 라우터까지의 경로를 지정한다. 
  - 네트워크를 한 단계씩 뛰어 넘으며 전달되는 방식을 홉-바이-홉 (Hop-by-Hop) 라우팅이라고 한다. 
  - 라우팅 테이블에서 Next Hop은 가장 인접한 라우터를 의미한다.


#### - 브로드캐스트 컨트롤

라우터는 멀티캐스트 정보를 습득하지 않고, 브로드캐스트 패킷을 전달하지 않는다. 이는 라우터의 주요한 특징 중 하나로써 분명한 도착지 정보가 있는 경우에만 통신을 허락한다. 만약에 패킷의 목적지 주소가 라우팅 테이블에 없다면 패킷을 버린다. 덕분에 패킷이 다른 네트워크로 전달되지 않는다.  


#### - 프로토콜 변환

라우터는 패킷의 2계층까지의 헤더 정보를 벗기고 3계층 주소를 확인하고, 2계층 헤더 정보를 새로 입혀서 내보낸다. 이 기능으로 패킷은 다른 프로토콜로 구성된 네트워크로 이동할 수 있다. 예를 들어서 LAN 기술을 WAN 기술로 변환하여 네트워크를 연결한다.  

- LAN (Local Area Network) : 지역 네트워크이다. 예를 들어 집에 있는 컴퓨터와 전화기, 프린트를 공유기를 통해 연결하는 경우에 LAN을 이용한다.  
- WAN (Wide Area Network) : LAN과 LAN의 사이를 구성하는 네트워크이다. 만약에 외부에서 집에 구성한 LAN에 접속하려는 경우에 ISP (Internet Service Provider) 업체에서 설치한 LAN선을 사용하여 ISP 네트워크 망에 접속한다.  


## TCP/IP (Transmission Control Protocol/Internet Protocal)

TCP는 인터넷 상에서 데이터를 메세지의 형태로 보내기 위해 IP와 함께 사용하는 프로토콜이다. 컴퓨터가 서로 통신하는 경우, 특정 규칙이나 프로토콜을 사용하여 순서대로 데이터를 전송 및 수신할 수 있다. 전세계적으로 가장 일상적으로 사용되는 프로토콜 세트 중 하나가 TCP/IP이다.  

일반적으로 TCP와 IP를 함께 사용하는데, IP가 데이터의 배달을 처리한다면 TCP는 패킷을 추적하거나 관리한다. TCP는 연결형 서비스를 지원하는 프로토콜로 인터넷 환경에서 기본으로 사용한다. 이에 반해 UDP는 비연결형 프로토콜이다. 데이터를 독립적인 관계를 지니는 패킷인 데이터 그램 단위로 처리한다. 

- TCP/IP는 컴퓨터 사이의 통신 표준 및 네트워크의 라우팅 및 상호연결에 대한 자세한 규칙을 지정하는 프로토콜 스위트이다.
- 네트워크에 연결된 여러 컴퓨터 사이의 통신을 허용한다. 각 네트워크는 호스트와 통신하는 다른 네트워크에 연결될 수 있다. 패킷 교환 및 스트림 전송으로 작동하는 많은 유형의 네트워크 기술이 있지만, TCP/IP는 하드웨어에 구애받지 않는다는 하나의 큰 장점이 있다.  
- TCP/IP는 컴퓨터 시스템을 네트워크에 접속하여 다른 인터넷 호스트와 통신할 수 있는 인터넷 호스트로 만드는 기능을 제공한다. 다음을 수행할 수 있는 명령과 기능이 포함된다. 
    - 시스템 사이에서 파일 전송
    - 원격 시스템에 로그인
    - 원격 시스템에서 명령 실행
    - 원격 시스템에 파일 인쇄
    - 원격 사용자에 이메일 전송
    - 원격 사용자와 대롸적 통신
    - 네트워크 관리

### TCP/IP 용어

- 클라이언트 : 네트워크 프로세스나 다른 컴퓨터의 데이터, 서비스 또는 자원들을 액세스하는 컴퓨터 또는 프로세스이다. 
- 호스트 : 인터넷 네트워크에 접속되고 다른 인터넷 호스트와 통신할 수 있는 컴퓨터이다. 
- 네트워크 : 둘 이상의 호스트 및 이들 사이의 연결 링크 조합이다.  
- 패킷 : 호스트와 네트워크 사이의 한 트랜잭션에 대한 제어 정보 및 데이터 블록이다. 
- 포트 : 프로세스에 대한 논리적 연결 지점이다. 데이터는 포트(또는 소켓)을 통해 프로세스 사이에서 전송된다. 각 포트는 데이터 송수신을 위한 큐를 제공한다. 
- 프로세스 : 실행 중인 프로그햄이다. 프로세스는 컴퓨터에서 활동 중인 요소이다. 터미널, 파일, 기타 입출력 장치는 각기 다른 프로세스를 통해 통신한다. 따라서 네트워크 통신은 프로세스 간 통신이다. 
- 프로토콜 : 물리적 또는 논리적 레벨로 통신을 처리하는 규칙 세트이다. 다른 프로토콜을 사용하여 서비스를 제공하는 경우도 있다. 예를 들어, 연결 레벨 프로토콜은 전송 레벨 프로토콜을 사용하여 두 호스트 사이의 연결을 유지하는 패킷을 전송한다.
- 서버 : 네트워크 상의 다른 컴퓨터 또는 프로세스가 액세스할 수 있는 데이터, 서비스 또는 자원을 제공하는 컴퓨터 또는 프로세스이다.  

### TCP/IP 네트워크 계획

TCP/IP는 유연한 네트워킹 도구이기 때문에 특정 요구에 맞게 조정하여 사용할 수 있다. 네트워크를 계획할 경우에 이와 같은 주요 문제점을 생각해봐야 한다.  

1. 사용하려는 네트워크 하드웨어의 유형 결정한다.
2. 네트워크의 물리적 배치를 계획한다. 각 호스트 머신이 제공할 기능을 고려한다.
3. 사용자의 요구사항에 가장 적합한 네트워크가 플랫 네트워크인지 계층 네트워크인지 결정한다.
    - 사용자의 네트워크가 작은 규모이고, 하나의 물리적 네트워크로 구성되는 경우에는 플랫 네트워크가 적합하다. 그러나 네트워크가 매우 크고, 여러 물리적 네트워크로 복잡한 경우에는 계층 네트워크가 효과적일 수 있다. 
4. 네트워크를 다른 네트워크에 연결하려면 게이트 웨이의 설정 및 구성 방식을 계획해야 한다. 
    - 게이트웨이로 서비스되는 하나 이상의 머신 결정
    - 정적 또는 동적 라우팅 또는 이 둘의 조합을 사용해야하는지 여부를 결정
5. 주소 지정 체계를 결정한다. 
6. 시스템을 서브넷으로 분할해야 하는지 여부를 결정하고, 분할하는 경우에는 서브넷 마스크 지정 방법을 결정한다.
7. 이름 지정 체계를 결정한다. 네트워크의 각 머신에는 자체적으로 고유 호스트 이름이 있어야 한다. 
8. 네트워크에 이름 해석을 위한 이름 서버가 필요한지, 아니면 /etc/hosts 파일의 사용만으로 충분한지 여부를 결정한다.
9. 네트워크에서 원격 사용자에게 제공하려는 서비스 유형을 결정한다.

### TCP와 UDP의 차이점

|                                    TCP                                    |                                 UDP                                 |
| :-----------------------------------------------------------------------: | :-----------------------------------------------------------------: |
|                        ![](/TIL/docs/src/projects/network/network_10.png)                         |                     ![](/TIL/docs/src/projects/network/network_11.png)                      |
|                   연결형 서비스로 가상 회선 방식을 제공                   |              비연결형 서비스로 데이터그램 방식을 제공               |
| 3way handshaking 과정을 통해 연결을 설정하고 4way handshaking을 통해 해제 | 정보를 주고 받을 때 정보를 보내거나 받는다는 신호절차를 거치지 않음 |
|                        ![](/TIL/docs/src/projects/network/network_12.png)                         |                     ![](/TIL/docs/src/projects/network/network_13.png)                      |

### TCP/IP 프로토콜

프로토콜은 시스템과 애플리케이션 프로그램에서 정보를 교환할 수 있도록 하는 메시지 형식 및 일련의 작업 절차에 대한 규칙 세트이다.  
수신 호스트가 메시지를 이해하려면 통신에 관련된 각 시스템이 이러한 규칙을 준수해야 한다.  
TCP/IP 프로토콜은 계층이라는 관점에서 이해할 수 있다. 계층은 애플리케이션 계층, 전송 계층, 네트워크 계층, 네트워크 인터페이스 계층, 하드웨어로 구성된다.  

![TCP/IP 프로토콜 스위트](/TIL/docs/src/projects/network/network_02.jpeg)  

### TCP/IP 4 계층

#### 1계층 네트워크 엑세스 계층(Network Access Layer)

물리적인 MAC 주소가 이 계층에 해당된다. OSI계층처럼 데이터 링크 계층 또는 물리 계층이라고 불린다.
- 프로토콜 : Ethernet, Token Ring, PPP, 무선랜 등이 있다.

#### 2계층 인터넷 계층(Internet Layer)

호스트와 네트워크 간에 데이터 그램을 전달한다. 네트워크 통신에서 node 사이에 라우팅 기능과 패킷 전송을 위한 계층이다.  
- 프로토콜 : IP, ARP, RARP, ICMP, OSPF 등이 있다.

#### 3계층 전송 계층(Trasport Layer)

연결제어, 신뢰성 있는 데이터 전송을 보장한다. 데이터를 패킷으로 나누고 다른 장치까지 데이터를 전송하기 위한 계층이다.
- 프로토콜 : TCP, UDP

#### 4계층 응용 계층(Application Layer)

TCP/UDP 기반의 응용 프로그램을 구현할 때 사용한다. 사용자가 네트워크에 엑세스 할 수 있도록 하는 응용 프로그램 그룹이다. 
- 프로토톨 : DNS, HTTP, SSH, FTP, SMTP, DHCP 등이 있다.


### TCP/IP가 정보를 발신자에서 수신자로 이동하는 방법
- 애플리케이션 프로그램은 메시지나 데이터 스트림을 인터넷 전송 계층 프로토콜 중 하나로 전송한다. (UDP 또는 TCP로 전송)
- 데이터 스트림을 받은 프로토콜은 패킷이라고 부르는 작은 조각으로 데이터를 나누고, 대상 주소를 추가하여 다음 계층을 따라 패킷을 패스한다.  
- 인터넷 네트워크 계층은 패킷을 IP(Internet Protocol) 데이터그램에 포함한 후 데이터그램 헤더 및 트레일러에 넣고 데이터그램 전송 위치를 결정한 후 네트워크 인터페이스 계층으로 데이터그램을 패스한다. 
- 네트워크 계층은 IP 데이터그램을 승인하고 이더넷이나 토큰 링 네트워크와 같은 특정 네트워크 하드웨어를 통해 이들을 프레임으로 전송한다.  

![전송자 애플리케이션에서 수신자 호스트로의 정보 이동](/TIL/docs/src/projects/network/network_03.jpeg)

위 그림을 보면 정보가 전송자에서 호스트로의 TCP/IP 프로토콜 계층에 따라 아래로 흐른다.
호스트가 수신한 프레임은 역 방향으로 프로토콜 계층을 이동한다. 각 계층은 데이터가 애플리케이션 계층에 다시 도달할 때까지 해당 헤더 정보를 스트립한다. 

![호스트에서 애플리케이션으로 정보 이동](/TIL/docs/src/projects/network/network_04.jpeg)

위 그림을 보면 정보가 호스트에서 전송자로 TCP/IP 프로토콜 계층에 따라 위로 흐른다.  
- 프레임은 네트워크 인터페이스 계층에서 수신한다.  
- 네트워크 인터페이스 계층은 이더넷 헤더를 스트립하여 데이터그램을 네트워크 계층으로 전송한다.
- 네트워크 계층에서는 인터넷 프로토콜이 IP 헤더를 스트립하고 전송 계층으로 패킷을 전송한다. 
- 전송 계층에서는 TCP가 TCP 헤더를 스트립하여 애플리케이션 계층으로 데이터를 전송한다.  

네트워크의 호스트는 정보의 전송과 수신을 동시에 수행한다. 아래의 그림은 TCP/IP 계층에 따라 양방향으로 데이터가 흐르는 것을 보여준다.  

![호스트 데이터 전송 및 수신](/TIL/docs/src/projects/network/network_05.jpeg)


### TCP/IP 주소 지정

TCP/IP는 사용자와 애플리케이션이 통신할 고유 네트워크나 호스트를 식별할 수 있게 하는 인터넷 주소 지정 체계를 포함하고 있다.  
인터넷 주소는 우편번호 주소와 유사한 방식으로 작용하여 선택한 대상으로 데이터가 라우트되도록 한다.  
TCP/IP는 네트워크, 서브네트워크, 호스트, 소켓에 주소를 지정하고 브로드캐스트 및 로컬 루프백에 대한 특수 주소를 사용하기 위한 표준을 제공한다.  

인터넷 주소는 네트워크 주소와 호스트(또는 로컬) 주소로 이루어진다.  
고유한 공식 네트워크 주소는 다른 인터넷 네트워크에 연결할 때 각 네트워크에 지정된다.  
그러나 로컬 네트워크를 다른 인터넷 네트워크에 연결하지 않은 경우 로컬 사용에 간편한 네트워크 주소로 지정될 수 있다.  

### 인터넷 주소

IP(Internet Protocol)는 32비트의 두 부분으로 된 주소 필드를 사용한다.  
인터넷 주소는 네트워크 주소 부분과 호스트 주소 부분으로 이루어져 있다. 이를 사용하면 원격 호스트가 정보 전송 시에 원격 네트워크와 원격 네트워크의 호스트 모두를 지정할 수 있다.  

네트워크 주소 - 호스트들을 모아놓은 네트워크를 지칭하는 주소이며, 네트워크 주소가 동일한 네트워크는 곧 로컬 네트워크이다.  
호스트 주소 - 네트워크에 연결된 컴퓨터나, 이와 비슷한 장치의 주소이다. 

네트워크 주소와 호스트 주소를 구분하는 경계점은 고정되어 있지 않다. IP주소 체계에는 클래스라는 개념이 존재하는데, 이 클래스를 기준으로 네트워크와 호스트를 구분할 수 있다.  

TCP/IP는 인터넷 주소의 세 가지 클래스인 클래스 A, 클래스 B, 클래스 C를 지원한다.  
다른 클래스는 32비트 주소를 할당하는 방법으로 지정한다.  
네트워크가 지정되는 특정 주소 클래스는 네트워크의 크기에 따라 다르다.  
하나의 네트워크가 호스트의 수를 몇 개까지 가질 수 있는가에 따라서 클래스가 나뉘어진다.  

- 클래스 A
    - 8비트 네트워크 주소와 24비트 로컬 또는 호스트 주소로 구성된다. (255.0.0.0 서브넷 마스크)
    - 하나의 네트워크가 가질 수 있는 호스트 수가 가장 많은 클래스이다. 
    - 맨 앞쪽 bit가 항상 0으로 시작된다. 그래서 클래스 A는 1.0.0.0 ~ 126.0.0.0 안에서 정해진다. 
- 클래스 B
    - 16비트 네트워크 주소와 16비트 로컬 또는 호스트 주소로 구성된다. (255.255.0.0 서브넷 마스크)
    - 클래스 B는 128.0.0.0 ~ 191.255.255.255까지로 규정된다. 
- 클래스 C
    - 24비트 네트워크 주소와 8비트 로컬 또는 호스트 주소로 구성된다. (255.255.255.0 서브넷 마스크)
    - 클래스 C는 192.0.0.0 ~ 223.255.255.255까지로 규정된다.
- 0을 사용하는 인터넷 주소 : C 클래스 인터넷 주소가 호스트 주소 부분에 0을 포함하면 TCP/IP는 네트워크의 와일드 카드 주소를 전송한다.  

![](/TIL/docs/src/projects/network/network_26.jpeg)  

### 서브넷 주소

서브넷 주소 지정을 이용하면 여러 네트워크로 이루어진 자율 시스템이 동일한 인터넷 주소를 공유할 수 있다.  
TCP/IP의 서브 네트워크 기능은 단일 네트워크를 여러개의 논리적 네트워크(서브넷)로 분할할 수 있다.  
하나의 인터넷 네트워크 주소를 서브넷에 내부적으로 구성하면 더 적은 인터넷 네트워크 주소가 필요하면서 로컬 라우팅 기능은 개선된다.  

![](/TIL/docs/src/projects/network/network_16.png)  

표준 인터넷 프로토콜 주소 필드에는 네트워크 주소와 로컬 주소의 두 부분이 있다.  
서브넷이 사용되도록 하기 위해 인터넷 주소의 로컬 주소 부분이 서브넷 번호와 호스트 번호로 분할된다.  
서브넷은 로컬 자율 시스템이 메시지를 확실하게 라우트할 수 있도록 식별된다.  

![클래스 A 주소](/TIL/docs/src/projects/network/network_06.jpeg)

이 그림은 전형적인 클래스 A 주소 구조를 보여준다.  
처음 8비트는 네트워크 주소를 포함하고 나머지 24비트는 로컬 호스트 주소를 포함한다.  
클래스 A 인터넷 주소에 대한 서브넷 주소를 작성하기 위해 두가지로 나눌 수 있다.  
물리적 네트워크(또는 서브넷)를 식별하는 번호, 서브넷의 호스트를 식별하는 번호로 로컬 주소를 나눌 수 있다.  
전송인은 알려진 네트워크 주소로 메시지를 라우트하고, 로컬 시스템이 해당 서브넷과 호스트에 메시지를 라우트한다.  
로컬 주소를 서브넷 주소와 호스트 주소로 분할하는 방법을 결정할 때, 서브넷 수와 해당 서브넷의 호스트 수를 고려해야 한다.  

![해당 서브넷 주소가 있는 클래스 A 주소](/TIL/docs/src/projects/network/network_07.jpeg)

서브넷 주소와 호스트 주소를 유연하게 지정할 수 있고, 다음과 같은 제한사항이 있다. 
- network_address는 네트워크의 인터넷 주소이다.
- subnet_address는 주어진 네트워크에 대한 폭이 고정된 필드이다.
- host_address는 폭이 최소 1비트인 필드이다.  

subnet_address 필드의 폭이 0인 경우 네트워크는 서브넷으로 구성되지 않고 네트워크에 대한 주소 지정은 인터넷 네트워크 주소를 이용하여 수행된다.  

### 서브넷 마스크

시스템은 서브넷 마스크를 사용하여 대상 주소와 호스트 주소를 비교한다.  
호스트가 대상에게 메시지를 전송할 때 시스템은 대상이 소스와 동일한 네트워크에 있는지 또는 로컬 인터페이스 중 하나를 통해 대상에 직접 연결할 수 있는지 여부를 판별한다.  

대상이 로컬이 아닌 경우에 시스템은 메시지를 게이트웨이로 전송한다. 게이트웨이는 동일한 비교 작업을 수행하여 대상 주소가 로컬로 도달할 수 있는지 여부를 확인한다.  

서브넷 마스크는 시스템에 서브넷 분할 체계에 대해 알려준다. 이 비트 마스크는 네트워크 주소 부분과 인터넷 주소의 서브넷 주소 부분으로 구성된다.  

모든 IP 주소에는 서브넷 마스크가 있다. IP 주소를 서브네팅하지 않고 전부 쓰더라도 서브넷 마스크은 따라 다닌다. 주어진 네트워크를 나누어서 사용하는 경우에는 디폴트 서브넷 마스크를 고쳐서 사용한다.  

![해당 서브넷 주소가 있는 클래스 A 주소](/TIL/docs/src/projects/network/network_08.jpeg)

서브넷 마스크는 인터넷 주소처럼 4바이트 세트이다. 서브넷 마스크는 네트워크 및 서브네트워크 주소의 비트 위치에 해당하는 상위 비트와 호스트 주소의 비트 위치에 해당하는 하위 비트로 구성된다. 서브넷 마스크는 다음 그림과 유사하다.  

![서브넷 마스크 예제](/TIL/docs/src/projects/network/network_09.jpeg)  

### 서브넷 마스크 예시

1. 공인 IP 주소로 210.100.1.0 (서브넷 마스크 255.255.255.0) 네트워크를 받았다. 이 공인 주소를 이용해서 30대의 PC의 네트워크를 만든 다음 이들 네트워크를 라우터를 이용해서 통신하려고 한다.  
    - 30대의 PC를 수용하는데 필요한 이진수 자릿수를 구하면 5자리이다. 
    - 그러므로 210.100.1.ssshhhhh (s - 서브넷, h - 호스트) 가 되어야 한다. 
    - 이를 십진수로 바꾸면 255.255.255.224이다.

2. 주어진 네트워크 : 201.222.5.0(255.255.255.0), 서브넷 요구 조건 : 서브넷 당 호스트 5개 이상, 총 서브넷 수 20개 이상
    - 20개의 서브넷 필요 = 최소 25 이상, 5개의 호스트 필요 = 최소 23 이상
    - ssssshhh 이므로 255.255.255.11111000이다.
    - 서브넷 마스크는 255.255.255.248 이다.

    - `00000`000 : 앞에 있는 5bit 자리로 32개의 서브넷이 만들어진다. 
    - 00000`000` : 뒤에 있는 3bit 자리로 6개의 호스트를 가지게 된다. 

### 주소 비교

대상 주소와 로컬 네트워크 주소는 소스 호스트의 서브넷 마스크에서 AND, OR 연산자를 수행하여 비교한다.  
비교는 다음과 같이 일어난다.

- 대상 주소 및 로컬 서브넷 주소 마스크의 논리적 AND를 수행한다.
- 이전의 결과와 로컬 인터페이스의 로컬 넷 주소에서 배타적 OR를 수행한다.  
    - 결과가 모두 0이면 로컬 인터페이스 중 하나를 통해 대상에 직접 도달할 수 있다고 가정한다.
- 자율 시스템에 둘 이상의 인터페이스가 있는 경우 각 로컬 인터페이스에 대해 비교 프로세스가 반복된다.  

```
CLASS A   73.1.5.2    =  01001001 00000001 00000101 00000010


CLASS B   145.21.6.3  =  10010001 00010101 00000110 00000011
```

로컬 네트워크 인터페이스의 해당 서브넷 마스크는 다음 예제에 표시된다. 
```
CLASS A   73.1.5.2    =  11111111 11111111 11100000 00000000


CLASS B   145.21.6.3  =  11111111 11111111 11111111 11000000
```

IP주소 뒤에는 서브넷 마스크의 bit수를 표시된다. 예를 들어 클래스 A는 서브넷 마스크의 초기 bit가 8개이므로 `/8` 가 붙어서 `192.168.3.19/8` 처럼 IP주소를 표시한다.  


### 특수 IP 주소

- `0.0.0.0/32` : 현재 네트워크이다. 근원지 주소인 자신의 주소를 모를 때 사용한다.
- `127.0.0.0/8` : Loopback 주소이다. 같은 장치 상에서 통신할 때 사용되며 주로 소프트웨어의 테스트 목적으로 사용된다. 
- `169.254.0.0/16` : Link-Local, Zero Configuration Networking을 위해 예약된 sunet이다. IP를 설정하지 못한 호스트들끼리 통신이 가능하도록 세팅된 주소범위이다.
- `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16` : 사설 IP 범위이다. 로컬 영역에서만 사용할 수 있는 LAN을 구성하고 인터넷 망에 연결하지 않는 사설말에 부여하는 IP주소이다. 주로 IP주소의 절약과 보안을 목적으로 사용된다. 
- `224.0.0.0/4(~239.255.255.255)` : Multicast address이다. 
- `255.255.255.255/32` : Broadcast address이다.

  
## 소규모 네트워크 구성

IP주소를 할당하여 네트워크를 구성한다. 

![](/TIL/docs/src/projects/network/network_15.png)  

위의 그림을 보면 인터넷, 라우터, 스위치, 클라이언트가 네트워크를 구성하고 있다. 서브넷팅으로 각각의 호스트가 통신할 수 있도록 IP주소를 할당해야 한다. 먼저, 서브넷이 어떻게 구성되어야 하는지 구분한다.  

![](/TIL/docs/src/projects/network/network_17.png)  

클라이언트와 연결된 라우터는 동일한 서브넷 주소를 가진다. 그리고 스위치로 연결된 클라이언트들이 하나의 서브넷에 포함된다. 스위치는 OSI 2계층 네트워크 장치로써 네트워크 상의 주변 장치들 간의 데이터를 전송하는 역할을 한다. 그렇기 때문에 스위치 주변 클라이언트들은 동일한 서브넷에 포함된다.  

구분된 서브넷과 인터넷은 라우터로 연결된다. 라우터는 OSI 3계층 네트워크 장치로써 패킷이 들어오면 목적지 IP 주소를 확인하고, 해당 주소의 경로를 지정해준다. 라우터는 패킷을 최적의 경로로 목적지까지 도달시키기 위해 라우팅 테이블에 경로 정보를 수집한다.  

![](/TIL/docs/src/projects/network/network_18.png)  

라우터는 네트워크를 한 단계씩 뛰어넘으며 전달되는 방식으로 패킷을 전달하기 때문에 목적지와 다음 라우터의 IP 주소를 가지고 있다. 위의 그림을 보면, 라우팅 테이블의 오른쪽에 다음 라우터 IP 주소인 `163.172.250.12`가 입력되어 있다. 그렇기 때문에 왼쪽에는 목적지 네트워크의 IP 주소를 입력하면 된다. 즉, 서브넷의 IP 주소를 입력하면 된다.  

![](/TIL/docs/src/projects/network/network_19.png)  

![](/TIL/docs/src/projects/network/network_20.png)  

인터넷 라우팅 테이블의 `68.58.0.0/16`은 하나의 라우터에 연결된 두 서브넷을 한 번에 처리하기 위한 네트워크 주소이다. 두 서브넷은 서로의 네트워크를 구분하기 위해 서브넷 마스크를 사용하게 된다.  

![](/TIL/docs/src/projects/network/network_21.png)  

이렇게 정해진 IP 주소 범위 안에서 장치의 IP 주소를 정해주면 네트워크가 연결된다. 

## 정리

### 왜 TCP/IP 주소를 사용하는가?

TCP/IP는 사용자와 애플리케이션이 통신할 고유 네트워크나 호스트를 식별할 수 있게하는 인터넷 주소 지정 체계를 포함하고 있다. 우편번호 주소와 유사한 방식으로 작용하여 선택한 대상으로 데이터가 라우트되도록 한다.  

### TCP/IP는 무엇인가?

시스템과 애플리케이션 프로그램에서 정보를 교환할 수 있도록 하는 메시지 형식 및 일련의 작업 절차에 대한 규칙 세트이다. 수신 호스트가 메시지를 이해하려면 통신에 관련된 각 시스템이 이러한 규칙을 준수해야 한다.  

### 인터넷 주소

IP(Internet Protocal)는 32비트를 네트워크 주소와 호스트 주소로 나누어 사용한다.  
- 네트워크 주소 : 호스트들을 모아놓은 네트워크를 지칭하는 주소이며 네트워크 주소가 동일한 네트워크는 곧 로컬 네트워크이다. 
- 호스트 주소 : 네트워크에 연결된 컴퓨터이거나 비슷한 장치의 주소이다.

하나의 네트워크에 있는 도메인은 네트워크 IP가 같아야하지만 호스트 IP는 달라야 한다.

### 네트워크 주소와 호스트 주소를 어떻게 구별하는가?

32비트 주소를 세가지 클래스로 나누어 주소를 할당한다.  
- 클래스 A : 1.0.0.0 ~ 127.255.255.255
- 클래스 B : 128.0.0.0 ~ 191.255.255.255
- 클래스 C : 192.0.0.0 ~ 223.255.255.255

네트워크 자원을 효율적으로 분배하기 위해서 서브넷 마스크를 사용할 수 있다. 서브넷 마스크는 네트워크 IP주소를 나타내는 비트의 갯수를 표시한다.  

### 라우팅 테이블

라우터는 네트워크를 한 단계씩 뛰어넘으며 전달되는 방식으로 패킷을 전달하기 때문에 다음 라우터의 IP 주소를 가진다. 








[IP 주소 체계의 과거와 현재](https://haeunyah.tistory.com/89?category=1002971)  
[TCP와 UDP의 특징과 차이](https://mangkyu.tistory.com/15)  
[OSI 7 Layer](https://www.youtube.com/watch?v=1pfTxp25MA8)  
[계층별 장비와 케이블](https://www.youtube.com/watch?v=xpAlLYIBIC4)  
[라우터의 동작 방식](https://haeunyah.tistory.com/98?category=1002971)  
[OSI 7Layer 개념 및 역할, 구조까지 한번에 알아보기](https://onecoin-life.com/19)  
