# Today I Learned

[![Gitbook](https://img.shields.io/badge/Gitbook-chanul.gitbook.io/til-blue.svg?style=for-the-badge\&logo=gitbook)](https://chanul.gitbook.io/til/)

* 매일 배우고 기록한다.
* 새로운 개발 지식을 찾았다면 쌓아둔다.
* 쌓아둔 개발 지식은 이해하기 쉽게 정리한다.

<hr>

### 22.06.03 : c++ 소멸자

소멸자란 개체가 범위를 벗어나거나 호출에 의해 명시적으로 제거될 때 자동으로 호출되는 맴버 함수이다. 소멸자의 이름은 클래스와 같고, 앞에 `~`가 붙는다.  

예제 코드에서 `String::~String` 소멸자는 `delete`를 사용하여 동적 할당된 공간의 할당을 취소한다.

```cpp
// spec1_destructors.cpp
#include <string>

class String {
public:
   String( char *ch );  // Declare constructor
   ~String();           //  and destructor.
private:
   char    *_text;
   size_t  sizeOfText;
};

// Define the constructor.
String::String( char *ch ) {
   sizeOfText = strlen( ch ) + 1;

   // Dynamically allocate the correct amount of memory.
   _text = new char[ sizeOfText ];

   // If the allocation succeeds, copy the initialization string.
   if( _text )
      strcpy_s( _text, sizeOfText, ch );
}

// Define the destructor.
String::~String() {
   // Deallocate the memory that was previously reserved
   //  for this string.
   delete[] _text;
}

int main() {
   String str("The piper in the glen...");
}
```

## 소멸자 선언

- 클래스와 이름이 같으며 물결표(~)가 앞에 붙는다.
- 인수를 받아들이지 않는다.
- 값(또는 void)를 반환하지 않아야 한다.

## 소멸자 사용

소멸자는 클래스 맴버 함수를 자유롭게 호출하고 클래스 맴버 데이터에 접근할 수 있다.  
다음 이벤트 중 하나가 발생하면 소멸자가 호출된다.  

- 블록 스코프에 지역 개체가 있는 상태에서 블록 스코프를 벗어난 경우
- `new`키워드로 할당된 개체가 `delete`키워드로 제거된 경우
- 임시 개체의 수명이 종료된 경우
- 프로그램이 종료되고 전역 또는 정적 개체가 있는 경우
- 소멸자는 소멸자 함수의 정규화된 이름을 사용하여 명시적으로 호출된다.  

소멸자 사용에 두가지 제한 사항이 있다.

- 주소를 가져갈 수 없다.
- 파생 클래스는 기본 클래스의 소멸자를 상속하지 않는다.

## 파괴 순서

개체가 범위를 벗어나거나 삭제된 경우 개체가 완전히 소멸되는 이벤트 순서는 다음과 같다.

1. 클래스의 소멸자가 호출되고 소멸자 함수의 본문이 실행된다.
2. 비정적 멤버 개체의 소멸자는 클래스 선언에 나타나는 역순으로 호출된다. 이러한 멤버의 생성에 사용되는 선택적 멤버 초기화 목록은 생성 또는 소멸 순서에 영향을 주지 않는다.
3. 비가상 기본 클래스의 소멸자는 선언의 역순으로 호출된다.
4. 가상 기본 클래스의 소멸자는 선언의 역순으로 호출된다.

```cpp
// order_of_destruction.cpp
#include <cstdio>

struct A1      { virtual ~A1() { printf("A1 dtor\n"); } };
struct A2 : A1 { virtual ~A2() { printf("A2 dtor\n"); } };
struct A3 : A2 { virtual ~A3() { printf("A3 dtor\n"); } };

struct B1      { ~B1() { printf("B1 dtor\n"); } };
struct B2 : B1 { ~B2() { printf("B2 dtor\n"); } };
struct B3 : B2 { ~B3() { printf("B3 dtor\n"); } };

int main() {
   A1 * a = new A3;
   delete a;
   printf("\n");

   B1 * b = new B3;
   delete b;
   printf("\n");

   B3 * b2 = new B3;
   delete b2;
}

Output: A3 dtor
A2 dtor
A1 dtor

B1 dtor

B3 dtor
B2 dtor
B1 dtor
```

[c++ 소멸자 참조](https://docs.microsoft.com/en-us/cpp/cpp/destructors-cpp?view=msvc-170)

### 22.06.02 : c++ timestamp, string

# c++ const

`const` 키워드는 값을 상수로 선언하여 변경할 수 없도록 한다. 한번 설정된 값이 read-only 메모리에 올라가게되어 변경하지 못하고 계속 유지하게 된다.

```cpp
const int	val = 0;
val = 1; // error!
```

## const 포인터

상수가 가리키는 공간을 수정하지 못하게 한다. `const`가 `int`에 적용되었기 때문에 포인터가 가리키고 있는 값을 변경할 수 없지만, 주소값은 변경할 수 있다.

```cpp
int			val = 0;
const int	*ptr = &val;

val = 1;	// ok
*ptr = 2;	// error
```

반대로 다음의 경우에는 주소값을 변경하지 못하게 한다. `const`가 포인터에 적용되어 주소값을 변경할 수 없지만, 포인터가 가리키고 있는 값을 변경할 수 있다.

```cpp
int			val_1 = 0;
int			val_2 = 0;
int* const	ptr = &val_1;

val_1 = 1;	// ok
*ptr = 2;	// ok
ptr = &val_2;	// error
``` 


[c++ const 이해하기](https://dydtjr1128.github.io/cpp/2020/01/08/Cpp-const.html)
[static 맴버와 const 멤버](http://www.parkjonghyuk.net/lecture/program2/chap06.pdf)

# c++ static

[static의 클래스, 맴버 함수, 객체, 함수 예제](https://wookiist.dev/71)

# timestamp

[현재 날짜와 현재 시간 출력](https://wookiist.dev/71)

# c++ string 클래스 맴버 함수

## 요소 접근 (Element access)

<details>
<summary>operator[] : 해당 위치의 문자 참조를 반환</summary>

```cpp
char& operator[] (size_t pos);

// pos : 문자의 위치
// 반환 : 문자열의 지정된 위치에 있는 문자
```

</details>  

<details>
<summary>at : 해당 위치의 문자 참조를 반환</summary>

```cpp
char& at (size_t pos);

// pos : 문자의 위치, 문자의 위치가 아니면 out_of_range 예외가 발생
// 반환 : 문자열의 지정된 위치에 있는 문자
```

</details>  

<details>
<summary>back : 마지막 위치의 문자 참조를 반환</summary>

```cpp
char& back ();

// 반환 : 문자열의 마지막 위치에 있는 문자
```

</details>  

<details>
<summary>front : 첫번째 위치의 문자 참조를 반환</summary>

```cpp
char& front ();

// 반환 : 문자열의 첫번째 위치에 있는 문자
```

</details>  


## 수정자 (Modifiers)

<details>
<summary>operator+= : 문자열에 추가</summary>

```cpp
string& operator+= (const string& str); // string (1)
string& operator+= (const char* s); // c-string(2)
string& operator+= (char c); // character (3)

// str : 복사되어 마지막에 붙는 문자열
// s : 복사되어 마지막에 붙는 null로 끝나는 문자 시퀀스 포인터
// c : 문자열의 값에 추가되는 문자
// 반환 : *this
```

</details>  

<details>
<summary>append : 문자열에 추가</summary>

```cpp
// string (1)	
string& append (const string& str);

// substring (2)	
string& append (const string& str, size_t subpos, size_t sublen);

// c-string (3)	
string& append (const char* s);

// buffer (4)	
string& append (const char* s, size_t n);

// fill (5)	
string& append (size_t n, char c);

// range (6)	
template <class InputIterator>
   string& append (InputIterator first, InputIterator last);

// str : 추가하려는 다른 문자열 
// subpos : 복사되는 부분 문자열의 첫번째 문자 위치
// sublen : 복사할 부분 문자열의 길이
// s : 문자 배열의 포인터
// n : 복사하려는 문자의 개수
// c : 문자 값, n번 반복
// first, last : Input iterators에서 첫번째와 마지막 위치
// il : initializer_list 객체
// 반환 : *this

```

## Example

```cpp
// appending to string
#include <iostream>
#include <string>

int main ()
{
  std::string str;
  std::string str2="Writing ";
  std::string str3="print 10 and then 5 more";

  // used in the same order as described above:
  str.append(str2);                       // "Writing "
  str.append(str3,6,3);                   // "10 "
  str.append("dots are cool",5);          // "dots "
  str.append("here: ");                   // "here: "
  str.append(10u,'.');                    // ".........."
  str.append(str3.begin()+8,str3.end());  // " and then 5 more"
  str.append<int>(5,0x2E);                // "....."

  std::cout << str << '\n';
  return 0;
}

/* output

Writing 10 dots here: .......... and then 5 more.....

*/
```

</details>  

<details>
<summary>push_back : 문자열 마지막에 문자를 추가</summary>

```cpp
void push_back (char c);

// c : 문자열에 추가되는 문자
// 반환 : 없음
```

</details>

</details>  

<details>
<summary>assign : 문자열에 새로운 값을 할당</summary>

```cpp
// string (1)	
string& assign (const string& str);

// substring (2)	
string& assign (const string& str, size_t subpos, size_t sublen);

// c-string (3)	
string& assign (const char* s);

// buffer (4)	
string& assign (const char* s, size_t n);

// fill (5)	
string& assign (size_t n, char c);

// range (6)	
template <class InputIterator>
   string& assign (InputIterator first, InputIterator last);

// str : 추가하려는 다른 문자열 
// subpos : 복사되는 부분 문자열의 첫번째 문자 위치
// sublen : 복사할 부분 문자열의 길이
// s : 문자 배열의 포인터
// n : 복사하려는 문자의 개수
// c : 문자 값, n번 반복
// first, last : Input iterators에서 첫번째와 마지막 위치
// il : initializer_list 객체
// 반환 : *this
```

## Example  

```cpp
// string::assign
#include <iostream>
#include <string>

int main ()
{
  std::string str;
  std::string base="The quick brown fox jumps over a lazy dog.";

  // used in the same order as described above:

  str.assign(base);
  std::cout << str << '\n';

  str.assign(base,10,9);
  std::cout << str << '\n';         // "brown fox"

  str.assign("pangrams are cool",7);
  std::cout << str << '\n';         // "pangram"

  str.assign("c-string");
  std::cout << str << '\n';         // "c-string"

  str.assign(10,'*');
  std::cout << str << '\n';         // "**********"

  str.assign<int>(10,0x2D);
  std::cout << str << '\n';         // "----------"

  str.assign(base.begin()+16,base.end()-12);
  std::cout << str << '\n';         // "fox jumps over"

  return 0;
}

/* output

The quick brown fox jumps over a lazy dog.
brown fox
pangram
c-string
**********
----------
fox jumps over

*/
```
</details>

<details>
<summary>insert : 문자열에서 지정하는 위치 앞에 문자를 삽입</summary>

```cpp
// string (1)	
 string& insert (size_t pos, const string& str);

// substring (2)	
 string& insert (size_t pos, const string& str, size_t subpos, size_t sublen);

// c-string (3)	
 string& insert (size_t pos, const char* s);

// buffer (4)	
 string& insert (size_t pos, const char* s, size_t n);

// fill (5)	
 string& insert (size_t pos, size_t n, char c);
    void insert (iterator p, size_t n, char c);

// single character (6)	
iterator insert (iterator p, char c);

// range (7)	
template <class InputIterator>
   void insert (iterator p, InputIterator first, InputIterator last);
   
// str : 추가하려는 다른 문자열 
// subpos : 복사되는 부분 문자열의 첫번째 문자 위치
// sublen : 복사할 부분 문자열의 길이
// s : 문자 배열의 포인터
// n : 복사하려는 문자의 개수
// c : 문자 값, n번 반복
// first, last : Input iterators에서 첫번째와 마지막 위치
// il : initializer_list 객체
// 반환 : 문자열 참조를 반환하는 경우에는 *this, 
// 반복자를 반환하는 경우에는 삽입된 첫번째 문자를 가리키는 반복자
```

## Example

```cpp
// inserting into a string
#include <iostream>
#include <string>

int main ()
{
  std::string str="to be question";
  std::string str2="the ";
  std::string str3="or not to be";
  std::string::iterator it;

  // used in the same order as described above:
  str.insert(6,str2);                 // to be (the )question
  str.insert(6,str3,3,4);             // to be (not )the question
  str.insert(10,"that is cool",8);    // to be not (that is )the question
  str.insert(10,"to be ");            // to be not (to be )that is the question
  str.insert(15,1,':');               // to be not to be(:) that is the question
  it = str.insert(str.begin()+5,','); // to be(,) not to be: that is the question
  str.insert (str.end(),3,'.');       // to be, not to be: that is the question(...)
  str.insert (it+2,str3.begin(),str3.begin()+3); // (or )

  std::cout << str << '\n';
  return 0;
}

/* output

to be, or not to be: that is the question...

*/
```

</details>

<details>
<summary>insert : 문자열에서 지정하는 위치 앞에 문자를 삽입</summary>

```cpp
// sequence (1)	
 string& erase (size_t pos = 0, size_t len = npos);

// character (2)	
iterator erase (iterator p);

// range (3)	
     iterator erase (iterator first, iterator last);
	 
// pos : 지우기 시작하는 위치, 문자열 길이를 초과하면 out_of_range
// len : 지우려는 문자의 개수
// p : 제거될 문자 반복자
// first, last : 제거할 문자열 내의 범위를 지정하는 반복자
```

</details>

<details>
<summary>insert : 문자열에서 지정하는 위치 앞에 문자를 삽입</summary>

```cpp
// string (1)	
string& replace (size_t pos,  size_t len,  const string& str);
string& replace (iterator i1, iterator i2, const string& str);

// substring (2)	
string& replace (size_t pos,  size_t len,  const string& str,
                 size_t subpos, size_t sublen);

// c-string (3)	
string& replace (size_t pos,  size_t len,  const char* s);
string& replace (iterator i1, iterator i2, const char* s);

// buffer (4)	
string& replace (size_t pos,  size_t len,  const char* s, size_t n);
string& replace (iterator i1, iterator i2, const char* s, size_t n);

// fill (5)	
string& replace (size_t pos,  size_t len,  size_t n, char c);
string& replace (iterator i1, iterator i2, size_t n, char c);

// range (6)	
template <class InputIterator>
  string& replace (iterator i1, iterator i2,
                   InputIterator first, InputIterator last);
				   
// str : 복사될 다른 문자열 
// pos : 변경될 문자의 위치
// len : 변경될 문자의 개수
// subpos : 복사되는 부분 문자열의 첫번째 문자 위치
// sublen : 복사할 부분 문자열의 길이
// s : 문자 배열의 포인터
// n : 복사하려는 문자의 개수
// c : 문자 값, n번 반복
// first, last : Input iterators에서 첫번째와 마지막 위치
// il : initializer_list 객체
// 반환 : *this
```

## Example

```cpp
// replacing in a string
#include <iostream>
#include <string>

int main ()
{
  std::string base="this is a test string.";
  std::string str2="n example";
  std::string str3="sample phrase";
  std::string str4="useful.";

  // replace signatures used in the same order as described above:

  // Using positions:                 0123456789*123456789*12345
  std::string str=base;           // "this is a test string."
  str.replace(9,5,str2);          // "this is an example string." (1)
  str.replace(19,6,str3,7,6);     // "this is an example phrase." (2)
  str.replace(8,10,"just a");     // "this is just a phrase."     (3)
  str.replace(8,6,"a shorty",7);  // "this is a short phrase."    (4)
  str.replace(22,1,3,'!');        // "this is a short phrase!!!"  (5)

  // Using iterators:                                               0123456789*123456789*
  str.replace(str.begin(),str.end()-3,str3);                    // "sample phrase!!!"      (1)
  str.replace(str.begin(),str.begin()+6,"replace");             // "replace phrase!!!"     (3)
  str.replace(str.begin()+8,str.begin()+14,"is coolness",7);    // "replace is cool!!!"    (4)
  str.replace(str.begin()+12,str.end()-4,4,'o');                // "replace is cooool!!!"  (5)
  str.replace(str.begin()+11,str.end(),str4.begin(),str4.end());// "replace is useful."    (6)
  std::cout << str << '\n';
  return 0;
}

/* output

replace is useful.

*/
```

</details>

<details>
<summary>swap : 문자열의 내용을 다른 문자열과 교환한다.</summary>

```cpp
void swap (string& str);

// str : 교환할 다른 문자열
// 반환 : 없음
```

## Example

```cpp
// swap strings
#include <iostream>
#include <string>

main ()
{
  std::string buyer ("money");
  std::string seller ("goods");

  std::cout << "Before the swap, buyer has " << buyer;
  std::cout << " and seller has " << seller << '\n';

  seller.swap (buyer);

  std::cout << " After the swap, buyer has " << buyer;
  std::cout << " and seller has " << seller << '\n';

  return 0;
}

/* output 

Before the swap, buyer has money and seller has goods
 After the swap, buyer has goods and seller has money

*/
```

</details>

<details>
<summary>pop_back : 문자열의 마지막을 지운다</summary>

```cpp
void pop_back();

// 반환 : 없음
```

## Example

```cpp
// string::pop_back
#include <iostream>
#include <string>

int main ()
{
  std::string str ("hello world!");
  str.pop_back();
  std::cout << str << '\n';
  return 0;
}

/* output 

hello world

*/
```

</details>

## 반복자 (Iterators)

<details>
<summary>begin : 문자열의 첫번째 문자를 가리키는 반복자를 반환</summary>

```cpp
iterator begin();

// 반환 : 문자열의 첫번째 문자를 가리키는 반복자
```

## Example

```cpp
// string::begin/end
#include <iostream>
#include <string>

int main ()
{
  std::string str ("Test string");
  for ( std::string::iterator it=str.begin(); it!=str.end(); ++it)
    std::cout << *it;
  std::cout << '\n';

  return 0;
}

/* output 

Test string

*/
```

</details>

<details>
<summary>end : 문자열의 마지막 문자를 가리키는 반복자를 반환</summary>

```cpp
iterator end();

// 반환 : 문자열의 마지막 문자를 가리키는 반복자
```

## Example

```cpp
// string::begin/end
#include <iostream>
#include <string>

int main ()
{
  std::string str ("Test string");
  for ( std::string::iterator it=str.begin(); it!=str.end(); ++it)
    std::cout << *it;
  std::cout << '\n';

  return 0;
}

/* output 

Test string

*/
```

</details>

## 수용력 (Capacity)

<details>
<summary>size & length : 문자열 길이</summary>

string::size와 string::length는 정확히 동일한 값을 반환한다.

```cpp
size_t size() const;
size_t length() const;

// 반환 : 문자열 바이트 수
```

## Example

```cpp
// string::size
#include <iostream>
#include <string>

int main ()
{
  std::string str ("Test string");
  std::cout << "The size of str is " << str.size() << " bytes.\n";
  return 0;
}

/* output 
The size of str is 11 bytes
*/
```

</details>

<details>
<summary>resize : 문자열 길이 재설정</summary>

문자열의 길이를 변경한다.  
n이 현재 문자열 길이보다 작으면 n번째 이후의 문자는 제거되며 길이가 줄어든다.  
n이 현재 문자열 길이보다 크다면 필요한 만큼 문자를 삽입하여 내용을 확장한다.  
c가 지정되었으면 c가 복사되고, 아니라면 null이 복사된다.

```cpp
void resize (size_t n);
void resize (size_t n, char c);

// n : 새 문자열 길이
// c : 새로운 공간에 넣을 문자
// 반환 : 없음
```

</details>

<details>
<summary>empty : 문자열이 비었는지 반환</summary>

```cpp
bool empty() const;

// 반환 : 문자열 길이가 0이라면 true, 아니라면 false
```

</details>

## String operations

<details>
<summary>substr : 부분 문자열 생성</summary>

```cpp
string substr (size_t pos = 0, size_t len = npos) const;

// pos : 부분 문자열로 복제될 첫번째 문자 위치, 
// 문자열 길이과 같다면 함수는 빈 문자열을 반환하고, 
// 문자열 길이보다 크다면 out_of_range
// len : 부분 문자열에 포함될 문자의 개수
// 반환 : 부분 문자열인 문자열
```

## Example

```cpp
// string::substr
#include <iostream>
#include <string>

int main ()
{
  std::string str="We think in generalities, but we live in details.";
                                           // (quoting Alfred N. Whitehead)

  std::string str2 = str.substr (3,5);     // "think"

  std::size_t pos = str.find("live");      // position of "live" in str

  std::string str3 = str.substr (pos);     // get from "live" to the end

  std::cout << str2 << ' ' << str3 << '\n';

  return 0;
}

/* output 
think live in details.
*/
```

</details>

<details>
<summary>compare : 문자열을 비교</summary>

```cpp
// string (1)	
int compare (const string& str) const;

// substrings (2)	
int compare (size_t pos, size_t len, const string& str) const;
int compare (size_t pos, size_t len, const string& str,
             size_t subpos, size_t sublen) const;

// c-string (3)	
int compare (const char* s) const;
int compare (size_t pos, size_t len, const char* s) const;

// buffer (4)	
int compare (size_t pos, size_t len, const char* s, size_t n) const;

// str : 비교하려는 다른 문자열
// pos : 비교를 시작하는 첫번째 문자열 위치
// len : 비교하는 문자열 길이
// subpos, sublen : 위의 pos, len과 같음
// s : 문자 배열의 포인터
// n : 비교하는 문자 개수
// 반환 : 문자열 간의 관계를 나타내는 부호있는 정수
```

## Example

```cpp
// comparing apples with apples
#include <iostream>
#include <string>

int main ()
{
  std::string str1 ("green apple");
  std::string str2 ("red apple");

  if (str1.compare(str2) != 0)
    std::cout << str1 << " is not " << str2 << '\n';

  if (str1.compare(6,5,"apple") == 0)
    std::cout << "still, " << str1 << " is an apple\n";

  if (str2.compare(str2.size()-5,5,"apple") == 0)
    std::cout << "and " << str2 << " is also an apple\n";

  if (str1.compare(6,5,str2,4,5) == 0)
    std::cout << "therefore, both are apples\n";

  return 0;
}

/* output 
green apple is not red apple
still, green apple is an apple
and red apple is also an apple
therefore, both are apples
*/
```

</details>


[c++ string](https://www.cplusplus.com/reference/string/string/)
[c++ string 클래스에 대해서 (총정리)](https://blockdmask.tistory.com/338)

### 22.06.01 : c++ class
> 
> ## class란
> 
> 클래스는 공통적인 속성을 가진 여러 자료형의 변수와 함수를 묶는 사용자 정의 자료형이다.  
> 사물의 특성과 행동을 모델링하여 데이터화한다.  
> 구조체와 다르게 접근제어 지시자를 선언하지 않으면 private로 선언된다.  
> 
> 선언지시자
> - private : 클래스 내에서만 접근허용
> - public : 어디서든 접근허용
> - protected : 클래스 안, 상속관계에만 접근허용
> 
> ```cpp
> class 클래스이름
> {
> 	// 변수(상태), 함수(행동) ...
> };
> ```
> 
> ```cpp
> class person
> {
> private :
> 	char	*name;
> 	int		age;
> public :
> 	// 내부 클래스 정의
> 	void	print_name(void)
> 	{
> 		std::cout << name << std::endl;
> 	}
> 	
> 	// 외부 클래스 정의
> 	void	print_age(void);
> }
> 
> // 외부 클래스 정의
> void	person::print_age(void)
> {
> 	std::cout << age << std::endl;
> }
> ```
> 
> ### 범위 지정 연산자 (::)
> 
> 범위 지정 연산자는 여러 범위에서 사용된 식별자를 식별하고 구분하는데 사용하는 연산자이다. 식별자로는 변수, 함수 또는 열거체가 올 수 있다.  
> 클래스에 이 연산자를 사용하면 네임스페이스 맴버를 식별하거나, 클래스의 정적 멤버를 호출할 수 있습니다. 
> 
> 범위 지정 연산자 문법
> - :: 식별자
> - 클래스이름 :: 식별자
> - 네임스페이스 :: 식별자
> - 열거체 :: 식별자
> 
> ## 클래스 생성자, 소멸자
> 
> 클래스는 객체가 생성될 때마다 컴파일러에 의해 생성자 함수가 호출된다.  
> 
> 1. 디폴트 생성자
> 2. 매개변수 생성자
> 3. 복사 생성자
> 4. 소멸자  
> 
> ### 1. 디폴트 생성자  
> 
> 내부적으로 알아서 생성되지만 사용자가 직접 정의할 수 있는 생성자이다. 함수 반환형이 아니며 함수 이름은 클래스 이름과 같다.
> 
> ```cpp
> class	Point
> {
> private :
> 	int	x;
> 	int	y;
> public :
> 	// 디폴트 생성자
> 	Point()
> 	{
> 		x = 10;
> 		y = 20;
> 	}
> }
> ```
> 
> ### 2. 매개변수 생성자
> 
> 매개변수 생성자를 통해서 객체 생성시 클래스 맴버 변수의 값을 초기화할 수 있다. 매개변수 생성자는 생성자 함수 매개변수로 값을 받아서 클래스 멤버 변수를 초기화한다. 매개변수 생성자 안의 this 키워드는 클래스 자신을 가리킨다.  
> 
> ```cpp
> class	Point
> {
> private :
> 	int	x;
> 	int	y;
> public :
> 	// 디폴트 생성자
> 	Point()
> 	{
> 		x = 10;
> 		y = 20;
> 	}
> 	// 매개변수 생성자
> 	Point(int x, int y)
> 	{
> 		this->x = x;
> 		this->y = y;
> 	}
> }
> ```
> 
> ### 3. 복사 생성자
> 
> 기존의 객체를 복사하여 새로운 객체를 생성한다. 컴파일어는 모든 클래스의 기본 복사 생성자를 제공한다.  
> 
> ```cpp
> // 디폴트 복사 생성자
> int main(void)
> {
> 	Point	a;
> 	Point	b = a;
> }
> ```
> 
> ### 4. 소멸자
> 
> 소멸자는 객체의 사용이 끝날때 호출하는 함수이다. 기본으로 디폴트 소멸자가 생성된다. 보통 동적 할당된 메모리를 해제할 때 사용된다.  
> 
> ```cpp
> class	Point
> {
> private :
> 	int	x;
> 	int	y;
> public :
> 	// 디폴트 생성자
> 	Point()
> 	{
> 		x = 10;
> 		y = 20;
> 	}
> 	// 매개변수 생성자
> 	Point(int x, int y)
> 	{
> 		this->x = x;
> 		this->y = y;
> 	}
> 	// 디폴트 소멸자
> 	~Point()
> 	{
> 		std::cout << "소멸자" << std::endl;
> 	}
> }
> ```
> 
> ## c++ iomanip
> 
> iomanip(Input Output Manipulator)는 입출력 조정자로 cout의 출력을 조정할 수 있다. 사용 함수는 다음과 같다.
> 
> <details>
> <summary>setw : 출력 폭을 정한다. </summary>
> 
> 다음 스트림 요소의 너비를 설정하며 지정하려는 각 요소 앞에 삽입해야 한다.  
> setw가 설정하는 출력 폭은 최소 값이다. 그러므로 데이터가 지정한 폭보다 길다면 지정된 폭은 무시되고 데이터의 길이에 맞추어진다.  
> 
> ```cpp
> #include <iomanip>
> 
> T6 setw(streamsize Wide);
> 
> // Wide : 표시 필드의 너비
> // 반환값 : 조작자는 스트림에서 추출하거나 스트림 str에 삽입할 때 호출 str.width(Wide)한 다음 반환하는 개체를 반환한다. 
> 
> ```
> 
> ```cpp
> int	val = 12345;
> 
> std::cout << "val:" << val << std::endl;
> std::cout << "val:" << setw(10) << val << std::endl;
> std::cout << "val:" << setw(3) << val << std::endl;
> 
> // val:12345
> // val:     12345
> // val:12345
> ```
> </details>
> 
> <details>
> <summary>setfill : 공백을 채우는데 사용할 문자를 설정한다.</summary>
> 
> ```cpp
> #include <iomanip>
> 
> template <class Elem>
> T4 setfill(Elem Ch);
> 
> // Ch : 오른쪽 맞춤된 디스플레이에서 공백을 채우는데 사용할 문자입니다.
> // 반환값 : 조작자는 스트림에서 추출하거나 스트림 str에 삽입할 때 호출 str.fill(Ch)한 다음 반환하는 개체를 반환한다. 형식 Elem은 스트림 str의 요소 형식과 동일해야 한다.
> 
> ```
> </details>  
> 
> 
> [클래스, 객체, 인스턴스 참조](https://valueelectronic.tistory.com/159)
> 
