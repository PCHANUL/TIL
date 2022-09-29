# iostream

## cout

c++은 iostream 라이브러리에 있는 std::cout 객체를 사용하여 콘솔에 출력할 수 있다.  

```cpp
std:cout << [ 출력할 값 ]
```

출력하려는 값의 타입에 상관없이 출력 연산자 뒤에 넣어주면 된다.  
데이터 타입을 나타내는 서식문자가 필요없다.

```cpp
#include <iostream>

int main(void)
{
	int count = 0;
	
	std::cout << "counts: ";
	std::cout << count;
	return (0);
}

// counts: 0
```

개행을 사용하는 경우에는 개행 문자인 `\n`이나 `std::endl`을 사용하면 된다.

```cpp
#include <iostream>

int main(void)
{
	int count = 0;
	
	std::cout << "counts: " << std::endl;
	std::cout << count;
	return (0);
}

// counts:
// 0
```

## cin

`std::cin`으로 c++의 입력을 받을 수 있다.

```cpp
std::cin >> [ 입력값을 받을 변수 ]
```

`std::cin`로 `std::cout`과 마찬가지로 데이터 타입을 알려주는 서식이 필요없이 변수만 해주면 된다. 값을 받을때 `&`로 주소를 넘겨주지 않아도 된다.

```cpp
#include <iostream>

int main(void)
{
	int	num;
	
	std::cout << "num: ";
	std::cin >> num;
	std::cout << "num is " << num << std::endl;
}
```
