# Today I Learned

[![Gitbook](https://img.shields.io/badge/Gitbook-chanul.gitbook.io/til-blue.svg?style=for-the-badge\&logo=gitbook)](https://chanul.gitbook.io/til/)

* 매일 배우고 기록한다.
* 새로운 개발 지식을 찾았다면 쌓아둔다.
* 쌓아둔 개발 지식은 이해하기 쉽게 정리한다.

<hr>

## 22.06.09 : 실수의 표현
> 
> # 컴퓨터가 실수를 표현하는 방식
> 
> 컴퓨터는 실수를 그대로 저장할 수 없기 때문에 다른 방식을 사용한다. 실수를 정수와 소수로 나누고, > 정수를 표현할 부분과 소수를 표현할 부분을 정한다. 표현할 자릿수를 정하는 방식에 따라서 `고정 > 소수점 방식`과 `부동 소수점 방식`으로 나뉜다.  
> 
> 1. 고정 소수점 방식 (Fixed Point)
> 2. 부동 소수점 방식 (Floatting Point)
> 
> # 고정 소수점 방식 (Fixed Point)
> 
> 고정 소수점 방식은 소수부의 자릿수를 미리 정한다. 32비트 실수를 고정 소수점 방식으로 표현하면 > 다음 그림과 같다.  
> 
> ![](src/fixed_01.png)  
> 
> 
> # 부동 소수점 방식 (Floating Point)
> 
> 부동 소수점 방식은 소수부의 자릿수가 고정되어 있지 않고, 고정 소수점 방식보다 넚은 범위의 수를 > 나타낼 수 있다. 부동 소수점 방식이 매우 큰 실수를 표현할 수 있는 이유는 다음같은 수식을 사용하기 > 때문이다.  
> 
> $$
> x = \pm(.1b_2 \cdots b_k) \times 2^e
> $$
> 
> 이 식에서 $(.1b_2 \cdots b_k)$는 가수부이고, $e$는 지수부라고 한다.
> 
> - 부호부 (Sign): 1비트, 숫자의 부호로써 양수이면 0이고 음수이면 1
> - 지수부 (Exponent): 8비트, 지수
> - 가수부 (Mantissa): 23비트, 가수 또는 유효숫자  
> 
> ![](src/floating_01.png)  
> 
> 다음과 같이 나타낼 수 있다.  
> 
> $$
> x = (-1)^S \times M \times 2^E
> $$
> 
> 십진수 21.8125를 정규화된 이진수로 변환해본다.
> 
> $$정수부: (21.)_{10} = (10101)_2$$
> $$소수부: (0.8125)_{10} = (1101)_2$$
> $$(21.8125)_{10} = (10101.1101)_2$$
> $$정규화: 0.101011101 \times 2^5$$
> $$S=0, M=101011101, E=(5)_{10}=(101)_2$$
> 
> 나머지 비트를 소수부는 앞에 채우고, 가수부는 뒤에 채워준다.  
> $$|\ 부호부(1)\ |\ 지수부(8)\ |\ 가수부(23)\ |$$
> $$|\ 0\ |\ 00000101\ |\ 1010111010000000000000000\ |$$
> 
> 
> [실수의 표현](https://ahnjg.tistory.com/16)  
> [부동 소수점](https://ko.wikipedia.org/wiki/> %EB%B6%80%EB%8F%99%EC%86%8C%EC%88%98%EC%A0%90)  
> 
> # 비트 연산자
> 
> 비트 단위 연산자는 변수 내의 비트를 조작한다. 비트 단위 연산자를 사용하면 메모리를 절약하여 > 사용할 수 있기 때문에 최적화가 필요한 특정 상황에서 사용된다.  
> 
> 비트 연산자 종류
> | Symbol |  Form  | Operation                             |
> | :----: | :----: | :------------------------------------ |
> |  `<<`  | x << y | x의 모든 비트를 y만큼 왼쪽으로 이동   |
> |  `>>`  | x << y | x의 모든 비트를 y만큼 오른쪽으로 이동 |
> |  `~`   |   ~x   | x의 모든 비트를 반전                  |
> |  `&`   | x & y  | x와 y비트들을 AND 연산                |
> |  `\|`  | x \| y | x와 y비트들을 OR 연산                 |
> |  `^`   | x ^ y  | x와 y비트들을 XOR 연산                |
> 
> 산술 할당 연산자처럼 비트 할당 연산자를 제공한다.
> 
> | Symbol |  Form  | Operation                             |
> | :----: | :----: | :------------------------------------ |
> |  `<<=`  | x <<= y | x의 모든 비트를 y만큼 왼쪽으로 이동   |
> |  `>>=`  | x <<= y | x의 모든 비트를 y만큼 오른쪽으로 이동 |
> |  `&=`   | x &= y  | x와 y비트들을 AND 연산                |
> |  `\|=`  | x \|= y | x와 y비트들을 OR 연산                 |
> |  `^=`   | x ^= y  | x와 y비트들을 XOR 연산                |
> 
## 22.06.08 : OCCF

> # 정식 클래스 형식 (Orthodox Canonical Class Form)
> 
> ```cpp
> class A final
> {
>    public:
>       A ();	// 기본 생성자
>       ~A (); // 기본 소멸자
>       A (const A &a);	// 복사 생성자
>       A & operator = (const A &a);	// 할당 연산자 오버로딩
> };
> ```
> 
> [OCCF](https://www.francescmm.com/orthodox-canonical-class-form/)  
> 
> # 얕은 복사와 깊은 복사
> 
> 새롭게 생성되는 변수에 다른 변수의 값을 대입하기 위해서 대입 연산자( `=` )를 사용한다. 객체도 마찬가지로 대입 연산자를 사용하여 다른 변수에 대입할 수 있다. 하지만 객체의 대입은 얕은 복사가 이루어진다.  
> 복사에는 얕은 복사와 깊은 복사가 있다. 얕은 복사는 동적 할당된 변수의 `주소값을 복사`하고, 깊은 복사는 새로 동적 할당하고 `데이터를 복사`한다.  
> 
> 얕은 복사의 `주소값을 복사` 예시 코드는 다음과 같다. 
> 
> ```cpp
> #include <iostream>
> #include <string>
> 
> class Person {
> public:
>     Person() {
>         name = new std::string("nonamed");
>     };
>     Person(std::string n) {
>         name = new std::string(n);
>     }
> 	~Person() {
> 		delete name;
> 	}
>     std::string	*name;
> };
> 
> int main() {
>     Person P1("Buddy");
>     Person P2 = P1;
> 
> 	// 출력: (P1 주소) (P1.name 주소) (P1.name 값)
> 	std::cout << "P1: ";
> 	std::cout << &P1 << " ";
> 	std::cout << P1.name << " ";
> 	std::cout << *P1.name << " " << std::endl;
> 
> 	// 출력: (P2 주소) (P2.name 주소) (P2.name 값)
> 	std::cout << "P2: ";
> 	std::cout << &P2 << " ";
> 	std::cout << P2.name << " ";
> 	std::cout << *P2.name << " " << std::endl;
> 
> 	// P1.name 값 변경
>     *P1.name = "Jay";
> 	
> 	std::cout << "P1: ";
> 	std::cout << &P1 << " ";
> 	std::cout << P1.name << " ";
> 	std::cout << *P1.name << " " << std::endl;
> 
> 	std::cout << "P2: ";
> 	std::cout << &P2 << " ";
> 	std::cout << P2.name << " ";
> 	std::cout << *P2.name << " " << std::endl;
> 
>     exit(EXIT_SUCCESS);
> }
> 
> // P1: 0x7ff7bf9675f0 0x6000020d9120 Buddy 
> // P2: 0x7ff7bf9675c0 0x6000020d9120 Buddy 
> // P1: 0x7ff7bf9675f0 0x6000020d9120 Jay 
> // P2: 0x7ff7bf9675c0 0x6000020d9120 Jay  
> ```  
> 
> P1.name의 값을 변경하였는데 P2.name의 값도 변경되었다. 변수의 주소가 같기 때문에 변수의 값이 항상 같다. 이렇게 동적 할당된 변수의 주소값을 복사해주는 방식이 `얕은 복사`이다. 새롭게 동적 할당을 하고 데이터를 복사하기 위해서는 `복사 생성자`와 `할당 연산자 오버로딩`을 클래스에 정의해주어야 한다.  
> 
> 
> # 복사 생성자
> 
> c++에서 복사 생성자는 다른 객체의 참조를 인수로 받아서 자신을 초기화하는 방법이다. 복사 생성자로 초기화되는 객체는 깊은 복사가 된다. 반대로 디폴트 복사 생성자는 얕은 복사가 이루어진다.
> 
> ```cpp
> ClassName(const ClassName&);
> ```
> 
> 복사 생성자는 다음의 상황에서 호출된다.  
> 1. 함수의 인자로 객체가 전달
> 2. 함수의 반환값으로 객체가 반환
> 3. 새로운 객체를 기존의 객체와 똑같이 초기화  
> 
> 이전의 코드에서 복사 생성자를 추가하면 다음과 같은 결과를 얻게 된다. 생성된 객체의 name 변수 주소가 다르게 출력되며 변수가 변경되어도 다른 객체의 변수는 변경되지 않는다.  
> 
> ```cpp
> class Person {
> public:
> 
>     // ...
> 	
> 	// 복사 생성자
> 	Person(const Person &origin) {
> 		name = new std::string(*origin.name);
> 	}
> };
> 
> int	main() {
> 	
> 	// 출력 코드
> 	// (P1 주소) (P1.name 주소) (P1.name 값)
> 	// (P2 주소) (P2.name 주소) (P2.name 값)
> 	
> }
> 
> // P1: 0x7ff7bbe3d5f0 0x600000051120 Buddy 
> // P2: 0x7ff7bbe3d5c0 0x600000051140 Buddy 
> // P1: 0x7ff7bbe3d5f0 0x600000051120 Jay 
> // P2: 0x7ff7bbe3d5c0 0x600000051140 Buddy
> ```  
> 
> [복사 생성자](http://www.tcpschool.com/cpp/cpp_conDestructor_copyConstructor)  
> 
> # 할당 연산자 오버로딩
> 
> `복사 생성자`로 깊은 복사가 가능하지만 새로운 변수를 초기화하는 위치에서만 가능하다. 다음과 같이 새로운 변수의 초기화 이후에 대입하면 얕은 복사가 이루어진다.  
> 
> ```cpp
> class Person {
> public:
> 
>     // ...
> 	
> 	// 복사 생성자
> 	Person(const Person &origin) {
> 		name = new std::string(*origin.name);
> 	}
> };
> 
> int	main() {
> 	Person P1("Buddy");
>     Person P2;
> 	P2 = P1;
> 	
> 	// 출력 코드
> 	// (P1 주소) (P1.name 주소) (P1.name 값)
> 	// (P2 주소) (P2.name 주소) (P2.name 값)
> 	
> }
> 
> // P1: 0x7ff7b56535f0 0x600000c45120 Buddy 
> // P2: 0x7ff7b56535c0 0x600000c45120 Buddy 
> // P1: 0x7ff7b56535f0 0x600000c45120 Jay 
> // P2: 0x7ff7b56535c0 0x600000c45120 Jay 
> ```  
> 
> 복사 생성자를 정의해도 대입 연산자( `=` )를 사용하는 경우에 얕은 복사가 이루어진다. 그렇기 때문에 `할당 연산자 오버로딩`을 정의해야 한다.  
> 
> `연산자 오버로딩`은 두개의 객체가 이미 생성되어 있는 상태에서 호출되기 때문에 주의할 점이 있다. 이전에 동적 할당된 데이터를 해제해야 한다. 해제하지 않고 데이터를 복사하게 되면 이전의 데이터를 해제할 수 없기 때문에 메모리 누수 문제가 발생한다.  
> 
> ```cpp
> class Person {
> public:
> 
>     // ...
> 	
> 	// 복사 생성자
> 	Person(const Person &origin) {
> 		name = new std::string(*origin.name);
> 	}
> 	
> 	// 대입 연산자 오버로딩
> 	Person& operator=(const Person &origin) {
> 		delete name;
> 		name = new std::string(*origin.name);
> 		return *this;
> 	}
> };
> 
> int	main() {
> 	Person P1("Buddy");
>     Person P2;
> 	P2 = P1;
> 	
> 	// 출력 코드
> 	// (P1 주소) (P1.name 주소) (P1.name 값)
> 	// (P2 주소) (P2.name 주소) (P2.name 값)
> 	
> }
> 
> // P1: 0x7ff7bafd25f0 0x600003d65120 Buddy 
> // P2: 0x7ff7bafd25c0 0x600003d65140 Buddy 
> // P1: 0x7ff7bafd25f0 0x600003d65120 Jay 
> // P2: 0x7ff7bafd25c0 0x600003d65140 Buddy 
> ```  
> 
> [대입 연산자 오버로딩](https://musket-ade.tistory.com/entry/C-%EB%8C%80%EC%9E%85-%EC%97%B0%EC%82%B0%EC%9E%90-%EC%98%A4%EB%B2%84%EB%A1%9C%EB%94%A9)
> 

## 22.06.07 : c++ 함수 포인터

> 함수 포인터는 함수를 가리키는 변수이다.  
> 
> ```cpp
> #include <iostream>
> 
> void	say_hello( void )
> {
> 	std::cout << "Hello" << std::endl;
> }
> 
> int	main( void )
> {
> 	void (*funcPtr)() = say_hello;
> 	
> 	say_hello();
> 	funcPtr();
> 	reurn (0);
> }
> 
> // Hello
> // Hello
> ```
> 
> 맴버 함수 포인터를 위한 연산자이다.  
> 
> ```cpp
> #include <iostream>
> 
> class Harl
> {
> 	
> public :
> 	void	say_hello( void );
> 	void	say_name( void );
> 	
> };
> 
> void	Harl::say_hello( void )
> {
> 	std::cout << "Hello!" << std::endl;
> }
> 
> void	Harl::say_name( void )
> {
> 	std::cout << "I'm Harl" << std::endl;
> }
> 
> int	main( void )
> {
> 	Harl	harl;
> 	Harl	*harl_ptr;
> 	void	(Harl::*fp)( void );
> 	
> 	fp = &Harl::say_hello;
> 	(harl.*fp)();
> 	
> 	harl_ptr = new Harl;
> 	fp = &Harl::say_name;
> 	(harl_ptr->*fp)();
> }
> ```
> 
> [맴버 함수 포인터](https://www.joinc.co.kr/w/Site/C/Documents/Using_Member_Function_Pointer)

## 22.06.06 : 파일 입출력
> 
> # fstream
> 
> ## (constructor)
> ```cpp
> // default (1)	
> fstream();
> 
> // initialization (2)	
> explicit fstream (const char* filename,
>                   ios_base::openmode mode = ios_base::in | ios_base::out);
> 				  
> // filename : 파일의 이름을 나타내는 문자열
> // mode : 파일에 대해 요청된 입/출력 모드 플래그 
> 
> ```
> 
> ### mode
> 
> | member constant | stands for | access                                                                                   |
> | :-------------- | :--------- | :--------------------------------------------------------------------------------------- |
> | in              | input      | File open for reading: the internal stream buffer supports input operations.             |
> | out             | output     | File open for writing: the internal stream buffer supports output operations.            |
> | binary          | binary     | Operations are performed in binary mode rather than text.                                |
> | ate             | at end     | The output position starts at the end of the file.                                       |
> | app             | append     | All output operations happen at the end of the file, appending to its existing contents. |
> | trunc           | truncate   | Any contents that existed in the file before it is open are discarded.                   |
> 
> ### Example
> 
> ```cpp
> // fstream constructor.
> #include <fstream>      // std::fstream
> 
> int main () {
> 
>   std::fstream fs ("test.txt", std::fstream::in | std::fstream::out);
> 
>   // i/o operations here
> 
>   fs.close();
> 
>   return 0;
> }
> ```
> 
> ## open
> 
> ```cpp
> void open (const char* filename,
>            ios_base::openmode mode = ios_base::in | ios_base::out);
> 		   
> // filename : 파일의 이름을 나타내는 문자열
> // mode : 파일에 대해 요청된 입/출력 모드 플래그 
> 
> ```
> 
> ### Example
> 
> ```cpp
> // fstream::open / fstream::close
> #include <fstream>      // std::fstream
> 
> int main () {
> 
>   std::fstream fs;
>   fs.open ("test.txt", std::fstream::in | std::fstream::out | std::fstream::app);
> 
>   fs << " more lorem ipsum";
> 
>   fs.close();
> 
>   return 0;
> }
> ```
> 
> ## is_open : 파일이 열려있는지 확인
> 
> ```cpp
> bool is_open();
> 
> // 반환 : 열려있으면 true, 아니면 false
> ```
> 
> ## close : 파일 닫기
> 
> ```cpp
> void close();
> 
> // 반환 : 없음
> ```
> 
> ## swap : 데이터 교환
> 
> ```cpp
> void swap(fstream& x);
> 
> // x : 다른 fstream 객체
> // 반환 : 없음
> ```
> 
> ### Example
> 
> ```cpp
> // swapping fstream objects
> #include <fstream>      // std::fstream
> 
> int main () {
>   std::fstream foo;
>   std::fstream bar ("test.txt");
> 
>   foo.swap(bar);
> 
>   foo << "lorem ipsum";
> 
>   foo.close();
> 
>   return 0;
> }
> ```
> 
> # istream
> 
> ## getline : 한 줄을 읽는다.
> 
> 스트림에서 문자를 추출한다. n개의 문자가 s에 기록될 때까지 s에 c-string으로 저장한다.  
> 구분 문자는 개행 문자(`\n`)이거나 지정된 문자(`delim`)이다.
> 
> ```cpp
> istream& getline (char* s, streamsize n );
> istream& getline (char* s, streamsize n, char delim );
> 
> // s : 추출된 문자 배열의 포인터
> // n : 읽을 문자의 최대 개수
> // delim : 명시적 구분 문자로서 이 문자가 발견되면 추출 작업 종료
> ```
> 
> ### Example
> 
> ```cpp
> // istream::getline example
> #include <iostream>     // std::cin, std::cout
> 
> int main () {
>   char name[256], title[256];
> 
>   std::cout << "Please, enter your name: ";
>   std::cin.getline (name,256);
> 
>   std::cout << "Please, enter your favourite movie: ";
>   std::cin.getline (title,256);
> 
>   std::cout << name << "'s favourite movie is " << title;
> 
>   return 0;
> }
> ```
> 
> # ostream
> 
> ## write : 데이터 쓰기
> 
> 문자열 앞에서부터 n개의 문자를 스트림에 넣는다.
> 
> ```cpp
> ostream& write (const char* s, streamsize n);
> 
> // s : 최소 n개의 문자를 가진 문자열
> // n : 삽입할 문자의 개수
> // 반환 : ostream 개체
> ```
> 
> ## Example
> 
> ```cpp
> // Copy a file
> #include <fstream>      // std::ifstream, std::ofstream
> 
> int main () {
>   std::ifstream infile ("test.txt",std::ifstream::binary);
>   std::ofstream outfile ("new.txt",std::ofstream::binary);
> 
>   // get size of file
>   infile.seekg (0,infile.end);
>   long size = infile.tellg();
>   infile.seekg (0);
> 
>   // allocate memory for file content
>   char* buffer = new char[size];
> 
>   // read content of infile
>   infile.read (buffer,size);
> 
>   // write to outfile
>   outfile.write (buffer,size);
> 
>   // release dynamically-allocated memory
>   delete[] buffer;
> 
>   outfile.close();
>   infile.close();
>   return 0;
> }
> ```
> 
> [c++ fstream](https://m.cplusplus.com/reference/ostream/ostream/write/)
> [파일 입출력에 대해서](https://blockdmask.tistory.com/322)

## 22.06.05 : 객체 생성 방법, 포인터와 참조자

>
> ## 객체 생성 방법
> c++에서는 객체를 생성하는 방법이 두가지이다. 스택에 생성하는 방법과 힙에 생성하는 방법이 있는데 이들의 차이는 클래스의 생성자와 소멸자를 통해 알 수 있다.  
> 
> ```cpp
> class Number
> {
> 	
> private :
> 	int val;
> 
> public :
> 	Number( int val );
> 	~Number( void );
> 	void	print_val( void );
> 	
> };
> 
> Number::Number( int val )
> {
> 	this->val = val;
> }
> 
> Number::~Number( void )
> {
> 	std::cout << "destroyed: " << this->val << std::endl;
> }
> 
> void	Number::print_val( void )
> {
> 	std::cout << "val: " << this->val << std::endl;
> }
> 
> Number	*test_heap( void )
> {
> 	Number	*num;
> 	
> 	num = new Number(20);
> 	num->print_val();
> 	return (num);
> }
> 
> void	test_stack( void )
> {
> 	Number	num(10);
> 	
> 	num.print_val();
> }
> 
> int	main( void )
> {
> 	Number	*num_heap;
> 
> 	num_heap = test_heap();
> 	test_stack();
> 	delete num_heap;
> 	return (0);
> }
> 
> /* output
> 
> val: 20
> val: 10
> destroyed: 10
> destroyed: 20
> 
> */
> ```
> - `test_stack` 함수에서 생성된 인스턴스는 함수가 종료되며 자동으로 해제된다.  
> - `test_heap` 함수에서 `new` 키워드로 생성된 인스턴스는 `delete` 키워드로 해제하기 전까지 메모리에 유지된다.  
> 
> 만약에 객체 배열을 동적 할당한다면 `delete []`로 해제한다.
> 
> ```cpp
> Number*	nums = new Number [5];
> delete [] nums;
> ```
> 
> [객체 생성의 두가지 방법과 차이](https://yoon90.tistory.com/13)
> [객체 배열과 동적 할당이란](https://blog.naver.com/PostView.nhn?blogId=kartmon&logNo=221503845077)
> 
> ## 포인터와 참조자
> 
> 포인터 변수는 메모리에 주소값을 저장하고, 읽어서 실제 데이터에 접근한다. C 언어에서는 변수나 상수를 가리키는 방법으로 포인터를 사용하였지만 c++에서는 포인터와 함께 참조자라는 방법을 사용할 수 있다.
> 
> ```cpp
> #include <iostream>
> 
> int	main( void )
> {
> 	int		num = 10;
> 	int*	ptr = &num;
> 	int&	ref = num;
> 
> 	// 변수
> 	std::cout << "num: " << num << std::endl;
> 	
> 	// 포인터
> 	std::cout << "ptr: " << ptr << std::endl;
> 	std::cout << "*ptr: " << *ptr << std::endl;
> 	
> 	// 참조자
> 	std::cout << "ref: " << ref << std::endl;
> 	std::cout << "&ref: " << &ref << std::endl;
> }
> 
> // num: 10
> // ptr: 0x7ff7b25ad5fc
> // *ptr: 10
> // ref: 10
> // &ref: 0x7ff7be3da5fc
> ```
> 
> 참조자는 `*` 대신에 `&`를 붙여서 선언하면 되고, 선언할 때에 반드시 참조할 변수를 명시해주어야 한다. 그리고 선언된 참조자는 자신이 참조하는 대상을 변경할 수 없다. 이후 동작들은 참조 대상의 값을 변경시킬 뿐이다. 다른 참조자를 대입하려해도 참조하는 주소는 변경되지 않고 데이터 값이 변경된다.  
> 
> ```cpp
> ref = 20;
> std::cout << "num: " << num << std::endl;
> std::cout << "ref: " << ref << std::endl;
> 
> // num: 20
> // ref: 20
> ```
> 
> ```cpp
> int		new_num = 30;
> int&	other_ref = new_num;
> 
> ref = other_ref;
> std::cout << ref << " : " << &ref << std::endl;
> std::cout << other_ref << " : " << &other_ref << std::endl;
> 
> // 30 : 0x7ff7bd3a75fc
> // 30 : 0x7ff7bd3a75e4
> ```
> 
> 다음은 함수가 참조자를 인자로 받는 경우이다.  
> 
> ```cpp
> void	change_num(int& n)
> {
> 	n = 20;
> }
> 
> int	main( void )
> {
> 	int		num = 10;
> 	int&	ref = num;
> 	
> 	std::cout << "num: " << num << std::endl;
> 	std::cout << "ref: " << ref << std::endl;
> 	change_num(ref);
> 	std::cout << "num: " << num << std::endl;
> 	std::cout << "ref: " << ref << std::endl;
> }
> 
> // num: 10
> // ref: 10
> // num: 20
> // ref: 20
> ```
> 
> [포인터 위키백과](https://ko.wikipedia.org/wiki/%ED%8F%AC%EC%9D%B8%ED%84%B0_(%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D)#:~:text=%ED%8F%AC%EC%9D%B8%ED%84%B0(pointer)%EB%8A%94%20%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D%20%EC%96%B8%EC%96%B4,%EA%B2%83%EC%9D%84%20%EC%97%AD%EC%B0%B8%EC%A1%B0%EB%9D%BC%EA%B3%A0%20%ED%95%9C%EB%8B%A4.)  
> [c++ 참조자](https://modoocode.com/141)
> 


### 22.06.03 : c++ 소멸자
> 
> 소멸자란 개체가 범위를 벗어나거나 호출에 의해 명시적으로 제거될 때 자동으로 호출되는 맴버 함수이다. 소멸자의 이름은 클래스와 같고, 앞에 `~`가 붙는다.  
> 
> 예제 코드에서 `String::~String` 소멸자는 `delete`를 사용하여 동적 할당된 공간의 할당을 취소한다.
> 
> ```cpp
> // spec1_destructors.cpp
> #include <string>
> 
> class String {
> public:
>    String( char *ch );  // Declare constructor
>    ~String();           //  and destructor.
> private:
>    char    *_text;
>    size_t  sizeOfText;
> };
> 
> // Define the constructor.
> String::String( char *ch ) {
>    sizeOfText = strlen( ch ) + 1;
> 
>    // Dynamically allocate the correct amount of memory.
>    _text = new char[ sizeOfText ];
> 
>    // If the allocation succeeds, copy the initialization string.
>    if( _text )
>       strcpy_s( _text, sizeOfText, ch );
> }
> 
> // Define the destructor.
> String::~String() {
>    // Deallocate the memory that was previously reserved
>    //  for this string.
>    delete[] _text;
> }
> 
> int main() {
>    String str("The piper in the glen...");
> }
> ```
> 
> ## 소멸자 선언
> 
> - 클래스와 이름이 같으며 물결표(~)가 앞에 붙는다.
> - 인수를 받아들이지 않는다.
> - 값(또는 void)를 반환하지 않아야 한다.
> 
> ## 소멸자 사용
> 
> 소멸자는 클래스 맴버 함수를 자유롭게 호출하고 클래스 맴버 데이터에 접근할 수 있다.  
> 다음 이벤트 중 하나가 발생하면 소멸자가 호출된다.  
> 
> - 블록 스코프에 지역 개체가 있는 상태에서 블록 스코프를 벗어난 경우
> - `new`키워드로 할당된 개체가 `delete`키워드로 제거된 경우
> - 임시 개체의 수명이 종료된 경우
> - 프로그램이 종료되고 전역 또는 정적 개체가 있는 경우
> - 소멸자는 소멸자 함수의 정규화된 이름을 사용하여 명시적으로 호출된다.  
> 
> 소멸자 사용에 두가지 제한 사항이 있다.
> 
> - 주소를 가져갈 수 없다.
> - 파생 클래스는 기본 클래스의 소멸자를 상속하지 않는다.
> 
> ## 파괴 순서
> 
> 개체가 범위를 벗어나거나 삭제된 경우 개체가 완전히 소멸되는 이벤트 순서는 다음과 같다.
> 
> 1. 클래스의 소멸자가 호출되고 소멸자 함수의 본문이 실행된다.
> 2. 비정적 멤버 개체의 소멸자는 클래스 선언에 나타나는 역순으로 호출된다. 이러한 멤버의 생성에 사용되는 선택적 멤버 초기화 목록은 생성 또는 소멸 순서에 영향을 주지 않는다.
> 3. 비가상 기본 클래스의 소멸자는 선언의 역순으로 호출된다.
> 4. 가상 기본 클래스의 소멸자는 선언의 역순으로 호출된다.
> 
> ```cpp
> // order_of_destruction.cpp
> #include <cstdio>
> 
> struct A1      { virtual ~A1() { printf("A1 dtor\n"); } };
> struct A2 : A1 { virtual ~A2() { printf("A2 dtor\n"); } };
> struct A3 : A2 { virtual ~A3() { printf("A3 dtor\n"); } };
> 
> struct B1      { ~B1() { printf("B1 dtor\n"); } };
> struct B2 : B1 { ~B2() { printf("B2 dtor\n"); } };
> struct B3 : B2 { ~B3() { printf("B3 dtor\n"); } };
> 
> int main() {
>    A1 * a = new A3;
>    delete a;
>    printf("\n");
> 
>    B1 * b = new B3;
>    delete b;
>    printf("\n");
> 
>    B3 * b2 = new B3;
>    delete b2;
> }
> 
> Output: A3 dtor
> A2 dtor
> A1 dtor
> 
> B1 dtor
> 
> B3 dtor
> B2 dtor
> B1 dtor
> ```
> 
> [c++ 소멸자 참조](https://docs.microsoft.com/en-us/cpp/cpp/destructors-cpp?view=msvc-170)  
> [인스턴스 배열의 소멸자는 역순으로 호출](https://stackoverflow.com/questions/31101854/order-of-destruction-for-stack-heap-allocated-arrays/31102112)
> 
### 22.06.02 : c++ timestamp, string
> 
> # c++ const
> 
> `const` 키워드는 값을 상수로 선언하여 변경할 수 없도록 한다. 한번 설정된 값이 read-only 메모리에 올라가게되어 변경하지 못하고 계속 유지하게 된다.
> 
> ```cpp
> const int	val = 0;
> val = 1; // error!
> ```
> 
> ## const 포인터
> 
> 상수가 가리키는 공간을 수정하지 못하게 한다. `const`가 `int`에 적용되었기 때문에 포인터가 가리키고 있는 값을 변경할 수 없지만, 주소값은 변경할 수 있다.
> 
> ```cpp
> int			val = 0;
> const int	*ptr = &val;
> 
> val = 1;	// ok
> *ptr = 2;	// error
> ```
> 
> 반대로 다음의 경우에는 주소값을 변경하지 못하게 한다. `const`가 포인터에 적용되어 주소값을 변경할 수 없지만, 포인터가 가리키고 있는 값을 변경할 수 있다.
> 
> ```cpp
> int			val_1 = 0;
> int			val_2 = 0;
> int* const	ptr = &val_1;
> 
> val_1 = 1;	// ok
> *ptr = 2;	// ok
> ptr = &val_2;	// error
> ``` 
> 
> 
> [c++ const 이해하기](https://dydtjr1128.github.io/cpp/2020/01/08/Cpp-const.html)  
> [static 맴버와 const 멤버](http://www.parkjonghyuk.net/lecture/program2/chap06.pdf)
> 
> # c++ static
> 
> [static의 클래스, 맴버 함수, 객체, 함수 예제](https://wookiist.dev/71)
> 
> # timestamp
> 
> [현재 날짜와 현재 시간 출력](https://wookiist.dev/71)
> 
> # c++ string 클래스 맴버 함수
> 
> [c++ string 페이지에 정리함](c++/StringMethods.md)  
> 
> ---
> 
> [c++ string 참조](https://www.cplusplus.com/reference/string/string/)  
> [c++ string 클래스에 대해서 (총정리) 참조](https://blockdmask.tistory.com/338)  
> 
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
