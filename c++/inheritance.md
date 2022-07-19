# 상속 - inheritance

c++에서는 클래스가 어떠한 클래스의 속성과 행동을 물려받는 상속이 있다. 클래스를 물려받으면 여러 클래스의 공통된 속성을 효율적으로 관리할 수 있다.  

클래스를 상속하려면 상속받을 클래스의 이름 옆에 접근 제한자와 상속할 클래스 이름을 붙여주면 된다. 접근 제한자로는 `public`, `private`, `protected`가 있다.

```cpp
class 부모클래스 
{
	// ...
}

class 자식클래스 : 접근제한자 부모클래스
{
	// ...
}
```

## 멤버 초기화

자식 클래스의 생성자는 부모 클래스의 멤버 변수까지 초기화해야한다.  

```cpp
#include <iostream>

class ClassName1
{
	private :
		int val;
	public :
		ClassName1(int _val) : val(_val) { }
		
		void print_val( void )
		{
			std::cout << val << std::endl;
		}
};

class ClassName2 : public ClassName1
{
	private :
		int id;
	public :
		ClassName2(int _id, int _val) : ClassName1(_val), id(_id) { }
		
		void print_info( void )
		{
			print_val();
			std::cout << id << std::endl;
		}
};
```

## 상속관계 접근 지정자

자식 클래스는 상속 접근 지정자에 따라서 부모 클래스에 접근할 수 있는 권한이 정해진다. 부모 클래스에 접근하는 경우는 두가지이다. 자식 클래스의 `정의부`와 `객체`에서 부모 클래스에 접근을 시도할 수 있으며 경우에 따라 접근 범위가 달라진다.  

| 부모 클래스 | 자식 클래스 정의부 |    자식 클래스 객체    |
| :---------: | :----------------: | :--------------------: |
|   public    |         O          | O (public 상속인 경우) |
|  protected  |         O          |           X            |
|   private   |         X          |           X            |

```cpp
class ParentClass
{
	private :
		int	a;
	public :
		int	b;
	protected :
		int	c;
};

class PublicChild : public ParentClass
{
	public :
		PublicChild(int _a, int _b, int _c)
		{
			a = _a;	// error: 'a' is a private member of 'ParentClass'
			b = _b;
			c = _c;
		}
};

class PrivateChild : private ParentClass
{
	public :
		PrivateChild(int _a, int _b, int _c)
		{
			a = _a;	// error: 'a' is a private member of 'ParentClass'
			b = _b;
			c = _c;
		}
};

class ProtectedChild : protected ParentClass
{
	public :
		ProtectedChild(int _a, int _b, int _c)
		{
			a = _a;	// error: 'a' is a private member of 'ParentClass'
			b = _b;
			c = _c;
		}
};

int	main( void )
{
	PublicChild		pub(1, 2, 3);
	PrivateChild	pri(1, 2, 3);
	ProtectedChild	pro(1, 2, 3);

	pub.a;	// error: 'a' is a private member of 'ParentClass'
	pub.b;
	pub.c;	// error: 'c' is a protected member of 'ParentClass'
	
	pri.a;	// error: 'a' is a private member of 'ParentClass'
	pri.b;	// error: 'b' is a private member of 'ParentClass'
	pri.c;	// error: 'c' is a private member of 'ParentClass'
	
	pro.a;	// error: 'a' is a private member of 'ParentClass'
	pro.b;	// error: 'b' is a protected member of 'ParentClass'
	pro.c;	// error: 'c' is a protected member of 'ParentClass'
	
	return (0);
}
```

