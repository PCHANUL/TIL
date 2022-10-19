---
layout: default
title: Containers
nav_order: 3
parent: Projects
has_children: true
permalink: /docs/projects/Containers
---

- [Containers](#containers)
  - [Requirements](#requirements)
  - [Implements](#implements)
  - [allocator](#allocator)
  - [iterator](#iterator)
  - [vector](#vector)
  - [Problems](#problems)
    - [1. 템플릿 함수에서 템플릿 인자가 `iterator`인지 확인](#1-템플릿-함수에서-템플릿-인자가-iterator인지-확인)
      - [is_class : class 타입인지 확인하는 메타 함수](#is_class--class-타입인지-확인하는-메타-함수)
    - [2. vector의 저장 공간 변경 방법](#2-vector의-저장-공간-변경-방법)
      - [_TmpVector](#_tmpvector)


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
  - 컨테이너의 반복자는 iterator_category로 구별되며 5가지의 반복자 타입이 있다.  
- [enable_if](../../../docs/c%2B%2B/type_traits/enable_if.md)

- [vector](../../../docs/c%2B%2B/vector)
  - 순차적으로 엑세스할 수 있는 시퀀스 컨테이너이다.
- map
  - 빠르게 검색할 수 있도록 정렬된 연관 컨테이너이다.
- stack
  - 시퀀스 컨테이너에 대해 다른 인터페이스를 제공하는 컨테이너 어댑터이다.

## allocator

사용자 정의 allocator가 들어와도 std::allocator를 사용하는 경우와 같은 동작을 할 수 있는가?


## iterator

iterator는 컨테이너에 있는 요소를 가리키는 객체이다.  
기능에 따라서 5가지 iterator가 있으며 동일한 맴버 함수를 사용할 수 있다.  
맴버 함수는 모든 iterator 타입을 받지만 다르게 동작해야 하므로 컴파일 시점에 조건 처리를 한다.  
이때 속성 정보 클래스라는 개념을 사용한다.  
iterator_traits라는 속성 정보 클래스로 컴파일 시점에 iterator의 속성을 확인할 수 있다.  
컴파일 시점에 iterator_traits로 확인하는 속성을 모든 iterator가 가지고 있어야 한다.  
iterator_traits로 분별된 iterator는 각각의 iterator로 오버로드된 맴버함수를 호출한다.  

이 프로젝트에서 구현하는 컨테이너는 vector와 map이다.  
vector의 iterator_category는 random_access_iterator_tag이고,  
map의 iterator_category는 bidirectional_iterator_tag이다.  

## vector 

vector는 추가적인 메모리가 필요한 경우에 기존 크기의 2배로 메모리를 재할당한다.  
- 생성자에서는 생성해야하는 크기만큼 메모리 공간을 확보한다.
- 복사 할당자는 복사받는 데이터의 크기만큼 공간이 없는 경우에만 재할당한다.
- push_back으로 요소를 추가하는 경우에 메모리 크기를 확인하고, 2배로 늘린다.
- pop_back으로 요소를 제거하는 경우에는 메모리를 재할당하지 않는다.

멤버 함수를 위한 함수
- __destroy_end : new_end를 받고, old_end까지의 요소를 제거한다.
- __construct_end : 새로운 공간을 할당받고, 요소들을 위치시킨다.


- [벡터의 용량과 크기](https://thebook.io/006842/ch02/03/02/)  


## Problems

### 1. 템플릿 함수에서 템플릿 인자가 `iterator`인지 확인
- 템플릿 인자에 `iterator_category`가 정의되어 있는지 확인하면 된다. 
- `iterator`는 `iterator_category`로 구분된다.
- `T::iterator_category`로 확인하면 맴버가 없는 경우에 오류가 발생된다.  
- 맴버를 확인하면 오류가 발생되므로 타입별로 오버로딩하는 방법을 사용할 수 없다. 

#### is_class : class 타입인지 확인하는 메타 함수

- 참조 링크 : [템플릿 프로그래밍](https://modoocode.com/295)  

`<type_traits>`에 있는 메타 함수 중에 `is_class`가 있다. 이 함수는 어떤 타입 `T`가 클래스인지 아닌지 확인한다.  

```cpp
namespace detail {
template <class T>
char test(int T::*);
struct two {
  char c[2];
};
template <class T>
two test(...);
}  // namespace detail

template <class T>
struct is_class
    : std::integral_constant<bool, sizeof(detail::test<T>(0)) == 1 &&
                                     !std::is_union<T>::value> {};
```

위의 예제 코드에서 `std::integral_constant`의 값은 템플릿 인자에 따라서 달라진다. 만약에 두번째 인자가 `true`가 되어서 `std::integral_constant<bool, true>`이면 `true`인 클래스가 된다.  

```cpp 
sizeof(detail::test<T>(0)) == 1 && !std::is_union<T>::value
```

즉, 위의 부분이 `true`이면 타입이 클래스라고 할 수 있다. `sizeof(detail::test<T>(0)) == 1` 부분이 중요하며 여기에서 클래스인지 아닌지 나뉜다. `detail` 네임스페이스에는 두개의 `test` 함수가 오버로드되어 인자에 따라서 호출되는 함수가 다르게 된다.  

첫번째 함수인 `char test(int T::*);`는 클래스가 아니라면 오버로드 후보에서 제외된다. `T::*`는 데이터 멤버를 가리키는 포인터이기 때문에 클래스에서만 사용할 수 있기 때문이다. 해당 클래스에 `int` 데이터 멤버가 없어도 유효한 문장이다.  

두번째 함수인 `two test(...);`는 타입에 상관없이 인스턴스화되며 `struct two { char c[2] };`에 의해서 `sizeof`가 2가 된다. 그러므로 클래스가 아니라면 `sizeof(detail::test<T>(0)) == 1`에서 `false`가 된다.  


### 2. vector의 저장 공간 변경 방법
- 

#### _TmpVector
- _TmpVector : vector의 저장 공간을 변경하기 위한 임시 객체이다. 
  - __begin, __end, __end_mem : 할당받은 메모리 주소를 저장한다.
  - ~_TmpVector : 메모리의 데이터를 소멸시키고, 메모리 공간을 해제한다.





