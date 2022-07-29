# Scene

Scene은 시스템에서 관리하는 라이프 사이클이 있는 App 사용자 인터페이스의 일부를 나타낸다. App 인스턴스는 포함된 Scene을 표시하고, 각 Scene은 View 계층 구조의 루트 요소 역할을 한다.  

시스템은 Scene의 타입, 플랫폼 및 컨텍스트에 따라서 다양한 방식으로 Scene을 제시한다. Scene은 전체 디스플레이, 디스플레이의 일부, 창, 창의 탭 등을 채울 수 있다. 경우에 따라 앱이 한 번에 둘 이상의 Scene 인스턴스를 표시할 수도 있다.  

뷰를 구성하는 방법과 유사하게 수정자를 사용하여 Scene을 구성한다. 예를 들어 Scene이 포함된 창의 모양을 windowStyle 수정자로 조정할 수 있다.  

App의 body에 Scene protocol을 준수하는 하나 이상의 인스턴스를 결합하여 App을 만든다. SiwftUI가 제공하는 기본 Scene을 사용하거나 다른 Scene에서 구성한 사용자 지정 Scene을 포함할 수 있다. 사용자 정의 Scene을 만들려면 Scene protocol을 준수하는 유형을 선언하면 된다.  

```swift
struct MyScene: Scene {
    var body: some Scene {
        WindowGroup {    // built-in scene
            MyRootView()
        }
    }
}
```

Scene은 사용자에게 표시하는 View 계층 구조의 컨테이너 역할을 한다. 시스템은 앱의 현재 상태에 따라 사용자 인터페이스에 View 계층 구조를 표시할 시기와 방법을 결정한다. 예를 들어 창 그룹의 경우 시스템에서 사용자가 macOS 및 iPadOS와 같은 플랫폼에 포함된 창을 생성하거나 제거할 수 있다. 다른 플랫폼에서는 동일한 뷰 계층 구조가 활성 상태일 때 전체 디스플레이를 사용할 수 있다.  

Scene 또는 해당 View 중 하나에서 환경 값을 읽어서 장면이 활성 상태인지 아니면 다른 상태인지 확인할 수 있다. 환경 속성을 사용하여 Scene의 단계를 포함하는 속성을 만들 수 있다. 

```swift
struct MyScene: Scene {
    @Environment(\.scenePhase) private var scenePhase

    // ...
}
```

Scene protocol은 Scene을 구성하는데 사용하는 기본 수정자를 제공한다. 이 수정자는 프로토콜 메서드로 정의되어 있다. 예를 들어 onChange 수정자를 사용하여 값이 변경되는 작업을 트리거할 수 있다. 아래의 예시는 창 그룹의 모든 Scene이 배경으로 이동할 때 캐시를 비운다.  

```swift
struct MyScene: Scene {
    @Environment(\.scenePhase) private var scenePhase
    @StateObject private var cache = DataCache()

    var body: some Scene {
        WindowGroup {
            MyRootView()
        }
        .onChange(of: scenePhase) { newScenePhase in
            if newScenePhase == .background {
                cache.empty()
            }
        }
    }
}
```
