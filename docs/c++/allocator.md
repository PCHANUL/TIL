---
layout: default
title: allocator
nav_order: 2
parent: c++ 
permalink: /docs/c++/allocator
---

* [std::allocator](#stdallocator)
	* [Member functions](#member-functions)
	* [Example](#example)
* [References](#references)

# std::allocator

`Allocator`는 동적 메모리 할당을 통합 관리하는데 필요한 기능이 정의된 객체이다. `std::allocator`는 사용자 지정 할당자가 제공되지 않은 경우에 표준 라이브러리 컨테이너에서 사용하는 기본 할당자이다. 일반적으로 동적 메모리 관리는 `new`와 `delete`를 사용하지만 컨테이너를 직접 구현하고 사용하는 경우 `std::allocator<T>`를 사용하면 표준 라이브러리와 유사한 인터페이스를 구현하기 용이하다.  

동적 할당하는 경우에 `allocator`는 초기화되지 않은 공간으로 메모리를 할당할 수 있다. 할당과 동시에 초기화가 이루어지는 `new`와 다르다. `new` 연산자는 메모리를 할당하고 값을 저장하여 불필요한 초기화 과정이 반복된다. 예를 들어서 `int a = 3`으로 값을 저장할 수 있지만, `new` 연산자는 `int a = 0`, `a = 3` 두과정을 거쳐서 값을 저장한다.  

그리고 할당받은 공간의 객체를 소멸시킬 수 있다. 메모리 공간을 해제시키는 `delete` 대신에 `std::allocator<T>::destroy` 함수로 메모리 공간을 초기 상태로 만든다. 덕분에 메모리의 재할당 대신에 다시 사용하며 메모리를 세밀하게 나누어 컨트롤할 수 있다.  


```cpp
template< class T >
struct allocator;
```

- `T` : 개체에 의해 할달된 요소의 유형

## Member functions

- [(constructor)](https://cplusplus.com/reference/memory/allocator/allocator/) : 할당자 객체 생성
- [(destructor)](https://cplusplus.com/reference/memory/allocator/~allocator/) : 할당자 소멸자
- [address](https://cplusplus.com/reference/memory/allocator/address/) : 주소를 반환
- [max_size](https://cplusplus.com/reference/memory/allocator/max_size/) : 할당 가능한 최대 크기
- [allocate](https://cplusplus.com/reference/memory/allocator/allocate/) : 저장공간 블록 할당
  - 초기화되지 않은 메모리 공간을 할당하여 시작 주소를 반환한다.
- [deallocate](https://cplusplus.com/reference/memory/allocator/deallocate/) : 저장공간 블록 해제
  - 할당받은 메모리 공간을 해제한다.
- [construct](https://cplusplus.com/reference/memory/allocator/construct/) : 객체 생성
  - 포인터가 가리키는 위치에 초기화되지 않은 공간에 요소를 저장한다.
- [destroy](https://cplusplus.com/reference/memory/allocator/destroy/) : 객체 파괴
  - 포인터가 가리키는 위치에 있는 객체의 소멸자를 호출한다. 메모리는 그대로 남는다.


## Example

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

# References

- [std::allocator<T> 클래스](https://woo-dev.tistory.com/51)


