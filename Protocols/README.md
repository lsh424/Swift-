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