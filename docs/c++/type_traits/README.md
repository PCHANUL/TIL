---
layout: default
title: type_traits
nav_order: 2
parent: c++ 
has_children: true
permalink: /docs/c++/type_traits
---

- [type traits](#type-traits)
  - [Example](#example)

# type traits

C++에서 traits는 어떻게 사용되는지 확인하기 위해서 type traits를 알아본다. type traits는 C++ 템플릿 메타 프로그래밍에서 유용하게 쓰이는 기술 중에 하나이다. type traits를 이용하여 type의 다양한 속성들에 대해 조사하거나 프로퍼티를 변경할 수 있다. 예를 들어 제네릭 타입인 T가 있는 경우, 제네릭 인자로 넘어온 타입 T에 따라서 컴파일되는 코드가 달라지는 조건부 컴파일에 사용된다. 그리고 특정 타입에 const를 붙이거나 제거하거나 참조로 만들거나 포인트로 만들어 타입을 변형하는데 사용된다. 이 모든 것이 컴파일 타임에 완료되어 실행 시간의 패널티 없이 동작한다. 이를 바로 메타 프로그래밍이라고 한다.  

type traits는 타입에 대한 상수 정보를 가진 구조체이다. 단순한 템플릿 구조체이지만 타입의 프로퍼티들에 대한 정보를 상수 맴버 변수에 담고 있다. `<type_traits>` 헤더에 정의되어 있는 type traits 중에 하나인 `std::is_unsigned`를 살펴보면 다음과 같다.  

```cpp
template<typename T>
struct is_unsigned;
```

타입 T가 부호 없는 정수인지 아닌지 알려준다. 내부에 정의된 상수 맴버 변수인 value는 T에 따라서 true 또는 false로 정해진다.  

```cpp
#include <iostream>
#include <type_traits> // C++11

int main() 
{
  ​​​​std::cout << std::is_unsigned<unsigned int>::value << '\n';
  ​​​​std::cout << std::is_unsigned<int>::value << '\n';
}

// 1
// 0
```

## Example

다음은 실제 시나리오에서 type traits를 사용하는 방법을 알아본다. 만약에 두개의 함수가 동일한 알고리즘을 가지고 있지만 하나는 부호 있는 정수에 대해서 작동하고, 나머지는 부호 없는 정수에 대해서 작동한다고 가정해본다. 이런 경우에 넘어오는 타입에 따라서 알맞는 함수가 호출되도록 조건부 컴파일을 하고 싶을 것이다.  

```cpp
void algorithm_signed  (int i)      { /* signed int에 최적화된 알고리즘*/ } 
void algorithm_unsigned(unsigned u) { /* unsigned int에 최적화된 알고리즘 */ } 

template <typename T>
void algorithm(T t)
{
  ​​​​if constexpr(std::is_signed<T>::value)
  ​​​​{
    ​​​​​​​​algorithm_signed(t);
  ​​​​}
  ​​​​else
  ​​​​{
    ​​​​​​​​if constexpr (std::is_unsigned<T>::value)
    ​​​​​​​​{
      ​​​​​​​​​​​​algorithm_unsigned(t);
    ​​​​​​​​}
    ​​​​​​​​else
    ​​​​​​​​{
      ​​​​​​​​​​​​static_assert(std::is_signed<T>::value || std::is_unsigned<T>::value, "Must be signed or unsigned!");
    ​​​​​​​​}
  ​​​​}
}
```

