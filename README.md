---
layout: default
title: Today I Learned
nav_order: 1
has_children: true
child_nav_order: desc
permalink: /
---

# Today I Learned <!-- omit in toc -->

## 22.10.07 : c++ iterator_traits

> # type traits
> 
> C++에서 traits는 어떻게 사용되는지 확인하기 위해서 type traits를 알아본다. type traits는 C++ 템플릿 메타 프로그래밍에서 유용하게 쓰이는 기술 중에 하나이다. type traits를 이용하여 type의 다양한 속성들에 대해 조사하거나 프로퍼티를 변경할 수 있다. 예를 들어 제네릭 타입인 T가 있는 경우, 제네릭 인자로 넘어온 타입 T에 따라서 컴파일되는 코드가 달라지는 조건부 컴파일에 사용된다. 그리고 특정 타입에 const를 붙이거나 제거하거나 참조로 만들거나 포인트로 만들어 타입을 변형하는데 사용된다. 이 모든 것이 컴파일 타임에 완료되어 실행 시간의 패널티 없이 동작한다. 이를 바로 메타 프로그래밍이라고 한다.  
> 
> # iterator_traits
> 
> iterator_traits는 반복자의 속성을 정의하는 클래스이다. 템플릿은 컴파일 타임에 인스턴스화되기 때문에 컴파일 타임에 조건에 따른 처리를 위해 `traits` 클래스를 사용한다. 템플릿 클래스를 사용하다보면 흔히 발생할 수 있기 때문에 반복자 이외에도 라이브러리에서 쉽게 찾아 볼 수 있다.  
> 
