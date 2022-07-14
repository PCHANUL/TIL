# Cpp

# cpp03 - 클래스의 상속

cpp module 03에서는 클래스 상속을 구현한다. 클래스 상속은 기존 클래스를 속성을 다른 클래스가 받고 의도에 맞게 수정하여 사용하거나 클래스 간의 종속 관계를 형성하여 객체를 조직화한다.  

### ex00

ClapTrap이라는 클래스를 구현한다. 앞으로 구현할 클래스들의 부모 클래스이다.  
이 클래스는 name, HP, EP, AD 속성을 priavte으로 가지며, attack, takeDamage, beRepaired라는 멤버 함수를 public으로 가진다.  

### ex01, ex02

ClapTrap 클래스를 상속받는 ScavTrap과 FragTrap 클래스를 만든다.  
상속받는 두 클래스를 구현하다보니 ClapTrap의 속성이 private로 구현되어 접근할 수 없었다. 그래서 getter와 setter를 ClapTrap에 구현하기로 한다. ClapTrap의 속성을 protect로 변경하여 접근을 허용하는 방법도 있다. 하지만 속성의 허용 범위를 두어야 하기 때문에 setter에서 검사 후 적용하는 방법을 사용하기로 한다. getter와 setter는 외부에서 접근하지 못하도록 protected로 설정한다.  

### ex03

ScavTrap과 FragTrap을 상속받는 DiamondTrap 클래스를 구현하는 과제이다. ScavTrap과 FragTrap은 앞에서 구현했듯이 모두 ClapTrap을 상속받고 있다. 그렇기 때문에 두 클래스를 상속받는 일은 문제를 발생시킨다. DiamondTrap이 ClapTrap의 속성이나 맴버 함수를 사용하고자 하면 ScavTrap의 ClapTrap을 참조하는지 FragTrap의 ClapTrap을 참조해야하는지 헷갈리기 때문이다. 이때 virtual 키워드를 사용하여 모호성을 해결한다. virtual 키워드는 객체에 대한 참조를 컴파일 시점이 아닌 런타임 시점으로 미룬다. 이를 각각 정적 바인딩과 동적 바인딩이라고 부른다. 객체가 호출되는 시점에 이동할 메모리 주소를 정적 바인딩은 컴파일 시점에 저장해주고, 동적 바인딩은 컴파일 시점에 메모리 공간을 확보한 다음 런타임 시점에 결정한다.  

# cpp04

### ex00 - override function

Animal 클래스를 상속받는 Cat 클래스와 Dog 클래스를 구현하는 과제이다. Cat과 Dog 클래스는 Animal의 makeSound 맴버 함수를 override하여 사용해야 한다. 그렇기 때문에 Animal 포인터 변수에 Cat이나 Dog 객체 포인터를 주고, makeSound 맴버 함수를 호출하면 Animal 클래스의 makeSound가 아니라 Cat과 Dog의 makeSound가 호출된다. 

### ex01 - deep copy

Brain 클래스를 구현하고, Cat과 Dog 클래스가 Brain 객체 포인터를 private으로 가지도록 한다. 이때 문제는 복사 생성자와 할당 연산자의 Brain 복사이다. brian이 할당되어 있다면 제거하고 복사하려는 객체가 가진 Brain과 똑같은 Brain을 생성해야 한다. 똑같은 Brain 객체를 생성하기 위해서 Brain의 복사 생성자를 사용했다. 

### ex02 - abstract class

이전에 구현한 Animal 클래스를 추상 클래스로 구현하는 과제이다. 추상 클래스에는 하나 이상의 순수 가상 함수가 존재해야 한다.  
순수 가상 함수란 함수의 정의가 이루어지지 않고 선언만 되어있는 함수이다. 그렇기 때문에 순수 가상 함수를 가진 클래스틑 객체로 생성되지 않고 상속에 사용된다. 상속받는 자식 클래스는 무조건 순수 가상 함수를 override 해야한다. 순수 가상 함수를 override 하지 않으면 순수 가상 함수가 남아있기 때문에 객체로 생성되지 않는다.  
이런 추상 클래스를 사용해야 하는 이유는 자식 클래스에서 반드시 선언되어야 하는 함수를 알려주기 위함이다. 만약에 가상 함수로 정의되어 있다면 자식 클래스에서 override 하지 않았어도 코드 상의 오류로 판단되지 않는다. 이런 실수를 방지할 수 있다. 

