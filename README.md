# Today I Learned

[![Gitbook](https://img.shields.io/badge/Gitbook-chanul.gitbook.io/til-blue.svg?style=for-the-badge\&logo=gitbook)](https://chanul.gitbook.io/til/)

* 매일 배우고 기록한다.
* 새로운 개발 지식을 찾았다면 쌓아둔다.
* 쌓아둔 개발 지식은 이해하기 쉽게 정리한다.

<hr>


### 22.05.19 : Context Switching
>
>## 프로세스
>컴퓨터에서 실행중인 프로그램을 의미한다. 
>프로그램은 코드와 리소스 등이 집합된 파일을 말하고, 
>프로세스는 메모리 상에서 프로그램이 실행되는 작업 단위를 말한다.
>
>## 프로세스의 상태
>
>커널 내에는 준비 큐, 대기 큐, 실행 큐등의 자료 구조가 있으며 커널은 이들을 이용하여 프로세스의 상태를 관리한다.
>	- 생성: 프로세스 생성
>	- 실행: 프로세스가 cpu를 차지하여 명령어들이 실행되고 있다.
>	- 준비: 프로세스가 cpu를 사용하지 않지만 사용할 수 있는 상태로 할당되기를 기다린다. 준비 상태의 프로세스 중에서 우선순위가 높은 프로세스가 cpu를 할당받는다.
>	- 대기: 프로세스가 어떤 사건을 기다리고 있는 상태이다. 
>	- 종료: 프로세스의 실행이 종료
>
>## 컨텍스트 스위칭
>
>하나의 프로세스가 cpu를 사용 중인 상태에서 다른 프로세스가 cpu를 사용하기 위해서 이전 프로세스의 상태를 보관하고 새로운 프로세스의 상태를 적재하는 작업을 말한다.
>
>	- cpu에 존재하는 레지스터들은 현재 실행중인 프로세스 관련 데이터들로 채워진다. 
>	- 실행 중인 프로세스가 변경되면 레지스터들의 값이 변경된다.
>	- 변경되기 전에 이전 프로세스를 나중에 이어서 실행하기 위해 데이터들을 저장해야 한다.
>	- 그리고 새로 실행되는 프로세스가 아니라면 이전에 실행될 때 레지스터들이 지니고 있던 데이터들을 불러와서 이어서 실행해야 한다.
>
>컨텍스트 스위칭과정은 시스템에 많은 부담을 주며, 레지스터의 개수가 많을수록, 데이터 종류가 많을수록 더 부담이 된다.
>컨텍스트 스위칭에 소요되는 시간을 줄이려면 저장하고 복원하는 컨텍스트 정보의 개수를 줄여야 한다.
>
>## 스레드의 컨텍스트 스위칭
>
>프로세스가 컨텍스트 스위칭이 일어나면 모든 데이터를 무너뜨리고 새로운 데이터를 쌓아야 한다.
>하지만 스레드는 공유하는 데이터의 영역이 많기 때문에 빠르게 컨텍스트 스위칭된다.
>

### 22.05.17 : Mutex
>
>mutex는 동기화를 달성하는 방법 중 하나이다. 공유 자원에 접근할 때에 lock, 끝났을 때에 unlock하므로 어떤 스레드는 먼저 들어간 스레드가 임계 구역에서 나오기 전까지 대기해야한다. 앞서 들어간 스레드가 unlock하며 임계 구역에서 나오면 대기하고 있던 스레드가 lock하며 임계 구역에 들어간다. 이처럼 접근 제어가 필요한 공간을 공간에 진입할 수 있는 시간을 제어하는 방식으로 동기화를 달성한다.
>
>```c
>// mutex 키 생성
>
>void *thread_one(void)	// 스레드 1 함수
>{
>	// 키 잠금
>	// 공유 자원에 접근
>	// 키 잠금 해제
>}
>
>void *thread_two(void)	// 스레드 2 함수
>{
>	// 키 잠금 (thread_one에서 이미 잠갔으므로 해체될 때까지 대기)
>	// 공유 자원에 접근
>	// 키 잠금 해제
>}
>
>void main(void)
>{
>	// thread_one 함수를 실행하는 스레드를 생성
>	// thread_two 함수를 실행하는 스레드를 생성
>}
>```
>
>### deadlock
>
>deadlock, 교착 상태는 다중 프로그래밍의 주요 난점 중 하나로써 두 개 이상의 작업이 상대방의 작업이 끝나기만을 기다리고 있는 상태로 아무것도 완료되지 못하는 상태를 말한다. 교착 상태가 일어나려면 다음과 같은 네 가지 조건을 충족시켜야 한다.
>
>	- 상호 배제: 프로세스들이 필요로 하는 자원에 대해 배타적인 통제권을 요구한다.
>	- 점유 대기: 프로세스가 할당된 자원을 가진 상태에서 다른 자원을 기다린다.
>	- 비선점: 프로세스가 어떤 자원의 사용을 끝낼 때까지 그 자원을 뺏을 수 없다.
>	- 순환 대기: 각 프로세스는 순환적으로 다음 프로세스가 요구하는 자원을 가지고 있다.
>
>교착 상태은 위의 조건 중 한 가지라도 만족하지 않으면 발생하지 않는다. 다음과 같은 방법으로 교착 상태를 예방할 수 있다.
>
>	- 상호 배제 부정: 여러 개의 프로세스가 공유 자원을 사용할 수 있도록 한다.
>	- 점유 대기 부정: 프로세스가 실행되기 전 필요한 모든 자원을 할당한다.
>	- 비선점 부정: 자원을 점유하고 있는 프로세스가 다른 자원을 요구할 때 점유하고 있는 자원을 반납하고 요구한 자원을 사용하기 위해 기다리도록 한다.
>	- 순환 대기 부정: 자원에 고유한 번호를 할당하고 순서대로 자원을 요구하도록 한다.  
> 
> 

### 22.05.16 : Thread
>
>응용 프로그램을 실행하면 코드는 코드 데이터 영역, 전역 변수는 데이터 영역, 지역 변수는 스택, 동적 할당은 힙 영역으로 나뉘어 메모리에 저장된다. 별도의 프로세스를 이용하면 메모리와 이외의 것들을 개별 공간에 가지게 되므로 높은 비용이 들고, 자원을 공유하기 어렵다. 그래서 하나의 프로세스에 여러개의 스레드를 만들어 사용한다.
>
>멀티 프로세스와 멀티 스레드는 모두 여러 흐름을 동시에 진행시키지만 차이점이 있다.
>	- 멀티 프로세스는 별개의 메모리를 차지하지만 멀티 스레드는 메모리를 공유하여 사용할 수 있다.
>	- 멀티 스레드는 각각의 스레드 중에 어떤 것이 먼저 실행되는지 순서를 알 수 없다.
>
>스레드의 종류
>	- 사용자 레벨 스레드
>		- 사용자 레벨의 라이브러리를 통해 구현된다.
>		- 스레드가 중단되면 나머지 모든 스레드가 중단된다.
>	- 커널 레벨 스레드
>		- 운영체제가 지원하는 스레드 기능으로 구현된다.
>		- 스레드가 도중에 중단되어도 커널은 다른 스레드를 중단시키지 않고 계속 실행시켜준다.
>
>	- 기본 데이터
> 		- 고유한 스레드 ID, 프로그램 카운터, 레지스터 집합, 스택
>		- 코드, 데이터, 파일 등 기타 자원은 프로세스 내의 다른 스레드와 공유한다.
>	- 특정 데이터
>		- 하나의 스레드에만 연관된 데이터로 개별 스레드만의 자료 공간이 필요하다.
>		- 특별한 경우에 사용하는 개별 스레드만의 자료 공간
>  
>	- 임계 구역
> 		 - 병렬 컴퓨팅에서 둘 이상의 스레드가 동시에 접근해서는 안되는 공유 자원을 접근하는 코드의 일부를 말한다. 어떤 스레드가 임계 구역에 들어가고자 한다면 대기해야 할 수도 있다. 스레드가 공유자원의 사용을 보장받기 위해서 임계 구역에 들어가거나 나올때 동기화 매커니즘이 사용된다.
>
>	- 스레드 동기화 방법
>  - mutex (locking 매커니즘)
>    - 스레드의 동시 접근을 허용하지 않고, 뮤텍스를 가지고 있어야 임계영역에 들어갈 수 있다.
>    - 임계역역에 들어간 스레드가 뮤텍스를 이용하여 본인이 나오기 전까지 다른 스레드가 들어오지 못하게 잠근다.
>    - wait를 호출한 스레드만이 signal을 호출할 수 있다.
>  - semaphore (signaling 매커니즘)
>    - 뮤텍스와 비슷하지만 동시 접근 동기화가 아닌 접근 순서 동기화에 관련있다.
>    - 세마포어 카운터를 사용하며, wait를 호출하면 카운터를 1만큼 줄인다.
>    - 카운터가 0보다 작거나 같아질 경우에 락이 실행된따.
>    - 만약에 카운터를 1로 설정하면 mutex처럼 사용할 수 있다.


### 22.05.14 : Swift Unit Test
>
>Unit Test는 소스 코드에서 특정 모듈이 의도대로 정확하게 작동하는지 테스트하는 과정이다. 함수와 메소드에 대한 테스트 케이스를 작성하면 변경된 코드에서 발생한 문제를 빠르게 파악할 수 있다. 테스크 케이스는 서로 분리되어야 한다. 이를 위해 가짜 객체(Mock object)를 생성하는 것도 좋은 방법이다.
>
>한마디로 Unit Test는 "의도대로 정확히 동작하는지 확인하는 절차"이다.
>
>Xcode에서는 프로젝트를 생성하며 Unit Test를 포함시킬 수 있다.
>![unit test](src/unitTest01.png)
>
>기존 프로젝트에 테스트를 추가할 수 있다. Navigator View의 탭에서 New Unit Test Target을 클릭하면 생성된다.
>![unit test](src/uniTest02.png)
>
>Unit Test 파일을 생성하면 다음과 같은 코드를 볼 수 있다. 가장 먼저 테스트를 진행할 프로젝트를 포함시켜준다. `@testable` 구문을 통해 프로젝트의 코드에 접근하여 사용할 수 있다. 테스트 클래스에 `setUpWithError`와 `tearDownWithError` 메서드를 볼 수 있다. 이 메서드들은 테스트 동작의 시작과 마지막에 실행되어 테스트 환경을 만들어 준다.
>
>```swift
>import XCTest
>@testable import ProjectName	// 테스트할 프로젝트를 포함시켜준다.
>
>class ProjectTests: XCTestCase {
>	override func setUpWithError() throws {
>		// 이 메서드는 클래스의 각 테스트 메서드를 호출하기 전에 호출된다.
>	}
>	
>	override func tearDownWithError() throws {
>		// 이 메서드는 클래스의 각 테스트 메서드를 호출한 후에 호출된다.
>	}
>}
>```
>
>테스트 메소드 이름은 테스트로 인식되기 위해서 반드시 test로 시작해야한다. 테스트는 given - when - then 구조로 작성하는 것이 좋다. 
>
>```swift
>func testAddTwoNums() {
>	// given - 필요한 값을 설정
>	let a = 10
>	let b = 20
>	
>	// when - 테스트 코드 실행
>	let result = addTwoNums(a, b)
>	
>	// then - 실행 결과 확인
>	XCTAssertEqual(result, 30, "Wrong calc")
>}
>```
>
>Unit Test를 실제 프로젝트에 도입하려면 모듈 분리 등 기존 코드에 많은 수정이 필요하고, 테스트 코드를 작성하는 데에도 원래 코드를 작성하는만큼의 시간이 소요될 수 있다.
>
>[Unit Test란?](https://silver-g-0114.tistory.com/142)  
>[UnitTest의 사용법](https://zeddios.tistory.com/48)

### 22.05.12 : 사용자 스토리
>
>유저, 가치, 그리고 우리가 작업할 수 있는 단위를 고민한다.
>- 유저에게 어떤 가치를 전달할 수 있는가
>- 가치를 전달하려면 무엇이 필요한가
>- 무엇을 만드는 효율을 어떻게 높일 수 있는가
>  
>우리는 무언가를 개발하여 누군가에게 가치를 전달해야 한다.
>사용자 스토리는 기능을 사용자의 관점에서 이야기하는 것이다.
>
>사용자 스토리는 '시스템 요구사항을 상세하게 설명하는 것'에서 '요구사항에 대해 이야기하는 것'으로 초점을 바꿀 수 있도록 도움을 준다. 사용자 스토리는 요구 사항을 다음과 같은 형식의 문장으로 표현된다. 
>### <사용자 유형>으로서, 나는 <어떤 욕구>를 원한다, <어떤 보상>을 받을 수 있도록
>이렇게 사용자 스토리를 작성하게 된다면 "어떻게" 대신에 "무엇"에 집중할 수 있다. 작성하는 경우에는 항상 사용자의 요구과 성취하려는 목적을 설정하고 집중해야 한다. 
>
>좋은 유저 스토리를 위해서는 잘 정의된 인수 기준이 필요하다. 인수 기준이란 유저 스토리가 수용되고, 걸러지는 기준을 말한다. 체크 리스트처럼 작동하여 만족하는 요건과 아닌 요건을 명시한다. 훌륭한 인수 기준을 위해서 준비 단계에서 기능 작동을 보여주는 방법에 대해 생각해야 한다. 인수 기준은 복잡해지고 많은 과정을 포함할 수 있지만 과다한 인수 기준이 있다면 애자일 방식이 아닐 것이다. 
>
> [사용자 스토리는 무엇인가?](https://yozm.wishket.com/magazine/detail/754/)
> 


### 22.05.08 : JavaScript Set
> ### 객체 Set
> Set 생성자를 사용하면 원시 값이나 객체 참조의 고유한 값을 저장하는 개체를 만들 수 있다. 
> ```javascript
> let obj = new Set([0])
>
> // 값 확인
> obj.has(0)	// true
> obj.has(2)	// false
> obj.size	// 1
>
> // 값 추가
> obj.add(1)	// [0, 1]
> obj.add(2)	// [0, 1, 2]
> obj.add(2)	// [0, 1, 2]
> 
> // 값 제거
> obj.delete(0)	// [1, 2]
> obj.has(0)	// false
> obj.has(1)	// true
> 
> ```
> Set 객체의 모든 값을 확인하는 방법
> ```javascript
> let obj = new Set([1, 2, 3])
>
> obj.forEach(ele => {
> 	console.log(ele)
> })
> // 1, 2, 3
> 
> let iterator = obj.values()
> console.log(iterator.next().value)	// 1
> console.log(iterator.next().value)	// 2
> console.log(iterator.next().value)	// 3
> console.log(iterator.next().value)	// undefined
> ```
> [JavaScript Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/values#try_it)

### 22.05.07 : JavaScript 반복문
> ### 반복문
> ```javascript
> // 배열
> let array = [1, 2, 3, 4, 5]
> 
> for (ele of array) {
> 	console.log(ele)	// 배열의 요소를 하나씩 출력
> }
> 
> // 객체
> let object = { a: 10, b: 20, c: 30 }
> 
> for (key in object) {
> 	console.log(key)	// 객체의 키를 하나씩 출력
> }
> ```
> [JavaScript 반복문](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Loops_and_iteration#for...of_%EB%AC%B8)
> <hr/>
> 
> ### 함수 인자 전달
> 함수에 전달되는 인자는 값이 복사되거나 주소가 복사되어 전달된다. JS에서 원시 데이터는 값이 복사되어 전달되고, 객체는 주소가 복사되어 전달된다. 배열도 객체이기 때문에 주소가 복사된다.
> ```javascript
> // 값의 복사
> function swap(a, b) {
> 	let tmp = a
> 	a = b
> 	b = tmp
> }
> 
> let a = 10
> let b = 20
> swap(a, b)
> console.log(a, b)	// 10, 20 (변경되지 않음)
>
> // 주소의 복사
> function swap_obj(c, d) {
> 	let tmp = c.val
> 	c.val = d.val
> 	d.val = tmp
> }
> 
> let c = { val: 10 }
> let d = { val: 20 }
> swap_obj(c, d)
> console.log(c.val, d.val)	// 20, 10 (변경됨)
> ```
> [JavaScript 함수 인자](https://lamarr.dev/javascript/2020/04/08/04.html)

### 22.05.06 : Swift Array, JavaScript String
> 배열 선언
> ```swift
> let numbers = [1, 2, 3, 4, 5]
> let strings = ["a", "b", "c", "d"]
> ```
> 배열 생성
> ```swift
> var doubles: [Double] = [] 
> 
> // 반복되는 문자가 포함된 배열 생성
> var digits = Array(repeating: 0, count: 10)
> print(digits) // [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
> ```
> 배열 추가
> ```swift
> var strings = ["a"]
> ```
> ```swift
> strings.append("b")
> strings.append(contentsOf: ["c", "d"])
> // ["a", "b", "c", "d"]
> string.insert("z", at: 2)
> // ["a", "b", "z", "c", "d"]
> ```
> [Swift Array](https://developer.apple.com/documentation/swift/array)
> <hr />
>
> ## JavaScript  
> 정수를 문자열로 변환하는 함수
> ```javaScript
> // toString()
> // toString(radix)
> 
> let num = 10
> 
> console.log(num.toString())  // '10'
> console.log(num.toString(2)) // '1010'
> ```
> [JavaScript number toString](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/toString)  
>
>
> 문자열을 정수로 변환하는 함수
> ```javaScript
> // parseInt(string)
> // parseInt(string, radix)
> 
> let str = '321'
> 
> console.log(parseInt(str))     // 321
> console.log(parseInt(str, 16)) // 801
> ```
> [JavaScript string parseInt](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt)

### 22.05.05 : Swift String
> 문자열의 인덱스가 문자열 길이를 초과하면 에러가 발생된다.
> ```swift
> let str = "abc"
> var idx = str.startIndex
> while (idx < str.endIndex) {
> 	idx = str.index(idx, offsetBy: 2)  // Error
> }
> ```
> 인덱스 함수에 `limitedBy` 인자를 넘겨서 문제를 해결할 수 있다. 이때는 함수가 `String.Index?` 을 반환하기 때문에 옵셔널을 처리해주어야 한다.
> ```swift
> while (idx < str.endIndex) {
> 	idx = str.index(idx, offsetBy: 2, limitedBy: str.endIndex) ?? str.endIndex
> }
> ```
> [swift String.Index](https://developer.apple.com/documentation/swift/string/1784601-index)
>
> 문자열 split  
> 특정 문자나 문자열을 기준으로 문자열을 분해하여 배열로 반환하는 함수이다.
> ```swift
>func split(separator: Character, maxSplits: Int = Int.max, omittingEmptySubsequences: Bool = true) -> [Substring]
> ```
> ```swift
> let str = "a b c"
> 
> str.split(separator: " ")
> // ["a", "b", "c"]  
> 
> str.split(separator: " ", maxSplits: 1)
> // ["a", "b c"]
> ```
> [Swift split](https://developer.apple.com/documentation/swift/string/2894564-split)  
>
> 문자열을 특정 길이로 나누는 함수를 `index` 함수로 구현할 수 있다.
> ```swift
> extension String {
>	 func split_len(_ len:Int) -> [String] {
> 		var result: [String] = []
>		var start = self.startIndex
>	
>		while (start < self.endIndex) {
>			let end = self.index(start, offsetBy: len, limitedBy: self.endIndex) ?? self.endIndex
>			result.append(String(self[start..<end]))
>			start = end
>		}
>		return result
>	}
> }
> 
> ```
>  
> 문자열 join  
> 배열의 문자열들을 하나의 문자열로 합하여 반환한다. 만약에 문자열 사이에 구분자를 넣고 싶다면 `separator` 인자를 넘겨주면 된다. 
> ```swift
> ["a", "b", "c"].joined()
> // "abc"
>
> ["a", "b", "c"].joined(separator: ", ")
> // "a, b, c"
> ```
> [Swift join](https://developer.apple.com/documentation/swift/sequence/1641243-joined) 
>

### 22.05.04 : Swift String

> #### Swift
>
> 문자열에서 특정 위치의 문자를 가져오기
>
> ```
> let str = "abc"
> let char = str[0] // Error
> ```
>
> Swift에서는 문자열의 인덱스를 표현하기 위해 `String.Index` 라는 타입을 사용한다.
>
> ```swift
> let char = str[str.startIndex] // "a"
> ```
>
> `startIndex` 라는 값을 제공하여 문자열의 첫번째 위치를 나타낸다.\
> 만약에 정수로 문자에 접근하길 원한다면 `String.Index(_:offsetBy:)` 함수를 사용하면 된다.
>
> ```swift
> let index = str.index(str.startIndex, offsetBy: 1)
> let char = str[index]  // "b"
> ```
>
> [apple doc - index(\_:offsetBy:)](https://developer.apple.com/documentation/swift/string/1786175-index)
>
> 부분 문자열을 가져오기
>
> ```swift
> let str = "abcdef"
> let start = str.index(str.startIndex, offsetBy: 1)
> let end = str.index(str.endIndex, offsetBY: -2)
> let substr = str[start...end]  // "bcde"
> ```
>
> `String.Index`로 범위를 만들고, 문자열의 subscript를 이용하여 부분 문자열을 가져올 수 있다. 그러나 `substr`의 타입은 문자열이 아니라 부분 문자열을 담고 있는 특수한 타입이다. 그러므로 문자열로 변환하여 사용해야 한다.
>
> ```swift
> let newStr = String(substr)
> ```
>
> [문자열 다루기](http://seorenn.blogspot.com/2018/05/swift-string-index.html)

### 22.05.03 : bash (if), diff

> #### bash 문법
>
> 조건문
>
> ```bash
> if [ 조건 ]
> then
> 	# true
> else
> 	# false
> fi
> ```
>
> [if문](https://ansan-survivor.tistory.com/539)
>
> diff 명령어\
> 파일을 비교하는 명령어
>
> ```bash
> diff [옵션] file1 file2
> ```
>
> ```bash
> diff file1 file2
>
> # 1c2   
> # < aaaa
> # ---
> # > bbbb
> ```
>
> ```bash
> diff -u file1 file2
>
> # --- file1       2022-05-03 21:01:24.000000000 +0900
> # +++ file2       2022-05-03 21:01:30.000000000 +0900
> # @@ -1 +1 @@
> # -aaaa
> # +bbbb
> ```
>
> 파일 정보가 두 줄로 출력되는데 file1은 `-` 로 표시되고, file2는 `+` 로 표시된다는 내용이다. `@@ -1 +1 @@`는 `-` (file1)의 1 line과 `+` (file2)의 1 line의 내용이라는 뜻이다.
>
> [diff 사용법](https://muyu.tistory.com/entry/diff-%EC%82%AC%EC%9A%A9%EB%B2%95)

### 22.05.02 : bash (read)

> #### bash 문법
>
> 파일의 내용을 한 줄씩 가져오기
>
> ```bash
> path="/path/to/filename"
> while IFS= read -r line
> do
> 	command1 on $line
> 	command2 on $line
> 	..
> 	....
> 	commandN
> done < "$path"
> ```
>
> [while..do..done bash loop](https://bash.cyberciti.biz/guide/While\_loop)
>
> 함수 사용하기
>
> ```bash
> # function의 파라미터는 $n
> function test {
> 	echo $1
> 	echo $2
> }
> test "hello" "world"
> ```
>
> [function bash](https://www.fun25.co.kr/blog/bash-function-example/?category=005)

### 22.05.01 : gitbook

> 깃북과 깃허브를 동기화하여 TIL 페이지 생성
>
> * 동기화가 완료되면 깃허브에 마크다운 파일이 생성되고, 마크다운 파일을 수정하여 깃북을 업데이트할 수 있다.
