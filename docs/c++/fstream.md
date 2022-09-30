---
layout: default
title: fstream
nav_order: 2
parent: c++ 
permalink: /docs/c++/fstream
---

* [파일 입출력 - fstream](#파일-입출력---fstream)
  * [(constructor)](#constructor)
    * [mode](#mode)
    * [Example](#example)
  * [open](#open)
    * [Example](#example-1)
  * [is_open : 파일이 열려있는지 확인](#is_open--파일이-열려있는지-확인)
  * [close : 파일 닫기](#close--파일-닫기)
  * [swap : 데이터 교환](#swap--데이터-교환)
    * [Example](#example-2)
* [istream](#istream)
  * [getline : 한 줄을 읽는다.](#getline--한-줄을-읽는다)
    * [Example](#example-3)
* [ostream](#ostream)
  * [write : 데이터 쓰기](#write--데이터-쓰기)
  * [Example](#example-4)

# 파일 입출력 - fstream

## (constructor)
```cpp
// default (1)	
fstream();

// initialization (2)	
explicit fstream (const char* filename,
                  ios_base::openmode mode = ios_base::in | ios_base::out);
				  
// filename : 파일의 이름을 나타내는 문자열
// mode : 파일에 대해 요청된 입/출력 모드 플래그 

```

### mode

| member constant | stands for | access                                                                                   |
| :-------------- | :--------- | :--------------------------------------------------------------------------------------- |
| in              | input      | File open for reading: the internal stream buffer supports input operations.             |
| out             | output     | File open for writing: the internal stream buffer supports output operations.            |
| binary          | binary     | Operations are performed in binary mode rather than text.                                |
| ate             | at end     | The output position starts at the end of the file.                                       |
| app             | append     | All output operations happen at the end of the file, appending to its existing contents. |
| trunc           | truncate   | Any contents that existed in the file before it is open are discarded.                   |

### Example

```cpp
// fstream constructor.
#include <fstream>      // std::fstream

int main () {

  std::fstream fs ("test.txt", std::fstream::in | std::fstream::out);

  // i/o operations here

  fs.close();

  return 0;
}
```

## open

```cpp
void open (const char* filename,
           ios_base::openmode mode = ios_base::in | ios_base::out);
		   
// filename : 파일의 이름을 나타내는 문자열
// mode : 파일에 대해 요청된 입/출력 모드 플래그 

```

### Example

```cpp
// fstream::open / fstream::close
#include <fstream>      // std::fstream

int main () {

  std::fstream fs;
  fs.open ("test.txt", std::fstream::in | std::fstream::out | std::fstream::app);

  fs << " more lorem ipsum";

  fs.close();

  return 0;
}
```

## is_open : 파일이 열려있는지 확인

```cpp
bool is_open();

// 반환 : 열려있으면 true, 아니면 false
```

## close : 파일 닫기

```cpp
void close();

// 반환 : 없음
```

## swap : 데이터 교환

```cpp
void swap(fstream& x);

// x : 다른 fstream 객체
// 반환 : 없음
```

### Example

```cpp
// swapping fstream objects
#include <fstream>      // std::fstream

int main () {
  std::fstream foo;
  std::fstream bar ("test.txt");

  foo.swap(bar);

  foo << "lorem ipsum";

  foo.close();

  return 0;
}
```

# istream

## getline : 한 줄을 읽는다.

스트림에서 문자를 추출한다. n개의 문자가 s에 기록될 때까지 s에 c-string으로 저장한다.  
구분 문자는 개행 문자(`\n`)이거나 지정된 문자(`delim`)이다.

```cpp
istream& getline (char* s, streamsize n );
istream& getline (char* s, streamsize n, char delim );

// s : 추출된 문자 배열의 포인터
// n : 읽을 문자의 최대 개수
// delim : 명시적 구분 문자로서 이 문자가 발견되면 추출 작업 종료
```

### Example

```cpp
// istream::getline example
#include <iostream>     // std::cin, std::cout

int main () {
  char name[256], title[256];

  std::cout << "Please, enter your name: ";
  std::cin.getline (name,256);

  std::cout << "Please, enter your favourite movie: ";
  std::cin.getline (title,256);

  std::cout << name << "'s favourite movie is " << title;

  return 0;
}
```

# ostream

## write : 데이터 쓰기

문자열 앞에서부터 n개의 문자를 스트림에 넣는다.

```cpp
ostream& write (const char* s, streamsize n);

// s : 최소 n개의 문자를 가진 문자열
// n : 삽입할 문자의 개수
// 반환 : ostream 개체
```

## Example

```cpp
// Copy a file
#include <fstream>      // std::ifstream, std::ofstream

int main () {
  std::ifstream infile ("test.txt",std::ifstream::binary);
  std::ofstream outfile ("new.txt",std::ofstream::binary);

  // get size of file
  infile.seekg (0,infile.end);
  long size = infile.tellg();
  infile.seekg (0);

  // allocate memory for file content
  char* buffer = new char[size];

  // read content of infile
  infile.read (buffer,size);

  // write to outfile
  outfile.write (buffer,size);

  // release dynamically-allocated memory
  delete[] buffer;

  outfile.close();
  infile.close();
  return 0;
}
```

[c++ fstream](https://m.cplusplus.com/reference/ostream/ostream/write/)
[파일 입출력에 대해서](https://blockdmask.tistory.com/322
