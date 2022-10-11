---
layout: default
title: random access iterator
nav_order: 2
grand_parent: c++
parent: iterator
permalink: /docs/c++/iterator/random_access_iterator
---

* [Random access iterator](#random-access-iterator)
	* [Operations](#operations)


# Random access iterator

임의 접근 반복자는 포인터와 동일한 기능을 제공하며, 어떠한 요소의 위치에서 임의 오프셋 위치에 있는 요소에 접근할 수 있다. 임의 접근 반복자는 기능면에서 가장 완전한 반복자이다. 모든 포인터 유형은 유효한 임의 접근 반복자 이기도 하다.  

## Operations

모든 임의 접근 반복자는 최소한 다음 작업을 지원한다. `X`는 임의 접근 반복자 타입이고, `a`와 `b`는 반복자 타입의 객체이며, `n`은 difference 타입 값이며, `t`는 반복자 타입이 가리키는 타입의 객체이다.  

- 기본 생성자, 복사 생성자, 복사 할당자, 소멸자 : `X a;`, `X b(a);`, `b = a;`
- 동등/부등 연산자를 사용 : `a == b`, `a != b`
- rvalue로 역참조 : `*a`, `a->m`
- 변경 가능한 반복자의 경우에 lvalue로 역참조 : `*a = t`
- 증가 연산자 : `++a`, `a++`, `*a++`
- 감소 연산자 : `--a`, `a--`, `*a--`
- 반복자와 정수 값 사이의 산술 연산자 : `a + n`, `n + a`, `a - n`, `a - b`
- 부등식 관계 연산자 : `a < b`, `a > b`, `a <= b`, `a >= b`
- 복합 할당 작업 지원 : `a += n`, `a -= n`
- 오프셋 역참조 연산자 : `a[n]`

상수 반복자는 ouput 반복자의 요구 사항을 충족하지 않는 반복자이다. 이를 역참조하면 상수 요소에 대한 참조가 생성된다.  
