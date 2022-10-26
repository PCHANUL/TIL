---
layout: default
title: map
nav_order: 2
parent: c++ 
permalink: /docs/c++/map
---

- [map](#map)
  - [Template parameters](#template-parameters)
  - [Member types](#member-types)
  - [Member classes](#member-classes)
  - [Member functions](#member-functions)
    - [Iterators](#iterators)
    - [Capacity](#capacity)
    - [Element access](#element-access)
    - [Modifiers](#modifiers)
    - [Observers](#observers)
    - [Operations](#operations)
    - [Allocator](#allocator)
    - [Non-member functions](#non-member-functions)

# map

`map`은 `key value`와 `mapped value`가 쌍으로 정렬되어 저장되는 연관 컨테이너이다. 
`map`에서 `key value`는 요소를 정렬하고 고유하게 식별하는 데 사용되며, `mapped value`는 `key`와 연결된 컨텐츠를 저장한다.  
`key value`와 `mapped value`는 서로 타입이 다를 수 있으며, 두 가지를 결합한 `pair` 타입인 `value_type` 멤버 타입으로 그룹화된다.  
내부적으로 `map`은 `key`를 `Compare` 객체로 비교하여 정렬한다.  
`mapped value`는 대괄호 연산자(operator[])에 사용하여 해당 `key`로 직접 접근할 수 있다.  
일반적으로 이진 탐색 트리로 구현되어 있다.  

## Template parameters

- `Key` : 맵의 각 요소가 고유하게 식별되는 키의 타입이다.
- `T` : 맵의 각 요소가 매핑된 값으로 저장하는 값의 타입이다.
- `Compare` : 두 개의 키를 비교하여 bool을 반환하는 이진 술어이다. `comp(a, b)` 표현식에서 `comp`는 이 타입의 객체이고 `a`와 `b`는 키 값이다. `a`가 `b` 앞에 오는 경우에 `true`가 반환된다. 맵 객체는 이 표현식을 사용하여 요소가 컨테이너에서 따르는 순서와 두 요소 키가 동일한지 여부를 결정한다. 맵 컨테이너는 동일한 키를 가질 수 없다. 기본값은 `a < b`를 확인하는 `less<T>`이며, 함수 포인터 또는 함수 객체일 수 있다. 
- `Alloc` : 저장 공간 할당 모델을 정의하는데 사용되는 할당자 개체의 유형이다. 기본적으로 가장 단순한 메모리 할당 모델을 정의하고 값에 독립적인 `alloc` 클래스 템플릿이 사용된다. 


## Member types

| member type            | definition                                                                             | notes                                        |
| ---------------------- | -------------------------------------------------------------------------------------- | -------------------------------------------- |
| key_type               | The first template parameter (Key)                                                     |                                              |
| mapped_type            | The second template parameter (T)                                                      |                                              |
| value_type             | pair<const key_type,mapped_type>                                                       |                                              |
| key_compare            | The third template parameter (Compare)                                                 | defaults to: less<key_type>                  |
| value_compare          | Nested function class to compare elements                                              | see value_comp                               |
| allocator_type         | The fourth template parameter (Alloc)                                                  | defaults to: allocator<value_type>           |
| reference              | allocator_type::reference                                                              | for the default allocator: value_type&       |
| const_reference        | allocator_type::const_reference                                                        | for the default allocator: const value_type& |
| pointer                | allocator_type::pointer                                                                | for the default allocator: value_type*       |
| const_pointer          | allocator_type::const_pointer                                                          | for the default allocator: const value_type* |
| iterator               | a bidirectional iterator to value_type                                                 | convertible to const_iterator                |
| const_iterator         | a bidirectional iterator to const value_type                                           |                                              |
| reverse_iterator       | reverse_iterator<iterator>                                                             |                                              |
| const_reverse_iterator | reverse_iterator<const_iterator>                                                       |                                              |
| difference_type        | a signed integral type, identical to: iterator_traits<iterator>::difference_type       | usually the same as ptrdiff_t                |
| size_type              | an unsigned integral type that can represent any non-negative value of difference_type | usually the same as size_t                   |

## Member classes
- value_compare : value_type의 객체를 비교

## Member functions

- (constructor) : 맵 구성
- (destructor) : 맵 소멸자
- operator= : 컨테이너 컨텐츠 복사

### Iterators
- begin : 처음의 반복자를 반환
- end : 끝의 반복자를 반환
- rbegin : 처음의 역방향 반복자를 반환
- rend : 끝의 역방향 반복자를 반환

### Capacity
- empty : 컨테이너가 비어있는지 확인
- size : 컨테이너의 크기 반환
- max_size : 최대 크기 반환

### Element access
- operator[] : 요소 접근
- at : 요소 접근

### Modifiers
- insert : 요소 삽입
- erase : 요소 지우기
- swap : 컨텐츠 교환
- clear : 컨텐츠 지우기

### Observers
- key_comp : 키 비교 객체 반환
- value_comp : 값 비교 객체 반환

### Operations
- find : 요소의 반복자 반환
- count : 특정 키가 있는 요소 계산
- lower_bound : 반복자를 하한으로 반환
- upper_bound : 반복자를 상한으로 반환
- equal_range : 동일한 요소의 범위 가져오기

### Allocator
- get_allocator : 할당자 가져오기

### Non-member functions
- relational operators : 맵에 대한 관계 연산자
- swap : 맵의 내용 교환
