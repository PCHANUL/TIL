---
layout: default
title: TCA
nav_order: 1
parent: ExternalLibrary
grand_parent: Swift
---

* [The Composable Architecture](#the-composable-architecture)
	* [Composability : Pullback](#composability--pullback)
	* [Functionality : Higher-order reducer](#functionality--higher-order-reducer)
	* [Purity](#purity)
* [Example](#example)
* [References](#references)

# The Composable Architecture

[github](https://github.com/pointfreeco/swift-composable-architecture)  
[forums](https://forums.swift.org/c/related-projects/swift-composable-architecture/61)  

TCA(The Composable Architecture)는 상태 관리 기반 아키텍처이다. 합성, 테스팅과 인체 공학을 염두해두었으며, SwiftUI와 UIKit을 지원하여 모든 애플 플랫폼에서 사용 가능하다. TCA를 통해 기능을 만들기 위해서 도메인을 구성하는 몇가지 타입을 정의해야 한다.  

- State : 비즈니스 로직을 수행하거나 UI를 그릴 때 필요한 데이터를 나타내는 타입이다.
- Action : 유저의 행동이나 알림 등 어플리케이션에서 발생할 수 있는 모든 행동을 나타내는 타입이다.
- Environment : API 클라이언트나 애널리틱스 클라이언트와 같이 어플리케이션이 필요로하는 의존성을 가지고 있는 타입이다.
- Reducer : 어떤 Action이 주어졌을 때, 지금 State를 다음 State로 변화시키는 방법을 가지는 함수이다. Reducer는 보통 실행할 수 있는 Effect 값을 반환한다.
- Store : 실제로 기능이 작동하는 공간이다. Store는 사용자 Action을 보내서 Reducer와 Effect를 실행할 수 있고, Store에서 일어나는 State 변화를 observe하여 UI를 업데이트할 수도 있다. 

이러한 타입을 정의하면 크고 복잡한 기능을 작고 독립된 모듈로 쪼갤 수 있다. 독립된 모듈은 서로 결합 가능하며 이해하기 쉽고, 테스트하기 쉽다. 작은 단위의 기능은 각각의 도메인에서 State, Action, Environment, Reducer를 정의하기 때문에 큰 도메인으로 합치기 위해서는 타입이 일치되어야 한다.  

## Composability : Pullback

TCA 라이브러리에서 제공하는 pullback 오퍼레이터는 큰 Reducer를 작은 Reducer로 분해하는 중요한 역할을 한다. `combine` 연산자를 사용하면 작은 도메인에서 작업하는 `Reducer`를 정의한 후에 큰 도메인에서 동작하는 커다란 Reducer로 결합할 수 있다.  

```swift
// Global domain that holds a local domain:
struct AppState { 
	var settings: SettingsState, 
	/* rest of state */ 
}

struct AppAction { 
	case settings(SettingsAction), 
	/* other actions */ 
}

struct AppEnvironment { 
	var settings: SettingsEnvironment, 
	/* rest of dependencies */ 
}

// A reducer that works on the local domain:
let settingsReducer = Reducer<SettingsState, SettingsAction, SettingsEnvironment> { ... }

// Pullback the settings reducer so that it works on all of the app domain:
let appReducer: Reducer<AppState, AppAction, AppEnvironment> = .combine(
  settingsReducer.pullback(
	state: \\.settings,
	action: /AppAction.settings,
	environment: { $0.settings }
  ),
  /* other reducers */
)
```

## Functionality : Higher-order reducer

higher-order function은 함수를 인자로 받고 리턴하는 함수이다. reducer를 higher-order function으로 구현하면 이벤트 트래킹, 로깅, 에러 핸들링 등의 목적으로 reducer의 코드 변경 없이 기능을 추가할 수 있다. 예를 들어서 state 변화와 실행된 action을 콘솔에 출력하기위해 logging 이라는 higher-order reducer를 다음과 같이 구현할 수 있다.  

```swift
extension Reducer {
	public func logging() -> Reducer {
		return .init { state, action, environment in
			print("State :", state)
			print("Action :", action)
			return self.run(&state, action, environment)
		}
	}
}
```

## Purity

Composable Architecture의 가장 큰 원칙 중 하나는 사이드 이펙트가 절대 직접적으로 실행되지 않고, `Effect` 타입에 감싼 후에 리듀서에 반환되고 나중에 스토어에서 실행된다는 것이다. 이는 데이터 플로우를 간결하게 하는데 가장 중요한 내용이다. 이 원칙을 따라야 사용자의 행동과 이펙트 실행 사이의 사이클에 대한 테스트 가능성을 보장받을 수 있다.  

Reducer는 environment로 주입된 기능 실행에 대한 사이드 이펙트를 핸들링하는 명세만 작성되어 있을 뿐 실제 런타임은 Store에서 subscription을 통해 동작한다. 사이드 이펙트가 Store에서 발생되기 때문에 Reducer에 적절한 environment를 주입하면 사이드 이펙트 없이 주입된 값에 의해서만 결과가 결정된다. 그렇기 때문에 Reducer가 항상 동일한 결과를 보장하여 테스트를 작성하기 쉬워진다.  

# Example

다음은 숫자를 증가/감소시키는 버튼과 함께 숫자를 표시하는 UI를 구현하는 예시이다.  
이 기능을 구현하기 위해 `ReducerProtocol`을 준수하는 기능의 도에민과 동작을 수용할 새 유형을 만든다.  

```swift
import ComposableArchitecture

struct Feature: ReducerProtocol {
}
```

여기에 기능의 state를 위한 타입을 정의해야 한다. 현재 count를 정수로 구성하고, 알람으로 보여줄 제목을 문자열로 구성한다.  

```swift
struct Feature: ReducerProtocol {
	struct State: Equatable {
		var count = 0
		var numberFactAlert: String?
	}
}
```

이제는 기능의 action들을 정의해야 한다. 감소 버튼, 증가 버튼 또는 팩트 버튼을 누르는 것과 같은 명백한 동작이 있다. 그러나 알람을 해제하거나 API 요청에서 응답을 수신할 때 발생하는 작업과 같이 약간 명확하지 않은 작업도 있다.  

```swift
struct Feature: ReducerProtocol {
	struct State: Equatable {
		var count = 0
		var numberFactAlert: String?
	}
	enum Action: Equatable {
		case factAlertDismissed
		case decrementButtonTapped
		case incrementButtonTapped
		case numberFactButtonTapped
		case numberFactResponse(TaskResult<String>)
	}
}
```

그리고 기능을 위한 실제 로직과 동작을 다루는 `reduce` 메소드를 구현한다. 이는 현재 state를 다음 state로 어떻게 변경시키는지 설명하고, 어떤 effect를 실행시켜야 하는지 설명한다. effect를 실행시키지 않아도 되는 action은 `.none`을 반환한다.  

```swift
struct Feature: ReducerProtocol {
  struct State: Equatable { … }
  enum Action: Equatable { … }
  
  func reduce(into state: inout State, action: Action) -> EffectTask<Action> {
    switch action {
      case .factAlertDismissed:
        state.numberFactAlert = nil
        return .none

      case .decrementButtonTapped:
        state.count -= 1
        return .none

      case .incrementButtonTapped:
        state.count += 1
        return .none

      case .numberFactButtonTapped:
        return .task { [count = state.count] in
          await .numberFactResponse(
            TaskResult {
              String(
                decoding: try await URLSession.shared
                  .data(from: URL(string: "http://numbersapi.com/\(count)/trivia")!).0,
                as: UTF8.self
              )
            }
          )
        }

      case let .numberFactResponse(.success(fact)):
        state.numberFactAlert = fact
        return .none

      case .numberFactResponse(.failure):
        state.numberFactAlert = "Could not load a number fact :("
        return .none
    }
  }
}
```

그리고 이제 마지막으로 기능을 표시하는 view을 정의한다. view는 `StoreOf<Feature>`을 유지한다. state의 모든 변경 사항을 관찰하고 다시 렌더링할 수 있고, state가 변경되도록 모든 유저 action을 store로 보낼 수 있다.  

```swift
struct FeatureView: View {
  let store: StoreOf<Feature>

  var body: some View {
    WithViewStore(self.store, observe: { $0 }) { viewStore in
      VStack {
        HStack {
          Button("−") { viewStore.send(.decrementButtonTapped) }
          Text("\(viewStore.count)")
          Button("+") { viewStore.send(.incrementButtonTapped) }
        }

        Button("Number fact") { viewStore.send(.numberFactButtonTapped) }
      }
      .alert(
        item: viewStore.binding(
          get: { $0.numberFactAlert.map(FactAlert.init(title:)) },
          send: .factAlertDismissed
        ),
        content: { Alert(title: Text($0.title)) }
      )
    }
  }
}

struct FactAlert: Identifiable {
  var title: String
  var id: String { self.title }
}
```

view를 표시할 준비가 되었다면 store를 구성할 수 있다. 앱의 진입점에 구성한다면 시작할 때, 초기 state와 앱에 공급될 reducer를 지정하여 수행할 수 있다.  

```swift
import ComposableArchitecture

@main
struct MyApp: App {
  var body: some Scene {
    WindowGroup {
      FeatureView(
        store: Store(
          initialState: Feature.State(),
          reducer: Feature()
        )
      )
    }
  }
}
```




# References

- [TCA_README_KR](https://gist.github.com/pilgwon/ea05e2207ab68bdd1f49dff97b293b17)
- [Riiid의 Swift Composable Architecture](https://medium.com/riiid-teamblog-kr/riiid%EC%9D%98-swift-composable-architecture-231a665e5f47) 
