---
layout: default
title: constexpr
nav_order: 2
grand_parent: c++ 
parent: type_traits
permalink: /docs/c++/type_traits/constexpr
---

# constexpr

`C++ 11`에 도입된 `constexpr` 키워드는 객체나 함수 앞에 붙여서 컴파일 타임에 객체 값이나 함수 반환 값을 알 수 있다는 의미를 전달한다. 컴파일러가 컴파일 타임에 값을 결정할 수 있는 식을 상수식(Constant expression)이라고 한다.  

## constexpr / const

`const`로 정의된 상수는 컴파일 타임에 값을 몰라도 된다. 하지만 `constexpr`로 정의된 상수는 컴파일 타임에 값을 알아야 한다.  

```cpp
{
    int a;

    const int b = a;
    constexpr int c = a; // error: constexpr variable 'c' must be initialized by a constant expression
}
{
    const int a = 1;
    constexpr int b = a;
}
```


# Reference

[씹어먹는 C++ - <16 - 2. constexpr 와 함께라면 컴파일 타임 상수는 문제없어>](https://modoocode.com/293)