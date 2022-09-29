# WindowGroup

동일한 구조의 Window 그룹을 표시하는 Scene이다.

## 선언

```swift
struct WindowGroup<Content> where Content : View
```

## 개요

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
