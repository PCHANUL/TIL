---
layout: default
title: Today I Learned
nav_order: 1
has_children: true
child_nav_order: desc
permalink: /
---

# Today I Learned <!-- omit in toc -->

---

- [23.06.26](#230626)
  - [Flutter, React Native](#flutter-react-native)
- [23.06.22](#230622)
  - [Flutter](#flutter)
    - [Scaffold](#scaffold)
- [23.06.21](#230621)
  - [Flutter](#flutter-1)
    - [runApp](#runapp)
    - [Widget](#widget)
- [23.06.20](#230620)
  - [Dart](#dart)
    - [var, dynamic](#var-dynamic)
    - [nullable, non-nullable, null](#nullable-non-nullable-null)
    - [final, const](#final-const)
    - [operator](#operator)
    - [List](#list)
    - [Map](#map)
    - [for loop](#for-loop)
    - [function](#function)
    - [typedef function](#typedef-function)
- [23.06.06](#230606)

## 23.06.26

### Flutter, React Native

새로운 프로젝트를 위해서 플러터와 리액트 네이티브를 비교하는 글을 찾아보았다.  

[Flutter vs React Native : 어느 것이 당신의 프로젝트에 더 좋습니까](https://appmaster.io/blog/flutter-vs-react-native-which-one-is-better-for-your-project)  
[플러터 VS 리액트 네이티브, 2023년의 승자는?](https://www.youtube.com/watch?v=Z9cCjrbTW50)  
[Flutter Forward 2023 핵심 정리](https://www.youtube.com/watch?v=sPRCMGEaydo)  

- 리액트 네이티브는 JS를 사용하고 리액트이기 때문에 바로 시작할 수 있다. 플러터는 Dart를 배워야 하지만 그리 어렵지 않다.
- 플러터는 여러 기능을 자체적으로 탑재되어 있고, 만들어진 컴포넌트를 제공한다. 하지만 리액트 네이티브는 서드파티 패키지를 설치해야 한다.
- 리액트 네이티브는 앱 업데이트를 코드 푸시를 통해 간편하게 할 수 있다. 하지만 플러터는 이 기능을 제공하지 않지만 파이어베이스의 remote config를 사용하면 가능하다.

## 23.06.22

### Flutter

#### Scaffold

https://api.flutter.dev/flutter/material/Scaffold-class.html

- Scaffold는 앱의 화면 구조를 제공한다.
- 예를 들어, `navigation bar` 또는 `bottom tab bar`와 같이 시각적인 레이아웃 구조를 제공한다.



## 23.06.21


### Flutter

```
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}
```

#### runApp

```
void runApp(Widget app)
```

- Widget은 레고 블록처럼 앱의 UI를 구성하는 요소이다.
- Widget을 만드려면 class를 만들면 된다. 

#### Widget

- Widget은 flutter SDK에 있는 core Widget에서 하나를 상속받아야 한다.
- `StatefulWidget`, `StatelessWidget`

```
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class App extends StatelessWidget { // error : Missing concrete implementation of 'StatelessWidget.build'.

}
```

- 상속받은 Widget은 구현해야하는 메서드가 정해져있다.
- StatelessWidget은 build 메서드가 반환하는 것을 화면이 띄워준다.

```
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class App extends StatelessWidget {
  
  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    throw UnimplementedError();
  }

}
```


## 23.06.20

### Dart

#### var, dynamic

- var는 선언된 타입을 변경할 수 없다.
- dynamic은 선언된 타입을 중간에 변경할 수 있다.
- 아래의 예시는 string 변수를 int로 타입을 변경한다.

```
void main() {
  dynamic name1 = 'name1';
  var name2 = 'name2';
  
  name1 = 1;
  name2 = 2; // error
}
```

#### nullable, non-nullable, null

- 변수가 null이 될 수 있는 경우에는 타입에 `?`를 붙인다.
- nullable이 아닌 변수에는 null이 지정될 수 없다.
- nullable인 변수가 null이 아님을 나타내기 위해서는 `!`를 붙인다.

```
void main() {
  String name1 = 'name1';
  
  name1 = null; // error
  
  String? name2 = 'name2';
  
  name2 = null;
  name2 = 'name2';
  
  print(name2!); // non-nullable
}
```

#### final, const

- final이나 const로 선언하면 타입을 생략할 수 있다.
- final로 변수를 선언하면 값을 변경할 수 없다.
- const도 변수의 값을 변경할 수 없다.
- final과 const의 차이는 값을 알아야하는 시점이다.
- const는 빌드 타임에 값을 알아야 하지만 final은 아니여도 된다.

```
void main() {
  final DateTime now = DateTime.now();
  
  const DateTime now2 = DateTime.now(); // error
}
```

#### operator

- `??=` : 변수가 null인 경우에 값을 변경하는 연산자

```
void main() {
  int? num = 10;
  
  num ??= 2;
  
  print(num); // 10
  
  num = null;
  num ??= 2;
  
  print(num); // 2
  
}
```

- `is TYPE` : 변수의 타입을 확인한다.

```
void main() {
  int number = 1;
  
  print(number is int); // true
  print(number is String); // false
  
  print(number is! int); // false
  print(number is! String); // true
}
```

#### List

```
void main() {
  List<int> numbers = [1, 2, 3, 4, 5];
  
  print(numbers.length); // 5
  
  numbers.add(6);
  print(numbers); // [1, 2, 3, 4, 5, 6]
  
  numbers.remove(6);
  print(numbers); // [1, 2, 3, 4, 5]
  
  print(numbers.indexOf(2)); // 1
}
```

#### Map

```
void main() {
  Map<String, String> dict = {
    'Harry Potter': '해리 포터',
    'Ron Weasley': '론 위즐리',
  };
  
  print(dict); // {Harry Potter: 해리 포터, Ron Weasley: 론 위즐리}
  
  dict.addAll({
    'Hermione Granger': '헤르미온느 그레인저',
  })
  print(dict); // {Harry Potter: 해리 포터, Ron Weasley: 론 위즐리, Hermione Granger: 헤르미온느 그레인저}
  
  dict.remove('Hermione Granger');
  print(dict); // {Harry Potter: 해리 포터, Ron Weasley: 론 위즐리}
}
```

#### for loop

```
void main() {
  List<int> numbers = [1, 2, 3, 4, 5];
  
  for (int number in numbers) {
    print(number);
  }
}
```

#### function

- optional parameter : 넘겨받지 않아도 되는 매개변수는 `[]`로 감싸고 nullable로 정의한다.

```
addNumber(int x, [int? y, int? z]) {
  print(x);
  print(y);
  print(z);
}
```

- named parameter : 이름이 정해져서 순서가 필요없는 매개변수이다. 만약에 optional 매개변수라면 required를 없애면 된다.

```
void main() {
  addNumber(x: 10, y: 20, z: 30);
  addNumber(x: 10, y: 20);
}

addNumber({
  required int x, 
  required int y, 
  int z = 30,
}) {
  print(x);
  print(y);
  print(z);
}
```

#### typedef function

- 지정된 형태의 함수를 변수에 할당한다.

```
// signature
typedef Oper = int Function(int x, int y);

int add(int x, int y) => x + y;
int subtract(int x, int y) => x - y;

void main() {
  Oper oper = add;  
  int result;
  
  result = oper(3, 1); // 4
  
  oper = subtract;
  
  result = oper(3, 1); // 2
  
  result = calc(3, 1, add); // 4
  result = calc(3, 1, subtract); // 2
}

int calc(int x, int y, Oper oper) {
  return oper(x, y);
}

```


## 23.06.06

- 픽셀 캔버스 v2
  - Scratch 계정과 연결하여 이미지를 계정에 업로드하는 기능 추가
  - 패널 UX 변경
  - 가로 기울이기


