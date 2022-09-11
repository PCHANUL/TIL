# Today I Learned

[![Gitbook](https://img.shields.io/badge/Gitbook-chanul.gitbook.io/til-blue.svg?style=for-the-badge\&logo=gitbook)](https://chanul.gitbook.io/til/)

<hr>

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
>     /// 초기에 주입받아야 할 데이터를 지정합니다
>     open func given(_ task: () -> Void) {
>         task()
>     }
> 
>     /// 발생해야 할 이벤트, 또는 메소드 호출등을 실행시킵니다
>     open func when(_ task: () -> Void) {
>         task()
>     }
> 
>     /// 결과 값이 기대와 같은지 확인합니다
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
>     /// 이 TestCase에서 관측하고자 하는 대상을 설정합니다.
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
>     // 이 테스트에서 처리해야 할 입력을 편하게 생성하도록 합니다.
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
>         // 여기에 설정 코드를 입력합니다. 이 메서드는 클래스의 각 테스트 메서드를 호출하기 전에 호출됩니다.
> 
>         // UI 테스트에서는 일반적으로 오류가 발생하면 즉시 중지하는 것이 가장 좋습니다.
>         continueAfterFailure = false
> 
>         // UI 테스트에서는 테스트를 실행하기 전에 테스트에 필요한 초기 상태(예: 인터페이스 방향)를 설정하는 것이 중요합니다. setUp 메소드는 이를 수행하기에 좋은 위치입니다.
>     }
> 
>     override func tearDownWithError() throws {
>         // 여기에 분해 코드를 넣으십시오. 이 메서드는 클래스의 각 테스트 메서드를 호출한 후에 호출됩니다.
>     }
> 
>     func testExample() throws {
>         // UI 테스트는 테스트하는 애플리케이션을 시작해야 합니다.
>         let app = XCUIApplication()
>         app.launch()
> 
>         // XCTAssert 및 관련 함수를 사용하여 테스트가 올바른 결과를 생성하는지 확인하십시오.
>     }
> 
>     func testLaunchPerformance() throws {
>         if #available(macOS 10.15, iOS 13.0, tvOS 13.0, watchOS 7.0, *) {
>             // 애플리케이션을 시작하는 데 걸리는 시간을 측정합니다.
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


