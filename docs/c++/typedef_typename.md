---
layout: default
title: typedef/typename
nav_order: 2
parent: c++
permalink: /docs/c++/typedef_typename
---

* [typedef](#typedef)
	* [Template parameter](#template-parameter)
	* [Example](#example)
* [typename](#typename)
	* [Example](#example-1)
* [References](#references)

# typedef

타입의 새로운 별칭을 정의하는 키워드이다. 다음과 같은 이유로 `typedef`를 사용한다.  

1. 읽고, 입력하기 편하다.
2. 호환성이 높아진다.
   - 예를 들어서 플랫폼에 따라서 다른 이름을 사용해야하는 경우가 있다. 
```cpp
#if defined USING_COMPILER_A
    typedef __int32 Int32;
    typedef __int64 Int64;
#elif deined USING_COMPILER_B
    typedef int Int32;
    typedef long long Int64;
#endif
```
3. 유연성이 생긴다.
   - 예를 들어서 아래의 코드를 보면, vector를 나중에 list나 deque로 손쉽게 변경할 수 있다.
```cpp
typedef vector<Customer> Customers;
typedef Customers::iterator CustIter;
//...
void f( Customers& custs )
{
    CustIter i = custs.begin();
    //...
}
```

## Template parameter

템플릿 내부의 이름 중에 템플릿 매개변수에 종속된 것을 `의존 이름`(dependent name)이라고 한다. 그리고 `의존 이름`이 클래스 안에 중첩되어 있으면 `중첩 의존 타입 이름`(nestes dependent type name)이라고 한다.  

```cpp
#include <iostream>

template<typename T>
class MyTempClass
{
public:
    typedef T TempType; // 의존 이름
};

int main()
{
    MyTempClass<int> a;
    MyTempClass<int>::TempType b; // 중첩 의존 타입 이름
    MyTempClass<double> c;
    MyTempClass<double>::TempType d; // 중첩 의존 타입 이름
    return 0;
}
```

## Example

다음 예시는 들어오는 컨테이너의 모든 요소를 출력한다. 컨테이너가 무엇인지 모르고, 컨테이너의 요소가 무엇인지 알 수 없다. 하지만 각 컨테이너 안에는 상수 반복자를 `typedef` 키워드로 `const_iterator`라고 정의했기 때문에 어떤 컨테이너가 들어와도 함수가 동작한다.  

```cpp
template <class T>
inline void PRINT_ELEMENTS (const T& coll, const char* optcstr="")
{
    typename T::const_iterator pos;
 
    std::cout << optcstr;
    for (pos=coll.begin(); pos!=coll.end(); ++pos) {
        std::cout << *pos << ' ';
    }
    std::cout << std::endl;
}
```


# typename

템플릿 매개변수를 선언하는 경우에는 `typename`과 `class`가 동일하다. 하지만 `typename`은 중첩 의존 타입 이름을 식별하는 용도에서 사용된다. c++ 컴파일러에게 해당 타입이 `typedef`로 재정의된 타입이라는 사실을 알리기 위해 `typename`을 사용한다. 아니라면 클래스 내부의 멤버 변수라고 생각할 수 있다.  


## Example

다음 코드에서 `iterator`는 `vector<T>`의 의존 타입이기 때문에 `typename` 키워드를 사용해야 했다.  

```cpp
template <typename T>
void print_vector(std::vector<T>& vec) 
{
  typename std::vector<T>::iterator iter = vec.begin();
  
  while (iter != vec.end()) 
  {
    std::cout << *iter << std::endl;
	++iter;
  }
}
```

# References 

- [typedef와 typename 그리고 중첩의존타입이름](https://lecor.tistory.com/76)  


