---
layout: default
title: string
nav_order: 2
parent: c++ 
permalink: /docs/c++/string
---

* [std::string](#stdstring)

# std::string


c++에는 문자열 자료형이 내장되어 있다. 문자열을 사용하기위해 `<string>`을 포함시킨다.

```cpp
#include <string>

std::string	str;
```

다음과 같이 문자열을 초기화할 수 있다.

```cpp
std::string str("Hello");
str = "world";
```

`std:string`을 `std:cin`과 함께 사용하는 경우에는 주의해야 한다. 표준 입력으로 공백이 포함된 문자열을 받는다면 입력된 문자열을 한번에 받지 않고 공백으로 구분하여 하나씩 받는다.

```cpp
#include <iostream>
#include <string>

int main(void)
{
	std::string	first;
	std::string	last;

	std::cout << "-> ";
	std::cin >> first;
	std::cout << "-> ";
	std::cin >> last;

	std::cout << "first: " << first << std::endl;
	std::cout << "last: " << last << std::endl;
}

// 공백이 없는 입력
// -> nick
// -> name
// first: nick
// last: name

// 공백이 있는 입력
// -> nick name
// -> first: nick
// last: name
```
