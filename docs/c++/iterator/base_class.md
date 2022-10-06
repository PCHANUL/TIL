---
layout: default
title: Iterator base class
nav_order: 2
grand_parent: c++
parent: iterator
permalink: /docs/c++/iterator/base_class
---

- [Iteratpr base class](#iteratpr-base-class)
- [Definition](#definition)
- [Template parameters](#template-parameters)
- [Example](#example)
- [References](#references)

# Iteratpr base class

이 기본 클래스는 실제로 어떤 반복자 유형에도 존재할 필요가 없는 일부 구성원 유형만 제공한다. (반복자 유형에는 특정 구성원 요구사항이 없음)  
그러나 적절한 인스턴스화를 자동으로 생성하기 위해 기본 iterator_traits 클래스 템플릿에 필요한 멤버를 정의합니다. (그리고 그러한 인스턴스화는 모든 반복자 유형에 대해 유효해야 함)

# Definition

```cpp
template <class Category, class T, class Distance = ptrdiff_t,
          class Pointer = T*, class Reference = T&>
  struct iterator {
    typedef T         value_type;
    typedef Distance  difference_type;
    typedef Pointer   pointer;
    typedef Reference reference;
    typedef Category  iterator_category;
  };
```

# Template parameters

- Category : 반복자가 속한 카테고리이다. 다음 iterator_tag 중 하나이어야 한다.
  - input_iterator_tag : 입력 반복자
  - ouput_iterator_tag : 출력 반복자
  - forward_iterator_tag : 순방향 반복자
  - bidirectional_iterator_tag : 양방향 반복자
  - random_access_iterator_tag : 임의 접근 반복자
- T : 반복자가 가리키는 요소의 타입이다.
- Distance : 두 반복자 간의 차이를 나타내는 타입이다.
- Pointer : 반복자가 가리키는 요소에 대한 포인터를 나타내는 타입이다.
- Reference : 반복자가 가리키는 요소에 대한 참조를 나타내는 타입이다.

# Example

```cpp
// std::iterator example
#include <iostream>     // std::cout
#include <iterator>     // std::iterator, std::input_iterator_tag

class MyIterator : public std::iterator<std::input_iterator_tag, int>
{
  int* p;
public:
  MyIterator(int* x) :p(x) {}
  MyIterator(const MyIterator& mit) : p(mit.p) {}
  MyIterator& operator++() {++p;return *this;}
  MyIterator operator++(int) {MyIterator tmp(*this); operator++(); return tmp;}
  bool operator==(const MyIterator& rhs) const {return p==rhs.p;}
  bool operator!=(const MyIterator& rhs) const {return p!=rhs.p;}
  int& operator*() {return *p;}
};

int main () {
  int numbers[] = { 10,20,30,40,50 };
  MyIterator from(numbers);
  MyIterator until(numbers + 5);
  
  for (MyIterator it=from; it!=until; it++)
    std::cout << *it << ' ';
  std::cout << '\n';

  return 0;
}
```


# References

- [cplusplus iterator base class](https://cplusplus.com/reference/iterator/iterator/)  

