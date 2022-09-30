---
layout: default
title: Scene
nav_order: 3
parent: SwiftUI
grand_parent: Swift
---

* [Scene](#scene)
* [ScenePhase](#scenephase)
    * [1. Inside a View instance](#1-inside-a-view-instance)
    * [2. Inside a App instance](#2-inside-a-app-instance)
    * [3. Inside a Scene instance](#3-inside-a-scene-instance)
    * [Getting scene phase](#getting-scene-phase)
* [WindowGroup](#windowgroup)
  * [Declaration](#declaration)
  * [Overview](#overview)
* [DocumentGroup](#documentgroup)
  * [Value type documents](#value-type-documents)
  * [Reference type documents](#reference-type-documents)
  * [Read and write configurations](#read-and-write-configurations)
* [Settings](#settings)
  * [Declaration](#declaration-1)
  * [Overview](#overview-1)

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


# WindowGroup

동일한 구조의 Window 그룹을 표시하는 Scene이다.

## Declaration

```swift
struct WindowGroup<Content> where Content : View
```

## Overview

App에서 View 계층 구조의 컨테이너로 사용한다. 그룹 컨텐츠로 선언한 계층 구조는 App이 해당 Group에서 생성하는 각 Window에 대한 템플릿 역할을 한다.  

```swift
@main
struct Mail: App {
    var body: some Scene {
        WindowGroup {
            MailViewer() // Declare a view hierarchy here.
        }
    }
}
```

SwiftUI는 특정 플랫폼별 동작을 처리한다. 예를 들어 macOS 및 iPadOS와 같은 플랫폼에서 사용자는 그룹에서 동시에 둘 이상의 창을 열 수 있다. macOS에서 사용자는 탭 인터페이스에서 열린 창을 함께 모을 수 있다. 또한 macOS에서 창 그룹은 표준 창 관리를 위한 명령을 자동으로 제공한다.  

그룹에서 생성된 모든 창은 독립적인 상태를 유지한다. 그룹에서 생성된 각각의 새 창에 대해 시스템은 Scene의 View 계층 구조에 의해 인스턴스화된 State 또는 StateObject 변수에 대해 새 저장소를 할당한다.  

일반적으로 문서 기반의 앱이 아니라면 WindowGroup을 사용하고, 맞다면 DocumentGroup을 사용한다.  

Hashable과 Codable을 모두 준수하는 지정된 유형의 데이터를 표시하도록 WindowGroup을 선택적으로 정의할 수 있다. openWindow 동작과 함께 사용하면 그룹에 대한 Window가 열리고 루트 View는 제시된 값에 대한 바인딩을 전달한다. 바인딩이 표시되는 것과 동일한 값을 갖는 창이 이미 존재하는 경우 해당 창이 대신 앞으로 표시된다. 바인딩 값은 State 복원을 위해 유지되고 Window가 복원될 때 디코딩된다. 그러면 바인딩이 디코딩된 값으로 설정된다. 디코딩 과정에서 오류가 발생하면 바인딩이 기본값 또는 nil로 설정된다. 일반적으로 보여주는 값은 가벼운 데이터를 사용하는 것이 좋다. Identifiable을 준수하는 구조화된 모델 값의 경우 값의 식별자가 잘 작동한다. 다음 예시는 새 창에서 지정된 메모 항목을 여는 버튼을 정의한다.  

```swift
@main
struct Notes: App {
    var body: some Scene {
        ...
        WindowGroup(for: Note.ID.self) { $noteID in
            ...
        }
    }
}

struct NewNoteWindow: View {
    var note: Note
    @Environment(\.openWindow) private var openWindow

    var body: some View {
        Button("Open Note In New Window") {
            openWindow(value: note.id)
        }
    }
}
```

# DocumentGroup

DocumentGroup은 문서를 열고 편집하기 위한 사용자 인터페이스를 생성하는 Scene 타입이다. 문서 데이터 모델과 문서 내용을 표시하는 뷰 계층 구조를 초기화한다. [FileDocument](https://developer.apple.com/documentation/swiftui/filedocument?changes=latest_minor) 프로토콜을 준수하는 값 모델을 사용하거나 [ReferenceFileDocument](https://developer.apple.com/documentation/swiftui/referencefiledocument?changes=latest_minor) 프로토콜을 준수하는 참조 모델을 사용할 수 있다. 문서 기반 앱에서 필요한 다중 창 지원, 패널 열기 및 저장, 드래그 앤 드롭과 같은 표준 동작을 지원한다.  

## Value type documents

- [protocol FileDocument](https://developer.apple.com/documentation/swiftui/filedocument?changes=latest_minor) : 문서를 직렬화하는데 사용되는 문서 모델  
- [struct FileDocumentConfiguration](https://developer.apple.com/documentation/swiftui/filedocumentconfiguration?changes=latest_minor) : 열린 파일 문서의 속성  

## Reference type documents

- [protocol ReferenceFileDocument](https://developer.apple.com/documentation/swiftui/referencefiledocument?changes=latest_minor) : 참조 유형 문서를 직렬화하는데 사용되는 문서 모델  
- [struct ReferenceFileDocumentConfiguration](https://developer.apple.com/documentation/swiftui/referencefiledocumentconfiguration?changes=latest_minor) : 열린 참조 파일 문서의 속성  

## Read and write configurations

- [struct FileDocumentReadConfiguration](https://developer.apple.com/documentation/swiftui/filedocumentreadconfiguration?changes=latest_minor) : 파일 읽기
- [struct FileDocumentWriteConfiguration](https://developer.apple.com/documentation/swiftui/filedocumentwriteconfiguration?changes=latest_minor) : 파일 쓰기


# Settings

Settings는 앱의 설정을 보거나 수정하기 위한 Scene이다.  

## Declaration

```swift
struct Settings<Content> where Content : View
```

## Overview

App 프로토콜을 사용하여 앱을 선언할 때 SwiftUI가 View를 앱의 설정대로 관리하도록 한다. 다음 예시는 macOS에서만 설정 Scene을 컴파일한다.  

```swift
@main
struct MyApp: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
        #if os(macOS)
        Settings {
            SettingsView()
        }
        #endif
    }
}
```

View를 Settings Scene의 인수로 전달하면 SwiftUI가 앱의 설정 메뉴 항목을 활성화한다. SwiftUI는 사용자가 Settings를 나타나게 하거나 사라지게 할 수 있도록 관리한다. 다음은 설정뷰에서 AppStorage 값을 변경하는 예시이다. 설정을 하나의 View에서 정의하거나 TabView를 사용하여 다른 컬렉션으로 그룹화할 수 있다.  

```swift
struct GeneralSettingsView: View {
    @AppStorage("showPreview") private var showPreview = true
    @AppStorage("fontSize") private var fontSize = 12.0

    var body: some View {
        Form {
            Toggle("Show Previews", isOn: $showPreview)
            Slider(value: $fontSize, in: 9...96) {
                Text("Font Size (\(fontSize, specifier: "%.0f") pts)")
            }
        }
        .padding(20)
        .frame(width: 350, height: 100)
    }
}
```

```swift
struct SettingsView: View {
    private enum Tabs: Hashable {
        case general, advanced
    }
    var body: some View {
        TabView {
            GeneralSettingsView()
                .tabItem {
                    Label("General", systemImage: "gear")
                }
                .tag(Tabs.general)
            AdvancedSettingsView()
                .tabItem {
                    Label("Advanced", systemImage: "star")
                }
                .tag(Tabs.advanced)
        }
        .padding(20)
        .frame(width: 375, height: 150)
    }
}
```
