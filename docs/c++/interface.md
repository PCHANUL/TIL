---
layout: default
title: interface
nav_order: 2
parent: c++ 
permalink: /docs/c++/interface
---

* [interface](#interface)
	* [순수 가상 함수와 추상 클래스](#순수-가상-함수와-추상-클래스)
		* [인터페이스](#인터페이스)
	* [interface를 사용하는 이유](#interface를-사용하는-이유)

# interface

`interface`는 특정 기능 구현을 약속하는 추상 형식이다.  
반드시 구현해야하는 기능 목록을 만들어서 이를 사용하는 클래스들이 기능을 구현하도록 하며, 구현하지 않는 경우 에어를 발생시킨다.  
다른 클래스를 작성할 때 기본이 되는 틀이 된다.  

## 순수 가상 함수와 추상 클래스

순수 가상 함수를 하나라도 가지고 있는 클래스를 추상 클래스라고 한다.  
추상 클래스는 다음과 같은 특징을 가지고 있다.
1. 순수 가상 함수를 선언한 클래스는 인스턴스 생성을 할 수 없다.  
2. 추상 클래스를 상속한 자식 클래스는 순수 가상 함수를 구현해야 인스턴스를 생성할 수 있다.

```cpp
class AbstractClass
{
	virtual void functionA() = 0;	// 순수 가상 함수
	virtual void functionB();		// 가상 함수
	void functionC();
}
```

### 인터페이스
인터페이스는 구현이 없이 가상 소멸자와 순수 가상 함수만 포함된다.  
모든 인터페이스에는 소멸자가 있어야 한다.  

## interface를 사용하는 이유

객체 지향의 개념에 맞게 잘 정돈된 설계를 위해서 사용한다.  
인터페이스를 사용하지 않아도 가능하지만 나중에 재사용의 문제나 관리적 측면에서 편리하다.  
제품의 규격을 정해서 호환성을 높이는 일과 같다.  

```cpp
class InterfaceClass
{
	public :
		virtual ~InterfaceClass();
		virtual void functionA() = 0;
		virtual void functionB() = 0;
		virtual void functionC() = 0;
}
```

[Abstract class vs interface](https://cplusplus.com/forum/beginner/157568/)
[c++ 추상 클래스에 대한 고찰](https://thrillfighter.tistory.com/172) 
