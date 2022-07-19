# exception

## 왜 예외를 쓰는 게 좋을까요?

c++에 예외가 도입된 이유를 생각해볼 필요가 있다. Java와 c# 같은 언어가 c++의 예외를 물려받은 이유는 결함내성 소프트웨어를 쉽게 작성하기 위함이다. 에러 처리를 한 후에도 일관성있는 상태를 유지해야 한다는 사실은 결함 내성을 달성하기 까다롭게 만든다.  

어떤 파일을 열고 데이터를 읽어들이는 C코드를 생각해본다.

```cpp
char*
OpenAndRead(char* fn, char* mode)
{
  FILE* fp = fopen(fn, mode);
  char *buf = malloc(BUF_SZ);
  fgets(buf, BUF_SZ, fp);
  return buf;
}
```

OpenAndRead()는 모든 서브루틴이 잘 수행된다는 가정을 한다면 잘 작동하고 읽기 쉽다. 하지만 fn 파일이 없는 경우, malloc()이나 fgets()가 실패되어 에러가 발생할 가능성이 있다. 이 코드를 결함내성이 있게 만드려면 각 서브루틴이 호출된 후에 에러가 발생하는지 확인하고, 어떤 행동을 해야하는지 결정해야한다. 단순히 에러 메시지를 출력하고 자원을 해제한 후에 -1을 반환하도록 바꾸어 보면 다음과 같을 것이다.  

```cpp
int
OpenAndRead(char* fn, char* mode, char** data)
{
  *data = NULL;
  char* buf = NULL;
  FILE* fp = fopen(fn, mode);
  if (!fp) {
    printf("Can't open %s\n", fn);
    return -1;
  }
 
  buf = malloc(BUF_SZ);
  if (!buf) {
    printf("Can't allocate memory for data\n");
    fclose(fp);
    return -1;
  }
 
  if (!fgets(buf, BUF_SZ, fp)) {
    printf("Can't read data from %s\n", fn);
    free(buf);
    fclose(fp);
    return -1;
  }
 
  *data = buf;
 
  return 0;
}
```

이 코드는 에러를 처리하지만 읽기 어렵다. 알고리즘과 에러 처리가 섞여있기 때문인데 이 문제를 예외를 발생시키는 방법으로 변경할 수 있다.  

```cpp
int
OpenAndRead(char* fn, char* mode, char** data)
{
  *data = NULL;
  char* buf = NULL;
  FILE* fp = NULL;
  try {
    fp = fopen(fn, mode);
    buf = malloc(BUF_SZ);
    fgets(buf, BUF_SZ, fp);
    *data = buf;
    return 0;
  }
  catch (const FileDoesNotExist&) {
    printf("Can't open %s\n", fn);
  }
  catch (const FileNotAccessible&) {
    printf("The access %s is not permissible for %s\n", mode, fn);
  }
  catch (const MemoryExhausted&) {
    printf("Can't allocate memory for data\n");
    fclose(fp);
  }
  catch (const EndOfFile&) {
    printf("Can't read data from %s\n", fn);
    fclose(fp);
    free(buf);
  }
 
  return -1;
}
```

OpenAndRead() 주 제어 흐름이 에러 처리와 분리되어 읽기 쉬워졌다. 하지만 아직 라인 수가 많다. 여기에서 예외가 발생했을 때 이루어지는 스택 풀기 과정을 이해하면 코드 라인 수를 줄일 수 있다.  

스택 풀기는 예외가 발생하면 예외 처리기를 만나기 전까지 스택을 풀어가는 과정을 말한다. c++ 에서는 try 영역에서 생성된 모든 자동 객체들에 대해 소멸자를 불러온다. 자동 객체들은 생성되었던 반대 순서로 소멸된다.  

자원을 소유하는 객체들을 자동 객체로 정의하면 예외가 발생하더라도 자동적으로 소멸된다. 이런 프로그래밍을 가능하게 만드는 자원 소유 객체들이 파일이나 메모리에 이미 정의되어 있다. 파일 자원용으로 std::fstream, 메모리 자원용으로는 boost::scoped_ptr을 사용할 수 있다. 이 객체를 사용하면 다음과 같이 구현할 수 있다.  

```cpp
char*
OpenAndRead(const std::string& fn, std::ios_base::openmode mode)
{
  try {
    std::fstream fs(fn.c_str(), mode);
    boost::scoped_ptr buf(new char[BUF_SZ+1]);
    fs.getline(buf.get(), BUF_SZ);
    return buf.get();
  }
  catch (const std::ios_base::failure&) {
    std::cout << "Can't open or read data from " << fn << std::endl;
  }
  catch (const std::bad_alloc&) {
    std::cout << "Can't allocate memory for data" << std::endl;
  }
 
  return 0;
}
```

