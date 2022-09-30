---
layout: default
title: new
nav_order: 2
parent: c++ 
permalink: /docs/c++/new
---

* [new](#new)
	* [객체 생성 방법](#객체-생성-방법)

# new

## 객체 생성 방법
c++에서는 객체를 생성하는 방법이 두가지이다. 스택에 생성하는 방법과 힙에 생성하는 방법이 있는데 이들의 차이는 클래스의 생성자와 소멸자를 통해 알 수 있다.  

```cpp
class Number
{
	
private :
	int val;

public :
	Number( int val );
	~Number( void );
	void	print_val( void );
	
};

Number::Number( int val )
{
	this->val = val;
}

Number::~Number( void )
{
	std::cout << "destroyed: " << this->val << std::endl;
}

void	Number::print_val( void )
{
	std::cout << "val: " << this->val << std::endl;
}

Number	*test_heap( void )
{
	Number	*num;
	
	num = new Number(20);
	num->print_val();
	return (num);
}

void	test_stack( void )
{
	Number	num(10);
	
	num.print_val();
}

int	main( void )
{
	Number	*num_heap;

	num_heap = test_heap();
	test_stack();
	delete num_heap;
	return (0);
}

/* output

val: 20
val: 10
destroyed: 10
destroyed: 20

*/
```
- `test_stack` 함수에서 생성된 인스턴스는 함수가 종료되며 자동으로 해제된다.  
- `test_heap` 함수에서 `new` 키워드로 생성된 인스턴스는 `delete` 키워드로 해제하기 전까지 메모리에 유지된다.  

만약에 객체 배열을 동적 할당한다면 `delete []`로 해제한다.

```cpp
Number*	nums = new Number [5];
delete [] nums;
```

[객체 생성의 두가지 방법과 차이](https://yoon90.tistory.com/13)
[객체 배열과 동적 할당이란](https://blog.naver.com/PostView.nhn?blogId=kartmon&logNo=221503845077)
