---
layout: default
title: iterator_traits
nav_order: 2
grand_parent: c++
parent: iterator
permalink: /docs/c++/iterator/iterator_traits
---

- [iterator\_traits](#iterator_traits)
  - [Example](#example)
- [Definition](#definition)
- [Member types](#member-types)
- [Reference](#reference)

# iterator_traits

iterator_traits는 반복자의 속성을 정의하는 클래스이다. 템플릿은 컴파일 타임에 인스턴스화되기 때문에 컴파일 타임에 조건에 따른 처리를 위해 `traits` 클래스를 사용한다. 템플릿 클래스를 사용하다보면 흔히 발생할 수 있기 때문에 반복자 이외에도 라이브러리에서 쉽게 찾아 볼 수 있다.  

## Example

STL에는 `advance`라는 함수 템플릿이 있다. `advance`는 반복자(iter)를 지정된 거리(dist)만큼 이동시킨다.  

```cpp
template<typename IterT, typename DistT>
void advance(IterT& iter, DistT dist)
```

그런데 5가지의 반복자를 살펴보면 `advance`가 단순히 지정된 거리만큼 이동시키기 어려워 보인다.
- Input iterator : 전진 가능
- Ouput iterator : 전진 가능
- Foward iterator : 전진 가능
- Bidirectional iterator : 전/후진 가능
- Random access iterator : 임의의 거리만큼 전/후진 가능

`Random access iterator`를 제외하고는 지정된 거리만큼 이동시킬 수 없고, dist에 음수가 들어와서 후진을 해야하는 경우라면 `Bidirectional iterator`와 `Random access iterator`을 제외하고는 다른 처리를 해주어야 한다. 즉, 함수 템플릿인 `advance`는 함수로 들어온 반복자의 종류를 컴파일 타임에 구분하여 처리해주어야 한다.  

`traits` 클래스는 컴파일 도중에 주어진 어떤 타입의 정보를 얻을 수 있게 하는 객체를 말한다. C++에 정의된 문법구조나 키워드가 아니라 프로그래머들이 관용적으로 사용하는 구현 기법이다. `traits` 클래스는 관례적으로 다음과 같은 항목을 충족해야 한다.  

1. 일반적으로 `traits`는 구조체로 구현되지만 traits class라고 부른다.
2. `traits`는 기본 제공 타입에 대해서 쓸 수 있어야 한다.

그렇다면 advance 함수에서 원하는 것은 인자로 들어온 iterator에 맞는 처리를 하는 것이다. 주어진 타입을 컴파일 시점에 확인하여 처리하는 조건처리 구문요소가 필요하다. 이를 함수 오버로딩으로 구현할 수 있다.  

```cpp
// iterator_category 객체를 반환
template<class _Iter> inline
typename iterator_traits<_Iter>::iterator_category _Iter_cat(const _Iter&)
{
  typename itereator_traits<_Iter>::iterator_category _Cat;
  return (_Cat);
}

// _Iter_cat에서 반환된 iterator_tag와 _Advance 함수를 호출한다.
// iterator_tag에 맞게 오버로드된 _Advance가 호출된다.
template<class _InIt, class _Diff>
inline void advance(_InIt& _Where, _Diff _Off)
{
  _Advance(_Where, _Off, _Iter_cat(_Where));
}

// 각 iterator_tag로 오버로드된 _Advance 함수들
template<class _InIt, class _Diff>
inline void _Advance(_InIt& _Where, _Diff _Off, input_iterator_tag)
{
    // ...
}
 
template<class _FI, class _Diff>
inline void _Advance(_FI& _Where, _Diff _Off, forward_iterator_tag)
{
    // ...
}
 
template<class _BI, class _Diff>
inline void _Advance(_BI& _Where, _Diff _Off, bidirectional_iterator_tag)
{
    // ...
}
 
template<class _RI, class _Diff>
inline void _Advance(_RI& _Where, _Diff _Off, random_access_iterator_tag)
{
    // ...
}
```

각 iterator 타입에 맞는 오버로드 함수를 준비하고, 메인 함수는 특성 정보 클래스를 통해 해당 iterator의 고유 iterator_tag을 생성하여 오버로드 함수를 호출하는 것이다.  

`iterator_traits`는 특성 정보 클래스를 구현하는 방법 중에 하나이다. 특성 정보 클래스는 컴파일 도중에 타입의 정보를 얻게하는 객체를 지칭하는 개념으로써 다양한 방식으로 활용된다.  


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


# Reference

- [type traits 기초](https://kukuta.tistory.com/399)  
- [cplusplus iterator_traits](https://cplusplus.com/reference/iterator/iterator_traits/)
- [특성정보(traits) 클래스](http://egloos.zum.com/sweeper/v/3007176)
