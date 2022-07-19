# c++ 타입 캐스팅

c 언어에서 사용하는 타입 캐스팅 방식을 c++에서 사용할 수 있지만 타입 캐스팅을 위한 연산자가 따로 제공된다. (static_cast, dynamic_cast, reinterpret_cast, const_cast)  
기존에 사용하는 c 스타일의 방식은 의도를 파악하기 어렵고, 가차없이 진행되지만 c++ 스타일의 방식으로 조금더 안정적인 코드를 작성할 수 있다.  

## static_cast 연산자

 - 컴파일 단계에서 타입 캐스팅이 적합한지 검사하고 에러를 발생시킨다.  
 - 기본 자료형 간의 타입 캐스팅에 사용된다. 
 - 포인터 타입을 다른 타입으로 변환하지 않는다.
 - 상속 관계에 있는 포인터 간의 변환이 가능하다.
 - downcast시에는 unsafe하게 동작할 수 있다.  

```cpp
double  d = 3.14;
int     x = static_cast<int>(d);

std::cout << x << std::endl;  // 3
```

 - 부모-자식 클래스 간의 타입 캐스팅을 지원하여 컴파일은 가능하지만 안전하지 않다.
 - `부모 -> 자식`, `자식 -> 부모` 모두 가능하다.
 - `부모 -> 자식`의 경우에 자식 클래스의 속성에 임의의 값이 들어간다.

### 부모 -> 자식 형변환

```cpp
class	Parent
{
	private :
		int	x;
	public :
		Parent(int x) : x(x)
		{
		}
		void	print_x(void)
		{
			std::cout << "x: " << x << std::endl;
		}
};

class	Child : public Parent
{
	private :
		int	y;
	public :
		Child(int x, int y) : Parent(x), y(y)
		{
		}
		void	print_point(void)
		{
			Parent::print_x();
			std::cout << "y: " << y << std::endl;
		}
};

int	main(void)
{
	Parent*	p1 = new Child(10, 20);
	Child*	c1 = static_cast<Child*>(p1);
	c1->print_point();
	
	Parent*	p2 = new Parent(10);
	Child*	c2 = static_cast<Child*>(p2);
	c2->print_point();
}

// c2->y 에는 임의의 값이 들어간다.
```

### Errors

```cpp
double	d = 3.14;
double	*ptr = &d;
int		*x = static_cast<int*>(d);

std::cout << x << std::endl;
/*
<컴파일 에러>
error: cannot cast from type 'double' to pointer type 'int *'
*/
```

```cpp
double	d = 3.14;
double	*ptr = &d;
int		*x = static_cast<int*>(ptr);

std::cout << x << std::endl;
/*
<컴파일 에러>
error: static_cast from 'double *' to 'int *' is not allowed
*/
```

## dynamic_cast 연산자

- 상속 관계에서 static_cast보다 안정적으로 타입 캐스팅을 처리한다. 
- `자식 -> 부모` 변환에서 safe downcasting이 가능하다.
- dynamic_cast는 런타임에서 안정성 검사를 진행한다.  
- 컴파일 시점에는 변환하려는 클래스가 다향성 클래스인지 검사한다.
- 런타임 시간에 해당 타입이 다운 캐스팅이 가능한지 검사하기 때문에 런타임 비용이 조금 높다.
- 성공할 경우 new_type의 value를 반환한다.
- 실패한 경우 
  - new_type이 포인터라면 null pointer
  - new_type이 참조라면 bad_cast (exception) 

```cpp
int	main(void)
{
	Parent*	p1 = new Child(10, 20);
	Child*	c1 = dynamic_cast<Child*>(p1); 
	// 컴파일 에러: error: 'Parent' is not polymorphic

	c1->print_point();
	
	Parent*	p2 = new Parent(10);
	Child*	c2 = dynamic_cast<Child*>(p2);
	// 컴파일 에러: error: 'Parent' is not polymorphic

	c2->print_point();
}
```

- dynamic_cast는 가상 함수를 가진 다향성 클래스에 한해서는 `부모 -> 자식` 타입 캐스팅을 허용한다.  

```cpp
Parent*	p1 = new Child(10, 20);
Child*	c1 = dynamic_cast<Child*>(p1); // error: 'Parent' is not polymorphic
```

```cpp
class	Parent
{
	// ...
	
	virtual void	print_x(void) // 가상 함수
};
```

```cpp
Parent*	p1 = new Child(10, 20);
Child*	c1 = dynamic_cast<Child*>(p1); // 컴파일 성공
```

- 부모 객체를 자식 클래스로 변환하면 컴파일을 성공하지만 null pointer가 반환된다.

```cpp
Parent	*p2 = new Parent(10);
ChildA	*c2 = dynamic_cast<ChildA *>(p2); 

std::cout << "isNull: " << (c2 == NULL ? "true" : "false") << std::endl;

// isNull: true
```

[dynamic_cast](https://blockdmask.tistory.com/241?category=249379)

## reinterpret_cast 연산자

- 포인터/참조의 타입 캐스팅 연산자로서 타입을 재해석하는 수준으로 변환을 진행한다.  
- 변환을 강제하기 때문에 안전하지 않다.  

```cpp
Person*	person = new Person();
int*	i = reinterpret_cast<int*>(person);
```

## const_cast 연산자

 - 포인터 또는 참조형의 const를 잠시 제거하는데 사용된다.
 - volatile 키워드를 잠시 제거하는데에도 사용 가능하다.
 - 다른 형변환 연산자처럼 타입을 변형하지 못한다.
 - 함수 포인터에는 사용하지 못한다.

```cpp
char        str[] = "hello";
const char* str1 = str;
char*		    str2 = const_cast<char*>(str1);

str2[0] = 'H';
std::cout << str << std::endl;  // Hello
std::cout << str1 << std::endl; // Hello
```

### Errors

```cpp
int  main(void)
{
  int	      i = 0;
  const int *ptr = &i;
  double    *d = const_cast<double *>(ptr);

  std::cout << d << std::endl;
  return (0);
}

/*
< 컴파일 에러 >
error: const_cast from 'const int *' to 'double *' is not allowed
*/
```

```cpp
int  main(void)
{
  int	      i = 0;
	const int *ptr = &i;
	double    *d = const_cast<double>(ptr);

	std::cout << d << std::endl;
  return (0);
}

/*
< 컴파일 에러 >
error: const_cast to 'double', which is not a reference,
      pointer-to-object, or pointer-to-data-member
*/
```

[c++ 형변환 연산자 정리](https://mynameisdabin.tistory.com/20)
