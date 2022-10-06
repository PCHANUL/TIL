---
layout: default
title: iterator_traits
nav_order: 2
grand_parent: c++
parent: iterator
permalink: /docs/c++/iterator/iterator_traits
---

- [traits class](#traits-class)
- [iterator_traits](#iterator_traits)
- [Definition](#definition)
- [Member types](#member-types)
- [Example](#example)
- [Reference](#reference)

# traits class

클래스를 상속 
다양한 템플릿 클래스에 대해 처리하는 함수를 만들 때 컴파일 타임에 템플릿이 무엇인지 알기위해 traits 클래스를 사용한다.

템플릿은 컴파일 타임에 인스턴스화 되기 때문에 템플릿이 무엇인지 알기 위해서 traits 클래스를 사용한다.




# iterator_traits

반복자의 속성을 정의하는 특성 클래스이다.  
표준 알고리즘은 해당 iterator_traits 인스턴스화의 구성원을 사용하여 전달된 반복자의 특정 속성과 해당 반복자가 나타내는 범위를 결정한다.  

# Definition

```cpp
template <class Iterator> class iterator_traits;
template <class T> class iterator_traits<T*>;
template <class T> class iterator_traits<const T*>;
```

# Member types

모든 반복자 유형에 대해 iterator_traits 클래스 템플릿의 해당 전문화가 정의되어야 하며 최소한 다음 멤버 유형이 정의되어야 한다.  

| member            | description                                         |
| ----------------- | --------------------------------------------------- |
| difference_type   | 한 반복자에서 다른 반복자를 뺀 결과를 표현하는 방식 |
| value_type        | 반복자가 가리킬 수 있는 요소의 유형                 |
| pointer           | 반복자가 가리킬 수 있는 요소에 대한 포인터의 유형   |
| reference         | 반복자가 가리킬 수 있는 요소에 대한 참조 유형       |
| iterator_category | 반복자의 범주이며, 다음 중 하나일 수 있다. input_iterator_tag, ouput_iterator_tag, forward_iterator_tag, bidirectional_iterator_tag, random_access_iterator_tag |

최소한 순방향 반복자가 아닌 출력 반복자의 경우 이러한 멤버 유형(iterator_category 제외)은 void로 정의될 수 있다.  
iterator_traits 클래스 템플릿은 반복자 유형 자체에서 이러한 유형을 가져오는 기본 정의와 함께 제공된다(아래 참조). 또한 포인터(T*) 및 const(const T*)에 대한 포인터에 특화되어 있다.  
모든 사용자 정의 클래스는 기본 클래스 std::iterator를 공개적으로 상속하는 경우 iterator_traits의 유효한 인스턴스화를 갖는다.  

# Example

```cpp
// iterator_traits example
#include <iostream>     // std::cout
#include <iterator>     // std::iterator_traits
#include <typeinfo>     // typeid

int main() {
  typedef std::iterator_traits<int*> traits;
  if (typeid(traits::iterator_category)==typeid(std::random_access_iterator_tag))
    std::cout << "int* is a random-access iterator";
  return 0;
}
```


# Reference

- [cplusplus iterator_traits](https://cplusplus.com/reference/iterator/iterator_traits/)
- [특성정보(traits) 클래스](http://egloos.zum.com/sweeper/v/3007176)