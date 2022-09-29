---
title: Mutex
author: park chanul
date: 2022-02-04
category: c
layout: post
---

# 뮤텍스 (Mutex)

mutex는 동기화를 달성하는 방법 중 하나이다. 공유 자원에 접근할 때에 lock, 끝났을 때에 unlock하므로 어떤 스레드는 먼저 들어간 스레드가 임계 구역에서 나오기 전까지 대기해야한다. 앞서 들어간 스레드가 unlock하며 임계 구역에서 나오면 대기하고 있던 스레드가 lock하며 임계 구역에 들어간다. 이처럼 접근 제어가 필요한 공간을 공간에 진입할 수 있는 시간을 제어하는 방식으로 동기화를 달성한다.

```c
// mutex 키 생성
pthread_mutex_t mutex;

void *thread_one(void)	// 스레드 1 함수
{
	// 키 잠금
	// 공유 자원에 접근
	// 키 잠금 해제
}

void *thread_two(void)	// 스레드 2 함수
{
	// 키 잠금 (thread_one에서 이미 잠갔으므로 해체될 때까지 대기)
	// 공유 자원에 접근
	// 키 잠금 해제
}

void main(void)
{
	// thread_one 함수를 실행하는 스레드를 생성
	// thread_two 함수를 실행하는 스레드를 생성
}
```
