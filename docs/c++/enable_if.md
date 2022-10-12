---
layout: default
title: const
nav_order: 2
parent: c++ 
permalink: /docs/c++/const
---

- [SFINAE](#sfinae)
- [enable_if](#enable_if)
  - [Example](#example)
- [References](#references)

# SFINAE 

템플릿과 함수의 오버로딩을 같이 사용하면 다음과 같은 문제가 발생된다.  

```cpp
void foo(unsigned i) {
  std::cout << "unsigned " << i << "\n";
}

template <typename T>
void foo(const T& t) {
  std::cout << "template " << t << "\n";
}

void main(void) {
    foo(42);    // prints "template 42"
}
```

`foo(42)`를 호출하면 `template 42`가 출력된다. 컴파일러는 두 버전의 함수 중에 호출할 함수를 찾는다. 이때 `void foo(unsigned i)` 함수를 호출하려면 타입 변환이 필요하지만 `void foo(const T& t)`는 타입 변환없이 호출이 가능하기 때문에 이 함수가 호출된다.  

SFINAE(Substitution Failure Is Not An Error)는 C++에서 템플릿 인자 치환이 실패한 경우 컴파일러는 이 오류를 무시하고, 오버로딩 후보에서 제외하는 규칙이다. C++ 표준 문구는 다음과 같다.  

> 만일 템플릿 인자 치환이 올바르지 않는 타입이나 구문을 생성한다면 타입 유추는 실패합니다. 올바르지 않는 타입이나 구문이라 하면, 치환된 인자로 썼을 때 문법상 틀린 것을 의미 합니다. 이 때, 함수의 즉각적인 맥락(immediate context)의 타입이나 구문만이 고려되고, 여기에서 발생한 오류 만이 타입 유추를 실패시킬 수 있습니다. 그 이후에, 올바르지 않다고 여겨지는 여러가지 상황들을 확인하면서 (예컨대 클래스가 아닌 타입이나, void 의 레퍼런스를 생성한다든지 등등) 이를 오버로딩 후보 목록에서 제외시킵니다.

```cpp
void foo(unsigned i) {
  std::cout << "unsigned " << i << "\n";
}

template <typename T>
void foo(const T& t) {
  std::cout << "template " << t << "\n";
  typename T::value_type n = -t();
}

void main(void) {
    foo(42);    // error: type 'int' cannot be used prior to '::' because it has no members
}
```

위의 예시에서 `foo(42)`의 경우에 SFINAE 규칙으로 인해 함수가 호출되지만 `T::value_type`에서 컴파일 오류가 발생된다. `foo`함수 안에 있는 `T::value_type`은 SFIAE에서 말하는 `함수의 즉각적인 맥락`에서 벗어나 있기 때문이다. 만약에 특정한 타입에만 동작하는 템플릿을 작성하고 싶다면 함수의 선언부에서 타입 치환 오류를 발생시켜야 한다. 이를 통해 컴파일러는 해당 함수를 오버로딩 후보에서 제외하고 필요없는 컴파일 오류를 발생시키지 않을 수 있다.  

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
- [SFINAE와 enable_if](https://modoocode.com/255)  
