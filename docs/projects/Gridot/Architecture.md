---
layout: default
title: Architecture
nav_order: 2
parent: Gridot
grand_parent: Projects
permalink: /docs/projects/Gridot/Architecture
---

* [Architecture](#architecture)
* [SwiftUI](#swiftui)
* [SwiftUI Data Flow](#swiftui-data-flow)
* [The Composable Architecture](#the-composable-architecture)
* [References](#references)

# Architecture

프로젝트에 맞는 아키텍처를 선택하기 위해 검색해보니 다양한 아키텍처가 있었다. 이들은 모두 `관심사 분리`라는 공통된 목적을 가지고 있으며 `의존성 규칙`으로 기능하게 된다. `관심사 분리`로 기능을 관심사에 따라서 독립적으로 개발한 뒤에 조합한다. 독립된 특정 기능에 집중하기 때문에 코드를 파악하기 수월하고, 기능을 변경하거나 추가하기 쉽다. 기능을 조합할 때에는 의존성이 저수준에서 고수준으로 향하는 `의존성 규칙`을 지켜야 한다. 이를 통해서 업무 로직(고수준 정책)은 세부 사항(저수준 정책)의 변경에 영향을 받지 않도록 할 수 있다.  

# SwiftUI

SwiftUI에서는 View와 데이터의 동기화가 자동으로 이루어진다. 명령형인 UIKit에서는 리액티브한 View를 구현하기 위한 아키텍처가 필요했지만 SwifUI는 View의 State가 변경되면 자동으로 View도 업데이트된다. WWDC 2019에서 SwiftUI를 발표하며 진행한 세션 중에 데이터 플로우에 대한 세션이 있었다. 이 세션을 통해 어떠한 데이터 플로우로 앱을 구현해야하는지 알 수 있었다.  

# [SwiftUI Data Flow](../../swift/SwiftUI/DataFlow)

1. Data Access as a Dependency
2. Source of Truth

두가지 개념으로 SwiftUI가 설명된다. 첫번째는 데이터 의존성이다. SwiftUI의 View가 데이터에 의존하여 데이터가 변경될 때 새로운 값으로 UI가 업데이트된다. 두번째는 하나의 원천 데이터이다. 상위의 View의 데이터를 계층의 여러 View가 참조하면 일관되게 데이터를 유지할 수 있다. 이러한 개념들을 위해 SwiftUI는 몇가지 데이터 플로우 툴을 제공한다.  

`@State`는 Property Wrapper로써 View가 State로 선언된 속성에 의존하도록 한다. 만약에 State 값이 변경되면 SwiftUI는 값에 의존하는 View 계층 구조의 부분이 업데이트된다. 그리고 State 값은 Source of Truth로써 View 계층에서 사용된다. 부모 View에서 선언된 State가 변경되면 계층에 있는 자식 View도 업데이트되지만 자식은 State 값을 변경하지 못한다. 이때 `@Binding`이라는 Property Wrapper로 State를 바인딩하여 수정할 수 있다. 자식이 부모의 State를 바인딩하여 수정하면 계층이 업데이트 될 것이다.  

결국 프레임워크가 의존성을 관리해주는 SwiftUI에서는 State만 변경하면 된다. 그래서 이는 다음과 같이 이해하기 쉬운 단방향 데이터 흐름으로 나타난다.  

![](/TIL/docs/src/projects/gridot/architecture_03.png)

# The Composable Architecture

TCA는 State를 관리하여 효율적으로 View의 변화를 관리할 수 있는 아키텍처이다. 이 아키텍처의 데이터 흐름을 보면 SwiftUI 프레임 워크에 적합하다는 사실을 알 수 있다.  

![](/TIL/docs/src/projects/gridot/architecture_02.png)  

- State : 비즈니스 로직을 수행하거나 UI를 그릴 때 필요한 데이터를 나타내는 타입이다.
- Action : 유저의 행동이나 알림 등 어플리케이션에서 발생할 수 있는 모든 행동을 나타내는 타입이다.
- Environment : API 클라이언트나 애널리틱스 클라이언트와 같이 어플리케이션이 필요로하는 의존성을 가지고 있는 타입이다.
- Reducer : 어떤 Action이 주어졌을 때, 지금 State를 다음 State로 변화시키는 방법을 가지는 함수이다. Reducer는 보통 실행할 수 있는 Effect 값을 반환한다.
- Store : 실제로 기능이 작동하는 공간이다. Store는 사용자 Action을 보내서 Reducer와 Effect를 실행할 수 있고, Store에서 일어나는 State 변화를 observe하여 UI를 업데이트할 수도 있다.  








# References
- [더 나은 객체지향 개발을 위한 아이디어: 관심사의 분리부터 제어의 역전까지](https://teamdable.github.io/techblog/SoC-to-IoC#:~:text=%ED%8A%B9%EC%A0%95%ED%95%9C%20%EA%B4%80%EC%8B%AC%EC%82%AC%EC%97%90%20%EB%94%B0%EB%9D%BC%20%EA%B8%B0%EB%8A%A5,concerns%2C%20SoC)  
- [SwiftUI에서 MVVM 사용을 멈춰야 하는가?](https://green1229.tistory.com/267)  
- [iOS 아키텍처 패턴 VIPER](https://bugle.tistory.com/48)  
- [Composable Architecture](https://green1229.tistory.com/138)  
- [TCA_README_KR](https://gist.github.com/pilgwon/ea05e2207ab68bdd1f49dff97b293b17)  
- [The Clean Architecture 번역](https://blog.coderifleman.com/2017/12/18/the-clean-architecture/)  
- [주니어 개발자의 클린 아키텍처 맛보기](https://techblog.woowahan.com/2647/)  
- [SwiftUI를 위한 클린 아키텍처](https://gon125.github.io/posts/SwiftUI%EB%A5%BC-%EC%9C%84%ED%95%9C-%ED%81%B4%EB%A6%B0-%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98/)  

