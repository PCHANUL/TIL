# Today I Learned

[![Gitbook](https://img.shields.io/badge/Gitbook-chanul.gitbook.io/til-blue.svg?style=for-the-badge\&logo=gitbook)](https://chanul.gitbook.io/til/)

* 매일 배우고 기록한다.
* 새로운 개발 지식을 찾았다면 쌓아둔다.
* 쌓아둔 개발 지식은 이해하기 쉽게 정리한다.

<hr>

### 22.06.01 : c++ class

## class란

클래스는 공통적인 속성을 가진 여러 자료형의 변수와 함수를 묶는 사용자 정의 자료형이다.  
사물의 특성과 행동을 모델링하여 데이터화한다.  
구조체와 다르게 접근제어 지시자를 선언하지 않으면 private로 선언된다.  

선언지시자
- private : 클래스 내에서만 접근허용
- public : 어디서든 접근허용
- protected : 클래스 안, 상속관계에만 접근허용

```cpp
class 클래스이름
{
	// 변수(상태), 함수(행동) ...
};
```

```cpp
class person
{
private :
	char	*name;
	int		age;
public :
	// 내부 클래스 정의
	void	print_name(void)
	{
		std::cout << name << std::endl;
	}
	
	// 외부 클래스 정의
	void	print_age(void);
}

// 외부 클래스 정의
void	person::print_age(void)
{
	std::cout << age << std::endl;
}
```

### 범위 지정 연산자 (::)

범위 지정 연산자는 여러 범위에서 사용된 식별자를 식별하고 구분하는데 사용하는 연산자이다. 식별자로는 변수, 함수 또는 열거체가 올 수 있다.  
클래스에 이 연산자를 사용하면 네임스페이스 맴버를 식별하거나, 클래스의 정적 멤버를 호출할 수 있습니다. 

범위 지정 연산자 문법
- :: 식별자
- 클래스이름 :: 식별자
- 네임스페이스 :: 식별자
- 열거체 :: 식별자

## 클래스 생성자, 소멸자

클래스는 객체가 생성될 때마다 컴파일러에 의해 생성자 함수가 호출된다.  

1. 디폴트 생성자
2. 매개변수 생성자
3. 복사 생성자
4. 소멸자  

### 1. 디폴트 생성자  

내부적으로 알아서 생성되지만 사용자가 직접 정의할 수 있는 생성자이다. 함수 반환형이 아니며 함수 이름은 클래스 이름과 같다.

```cpp
class	Point
{
private :
	int	x;
	int	y;
public :
	// 디폴트 생성자
	Point()
	{
		x = 10;
		y = 20;
	}
}
```

### 2. 매개변수 생성자

매개변수 생성자를 통해서 객체 생성시 클래스 맴버 변수의 값을 초기화할 수 있다. 매개변수 생성자는 생성자 함수 매개변수로 값을 받아서 클래스 멤버 변수를 초기화한다. 매개변수 생성자 안의 this 키워드는 클래스 자신을 가리킨다.  

```cpp
class	Point
{
private :
	int	x;
	int	y;
public :
	// 디폴트 생성자
	Point()
	{
		x = 10;
		y = 20;
	}
	// 매개변수 생성자
	Point(int x, int y)
	{
		this->x = x;
		this->y = y;
	}
}
```

### 3. 복사 생성자

기존의 객체를 복사하여 새로운 객체를 생성한다. 컴파일어는 모든 클래스의 기본 복사 생성자를 제공한다.  

```cpp
// 디폴트 복사 생성자
int main(void)
{
	Point	a;
	Point	b = a;
}
```

### 4. 소멸자

소멸자는 객체의 사용이 끝날때 호출하는 함수이다. 기본으로 디폴트 소멸자가 생성된다. 보통 동적 할당된 메모리를 해제할 때 사용된다.  

```cpp
class	Point
{
private :
	int	x;
	int	y;
public :
	// 디폴트 생성자
	Point()
	{
		x = 10;
		y = 20;
	}
	// 매개변수 생성자
	Point(int x, int y)
	{
		this->x = x;
		this->y = y;
	}
	// 디폴트 소멸자
	~Point()
	{
		std::cout << "소멸자" << std::endl;
	}
}
```

## c++ iomanip

iomanip(Input Output Manipulator)는 입출력 조정자로 cout의 출력을 조정할 수 있다. 사용 함수는 다음과 같다.

<details>
<summary>setw : 출력 폭을 정한다. </summary>

다음 스트림 요소의 너비를 설정하며 지정하려는 각 요소 앞에 삽입해야 한다.  
setw가 설정하는 출력 폭은 최소 값이다. 그러므로 데이터가 지정한 폭보다 길다면 지정된 폭은 무시되고 데이터의 길이에 맞추어진다.  

```cpp
#include <iomanip>

T6 setw(streamsize Wide);

// Wide : 표시 필드의 너비
// 반환값 : 조작자는 스트림에서 추출하거나 스트림 str에 삽입할 때 호출 str.width(Wide)한 다음 반환하는 개체를 반환한다. 

```

```cpp
int	val = 12345;

std::cout << "val:" << val << std::endl;
std::cout << "val:" << setw(10) << val << std::endl;
std::cout << "val:" << setw(3) << val << std::endl;

// val:12345
// val:     12345
// val:12345
```
</details>

<details>
<summary>setfill : 공백을 채우는데 사용할 문자를 설정한다.</summary>

```cpp
#include <iomanip>

template <class Elem>
T4 setfill(Elem Ch);

// Ch : 오른쪽 맞춤된 디스플레이에서 공백을 채우는데 사용할 문자입니다.
// 반환값 : 조작자는 스트림에서 추출하거나 스트림 str에 삽입할 때 호출 str.fill(Ch)한 다음 반환하는 개체를 반환한다. 형식 Elem은 스트림 str의 요소 형식과 동일해야 한다.

```
</details>  


[클래스, 객체, 인스턴스 참조](https://valueelectronic.tistory.com/159)
