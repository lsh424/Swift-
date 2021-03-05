# Initialization

[Swift Docs](https://docs.swift.org/swift-book/LanguageGuide/Initialization.html)  

Initialization is the process of preparing an instance of a class, structure, or enumeration for use. This process involves setting an initial value for each stored property on that instance and performing any other setup or initialization that’s required before the new instance is ready for use.
> 초기화는 사용할 클래스, 구조체 또는 열거형의 인스턴스를 준비하는 프로세스입니다. 이 프로세스에는 해당 인스턴스에 저장된 각 프로퍼티의 초기 값을 설정하고 새 인스턴스를 사용할 준비가되기 전에 필요한 다른 설정 또는 초기화를 수행하는 작업이 포함됩니다.

You implement this initialization process by defining initializers, which are like special methods that can be called to create a new instance of a particular type. Unlike Objective-C initializers, Swift initializers don’t return a value. Their primary role is to ensure that new instances of a type are correctly initialized before they’re used for the first time.
> 특정 타입의 새 인스턴스를 만들기 위해 호출 할 수있는 이니셜라이저를 정의하여이 초기화 프로세스를 구현합니다. Objective-C 이니셜 라이저와 달리 Swift 이니셜 라이저는 값을 반환하지 않습니다. 기본 역할은 타입의 새 인스턴스가 처음 사용되기 전에 올바르게 초기화되었는지 확인하는 것입니다.

Instances of class types can also implement a deinitializer, which performs any custom cleanup just before an instance of that class is deallocated. 
> 클래스 타입의 인스턴스는 해당 클래스의 인스턴스가 할당 해제되기 직전에 사용자 지정 정리를 수행하는 deinitializer를 구현할수도 있습니다. 

## Setting Initial Values for Stored Properties

Classes and structures must set all of their stored properties to an appropriate initial value by the time an instance of that class or structure is created. Stored properties can’t be left in an indeterminate state.
> 클래스 및 구조체는 해당 클래스 또는 구조체의 인스턴스가 생성 될 때까지 저장프로퍼티를 적절한 초기값으로 설정해야합니다. 저장프로퍼티는 불확실한 상태로 남을 수 없습니다.

You can set an initial value for a stored property within an initializer, or by assigning a default property value as part of the property’s definition. These actions are described in the following sections.
> 이니셜라이저 내에서 저장프로퍼티의 초기 값을 설정하거나 프로퍼티 정의의 일부로 기본 프로퍼티 값을 할당 할 수 있습니다. 이러한 작업은 다음 섹션에서 설명합니다.


## Initializers

Initializers are called to create a new instance of a particular type. In its simplest form, an initializer is like an instance method with no parameters, written using the init keyword:
> 이니셜라이저는 특정 타입의 새 인스턴스를 만들기 위해 호출됩니다. 가장 간단한 형태로 이니셜라이저는 매개 변수가 없는 인스턴스 메소드와 같으며 init 키워드를 사용하여 작성됩니다.

```swift
init() {
    // perform some initialization here
}
```  

```swift
struct Fahrenheit {
    var temperature: Double
    init() {
        temperature = 32.0
    }
}
var f = Fahrenheit()
print("The default temperature is \(f.temperature)° Fahrenheit")
// Prints "The default temperature is 32.0° Fahrenheit"
```  

## Customizing Initialization

### Initialization Parameters

You can provide initialization parameters as part of an initializer’s definition, to define the types and names of values that customize the initialization process. 
> 초기화 프로세스를 커스터마이즈 하는 값의 이름과 타입을 정의하기 위해 이니셜라이저를 정의할 때 초기화 매개 변수를 제공 할 수 있습니다.

The following example defines a structure called Celsius, which stores temperatures expressed in degrees Celsius. The Celsius structure implements two custom initializers called init(fromFahrenheit:) and init(fromKelvin:), which initialize a new instance of the structure with a value from a different temperature scale:
> 다음 예제는 섭씨로 표현 된 온도를 저장하는 Celsius라는 구조체를 정의합니다. Celsius 구조체는 init (fromFahrenheit :) 및 init (fromKelvin :)이라는 두 개의 사용자 지정 이니셜라이저를 구현합니다.이 초기화는 다른 온도 눈금의 값으로 구조의 새 인스턴스를 초기화합니다.

```swift
struct Celsius {
    var temperatureInCelsius: Double
    init(fromFahrenheit fahrenheit: Double) {
        temperatureInCelsius = (fahrenheit - 32.0) / 1.8
    }
    init(fromKelvin kelvin: Double) {
    temperatureInCelsius = kelvin - 273.15
    }
}

let boilingPointOfWater = Celsius(fromFahrenheit: 212.0)
// boilingPointOfWater.temperatureInCelsius is 100.0
let freezingPointOfWater = Celsius(fromKelvin: 273.15)
// freezingPointOfWater.temperatureInCelsius is 0.0
``` 

### Parameter Names and Argument Labels

As with function and method parameters, initialization parameters can have both a parameter name for use within the initializer’s body and an argument label for use when calling the initializer.
> 함수 및 메서드 매개 변수와 마찬가지로 초기화 매개 변수에는 이니셜 라이저 본문 내에서 사용할 매개 변수 이름과 이니셜 라이저를 호출 할 때 사용할 인수 레이블이 모두 있을 수 있습니다.

However, initializers don’t have an identifying function name before their parentheses in the way that functions and methods do. Therefore, the names and types of an initializer’s parameters play a particularly important role in identifying which initializer should be called. Because of this, Swift provides an automatic argument label for every parameter in an initializer if you don’t provide one.
> 하지만, 이니셜라이저는 함수 및 메서드와 같이 괄호 앞에 식별 함수 이름이 없습니다. 따라서 이니셜라이저 매개 변수의 이름과 타입은 호출해야하는 이니셜 라이저를 식별하는 데 특히 중요한 역할을합니다. 이로 인해 Swift는 초기화 프로그램의 모든 매개 변수에 대해 자동 인수 레이블을 제공합니다.

```swift
struct Color {
    let red, green, blue: Double
    init(red: Double, green: Double, blue: Double) {
        self.red   = red
        self.green = green
        self.blue  = blue
    }
    init(white: Double) {
        red   = white
        green = white
        blue  = white
    }
}
``` 

Both initializers can be used to create a new Color instance, by providing named values for each initializer parameter:
> 두 이니셜라이저는 각 이니셜라이저 매개 변수에 이름이 지정된 값을 제공하여 새 Color 인스턴스를 만드는 데 사용할 수 있습니다.

```swift
let magenta = Color(red: 1.0, green: 0.0, blue: 1.0)
let halfGray = Color(white: 0.5)
``` 

Note that it isn’t possible to call these initializers without using argument labels. Argument labels must always be used in an initializer if they’re defined, and omitting them is a compile-time error:
> 인수 레이블을 사용하지 않고는 이러한 이니셜라이저를 호출 할 수 없습니다. 인수 레이블은 정의 된 경우 항상 이니셜라이저에서 사용해야하며 생략하면 컴파일 오류가 발생합니다.

```swift
let veryGreen = Color(0.0, 1.0, 0.0)
// this reports a compile-time error - argument labels are required
``` 

### Initializer Parameters Without Argument Labels

If you don’t want to use an argument label for an initializer parameter, write an underscore (_) instead of an explicit argument label for that parameter to override the default behavior.
> 이니셜라이저 매개 변수에 인수 레이블을 사용하지 않으려면 해당 매개 변수에 대한 명시 적 인수 레이블 대신 밑줄 (_)을 작성하여 기본 동작을 재정의합니다.

```swift
struct Celsius {
    var temperatureInCelsius: Double
    init(fromFahrenheit fahrenheit: Double) {
        temperatureInCelsius = (fahrenheit - 32.0) / 1.8
    }
    init(fromKelvin kelvin: Double) {
        temperatureInCelsius = kelvin - 273.15
    }
    init(_ celsius: Double) {
        temperatureInCelsius = celsius
    }
}

let bodyTemperature = Celsius(37.0)
// bodyTemperature.temperatureInCelsius is 37.0
``` 

### Optional Property Types

If your custom type has a stored property that’s logically allowed to have “no value”—perhaps because its value can’t be set during initialization, or because it’s allowed to have “no value” at some later point—declare the property with an optional type. Properties of optional type are automatically initialized with a value of nil, indicating that the property is deliberately intended to have “no value yet” during initialization.
> custom type에 논리적으로 "값 없음"을 가질 수있는 저장프로퍼티가 있는 경우 (아마도 초기화 중에 해당 값을 설정할 수 없거나 나중에 "값 없음"을 가질 수 있기 때문에) 프로퍼티를 옵셔널타입으로 설정합니다. 옵셔널 타입 프로퍼티는 nil 값으로 자동으로 초기화됩니다. 이는 초기화 중에 프로퍼티가 의도적으로 "아직 값이 없음"을 갖도록 의도되었음을 나타냅니다.

```swift
class SurveyQuestion {
    var text: String
    var response: String?
    init(text: String) {
        self.text = text
    }

    func ask() {
        print(text)
    }
}

let cheeseQuestion = SurveyQuestion(text: "Do you like cheese?")
cheeseQuestion.ask()
// Prints "Do you like cheese?"
cheeseQuestion.response = "Yes, I do like cheese."
``` 

The response to a survey question can’t be known until it’s asked, and so the response property is declared with a type of String?, or “optional String”. It’s automatically assigned a default value of nil, meaning “no string yet”, when a new instance of SurveyQuestion is initialized.
> 설문 조사 질문에 대한 응답은 질문을받을 때까지 알 수 없으므로 응답 프로퍼티는 String? 또는 "optional String" 타입으로 선언됩니다. SurveyQuestion의 새 인스턴스가 초기화 될 때 "아직 문자열이 없음"을 의미하는 기본값 nil이 자동으로 할당됩니다.

### Memberwise Initializers for Structure Types

Structure types automatically receive a memberwise initializer if they don’t define any of their own custom initializers. Unlike a default initializer, the structure receives a memberwise initializer even if it has stored properties that don’t have default values.
> 구조체 타입은 자체 사용자 정의 이니셜라이저를 정의하지 않는 경우 자동으로 멤버별 이니셜라이저를받습니다. 기본 이니셜라이저와 달리 구조체는 기본값이없는 저장프로퍼티의 경우에도 멤버 별 이니셜 라이저를받습니다.

The memberwise initializer is a shorthand way to initialize the member properties of new structure instances. Initial values for the properties of the new instance can be passed to the memberwise initializer by name.
> 멤버 별 이니셜라이저는 새 구조체 인스턴스의 멤버 속성을 초기화하는 간단한 방법입니다. 새 인스턴스의 속성에 대한 초기 값은 이름으로 멤버 단위 이니셜라이저에 전달할 수 있습니다.

The example below defines a structure called Size with two properties called width and height. Both properties are inferred to be of type Double by assigning a default value of 0.0.
> 아래 예제는 width 및 height라는 두 가지 프로퍼티를 사용하여 Size라는 구조체를 정의합니다. 두 프로퍼티 모두 기본값 0.0을 할당하여 Double 타입으로 추정됩니다.

The Size structure automatically receives an init(width:height:) memberwise initializer, which you can use to initialize a new Size instance:
> Size 구조체는 새 Size 인스턴스를 초기화하는 데 사용할 수있는 init(width : height :) 멤버 별 이니셜라이저를 자동으로받습니다.

```swift
struct Size {
    var width = 0.0, height = 0.0
}

let twoByTwo = Size(width: 2.0, height: 2.0)
``` 

When you call a memberwise initializer, you can omit values for any properties that have default values. In the example above, the Size structure has a default value for both its height and width properties. You can omit either property or both properties, and the initializer uses the default value for anything you omit—for example:
> 멤버 단위 이니셜 라이저를 호출 할 때 기본값이있는 속성의 값을 생략 할 수 있습니다. 위의 예에서 Size 구조는 height 및 width 속성 모두에 대한 기본값을 갖습니다. 속성 중 하나 또는 두 속성을 모두 생략 할 수 있으며 이니셜 라이저는 생략 한 항목에 대해 기본값을 사용합니다. 예를 들면 다음과 같습니다.

```swift
let zeroByTwo = Size(height: 2.0)
print(zeroByTwo.width, zeroByTwo.height)
// Prints "0.0 2.0"

let zeroByZero = Size()
print(zeroByZero.width, zeroByZero.height)
// Prints "0.0 0.0"
``` 

## Initializer Delegation for Value Types

Initializers can call other initializers to perform part of an instance’s initialization. This process, known as initializer delegation, avoids duplicating code across multiple initializers.
> 이니셜라이저는 다른 이니셜라이저를 호출하여 인스턴스 초기화의 일부를 수행 할 수 있습니다. initializer delegation이라고하는 이 프로세스는 여러 이니셜라이저에서 코드가 중복되는 것을 방지합니다.

The rules for how initializer delegation works, and for what forms of delegation are allowed, are different for value types and class types. Value types (structures and enumerations) don’t support inheritance, and so their initializer delegation process is relatively simple, because they can only delegate to another initializer that they provide themselves. Classes, however, can inherit from other classes, as described in Inheritance. This means that classes have additional responsibilities for ensuring that all stored properties they inherit are assigned a suitable value during initialization. These responsibilities are described in Class Inheritance and Initialization below.
> initializer delegation이 작동하는 방식과 허용되는 delegation에 대한 규칙은 값 타입과 클래스 타입에 따라 다릅니다. 값 타입 (구조체 및 열거형)은 상속을 지원하지 않으므로 initializer delegation 프로세스는 자신이 제공하는 다른 initializer에만 위임 할 수 있기 때문에 비교적 간단합니다. 그러나 클래스는 상속에 설명 된대로 다른 클래스에서 상속 할 수 있습니다. 이는 클래스가 상속하는 모든 저장 프로퍼티에 초기화 중에 적절한 값이 할당되도록하는 추가 책임이 있음을 의미합니다. 이러한 책임은 아래 클래스 상속 및 초기화에 설명되어 있습니다.

For value types, you use self.init to refer to other initializers from the same value type when writing your own custom initializers. You can call self.init only from within an initializer.
> 값 타입의 경우 custom initializer를 작성할 때 self.init를 사용하여 동일한 값 타입의 다른 이니셜라이저를 참조합니다. 이니셜 라이저 내에서만 self.init를 호출 할 수 있습니다.

Note that if you define a custom initializer for a value type, you will no longer have access to the default initializer (or the memberwise initializer, if it’s a structure) for that type. This constraint prevents a situation in which additional essential setup provided in a more complex initializer is accidentally circumvented by someone using one of the automatic initializers.
> 값 타입에 대한 custom initializer를 정의하면 해당 타입의 기본 이니셜라이저 (또는 구조체 인 경우 memberwise initializer)에 더 이상 액세스 할 수 없습니다. 이 제약은 자동 이니셜라이저 중 하나를 사용하는 누군가가 더 복잡한 이니셜 라이저에서 제공하는 추가 필수 설정을 실수로 우회하는 상황을 방지합니다.

The following example defines a custom Rect structure to represent a geometric rectangle. The example requires two supporting structures called Size and Point, both of which provide default values of 0.0 for all of their properties:
> 다음 예제에서는 기하학적 직사각형을 나타내는 사용자 지정 Rect 구조체를 정의합니다. 이 예제에는 Size 및 Point라는 두 개의 지원 구조체가 필요하며, 둘 다 모든 프로퍼티에 대해 기본값 0.0을 제공합니다.

```swift
struct Size {
    var width = 0.0, height = 0.0
}

struct Point {
    var x = 0.0, y = 0.0
}
``` 

You can initialize the Rect structure below in one of three ways—by using its default zero-initialized origin and size property values, by providing a specific origin point and size, or by providing a specific center point and size. These initialization options are represented by three custom initializers that are part of the Rect structure’s definition:
> 아래의 Rect 구조체는 기본 0으로 초기화 된 원점 및 크기 프로퍼티 값을 사용하거나 특정 원점 및 크기를 제공하거나 특정 중심점 및 크기를 제공하는 세 가지 방법 중 하나로 초기화 할 수 있습니다. 이러한 초기화 옵션은 Rect 구조체 정의의 일부인 세 가지 사용자 지정 이니셜 라이저로 표시됩니다.

```swift
struct Rect {
    var origin = Point()
    var size = Size()
    init() {}
    init(origin: Point, size: Size) {
        self.origin = origin
        self.size = size
    }
    
    init(center: Point, size: Size) {
        let originX = center.x - (size.width / 2)
        let originY = center.y - (size.height / 2)
        self.init(origin: Point(x: originX, y: originY), size: size)
    }
}
``` 

```swift
let basicRect = Rect()
// basicRect's origin is (0.0, 0.0) and its size is (0.0, 0.0)

let originRect = Rect(origin: Point(x: 2.0, y: 2.0),
                    size: Size(width: 5.0, height: 5.0))
// originRect's origin is (2.0, 2.0) and its size is (5.0, 5.0)

let centerRect = Rect(center: Point(x: 4.0, y: 4.0),
                    size: Size(width: 3.0, height: 3.0))
// centerRect's origin is (2.5, 2.5) and its size is (3.0, 3.0)
``` 

## Class Inheritance and Initialization

Designated initializers are the primary initializers for a class. A designated initializer fully initializes all properties introduced by that class and calls an appropriate superclass initializer to continue the initialization process up the superclass chain.
> Designated initializer는 클래스의 기본 이니셜라이저입니다. Designated initializer는 해당 클래스에 의해 도입 된 모든 프로퍼티를 완전히 초기화하고 적절한 수퍼 클래스 이니셜라이저를 호출하여 수퍼 클래스 체인까지 초기화 프로세스를 계속합니다.

Classes tend to have very few designated initializers, and it’s quite common for a class to have only one. Designated initializers are “funnel” points through which initialization takes place, and through which the initialization process continues up the superclass chain.
> 클래스에는 designated initializer가 거의없는 경향이 있으며 클래스에 하나만있는 것이 일반적입니다. designated initializer는 초기화가 수행되고 초기화 프로세스가 수퍼 클래스 체인까지 계속되는 "funnel" 지점입니다.

Every class must have at least one designated initializer. In some cases, this requirement is satisfied by inheriting one or more designated initializers from a superclass, as described in Automatic Initializer Inheritance below.
> 모든 클래스에는 designated initializer가 하나 이상 있어야합니다. 경우에 따라 이 요구 사항은 아래의 자동 이니셜라이저 상속에 설명 된대로 수퍼 클래스에서 하나 이상의 designated initializer를 상속하여 충족됩니다.

Convenience initializers are secondary, supporting initializers for a class. You can define a convenience initializer to call a designated initializer from the same class as the convenience initializer with some of the designated initializer’s parameters set to default values. You can also define a convenience initializer to create an instance of that class for a specific use case or input value type.
> Convenience initializer는 보조 이니셜라이저로 클래스에 대한 이니셜라이저를 지원합니다. designated initializer의 일부 매개 변수를 기본값으로 설정하여 편의 이니셜 라이저와 동일한 클래스에서 지정된 이니셜라이저를 호출하도록 Convenience initializer를 정의 할 수 있습니다. 또한 Convenience initializer를 정의하여 특정 사용 사례 또는 입력 값 타입에 대한 해당 클래스의 인스턴스를 만들 수 있습니다.

You don’t have to provide convenience initializers if your class doesn’t require them. 
> 클래스에 필요하지 않은 경우 convenience initializer를 제공 할 필요가 없습니다. 

## Initializer Delegation for Class Types

To simplify the relationships between designated and convenience initializers, Swift applies the following three rules for delegation calls between initializers:
> designated initializer와 convenience initializer 간의 관계를 단순화하기 위해 Swift는 이니셜라이저 간의 위임 호출에 대해 다음 세 가지 규칙을 적용합니다.

Rule 1
A designated initializer must call a designated initializer from its immediate superclass.
> designated initializer는 직계 수퍼 클래스에서 지정된 이니셜 라이저를 호출해야합니다.
> 
Rule 2
A convenience initializer must call another initializer from the same class.
> convenience initializer는 동일한 클래스에서 다른 이니셜 라이저를 호출해야합니다.

Rule 3
A convenience initializer must ultimately call a designated initializer.
> convenience initializer는 궁극적으로 designated initializer를 호출해야합니다.


즉 이렇게 기억하면 좋다:

* 지정 이니셜라이저는 항상 위로 위임
* 편의 이니셜라이저는 항상 가로질러 위임

그림 추가 

Here, the superclass has a single designated initializer and two convenience initializers. One convenience initializer calls another convenience initializer, which in turn calls the single designated initializer. This satisfies rules 2 and 3 from above. The superclass doesn’t itself have a further superclass, and so rule 1 doesn’t apply.
> 여기에서 superclass에는 designated initializer 하나와 convenience initializer 두 개가 있습니다. 하나의 convenience initializer저는 다른 convenience initializer를 호출하고, 이는 차례로 designated initializer를 호출합니다. 이는 위의 규칙 2 및 3을 충족합니다. superclass 자체에는 추가적인 superclass가 없으므로 규칙 1이 적용되지 않습니다.

The subclass in this figure has two designated initializers and one convenience initializer. The convenience initializer must call one of the two designated initializers, because it can only call another initializer from the same class. This satisfies rules 2 and 3 from above. Both designated initializers must call the single designated initializer from the superclass, to satisfy rule 1 from above.
> 이 그림의 하위 클래스에는 두 개의 designated initializer와 하나의 convenience initializer가 있습니다. convenience initializer는 동일한 클래스에서 다른 이니셜 라이저만 호출 할 수 있으므로 두 designated initializer중 하나를 호출해야합니다. 이는 위의 규칙 2 및 3을 충족합니다. designated initializer는 모두 위의 규칙 1을 충족하기 위해 superclass에서 designated initializer를 호출해야합니다.

The figure below shows a more complex class hierarchy for four classes. It illustrates how the designated initializers in this hierarchy act as “funnel” points for class initialization, simplifying the interrelationships among classes in the chain:
> 아래 그림은 4 개의 클래스에 대해 보다 더 복잡한 클래스 계층 구조를 보여줍니다. 이 계층 구조에서 designated initializer가 클래스 초기화를위한 "깔때기"지점으로 작동하여 체인에있는 클래스 간의 상호 관계를 단순화하는 방법을 보여줍니다.

그림 추가 


## Initializer Inheritance and Overriding

Unlike subclasses in Objective-C, Swift subclasses don’t inherit their superclass initializers by default. Swift’s approach prevents a situation in which a simple initializer from a superclass is inherited by a more specialized subclass and is used to create a new instance of the subclass that isn’t fully or correctly initialized.
> Objective-C의 서브 클래스와 달리 Swift 서브 클래스는 기본적으로 수퍼클래스 이니셜라이저를 상속하지 않습니다. Swift의 접근 방식은 슈퍼 클래스의 간단한 초기화 프로그램이 보다 전문화 된 하위 클래스에 상속되고 완전히 또는 올바르게 초기화되지 않은 하위클래스의 새 인스턴스를 만드는 데 사용되는 상황을 방지합니다.

If you want a custom subclass to present one or more of the same initializers as its superclass, you can provide a custom implementation of those initializers within the subclass.
> 사용자 정의 서브클래스가 수퍼클래스와 동일한 이니셜 라이저 중 하나 이상을 표시하도록하려면 서브클래스 내에서 해당 이니셜라이저의 사용자 정의 구현을 제공 할 수 있습니다.

When you write a subclass initializer that matches a superclass designated initializer, you are effectively providing an override of that designated initializer. Therefore, you must write the override modifier before the subclass’s initializer definition. This is true even if you are overriding an automatically provided default initializer, as described in Default Initializers.
> 슈퍼클래스로 designated initializer와 일치하는 하위 클래스 이니셜라이저를 작성할 때 designated initializer의 오버라이드를 효과적으로 제공하는 것입니다. 따라서 하위 클래스의 이니셜라이저 정의 전에 override작성해야합니다. 기본 이니셜라이저에 설명 된대로 자동으로 제공된 기본 이니셜라이저를 재정의하는 경우에도 마찬가지입니다.

As with an overridden property, method or subscript, the presence of the override modifier prompts Swift to check that the superclass has a matching designated initializer to be overridden, and validates that the parameters for your overriding initializer have been specified as intended.
> 재정의 된 프로퍼티, 메서드 또는 섭스크립트와 마찬가지로 override가 있으면 Swift가 슈퍼클래스에 재정의 할 일치하는 designated initializer가 있는지 확인하고 재정의하는 이니셜라이저의 매개 변수가 의도 한대로 지정되었는지 확인하도록 프롬프트합니다.

Conversely, if you write a subclass initializer that matches a superclass convenience initializer, that superclass convenience initializer can never be called directly by your subclass, as per the rules described above in Initializer Delegation for Class Types. Therefore, your subclass is not (strictly speaking) providing an override of the superclass initializer. As a result, you don’t write the override modifier when providing a matching implementation of a superclass convenience initializer.
> 반대로, 수퍼클래스 convenience initializer와 일치하는 하위 클래스 이니셜라이저를 작성하는 경우, 클래스 유형에 대한 이니셜라이저 위임에 설명 된 규칙에 따라 해당 수퍼클래스 convenience initializer를 하위 클래스에서 직접 호출 할 수 없습니다. 따라서 하위 클래스는 (엄격하게 말하면) 수퍼 클래스 이니셜라이저의 재정의를 제공하지 않습니다. 결과적으로 수퍼클래스 convenience initializer의 일치하는 구현을 제공 할 때 override를 작성하지 않습니다.

The example below defines a base class called Vehicle. This base class declares a stored property called numberOfWheels, with a default Int value of 0. The numberOfWheels property is used by a computed property called description to create a String description of the vehicle’s characteristics:
> 아래 예제는 Vehicle이라는 기본 클래스를 정의합니다. 이 기본 클래스는 기본 Int 값이 0 인 numberOfWheels라는 저장 프로퍼티를 선언합니다. numberOfWheels 프로퍼티는 description이라는 계산 프로퍼티에서 차량의 특성에 대한 문자열 설명을 만드는 데 사용됩니다.

```swift
class Vehicle {
    var numberOfWheels = 0
    var description: String {
        return "\(numberOfWheels) wheel(s)"
    }
}
``` 

The Vehicle class provides a default value for its only stored property, and doesn’t provide any custom initializers itself. As a result, it automatically receives a default initializer, as described in Default Initializers. The default initializer (when available) is always a designated initializer for a class, and can be used to create a new Vehicle instance with a numberOfWheels of 0:
> Vehicle 클래스는 저장 프로퍼티에 대한 기본값을 제공하며 custom initializer를 제공하지 않습니다. 결과적으로 기본 이니셜라이저에 설명 된대로 기본 이니셜라이저를 자동으로받습니다. 기본 이니셜라이저 (사용 가능한 경우)는 항상 클래스에 대해 designated initializer이며 numberOfWheels가 0 인 새 Vehicle 인스턴스를 만드는 데 사용할 수 있습니다.

```swift
let vehicle = Vehicle()
print("Vehicle: \(vehicle.description)")
// Vehicle: 0 wheel(s)
``` 

The next example defines a subclass of Vehicle called Bicycle:
> 다음 예제는 Bicycle이라는 Vehicle의 서브 클래스를 정의합니다 :

```swift
class Bicycle: Vehicle {
    override init() {
        super.init()
        numberOfWheels = 2
    }
}
``` 

The Bicycle subclass defines a custom designated initializer, init(). This designated initializer matches a designated initializer from the superclass of Bicycle, and so the Bicycle version of this initializer is marked with the override modifier.
> Bicycle 하위 클래스는 designated initializer init()를 정의합니다. 이 designated initializer는 Bicycle의 슈퍼 클래스에서의 designated initializer와 일치하므로이 이니셜 라이저의 Bicycle 버전은 override로 표시됩니다.

The init() initializer for Bicycle starts by calling super.init(), which calls the default initializer for the Bicycle class’s superclass, Vehicle. This ensures that the numberOfWheels inherited property is initialized by Vehicle before Bicycle has the opportunity to modify the property. After calling super.init(), the original value of numberOfWheels is replaced with a new value of 2.
> Bicycle 용 init () 이니셜 라이저는 Bicycle 클래스의 수퍼 클래스 인 Vehicle에 대한 기본 이니셜 라이저를 호출하는 super.init () 호출로 시작됩니다. 이렇게하면 Bicycle이 속성을 수정할 기회가 생기기 전에 numberOfWheels 상속 프로퍼티들이 Vehicle에 의해 초기화됩니다. super.init ()를 호출 한 후 numberOfWheels의 원래 값이 새 값 2로 대체됩니다.

If you create an instance of Bicycle, you can call its inherited description computed property to see how its numberOfWheels property has been updated:
> Bicycle 인스턴스를 만드는 경우 상속 된 description 계산 프로퍼티를 호출하여 numberOfWheels 속성이 어떻게 업데이트되었는지 확인할 수 있습니다.

```swift
let bicycle = Bicycle()
print("Bicycle: \(bicycle.description)")
// Bicycle: 2 wheel(s)
``` 

If a subclass initializer performs no customization in phase 2 of the initialization process, and the superclass has a zero-argument designated initializer, you can omit a call to super.init() after assigning values to all of the subclass’s stored properties.
> 하위 클래스 이니셜라이저가 초기화 프로세스의 2 단계에서 customization를 수행하지 않고 수퍼 클래스에 인수가없는 designated initializer가있는 경우 모든 하위 클래스의 저장프로퍼티에 값을 할당 한 후 super.init() 호출을 생략 할 수 있습니다.

This example defines another subclass of Vehicle, called Hoverboard. In its initializer, the Hoverboard class sets only its color property. Instead of making an explicit call to super.init(), this initializer relies on an implicit call to its superclass’s initializer to complete the process.
> 이 예제는 Hoverboard라는 Vehicle의 또 다른 하위 클래스를 정의합니다. 이니셜라이저에서 Hoverboard 클래스는 색상 속성만 설정합니다. 이 이니셜라이저는 super.init()을 명시 적으로 호출하는 대신 수퍼 클래스의 이니셜라이저에 대한 암시적 호출을 사용하여 프로세스를 완료합니다.

```swift
class Hoverboard: Vehicle {
    var color: String
    init(color: String) {
        self.color = color
        // super.init() implicitly called here
    }
    override var description: String {
        return "\(super.description) in a beautiful \(color)"
    }
}
``` 

An instance of Hoverboard uses the default number of wheels supplied by the Vehicle initializer.
> Hoverboard의 인스턴스는 Vehicle 이니셜라이저에서 제공하는 기본 휠 수를 사용합니다.

```swift
let hoverboard = Hoverboard(color: "silver")
print("Hoverboard: \(hoverboard.description)")
// Hoverboard: 0 wheel(s) in a beautiful silver
``` 

## Automatic Initializer Inheritance

.... 추가 작성 예정 왜케 많은거야
