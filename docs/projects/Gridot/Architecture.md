---
layout: default
title: Architecture
nav_order: 2
parent: Gridot
grand_parent: Projects
permalink: /docs/projects/Architecture
---

* [Architecture](#architecture)
	* [아키텍처 선택](#아키텍처-선택)
		* [선택 기준](#선택-기준)
	* [참조 링크](#참조-링크)

# Architecture

## 아키텍처 선택

프로젝트에 맞는 아키텍처를 선택하기 위해서 검색해보니 다음과 같은 패턴들을 찾게 되었다.  

1. MVC (Model-View-Controller)
2. MVVM (Model-View-ViewModel)
3. VIPER (View-Interactor-Presenter-Entity-Router)
4. TCA (The Composable Architecture)


### 선택 기준

- SwiftUI는 선언적이며 상태 중심의 프레임 워크이다. View에 바인딩 되어 있는 State를 변경하여 View를 업데이트 한다.  
- 테스트를 위해 아키텍처가 희생되지 않아야 한다.
- 
- 다양한 아키텍처가 있지만 모두 `관심사 분리`라는 하나의 목적을 가지고 있다. 그리고 `의존성 규칙`으로 아키텍처가 기능하게 된다. 소스 코드의 의존성은 반드시 저수준에서 고수준으로 행해야한다. 이를 통해서 업무 로직(고수준 정책)은 세부 사항(저수준 정책)의 변경에 영향을 받지 않도록 할 수 있다.  








## 참조 링크
- [SwiftUI에서 MVVM 사용을 멈춰야 하는가?](https://green1229.tistory.com/267)  
- [iOS 아키텍처 패턴 VIPER](https://bugle.tistory.com/48)  
- [Composable Architecture](https://green1229.tistory.com/138)  
- [The Clean Architecture 번역](https://blog.coderifleman.com/2017/12/18/the-clean-architecture/)  
- [주니어 개발자의 클린 아키텍처 맛보기](https://techblog.woowahan.com/2647/)  
- [SwiftUI를 위한 클린 아키텍처](https://gon125.github.io/posts/SwiftUI%EB%A5%BC-%EC%9C%84%ED%95%9C-%ED%81%B4%EB%A6%B0-%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98/)  

