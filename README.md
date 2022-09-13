# Today I Learned

[![Gitbook](https://img.shields.io/badge/Gitbook-chanul.gitbook.io/til-blue.svg?style=for-the-badge\&logo=gitbook)](https://chanul.gitbook.io/til/)

<hr>

## 22.09.13 : safe index, Generic
> 
> [Handling Index Out of Range Exception the Swift Way](https://www.vadimbulavin.com/handling-out-of-bounds-exception/)  
> 
> 인덱스로 배열의 요소에 접근하다보면 범위를 벗어나는 경우가 나타난다. 이를 방지하기 위해서 검사하는 코드를 다음과 같이 추가해왔다.  
> 
> ```swift
> if index >= 0 && index < array.count {
> 	print(array[index])
> }
> ```  
> 
> 이는 오류를 방지하지만 요소에 접근할 때마다 반복되어야 한다. 그러므로 Array extension에 메소드를 추가하여 구현해본다.  
> 
> ```swift 
> extension Array {
> 	func getElement(at index: Int) -> Element? {
> 		let isValidIndex = index >= 0 && index < count
> 		return isValidIndex ? self[index] : nil	
> 	}
> }
> ```  
> 
> 이 로직을 subscript로 구현하면 가독성을 높일 수 있다.  
> 
> ```swift
> extension Array {
> 	subscript(safe index: Index) -> Element? {
> 		let isValidIndex = index => 0 && index < count
> 		return isValidIndex ? self[index] : nil
> 	}
> }
> ```  
> 
> 구체적인 컬렉션에 구애받지 않도록 확장해본다.  
> 
> ```swift
> extension Collection {
> 	subscript(safe index: Index) -> Element? {
> 		return indices.contains(index) ? self[index] : nil
> 	}
> }
> ```  
> 
> 다음과 같이 보편적으로 사용할 수 있게 되었다.  
> 
> ```swift
> [1, 2, 3][safe: 4] // Array - prints 'nil'
> (0..<3)[safe: 4] // Range - prints 'nil'
> ```

> 
> # Generics
> 
> [Generics - The Swift Programming Language (Swift 5.5)](https://docs.swift.org/swift-book/LanguageGuide/Generics.html)  
> [공식문서 번역](https://minsone.github.io/mac/ios/swift-generics-summary)  
> 
> 제네릭 코드는 모든 유형에 따라 유연하고 재사용가능한 함수를 작성할 수 있게 한다. 중복을 피하고 추상적인 방식으로 명확한 의도를 표현하는 코드를 작성할 수 있다.  
> 
> swapTwoInts(_: _:)는 두 Int 값을 교환하는 일반 함수이다.  
> 
> ```swift
> func swapTwoInts(_ a: inout Int, _ b: inout Int) {
> 	let tempA = a
> 	a = b
> 	b = tempA
> }
> 
> var intA = 3
> var intB = 100
> swapTwoInts(&intA, &intB)
> print("intA = \(intA), intB = \(intB)")
> // Prints "intA = 100, intB = 3"
> ```
> 
> swapTwoInts(_: _:)는 Int 값에만 사용할 수 있다. 두 String 값이나 Double 값을 교환하려면 매개변수 값 유형이 다른 새로운 함수를 작성해야 한다.  
> 
> ```swift
> func swapTwoStrings(_ a: inout String, _ b: inout String) {
> 	let tempA = a
> 	a = b
> 	b = tempA
> }
> 
> func swapTwoDoubles(_ a: inout Double, _ b: inout Double) {
> 	let tempA = a
> 	a = b
> 	b = tempA
> }
> ```
> 
> 제네릭 함수로 작성하면 모든 유형에서 작동된다.  
> 
> ```swift
> func swapTwoValues<T>(_ a: inout T, _ b: inout T) {
> 	let tempA = a
> 	a = b
> 	b = tempA
> }
> 
> var intA = 3
> var intB = 100
> swapTwoValues(&intA, &intB)
> print("intA = \(intA), intB = \(intB)")
> // Prints "intA = 100, intB = 3"
> 
> var intC = "hello"
> var intD = "world"
> swapTwoValues(&intC, &intD)
> print("intC = \(intC), intD = \(intD)")
> // Prints "intC = world, intD = hello"
> ```
> 
> 위의 예시 함수인 swapTwoValues(_: _:)는 T가 유형 매개변수로 사용된 예시입니다. 유형 매개변수는 함수 이름 바로 뒤에 꺾쇠 괄호 쌍 사이에 작성된다.  
> 
> 유형 매개변수를 지정하면 함수의 반환 유형, 또는 함수 내에서 유형의 약어로 사용될 수 있다. 각각의 유형 매개변수는 함수가 호출될 때마다 실제 유형으로 대체된다.  
> 
> 꺾쇠 괄호 안에 여러 유형 매개변수를 쉼표로 구분하여 작성하면 둘 이상의 유형 매개변수를 사용할 수 있다.  
> 
> ```swift
> struct IntStack {
> 	var items: [Int] = []
> 	mutating func push(_ item: Int) {
> 		items.append(item)
> 	}
> 	mutating func pop() -> Int {
> 		return items.removeLast()
> 	}
> }
> ```
> 
> ```swift
> struct Stack<Element> {
> 	var items: [Element] = []
> 	mutating func push(_ item: Element) {
> 		items.append(item)
> 	}
> 	mutating func pop() -> Element {
> 		return items.removeLast()
> 	}
> }
> ```
> 
> Stack 꺽쇠 괄호 안에 유형을 작성하여 새 인스턴스를 만들면 해당 유형을 사용하게 된다.  
> 
> ```swift
> func swapTwoInts(_ a: inout Int, _ b: inout Int) {
> 	let tempA = a
> 	a = b
> 	b = tempA
> }
> 
> var intA = 3
> var intB = 100
> swapTwoInts(&intA, &intB)
> print("intA = \(intA), intB = \(intB)")
> // Prints "intA = 100, intB = 3"
> ```  
> 
> 
> ## Type Constraints(타입 제약)
> 
> 제네릭 함수와 제네릭 타입을 사용할 때 특정 타입으로 강제하면 유용한 경우가 있다. 타입 강제는 특정 클래스로부터 타입 인자가 상속받아야 하거나, 특정 프로토콜 또는 프로토콜 결합과 일치해야 한다.  
> 
> ### Type Constraint Syntax(타입 제약 문법)
> 
> ```swift
> func findIndex<T>(array: [T], valueToFind: T) -> Int? {
>     for (index, value) in enumerate(array) {
>         if value == valueToFind {
>             return index
>         }
>     }
>     return nil
> }
> ```  
> 
> 위의 함수는 제네릭 함수로써 배열에서 요소를 찾고 인덱스를 반환한다. 이는 컴파일 되지 않는다. 동등 연산자(==)로 비교될 수 없는 타입이 있기 때문이다. 만약에 클래스나 구조체가 들어온다면 같다고 추측할 수 없기 때문이다. Equatable인 타입은 동등 연산자 지원을 보장한다. 함수를 정의할 때 타입 인자를 Equatable로 다음과 같이 제약할 수 있다.  
> 
> ```swift
> func findIndex<T: Equatable>(array: [T], valueToFind: T) -> Int? {
>     for (index, value) in enumerate(array) {
>         if value == valueToFind {
>             return index
>         }
>     }
>     return nil
> }
> ```  
> 
> 단일 타입 인자가 `T: Equatable`로 정의되며, Equatable 프로토콜에 일치하는 타입 T를 의미한다.  
> 
> ## Associated Types(연관 타입)
> 
> 프로토콜 정의의 한 부분으로 하나 이상의 연관 타입을 선언하면 유용하다. 연관 타입은 자리를 표시하는 이름을 정하고, 실제 타입은 프로토콜이 도입될 때까지 정해지지 않는다.  
> 
> ### Associated Types in Action(연관 타입 사용)
> 
> ```swift
> protocol Container {
>     typealias ItemType
>     mutating func append(item: ItemType)
>     var count: Int { get }
>     subscript(i: Int) -> ItemType { get }
> }
> ```
> 
> `ItemType`이라는 연관 타입을 가지는 프로토콜이 선언되었다. `Container` 프로토콜은 append 메소드로 새로운 item을 컨테이너에 추가하고, 정수 인덱스 값으로 각 요소를 받는다. `typealias ItemType`으로 선언된 연관 타입은 다음과 같이 사용된다.  
> 
> ```swift
> struct IntStack: Container {
>     // original IntStack implementation
>     var items = [Int]()
>     mutating func push(item: Int) {
>         items.append(item)
>     }
>     mutating func pop() -> Int {
>         return items.removeLast()
>     }
>     // conformance to the Container protocol
>     typealias ItemType = Int
>     mutating func append(item: Int) {
>         self.push(item)
>     }
>     var count: Int {
>         return items.count
>     }
>     subscript(i: Int) -> Int {
>         return items[i]
>     }
> }
> ```
> 
> Swift의 타입 추론 덕분에 구체화된 타입을 선언할 필요가 없다. 그래서 다음과 같이 `typealias ItemType = Int`를 제거하고 제네릭으로 작성할 수 있다.  
> 
> ```swift
> struct Stack<T>: Container {
>     // original Stack<T> implementation
>     var items = [T]()
>     mutating func push(item: T) {
>         items.append(item)
>     }
>     mutating func pop() -> T {
>         return items.removeLast()
>     }
>     // conformance to the Container protocol
>     mutating func append(item: T) {
>         self.push(item)
>     }
>     var count: Int {
>         return items.count
>     }
>     subscript(i: Int) -> T {
>         return items[i]
>     }
> }
> ```
> 
> 
> ## Where Clauses
> 
> 타입 제약을 사용하면 타입과 연결된 타입 매개 변수에 대한 요구 사항을 정의할 수 있다.  
> 
> `where`을 사용하면 연결된 타입이 특정 프로토콜을 준수해야 하거나 특정 형식 매개변수와 연결된 형식이 같아야 한다고 요구할 수 있다. 아래의 예제는 일반 함수를 정의하여 두 인스턴스에 같은 항목이 같은 순서로 포함되어 있는지 확인한다. 검사할 두개의 컨테이너는 동일한 타입의 컨테이너일 필요는 없지만 동일한 타입의 `Item`을 보유해야 한다.  
> 
> ```swift
> func allItemsMatch<C1: Container, C2: Container>
>     (_ someContainer: C1, _ anotherContainer: C2) -> Bool
>     where C1.Item == C2.Item, C1.Item: Equatable {
> 
> 	// Check that both containers contain the same number of items.
> 	if someContainer.count != anotherContainer.count {
> 		return false
> 	}
> 
> 	// Check each pair of items to see if they're equivalent.
> 	for i in 0..<someContainer.count {
> 		if someContainer[i] != anotherContainer[i] {
> 			return false
> 		}
> 	}
> 
> 	// All items match, so return true.
> 	return true
> }
> ```
> 
> 다음 요구사항이 함수의 두 타입 매개변수에 적용된다. 
> - C1은 Container 프로토콜을 준수해야 한다. 
> - C2 또한 Container 프로토콜을 준수해야 한다. 
> - C1의 Item과 C2의 Item은 반드시 같아야 한다. 
> - C1의 Item은 Equatable 프로토콜을 준수해야 한다. 
> 
> 
> ## Extension with a Generic Where Clause
> 
> 제너릭 Where을 extension에 사용할 수 있다. 아래의 제너릭 Stack 구조체 extension에서는 isTop 메소드를 추가한다.  
> 
> ```swift
> extension Stack where Element: Equatable {
>     func isTop(_ item: Element) -> Bool {
>         guard let topItem = items.last else {
>             return false
>         }
>         return topItem == item
>     }
> }
> ```
> 
> 새로운 isTop 메소드에서 동등 연산자(==)가 사용되기 때문에 item은 `Equatable`을 준수해야 한다. 그러므로 제네릭 where을 사용하지 않는다면 컴파일되지 않는다.  
> 
> 만약에 특정 타입이 필요하다면 다음과 같이 제네릭 where을 사용할 수 있다.  
> 
> ```swift
> extension Container where Item == Double {
>     func average() -> Double {
>         var sum = 0.0
>         for index in 0..<count {
>             sum += self[index]
>         }
>         return sum / Double(count)
>     }
> }
> print([1260.0, 1200.0, 98.6, 37.0].average())
> // Prints "648.9"
> ```
> 

## 22.09.12 : UITest

> ## 페이지 객체 패턴을 사용한 UITest
> 
> [페이지 객체 패턴 UITest](https://swiftwithmajid.com/2021/03/24/ui-testing-using-page-object-pattern-in-swift/)  
> 
> 다양한 문제를 해결하기 위해서 디자인 패턴을 사용한다. 테스트 코드에도 디자인 패턴을 적용할 수 있다. 페이지 개체 패턴이라는 디자인 패턴으로 UI 테스트를 유지 가능하고 일관된 상태로 유지할 수 있다. 특정 화면의 모든 상호 작용을 정의하고 해당 화면의 UI 상태를 확인하는데 필요한 모든 기능을 제공하기 때문에 페이지 개체 패턴이라 부른다.  
> 
> 

## 22.09.11 : @Binding

> ## @Binding 변수 초기화
> 
> [SwiftUI: How to implement a custom init with @Binding variables](https://stackoverflow.com/questions/56973959/swiftui-how-to-implement-a-custom-init-with-binding-variables)  
> 
> 바인딩 매개변수를 초기화한다. 속성 래퍼인 `@Binding`을 사용하는 변수를 초기화하기 위해서는 언더바를 앞에 붙히면 된다.  
> 
> ```swift
> struct AmountView: View {
> 	@Binding var amount: Double
> 	
> 	init(withAmount: Binding<Double>) {
> 		// self.$amount = withAmount	// beta 3
> 		self._amount = withAmount	// beta 4
> 	}
> }
> ```

## 22.09.07 : 테스트 빌드

> [@testable import에 대한 고찰](https://zeddios.tistory.com/1078)  
> [UITest에서는 앱 모듈에 접근할 수 없다](https://stackoverflow.com/questions/33755019/linker-error-when-accessing-application-module-in-ui-tests-in-xcode-7-1?rq=1)  
> 
> UI Test에서는 앱 코드에 접근할 수 없으며, 별도의 프로세스로 앱 외부에서 실행된다. 이는 UI 테스트에서 앱 코드에 접근하지 못하도록 의도된 디자인이라고 한다. 앱 코드에 접근하기 원하면 Unit Test를 사용해야 한다.  
> 
> ## @testable import 
> 
> import에 @testable을 추가하면 Target에 대한 높은 접근 권한을 가지게 된다.  
> 
> ```swift
> import XCTest
> @testable import TargetName
> ```  
> 
> Unit 테스트를 작성하면 앱 코드에 접근하기 위해 다음과 같이 코드를 작성한다. Target은 앱, 번들, 프레임워크 등이 될 수 있으며 Swift에서 별도의 module로 처리된다. 그래서 각각의 Target의 코드가 public이나 open으로 작성되어있지 않으면 접근할 수 없다. Swift 클래스는 internal이 default Access level이다. 기본적으로 클래스는 외부에서 접근할 수 없도록 정의되기 때문에 모든 정의를 public으로 정해야 한다. 이는 Swift의 type safety를 저해하기 때문에 @testable을 추가하여 높은 접근 권한을 가지도록 한다. internal이나 public으로 표시된 클래스는 open처럼 사용할 수 있다.  
> 
> 
> ## Access level
> 
> [접근 한정자](https://blog.asamaru.net/2017/01/04/swift-3-access-controls/)  
> 
> ### open
> 
> 소속 모듈을 import하는 모든 모듈에서 class와 class 맴버에 접근하고, sub class를 생성하거나 메소드를 override할 수 있다. 
> 
> ### public
> 
> open과 동일한 접근을 허용하지만 외부 모듈에서 sub class 생성과 override가 제한된다. 
> 
> ### internal
> 
> 기본 접근 수준으로써 접근 한정자가 지정되지 않은 경우에 설정된다. 소속 모듈에서 사용할 수 있지만 외부 모듈에서는 접근할 수 없다.
> 
> ### fileprivate
> 
> 소속 소스 파일 내에서만 접근 가능하다.
> 
> ### private
> 
> 현재 소스를 둘러싸는 선언 내에서만 접근 가능하다. 


## 22.09.06 : 테스트 케이스 및 테스트 방법 정의
> 
> [테스트 케이스 및 테스트 방법 정의](https://developer.apple.com/documentation/xctest/defining_test_cases_and_test_methods)  
> 
> 프로젝트에 테스트를 추가하기
> - 테스트 대상 내에서 XCTestCase의 새 하위 클래스를 만든다.
> - 테스트 케이스에 하나 이상의 테스트 메소드를 추가한다.
> - 각 테스트 방법에 하나 이상의 테스트 어설견을 추가한다.
> 
> 테스트 메서드는 XCTestCase의 하위 클래스의 메서드이며, 매개 변수과 반환 값이 없고, 메소드 이름이 소문자 `test`로 시작해야 한다. 테스트 메소드는 XCTest 프레임 워크에 의해 자동으로 감지된다. 아래의 코드는 테스트 케이스와 테스트 메소드 예시이다.  
> 
> ```swift
> class TableValidationTests: XCTestCase {
> 	// 새 테이블 인스턴스에 행과 열이 없는지 테스트한다.
> 	func testEmptyTableRowAndColumnCount() {
> 		let table = Table()
> 		XCTAssertEqual(table.rowCount, 0, "Row count was not zero.")
> 		XCTAssertEqual(table.columnCount, 0, "Column count was not zero.")
> 	}
> }
> ```
> 
> 예시 코드에서는 `TableValidationTests`라는 클래스와 `testEmptyTableRowAndColumnCount()`라는 테스트 메소드를 정의했다. 이 테스트 메소드는 `Table` 인스턴스를 생성하고 `rowCount`와 `columnCount` 속성이 0인지 확인한다.  
> 
> 테스트 구성을 명확히 하려면 테스트 케이스 이름을 테스트를 요약하는 이름으로 지정하고, 테스트 메소드 이름은 실패한 테스트를 식별하는데 도움이 되도록 테스트하는 항목을 명확히하는 이름을 지정하면 좋다.  
> 
> ### Asserting Test Conditions
> 
> 코드가 예상대로 작동하는지 확인하기 위해 테스트 메소드 내에서 조건을 확인할 수 있다. XCTAssert 함수군을 사용해서 Boolean 조건, nil 또는 non-nil 값, 예상한 값, 그리고 애러 발생을 체크하면 된다.  
> 
> 


## 22.09.04 : VoiceOver와 친해지기

> [보이스오버와 친해지기[유튜브]](https://www.youtube.com/watch?v=M3JF7ZJixaY&list=PLIGFku39tFfaVByI2WGSvxhQ2ZbCbnzzL&index=1)  
> 
> 아이폰 앱을 만드는데 접근성 지원이 중요하다. 특히, 시각장애인 접근성 지원이 중요하다.  
> 아이폰의 VoiceOver 기능을 활용하자는 이야기를 꺼내면 다음과 같은 생각을 한다. 과연 시각장애인이 과연 앱을 자유롭게 사용할 수 있을까 하는 의구심과 만약에 VoiceOver 기능을 지원한다고 하면 굉장히 많은 일을 해야 한다고 생각할 수 있다. 그러나 조금씩만 신경 쓰면 지원이 가능하고, 이를 통해 더 많은 사람들에게 편의성을 제공할 수 있다면 해야 한다고 생각한다. 
> 
> 
> ## 통합 UI테스트 도입의 선행작업  
> [뱅크샐러드 통합UI테스트](https://blog.banksalad.com/tech/test-in-banksalad-ios-1/)  
> 
> 테스트를 작성하려고 시도했을 때, 마음처럼 되지 않는 경험을 할 수 있다. 이런 경우는 앱의 접근성 경험이 제대로 갖춰지지 않아서 생긴 문제일 가능성이 크다. 통합 UI테스트에서 실제로 버튼을 누르는 UITestRunner는 "접근성 트리"를 활용해서 버튼을 찾기 때문이다. 컴퓨터에는 눈이 없기 때문에 시각 장애인이 앱을 사용하는 방식과 같이 동작하는 것이다. 그렇기 때문에 통합 UI테스트를 도입하기 전에 VoiceOver로 앱을 충분히 사용할 수 있도록 작업을 해야한다. VoiceOver 지원 작업은 쉽다. UI레이어 차원의 작업이므로 비주얼한 영역에 전혀 영향을 끼치지 않는 작업이기 때문이다.  
> 
> ## 단위 테스트
> [뱅크샐러드 스펙별 단위 테스트](https://blog.banksalad.com/tech/test-in-banksalad-ios-3/)  
> 
> 테스트 코드 작성은 쉽고, 빠르게 작성할 수 있으며, 앱의 기능 대부분을 테스트할 수 있고, 장기적인 차원 뿐만 아니라 단기적인 차원에서 개발 속도를 훨씬 빠르게 만들어 준다. 뱅크샐러드에서는 최대한 단순하고 일관된 형태로 단위 테스트를 작성할 수 있는 도구들을 `TestUtility`라는 모듈에서 관리하고 있다.  
> 
> 1. BaseTestCase : 모든 테스트 케이스들의 기반이 된다. 테스트 코드들이 given, when, then의 문법으로 짜일 수 있는 구조를 제공한다.  
> 2. RxTestCase : RxSwift로 비동기 로직을 관리하기 때문에 Rx를 통한 입력과 출력을 확인하는 테스트 케이스가 대부분이다.  
> 3. EventLoggingTestCase : 로깅은 눈에 보이지 않기 때문에 문제를 인지하는데 시간이 오래걸린다. 그렇기 때문에 가장 먼저 테스트해야 한다.  
> 4. PresentationTestCase : Form 필드의 로직을 하나씩 테스트하기보다 큰 틀에서 Form 전체를 하나의 인풋으로 보고, 화면전환 여부를 하나의 아웃풋으로 보는 테스트를 작성할 수 있다.  
> 
> ### BaseTestCase
> 
> ```swift
> open class BaseTestCase: XCTestCase {
>     /// 초기에 주입받아야 할 데이터를 지정한다
>     open func given(_ task: () -> Void) {
>         task()
>     }
> 
>     /// 발생해야 할 이벤트, 또는 메소드 호출등을 실행시킵니다
>     open func when(_ task: () -> Void) {
>         task()
>     }
> 
>     /// 결과 값이 기대와 같은지 확인한다
>     open func then(_ task: () -> Void) {
>         task()
>     }
> }
> ```
> 
> ### RxTestCase
> 
> BaseTestCase를 상속받는다.  
> RxTestCase에서는 `when(observing: Observer)` 메소드가 추가된다. 매개변수인 Observer는 resultEvents에 이벤트를 전달해서 테스트의 마지막인 `then`에서는 언제나 resultEvents에 이벤트들이 쌓여있는지 검사한다. 이렇게 입력과 출력을 명확히 함으로써 가독성을 향상시키고, TestScheduler 관련 보일러플레이트 코드들을 최대한 줄여서 테스트 코드가 3줄 내외로 작성될 수 있도록 했다.  
> 
> ```swift
> /// Rx로 만들어진 이벤트 스트림을 테스트하는 테스트케이스
> open class RxTestCase<T>: BaseTestCase {
>     open var scheduler: TestScheduler!
>     open var disposeBag: DisposeBag!
>     private var resultObserver: TestableObserver<T>!
> 
>     /// 이 TestCase에서 관측하고자 하는 대상을 설정한다.
>     open var eventsToObserve: Observable<T> = .empty() {
>         didSet {
>             disposeBag = DisposeBag()
>             scheduler = TestScheduler(initialClock: 0)
>             resultObserver = scheduler.createObserver(T.self)
>             eventsToObserve.bind(to: resultObserver).disposed(by: disposeBag)
>         }
>     }
> 
>     open func when(observing events: Observable<T>, _ task: () -> Void) {
>         self.eventsToObserve = events
>         task()
>         executeEvents()
>     }
> 
>     // 이 테스트에서 처리해야 할 입력을 편하게 생성하도록 한다.
>     open func createEvents<U>(_ events: [Recorded<Event<U>>], to relay: PublishRelay<U>) {
>         scheduler.createHotObservable(events)
>             .bind(to: relay)
>             .disposed(by: disposeBag)
>     }
> 
>     open func createEvents<U>(_ events: [Recorded<Event<U>>], to subject: PublishSubject<U>) {
>         scheduler.createHotObservable(events)
>             .bind(to: subject)
>             .disposed(by: disposeBag)
>     }
> 
>     open func executeEvents(advanceTo futureTime: VirtualTimeScheduler<TestSchedulerVirtualTimeConverter>.VirtualTime = 10) {
>         scheduler.start()
>         scheduler.advanceTo(futureTime)
>         scheduler.stop()
>     }
> 
>     /// 테스트의 결과. 테스트의 마지막에는 , 이 값에 기대한 값이 들어있는지 확인한다.
>     open var resultEvents: [Recorded<Event<T>>] {
>         resultObserver.events
>     }
> }
> ```
> 
> ```swift
> class SampleRxTestCase: RxTestCase<String> {
> 
>     func test버튼하나만_누르면_동작_안함() {
>         given {
>             viewModel = SampleViewModel(data: "Hello")
>         }
> 
>         when(observing: viewModel.log) {
>             createEvents([.next(0, Void())], to: viewModel.aButtonClicked)
>         }
> 
>         then {
>             XCTAssert(resultEvents.isEmpty)
>         }
>     }
> 
>     func test버튼_두개를_눌러야_동작함() {
>         given {
>             viewModel = SampleViewModel(data: "World")
>         }
> 
>         when(observing: viewModel.log) {
>             createEvents([.next(0, Void())], to: viewModel.aButtonClicked)
>             createEvents([.next(1, Void())], to: viewModel.bButtonClicked)
>         }
> 
>         then {
>             XCTAssert(resultEvents == [.next(1, "World")])
>         }
>     }
> } 
> ```
> 
> ### EventLoggingTestCase
> 
> 로깅을 뱅크샐러드에서는 `EventLogger` 라는 도구를 활용해 기록한다. 이 도구에서 이벤트를 기록했는지 여부를 테스트코드에서 관측할 수 있도록 돕는 `eventObserver` 속성을 추가했다. 그러므로 EventLogging 관련 코드를 테스트 할 때, 출력은 언제나 eventObserver의 내용이다.  
> 코드는 간단하지만 이벤트 코드에 대한 테스트의 중요성을 프로젝트 차원에서 강조하기 위해 EventLoggingTestCase를 만들었다. 또한, 검색을 통해서 관련 예제를 빠르게 찾아볼 수 있다는 효과도 있다.  
> 
> ```swift
> open class EventLoggingTestCase: RxTestCase<(name: String, properties: [String: Any]?)> {
>     open override func setUpWithError() throws {
>         try super.setUpWithError()
>         EventLogger.eventObserver = PublishSubject<(name: String, properties: [String: Any]?)>()
>     }
> 
>     open override func when(_ task: () -> Void) {
>         eventsToObserve = EventLogger.eventObserver
>         task()
>         executeEvents()
>     }
> }
> ```
> 

## 22.09.03 : Test
> 
> [뱅크샐러드 iOS팀이 숨쉬듯이 테스트코드 짜는 방식](https://blog.banksalad.com/tech/test-in-banksalad-ios-3/)  
> [Testing & Debugging](https://minosaekki.tistory.com/40)  
> 
> # TDD를 향해서
> 
> 테스트 코드는 내가 만들어야 하는 코드를 작성하도록 강제하기 때문에 구현 전에 작성되어야 진가를 발휘한다. 그러나 TDD로 iOS 앱을 개발하려니 감이 잡히지 않는다. 처음부터 모든 것을 TDD로 개발할 필요는 없다. 테스트하기 쉬운 영역이 분명히 존재하며, 그 영역에서 시작하여 TDD로 구현하는 연습을 할 수 있다. 점점 영역을 확장하다보면 어느새 대부분의 스펙들을 TDD로 작성 할 수 있게 될 것이다.  
> 
> # 테스트 파일 생성
> 
> ```swift
> import XCTest
> 
> class Tests_iOS: XCTestCase {
> 
>     override func setUpWithError() throws {
>         // 여기에 설정 코드를 입력한다. 이 메서드는 클래스의 각 테스트 메서드를 호출하기 전에 호출된다.
> 
>         // UI 테스트에서는 일반적으로 오류가 발생하면 즉시 중지하는 것이 가장 좋다.
>         continueAfterFailure = false
> 
>         // UI 테스트에서는 테스트를 실행하기 전에 테스트에 필요한 초기 상태(예: 인터페이스 방향)를 설정하는 것이 중요한다. setUp 메소드는 이를 수행하기에 좋은 위치입니다.
>     }
> 
>     override func tearDownWithError() throws {
>         // 여기에 분해 코드를 넣으십시오. 이 메서드는 클래스의 각 테스트 메서드를 호출한 후에 호출된다.
>     }
> 
>     func testExample() throws {
>         // UI 테스트는 테스트하는 애플리케이션을 시작해야 한다.
>         let app = XCUIApplication()
>         app.launch()
> 
>         // XCTAssert 및 관련 함수를 사용하여 테스트가 올바른 결과를 생성하는지 확인하십시오.
>     }
> 
>     func testLaunchPerformance() throws {
>         if #available(macOS 10.15, iOS 13.0, tvOS 13.0, watchOS 7.0, *) {
>             // 애플리케이션을 시작하는 데 걸리는 시간을 측정한다.
>             measure(metrics: [XCTApplicationLaunchMetric()]) {
>                 XCUIApplication().launch()
>             }
>         }
>     }
> }
> ```
> 
> - 파일에서 import하는 XCTest 프레임워크는 default 테스팅 라이브러리를 포함하고 있다. 
> - Tests_iOS 클래스는 XCTestCase를 상속하고 있다. 
> - Tests_iOS 클래스 안에 있는 defualt 메서드 중에 `setUpWithError`와 `tearDownWithError` 메서드는 테스트 과정에서 중요한 역할을 한다.
> - `setUpWithError`는 클래스의 `test` 메서드가 호출되기 전에 호출되어 초기 상태를 설정한다.
>   - 테스트 전에 앱이 특정 상태에 있는지 확인하고 변경된 사항을 되돌릴 수 있다. 
>   - `continueAfterFailure` 값을 `false`로 설정하면 첫번째 실패 이후에 테스트 과정을 멈추게 한다. 
> - `tearDownWithError`는 `test` 메서드가 완료될 때마다 호출된다. 
> - 테스트 메서드 이름은 반드시 `test`로 시작해야 프레임 워크가 해당 테스트를 진행한다. 
> 
> 


