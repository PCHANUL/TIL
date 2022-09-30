---
layout: default
title: deepCopy
nav_order: 2
parent: c++ 
permalink: /docs/c++/deepCopy
---

* [얕은 복사와 깊은 복사](#얕은-복사와-깊은-복사)
* [복사 생성자](#복사-생성자)
* [할당 연산자 오버로딩](#할당-연산자-오버로딩)

# 얕은 복사와 깊은 복사

새롭게 생성되는 변수에 다른 변수의 값을 대입하기 위해서 대입 연산자( `=` )를 사용한다. 객체도 마찬가지로 대입 연산자를 사용하여 다른 변수에 대입할 수 있다. 하지만 객체의 대입은 얕은 복사가 이루어진다.  
복사에는 얕은 복사와 깊은 복사가 있다. 얕은 복사는 동적 할당된 변수의 `주소값을 복사`하고, 깊은 복사는 새로 동적 할당하고 `데이터를 복사`한다.  

얕은 복사의 `주소값을 복사` 예시 코드는 다음과 같다. 

```cpp
#include <iostream>
#include <string>

class Person {
public:
    Person() {
        name = new std::string("nonamed");
    };
    Person(std::string n) {
        name = new std::string(n);
    }
	~Person() {
		delete name;
	}
    std::string	*name;
};

int main() {
    Person P1("Buddy");
    Person P2 = P1;

	// 출력: (P1 주소) (P1.name 주소) (P1.name 값)
	std::cout << "P1: ";
	std::cout << &P1 << " ";
	std::cout << P1.name << " ";
	std::cout << *P1.name << " " << std::endl;

	// 출력: (P2 주소) (P2.name 주소) (P2.name 값)
	std::cout << "P2: ";
	std::cout << &P2 << " ";
	std::cout << P2.name << " ";
	std::cout << *P2.name << " " << std::endl;

	// P1.name 값 변경
    *P1.name = "Jay";
	
	std::cout << "P1: ";
	std::cout << &P1 << " ";
	std::cout << P1.name << " ";
	std::cout << *P1.name << " " << std::endl;

	std::cout << "P2: ";
	std::cout << &P2 << " ";
	std::cout << P2.name << " ";
	std::cout << *P2.name << " " << std::endl;

    exit(EXIT_SUCCESS);
}

// P1: 0x7ff7bf9675f0 0x6000020d9120 Buddy 
// P2: 0x7ff7bf9675c0 0x6000020d9120 Buddy 
// P1: 0x7ff7bf9675f0 0x6000020d9120 Jay 
// P2: 0x7ff7bf9675c0 0x6000020d9120 Jay  
```  

P1.name의 값을 변경하였는데 P2.name의 값도 변경되었다. 변수의 주소가 같기 때문에 변수의 값이 항상 같다. 이렇게 동적 할당된 변수의 주소값을 복사해주는 방식이 `얕은 복사`이다. 새롭게 동적 할당을 하고 데이터를 복사하기 위해서는 `복사 생성자`와 `할당 연산자 오버로딩`을 클래스에 정의해주어야 한다.  


# 복사 생성자

c++에서 복사 생성자는 다른 객체의 참조를 인수로 받아서 자신을 초기화하는 방법이다. 복사 생성자로 초기화되는 객체는 깊은 복사가 된다. 반대로 디폴트 복사 생성자는 얕은 복사가 이루어진다.

```cpp
ClassName(const ClassName&);
```

복사 생성자는 다음의 상황에서 호출된다.  
1. 함수의 인자로 객체가 전달
2. 함수의 반환값으로 객체가 반환
3. 새로운 객체를 기존의 객체와 똑같이 초기화  

이전의 코드에서 복사 생성자를 추가하면 다음과 같은 결과를 얻게 된다. 생성된 객체의 name 변수 주소가 다르게 출력되며 변수가 변경되어도 다른 객체의 변수는 변경되지 않는다.  

```cpp
class Person {
public:

    // ...
	
	// 복사 생성자
	Person(const Person &origin) {
		name = new std::string(*origin.name);
	}
};

int	main() {
	
	// 출력 코드
	// (P1 주소) (P1.name 주소) (P1.name 값)
	// (P2 주소) (P2.name 주소) (P2.name 값)
	
}

// P1: 0x7ff7bbe3d5f0 0x600000051120 Buddy 
// P2: 0x7ff7bbe3d5c0 0x600000051140 Buddy 
// P1: 0x7ff7bbe3d5f0 0x600000051120 Jay 
// P2: 0x7ff7bbe3d5c0 0x600000051140 Buddy
```  

[복사 생성자](http://www.tcpschool.com/cpp/cpp_conDestructor_copyConstructor)  

# 할당 연산자 오버로딩

`복사 생성자`로 깊은 복사가 가능하지만 새로운 변수를 초기화하는 위치에서만 가능하다. 다음과 같이 새로운 변수의 초기화 이후에 대입하면 얕은 복사가 이루어진다.  

```cpp
class Person {
public:

    // ...
	
	// 복사 생성자
	Person(const Person &origin) {
		name = new std::string(*origin.name);
	}
};

int	main() {
	Person P1("Buddy");
    Person P2;
	P2 = P1;
	
	// 출력 코드
	// (P1 주소) (P1.name 주소) (P1.name 값)
	// (P2 주소) (P2.name 주소) (P2.name 값)
	
}

// P1: 0x7ff7b56535f0 0x600000c45120 Buddy 
// P2: 0x7ff7b56535c0 0x600000c45120 Buddy 
// P1: 0x7ff7b56535f0 0x600000c45120 Jay 
// P2: 0x7ff7b56535c0 0x600000c45120 Jay 
```  

복사 생성자를 정의해도 대입 연산자( `=` )를 사용하는 경우에 얕은 복사가 이루어진다. 그렇기 때문에 `할당 연산자 오버로딩`을 정의해야 한다.  

`연산자 오버로딩`은 두개의 객체가 이미 생성되어 있는 상태에서 호출되기 때문에 주의할 점이 있다. 이전에 동적 할당된 데이터를 해제해야 한다. 해제하지 않고 데이터를 복사하게 되면 이전의 데이터를 해제할 수 없기 때문에 메모리 누수 문제가 발생한다.  

```cpp
class Person {
public:

    // ...
	
	// 복사 생성자
	Person(const Person &origin) {
		name = new std::string(*origin.name);
	}
	
	// 대입 연산자 오버로딩
	Person& operator=(const Person &origin) {
		delete name;
		name = new std::string(*origin.name);
		return *this;
	}
};

int	main() {
	Person P1("Buddy");
    Person P2;
	P2 = P1;
	
	// 출력 코드
	// (P1 주소) (P1.name 주소) (P1.name 값)
	// (P2 주소) (P2.name 주소) (P2.name 값)
	
}

// P1: 0x7ff7bafd25f0 0x600003d65120 Buddy 
// P2: 0x7ff7bafd25c0 0x600003d65140 Buddy 
// P1: 0x7ff7bafd25f0 0x600003d65120 Jay 
// P2: 0x7ff7bafd25c0 0x600003d65140 Buddy 
```  

[대입 연산자 오버로딩](https://musket-ade.tistory.com/entry/C-%EB%8C%80%EC%9E%85-%EC%97%B0%EC%82%B0%EC%9E%90-%EC%98%A4%EB%B2%84%EB%A1%9C%EB%94%A9)
