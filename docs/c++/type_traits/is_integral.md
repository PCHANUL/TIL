---
layout: default
title: is_integral
nav_order: 2
grand_parent: c++ 
parent: type_traits
permalink: /docs/c++/type_traits/is_integral
---

- [is_integral](#is_integral)
  - [Example](#example)

# is_integral

```cpp
template <class T>
struct is_integral;
```

`T` 타입이 정수 타입인지 확인한다. `T`가 정수 타입이라면 `true_type`을 상속하고, 아니라면 `false_type`을 상속한다. 모든 기본 정수 형식과 모든 별칭, const 및 volatile 변형까지 이 클래스에서 정수 타입으로 간주된다. enum은 정수형으로 간주되지 않는다. 

## Example

```cpp
// is_integral example
#include <iostream>
#include <type_traits>

int main() {
  std::cout << std::boolalpha;
  std::cout << "is_integral:" << std::endl;
  std::cout << "char: " << std::is_integral<char>::value << std::endl;
  std::cout << "int: " << std::is_integral<int>::value << std::endl;
  std::cout << "float: " << std::is_integral<float>::value << std::endl;
  return 0;
}
``` 


