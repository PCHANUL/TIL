# iomanip

iomanip(Input Output Manipulator)는 입출력 조정자로 cout의 출력을 조정할 수 있다. 사용 함수는 다음과 같다.

<details>
<summary>setw : 출력 폭을 정한다. </summary>

다음 스트림 요소의 너비를 설정하며 지정하려는 각 요소 앞에 삽입해야 한다.  
setw가 설정하는 출력 폭은 최소 값이다. 그러므로 데이터가 지정한 폭보다 길다면 지정된 폭은 무시되고 데이터의 길이에 맞추어진다.  

```cpp
#include <iomanip>

T6 setw(streamsize Wide);

// Wide : 표시 필드의 너비
// 반환값 : 조작자는 스트림에서 추출하거나 스트림 str에 삽입할 때 호출 str.width(Wide)한 다음 반환하는 개체를 반환한다. 

```

```cpp
int	val = 12345;

std::cout << "val:" << val << std::endl;
std::cout << "val:" << setw(10) << val << std::endl;
std::cout << "val:" << setw(3) << val << std::endl;

// val:12345
// val:     12345
// val:12345
```
</details>

<details>
<summary>setfill : 공백을 채우는데 사용할 문자를 설정한다.</summary>

```cpp
#include <iomanip>

template <class Elem>
T4 setfill(Elem Ch);

// Ch : 오른쪽 맞춤된 디스플레이에서 공백을 채우는데 사용할 문자입니다.
// 반환값 : 조작자는 스트림에서 추출하거나 스트림 str에 삽입할 때 호출 str.fill(Ch)한 다음 반환하는 개체를 반환한다. 형식 Elem은 스트림 str의 요소 형식과 동일해야 한다.

```
</details>  
