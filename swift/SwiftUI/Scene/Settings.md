# Settings

Settings는 앱의 설정을 보거나 수정하기 위한 Scene이다.  

## 선언

```swift
struct Settings<Content> where Content : View
```

## 개요

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
