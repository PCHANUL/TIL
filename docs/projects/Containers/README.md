---
layout: default
title: Containers
nav_order: 3
parent: Projects
has_children: true
permalink: /docs/projects/Containers
---

* [Containers](#containers)
  * [Requirements](#requirements)
  * [Implements](#implements)
  * [iterator](#iterator)


# Containers

이 프로젝트에서는 C++ STL의 몇 가지 컨테이너 타입을 구현한다. C++98 표준을 준수해야 하여 C++98 이후의 기능은 구현하지 않지만, 더 이상 사용하지 않는 C++98 기능은 구현해야 한다.  

## Requirements

- 메모리를 할당할 때 메모리 누수를 방지해야 한다. 
- 외부 라이브러리는 사용할 수 없다. C++11 및 Boost 라이브러리가 금지된다. 
- 각 헤더를 다른 헤더와 독립적으로 사용할 수 있도록 include guard를 추가해야 한다. 
- 네임 스페이스는 반드시 ft이어야 한다. 
- 사용된 내부 데이터 구조는 논리적이며 정당화되어야 한다. 
- 표준 컨테이너에서 제공되는 것보다 더 많은 public 기능을 구현할 수 없다.
- 컨테이너에 iterator 시스템이 있다면 구현해야 한다.
- std::allocator를 사용해야 한다.
- 비멤버 오버로드의 경우에 friend 키워드가 허용된다. 각 friend 키워드의 사용은 정당화되어야 한다.  
- map::value_compare를 구현해야하며 friend 키워드가 허용된다. 

## Implements

- [allocator](../../../docs/c%2B%2B/allocator)
  - allocator는 동적 메모리 할당을 관리하는데 필요한 기능이 정의된 객체이다.
  - STL은 사용자 지정 allocator가 제공되지 않으면 `std::allocator`를 사용한다.
- [iterator](../../../docs/c%2B%2B/iterator/README)
  - STL 컨테이너에 저장된 요소를 순회하며, 각각의 요소에 대한 접근을 제공한다.


- [vector](../../../docs/c%2B%2B/vector)
  - 순차적으로 엑세스할 수 있는 시퀀스 컨테이너이다.
- map
  - 빠르게 검색할 수 있도록 정렬된 연관 컨테이너이다.
- stack
  - 시퀀스 컨테이너에 대해 다른 인터페이스를 제공하는 컨테이너 어댑터이다.

## iterator

iterator는 컨테이너에 있는 요소를 가리키는 객체이다.  
기능에 따라서 5가지 iterator가 있으며 동일한 맴버 함수를 사용할 수 있다.  
맴버 함수는 모든 iterator 타입을 받지만 다르게 동작해야 하므로 컴파일 시점에 조건 처리를 한다.  
이때 속성 정보 클래스라는 개념을 사용한다.  
iterator_traits라는 속성 정보 클래스로 컴파일 시점에 iterator의 속성을 확인할 수 있다.  
컴파일 시점에 iterator_traits로 확인하는 속성을 모든 iterator가 가지고 있어야 한다.  
iterator_traits로 분별된 iterator는 각각의 iterator로 오버로드된 맴버함수를 호출한다.  

이 프로젝트에서 구현하는 컨테이너는 vector와 map이다.  
vector는 random_access iterator를 사용하고, map은 bidirectional iterator를 사용한다.  






