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


## cpp06 ex00 - Conversion of scalar types

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


 - [ ] input
   - [x] input string을 strtod() 함수를 호출하여 double로 변환한다. 
   - [x] 수에 대한 값 이외에 문자가 들어오면 잘못된 입력임으로 impossible
 - [x] impossible
   - [x] input string에 숫자 이외의 문자가 있는 경우
   - [x] 숫자가 없이 문자만 입력되는 경우

 - [ ] char
   - [x] overflow - impossible (input < 0 || input > 128)
   - [x] Non displayable (0 - 31)
   - [x] (32 - 127)은 문자 출력
   - [ ] nan - impossible
   - [ ] 
 - [ ] int
   - [ ] 범위를 벗어나면 overflow
   - [ ] nan이면 impossible이다.  
   - [ ] 
 - [ ] float
   - [x] 소수점이 없는 경우에 ".0"
   - [x] inff에 '+'
   - [ ] 
 - [ ] double
   - [x] 소수점이 없는 경우에 ".0"
   - [x] inff에 '+'
   - [ ] 