c++ 런타임이 모든 자원 해제를 알아서 해주기 때문에 에러 코드에 자원 해제 코드를 추가하지 않아도 된다. OpenAndRead()가 상당히 간단해졌다.  

예외를 사용하는 경우에 다음과 같은 문제도 해결할 수 있다. 만약에 에러가 발생한 원인을 알고 싶지만 OpenAndRead를 호출하는 시점이 멀리 떨어져 있다면 어떻게 원인을 알 수 있는지 생각할 수 있다. 다음과 같은 상황에서는 원인을 파악할 수 없다.

```cpp
int
OpenAndRead(char* fn, char* mode, char** data)
{
  *data = NULL;
  char* buf = NULL;
  FILE* fp = fopen(fn, mode);
  if (!fp) {
    printf("Can't open %s\n", fn);
    return -1;
  }
 
  buf = malloc(BUF_SZ);
  if (!buf) {
    printf("Can't allocate memory for data\n");
    fclose(fp);
    return -2;
  }
 
  if (!fgets(buf, BUF_SZ, fp)) {
    printf("Can't read data from %s\n", fn);
    free(buf);
    fclose(fp);
    return -3;
  }
 
  *data = buf;
 
  return 0;
}
 
char* foo(char* fn, char* mode)
{
  char* data = 0;
  if (OpenAndRead(fn, mode, &data) >= 0)
    return data;
  else
    return 0;
}
 
void bar()
{
  ...... // fn과 mode를 얻어낸다
  char* data = foo(fn, mode);
  if (data) {
    ...... // 정상 처리
  }
  else {
    ...... // bar()가 뭘 할 수 있을까요?
  }
}
```

OpenAndRead()는 반환 코드를 통해서 에러를 구분하려 하지만 foo()에서 무시하여 bar()는 무슨 일이 일어났는지 알 수 없다. bar()가 할 수 있는 일은 에러 메시지를 출력하고 종료하거나 에러를 무시할 수 밖에 없다. 이런 문제는 예외를 사용하여 해결할 수 있다. 호출 경로상의 어떤 함수가 예외를 처리하지 않고 무시하면 된다. OpenAndRead()와 foo()는 예외를 어떻게 처리해야 할지 모른다고 가정하고, bar()는 처리할 수 있다고 가정한다면 다음과 같이 구현된다.  

```cpp
char*
OpenAndRead(const std::string& fn, std::ios_base::openmode mode)
{
  std::fstream fs(fn.c_str(), mode); // std::ios_base::failure() 가 발생할지 모릅니다.
  boost::scoped_ptr buf(new char[BUF_SZ+1]);  // std::bad_alloc() 가 발생할지 모릅니다
  fs.getline(buf.get(), BUF_SZ);
  return buf.get();
}
 
char* foo(char* fn, char* mode)
{
  // 아무런 예외도 잡지 않고 있습니다
  return OpenAndRead(fn, mode);
}
 
void bar()
{
  ...... // fn과 mode를 알아냅니다
  try {
    char* data = foo(fn, mode);
  }
  catch (const std::ios_base::failure&) {
    std::cout << "Can't open or read data from " << fn << std::endl;
    // 왜 fn 열어서 데이터를 읽으려고 했는지를 안다면 좀 더 정확한 진단 메시지를 출력하고,
    // 사용자에게 뭔가 조치를 취하라고 알려줄 수 있습니다
  }
  catch (const std::bad_alloc&) {
    std::cout << "Can't allocate memory for data" << std::endl;
    // 이런 경우를 대비해 메모리를 미리 따로 마련해 놓았다면 bad_alloc()를 
    // 좀 더 부드럽게 처리할 수도 있을 것입니다.
  }
}
```

이제는 OpenAndRead()와 foo()가 다시 간단해졌고, bar()만 에러 처리에 관여하고 있다. 그리고 예외가 무엇을 의미하는지 알고 충분한 상황 정보가 있기 때문에 예외를 잘 처리할 수 있다.  

또 한가지 예외의 장점은 객체라는 점이다. 객체이기 때문에 많은 정보를 담아서 발생시키면 정확한 에러 상황을 알 수 있다. 예외 처리기는 그 정보들이 접근하여 어떤 일이 벌어지고 있는지 정확히 알아내는데 도움이 된다.  