### ex03 - interface

추상 클래스와 인터페이스를 구현하여 프로그램을 구현하는 과제이다. 인터페이스는 순수 가상 함수로만 이루어진 클래스이다. 추상 클래스에는 맴버 변수와 맴버 함수를 자체적으로 가질 수 있지만 인터페이스는 순수 가상 함수로 이루어져 있다. 그렇기 때문에 자식 클래스가 사용하지 않는 부모 클래스의 데이터를 가지게 되는 추상 클래스보다 꼭 선언해야하는 함수를 정의한 인터페이스를 사용한다.  

AMateria 추상 클래스를 정의하고 자식 클래스로 Ice와 Cure를 정의한다. AMateria 클래스에서 순수 가상 함수로 선언되는 함수가 아니라면 자식 클래스에서 호출된 경우를 생각하여 함수를 정의해야 한다.  

ICharacter 인터페이스를 정의하고 Character 클래스를 구현한다. Character는 4개의 slot을 가지고 있고 Materia를 저장할 수 있다. 만약에 slot이 가득한 상태에서 새로운 Materia를 equip한다면 아무일도 일어나지 않고, 존재하지 않는 Materia 클래스를 equip하려는 상황도 마찬가지이다.  

Character 클래스는 반드시 deep copy 되어야 하고 복사 또는 제거될 때 Materia들을 반드시 delete 해야한다. Character에서 equip, unequip하는 Materia를 모두 가지고 있다가 나중에 delete 해야하기 때문에 리스트에 데이터를 가지고 있기로 한다.  
Materia가 생성되어 equip된다. 그런데 하나의 Materia가 여러 Charater에 equip되는 일은 부자연스럽기 때문에 Materia에는 is_equiped 변수가 있다. 이 변수로 Charater가 이미 장착된 Materia를 장착하지 않도록 한다.  
만약에 Character가 덮어씌워지거나 제거되는 경우에는 인벤토리에 있는 Materia를 delete 해야한다.
Materia를 자유롭게 delete하라고 명시되어 있으므로 나는 Character의 trash에 버리기로 한다. copy 또는 destroy 되는 경우에 trash는 clear 된다.
MateriaSource에서 모든 Materia를 관리하는 경우에는 문제가 발생할 수 있다. 만약에 MateriaSource가 Character보다 먼저 제거되는 경우에는 Character가 equip하고 있던 Materia가 모두 제거된다. 그러므로 Materia는 Character에서 관리해야 한다. MateriaSource는 Materia를 생성하는 역할만 한다.

# cpp05 - exception

cpp05는 std::exception 클래스를 상속받는 예외 객체를 재정의하고 예외 상황에서 사용해본다.  
예외 처리는 try~catch 구문과 throw로 구현된다. 예외가 발생할 수 있는 영역을 try로 감싸고, 이 영역에서 throw로 예외가 던져진다면 catch 영역에서 예외를 받는다. 예외 영역에서는 처리하려는 예외의 타입을 정할 수 있다. 이 과제에서는 catch 영역에서 (const std::exception &)를 받는다.

### ex00 - Bureaucrat

Bureaucrat라는 클래스를 정의하고 grade 값이 범위를 벗어난다면 예외가 발생하도록 구현한다.  
생성자에서는 name 문자열과 grade 정수를 받고, grade가 1보다 작거나 150보다 크다면 예외가 발생된다.  
예외가 발생되는 시점은 생성되는 시점과 increment, decrement 맴버함수를 호출하는 시점이다.
이때마다 grade 값을 확인하고 std::exception 클래스를 상속받은 객체를 throw한다. 

### ex01 - Form

