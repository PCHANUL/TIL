---
layout: default
title: const
nav_order: 2
parent: c++ 
permalink: /docs/c++/const
---

* [const](#const)
	* [const 포인터](#const-포인터)
	* [const class overloading](#const-class-overloading)

# const

`const` 키워드는 값을 상수로 선언하여 변경할 수 없도록 한다. 한번 설정된 값이 read-only 메모리에 올라가게되어 변경하지 못하고 계속 유지하게 된다.

```cpp
const int	val = 0;
val = 1; // error!
```

## const 포인터

상수가 가리키는 공간을 수정하지 못하게 한다. `const`가 `int`에 적용되었기 때문에 포인터가 가리키고 있는 값을 변경할 수 없지만, 주소값은 변경할 수 있다.

```cpp
int			val = 0;
const int	*ptr = &val;

val = 1;	// ok
*ptr = 2;	// error
```

반대로 다음의 경우에는 주소값을 변경하지 못하게 한다. `const`가 포인터에 적용되어 주소값을 변경할 수 없지만, 포인터가 가리키고 있는 값을 변경할 수 있다.

```cpp
int			val_1 = 0;
int			val_2 = 0;
int* const	ptr = &val_1;

val_1 = 1;	// ok
*ptr = 2;	// ok
ptr = &val_2;	// error
``` 

## const class overloading

const 키워드는 값을 상수로 선언하여 변경할 수 없게 한다.  
함수 선언부에는 세가지 위치에서 사용할 수 있다.

`const` returnType function( `const` paramType& ) `const` { ... }

1. `const returnType` : 함수에서 반환되는 객체나 변수가 변경되지 않음을 나타낸다.
2. `const paramType&` : 함수에 전달되는 참조 값이 변경되지 않음을 나타낸다.
3. `const { ... }` : 함수 내에서 클래스 내부의 어떤 변수도 변경되지 않음을 나타낸다.


[c++ const 이해하기](https://dydtjr1128.github.io/cpp/2020/01/08/Cpp-const.html)  
[static 멤버와 const 멤버](http://www.parkjonghyuk.net/lecture/program2/chap06.pdf)