[상속이란](https://blog.hexabrain.net/173)  
[상속 접근 지정자](https://thrillfighter.tistory.com/531)  



## 상속 관계에서 생성자

상속 관계에 있는 자식 객체를 생성하면 부모 클래스의 생성자부터 차례대로 생성자 호출이 일어난다. 그런데 만약에 부모 클래스의 생성자가 매개변수를 받는다면 자식 클래스 생성자에서 호출해야한다. 다음과 같은 방법으로 자식 클래스에서 부모 클래스의 생성자를 호출할 수 있다.  

```cpp
class Parent
{
	private :
		int	val;
	public :
		Parent( int val )
		{
			this->val = val;
		}
};

class Child : public Parent
{
	private :
		int	id;
	public :
		Child( int val, int id ) : Parent(val)	// 부모 클래스 생성자
		{
			this->id = id;
		}
};
```

## 상속 관계에서 대입 연산자

상속받는 자식 클래스가 대입 연산자를 새로 정의하면 부모 클래스의 대입 연산자가 디폴트로 생성된다. 만약에 자식 클래스에서 부모 클래스의 대입 연산자를 정의한다면 부모 클래스의 대입 연산자를 명시적으로 호출해야한다.  

```cpp
class Parent
{
	private :
		int	val;
	public :
		Parent( int val ) : val(val) { }
		Parent& operator = ( const Parent& origin )
		{
			this->val = origin.val;
			return (*this);
		}
};

class Child : public Parent
{
	public :
		Child( int val ) : Parent(val) { }
		Child& operator = ( const Parent &origin )
		{
			Parent::operator = (origin);
			return (*this);
		}
};
```

[상속 구조 대입 연산자](https://saelly.tistory.com/122)

## 상속 관계에서 메소드

부모 클래스에서 정의된 함수를 자식 클래스에서 재정의하는 상속 오버 라이딩을 할 수 있다.  

```cpp
#include <iostream>

class Parent
{
	private :
		int	val;
	public :
		Parent( int val ) : val(val) { }
		Parent& operator = ( const Parent& origin )
		{
			this->val = origin.val;
			return (*this);
		}
		
		void	print( void )
		{
			std::cout << this->val << std::endl;
		}
};

class Child : public Parent
{
	private :
		int	id;
	public :
		Child( int val ) : Parent(val), id(val) { }
		Child& operator = ( const Parent &origin )
		{
			Parent::operator = (origin);
			return (*this);
		}
		
		void	print( void )
		{
			std::cout << this->id << std::endl;
		}
};

int	main( void )
{
	Child	one(1, 2);
	
	one.print();	// 2
	return (0);
}

```

# virtual 키워드

virtual 키워드를 함수에 붙이면 오버라이딩이 가능해진다.  
오버라이딩을 해야하는 이유는 부모 클래스 포인터가 자식 클래스를 가리키는 경우에 부모가 자식을 원할하게 사용하기 위함이다.  

```cpp

class A
{
	// ...
};

class B : public A
{
	// ...
};

class C : public B
{
	// ...
}

int	main( void )
{
	A *ptr = C();	// 부모 포인터가 자식을 가리키는 경우
	
	return (0);
}

```

부모가 자식을 가리켜서 새롭게 정의된 자식의 기능을 사용할 수 있다면 불필요함을 줄일 수 있다.  
이를 다형성이라고 하며, 오버라이딩을 성립시켜주는 `virtual` 키워드로 구현된다.  
`virtual` 키워드가 붙은 함수는 `동적 바인딩`되기 때문에 오버라이딩된다. 

## 정적 바인딩과 동적 바인딩

정적 바인딩은 컴파일 시점에 데이터 타입을 기준으로 실행될 함수를 정하지만  
동적 바인딩은 런타임 시점에 객체 타입을 기준으로 실행될 함수를 정한다.  

함수는 컴파일 시점에 코드가 메모리에 저장되고, 함수를 호출하는 부분에 함수가 저장된 메모리 주소값이 저장된다.  
바인딩으로 함수를 호출하는 부분에 함수가 위치한 메모리 주소로 연결시켜줄 수 있다.  

```
프로그램 실행 -> 함수 호출 -> 함수가 저장된 주소로 점프 -> 함수 실행 -> 원래 위치로
```

정적 바인딩과 반대로 동적 바인딩은 실행 파일을 생성하는 시점에 바인딩하지 않고 보류 상태로 둔다.  
점프할 메모리를 저장하기 위해 따로 메모리 공간을 가지고 있는다.  

[정적 바인딩, 동적 바인딩](https://secretroute.tistory.com/entry/140819)  

## virtual 소멸자 함수

부모 클래스의 포인터로 자식 클래스를 호출할 때 가상 함수로 정의되지 않은 자식 클래스의 함수를 호출하면 부모 클래스의 멤버 함수가 호출된다.
소멸자도 자식 클래스의 소멸자가 아닌 부모 클래스의 소멸자가 호출된다.  
가상 함수로 소멸자가 사용되었다면 자식 클래스에서 재정의 될 수 있음을 명시하기 때문에 자식 클래스의 소멸자부터 차례대로 부모 클래스의 소멸자가 호출된다.  

<details>
<summary>virtual 키워드가 없는 경우</summary>

```cpp
#include <iostream>

class A
{
	public :
    	~A( void )
    	{
    		std::cout << "destroyed A" << std::endl;
    	}
};

class B : public A
{
	public :
		~B( void )
		{
			std::cout << "destroyed B" << std::endl;
		}
};

class C : public B
{
	public :
		~C( void )
		{
			std::cout << "destroyed C" << std::endl;
		}
};

int	main( void )
{
	A *a = new C();
	B *b = new C();
	C *c = new C();
	
	delete a;
	std::cout << std::endl;
	delete b;
	std::cout << std::endl;
	delete c;
}

/* prints 

destroyed A

destroyed B
destroyed A

destroyed C
destroyed B
destroyed A

*/

```
</details>


<details>
<summary>virtual 키워드가 있는 경우</summary>

```cpp
#include <iostream>

class A
{
	public :
    	virtual ~A( void )
    	{
    		std::cout << "destroyed A" << std::endl;
    	}
};

class B : public A
{
	public :
		~B( void )
		{
			std::cout << "destroyed B" << std::endl;
		}
};

class C : public B
{
	public :
		~C( void )
		{
			std::cout << "destroyed C" << std::endl;
		}
};

int	main( void )
{
	A *a = new C();
	B *b = new C();
	C *c = new C();
	
	delete a;
	std::cout << std::endl;
	delete b;
	std::cout << std::endl;
	delete c;
}

/* prints 

destroyed C
destroyed B
destroyed A

destroyed C
destroyed B
destroyed A

destroyed C
destroyed B
destroyed A

*/

```
</details>

[부모 클래스 소멸자에 virtual을 사용하는 이유](https://younggwan.tistory.com/45)  

 22.06.15 : 다중 상속, 가상 상속, 다이아몬드 상속 문제

## 다중 상속

c++ 클래스는 2개 이상의 부모 클래스를 상속받을 수 있다.

```cpp
class A
{
	// ...
};

class B
{
	// ...
};

class C : public A, public B
{
	// ...
};
```

다중 상속은 다이아몬드 형태의 상속 구조를 만들 수 있다. 이러한 상속 계층에서 클래스의 다중 인스턴스를 방지하기 위해서 가상 상속 키워드인 `virtual`을 사용한다.  
아래의 왼쪽 그림을 보면 `class X`가 `class C`에 두번 상속된다. `class A`와 `class B`의 부모 클래스로 `class X`가 중복되어 상속되면 `class C`가 객체에 의해 접근될때 어떤 데이터나 함수가 호출되어야하는지에 대한 모호성이 발생된다. 모호성은 컴파일러를 혼동시키기 때문에 오류를 표시하고 이를 피해기 위해서 `virtual` 키워드를 붙여서 상속받는다.  


![](src/inherit_01.png)

```cpp
class X
{
	// ...
}

class A : virtual public X
{
	// ...
};

class B : virtual public X
{
	// ...
};

class C : public A, public B
{
	// ...
};
```

[가상 상속으로 다이아몬드 상속 문제 해결](https://ansohxxn.github.io/cpp/chapter12-8/)
