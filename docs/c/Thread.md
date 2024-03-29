---
layout: default
title: Thread
parent: c
nav_order: 6
permalink: /docs/c/Thread
---

* [스레드 (thread)](#스레드-thread)
  * [스레드의 종류](#스레드의-종류)
  * [스레드의 동기화](#스레드의-동기화)
    * [임계 구역](#임계-구역)
    * [스레드 동기화 방법](#스레드-동기화-방법)
  * [스레드 종료 방법](#스레드-종료-방법)

# 스레드 (thread)

![스레드](/TIL/docs/src/projects/philosophers/philo_07.png)  

스레드는 프로세스의 실행 단위이다. 스레드는 프로세스에 할당된 메모리, 자원을 공유하며 여러 실행 흐름을 동시에 진행시킨다. 응용 프로그램을 실행하면 코드는 코드 데이터 영역, 전역 변수는 데이터 영역, 지역 변수는 스택, 동적 할당은 힙 영역으로 나뉘어 메모리에 저장된다. 만약에 여러 흐름으로 작업을 수행해야하는 상황에서 별도의 프로세스를 이용하면, 메모리와 이외의 것들을 개별 공간에 가지게 되므로 높은 비용이 들며 자원을 공유하기 어렵다. 그러므로 프로세스의 메모리를 공유하여 사용해야하는 경우에 스레드를 생성하여 작업을 수행한다. 

기본 데이터  
  - 고유한 스레드 ID, 프로그램 카운터, 레지스터 집합, 스택
  - 코드, 데이터, 파일 등 기타 자원은 프로세스 내의 다른 스레드와 공유한다.  

특정 데이터  
  - 하나의 스레드에만 연관된 데이터로 개별 스레드만의 자료 공간이 필요하다.
  - 특별한 경우에 사용하는 개별 스레드만의 자료 공간

## 스레드의 종류
- 사용자 레벨 스레드
  - 사용자 레벨의 라이브러리를 통해 구현된다.
  - 스레드가 중단되면 나머지 모든 스레드가 중단된다.
- 커널 레벨 스레드
  - 운영체제가 지원하는 스레드 기능으로 구현된다.
  - 스레드가 도중에 중단되어도 커널은 다른 스레드를 중단시키지 않고 계속 실행시켜준다.

## 스레드의 동기화

### 임계 구역
  - 병렬 컴퓨팅에서 둘 이상의 스레드가 동시에 접근해서는 안되는 공유 자원을 접근하는 코드의 일부를 말한다. 어떤 스레드가 임계 구역에 들어가고자 한다면 대기해야 할 수도 있다. 스레드가 공유자원의 사용을 보장받기 위해서 임계 구역에 들어가거나 나올때 동기화 매커니즘이 사용된다.

### 스레드 동기화 방법
  - mutex (locking 매커니즘)
    - 스레드의 동시 접근을 허용하지 않고, 뮤텍스를 가지고 있어야 임계영역에 들어갈 수 있다.
    - 임계역역에 들어간 스레드가 뮤텍스를 이용하여 본인이 나오기 전까지 다른 스레드가 들어오지 못하게 잠근다.
    - wait를 호출한 스레드만이 signal을 호출할 수 있다.
  - semaphore (signaling 매커니즘)
    - 뮤텍스와 비슷하지만 동시 접근 동기화가 아닌 접근 순서 동기화에 관련있다.
    - 세마포어 카운터를 사용하며, wait를 호출하면 카운터를 1만큼 줄인다.
    - 카운터가 0보다 작거나 같아질 경우에 락이 실행된다.
    - 만약에 카운터를 1로 설정하면 mutex처럼 사용할 수 있다.


## 스레드 종료 방법  

멀티 스레드 프로그래밍에서 여러 스레드를 해제시키는 방법으로 두가지 방법이 있다.  
  1. 메인에서 스레드가 종료되기를 기다리다가 해제한다.
  2. 각각의 스레드가 종료되며 해제된다.

두가지 방법은 스레드의 상태에 따라서 달라진다. 각각 `non-detached(joinable)`와 `detached`라고 한다.  
  1. `non-detached`로 스레드를 생성한 경우에는 `pthread_join`으로 스레드의 자원을 해제해야 한다.  
  2. `detached`로 스레드를 생성한 경우에는 스레드의 자원이 종료와 함께 해제된다. 

예시 코드로 차이를 확인할 수 있다. 아래의 코드를 실행해보면 `thread_func`의 메시지가 출력되지 않는다. `thread_func`가 `main`이 종료되자마자 정리된다. 그러나 `pthread_detach`를 `pthread_join`으로 변경한다면 메시지가 출력되는 것을 볼 수 있다. 

```c
#include <stdio.h>
#include <unistd.h>
#include <pthread.h>

void	*thread_func(void *param)
{
	(void)param;
	
	usleep(1000);
	printf("Inside thread function");
	return (0);
}

int main(void) 
{ 
	pthread_t	thread;
	
	pthread_create(&thread, NULL, thread_func, NULL);
	pthread_detach(thread);	// pthread_join(thread)로 변경
	return (0);
}
``` 