Form 클래스는 이전에 구현한 Bureaucrat와 비슷하게 구현된다.  
생성되는 시점에 받는 sign_grade와 exec_grade를 범위에서 벗어나지 않는지 확인한다.  
구현되어야 하는 맴버함수는 beSigned()이다. 이 함수는 Bureaucrat를 받아서 sign 자격을 확인한다. 인자의 grade와 자신의 sign_grade를 비교하여 sign_grade가 더 작다면 sign 할 수 없다는 예외를 발생시킨다.  
추가로 이미 sign 되었음에도 시도하면 예외를 발생시키도록 구현하였다.  

### ex02 - Child forms

Form 클래스를 상속받는 자식 클래스를 구현한다.  
이 자식 클래스들은 정해진 sign_grade와 exec_grade로 생성되고 execute를 override 해야한다.  
Form 클래스에 execute 함수를 순수 가상 함수로 선언하여 더이상 객체로 생성될 수 없다.  
자식 클래스들의 execute 함수를 각자 주어진 동작을 실행하기 전에 grade를 확인한다.  
인자로 들어오는 Bureaucrat의 grade를 exec_grade와 비교하여 낮은 grade라면 예외를 발생시킨다.  



# cpp06 - type casting

c에서는 형변환을 가차없이 진행했지만 c++에서는 형변환을 위한 연산자를 제공하여 조금더 안정적인 형변환을 할 수 있도록 한다. 

 - static_cast : 기본 자료형 간의 형변환에 사용된다. 컴파일 단계에서 형변환을 검사하고 에러를 발생시킨다.
 - dynamic_cast : 상속 관계에서 안정적으로 형변환을 처리한다. 런타임 단계에서 형변환 검사를 진행한다. 
 - reinterpret_cast : 포인터/참조의 형변환 연산자이다. 변환을 강제하기 때문에 안전하지 않다.
 - const_cast : const를 제거하기 위해 사용된다.  

### ex00 - Conversion of scalar types

생성자에서 input을 double로 변환하여 저장한다.  
double을 다른 타입으로 타입 캐스팅하여 사용한다.  
가장 넓은 범위를 표현할 수 있는 double로 변환한다.  

input string을 double로 변환하기 위한 방법을 찾아야 한다.  
`stod()`는 c++11 함수이기 때문에 사용할 수 없다.  
c++98에서 사용할 수 있는 `strtod()` 함수를 사용하여 변환한다.  

```cpp
double  strtod(const char* str, char** str_end);

// str - null로 끝나는 변환될 문자열 포인터
// str_end - 숫자 문자열이 변환되고 남은 문자열의 첫번째 문자 포인터
// 성공시 - str의 내용에 해당되는 double 값
// 실패시 - 변환이 안되면 0이 반환되고, 범위를 초과한 경우에는 HUGE_VAL가 반환된다.
```

