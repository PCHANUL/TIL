# 연산자 오버로드 - operator overload

## 연산자 오버로드 규칙

- 새로운 연산자를 정의할 수 없다.
- 오버로드 된 연산자는 비정적 클래스 멤버 함수이거나 전역함수이어야 한다.
  - private 또는 protected 접근자의 전역 함수는 클래스의 friend로 선언해야 한다.
- 멤버 함수로 오버로드된 연산자의 첫번째 파라미터는 항상 연산자가 호출되는 객체의 클래스 형식이다.
  - 첫번째 파라미터에 대한 변환은 제공되지 않는다.  

- 연산자 오버로딩은 두가지이다.
   - 멤버 연산자 오버로딩: 클래스 내부에 있는 연산자 오버로딩이다. 왼쪽 피연산자는 자신(this), 오른쪽 피연산자는 매개변수
   - 전역 연산자 오버로딩: 전역 공간에 있는 연산자 오버로딩이다. 왼쪽 피연산자는 첫번째 매개변수, 오른쪽 피연산자는 두번째 매개변수

## 연산자 오버로딩 예시

```cpp
#include <iostream>

class Point
{
	
private :
	int	x;
	int	y;
public :
	Point(int x, int y);
	void	print( void );
	
	// + 연산자 오버로딩
	Point operator + (const Point& p);
};

Point::Point(int x, int y)
{
	this->x = x;
	this->y = y;
}

void	Point::print( void )
{
	std::cout << " x: " << this->x;
	std::cout << " y: " << this->y;
	std::cout << std::endl;
}

Point Point::operator + (const Point& p)
{
	Point result(
		this->x + p.x,
		this->y + p.y
	);
	return (result);
}

int	main( void )
{
	Point a(10, 20);
	Point b(3, 4);
	Point c = a + b;

	a.print();
	b.print();
	c.print();
}

// x: 10 y: 20
// x: 3 y: 4
// x: 13 y: 24

```  

[c++ 연산자 오버로딩](https://yeolco.tistory.com/119)  
[전역 연산자 오버로딩](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=dd1587&logNo=221102620758)  
