---
layout: default
title: template
nav_order: 2
parent: c++ 
permalink: /docs/c++/template
---

* [template](#template)
    * [Function Template](#function-template)
    * [Class Template](#class-template)
    * [Template specialization](#template-specialization)
    * [템플릿에 대한 비유형 매개변수](#템플릿에-대한-비유형-매개변수)

# template

template는 함수나 클래스를 개별적으로 다시 작성하지 않아도, 여러 자료형으로 사용할 수 있도록 만들어 놓은 틀이다.  
template는 Function Template와 Class Template로 나뉘어진다.  
template 매개변수는 타입을 인수로 전달하는데 사용할 수 있는 특별한 종류의 매개변수이다. 
template를 선언하는 형식은 다음과 같다. 

```cpp
template <class identifier> function_declaration;
template <typename identifier> function_declaration;
```

### Function Template

함수를 정의할 때, 함수의 기능은 명확하지만 자료형을 모호하게 둔다. 
c++에서는 다형성의 오버로딩 특성에 의해서 함수 이름이 같아도 되기 때문에 다음과 같이 함수를 정의하게 된다.  

```cpp
int sum(int a, int b)
{
    return (a + b);
}

double sum(double a, double b)
{
    return (a + b);
}
```

인자의 타입을 다르게 하여 같은 이름의 함수를 반복적으로 정의해야 한다. 이렇게 반복해야하는 문제를 해결하기 위해 template를 사용한다. 다음은 위의 코드를 template로 작성한 코드이다.  

```cpp
template <typename T>
T sum(T a, T b)
{
    return (a + b);
}
```

두개의 인자가 타입이 다른 경우

```cpp
template <class T1, class T2>
void printAll(T1 a, T2 b)
{
    cout << "T1: " << a << endl;
    cout << "T2: " << b << endl;
}
```

이 함수 템플릿을 사용하기 위해서는 다음과 같은 형식을 사용한다.

```cpp
function_name <type> (parameters);
```

만약에 위에 구현된 함수를 호출하는 경우에는 다음과 같이 작성할 수 있다. 

```cpp
int x, y;
sum <int> (x, y);
```

컴파일러는 템플릿 함수에 대한 호출을 만나면 실제 템플릿 매개 변수로 전달된 타입으로 대체하는 함수를 자동으로 생성한 다음 호출한다. 

```cpp

template <class T>
T sum(T a, T b)
{
    T   result;
    result = a + b;
    return (result);
}

int main(void)
{
    int i=5, j=10, k;
    long x=5, y=10, z;

    k = sum<int>(i, j);
    z = sum<long>(x, y);

    std::cout << k << std::endl;
    std::cout << z << std::endl;
    return (0);
}
```

위의 예에서는 함수 템플릿 sum()을 두번 사용했다. 처음에는 int 타입의 인수를 사용하고, 다음은 long 타입의 인수를 사용했다. 컴파일러는 적절한 함수를 인스턴스화한 다음에 호출한다. `T` 타입이 sum() 템플릿 함수안에서 객체를 선언하는 데에도 사용된다.  

컴파일러는 각 호출에 필요한 타입을 자동으로 결정한다. 템플릿 함수가 동일한 타입의 인수가 들어올 것이라고 예상하고 있기 때문에 다른 타입의 인수를 보내면 컴파일 에러가 발생한다. 만약에 두 인수의 타입을 다르게 한다면 다음과 같이 둘 이상의 타입 매개 변수를 허용하는 함수 템플릿을 정의할 수도 있다.  

```cpp
template <class T, class U>
T GetMin(T a, U b)
{
    return (a<b?a:b);
}
```

이 경우에 다른 두 매개변수를 허용하고 함수는 T 유형의 객체를 반환한다.  

### Class Template

클래스 템플릿을 작성하여 템플릿 매개변수를 유형으로 사용하는 멤버를 가질 수 있다.  

```cpp
template <class T>
class mypair {
    T values [2];
  public:
    mypair (T first, T second)
    {
      values[0]=first; values[1]=second;
    }
};
```

이 클래스의 객체를 선언하여 사용하려면 다음과 같이 작성할 수 있다. 

```cpp
mypair<int> myobject (115, 36);

mypair<double> myfloats (3.0, 2.18);
```

클래스 템플릿 선언의 외부에 함수 멤버를 정의한다면 정의 앞에 템플릿 <...> 점두사를 붙여야 한다.  

```cpp
// class templates
#include <iostream>
using namespace std;

template <class T>
class mypair {
    T a, b;
  public:
    mypair (T first, T second)
      {a=first; b=second;}
    T getmax ();
};

template <class T>
T mypair<T>::getmax ()
{
  T retval;
  retval = a>b? a : b;
  return retval;
}

int main () {
  mypair <int> myobject (100, 75);
  cout << myobject.getmax();
  return 0;
}
```

### Template specialization

다음은 클래스 템플릿 전문화에 사용되는 구문이다.

```cpp
template <> class mycontainer <char> { ... };
```

클래스 템플릿 이름 앞에 빈 template<> 매개변수 목록이 된다. 템플릿 전문화를 명시적으로 선언하는 것이다. 이 접두사보다 더 중요한 것은 클래스 템플릿 이름 위에있는 <char> 매개변수이다. 이 특수화 매개변수 자체는 템플릿 클래스 특수화를 선언할 유형을 식별한다. 일반 클래스 템플릿과 전문화 간의 차이점을 확인할 수 있다. 

```cpp
template <class T> class mycontainer { ... };   // 일반 템플릿
template <> class mycontainer <char> { ... };   // 전문화
```

템플릿의 특정 패턴에 대해서 별도의 처리가 하고 싶은 경우에 사용할 수 있다.  
예를 들어 다음과 같은 상황이 있을 수 있다.  

```cpp
template <typename T>
T Max(const T a, const T b)
{
    return (a > b ? a : b);
}

int main(void)
{
    std::cout << Max<int>(100, 200) << std::endl;
    std::cout << Max<const char*>("one", "two") << std::endl;
    return (0);
}
```

`int` 타입인 경우에는 정상적으로 크기를 비교하지만, `const char *` 타입인 경우에는 단순히 주소를 비교하여 출력된다.  
위의 코드에 특수화를 사용하여 제대로 동작하도록 수정하면 다음과 같다.  

```cpp
template <typename T>
T Max(const T a, const T b)
{
    return (a > b ? a : b);
}

// char* 타입에 대한 특수화
template <>
char* Max<char*>(char* a, char* b)
{
    return (strcmp(a, b) > 0 ? a : b);
}

// const char* 타입에 대한 특수화
template <>
const char* Max<const char*>(const char* a, const char* b)
{
    return (stdlen(a) > strlen(b) ? a : b);
}

int main(void)
{
    std::cout << Max<int>(100, 200) << std::endl;
    std::cout << Max<const char*>("one", "two") << std::endl;
    return (0);
}
```

`char*`, `const char*` 타입의 함수를 직접 명시하므로 컴파일러가 해당 타입 함수가 필요할 때 임의대로 만들지 않고 명시된 함수를 사용하도록 만든다. 

### 템플릿에 대한 비유형 매개변수

유형을 나타내는 class 또는 typename 키워드 대신에 일반 타입 매개변수를 가질 수도 있다. 일반 타입 매개변수를 설정하면 다음과 같이 정의된다.

```cpp
template <class T=char, int N=10> class mysequence { ... };
```


[template에 관하여](https://blockdmask.tistory.com/43)
[template](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=4roring&logNo=221145837492)
