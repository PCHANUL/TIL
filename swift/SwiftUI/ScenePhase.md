# ScenePhase

Scene의 작동 상태를 나타낸다. 시스템은 장면의 작동 상태를 반영하는 단계를 통해 앱의 Scene 인스턴스를 이동한다. 단계 변화을 트리거하여 활용할 수 있다. 다음 예제는 Environment에서 scenePhase 값을 관찰하여 현재 단계를 읽는다.

```swift
@Environment(\.scenePhase) private var scenePhase
```

값을 해석하는 방법은 값을 읽은 위치에 따라 다르다.  

### 1. Inside a View instance
View가 포함된 Scene의 단계를 반영하는 값을 얻는다. 다음 예제는 onChange 메서드를 사용하여 해당 View를 둘러싼 Scene이 ScenePhase.active 단계에 들어갈 떄마다 타이머를 활성화하고 다른 단계에 들어갈 때 타이머를 비활성한다.  

```swift
struct MyView: View {
    @ObservedObject var model: DataModel
    @Environment(\.scenePhase) private var scenePhase

    var body: some View {
        TimerView()
            .onChange(of: scenePhase) { phase in
                model.isTimerRunning = (phase == .active)
            }
    }
}
```

### 2. Inside a App instance
모든 Scene의 단계를 반영하는 집계 값을 얻는다. App은 활성화된 Scene이 있는 경우 ScenePhase.active 값을 보고하고, 아닌 경우에는 ScenePhase.inactive 값을 보고한다. 여기에는 단일 Scene 선언에서 생성된 여러 Scene 인스턴스도 포함된다. 다음 예제는 WindowGroup에서 ScenePhase.backgroud 단계에 들어가면 앱이 곧 종료된다고 예상할 수 있다. 그렇기 때문에 이때 모든 리소스를 해제할 수 있다.  

```swift
@main
struct MyApp: App {
    @Environment(\.scenePhase) private var scenePhase

    var body: some Scene {
        WindowGroup {
            MyRootView()
        }
        .onChange(of: scenePhase) { phase in
            if phase == .background {
                // Perform cleanup when all scenes within
                // MyApp go to the background.
            }
        }
    }
}
```

### 3. Inside a Scene instance
custom Scene 인스턴스에서 읽는다면 값은 custom Scene을 구성하는 모든 scene의 집계를 유사하게 반영한다.  

```swift
struct MyScene: Scene {
    @Environment(\.scenePhase) private var scenePhase

    var body: some Scene {
        WindowGroup {
            MyRootView()
        }
        .onChange(of: scenePhase) { phase in
            if phase == .background {
                // Perform cleanup when all scenes within
                // MyScene go to the background.
            }
        }
    }
}
```

### Getting scene phase
> 
> - `case active` : scene이 앞에 있으며 반응하고 있다.
> - `case inactive` : scene이 앞에 있지만 작업이 중단되어 있다. 
> - `case background` : scene의 현재 UI가 보이지 않는다. 
