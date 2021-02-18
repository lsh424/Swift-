# Methods

[Swift Docs](https://docs.swift.org/swift-book/LanguageGuide/Methods.html)

Methods are functions that are associated with a particular type. Classes, structures, and enumerations can all define instance methods, which encapsulate specific tasks and functionality for working with an instance of a given type. Classes, structures, and enumerations can also define type methods, which are associated with the type itself. Type methods are similar to class methods in Objective-C.
> 메서드는 특정 타입과 관련된 함수입니다. 클래스, 구조체 및 열거형은 모두 지정된 유형의 인스턴스로 작업하기위한 특정 작업 및 기능을 캡슐화하는 인스턴스 메서드를 정의 할 수 있습니다. 클래스, 구조체 및 열거형은 타입 자체와 연관된 타입 메소드를 정의 할 수도 있습니다. 타입 메서드는 Objective-C의 클래스 메서드와 유사합니다.

The fact that structures and enumerations can define methods in Swift is a major difference from C and Objective-C. In Objective-C, classes are the only types that can define methods. In Swift, you can choose whether to define a class, structure, or enumeration, and still have the flexibility to define methods on the type you create.
> 구조체와 열거형이 Swift에서 메서드를 정의 할 수 있다는 사실은 C 및 Objective-C와의 주요 차이점입니다. Objective-C에서 클래스는 메서드를 정의 할 수있는 유일한 유형입니다. Swift에서는 클래스, 구조체 또는 열거형을 정의할지 여부를 선택할 수 있으며 생성한 타입에 대한 메서드를 유연하게 정의 할 수 있습니다.

## Instance Methods

Instance methods are functions that belong to instances of a particular class, structure, or enumeration. They support the functionality of those instances, either by providing ways to access and modify instance properties, or by providing functionality related to the instance’s purpose. Instance methods have exactly the same syntax as functions, as described in Functions.  
> 인스턴스 메서드는 특정 클래스, 구조체 또는 열거형의 인스턴스에 속하는 함수입니다. 인스턴스 프로퍼티에 액세스하고 수정하는 방법을 제공하거나 인스턴스의 목적과 관련된 기능을 제공하여 해당 인스턴스의 기능을 지원합니다. 인스턴스 메서드는 함수에 설명 된대로 함수와 정확히 동일한 구문을 갖습니다.

## The self Property

Every instance of a type has an implicit property called self, which is exactly equivalent to the instance itself. You use the self property to refer to the current instance within its own instance methods.
> 타입의 모든 인스턴스에는 self라는 암시 적 속성이 있으며 이는 인스턴스 자체와 정확히 동일합니다. self property를 사용하여 자체 인스턴스 메서드 내에서 현재 인스턴스를 참조합니다.

The main exception to this rule occurs when a parameter name for an instance method has the same name as a property of that instance. In this situation, the parameter name takes precedence, and it becomes necessary to refer to the property in a more qualified way. You use the self property to distinguish between the parameter name and the property name.
> 이 규칙의 주요 예외는 인스턴스 메서드의 매개 변수 이름이 해당 인스턴스의 프로퍼티와 동일한 이름을 가질 때 발생합니다. 이 상황에서는 매개 변수 이름이 우선하므로보다 규정 된 방식으로 프로퍼티를 참조해야합니다. self 속성을 사용하여 매개 변수 이름과 속성 이름을 구분합니다.

```swift
struct Point {
    var x = 0.0, y = 0.0
    func isToTheRightOf(x: Double) -> Bool {
        return self.x > x
    }
}

let somePoint = Point(x: 4.0, y: 5.0)
if somePoint.isToTheRightOf(x: 1.0) {
    print("This point is to the right of the line where x == 1.0")
}
// Prints "This point is to the right of the line where x == 1.0"
``` 

Without the self prefix, Swift would assume that both uses of x referred to the method parameter called x.
> self 접두사가 없으면 Swift는 두 x를 메서드 매개 변수를 참조한다고 가정합니다.

## Modifying Value Types from Within Instance Methods

Structures and enumerations are value types. By default, the properties of a value type can’t be modified from within its instance methods.
> 구조체와 열거형은 값 유형입니다. 기본적으로 값 유형의 프로퍼티는 인스턴스 메서드 내에서 수정할 수 없습니다.

However, if you need to modify the properties of your structure or enumeration within a particular method, you can opt in to mutating behavior for that method. The method can then mutate (that is, change) its properties from within the method, and any changes that it makes are written back to the original structure when the method ends. The method can also assign a completely new instance to its implicit self property, and this new instance will replace the existing one when the method ends.
> 그러나 특정 메서드 내에서 구조체 또는 열거형의 프로퍼티를 수정해야하는 경우 해당 메서드에 대한 동작을 변경하도록 선택할 수 있습니다. 그런 다음 메서드는 메서드 내에서 프로퍼티를 변경 (즉, 변경) 할 수 있으며 메서드가 종료 될 때 변경 한 모든 내용은 원래 구조체에 다시 기록됩니다. 메서드는 암시적 자체 프로퍼티(self)에 완전히 새로운 인스턴스를 할당 할 수도 있으며 이 새 인스턴스는 메서드가 종료 될 때 기존 인스턴스를 대체합니다.

You can opt in to this behavior by placing the mutating keyword before the func keyword for that method:
> 해당 메서드의 func 키워드 앞에 mutating 키워드를 배치하여 이 동작을 선택할 수 있습니다. -> mutating을 붙임으로써 인스턴스 내 프로퍼티 수정이 가능하다


```swift
struct Point {
    var x = 0.0, y = 0.0
    mutating func moveBy(x deltaX: Double, y deltaY: Double) {
        x += deltaX
        y += deltaY
    }
}
var somePoint = Point(x: 1.0, y: 1.0)
somePoint.moveBy(x: 2.0, y: 3.0)
print("The point is now at (\(somePoint.x), \(somePoint.y))")
// Prints "The point is now at (3.0, 4.0)"
``` 

The Point structure above defines a mutating moveBy(x:y:) method, which moves a Point instance by a certain amount. Instead of returning a new point, this method actually modifies the point on which it’s called. The mutating keyword is added to its definition to enable it to modify its properties.
> 위의 Point 구조체는 Point 인스턴스를 일정량 이동하는 mutating moveBy (x : y :) 메서드를 정의합니다. 새 포인트를 반환하는 대신 이 메서드는 실제로 호출 된 포인트를 수정합니다. mutating 키워드는 프로퍼티를 수정할 수 있도록 정의에 추가됩니다.

Note that you cannot call a mutating method on a constant of structure type, because its properties cannot be changed, even if they are variable properties, as described in Stored Properties of Constant Structure Instances:
> 상수 구조체 인스턴스의 저장 프로퍼티에 설명 된대로 변수 속성이더라도 속성을 변경할 수 없기 때문에 구조체 타입의 상수에 대해 변경 메서드를 호출 할 수 없습니다.

```swift
let fixedPoint = Point(x: 3.0, y: 3.0)
fixedPoint.moveBy(x: 2.0, y: 3.0)
// this will report an error
``` 

## Assigning to self Within a Mutating Method

Mutating methods can assign an entirely new instance to the implicit self property. The Point example shown above could have been written in the following way instead:
> Mutating 메서드는 암시적 자체 속성(self)에 완전히 새로운 인스턴스를 할당 할 수 있습니다. 위에 표시된 Point 예제는 대신 다음과 같은 방식으로 작성되었을 수 있습니다.

```swift
struct Point {
    var x = 0.0, y = 0.0
    mutating func moveBy(x deltaX: Double, y deltaY: Double) {
        self = Point(x: x + deltaX, y: y + deltaY)
    }
}
``` 

This version of the mutating moveBy(x:y:) method creates a new structure whose x and y values are set to the target location. The end result of calling this alternative version of the method will be exactly the same as for calling the earlier version.
> 이 버전의 mutating moveBy (x : y :) 메서드는 x 및 y 값이 대상 위치로 설정된 새 구조체를 만듭니다. 이 대체 버전의 메서드를 호출 한 최종 결과는 이전 버전을 호출 할 때와 정확히 동일합니다.

Mutating methods for enumerations can set the implicit self parameter to be a different case from the same enumeration:
> 열거형의 Mutating 메서드는 암시적 self 매개 변수를 동일한 열거형과 다른 케이스로 설정할 수 있습니다.

```swift
enum TriStateSwitch {
    case off, low, high
    mutating func next() {
        switch self {
        case .off:
            self = .low
        case .low:
            self = .high
        case .high:
            self = .off
        }
    }
}
var ovenLight = TriStateSwitch.low
ovenLight.next()
// ovenLight is now equal to .high
ovenLight.next()
// ovenLight is now equal to .off
``` 

This example defines an enumeration for a three-state switch. The switch cycles between three different power states (off, low and high) every time its next() method is called.
> 이 예에서는 3 개 상태 스위치에 대한 열거형을 정의합니다. 스위치는 next() 메서드가 호출 될 때마다 세 가지 전원 상태 (꺼짐, 낮음 및 높음) 사이를 순환합니다.

## Type Methods

Instance methods, as described above, are methods that you call on an instance of a particular type. You can also define methods that are called on the type itself. These kinds of methods are called type methods. You indicate type methods by writing the static keyword before the method’s func keyword. Classes can use the class keyword instead, to allow subclasses to override the superclass’s implementation of that method.
> 위에서 설명한대로 인스턴스 메서드는 특정 유형의 인스턴스에서 호출하는 메서드입니다. 타입 자체에서 호출되는 메소드를 정의 할 수도 있습니다. 이러한 종류의 메서드를 타입 메서드라고합니다. 메서드의 func 키워드 앞에 static 키워드를 작성하여 타입 메서드를 나타냅니다. 클래스는 대신 class 키워드를 사용하여 하위 클래스가 해당 메서드의 수퍼 클래스 구현을 재정의 할 수 있습니다.

Type methods are called with dot syntax, like instance methods. However, you call type methods on the type, not on an instance of that type. Here’s how you call a type method on a class called SomeClass:
> 타입 메서드는 인스턴스 메서드와 같이 점 구문으로 호출됩니다. 그러나 해당 타입의 인스턴스가 아닌 타입에 대해 타입 메서드를 호출합니다. SomeClass라는 클래스에서 타입 메서드를 호출하는 방법은 다음과 같습니다.

```swift
class SomeClass {
    class func someTypeMethod() {
        // type method implementation goes here
    }
}
SomeClass.someTypeMethod()
``` 

Within the body of a type method, the implicit self property refers to the type itself, rather than an instance of that type. This means that you can use self to disambiguate between type properties and type method parameters, just as you do for instance properties and instance method parameters.
> 타입 메서드의 본문 내에서 암시적 자체 속성(self)는 해당 타입의 인스턴스가 아닌 타입 자체를 참조합니다. 즉, 인스턴스 프로퍼티 및 인스턴스 메서드 매개 변수에 대해 수행하는 것처럼 self를 사용하여 타입 프로퍼티와 타입 메서드 매개 변수를 명확하게 할 수 있습니다.

More generally, any unqualified method and property names that you use within the body of a type method will refer to other type-level methods and properties. A type method can call another type method with the other method’s name, without needing to prefix it with the type name. Similarly, type methods on structures and enumerations can access type properties by using the type property’s name without a type name prefix.
> 일반적으로 타입 메서드 본문 내에서 사용하는 정규화되지 않은 메서드 및 프로퍼티 이름은 다른 type-level 메서드 및 프로퍼티를 참조합니다. 타입 메소드는 타입 이름을 접두사로 사용할 필요없이 다른 메소드의 이름으로 다른 타입 메소드를 호출 할 수 있습니다. 마찬가지로 구조체 및 열거형의 타입 메서드는 타입 이름 접두사없이 타입 프로퍼티의 이름을 사용하여 타입 프로퍼티에 액세스 할 수 있습니다.

The example below defines a structure called LevelTracker, which tracks a player’s progress through the different levels or stages of a game. It’s a single-player game, but can store information for multiple players on a single device.
> 아래 예는 게임의 다양한 레벨 또는 단계를 통해 플레이어의 진행 상황을 추적하는 LevelTracker라는 구조체를 정의합니다. 싱글 플레이어 게임이지만 단일 기기에 여러 플레이어에 대한 정보를 저장할 수 있습니다.

All of the game’s levels (apart from level one) are locked when the game is first played. Every time a player finishes a level, that level is unlocked for all players on the device. The LevelTracker structure uses type properties and methods to keep track of which levels of the game have been unlocked. It also tracks the current level for an individual player.
> 게임이 처음 플레이 될 때 모든 게임 레벨 (1 레벨 제외)이 잠깁니다. 플레이어가 레벨을 완료 할 때마다 해당 레벨은 장치의 모든 플레이어에 대해 잠금 해제됩니다. LevelTracker 구조는 유형 속성과 메서드를 사용하여 잠금 해제 된 게임 레벨을 추적합니다. 또한 개별 플레이어의 현재 레벨을 추적합니다.


```swift
struct LevelTracker {
    static var highestUnlockedLevel = 1
    var currentLevel = 1

    static func unlock(_ level: Int) {
        if level > highestUnlockedLevel { highestUnlockedLevel = level }
    }

    static func isUnlocked(_ level: Int) -> Bool { 
        return level <= highestUnlockedLevel
    }

    @discardableResult
    mutating func advance(to level: Int) -> Bool {
        if LevelTracker.isUnlocked(level) {
            currentLevel = level
            return true
        } else {
            return false
        }
    }
}
``` 

```swift
class Player {
    var tracker = LevelTracker()
    let playerName: String
    func complete(level: Int) {
        LevelTracker.unlock(level + 1)
        tracker.advance(to: level + 1)
    }
    init(name: String) {
        playerName = name
    }
}
``` 

```swift
var player = Player(name: "Argyrios")
player.complete(level: 1)
print("highest unlocked level is now \(LevelTracker.highestUnlockedLevel)")
// Prints "highest unlocked level is now 2"
``` 

```swift
player = Player(name: "Beto")
if player.tracker.advance(to: 6) {
    print("player is now on level 6")
} else {
    print("level 6 hasn't yet been unlocked")
}
// Prints "level 6 hasn't yet been unlocked"
``` 
