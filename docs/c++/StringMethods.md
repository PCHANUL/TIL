---
layout: default
title: StringMethods
nav_order: 2
parent: c++ 
permalink: /docs/c++/StringMethods
---

* [String Methods](#string-methods)
  * [Element access](#element-access)
    * [operator[] : 해당 위치의 문자 참조를 반환](#operator--해당-위치의-문자-참조를-반환)
    * [at : 해당 위치의 문자 참조를 반환](#at--해당-위치의-문자-참조를-반환)
    * [back : 마지막 위치의 문자 참조를 반환](#back--마지막-위치의-문자-참조를-반환)
    * [front : 첫번째 위치의 문자 참조를 반환](#front--첫번째-위치의-문자-참조를-반환)
  * [Modifiers](#modifiers)
    * [operator+= : 문자열에 추가](#operator--문자열에-추가)
    * [append : 문자열에 추가](#append--문자열에-추가)
    * [push_back : 문자열 마지막에 문자를 추가](#push_back--문자열-마지막에-문자를-추가)
    * [assign : 문자열에 새로운 값을 할당](#assign--문자열에-새로운-값을-할당)
    * [insert : 문자열에서 지정하는 위치 앞에 문자를 삽입](#insert--문자열에서-지정하는-위치-앞에-문자를-삽입)
    * [erase : 문자열에서 지정하는 위치의 문자를 제거](#erase--문자열에서-지정하는-위치의-문자를-제거)
    * [replace : 문자열에서 지정하는 위치의 문자를 다른 문자열로 변경](#replace--문자열에서-지정하는-위치의-문자를-다른-문자열로-변경)
    * [swap : 문자열의 내용을 다른 문자열과 교환한다.](#swap--문자열의-내용을-다른-문자열과-교환한다)
    * [pop_back : 문자열의 마지막을 지운다](#pop_back--문자열의-마지막을-지운다)
  * [Iterators](#iterators)
    * [begin : 문자열의 첫번째 문자를 가리키는 반복자를 반환](#begin--문자열의-첫번째-문자를-가리키는-반복자를-반환)
    * [end : 문자열의 마지막 문자를 가리키는 반복자를 반환](#end--문자열의-마지막-문자를-가리키는-반복자를-반환)
  * [Capacity](#capacity)
    * [size & length : 문자열 길이](#size--length--문자열-길이)
    * [resize : 문자열 길이 재설정](#resize--문자열-길이-재설정)
    * [empty : 문자열이 비었는지 반환](#empty--문자열이-비었는지-반환)
  * [String operations](#string-operations)
    * [substr : 부분 문자열 생성](#substr--부분-문자열-생성)
    * [compare : 문자열을 비교](#compare--문자열을-비교)

# String Methods


## Element access

### operator[] : 해당 위치의 문자 참조를 반환

```cpp
char& operator[] (size_t pos);

// pos : 문자의 위치
// 반환 : 문자열의 지정된 위치에 있는 문자
```

### at : 해당 위치의 문자 참조를 반환

```cpp
char& at (size_t pos);

// pos : 문자의 위치, 문자의 위치가 아니면 out_of_range 예외가 발생
// 반환 : 문자열의 지정된 위치에 있는 문자
```

### back : 마지막 위치의 문자 참조를 반환

```cpp
char& back ();

// 반환 : 문자열의 마지막 위치에 있는 문자
```

### front : 첫번째 위치의 문자 참조를 반환

```cpp
char& front ();

// 반환 : 문자열의 첫번째 위치에 있는 문자
```


## Modifiers

### operator+= : 문자열에 추가

```cpp
string& operator+= (const string& str); // string (1)
string& operator+= (const char* s); // c-string(2)
string& operator+= (char c); // character (3)

// str : 복사되어 마지막에 붙는 문자열
// s : 복사되어 마지막에 붙는 null로 끝나는 문자 시퀀스 포인터
// c : 문자열의 값에 추가되는 문자
// 반환 : *this
```

### append : 문자열에 추가

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

**Example**

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

### push_back : 문자열 마지막에 문자를 추가

```cpp
void push_back (char c);

// c : 문자열에 추가되는 문자
// 반환 : 없음
```

### assign : 문자열에 새로운 값을 할당

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

**Example**

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

### insert : 문자열에서 지정하는 위치 앞에 문자를 삽입

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

**Example**

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

### erase : 문자열에서 지정하는 위치의 문자를 제거

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

### replace : 문자열에서 지정하는 위치의 문자를 다른 문자열로 변경

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

**Example**

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

### swap : 문자열의 내용을 다른 문자열과 교환한다.

```cpp
void swap (string& str);

// str : 교환할 다른 문자열
// 반환 : 없음
```

**Example**

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

### pop_back : 문자열의 마지막을 지운다

```cpp
void pop_back();

// 반환 : 없음
```

**Example**

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

## Iterators

### begin : 문자열의 첫번째 문자를 가리키는 반복자를 반환

```cpp
iterator begin();

// 반환 : 문자열의 첫번째 문자를 가리키는 반복자
```

**Example**

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

### end : 문자열의 마지막 문자를 가리키는 반복자를 반환

```cpp
iterator end();

// 반환 : 문자열의 마지막 문자를 가리키는 반복자
```

**Example**

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

## Capacity

### size & length : 문자열 길이

string::size와 string::length는 정확히 동일한 값을 반환한다.

```cpp
size_t size() const;
size_t length() const;

// 반환 : 문자열 바이트 수
```

**Example**

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

### resize : 문자열 길이 재설정

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

### empty : 문자열이 비었는지 반환

```cpp
bool empty() const;

// 반환 : 문자열 길이가 0이라면 true, 아니라면 false
```

## String operations

### substr : 부분 문자열 생성

```cpp
string substr (size_t pos = 0, size_t len = npos) const;

// pos : 부분 문자열로 복제될 첫번째 문자 위치, 
// 문자열 길이과 같다면 함수는 빈 문자열을 반환하고, 
// 문자열 길이보다 크다면 out_of_range
// len : 부분 문자열에 포함될 문자의 개수
// 반환 : 부분 문자열인 문자열
```

**Example**

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

### compare : 문자열을 비교

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

**Example**

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

---

[c++ string](https://www.cplusplus.com/reference/string/string/)  
[c++ string 클래스에 대해서 (총정리)](https://blockdmask.tistory.com/338)  
