# Cpp

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




