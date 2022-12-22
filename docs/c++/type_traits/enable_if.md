---
layout: default
title: enable_if
nav_order: 2
grand_parent: c++ 
parent: type_traits
permalink: /docs/c++/type_traits/enable_if
---

- [enable\_if](#enable_if)
  - [Example](#example)
- [References](#references)

# enable_if

`enable_if`는 다음과 같이 정의될 수 있다.   

```cpp
template <bool, typename T = void>
struct enable_if {};

template <typename T>
struct enable_if<true, T> {
    typedef T type;
}
```

`enable_if`는 첫번째 템플릿 매개변수가 `true`이라면 두번째 템플릿 매개변수의 `typename`을 맴버 유형으로 활성화한다. 첫번째에서 `false`가 나온다면 함수를 오버로딩 후보에서 제외한다.  

## Example

```cpp
template <class T, 
          typename std::enable_if<std::is_integral<T>::value, T>::type* = nullptr>
void do_stuff(T& t) {
  std::cout << "do_stuff integral\n";
  // 정수 타입들을 받는 함수 (int, char, unsigned, etc.)
}

template <class T,
          typename std::enable_if<std::is_class<T>::value, T>::type* = nullptr>
void do_stuff(T& t) {
    std::cout << "do_stuff class\n";
  // 일반적인 클래스들을 받음
}
```

`do_stuff(int)` 함수를 호출하면 컴파일러는 첫번째 오버로딩을 선택한다. 두번째 오버로딩에서 `std::is_class<T>::value`가 `false`이기 때문이다.  


# References

- [cplusplus enable_if](https://en.cppreference.com/w/cpp/types/enable_if)  
