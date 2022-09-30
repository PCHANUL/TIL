---
layout: default
title: SwiftUI Lifecycle
nav_order: 1
parent: SwiftUI
grand_parent: Swift
---

* [SwiftUI Lifecycle](#swiftui-lifecycle)
  * [SwiftUI App Protocol : 새로운 시작점](#swiftui-app-protocol--새로운-시작점)
  * [Scene의 라이프 사이클 상태](#scene의-라이프-사이클-상태)
  * [AppDelegate를 SwiftUI App Protocol과 함께 사용하는 방법](#appdelegate를-swiftui-app-protocol과-함께-사용하는-방법)

# SwiftUI Lifecycle 

SwiftUI는 iOS 14부터 UIKit에서 벗어나는 새로운 라이프 사이클을 가지게 되었다. UIKit의 AppDelegate와 SceneDelegate를 사용하는 대신에 App Protocol과 SceneBuilder, scenePhase enumerator, UIApplicationDelegateAdapter가 제공된다. 먼저 SceneDelegate를 빠르게 훑어본 다음에 새로운 라이프 사이클에 대해서 살펴본다.  

SceneDelegate는 iPadOS의 다중 창 지원을 해결하기 위해 iOS 13에 도입되었다. 이로 인해 window 개념에서 scene 개념으로 전환되며 AppDelegate의 역할이 분리되었다.  

iOS 14부터 새로운 SwiftUI 애플리케이션 프로젝트를 생성할 때 `SwiftUI App Lifecycle`과 `UIKit App Delegate` 중에서 선택할 수 있게 되었다. 후자는 예전과 같이 AppDelegate와 SceneDelegate 보일러 플레이트 코드와 UIHostingController가 SwiftUI View를 포함하는데 사용된다. 하지만 전자는 완전히 새롭게 SwiftUI를 시작할 수 있게 한다.  

## SwiftUI App Protocol : 새로운 시작점

```swift
@main
struct ProjectName: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}
```

위의 코드는 짧지만 많은 작업이 자동으로 이루어진다. 기본적으로 구조체는 Scene과 View을 한 곳에 통합하여 SwiftUI 애플리케이션 계층 구조에 대한 조감도를 제공한다. 코드를 작성하는데 주의할 점은 다음과 같다.  

- `@main` 속성은 위의 구조체가 SwiftUI 애플리케이션의 시작점임을 나타낸다.  
- App protocol을 준수하며 하나 이상의 scene을 구성하기위한 SceneBuilder를 구현해야 한다. 또한, App protocol은 SwiftUI 애플리케이션을 시작하는 main() 메서드를 트리거하는 역할을 한다.  
- 위의 예에서 WindowGroup은 SwiftUI view를 감싸는 컨테이너 scene이다. iPadOS나 macOS는 위의 애플리케이션을 실행하며 여러 WindowGroup scene을 생성하는 기능을 지원한다.  

WindowGroup 외에도 문서 기반 앱의 DocumentGroup이나 Settings와 WKNotificationScene와 같은 scene 타입을 사용할 수 있다.  

복잡한 Scene을 구성할 때는 속성을 @SceneBuilder로 반드시 설정해야 한다. 예를 들어 아래의 코드는 macOS 플랫폼에 대한 환경 설정 메뉴 장면을 생성한다.  

```swift
@main
struct SwiftUITestApp: App {
	@SceneBuilder var body: some Scene {
		WindowGroup {
			ContentView()
		}
		
		#if os(macOS)
			Settings {
				ContentView()
			}
		#endif
	}
}
```

## Scene의 라이프 사이클 상태

SceneDelegate에서와 같이 scene의 라이프 사이클 업데이트를 수신하기 위해서 scenePhase 열거자를 사용한다. scenePhase는 @Environment 속성 래퍼를 사용하여 가져오고, onChange(of:) 수정자를 사용하여 변경사항을 받을 수 있다.

```swift
import SwiftUI

@main
struct TestApp: App {
    @Environment(\.scenePhase) var scenePhase

    var body: some Scene { 
        WindowGroup {
            ContentView()
        }.onChange(of: scenePhase) { newScenePhase in
            switch newScenePhase {
            case .active:
              print("App is active")
            case .inactive:
              print("App is inactive")
            case .background:
              print("App is in background")
            @unknown default:
              print("Oh - interesting: I received an unexpected new value.")
            }
          }
    }
}
```

- App is active : 앱이 켜진 직후
- App is inactive : 앱 스위칭 할 수 있게 올린 상태
- App is in baskgroud : 아이폰 홈 화면으로 나온 상태
- App is inactive & App is active : 앱 재진입시

## AppDelegate를 SwiftUI App Protocol과 함께 사용하는 방법

AppDelegate 클래스는 애플리케이션의 중요한 부분이다. 알림 처리를 하든지 Firebase 처리를 하든지 다양한 수명 주기 방식으로 중요한 역할을 한다.  

AppDelegate 기능을 연결하기 위해서 UIKit을 SwiftUI 애플리케이션으로 가져와야 한다. SwiftUI는 새로운 속성 래퍼인 UIApplicationDelegateAdaptor를 제공한다. 이를 통해 AppDelegate를 SwiftUI 구조 안에 주입시킬 수 있다.  

```swift
class AppDelegate: NSObject, UIApplicationDelegate {
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey : Any]? = nil) -> Bool {
        return true
    }
}

@main
struct MyApp: App {

    @UIApplicationDelegateAdaptor(AppDelegate.self) var appDelegate

    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}
```

추가로 AppDelegate를 사용하지 않고 초기화 단계에서 작업을 처리하는 방법이 있다. 아래의 코드는 앱 최초 실행 시에 Firebase 모듈을 초기화한다.  

```swift
import SwiftUI
import Firebase

@main
struct SwiftUITest: App {
  
    init() {
        FirebaseApp.configure()
    }

    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}
```

---

- [SwiftUI’s New App Lifecycle and Replacements for AppDelegate and SceneDelegate in iOS 14](https://betterprogramming.pub/swiftuis-new-app-lifecycle-and-replacements-for-appdelegate-and-scenedelegate-in-ios-14-c9cf4a2367a9)  
- [SwiftUI App lifecycle 정리](https://huniroom.tistory.com/entry/iOS14SwfitUI-SwiftUI-life-cycle-%EC%97%90%EC%84%9C-%EB%94%A5%EB%A7%81%ED%81%AC-%EC%B2%98%EB%A6%AC)  
- [AppDelegate를 대신해 모듈을 초기화하는 방법](https://maart.tistory.com/71)  
