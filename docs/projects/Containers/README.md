---
layout: default
title: Containers
nav_order: 3
parent: Projects
has_children: true
permalink: /docs/projects/Containers
---

- [Subject](#subject)
  - [Requirements](#requirements)
  - [Implements](#implements)
- [SFINAE](#sfinae)
- [Template Meta Programming(TMP)](#template-meta-programmingtmp)
- [Standard Template Library(STL)](#standard-template-librarystl)
- [Container](#container)
  - [vector](#vector)
  - [map](#map)
- [iterator](#iterator)
  - [iterator\_traits](#iterator_traits)
  - [reverse\_iterator](#reverse_iterator)
  - [map iterator](#map-iterator)
- [Problems](#problems)
  - [1. 템플릿 함수에서 템플릿 인자가 `iterator`인지 확인](#1-템플릿-함수에서-템플릿-인자가-iterator인지-확인)
    - [is\_class : class 타입인지 확인하는 메타 함수](#is_class--class-타입인지-확인하는-메타-함수)
  - [2. vector의 저장 공간 관리](#2-vector의-저장-공간-관리)
    - [\_TmpVector : vector의 메모리 재할당을 위한 클래스](#_tmpvector--vector의-메모리-재할당을-위한-클래스)
  - [3. const iterator](#3-const-iterator)


# Subject

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

- [allocator](/docs/c++/allocator.md)
  - allocator는 동적 메모리 할당을 관리하는데 필요한 기능이 정의된 객체이다.
  - STL은 사용자 지정 allocator가 제공되지 않으면 `std::allocator`를 사용한다.
- [iterator](/docs/c++/iterator/README.md)
  - STL 컨테이너에 저장된 요소를 순회하며, 각각의 요소에 대한 접근을 제공한다.  
  - 컨테이너의 반복자는 iterator_category로 구별되며 5가지의 반복자 타입이 있다.  
- [vector](/docs/c++/vector.md)
  - 순차적으로 엑세스할 수 있는 시퀀스 컨테이너이다.
- [map](/docs/c++/map.md)
  - 빠르게 검색할 수 있도록 정렬된 연관 컨테이너이다.
- stack
  - 시퀀스 컨테이너에 대해 다른 인터페이스를 제공하는 컨테이너 어댑터이다.
- iterator_traits
- reverse_iterator
- [enable_if](/docs/c++/type_traits/enable_if.md)
  - 템플릿을 위한 조건문이다.
- is_integral
- equal / lexicographical_compare
- pair, make_pair

# [SFINAE](/docs/c++/type_traits/SFINAE.md)

> "Substitution Failure Is Not An Error"

이 규칙은 함수 템플릿의 오버로드 결정 중에 적용된다. 컴파일러는 템플릿 인자 치환에 실패한 경우, 오버로딩 후보에서 제외시키고 오류는 발생시키지 않는다. 이 기능은 템플릿 메타 프로그래밍에 사용된다.  

# Template Meta Programming(TMP)

템플릿 메타 프로그래밍은 컴파일 시점에 실행되는 코드를 작성하는 일을 말한다. 

# Standard Template Library(STL)

표준 템플릿 라이브러리는 템플릿 메타 프로그래밍을 활용하여 구현된 라이브러리이다. 대표적으로 다음과 같은 라이브러리를 표준 템플릿 라이브러리라고 부른다.  

- 임의 타입의 객체를 보관할 수 있는 컨테이너
- 컨테이너에 보관된 원소에 접근할 수 있는 반복자
- 반복자로 일련의 작업을 수행하는 알고리즘

# Container

C++ STL에서 컨테이너는 타입에 상관없이 한 종류의 객체들을 보관할 수 있다. 이는 기능에 따라 다음과 같이 나뉜다.  

- 객체를 순차적으로 보관하는 시퀀스 컨테이너
- 키 값을 바탕으로 빠르게 검색하는 연관 컨테이너
- 신속하게 검색할 수 있으며 정렬되지 않은 연관 컨테이너
- 시퀀스 컨테이너에 대하여 다른 인터페이스를 제공하는 컨테이너 어댑터

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

## map

map은 key 값과 mapped 값이 쌍으로 저장되는 연관 컨테이너이다.  
두개의 값은 pair 타입으로 연결되며, compare 객체로 비교하여 정렬된다.  
각각의 pair는 key 값을 기준으로 정렬되어 저장된다.  
내부적으로 map은 red-black tree 구조로 요소들을 저장한다. [RBTree](/docs/projects/Containers/redblacktree.md)

tree 클래스의 Compare는 map의 value_compare이다.  
tree는 템플릿 인자로 value_type, Compare, Alloc을 받는다.  

# iterator

iterator는 컨테이너에 있는 요소를 가리키는 객체이다. 기능에 따라서 5가지 iterator가 있으며 동일한 맴버 함수를 사용할 수 있다. 맴버 함수는 모든 iterator 타입을 받지만 다르게 동작해야 하므로 컴파일 시점에 조건 처리를 한다. 이를 위해 속성 정보 클래스(type traits)라는 개념을 사용한다.  

## iterator_traits

iterator의 속성을 컴파일 시점에 알기 위해 iterator_traits를 사용한다.
iterator_traits라는 속성 정보 클래스로 컴파일 시점에 iterator의 속성을 확인할 수 있다.  
컴파일 시점에 iterator_traits로 확인하는 속성을 모든 iterator가 가지고 있어야 한다.  
iterator_traits로 분별된 iterator는 각각의 iterator로 오버로드된 맴버함수를 호출한다.  

이 프로젝트에서 구현하는 컨테이너는 vector와 map이다.  
vector의 iterator_category는 random_access_iterator_tag이고,  
map의 iterator_category는 bidirectional_iterator_tag이다.  

## reverse_iterator

역방향 반복자는 반복자의 방향을 바꾸는 반복자 어댑터이다. 양방향 반복자가 역방향 반복자가 되면 끝에서 시작으로 이동하는 새로운 반복자를 생성한다. 반복자 `i`에서 생성된 역방향 반복자 `r`의 경우에 `&*r == &*(i-1)` 관계가 항상 `true`이다. 

## map iterator

map 컨테이너의 반복자는 bidirectional iterator이다.  
map은 tree로 구현되어 있기 때문에 
map의 반복자는 템플릿 인자로 node를 받는다.  
그리고 반복자에서 node에 대한 연산자를 제공한다.  

# Problems

## 1. 템플릿 함수에서 템플릿 인자가 `iterator`인지 확인
- 템플릿 인자에 `iterator_category`가 정의되어 있는지 확인하면 된다. 
- `iterator`는 `iterator_category`로 구분된다.
- `T::iterator_category`로 확인하면 맴버가 없는 경우에 오류가 발생된다.  
- 맴버를 확인하면 오류가 발생되므로 타입별로 오버로딩하는 방법을 사용할 수 없다. 

### is_class : class 타입인지 확인하는 메타 함수

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


## 2. vector의 저장 공간 관리
- 새로운 요소를 추가하는 경우에 저장 공간을 확인하고, 저장 공간이 부족하다면 2배로 확장한다.
- 저장 공간 크기를 변경하는 함수는 다음과 같다.
- push_back, insert, resize, reserve, assign
- 저장 공간 크기를 변경할 때마다 사용되는 변수와 함수를 포함하는 클래스를 구현한다.

### _TmpVector : vector의 메모리 재할당을 위한 클래스
- _TmpVector : vector의 저장 공간 크기을 변경하기 위한 임시 객체이다. 
  - __begin, __end, __end_mem : 할당받은 메모리 주소를 저장한다.
  - _TmpVector : 생성해야하는 저장 공간의 크기와 새로운 요소를 받아서 메모리를 할당받는다.
  - ~_TmpVector : 메모리의 데이터를 소멸시키고, 메모리 공간을 해제한다.
  - insert_end(iter, iter) : 두 반복자를 인자로 받아서 끝에서 부터 요소를 추가한다.
  - insert_end(ele) : 하나의 요소를 받아서 끝에 추가한다.
  - move(vec) : vector와 메모리 주소를 교환한다.
  - swap(ptr, ptr) : 두 주소를 교환한다.

## 3. const iterator 





