---
layout: default
title: vector
nav_order: 2
parent: c++ 
permalink: /docs/c++/vector
---

- [vector](#vector)
	- [정의](#정의)
	- [속성](#속성)
	- [맴버 타입](#맴버-타입)
	- [맴버 함수](#맴버-함수)

# vector

`std::vector`는 동적 크기 배열을 캡슐화하는 시퀀스 컨테이너이다. 요소가 연속적으로 저장된다. 그러므로 `iterator`를 통해서 요소에 접근할 수 있고, 요소에 대한 일반 포인터의 오프셋을 사용하여 요소에 접근할 수 있다. `vector`의 크기가 동적으로 변경될 수 있으며 저장공간은 컨테이너에서 자동으로 처리된다.  

내부적으로 `vector`는 동적으로 할당된 배열을 사용하여 요소를 저장한다. 새 요소가 삽입될 때 크기를 늘리기 위해 재할당해야 할 수도 있다. 이는 처리 시간 면에서 상대적으로 비용이 많이 드는 작업이기 때문에 요소가 컨테이너에 추가될 때마다 `vector`가 재할당되지 않는다.  

대신에 `vector` 컨테이너는 추가적인 저장공간을 할당할 수 있다. 따라서 컨테이너는 요소를 포함하는데 필요한 저장공간보다 더 큰 용량을 가질 수 있다. 재할당은 `vector` 끝에 요소를 삽입할 때 상각된 상수 시간이 제공될 수 있도록 크기가 대수적으로 증가하는 간격에서만 발생해야 한다.  

따라서 `array`와 비교하면 `vector`는 저장공간을 효율적인 방식으로 관리하는 기능의 대가로 더 많은 메모리를 소비한다.  

## 정의

```cpp
template<
	class T,
	class Alloc = std::allocator<T>
> class vector;
```

- `T` : 요소의 타입
  - 반드시 [CopyAssignable](https://en.cppreference.com/w/cpp/named_req/CopyAssignable), [CopyConstructible](https://en.cppreference.com/w/cpp/named_req/CopyConstructible) 조건을 충족해야 한다.
- `Alloc` : 저장공간 할당 모델을 정의하는데 사용되는 할당 모델
  - 기본으로 `allocator` 클래스 템플릿이 사용된다. 
  - 컨테이너는 저장공간이 필요한 경우에 `allocator`를 사용하여 배열을 재할당한다. 


## 속성

- Sequence : 시퀀스 컨테이너의 요소는 엄격한 선형 시퀀스로 정렬된다. 개별 요소는 이 순서에서 해당 위치로 액세스된다. 
- Dynamic array : 포인터 연산을 통해서 시퀀스의 모든 요소에 직접 접근할 수 있으며 시퀀스 끝에 요소를 빠르게 추가/제거할 수 있다. 
- Allocator-aware : 컨테이너는 할당자 개체를 사용하여 저장공간 요구 사항을 동적으로 처리한다. 

## 맴버 타입

| member type            | definition                                                                             | notes                                        |
| ---------------------- | -------------------------------------------------------------------------------------- | -------------------------------------------- |
| value_type             | The first template parameter (T)                                                       |                                              |
| allocator_type         | The second template parameter (Alloc)                                                  | defaults to: allocator<value_type>           |
| reference              | allocator_type::reference                                                              | for the default allocator: value_type&       |
| const_reference        | allocator_type::const_reference                                                        | for the default allocator: const value_type& |
| pointer                | allocator_type::pointer                                                                | for the default allocator: value_type*       |
| const_pointer          | allocator_type::const_pointer                                                          | for the default allocator: const value_type* |
| iterator               | a random access iterator to value_type                                                 | convertible to const_iterator                |
| const_iterator         | a random access iterator to const value_type                                           |                                              |
| reverse_iterator       | reverse_iterator<iterator>                                                             |                                              |
| const_reverse_iterator | reverse_iterator<const_iterator>                                                       |                                              |
| difference_type        | a signed integral type, identical to: iterator_traits<iterator>::difference_type       | usually the same as ptrdiff_t                |
| size_type              | an unsigned integral type that can represent any non-negative value of difference_type | usually the same as size_t                   |


## 맴버 함수

- [(constructor)](https://cplusplus.com/reference/vector/vector/vector/) : vector 생성자
- [(destructor)](https://cplusplus.com/reference/vector/vector/~vector/) : vector 소멸자
- 





