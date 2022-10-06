---
layout: default
title: TCA
nav_order: 1
parent: ExternalLibrary
grand_parent: Swift
---

* [The Composable Architecture](#the-composable-architecture)
* [References](#references)

# The Composable Architecture

[swift-composable-architecture github](https://github.com/pointfreeco/swift-composable-architecture)  

TCA(The Composable Architecture)는 상태 관리 기반 아키텍처이다. 합성, 테스팅과 인체 공학을 염두해두었으며, SwiftUI와 UIKit을 지원하여 모든 애플 플랫폼에서 사용 가능하다. TCA를 통해 기능을 만들기 위해서 도메인을 구성하는 몇가지 타입을 정의해야 한다.  

- State : 비즈니스 로직을 수행하거나 UI를 그릴 때 필요한 데이터를 나타내는 타입이다.
- Action : 유저의 행동이나 알림 등 어플리케이션에서 발생할 수 있는 모든 행동을 나타내는 타입이다.
- Environment : API 클라이언트나 애널리틱스 클라이언트와 같이 어플리케이션이 필요로하는 의존성을 가지고 있는 타입이다.
- Reducer : 어떤 Action이 주어졌을 때, 지금 State를 다음 State로 변화시키는 방법을 가지는 함수이다. Reducer는 보통 실행할 수 있는 Effect 값을 반환한다.
- Store : 실제로 기능이 작동하는 공간이다. Store는 사용자 Action을 보내서 Reducer와 Effect를 실행할 수 있고, Store에서 일어나는 State 변화를 observe하여 UI를 업데이트할 수도 있다. 

TCA 동작 흐름은 다음과 같이 그려진다.  






# References

- [TCA_README_KR](https://gist.github.com/pilgwon/ea05e2207ab68bdd1f49dff97b293b17)
- [Riiid의 Swift Composable Architecture](https://medium.com/riiid-teamblog-kr/riiid%EC%9D%98-swift-composable-architecture-231a665e5f47) 
