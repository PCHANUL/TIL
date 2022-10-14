---
layout: default
title: integral_constant
nav_order: 2
grand_parent: c++ 
parent: type_traits
permalink: /docs/c++/type_traits/integral_constant
---

# integral_constant

```cpp
template <class T, T v>
struct integral_constant {
  static constexpr T value = v;
  typedef T value_type;
  typedef integral_constant<T,v> type;
  constexpr operator T() { return v; }
};
```

이 템플릿은 컴파일 타임 상수를 타입으로 제공하도록 설계되었다. 표준 라이브러리의 여러 부분에서 특성 타입의 기본 클래스로 사용된다.  

## Example

```cpp
// factorial as an integral_constant
#include <iostream>
#include <type_traits>

template <unsigned n>
struct factorial : std::integral_constant<int,n * factorial<n-1>::value> {};

template <>
struct factorial<0> : std::integral_constant<int,1> {};

int main() {
  std::cout << factorial<5>::value;  // constexpr (no calculations on runtime)
  return 0;
}

// output: 120
```