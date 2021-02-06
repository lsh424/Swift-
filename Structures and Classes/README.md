# Structures and Classes

[Swift Docs](https://docs.swift.org/swift-book/LanguageGuide/ClassesAndStructures.html)  

## Comparing Structures and Classes

Structures and classes in Swift have many things in common. Both can:
> Swift에서 구조체와 클래스는 공통점이 많습니다. 

* Define properties to store values (값을 저장할 프로퍼티 정의 가능)
* Define methods to provide functionality (기능을 제공하는 메서드 정의 가능)
* Define subscripts to provide access to their values using subscript syntax (섭스크립트 정의 가능)
* Define initializers to set up their initial state (이니셜라이저 정의 가능)
* Be extended to expand their functionality beyond a default implementation (기능 확장 가능)
* Conform to protocols to provide standard functionality of a certain kind (프로토콜 채택 가능)

Classes have additional capabilities that structures don’t have:
> 클래스는 구조체에는 없는 추가 기능들이 있습니다.

* Inheritance enables one class to inherit the characteristics of another. (상속 가능)
* Type casting enables you to check and interpret the type of a class instance at runtime. (타입 캐스팅을 통해 클래스 인스턴스 타입 확인 및 사용 가능)
* Deinitializers enable an instance of a class to free up any resources it has assigned. (Deinitializer 사용 가능)
* Reference counting allows more than one reference to a class instance. (참조 카운팅은 클래스 인스턴스에 대해 하나 이상의 참조 허용)

The additional capabilities that classes support come at the cost of increased complexity. As a general guideline, prefer structures because they’re easier to reason about, and use classes when they’re appropriate or necessary. In practice, this means most of the custom data types you define will be structures and enumerations. For a more detailed comparison, see Choosing Between Structures and Classes.
> 클래스가 지원하는 추가 기능은 복잡성이 증가합니다. 일반적인 guideline에서는 추론하기 쉽기 때문에 구조체를 선호하고 적절하거나 필요할 때 클래스를 사용합니다. 실제로 이것은 정의하는 대부분의 사용자 정의 데이터 type들이 구조체 및 열거형 이라는 것을 의미합니다. 자세한 비교는 아래 링크 참고.

[Choosing Between Structures and Classes](https://developer.apple.com/documentation/swift/choosing_between_structures_and_classes)

## Structures and Enumerations Are Value Types

A value type is a type whose value is copied when it’s assigned to a variable or constant, or when it’s passed to a function.
> value type은 변수 또는 상수에 할당되거나 함수에 전달 될 때 값이 복사되는 유형입니다.

You’ve actually been using value types extensively throughout the previous chapters. In fact, all of the basic types in Swift—integers, floating-point numbers, Booleans, strings, arrays and dictionaries—are value types, and are implemented as structures behind the scenes.
> 실제로 이전 장에서 value type을 광범위하게 사용했습니다. 실제로 Swift의 모든 기본 유형 (정수, 부동 소수점 숫자, 부울, 문자열, 배열 및 딕셔너리)은 value type이며 구조체로 구현됩니다.

All structures and enumerations are value types in Swift. This means that any structure and enumeration instances you create—and any value types they have as properties—are always copied when they are passed around in your code.
> 모든 구조체와 열거형은 value type입니다. 즉, 사용자가 만든 모든 구조체 및 열거형 인스턴스와 속성으로 갖는 모든 value type은 코드에서 전달 될 때 항상 복사됩니다.


## Identity Operators

Because classes are reference types, it’s possible for multiple constants and variables to refer to the same single instance of a class behind the scenes. (The same isn’t true for structures and enumerations, because they are always copied when they are assigned to a constant or variable, or passed to a function.)
> 클래스는 참조 유형이므로 여러 상수와 변수가 배후에서 클래스의 동일한 단일 인스턴스를 참조 할 수 있습니다. (구조체 및 열거형은 상수 또는 변수에 할당되거나 함수에 전달 될 때 항상 복사되므로 동일하지 않습니다.)

It can sometimes be useful to find out whether two constants or variables refer to exactly the same instance of a class. To enable this, Swift provides two identity operators:
> 두 개의 상수 또는 변수가 정확히 동일한 클래스 인스턴스를 참조하는지 확인하는 것이 유용 할 수 있습니다. Swift는 두 가지 identity 연산자를 제공합니다.

* dentical to (===)
* Not identical to (!==)

Use these operators to check whether two constants or variables refer to the same single instance:
> 이 연산자들을 사용하여 두 개의 상수 또는 변수가 동일한 단일 인스턴스를 참조하는지 확인합니다.

```swift
if tenEighty === alsoTenEighty {
print("tenEighty and alsoTenEighty refer to the same VideoMode instance.")
} // Prints "tenEighty and alsoTenEighty refer to the same VideoMode instance."
```  

