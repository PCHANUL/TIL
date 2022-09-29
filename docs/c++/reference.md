# 참조자 - reference

포인터 변수는 메모리에 주소값을 저장하고, 읽어서 실제 데이터에 접근한다. C 언어에서는 변수나 상수를 가리키는 방법으로 포인터를 사용하였지만 c++에서는 포인터와 함께 참조자라는 방법을 사용할 수 있다.

```cpp
#include <iostream>

int	main( void )
{
	int		num = 10;
	int*	ptr = &num;
	int&	ref = num;

	// 변수
	std::cout << "num: " << num << std::endl;
	
	// 포인터
	std::cout << "ptr: " << ptr << std::endl;
	std::cout << "*ptr: " << *ptr << std::endl;
	
	// 참조자
	std::cout << "ref: " << ref << std::endl;
	std::cout << "&ref: " << &ref << std::endl;
}

// num: 10
// ptr: 0x7ff7b25ad5fc
// *ptr: 10
// ref: 10
// &ref: 0x7ff7be3da5fc
```

참조자는 `*` 대신에 `&`를 붙여서 선언하면 되고, 선언할 때에 반드시 참조할 변수를 명시해주어야 한다. 그리고 선언된 참조자는 자신이 참조하는 대상을 변경할 수 없다. 이후 동작들은 참조 대상의 값을 변경시킬 뿐이다. 다른 참조자를 대입하려해도 참조하는 주소는 변경되지 않고 데이터 값이 변경된다.  

```cpp
ref = 20;
std::cout << "num: " << num << std::endl;
std::cout << "ref: " << ref << std::endl;

// num: 20
// ref: 20
```

```cpp
int		new_num = 30;
int&	other_ref = new_num;

ref = other_ref;
std::cout << ref << " : " << &ref << std::endl;
std::cout << other_ref << " : " << &other_ref << std::endl;

// 30 : 0x7ff7bd3a75fc
// 30 : 0x7ff7bd3a75e4
```

다음은 함수가 참조자를 인자로 받는 경우이다.  

```cpp
void	change_num(int& n)
{
	n = 20;
}

int	main( void )
{
	int		num = 10;
	int&	ref = num;
	
	std::cout << "num: " << num << std::endl;
	std::cout << "ref: " << ref << std::endl;
	change_num(ref);
	std::cout << "num: " << num << std::endl;
	std::cout << "ref: " << ref << std::endl;
}

// num: 10
// ref: 10
// num: 20
// ref: 20
```

[포인터 위키백과](https://ko.wikipedia.org/wiki/%ED%8F%AC%EC%9D%B8%ED%84%B0_(%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D)#:~:text=%ED%8F%AC%EC%9D%B8%ED%84%B0(pointer)%EB%8A%94%20%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D%20%EC%96%B8%EC%96%B4,%EA%B2%83%EC%9D%84%20%EC%97%AD%EC%B0%B8%EC%A1%B0%EB%9D%BC%EA%B3%A0%20%ED%95%9C%EB%8B%A4.)  
[c++ 참조자](https://modoocode.com/141)
