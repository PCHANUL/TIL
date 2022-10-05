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
- 비맴버 오버로드의 경우에 friend 키워드가 허용된다. 각 friend 키워드의 사용은 정당화되어야 한다.  
- map::value_compare를 구현해야하며 friend 키워드가 허용된다. 

## Implements

- [allocator](../../c%2B%2B/allocator.md)
  - allocator는 동적 메모리 할당을 관리하는데 필요한 기능이 정의된 객체이다.
  - STL은 사용자 지정 allocator가 제공되지 않으면 `std::allocator`를 사용한다.
- iterator
  - STL 컨테이너에 저장된 요소를 순회하며, 각각의 요소에 대한 접근을 제공한다.


- [vector](../../c%2B%2B/vector.md)
  - 순차적으로 엑세스할 수 있는 시퀀스 컨테이너이다.
- map
  - 빠르게 검색할 수 있도록 정렬된 연관 컨테이너이다.
- stack
  - 시퀀스 컨테이너에 대해 다른 인터페이스를 제공하는 컨테이너 어댑터이다.