[cppreference (strtod)](https://en.cppreference.com/w/cpp/string/byte/strtof)
[소수점 처리](https://m.cplusplus.com/reference/ios/fixed/)


Conversion 클래스의 생성자는 인자로 (const char *argv) 를 받는다. argv는 변환되어야 하는 문자열로써 반드시 char, int, float 또는 double 타입이 문자열로 표현되어 들어온다. 그리고 char를 제외하고 모두 10진수 표현이다.  

 - char : non displayable 문자는 들어오지 않는다. 그리고 문자로 변환에서 표시할 수 없다면 적절한 메시지를 출력한다.  
 - float : ( -inff, +inff, nanf ) 세가지를 모두 처리해야 한다.
 - double : ( -inf, +inf, nan ) 세가지를 모두 처리해야 한다.

인자로 들어온 리터럴을 확인하고, 실제 세가지 타입으로 변환해야 한다. 만약에 변환이 말이 안되거나 오버플로우라면 impossible 메시지를 출력한다. 

float와 double이 출력되는 형식을 위해서 iomanip을 사용한다. 소수점을 1자리로 고정하려면 precision과 fixed를 사용하면 된다. precision은 출력되는 자리수를 정해주고, fixed는 정해진 자리수를 소수점에 한해서 적용되도록 한다.  


### ex01

객체 직렬화(Serialization)는 객체의 메모리를 연속적인 바이트로 만들고, 연속적인 바이트를 원래 객체로 복원하는 작업을 말한다. 주로 메모리에 있는 데이터를 스트림으로 보낼 때 사용한다. 스트림을 이용하면 객체를 파일에 입력이나 출력을 할 수 있으며, 네트워크에서 송수신할 수 있다. 

### ex02

dynamic_cast으로 상속 관계의 변환을 다룬다. dynamic_cast는 상속 관계에 있는 클래스의 형변환을 위해 사용된다. static_cast는 컴파일 시점에서 형변환이 가능한지 검사하지만 dynamic_cast는 런타임에 형변환이 가능한지 확인하게 된다. 그래서 dynamic_cast를 사용한 뒤에 정상적으로 형변환이 일어났는지 확인하는 과정이 필요하다. 형변환이 성공한 경우에는 정상적으로 변환된 주소의 값을 반환하지만 실패하는 경우는 두가지로 나누어 진다. 포인터를 변환하는 경우에 실패하면 NULL 포인터를 반환하고, 참조를 변환하는 경우에 실패하면 예외를 던진다. 변환이 성공하는 경우와 실패하는 경우를 구별하여 과제를 해결할 수 있다.  



# cpp07

함수 템플릿으로 함수를 구현하는 과제이다.

### ex00

swap, min, max 함수를 템플릿 함수로 구현하는 과제이다. 템플릿 함수로 구현되기 때문에 아무 타입이 들어와도 사용할 수 있는 함수가 된다.

### ex01

배열의 요소에 함수를 사용하는 함수인 iter를 구현한다. 인자로 배열, 배열 길이, 함수 포인터가 들어온다.  

예외처리 (배열, 배열 길이, 함수 포인터)
- 함수 포인터의 매개변수 타입이 배열 요소의 타입과 다르다면 컴파일 에러가 발생된다. 타입이 같거나 템플릿인 경우에는 컴파일된다.  
- 배열로 null이 들어오는 경우에는 컴파일 에러가 발생된다. 
- 배열 길이가 음수로 들어오는 경우에 segmentation fault가 뜬다.
- 함수 포인터의 경우에도 null이 들어올 수 없다. 컴파일러가 타입 검사를 진행하여 컴파일 에러를 발생시켜서 컴파일되지 않는다.  

배열의 길이에서 음수인 경우만 처리하면 된다. 만약에 오버플로우가 발생하는 값을 넣으면 컴파일러가 타입을 long으로 이해하고 매개변수로 받으려는 타입과 다름을 확인하고 에러를 발생시킨다.  


### ex02

템플릿 클래스 Array를 구현한다.  
- 인자가 없는 생성자는 빈배열을 생성
- unsigne int n 인자를 받는 생성자는 n 요소의 배열을 만든다.  
- 복사 생성자와 할당 연산자를 구현하는 경우에는 원본 배열이나 복사본을 수정해도 다른 배열에 영향이 없어야 한다.
- new[] 연산자를 사용할 때 반드시 메모리를 할당해야 한다. 메모리 사전 할당은 금지된다. 프로그램은 할당되지 않은 메모리에 접근해서는 안된다.  
- [] 연산자로 요소에 접근할 수 있어야 한다.
- [] 연산자로 범위를 벗어난 요소에 접근을 시도하는 경우에는 예외를 던진다.
- size() 맴버 함수는 배열의 요소 개수를 반환한다. 이 맴버 함수는 인자가 없으며 반드시 현재 인스턴스를 수정하지 않아야 한다.


- new 연산자

new 연산자는 메모리 공간 할당, 생성자 호출, 자료형에 맞게 반환된 주소 값이 형 변환한다. 그러므로 c처럼 반환되는 주소값의 형변환을 하지 않아도 된다.  

```cpp
void* operator new      (std::size_t count);
void* operator new[]    (std::size_t count);
```

- new 연산자 오버로딩

new 연산자를 오버로딩하려면 메모리 공간의 할당을 구현하면 된다. new 연산자를 사용하면 먼저 필요한 메모리 공간을 계산한다. 이때 operator new 함수를 호출하며 인자로 크기 값을 전달한다. 그래서 다음과 같이 operator new 함수를 정의하면 된다.  

```cpp
void* operator new (size_t size)
{
    void *ptr = new char[size];
    return ptr;
}
```

- delete 연산자 오버로딩



