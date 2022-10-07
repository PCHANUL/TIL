---
layout: default
title: String, Character
parent: Swift
nav_order: 1
---


* [String, Character](#string-character)
	* [String Indices](#string-indices)
	* [Inserting and Removing](#inserting-and-removing)
	* [Substrings](#substrings)


# String, Character 

문자열은 "Hello word"와 같이 문자들이 연속되어 있다. Swift의 문자열은 `String` 타입으로 표현되며 `Character` 값의 집합을 포함하고 있다. 문자열은 언제나 독립적인 유니코드 문자로 구성되며, 다양한 유니코드 문자들을 지원한다. Swift의 `String` 타입은 구문이 단순하며 빠르고 현대적인 문자열을 구현하고 있다.  

## String Indices
Swift에서는 문자열의 인덱스를 표현하기 위해 `String.Index` 라는 타입을 사용한다. 문자들이 저장하는 메모리 양이 다를 수 있으며, 어떤 `Character` 가 특정 위치에 있는지 결정하기 위해서 시작이나 끝으로 각 유니코드 스칼라를 반복한다. 그러므로 Swift 문자열은 정수형 값으로 인덱스될 수 없다.
 ```swift
 let str = "abc"
 
 print(str[str.startIndex])	// "a"
 print(str[0])	// Error
 ```

 - 문자열의 첫번째 위치와 마지막 위치에 접근하기 위해 `startIndex`와 `endIndex`를 사용한다.  
 - 주어진 인덱스의 앞과 뒤에 접근하기 위해 `index(before:)`와 `index(after:)`를 사용한다.  
 - 주어진 인덱스로부터 떨어진 접근하기 위해서는 `index(_:offsetBy:)` 함수를 사용하면 된다.  
 - 그리고 서브스크립트 문법에 주어진 인덱스를 사용하여 문자에 접근할 수 있다.

 ```swift
 let str = "Hello world"
 
 print(str.startIndex)	// "H"
 print(str.endIndex)	// "d"
 
 print(str.index(after: str.startIndex)	// "e"
 print(str.index(after: str.endIndex))	// "l"
 
 print(str.index(str.startIndex, offsetBy: 6))	// "w"
 ```

## Inserting and Removing

문자열의 삽입 메소드에는 `insert(_:at:)`와 `insert(contentsOf:at:)`가 있다.    

```swift
var welcome = "hello"
welcome.insert("!", at: welcome.endIndex)
// welcome now equals "hello!"

welcome.insert(contentsOf: " there", at: welcome.index(before: welcome.endIndex))
// welcome now equals "hello there!"
```

문자열의 제거 메소드에는 `remove(at:)`와 `removeSubrange(_:)`가 있다.  

```swift
welcome.remove(at: welcome.index(before: welcome.endIndex))
// welcome now equals "hello there"

let range = welcome.index(welcome.endIndex, offsetBy: -6)..<welcome.endIndex
welcome.removeSubrange(range)
// welcome now equals "hello"
```

> `RangeReplaceableCollection` 프로토콜을 준수하는 모든 타입에서 위의 4가지 메소드를 사용할 수 있다. 즉, `Array, Dictionary, Set`과 같은 컬렉션 타입에서 같은 메소드를 사용할 수 있다.  

## Substrings
 문자열에서 부분 문자열을 얻고자 할때 `Substring`의 인스턴스가 된다. Swift에서 부분 문자열은 대부분 문자열과 같은 메소드를 가지고 있으며, 문자열을 작업하는 같은 방법으로 부분 문자열을 작업할 수 있다. 문자열과 부분 문자열의 다른점은 성능 최적화이다. 부분 문자열은 원본 문자열의 저장소를 재사용하기 때문에 수정되기 전까지 메모리에 복사하는 성능 비용을 지불하지 않는다. 부분 문자열이 사용되는 동안은 전체 원본 문자열은 반드시 메모리에 유지되어야 한다. 아래의 그림은 `String`과 `Substring`의 관계를 보여준다.

 ![substring](/TIL/docs/src/string01.png)

 ```swift
 let greeting = "Hello, world!" 
 let index = greeting.firstIndex(of: ",") ?? greeting.endIndex 
 let beginning = greeting[..<index] // "Hello" 
 
 // 부분 문자열을 문자열로 변환하여 메모리에 저장
 let newString = String(beginning)
 ```

[참조]  
[문자열과 문자](https://kka7.tistory.com/140)  
[문자열 다루기](http://seorenn.blogspot.com/2018/05/swift-string-index.html)  

