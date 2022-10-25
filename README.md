---
layout: default
title: Today I Learned
nav_order: 1
has_children: true
child_nav_order: desc
permalink: /
---

- [22.10.25](#221025)
  - [reverse_iterator](#reverse_iterator)
  - [explicit keyword](#explicit-keyword)
- [22.10.07](#221007)
  - [type traits](#type-traits)
  - [iterator_traits](#iterator_traits)

# Today I Learned <!-- omit in toc -->

## 22.10.25

### reverse_iterator

역방향 반복자는 반복자의 방향을 바꾸는 반복자 어댑터이다. 양방향 반복자가 역방향 반복자가 되면 끝에서 시작으로 이동하는 새로운 반복자를 생성한다. 반복자 `i`에서 생성된 역방향 반복자 `r`의 경우에 `&*r == &*(i-1)` 관계가 항상 `true`이다. 반복자가 반전될 때에 동일한 요소가 아니라 그 앞에 있는 요소를 가리킨다. 이는 반복자의 마지막 요소를 정렬하기 위함이다. 

```cpp
// reverse_iterator example
#include <iostream>     // std::cout
#include <iterator>     // std::reverse_iterator
#include <vector>       // std::vector

int main () {
  std::vector<int> myvector;
  for (int i=0; i<10; i++) myvector.push_back(i);

  typedef std::vector<int>::iterator iter_type;
                                                         // ? 9 8 7 6 5 4 3 2 1 0 ?
  iter_type from (myvector.begin());                     //   ^
                                                         //         ------>
  iter_type until (myvector.end());                      //                       ^
                                                         //
  std::reverse_iterator<iter_type> rev_until (from);     // ^
                                                         //         <------
  std::reverse_iterator<iter_type> rev_from (until);     //                     ^

  std::cout << "myvector:";
  while (rev_from != rev_until)
    std::cout << ' ' << *rev_from++;
  std::cout << '\n';

  return 0;
}
```

### explicit keyword

`explicit` 키워드는 원하지 않는 형변환이 일어나지 않도록 제한하는 키워드이다. 함수에 명시된 타입과 다른 타입의 인자가 들어온다면 자동적으로 형변환을 위해 생성자가 호출된다. 컴파일러가 알아서 형변환하는 상황에는 예상하지 못한 버그가 발생될 수 있기 때문에 `explicit` 키워드로 이를 방지할 수 있다.  

```cpp
#include <iostream>
class A{
public:
   int num;
   explicit A(int n) : num(n){};
};
void printA(A a){
   std::cout << a.num << std::endl;
}
int main(){
   int n = 26;
   printA(n); //Error!
}
```

- 참조 : https://dydtjr1128.github.io/cpp/2019/07/13/Cpp-explicit-keyowrd.html



## 22.10.07

### type traits

C++에서 traits는 어떻게 사용되는지 확인하기 위해서 type traits를 알아본다. type traits는 C++ 템플릿 메타 프로그래밍에서 유용하게 쓰이는 기술 중에 하나이다. type traits를 이용하여 type의 다양한 속성들에 대해 조사하거나 프로퍼티를 변경할 수 있다. 예를 들어 제네릭 타입인 T가 있는 경우, 제네릭 인자로 넘어온 타입 T에 따라서 컴파일되는 코드가 달라지는 조건부 컴파일에 사용된다. 그리고 특정 타입에 const를 붙이거나 제거하거나 참조로 만들거나 포인트로 만들어 타입을 변형하는데 사용된다. 이 모든 것이 컴파일 타임에 완료되어 실행 시간의 패널티 없이 동작한다. 이를 바로 메타 프로그래밍이라고 한다.  

### iterator_traits

iterator_traits는 반복자의 속성을 정의하는 클래스이다. 템플릿은 컴파일 타임에 인스턴스화되기 때문에 컴파일 타임에 조건에 따른 처리를 위해 `traits` 클래스를 사용한다. 템플릿 클래스를 사용하다보면 흔히 발생할 수 있기 때문에 반복자 이외에도 라이브러리에서 쉽게 찾아 볼 수 있다.  

