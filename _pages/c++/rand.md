# rand 함수

```cpp
#include <cstdlib>

int	rand(void);

// 반환 : [0 ~ RAND_MAX] 범위에서 랜덤한 숫자
```

RAND_MAX는 stdlib.h 헤더 파일에 32767으로 작성되어있다. 0부터 32767 사이에서 랜덤한 값을 얻을 수 있지만 프로그램이 생성되는 시점에 정해져서 프로그램을 여러번 실행시켜도 같은 값을 반환한다.  

```cpp
#include <iostream>
#include <cstdlib>

int	main(void)
{
	std::cout << rand() << std::endl;
	std::cout << rand() << std::endl;
	std::cout << rand() << std::endl;
	return (0);
}

// 16807
// 282475249
// 1622650073

// 프로그램을 계속 실행해도 같은 값이 나온다.
```

## srand 함수

```cpp
#include <cstdlib>

void	srand(unsigned int seed);

// seed : srand는 seed값과 매칭되는 숫자를 정한다.
```

srand 함수에 매개변수로 들어오는 seed 값에 의해서 rand함수의 결과값이 달라진다.

```cpp
#include <iostream>
#include <cstdlib>

int	main(void)
{
	std::cout << rand() << std::endl;
	std::cout << rand() << std::endl;
	std::cout << rand() << std::endl;
	srand(1);
	std::cout << rand() << std::endl;
	srand(2);
	std::cout << rand() << std::endl;
	srand(3);
	std::cout << rand() << std::endl;
	srand(3);
	std::cout << rand() << std::endl;
	return (0);
}

// 16807
// 282475249
// 1622650073
// 16807
// 33614
// 50421
// 50421

// srand으로 값을 변경하지만 같은 seed 값으로 변경된 rand() 값은 항상 동일하다.
```

같은 seed 값은 동일한 랜덤값을 가지지만 만약에 항상 변하는 값을 seed로 넘겨준다면 원하는대로 호출할 때마다 랜덤한 값을 만들 수 있다.  
그래서 seed 값으로 시간 값을 넘겨주는 방법을 사용한다.  

```cpp
#include <iostream>
#include <cstdlib>
#include <time.h>

int	main(void)
{
	srand((unsigned int)time(NULL));
	std::cout << rand() << std::endl;
	return (0);
}

// 프로그램을 실행할 때마다 다른 랜덤값을 출력한다.
```

```cpp
#include <iostream>
#include <cstdlib>
#include <time.h>

int	main(void)
{
	int	range = 10;

	srand((unsigned int)time(NULL));
	std::cout << rand() % range << std::endl;
	return (0);
}

// 랜덤한 값을 특정값으로 나눈 나머지를 가져오는 방식으로 랜덤값의 범위를 지정한다.
```