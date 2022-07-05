# Cpp

## cpp03 - 클래스의 상속

cpp module 03에서는 클래스 상속을 구현한다. 클래스 상속은 기존 클래스를 속성을 다른 클래스가 받고 의도에 맞게 수정하여 사용하거나 클래스 간의 종속 관계를 형성하여 객체를 조직화한다.  

### ex00

ClapTrap이라는 클래스를 구현한다. 앞으로 구현할 클래스들의 부모 클래스이다.  
이 클래스는 name, HP, EP, AD 속성을 priavte으로 가지며, attack, takeDamage, beRepaired라는 멤버 함수를 public으로 가진다.  

### ex01, ex02

ClapTrap 클래스를 상속받는 ScavTrap과 FragTrap 클래스를 만든다.  
상속받는 두 클래스를 구현하다보니 ClapTrap의 속성이 private로 구현되어 접근할 수 없었다. 그래서 getter와 setter를 ClapTrap에 구현하기로 한다. ClapTrap의 속성을 protect로 변경하여 접근을 허용하는 방법도 있다. 하지만 속성의 허용 범위를 두어야 하기 때문에 setter에서 검사 후 적용하는 방법을 사용하기로 한다. getter와 setter는 외부에서 접근하지 못하도록 protected로 설정한다.  


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




