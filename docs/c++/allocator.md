---
layout: default
title: allocator
nav_order: 2
parent: c++ 
permalink: /docs/c++/allocator
---

* [std::allocator](#stdallocator)
	* [정의](#정의)
	* [멤버 함수](#멤버-함수)
	* [예시](#예시)

# std::allocator

`Allocator`는 동적 메모리 할당을 통합 관리하는데 필요한 기능이 정의된 객체이다. `std::allocator`는 사용자 지정 할당자가 제공되지 않은 경우에 표준 라이브러리 컨테이너에서 사용하는 기본 할당자이다.  

일반적으로 동적 메모리 관리는 `new`와 `delete`를 사용하지만 컨테이너를 직접 구현하고 사용하는 경우 `std::allocator<T>`를 사용하면 표준 라이브러리와 유사한 인터페이스를 구현하기 용이하다.  


## 정의

```cpp
template< class T >
struct allocator;
```

- `T` : 개체에 의해 할달된 요소의 유형

## 멤버 함수

- [(constructor)](https://cplusplus.com/reference/memory/allocator/allocator/) : 할당자 객체 생성
- [(destructor)](https://cplusplus.com/reference/memory/allocator/~allocator/) : 할당자 소멸자
- [address](https://cplusplus.com/reference/memory/allocator/address/) : 주소를 반환
- [allocate](https://cplusplus.com/reference/memory/allocator/allocate/) : 저장공간 블록 할당
- [deallocate](https://cplusplus.com/reference/memory/allocator/deallocate/) : 저장공간 블록 해제
- [max_size](https://cplusplus.com/reference/memory/allocator/max_size/) : 할당 가능한 최대 크기
- [construct](https://cplusplus.com/reference/memory/allocator/construct/) : 객체 생성
- [destroy](https://cplusplus.com/reference/memory/allocator/destroy/) : 객체 파괴


## 예시

```cpp
#include <memory>

int main()
{
	// default allocator for ints
	std::allocator<int> alloc;
	const size_t size = 2;

	// allocate
	int*	p = alloc.allocate(size);

	// deallocate
	alloc.deallocate(p, size);
}
```
