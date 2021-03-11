# Extensions

[Swift Docs](https://docs.swift.org/swift-book/LanguageGuide/Extensions.html#)  

Extensions add new functionality to an existing class, structure, enumeration, or protocol type. This includes the ability to extend types for which you don’t have access to the original source code (known as retroactive modeling). Extensions are similar to categories in Objective-C. (Unlike Objective-C categories, Swift extensions don’t have names.)
> Extension은 기존 클래스, 구조체, 열거형 또는 프로토콜 유형에 새로운 기능을 추가합니다. 여기에는 원본 소스 코드에 액세스 할 수 없는 타입을 확장하는 기능(소급 모델링이라고 함)이 포함됩니다. 확장은 Objective-C의 카테고리와 유사합니다. (Objective-C 카테고리와 달리 Swift 확장에는 이름이 없습니다.)

Extensions in Swift can:

* Add computed instance properties and computed type properties (computed instance properties 및 computed type properties 추가)
* Define instance methods and type methods (인스턴스 메서드 및 타입 메서드 정의)
* Provide new initializers (새로운 이니셜 라이저 제공)
* Define subscripts (subscript 정의)
* Define and use new nested types (새로운 nested type 정의 및 사용)
* Make an existing type conform to a protocol (기존 타입이 프로토콜을 준수하도록합니다.)

In Swift, you can even extend a protocol to provide implementations of its requirements or add additional functionality that conforming types can take advantage of. For more details, see Protocol Extensions.
> Swift에서는 프로토콜을 확장하여 요구 사항의 구현을 제공하거나 준수 유형이 활용할 수있는 추가 기능을 추가 할 수도 있습니다. 자세한 내용은 프로토콜 확장을 참조하세요.

note
Extensions can add new functionality to a type, but they can’t override existing functionality.
> Extension은 타입에 새로운 기능을 추가 할 수 있지만 기존 기능을 override(재정의) 할 수는 없습니다.

## Computed Properties

Extensions can add computed instance properties and computed type properties to existing types. This example adds five computed instance properties to Swift’s built-in Double type, to provide basic support for working with distance units:
> Extension은 computed instance properties와 computed type properties를 기존 타입에 추가 할 수 있습니다. 이 예제에서는 5개의 computed instance properties를 Swift의 내장 Double 타입에 추가하여 거리 단위 작업에 대한 기본 지원을 제공합니다.

```swift
extension Double {
    var km: Double { return self * 1_000.0 }
    var m: Double { return self }
    var cm: Double { return self / 100.0 }
    var mm: Double { return self / 1_000.0 }
    var ft: Double { return self / 3.28084 }
}
let oneInch = 25.4.mm
print("One inch is \(oneInch) meters")
// Prints "One inch is 0.0254 meters"
let threeFeet = 3.ft
print("Three feet is \(threeFeet) meters")
// Prints "Three feet is 0.914399970739201 meters"
``` 

Note
Extensions can add new computed properties, but they can’t add stored properties, or add property observers to existing properties.
> Extension은 새로운 computed properties를 추가 할 수 있지만 저장프로퍼티를 추가하거나 기존 프로퍼티에 프로퍼티옵저버를 추가 할 수는 없습니다.

## Initializers

Extensions can add new initializers to existing types. This enables you to extend other types to accept your own custom types as initializer parameters, or to provide additional initialization options that were not included as part of the type’s original implementation.
> Extension은 기존 타입에 새 이니셜라이저를 추가 할 수 있습니다. 이를 통해 다른 타입을 확장하여 사용자 정의 타입을 이니셜라이저 매개 변수로 허용하거나 타입의 원래 구현의 일부로 포함되지 않은 추가 초기화 옵션을 제공 할 수 있습니다.

Extensions can add new convenience initializers to a class, but they can’t add new designated initializers or deinitializers to a class. Designated initializers and deinitializers must always be provided by the original class implementation.
> Extension은 클래스에 새로운 convenience initializer를 추가 할 수 있지만 클래스에 새로 designated initializer 또는 deinitializer를 추가 할 수는 없습니다. designated initializer와 deinitializer는 항상 원래 클래스 구현에서 제공해야합니다.

```swift
struct Size {
    var width = 0.0, height = 0.0
}

struct Point {
    var x = 0.0, y = 0.0
}

struct Rect {
    var origin = Point()
    var size = Size()
}

let defaultRect = Rect()
let memberwiseRect = Rect(origin: Point(x: 2.0, y: 2.0),
   size: Size(width: 5.0, height: 5.0))
   
extension Rect {
    init(center: Point, size: Size) {
        let originX = center.x - (size.width / 2)
        let originY = center.y - (size.height / 2)
        self.init(origin: Point(x: originX, y: originY), size: size)
    }
}

let centerRect = Rect(center: Point(x: 4.0, y: 4.0),
                      size: Size(width: 3.0, height: 3.0))
// centerRect's origin is (2.5, 2.5) and its size is (3.0, 3.0)
``` 

Note
If you provide a new initializer with an extension, you are still responsible for making sure that each instance is fully initialized once the initializer completes.
> extension이있는 새 이니셜라이저를 제공하는 경우, 이니셜라이저가 완료되면 각 인스턴스가 완전히 초기화되었는지 확인해야합니다.

## Methods

Extensions can add new instance methods and type methods to existing types. The following example adds a new instance method called repetitions to the Int type:
> 확장은 기존 타입에 새 인스턴스 메소드 및 타입 메소드를 추가 할 수 있습니다. 다음 예제에서는 반복이라는 새 인스턴스 메서드를 Int 타입에 추가합니다.


```swift
extension Int {
    func repetitions(task: () -> Void) {
        for _ in 0..<self {
            task()
        }
    }
}
``` 

### Mutating Instance Methods

Instance methods added with an extension can also modify (or mutate) the instance itself. Structure and enumeration methods that modify self or its properties must mark the instance method as mutating, just like mutating methods from an original implementation.
> extension으로 추가 된 인스턴스 메서드는 인스턴스 자체를 수정 (또는 변경) 할 수도 있습니다. self 또는 해당 프로퍼티를 수정하는 구조체 및 열거형 메서드는 원래 mutating 메서드와 마찬가지로 인스턴스 메서드를 mutating으로 표시해야합니다.

The example below adds a new mutating method called square to Swift’s Int type, which squares the original value:
> 아래 예제는 square라는 새로운 mutating 메서드를 Swift의 Int 타입에 추가하여 원래 값을 제곱합니다.

```swift
extension Int {
    mutating func square() {
        self = self * self
    }
}
var someInt = 3
someInt.square()
// someInt is now 9
``` 

## Subscripts

Extensions can add new subscripts to an existing type. This example adds an integer subscript to Swift’s built-in Int type. This subscript [n] returns the decimal digit n places in from the right of the number:
> Extension은 기존 타입에 subscript를 추가 할 수 있습니다. 이 예제는 Swift의 내장 Int 타입에 정수 subscript를 추가합니다. 이 아래 첨자 [n]은 숫자 오른쪽에서 n 자리의 십진수를 반환합니다.

123456789[0] returns 9
123456789[1] returns 8

```swift
extension Int {
    subscript(digitIndex: Int) -> Int {
        var decimalBase = 1
        for _ in 0..<digitIndex {
            decimalBase *= 10
        }
        return (self / decimalBase) % 10
    }
}
746381295[0]
// returns 5
746381295[1]
// returns 9
746381295[2]
// returns 2
746381295[8]
// returns 7
``` 

## Nested Types

Extensions can add new nested types to existing classes, structures, and enumerations:
> Extension은 기존 클래스, 구조체 및 열거형에 새로운 중첩 타입을 추가 할 수 있습니다.

```swift
extension Int {
    enum Kind {
        case negative, zero, positive
    }
    var kind: Kind {
        switch self {
        case 0:
            return .zero
        case let x where x > 0:
            return .positive
        default:
            return .negative
        }
    }
}
``` 
