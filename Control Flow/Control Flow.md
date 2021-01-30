# Control Flow  

[Swift Docs](https://docs.swift.org/swift-book/LanguageGuide/ControlFlow.html)

Some users might want fewer tick marks in their UI. They could prefer one mark every 5 minutes instead. Use the stride(from:to:by:) function to skip the unwanted marks.  
> 몇몇 사용자는 UI에 더 적은 눈금 표시를 원할 수 있습니다. 대신 5 분마다 한 마크를 선호 할 수 있습니다. stride (from : to : by :) 함수를 사용하여 원하지 않는 표시를 건너 뜁니다.  

```swift
let minutes = 60
let minuteInterval = 5
for tickMark in stride(from: 0, to: minutes, by: minuteInterval) {
// render the tick mark every 5 minutes (0, 5, 10, 15 ... 45, 50, 55)
}
```  

Closed ranges are also available, by using stride(from:through:by:) instead:  
> 대신 stride (from : through : by :)를 사용하여 닫힌 범위를 사용할 수도 있습니다.  

```swift
let hours = 12
let hourInterval = 3
for tickMark in stride(from: 3, through: hours, by: hourInterval) {
// render the tick mark every 3 hours (3, 6, 9, 12)
}
```

## Tuples

You can use tuples to test multiple values in the same switch statement. Each element of the tuple can be tested against a different value or interval of values. Alternatively, use the underscore character (_), also known as the wildcard pattern, to match any possible value.
The example below takes an (x, y) point, expressed as a simple tuple of type (Int, Int), and categorizes it on the graph that follows the example.

> 튜플을 사용하여 동일한 switch 문에서 여러 값을 테스트 할 수 있습니다. 튜플의 각 요소는 다른 값 또는 값 간격에 대해 테스트 할 수 있습니다. 또는 와일드 카드 패턴이라고도하는 밑줄 문자 (_)를 사용하여 어떠한 값이든 match할 수 있습니다. 아래 예제는 (Int, Int) 유형의 간단한 튜플로 표현 된 (x, y) 포인트를 가져 와서 예제를 따르는 그래프에서 분류합니다.

```swift
let somePoint = (1, 1)
switch somePoint {
case (0, 0):
print("\(somePoint) is at the origin")
case (_, 0):
print("\(somePoint) is on the x-axis")
case (0, _):
print("\(somePoint) is on the y-axis")
case (-2...2, -2...2):
print("\(somePoint) is inside the box")
default:
print("\(somePoint) is outside of the box")
}
// Prints "(1, 1) is inside the box"
}
```

## Value Bindings

A switch case can name the value or values it matches to temporary constants or variables, for use in the body of the case. This behavior is known as value binding, because the values are bound to temporary constants or variables within the case’s body.  
> 스위치 케이스는 케이스 본문에서 사용하기 위해 임시 상수 또는 변수와 일치하는 값의 이름을 지정할 수 있습니다. 값이 케이스 본문 내의 임시 상수 또는 변수에 바인딩되기 때문에이 동작을 값 바인딩이라고합니다.

The example below takes an (x, y) point, expressed as a tuple of type (Int, Int), and categorizes it on the graph that follows:
> 아래 예제는 (Int, Int) 유형의 튜플로 표현 된 (x, y) 포인트를 가져 와서 다음 그래프에서 분류합니다.

```swift
let anotherPoint = (2, 0)
switch anotherPoint {
case (let x, 0):
print("on the x-axis with an x value of \(x)")
case (0, let y):
print("on the y-axis with a y value of \(y)")
case let (x, y):
print("somewhere else at (\(x), \(y))")
}
// Prints "on the x-axis with an x value of 2"
```

A switch case can use a where clause to check for additional conditions. The example below categorizes an (x, y) point on the following graph:  
> 스위치 케이스는 where 절을 사용하여 추가 조건을 확인할 수 있습니다. 아래 예는 다음 그래프에서 (x, y) 점을 분류합니다.

```swift
let yetAnotherPoint = (1, -1)
switch yetAnotherPoint {
case let (x, y) where x == y:
print("(\(x), \(y)) is on the line x == y")
case let (x, y) where x == -y:
print("(\(x), \(y)) is on the line x == -y")
case let (x, y):
print("(\(x), \(y)) is just some arbitrary point")
}
// Prints "(1, -1) is on the line x == -y"
```

## Checking API Availability

Swift has built-in support for checking API availability, which ensures that you don’t accidentally use APIs that are unavailable on a given deployment target.
> Swift에는 API 가용성 확인을위한 기본 지원 기능이있어 특정 배포 대상에서 사용할 수없는 API를 실수로 사용하지 않도록합니다.

```swift
if #available(iOS 10, macOS 10.12, *) {
// Use iOS 10 APIs on iOS, and use macOS 10.12 APIs on macOS
} else {
// Fall back to earlier iOS and macOS APIs
}
```

In its general form, the availability condition takes a list of platform names and versions. You use platform names such as iOS, macOS, watchOS, and tvOS—for the full list, see Declaration Attributes. In addition to specifying major version numbers like iOS 8 or macOS 10.10, you can specify minor versions numbers like iOS 11.2.6 and macOS 10.13.3.  

> 일반적인 형식에서 가용성 조건은 플랫폼 이름 및 버전 목록을 사용합니다. iOS, macOS, watchOS 및 tvOS와 같은 플랫폼 이름을 사용합니다. 전체 목록은 선언 속성을 참조하세요. iOS 8 또는 macOS 10.10과 같은 주 버전 번호를 지정하는 것 외에도 iOS 11.2.6 및 macOS 10.13.3과 같은 부 버전 번호를 지정할 수 있습니다.

```swift
if #available(platform name version, ..., *) {
//statements to execute if the APIs are available
} else {
//fallback statements to execute if the APIs are unavailable
}
```

