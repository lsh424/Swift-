# Protocols

[Swift Docs](https://docs.swift.org/swift-book/LanguageGuide/Protocols.html)  

A protocol defines a blueprint of methods, properties, and other requirements that suit a particular task or piece of functionality. The protocol can then be adopted by a class, structure, or enumeration to provide an actual implementation of those requirements. Any type that satisfies the requirements of a protocol is said to conform to that protocol.
> 프로토콜은 특정 작업 또는 기능에 적합한 메서드, 프로퍼티 및 기타 요구 사항의 청사진을 정의합니다. 그런 다음 해당 요구 사항의 실제 구현을 제공하기 위해 클래스, 구조체 또는 열거형에서 프로토콜을 채택 할 수 있습니다. 프로토콜의 요구 사항을 충족하는 모든 타입은 해당 프로토콜을 준수한다고합니다.

In addition to specifying requirements that conforming types must implement, you can extend a protocol to implement some of these requirements or to implement additional functionality that conforming types can take advantage of.
> 형식을 준수해야하는 요구 사항을 지정하는 것 외에도 프로토콜을 확장하여 이러한 요구 사항 중 일부를 구현하거나 형식을 준수하는 추가 기능을 구현할 수 있습니다.

## Protocol Syntax

```swift
protocol SomeProtocol {
    // protocol definition goes here
}

struct SomeStructure: FirstProtocol, AnotherProtocol {
    // structure definition goes here
}

class SomeClass: SomeSuperclass, FirstProtocol, AnotherProtocol {
    // class definition goes here
}
```  

## Property Requirements

A protocol can require any conforming type to provide an instance property or type property with a particular name and type. The protocol doesn’t specify whether the property should be a stored property or a computed property—it only specifies the required property name and type. The protocol also specifies whether each property must be gettable or gettable and settable.
> 프로토콜은 특정 이름 및 타입과 함께 인스턴스 프로퍼티 또는 타입 프로퍼티를 제공하기 위해 모든 준수 타입을 요구할 수 있습니다. 프로토콜은 프로퍼티가 저장 프로퍼티인지 계산 프로퍼티인지 여부를 지정하지 않으며 필요한 프로퍼티 이름과 타입만 지정합니다. 프로토콜은 각 프로퍼티가 gettable인지, gettable 및 settable인지 여부도 지정합니다.

If a protocol requires a property to be gettable and settable, that property requirement can’t be fulfilled by a constant stored property or a read-only computed property. If the protocol only requires a property to be gettable, the requirement can be satisfied by any kind of property, and it’s valid for the property to be also settable if this is useful for your own code.
> 프로토콜에서 프로퍼티를 gettable & settable로 설정하길 요구하는 경우, 해당 프로퍼티 요구 사항은 상수 저장 프로퍼티 또는 읽기 전용 계산 프로퍼티로 충족 될 수 없습니다. 프로토콜에서 프로퍼티를 가져 오기만하면되는 경우 모든 종류의 프로퍼티로 요구 사항을 충족 할 수 있으며 사용자 고유의 코드에 유용 할 경우 프로퍼티를 설정할 수도 있습니다.

Property requirements are always declared as variable properties, prefixed with the var keyword. Gettable and settable properties are indicated by writing { get set } after their type declaration, and gettable properties are indicated by writing { get }.
> 프로퍼티 요구 사항은 항상 var 키워드로 시작하는 변수 프로퍼티로 선언됩니다. Gettable 및 settable 프로퍼티는 타입 선언 후에 {get set}을 작성하여 표시하고 gettable 속성은 {get}을 작성하여 표시합니다.


```swift
protocol SomeProtocol {
    var mustBeSettable: Int { get set }
    var doesNotNeedToBeSettable: Int { get }
}
```  

```swift
protocol AnotherProtocol {
    static var someTypeProperty: Int { get set }
}
``` 

```swift
protocol FullyNamed {
    var fullName: String { get }
}


struct Person: FullyNamed {
    var fullName: String
}
let john = Person(fullName: "John Appleseed")
// john.fullName is "John Appleseed"


class Starship: FullyNamed {
    var prefix: String?
    var name: String
    init(name: String, prefix: String? = nil) {
        self.name = name
        self.prefix = prefix
    }
    var fullName: String {
        return (prefix != nil ? prefix! + " " : "") + name
    }
}
var ncc1701 = Starship(name: "Enterprise", prefix: "USS")
// ncc1701.fullName is "USS Enterprise"
``` 

## Method Requirements

Protocols can require specific instance methods and type methods to be implemented by conforming types. These methods are written as part of the protocol’s definition in exactly the same way as for normal instance and type methods, but without curly braces or a method body. Variadic parameters are allowed, subject to the same rules as for normal methods. Default values, however, can’t be specified for method parameters within a protocol’s definition.
> 프로토콜은 형식을 준수하여 구현되는 특정 인스턴스 메서드 및 타입 메서드를 요구할 수 있습니다. 이러한 메서드는 일반 인스턴스 및 타입 메서드와 똑같은 방식으로 프로토콜 정의의 일부로 작성되지만 중괄호 나 메서드 본문은 없습니다. 일반적인 방법과 동일한 규칙에 따라 가변 매개 변수가 허용됩니다. 그러나 프로토콜 정의 내에서 메서드 매개 변수에 대해 기본값을 지정할 수 없습니다.

As with type property requirements, you always prefix type method requirements with the static keyword when they’re defined in a protocol. This is true even though type method requirements are prefixed with the class or static keyword when implemented by a class:
> 타입 프로퍼티 요구 사항과 마찬가지로 프로토콜에 타입 메서드를 정의 할 경우 정의 사항 앞에 static 키워드를 붙입니다. 이는 클래스에 의해 구현 될 때 타입 메소드 요구 사항에 class 또는 static 키워드가 접두어로 붙는 경우에도 마찬가지입니다.

```swift
protocol SomeProtocol {
    static func someTypeMethod()
}

protocol RandomNumberGenerator {
    func random() -> Double
}
```

This protocol, RandomNumberGenerator, requires any conforming type to have an instance method called random, which returns a Double value whenever it’s called. Although it’s not specified as part of the protocol, it’s assumed that this value will be a number from 0.0 up to (but not including) 1.0.
> 프로토콜 RandomNumberGenerator는 호출 될 때마다 Double 값을 반환하는 random이라는 인스턴스 메서드를 갖는 모든 준수 유형을 필요로합니다. 프로토콜의 일부로 지정되지는 않았지만이 값은 0.0에서 1.0 (포함하지 않음) 사이의 숫자라고 가정합니다.

The RandomNumberGenerator protocol doesn’t make any assumptions about how each random number will be generated—it simply requires the generator to provide a standard way to generate a new random number.
> RandomNumberGenerator 프로토콜은 각 난수가 생성되는 방식에 대해 어떤 가정도하지 않습니다. 단순히 생성기가 새로운 난수를 생성하는 표준 방법을 제공하면됩니다.

Here’s an implementation of a class that adopts and conforms to the RandomNumberGenerator protocol. This class implements a pseudorandom number generator algorithm known as a linear congruential generator:
> 다음은 RandomNumberGenerator 프로토콜을 채택하고 준수하는 클래스 구현입니다. 이 클래스는 선형 합동 생성기로 알려진 의사 난수 생성기 알고리즘을 구현합니다.

```swift
class LinearCongruentialGenerator: RandomNumberGenerator {
    var lastRandom = 42.0
    let m = 139968.0
    let a = 3877.0
    let c = 29573.0
    func random() -> Double {
        lastRandom = ((lastRandom * a + c)
            .truncatingRemainder(dividingBy:m))
        return lastRandom / m
    }
}
let generator = LinearCongruentialGenerator()
print("Here's a random number: \(generator.random())")
// Prints "Here's a random number: 0.3746499199817101"
print("And another one: \(generator.random())")
// Prints "And another one: 0.729023776863283"
```

## Mutating Method Requirements

It’s sometimes necessary for a method to modify (or mutate) the instance it belongs to. For instance methods on value types (that is, structures and enumerations) you place the mutating keyword before a method’s func keyword to indicate that the method is allowed to modify the instance it belongs to and any properties of that instance. This process is described in Modifying Value Types from Within Instance Methods.
> 메서드가 속한 인스턴스를 수정(또는 변경)해야하는 경우가 있습니다. 값 타입 (즉, 구조체 및 열거형)에 대한 인스턴스 메서드의 경우 메서드의 func 키워드 앞에 mutating 키워드를 배치하여 메서드가 속한 인스턴스와 해당 인스턴스의 프로퍼티를 수정할 수 있음을 나타냅니다. 이 프로세스는 인스턴스 메서드 내에서 값 유형 수정에 설명되어 있습니다.

If you define a protocol instance method requirement that’s intended to mutate instances of any type that adopts the protocol, mark the method with the mutating keyword as part of the protocol’s definition. This enables structures and enumerations to adopt the protocol and satisfy that method requirement.
> 프로토콜을 채택하는 모든 타입의 인스턴스를 변경하기위한 프로토콜 인스턴스 메소드 요구 사항을 정의하는 경우 프로토콜 정의의 일부로 mutating 키워드를 사용하여 메소드를 표시하십시오. 이를 통해 구조체와 열거형이 프로토콜을 채택하고 해당 메서드 요구 사항을 충족 할 수 있습니다.

Note
If you mark a protocol instance method requirement as mutating, you don’t need to write the mutating keyword when writing an implementation of that method for a class. The mutating keyword is only used by structures and enumerations.
> 프로토콜 인스턴스 메서드 요구 사항을 mutating으로 표시하면 클래스에 대한 해당 메서드의 구현을 작성할 때 mutating 키워드를 작성할 필요가 없습니다. mutating 키워드는 구조체와 열거형에서만 사용됩니다.

```swift
protocol Togglable {
    mutating func toggle()
}

enum OnOffSwitch: Togglable {
    case off, on
    mutating func toggle() {
        switch self {
        case .off:
            self = .on
        case .on:
            self = .off
        }
    }
}
var lightSwitch = OnOffSwitch.off
lightSwitch.toggle()
// lightSwitch is now equal to .on
```

## Initializer Requirements

Protocols can require specific initializers to be implemented by conforming types. You write these initializers as part of the protocol’s definition in exactly the same way as for normal initializers, but without curly braces or an initializer body:
> 프로토콜은 형식을 준수하여 구현되는 특정 이니셜라이저를 요구할 수 있습니다. 이 이니셜라이저는 일반 이니셜라이저와 똑같은 방식으로 프로토콜 정의의 일부로 작성하지만 중괄호 나 이니셜라이저 본문은 사용하지 않습니다.

```swift
protocol SomeProtocol {
    init(someParameter: Int)
}
```

### Class Implementations of Protocol Initializer Requirements

You can implement a protocol initializer requirement on a conforming class as either a designated initializer or a convenience initializer. In both cases, you must mark the initializer implementation with the required modifier:
> designated initializer 또는 convenience initializer로 준수 클래스에 프로토콜 이니셜라이저 요구 사항을 구현할 수 있습니다. 두 경우 모두 이니셜 라이저 구현을 required로 표시해야합니다.

```swift
class SomeClass: SomeProtocol {
    required init(someParameter: Int) {
        // initializer implementation goes here
    }
}
```

The use of the required modifier ensures that you provide an explicit or inherited implementation of the initializer requirement on all subclasses of the conforming class, such that they also conform to the protocol.
> required를 사용하면 준수하는 클래스의 모든 하위 클래스에서 이니셜라이저 요구 사항의 명시 적 또는 상속 된 구현을 제공하여 프로토콜도 준수합니다.

If a subclass overrides a designated initializer from a superclass, and also implements a matching initializer requirement from a protocol, mark the initializer implementation with both the required and override modifiers:
> 서브 클래스가 슈퍼 클래스에서 designated initializer를 재정의하고 프로토콜에서 일치하는 이니셜라이저 요구 사항도 구현하는 경우 required 및 override로 이니셜 라이저 구현을 표시합니다.

```swift
protocol SomeProtocol {
    init()
}

class SomeSuperClass {
    init() {
        // initializer implementation goes here
    }
}

class SomeSubClass: SomeSuperClass, SomeProtocol {
    // "required" from SomeProtocol conformance; "override" from SomeSuperClass
    required override init() {
        // initializer implementation goes here
    }
}
```

## Protocols as Types

Protocols don’t actually implement any functionality themselves. Nonetheless, you can use protocols as a fully fledged types in your code. Using a protocol as a type is sometimes called an existential type, which comes from the phrase “there exists a type T such that T conforms to the protocol”.
> 프로토콜 자체는 실제로 어떤 기능도 구현하지 않습니다. 그럼에도 불구하고 프로토콜을 코드에서 완전한 타입으로 사용할 수 있습니다. 프로토콜을 타입으로 사용하는 것은 때때로 "T가 프로토콜을 준수하는 유형 T가 존재한다"는 구절에서 비롯된 존재 타입이라고합니다.

You can use a protocol in many places where other types are allowed, including:
> 다음을 포함하여 다른 타입이 허용되는 여러 위치에서 프로토콜을 사용할 수 있습니다.

* As a parameter type or return type in a function, method, or initializer (함수, 메서드 또는 이니셜라이저에서 매개변수 타입 또는 반환 타입으로)
* As the type of a constant, variable, or property (상수, 변수 또는 프로퍼티의 타입으로)
* As the type of items in an array, dictionary, or other container (배열, 딕셔너리 또는 기타 컨테이너의 항목 타입으로)

NOTE
Because protocols are types, begin their names with a capital letter (such as FullyNamed and RandomNumberGenerator) to match the names of other types in Swift (such as Int, String, and Double).
> 프로토콜은 타입이므로 Swift의 다른 타입 (예 : Int, String 및 Double)과 일치하도록 이름을 대문자 (예 : FullyNamed 및 RandomNumberGenerator)로 시작하십시오.

Here’s an example of a protocol used as a type:
> 다음은 유형으로 사용되는 프로토콜의 예입니다.

```swift
class Dice {
    let sides: Int
    let generator: RandomNumberGenerator
    init(sides: Int, generator: RandomNumberGenerator) {
        self.sides = sides
        self.generator = generator
    }
    func roll() -> Int {
        return Int(generator.random() * Double(sides)) + 1
    }
}
```

The generator property is of type RandomNumberGenerator. Therefore, you can set it to an instance of any type that adopts the RandomNumberGenerator protocol. Nothing else is required of the instance you assign to this property, except that the instance must adopt the RandomNumberGenerator protocol. Because its type is RandomNumberGenerator, code inside the Dice class can only interact with generator in ways that apply to all generators that conform to this protocol. That means it can’t use any methods or properties that are defined by the underlying type of the generator. However, you can downcast from a protocol type to an underlying type in the same way you can downcast from a superclass to a subclass, as discussed in Downcasting.
> generator 프로퍼티는 RandomNumberGenerator 타입입니다. 따라서 RandomNumberGenerator 프로토콜을 채택하는 모든 타입의 인스턴스로 설정할 수 있습니다. 인스턴스가 RandomNumberGenerator 프로토콜을 채택해야한다는 점을 제외하고는이 프로퍼티에 할당하는 인스턴스에 다른 것은 필요하지 않습니다. 타입이 RandomNumberGenerator이기 때문에 Dice 클래스 내부의 코드는 이 프로토콜을 준수하는 모든 generator에 적용되는 방식으로만 generator와 상호 작용할 수 있습니다. 즉, generator의 기본 타입으로 정의 된 메서드 나 프로퍼티를 사용할 수 없습니다. 그러나 다운 캐스팅에 설명 된대로 수퍼 클래스에서 서브 클래스로 다운 캐스트 할 수있는 것과 동일한 방식으로 프로토콜 타입에서 기본 타입으로 다운 캐스팅 할 수 있습니다.

Dice also has an initializer, to set up its initial state. This initializer has a parameter called generator, which is also of type RandomNumberGenerator. You can pass a value of any conforming type in to this parameter when initializing a new Dice instance.
> Dice에는 초기 상태를 설정하는 이니셜라이저도 있습니다. 이 초기화 프로그램에는 또한 RandomNumberGenerator 타입 인 generator라는 매개 변수가 있습니다. 새로운 Dice 인스턴스를 초기화 할 때이 매개 변수에 적합한 타입의 값을 전달할 수 있습니다.

Dice provides one instance method, roll, which returns an integer value between 1 and the number of sides on the dice. This method calls the generator’s random() method to create a new random number between 0.0 and 1.0, and uses this random number to create a dice roll value within the correct range. Because generator is known to adopt RandomNumberGenerator, it’s guaranteed to have a random() method to call.
> Dice는 1과 주사위의 변수 사이의 정수 값을 반환하는 하나의 인스턴스 메서드 인 roll을 제공합니다. 이 메서드는 생성기의 random() 메서드를 호출하여 0.0에서 1.0 사이의 새 난수를 만들고이 난수를 사용하여 올바른 범위 내에서 주사위 굴림 값을 만듭니다. generator는 RandomNumberGenerator를 채택하는 것으로 알려져 있기 때문에 호출 할 random() 메서드가 보장됩니다.

```swift
var d6 = Dice(sides: 6, generator: LinearCongruentialGenerator())
for _ in 1...5 {
    print("Random dice roll is \(d6.roll())")
}
// Random dice roll is 3
// Random dice roll is 5
// Random dice roll is 4
// Random dice roll is 5
// Random dice roll is 4
```

## Delegation

Delegation is a design pattern that enables a class or structure to hand off (or delegate) some of its responsibilities to an instance of another type. This design pattern is implemented by defining a protocol that encapsulates the delegated responsibilities, such that a conforming type (known as a delegate) is guaranteed to provide the functionality that has been delegated. Delegation can be used to respond to a particular action, or to retrieve data from an external source without needing to know the underlying type of that source.
> 위임은 클래스 또는 구조체가 책임의 일부를 다른 타입의 인스턴스에 넘겨 주거나 위임 할 수 있도록하는 디자인 패턴입니다. 이 디자인 패턴은 위임 된 기능을 제공하기 위해 준수 형식 (대리자라고 함)이 보장되도록 위임 된 책임을 캡슐화하는 프로토콜을 정의하여 구현됩니다. 위임은 특정 작업에 응답하거나 해당 소스의 기본 유형을 몰라도 외부 소스에서 데이터를 검색하는 데 사용할 수 있습니다.

The example below defines two protocols for use with dice-based board games:
> 아래 예는 주사위 기반 보드 게임에 사용할 두 가지 프로토콜을 정의합니다.

```swift
protocol DiceGame {
    var dice: Dice { get }
    func play()
}

protocol DiceGameDelegate: AnyObject {
    func gameDidStart(_ game: DiceGame)
    func game(_ game: DiceGame, didStartNewTurnWithDiceRoll diceRoll: Int)
    func gameDidEnd(_ game: DiceGame)
}
```

The DiceGame protocol is a protocol that can be adopted by any game that involves dice.
> DiceGame 프로토콜은 dice를 포함하는 모든 게임에서 채택 할 수있는 프로토콜입니다.

The DiceGameDelegate protocol can be adopted to track the progress of a DiceGame. To prevent strong reference cycles, delegates are declared as weak references. For information about weak references, see Strong Reference Cycles Between Class Instances. Marking the protocol as class-only lets the SnakesAndLadders class later in this chapter declare that its delegate must use a weak reference. A class-only protocol is marked by its inheritance from AnyObject, as discussed in Class-Only Protocols.
> DiceGameDelegate 프로토콜을 채택하여 DiceGame의 진행 상황을 추적 할 수 있습니다. 강력한 참조주기를 방지하기 위해 delegate는 약한 참조로 선언됩니다. 약한 참조에 대한 자세한 내용은 클래스 인스턴스 간의 강력한 참조주기 단원을 참조하십시오. 프로토콜을 클래스 전용으로 표시하면 이 장의 뒷부분에서 SnakesAndLadders 클래스가 해당 대리자가 약한 참조를 사용해야한다고 선언 할 수 있습니다. 클래스 전용 프로토콜은 클래스 전용 프로토콜에 설명 된대로 AnyObject의 상속으로 표시됩니다.

Here’s a version of the Snakes and Ladders game originally introduced in Control Flow. This version is adapted to use a Dice instance for its dice-rolls; to adopt the DiceGame protocol; and to notify a DiceGameDelegate about its progress:
> 다음은 원래 Control Flow에 도입 된 Snakes and Ladders 게임 버전입니다. 이 버전은 주사위 굴림에 Dice 인스턴스를 사용하도록 조정되었습니다. DiceGame 프로토콜 채택 진행 상황을 DiceGameDelegate에 알리려면 :

```swift
class SnakesAndLadders: DiceGame {
    let finalSquare = 25
    let dice = Dice(sides: 6, generator: LinearCongruentialGenerator())
    var square = 0
    var board: [Int]
    init() {
        board = Array(repeating: 0, count: finalSquare + 1)
        board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
        board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
    }
    weak var delegate: DiceGameDelegate?
    func play() {
        square = 0
        delegate?.gameDidStart(self)
        gameLoop: while square != finalSquare {
            let diceRoll = dice.roll()
            delegate?.game(self, didStartNewTurnWithDiceRoll: diceRoll)
            switch square + diceRoll {
            case finalSquare:
                break gameLoop
            case let newSquare where newSquare > finalSquare:
                continue gameLoop
            default:
                square += diceRoll
                square += board[square]
            }
        }
        delegate?.gameDidEnd(self)
    }
}
```

This version of the game is wrapped up as a class called SnakesAndLadders, which adopts the DiceGame protocol. It provides a gettable dice property and a play() method in order to conform to the protocol. (The dice property is declared as a constant property because it doesn’t need to change after initialization, and the protocol only requires that it must be gettable.)
> 이 버전의 게임은 DiceGame 프로토콜을 채택하는 SnakesAndLadders라는 클래스로 래핑됩니다. 프로토콜을 준수하기 위해 gettable dice 프로퍼티와 play() 메서드를 제공합니다. (dice 프로퍼티는 초기화 후에 변경할 필요가없고 프로토콜에서 얻을 수 있어야만하기 때문에 상수 프로퍼티로 선언됩니다.)

Note that the delegate property is defined as an optional DiceGameDelegate, because a delegate isn’t required in order to play the game. Because it’s of an optional type, the delegate property is automatically set to an initial value of nil. Thereafter, the game instantiator has the option to set the property to a suitable delegate. Because the DiceGameDelegate protocol is class-only, you can declare the delegate to be weak to prevent reference cycles.
> delegate가 게임을 플레이하는 데 필요하지 않기 때문에 delegate 프로퍼티는 optional DiceGameDelegate로 정의됩니다. optional 타입 이므로 delegate property는 자동으로 초기 값 nil로 설정됩니다. 그 후에 game instantiator는 프로퍼티를 적절한 대리자로 설정하는 옵션을 갖게됩니다. DiceGameDelegate 프로토콜은 클래스 전용이므로 참조주기를 방지하기 위해 델리게이트를 약한 참조로 선언 할 수 있습니다.


DiceGameDelegate provides three methods for tracking the progress of a game. These three methods have been incorporated into the game logic within the play() method above, and are called when a new game starts, a new turn begins, or the game ends.
> DiceGameDelegate는 게임 진행 상황을 추적하는 세 가지 방법을 제공합니다. 이 세 가지 메서드는 위의 play() 메서드 내의 게임 논리에 통합되었으며 새 게임이 시작되거나 새 턴이 시작되거나 게임이 끝날 때 호출됩니다.

```swift
class DiceGameTracker: DiceGameDelegate {
    var numberOfTurns = 0
    func gameDidStart(_ game: DiceGame) {
        numberOfTurns = 0
        if game is SnakesAndLadders {
            print("Started a new game of Snakes and Ladders")
        }
        print("The game is using a \(game.dice.sides)-sided dice")
    }
    func game(_ game: DiceGame, didStartNewTurnWithDiceRoll diceRoll: Int) {
        numberOfTurns += 1
        print("Rolled a \(diceRoll)")
    }
    func gameDidEnd(_ game: DiceGame) {
        print("The game lasted for \(numberOfTurns) turns")
    }
}
```

DiceGameTracker implements all three methods required by DiceGameDelegate. It uses these methods to keep track of the number of turns a game has taken. It resets a numberOfTurns property to zero when the game starts, increments it each time a new turn begins, and prints out the total number of turns once the game has ended.
> DiceGameTracker는 DiceGameDelegate에 필요한 세 가지 메서드를 모두 구현합니다. 이 방법을 사용하여 게임의 턴 수를 추적합니다. 게임이 시작될 때 numberOfTurns 프로퍼티를 0으로 재설정하고 새 턴이 시작될 때마다 증가하고 게임이 종료되면 총 턴 수를 print합니다.

Here’s how DiceGameTracker looks in action:
> DiceGameTracker의 작동 방식은 다음과 같습니다.


```swift
let tracker = DiceGameTracker()
let game = SnakesAndLadders()
game.delegate = tracker
game.play()
// Started a new game of Snakes and Ladders
// The game is using a 6-sided dice
// Rolled a 3
// Rolled a 5
// Rolled a 4
// Rolled a 5
// The game lasted for 4 turns
```

## Adding Protocol Conformance with an Extension

You can extend an existing type to adopt and conform to a new protocol, even if you don’t have access to the source code for the existing type. Extensions can add new properties, methods, and subscripts to an existing type, and are therefore able to add any requirements that a protocol may demand. For more about extensions, see Extensions.
> 기존 유형의 소스 코드에 액세스 할 수없는 경우에도 기존 유형을 확장하여 새 프로토콜을 채택하고 준수 할 수 있습니다. Extension은 기존 타입에 새 프로퍼티, 메서드 및 섭스크립트를 추가 할 수 있으므로 프로토콜이 요구할 수있는 모든 요구 사항을 추가 할 수 있습니다. Extension에 대한 자세한 내용은 Extension을 참조하십시오.

```swift
protocol TextRepresentable {
    var textualDescription: String { get }
}

extension Dice: TextRepresentable {
    var textualDescription: String {
        return "A \(sides)-sided dice"
    }
}

let d12 = Dice(sides: 12, generator: LinearCongruentialGenerator())
print(d12.textualDescription)
// Prints "A 12-sided dice"
```  

### Conditionally Conforming to a Protocol

A generic type may be able to satisfy the requirements of a protocol only under certain conditions, such as when the type’s generic parameter conforms to the protocol. You can make a generic type conditionally conform to a protocol by listing constraints when extending the type. Write these constraints after the name of the protocol you’re adopting by writing a generic where clause. For more about generic where clauses, see Generic Where Clauses.
> generic type은 타입의 일반 매개 변수가 프로토콜을 준수하는 경우와 같은 특정 조건에서만 프로토콜의 요구 사항을 충족시킬 수 있습니다. 타입을 확장 할 때 제약 조건을 나열하여 제네릭 타입이 프로토콜을 조건부로 준수하도록 만들 수 있습니다. 일반적인 where 절을 작성하여 채택중인 프로토콜의 이름 뒤에 이러한 제약 조건을 작성합니다. 일반 where 절에 대한 자세한 내용은 Generic Where 절을 참조하십시오.

The following extension makes Array instances conform to the TextRepresentable protocol whenever they store elements of a type that conforms to TextRepresentable.
> 다음 extension은 Array 인스턴스가 TextRepresentable을 준수하는 타입의 요소를 저장할 때마다 TextRepresentable 프로토콜을 준수하도록합니다.

```swift
extension Array: TextRepresentable where Element: TextRepresentable {
    var textualDescription: String {
        let itemsAsText = self.map { $0.textualDescription }
        return "[" + itemsAsText.joined(separator: ", ") + "]"
    }
}
let myDice = [d6, d12]
print(myDice.textualDescription)
// Prints "[A 6-sided dice, A 12-sided dice]"
```  

### Declaring Protocol Adoption with an Extension

If a type already conforms to all of the requirements of a protocol, but hasn’t yet stated that it adopts that protocol, you can make it adopt the protocol with an empty extension:
> 타입이 이미 프로토콜의 모든 요구 사항을 준수하지만 해당 프로토콜을 채택한다고 아직 명시하지 않은 경우 빈 확장자가있는 프로토콜을 채택하도록 만들 수 있습니다.

```swift
struct Hamster {
    var name: String
    var textualDescription: String {
        return "A hamster named \(name)"
    }
}
extension Hamster: TextRepresentable {}
```  

Instances of Hamster can now be used wherever TextRepresentable is the required type:
> 이제 TextRepresentable이 required type이면 어디에서나 Hamster 인스턴스를 사용할 수 있습니다.

```swift
let simonTheHamster = Hamster(name: "Simon")
let somethingTextRepresentable: TextRepresentable = simonTheHamster
print(somethingTextRepresentable.textualDescription)
// Prints "A hamster named Simon"
```  

NOTE
Types don’t automatically adopt a protocol just by satisfying its requirements. They must always explicitly declare their adoption of the protocol.
> 타입은 요구 사항을 충족한다고해서 프로토콜을 자동으로 채택하지 않습니다. 그들은 항상 프로토콜 채택을 명시 적으로 선언해야합니다.


## Adopting a Protocol Using a Synthesized Implementation

Swift can automatically provide the protocol conformance for Equatable, Hashable, and Comparable in many simple cases. Using this synthesized implementation means you don’t have to write repetitive boilerplate code to implement the protocol requirements yourself.
> Swift는 많은 간단한 경우에 Equatable, Hashable 및 Comparable에 대한 프로토콜 적합성을 자동으로 제공 할 수 있습니다. 이렇게 synthesized된 구현을 사용하면 프로토콜 요구 사항을 직접 구현하기 위해 반복적인 상용구 코드를 작성할 필요가 없습니다.

Swift provides a synthesized implementation of Equatable for the following kinds of custom types:
> Swift는 커스텀 타입에 대해 Equatable의 synthesized된 구현을 제공합니다.

* Structures that have only stored properties that conform to the Equatable protocol (Equatable 프로토콜을 준수하는 저장 프로퍼티만 있는 구조체)
* Enumerations that have only associated types that conform to the Equatable protocol (Equatable 프로토콜을 준수하는 associated type 만있는 열거형)
* Enumerations that have no associated types (associated type이 없는 열거형)

To receive a synthesized implementation of ==, declare conformance to Equatable in the file that contains the original declaration, without implementing an == operator yourself. The Equatable protocol provides a default implementation of !=.
> ==의 synthesized 구현을 수신하려면 == 연산자를 직접 구현하지 않고 원래 선언이 포함 된 파일에서 Equatable 을 선언하십시오. Equatable 프로토콜은 !=의 기본 구현을 제공합니다.

The example below defines a Vector3D structure for a three-dimensional position vector (x, y, z), similar to the Vector2D structure. Because the x, y, and z properties are all of an Equatable type, Vector3D receives synthesized implementations of the equivalence operators.
> 아래 예제는 Vector2D 구조체와 유사한 3차원 위치 벡터 (x, y, z)에 대한 Vector3D 구조체를 정의합니다. x, y 및 z 프로퍼티는 모두 Equatable 타입이므로 Vector3D는 등가 연산자의 합성 된 구현을 수신합니다. (예제 통해 확인)

```swift
struct Vector3D: Equatable {
    var x = 0.0, y = 0.0, z = 0.0
}

let twoThreeFour = Vector3D(x: 2.0, y: 3.0, z: 4.0)
let anotherTwoThreeFour = Vector3D(x: 2.0, y: 3.0, z: 4.0)
if twoThreeFour == anotherTwoThreeFour {
    print("These two vectors are also equivalent.")
}
// Prints "These two vectors are also equivalent."
```  

Swift provides a synthesized implementation of Hashable for the following kinds of custom types:
> Swift는 다음과 같은 커스텀 타입에 대해 synthesized된 Hashable 구현을 제공합니다.

* Structures that have only stored properties that conform to the Hashable protocol (Hashable 프로토콜을 준수하는 저장 프로퍼티만있는 구조)
* Enumerations that have only associated types that conform to the Hashable protocol (Hashable 프로토콜을 준수하는 associated type만 있는 열거형)
* Enumerations that have no associated types (associated type이없는 열거형)

To receive a synthesized implementation of hash(into:), declare conformance to Hashable in the file that contains the original declaration, without implementing a hash(into:) method yourself.
> hash(into :)의 synthesized 된 구현을 수신하려면 hash(into :) 메서드를 직접 구현하지 않고 원래 선언이 포함 된 파일에서 Hashable 을 선언합니다.

Swift provides a synthesized implementation of Comparable for enumerations that don’t have a raw value. If the enumeration has associated types, they must all conform to the Comparable protocol. To receive a synthesized implementation of <, declare conformance to Comparable in the file that contains the original enumeration declaration, without implementing a < operator yourself. The Comparable protocol’s default implementation of <=, >, and >= provides the remaining comparison operators.
> Swift는 원시 값이 없는 열거형을 위해 Comparable의 synthesized된 구현을 제공합니다. 열거형에 associated type이 있는 경우 모두 Comparable 프로토콜을 준수해야합니다. < 연산자를 직접 구현하지 않고 < 의 synthesized된 구현을 수신하려면 원래 열거형 선언이 포함 된 파일에서 Comparable에 대한 적합성을 선언하십시오. Comparable 프로토콜의 기본 구현 인 < =,> 및> =는 나머지 비교 연산자를 제공합니다. (예제로 확인)

The example below defines a SkillLevel enumeration with cases for beginners, intermediates, and experts. Experts are additionally ranked by the number of stars they have.
> 아래 예제는 초보자, 중급자 및 전문가를위한 사례가있는 SkillLevel 열거형을 정의합니다. 전문가들은 그들이 가진 별의 수에 따라 추가적으로 순위가 매겨집니다.

```swift
enum SkillLevel: Comparable {
    case beginner
    case intermediate
    case expert(stars: Int)
}
var levels = [SkillLevel.intermediate, SkillLevel.beginner,
              SkillLevel.expert(stars: 5), SkillLevel.expert(stars: 3)]
for level in levels.sorted() {
    print(level)
}
// Prints "beginner"
// Prints "intermediate"
// Prints "expert(stars: 3)"
// Prints "expert(stars: 5)"
```  

## Collections of Protocol Types

A protocol can be used as the type to be stored in a collection such as an array or a dictionary, as mentioned in Protocols as Types. This example creates an array of TextRepresentable things:
> 프로토콜은 타입으로서의 프로토콜에 언급 된대로 배열 또는 딕셔너리와 같은 컬렉션에 저장 될 유형으로 사용할 수 있습니다. 이 예제는 TextRepresentable 배열을 만듭니다.

```swift
let things: [TextRepresentable] = [game, d12, simonTheHamster]

for thing in things {
    print(thing.textualDescription)
}
// A game of Snakes and Ladders with 25 squares
// A 12-sided dice
// A hamster named Simon
```  

Note that the thing constant is of type TextRepresentable. It’s not of type Dice, or DiceGame, or Hamster, even if the actual instance behind the scenes is of one of those types. Nonetheless, because it’s of type TextRepresentable, and anything that’s TextRepresentable is known to have a textualDescription property, it’s safe to access thing.textualDescription each time through the loop.
> thing constant는 TextRepresentable 타입입니다. Dice, DiceGame 또는 Hamster 타입이 아닙니다. 실제 인스턴스가 이러한 타입 중 하나 인 경우에도 마찬가지입니다. 그럼에도 불구하고 TextRepresentable 타입이고 TextRepresentable은 textualDescription 프로퍼티가 있는 것으로 알려져 있기 때문에 루프를 통해 매번 thing.textualDescription에 액세스하는 것이 안전합니다.

## Protocol Inheritance

A protocol can inherit one or more other protocols and can add further requirements on top of the requirements it inherits. The syntax for protocol inheritance is similar to the syntax for class inheritance, but with the option to list multiple inherited protocols, separated by commas:
> 프로토콜은 하나 이상의 다른 프로토콜을 상속 할 수 있으며 상속하는 요구 사항에 추가 요구 사항을 추가 할 수 있습니다. 프로토콜 상속 구문은 클래스 상속 구문과 유사하지만 여러 상속 프로토콜을 쉼표로 구분하여 나열하는 옵션이 있습니다.

```swift
protocol InheritingProtocol: SomeProtocol, AnotherProtocol {
    // protocol definition goes here
}

protocol PrettyTextRepresentable: TextRepresentable {
    var prettyTextualDescription: String { get }
}
``` 

This example defines a new protocol, PrettyTextRepresentable, which inherits from TextRepresentable. Anything that adopts PrettyTextRepresentable must satisfy all of the requirements enforced by TextRepresentable, plus the additional requirements enforced by PrettyTextRepresentable. In this example, PrettyTextRepresentable adds a single requirement to provide a gettable property called prettyTextualDescription that returns a String.
> 이 예제는 TextRepresentable에서 상속되는 새 프로토콜 PrettyTextRepresentable을 정의합니다. PrettyTextRepresentable을 채택하는 모든 것은 TextRepresentable에서 시행하는 모든 요구 사항과 PrettyTextRepresentable에서 시행하는 추가 요구 사항을 충족해야합니다. 이 예에서 PrettyTextRepresentable은 String을 반환하는 prettyTextualDescription이라는 gettable 속성을 제공하기위한 단일 요구 사항을 추가합니다.

```swift
extension SnakesAndLadders: PrettyTextRepresentable {
    var prettyTextualDescription: String {
        var output = textualDescription + ":\n"
        for index in 1...finalSquare {
            switch board[index] {
            case let ladder where ladder > 0:
                output += "▲ "
            case let snake where snake < 0:
                output += "▼ "
            default:
                output += "○ "
            }
        }
        return output
    }
}

print(game.prettyTextualDescription)
// A game of Snakes and Ladders with 25 squares:
// ○ ○ ▲ ○ ○ ▲ ○ ○ ▲ ▲ ○ ○ ○ ▼ ○ ○ ○ ○ ▼ ○ ○ ▼ ○ ▼ ○
``` 

## Class-Only Protocols

You can limit protocol adoption to class types (and not structures or enumerations) by adding the AnyObject protocol to a protocol’s inheritance list.
> 프로토콜의 상속 목록에 AnyObject 프로토콜을 추가하여 프로토콜 채택을 클래스 유형 (구조 또는 열거 형이 아님)으로 제한 할 수 있습니다.

```swift
protocol SomeClassOnlyProtocol: AnyObject, SomeInheritedProtocol {
    // class-only protocol definition goes here
}
``` 

In the example above, SomeClassOnlyProtocol can only be adopted by class types. It’s a compile-time error to write a structure or enumeration definition that tries to adopt SomeClassOnlyProtocol.
> 위의 예에서 SomeClassOnlyProtocol은 클래스 타입에 의해서만 채택 될 수 있습니다. SomeClassOnlyProtocol을 채택하려는 구조체 또는 열거형 정의를 작성하는 것은 컴파일 오류가 발생합니다.


## Protocol Composition

It can be useful to require a type to conform to multiple protocols at the same time. You can combine multiple protocols into a single requirement with a protocol composition. Protocol compositions behave as if you defined a temporary local protocol that has the combined requirements of all protocols in the composition. Protocol compositions don’t define any new protocol types.
> 동시에 여러 프로토콜을 준수하는 타입을 요구하는 것이 유용 할 수 있습니다. 프로토콜 구성을 사용하여 여러 프로토콜을 단일 요구 사항으로 결합 할 수 있습니다. 프로토콜 컴포지션은 컴포지션의 모든 프로토콜에 대한 요구 사항이 결합 된 임시 로컬 프로토콜을 정의한 것처럼 작동합니다. 프로토콜 구성은 새로운 프로토콜 타입을 정의하지 않습니다.

Protocol compositions have the form SomeProtocol & AnotherProtocol. You can list as many protocols as you need, separating them with ampersands (&). In addition to its list of protocols, a protocol composition can also contain one class type, which you can use to specify a required superclass.
> 프로토콜 구성은 SomeProtocol 및 AnotherProtocol 형식입니다. 앰퍼샌드 (&)로 구분하여 필요한만큼 프로토콜을 나열 할 수 있습니다. 프로토콜 목록 외에도 프로토콜 구성에는 필요한 수퍼 클래스를 지정하는 데 사용할 수있는 하나의 클래스 유형이 포함될 수 있습니다.

Here’s an example that combines two protocols called Named and Aged into a single protocol composition requirement on a function parameter:
> 다음은 Named 및 Aged라는 두 프로토콜을 함수 매개 변수에 대한 단일 프로토콜 구성 요구 사항으로 결합한 예입니다.

```swift
protocol Named {
    var name: String { get }
}
protocol Aged {
    var age: Int { get }
}
struct Person: Named, Aged {
    var name: String
    var age: Int
}
func wishHappyBirthday(to celebrator: Named & Aged) {
    print("Happy birthday, \(celebrator.name), you're \(celebrator.age)!")
}
let birthdayPerson = Person(name: "Malcolm", age: 21)
wishHappyBirthday(to: birthdayPerson)
// Prints "Happy birthday, Malcolm, you're 21!"
``` 

In this example, the Named protocol has a single requirement for a gettable String property called name. The Aged protocol has a single requirement for a gettable Int property called age. Both protocols are adopted by a structure called Person.
> 이 예제에서 Named 프로토콜에는 name이라는 gettable String 속성에 대한 단일 요구 사항이 있습니다. Aged 프로토콜에는 age라는 gettable Int 속성에 대한 단일 요구 사항이 있습니다. 두 프로토콜 모두 Person이라는 구조체에서 채택됩니다.

The example also defines a wishHappyBirthday(to:) function. The type of the celebrator parameter is Named & Aged, which means “any type that conforms to both the Named and Aged protocols.” It doesn’t matter which specific type is passed to the function, as long as it conforms to both of the required protocols.
> 이 예제는 wishHappyBirthday(to :) 함수도 정의합니다. celebrator 매개 변수의 타입은 Named & Aged이며, 이는 "Named 및 Aged 프로토콜을 모두 준수하는 모든 타입"을 의미합니다. 필수 프로토콜을 모두 준수하는 한 함수에 전달되는 특정 타입은 중요하지 않습니다.

The example then creates a new Person instance called birthdayPerson and passes this new instance to the wishHappyBirthday(to:) function. Because Person conforms to both protocols, this call is valid, and the wishHappyBirthday(to:) function can print its birthday greeting.
> 그런 다음이 예제는 birthdayPerson이라는 새 Person 인스턴스를 만들고 이 새 인스턴스를 wishHappyBirthday(to :) 함수에 전달합니다. Person은 두 프로토콜을 모두 따르기 때문에이 호출은 유효하며 wishHappyBirthday(to :) 함수는 생일 인사말을 인쇄 할 수 있습니다.

Here’s an example that combines the Named protocol from the previous example with a Location class:
> 다음은 이전 예의 Named 프로토콜을 Location 클래스와 결합한 예입니다.

```swift
class Location {
    var latitude: Double
    var longitude: Double
    init(latitude: Double, longitude: Double) {
        self.latitude = latitude
        self.longitude = longitude
    }
}
class City: Location, Named {
    var name: String
    init(name: String, latitude: Double, longitude: Double) {
        self.name = name
        super.init(latitude: latitude, longitude: longitude)
    }
}
func beginConcert(in location: Location & Named) {
    print("Hello, \(location.name)!")
}

let seattle = City(name: "Seattle", latitude: 47.6, longitude: -122.3)
beginConcert(in: seattle)
// Prints "Hello, Seattle!"
```

The beginConcert(in:) function takes a parameter of type Location & Named, which means “any type that’s a subclass of Location and that conforms to the Named protocol.” In this case, City satisfies both requirements.
> beginConcert(in :) 함수는 "Location의 하위 클래스이고 Named 프로토콜을 준수하는 모든 타입"을 의미하는 Location & Named 타입의 매개 변수를 사용합니다. 이 경우에 City는 두 가지 요건을 모두 충족합니다.


## Checking for Protocol Conformance

You can use the is and as operators described in Type Casting to check for protocol conformance, and to cast to a specific protocol. Checking for and casting to a protocol follows exactly the same syntax as checking for and casting to a type:
> 타입 캐스팅에 설명 된 is 및 as 연산자를 사용하여 프로토콜 적합성을 확인하고 특정 프로토콜로 캐스팅 할 수 있습니다. 프로토콜을 확인하고 캐스트하는 것은 타입을 확인하고 캐스트하는 것과 정확히 동일한 구문을 따릅니다.

```swift
protocol HasArea {
    var area: Double { get }
}

class Circle: HasArea {
    let pi = 3.1415927
    var radius: Double
    var area: Double { return pi * radius * radius }
    init(radius: Double) { self.radius = radius }
}

class Country: HasArea {
    var area: Double
    init(area: Double) { self.area = area }
}

class Animal {
    var legs: Int
    init(legs: Int) { self.legs = legs }
}

let objects: [AnyObject] = [
    Circle(radius: 2.0),
    Country(area: 243_610),
    Animal(legs: 4)
]

for object in objects {
    if let objectWithArea = object as? HasArea {
        print("Area is \(objectWithArea.area)")
    } else {
        print("Something that doesn't have an area")
    }
}
// Area is 12.5663708
// Area is 243610.0
// Something that doesn't have an area
```


## Optional Protocol Requirements

You can define optional requirements for protocols. These requirements don’t have to be implemented by types that conform to the protocol. Optional requirements are prefixed by the optional modifier as part of the protocol’s definition. Optional requirements are available so that you can write code that interoperates with Objective-C. Both the protocol and the optional requirement must be marked with the @objc attribute. Note that @objc protocols can be adopted only by classes that inherit from Objective-C classes or other @objc classes. They can’t be adopted by structures or enumerations.
> 프로토콜에 대한 선택적 요구사항을 정의 할 수 있습니다. 이러한 요구 사항은 프로토콜을 준수하는 유형으로 구현할 필요가 없습니다. Optional은 프로토콜 정의의 일부로 optional이 앞에 붙습니다. Objective-C와 상호 운용되는 코드를 작성할 수 있도록 Optional을 사용할 수 있습니다. 프로토콜과 optional은 모두 @objc 속성으로 표시되어야합니다. @objc 프로토콜은 Objective-C 클래스 또는 다른 @objc 클래스에서 상속 된 클래스에서만 채택 할 수 있습니다. 구조체 나 열거형에 채택 될 수 없습니다.

When you use a method or property in an optional requirement, its type automatically becomes an optional. For example, a method of type (Int) -> String becomes ((Int) -> String)?. Note that the entire function type is wrapped in the optional, not the method’s return value.
> optional에서 메서드 또는 프로퍼티를 사용하면 해당 타입이 자동으로 optional이 됩니다. 예를 들어, (Int)-> String 타입의 메서드는 ((Int)-> String)?이됩니다. 전체 함수 타입은 메서드의 반환 값이 아니라 optional로 래핑됩니다.

The following example defines an integer-counting class called Counter, which uses an external data source to provide its increment amount. This data source is defined by the CounterDataSource protocol, which has two optional requirements:
> 다음 예제에서는 외부 데이터 소스를 사용하여 증분량을 제공하는 Counter라는 정수 계산 클래스를 정의합니다. 이 데이터 원본은 CounterDataSource 프로토콜에 의해 정의되며 다음과 같은 두 가지 선택적 요구 사항이 있습니다.

```swift
@objc protocol CounterDataSource {
    @objc optional func increment(forCount count: Int) -> Int
    @objc optional var fixedIncrement: Int { get }
}
```

The Counter class, defined below, has an optional dataSource property of type CounterDataSource?:
> 아래에 정의 된 Counter 클래스에는 CounterDataSource? 타입의 optiona dataSource 프로퍼티가 있습니다.

```swift
class Counter {
    var count = 0
    var dataSource: CounterDataSource?
    func increment() {
        if let amount = dataSource?.increment?(forCount: count) {
            count += amount
        } else if let amount = dataSource?.fixedIncrement {
            count += amount
        }
    }
}
```

The Counter class stores its current value in a variable property called count. The Counter class also defines a method called increment, which increments the count property every time the method is called.
> Counter 클래스는 현재 값을 count라는 변수 프로퍼티에 저장합니다. Counter 클래스는 또한 메서드가 호출 될 때마다 count 프로퍼티를 증가시키는 increment라는 메서드를 정의합니다.

The increment() method first tries to retrieve an increment amount by looking for an implementation of the increment(forCount:) method on its data source. The increment() method uses optional chaining to try to call increment(forCount:), and passes the current count value as the method’s single argument.
> increment() 메서드는 먼저 데이터 소스에서 increment(forCount :) 메서드의 구현을 찾아서 증가량을 검색하려고합니다. increment() 메서드는 선택적인 연결을 사용하여 increment (forCount :) 호출을 시도하고 현재 개수 값을 메서드의 단일 인수로 전달합니다.

Note that two levels of optional chaining are at play here. First, it’s possible that dataSource may be nil, and so dataSource has a question mark after its name to indicate that increment(forCount:) should be called only if dataSource isn’t nil. Second, even if dataSource does exist, there’s no guarantee that it implements increment(forCount:), because it’s an optional requirement. Here, the possibility that increment(forCount:) might not be implemented is also handled by optional chaining. The call to increment(forCount:) happens only if increment(forCount:) exists—that is, if it isn’t nil. This is why increment(forCount:) is also written with a question mark after its name.
> 여기서는 두 가지 수준의 optional chaining이 사용됩니다. 첫째, dataSource가 nil 일 수 있으므로 dataSource는 이름 뒤에 물음표가있어 dataSource가 nil이 아닌 경우에만 increment (forCount :)를 호출해야 함을 나타냅니다. 둘째, dataSource가 존재하더라도 선택적인 요구 사항이므로 increment(forCount :)를 구현한다는 보장이 없습니다. 여기서 increment(forCount :)가 구현되지 않을 가능성도 선택적 체인으로 처리됩니다. increment(forCount :)에 대한 호출은 increment(forCount :)가 존재하는 경우, 즉 nil이 아닌 경우에만 발생합니다. 이것이 increment(forCount :) 또한 이름 뒤에 물음표와 함께 쓰여지는 이유입니다.


## Protocol Extensions

Protocols can be extended to provide method, initializer, subscript, and computed property implementations to conforming types. This allows you to define behavior on protocols themselves, rather than in each type’s individual conformance or in a global function.
> 프로토콜을 확장하여 메서드, 이니셜라이저, 섭스크립트 및 계산 프로퍼티 구현을 준수 유형에 제공 할 수 있습니다. 이를 통해 각 유형의 개별 적합성 또는 전역 기능이 아닌 프로토콜 자체에 대한 동작을 정의 할 수 있습니다.

For example, the RandomNumberGenerator protocol can be extended to provide a randomBool() method, which uses the result of the required random() method to return a random Bool value:
> 예를 들어 RandomNumberGenerator 프로토콜은 임의의 Bool 값을 반환하기 위해 필요한 random() 메서드의 결과를 사용하는 randomBool() 메서드를 제공하도록 확장 될 수 있습니다.

```swift
extension RandomNumberGenerator {
    func randomBool() -> Bool {
        return random() > 0.5
    }
}

let generator = LinearCongruentialGenerator()
print("Here's a random number: \(generator.random())")
// Prints "Here's a random number: 0.3746499199817101"
print("And here's a random Boolean: \(generator.randomBool())")
// Prints "And here's a random Boolean: true"
```

By creating an extension on the protocol, all conforming types automatically gain this method implementation without any additional modification.
> 프로토콜에 대한 extension을 생성함으로써 모든 준수 유형은 추가 수정없이이 메서드 구현을 자동으로 얻습니다.

Protocol extensions can add implementations to conforming types but can’t make a protocol extend or inherit from another protocol. Protocol inheritance is always specified in the protocol declaration itself.
> 프로토콜 확장은 준수 유형에 구현을 추가 할 수 있지만 프로토콜을 확장하거나 다른 프로토콜에서 상속 할 수는 없습니다. 프로토콜 상속은 항상 프로토콜 선언 자체에 지정됩니다.

### Providing Default Implementations

You can use protocol extensions to provide a default implementation to any method or computed property requirement of that protocol. If a conforming type provides its own implementation of a required method or property, that implementation will be used instead of the one provided by the extension.
> protocol extension을 사용하여 해당 프로토콜의 모든 메서드 또는 계산 프로퍼티 요구 사항에 기본 구현을 제공 할 수 있습니다. 준수 타입이 필수 메서드 또는 속성의 자체 구현을 제공하는 경우 extension에서 제공하는 구현 대신 해당 구현이 사용됩니다.


### Adding Constraints to Protocol Extensions

When you define a protocol extension, you can specify constraints that conforming types must satisfy before the methods and properties of the extension are available. You write these constraints after the name of the protocol you’re extending by writing a generic where clause. For more about generic where clauses, see Generic Where Clauses.
> protocol extension을 정의 할 때 extension의 메서드와 프로퍼티를 사용할 수 있기 전에 준수 형식이 충족해야하는 제약 조건을 지정할 수 있습니다. 일반적인 where 절을 작성하여 확장하려는 프로토콜의 이름 뒤에 이러한 제약 조건을 작성합니다. 일반 where 절에 대한 자세한 내용은 Generic Where 절을 참조하십시오.

For example, you can define an extension to the Collection protocol that applies to any collection whose elements conform to the Equatable protocol. By constraining a collection’s elements to the Equatable protocol, a part of the standard library, you can use the == and != operators to check for equality and inequality between two elements.
> 예를 들어 요소가 Equatable 프로토콜을 준수하는 컬렉션에 적용되는 컬렉션 프로토콜에 대한 extension을 정의 할 수 있습니다. 컬렉션의 요소를 표준 라이브러리의 일부인 Equatable 프로토콜로 제한하면 == 및 != 연산자를 사용하여 두 요소 간의 같음과 같지 않음을 확인할 수 있습니다.

```swift
extension Collection where Element: Equatable {
    func allEqual() -> Bool {
        for element in self {
            if element != self.first {
                return false
            }
        }
        return true
    }
}
```

The allEqual() method returns true only if all the elements in the collection are equal.
> allEqual() 메서드는 컬렉션의 모든 요소가 동일한 경우에만 true를 반환합니다.

```swift
let equalNumbers = [100, 100, 100, 100, 100]
let differentNumbers = [100, 100, 200, 100, 200]

print(equalNumbers.allEqual())
// Prints "true"
print(differentNumbers.allEqual())
// Prints "false"
```