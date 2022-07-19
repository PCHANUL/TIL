# c++ 소멸자

소멸자란 개체가 범위를 벗어나거나 호출에 의해 명시적으로 제거될 때 자동으로 호출되는 멤버 함수이다. 소멸자의 이름은 클래스와 같고, 앞에 `~`가 붙는다.  

예제 코드에서 `String::~String` 소멸자는 `delete`를 사용하여 동적 할당된 공간의 할당을 취소한다.

```cpp
// spec1_destructors.cpp
#include <string>

class String {
public:
   String( char *ch );  // Declare constructor
   ~String();           //  and destructor.
private:
   char    *_text;
   size_t  sizeOfText;
};

// Define the constructor.
String::String( char *ch ) {
   sizeOfText = strlen( ch ) + 1;

   // Dynamically allocate the correct amount of memory.
   _text = new char[ sizeOfText ];

   // If the allocation succeeds, copy the initialization string.
   if( _text )
      strcpy_s( _text, sizeOfText, ch );
}

// Define the destructor.
String::~String() {
   // Deallocate the memory that was previously reserved
   //  for this string.
   delete[] _text;
}

int main() {
   String str("The piper in the glen...");
}
```

## 소멸자 선언

- 클래스와 이름이 같으며 물결표(~)가 앞에 붙는다.
- 인수를 받아들이지 않는다.
- 값(또는 void)를 반환하지 않아야 한다.

## 소멸자 사용

소멸자는 클래스 멤버 함수를 자유롭게 호출하고 클래스 멤버 데이터에 접근할 수 있다.  
다음 이벤트 중 하나가 발생하면 소멸자가 호출된다.  

- 블록 스코프에 지역 개체가 있는 상태에서 블록 스코프를 벗어난 경우
- `new`키워드로 할당된 개체가 `delete`키워드로 제거된 경우
- 임시 개체의 수명이 종료된 경우
- 프로그램이 종료되고 전역 또는 정적 개체가 있는 경우
- 소멸자는 소멸자 함수의 정규화된 이름을 사용하여 명시적으로 호출된다.  

소멸자 사용에 두가지 제한 사항이 있다.

- 주소를 가져갈 수 없다.
- 파생 클래스는 기본 클래스의 소멸자를 상속하지 않는다.

## 파괴 순서

개체가 범위를 벗어나거나 삭제된 경우 개체가 완전히 소멸되는 이벤트 순서는 다음과 같다.

1. 클래스의 소멸자가 호출되고 소멸자 함수의 본문이 실행된다.
2. 비정적 멤버 개체의 소멸자는 클래스 선언에 나타나는 역순으로 호출된다. 이러한 멤버의 생성에 사용되는 선택적 멤버 초기화 목록은 생성 또는 소멸 순서에 영향을 주지 않는다.
3. 비가상 기본 클래스의 소멸자는 선언의 역순으로 호출된다.
4. 가상 기본 클래스의 소멸자는 선언의 역순으로 호출된다.

```cpp
// order_of_destruction.cpp
#include <cstdio>

struct A1      { virtual ~A1() { printf("A1 dtor\n"); } };
struct A2 : A1 { virtual ~A2() { printf("A2 dtor\n"); } };
struct A3 : A2 { virtual ~A3() { printf("A3 dtor\n"); } };

struct B1      { ~B1() { printf("B1 dtor\n"); } };
struct B2 : B1 { ~B2() { printf("B2 dtor\n"); } };
struct B3 : B2 { ~B3() { printf("B3 dtor\n"); } };

int main() {
   A1 * a = new A3;
   delete a;
   printf("\n");

   B1 * b = new B3;
   delete b;
   printf("\n");

   B3 * b2 = new B3;
   delete b2;
}

Output: A3 dtor
A2 dtor
A1 dtor

B1 dtor

B3 dtor
B2 dtor
B1 dtor
```

[c++ 소멸자 참조](https://docs.microsoft.com/en-us/cpp/cpp/destructors-cpp?view=msvc-170)  
[인스턴스 배열의 소멸자는 역순으로 호출](https://stackoverflow.com/questions/31101854/order-of-destruction-for-stack-heap-allocated-arrays/31102112)
