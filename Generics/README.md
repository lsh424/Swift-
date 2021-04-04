# Generics

[Swift Docs](https://docs.swift.org/swift-book/LanguageGuide/Generics.html)  

Generic code enables you to write flexible, reusable functions and types that can work with any type, subject to requirements that you define. You can write code that avoids duplication and expresses its intent in a clear, abstracted manner.
> Generic을 사용하면 정의한 요구 사항에 따라 모든 타입에서 작동 할 수있는 유연하고 재사용 가능한 함수 및 타입을 작성할 수 있습니다. 중복을 피하고 명확하고 추상적인 방식으로 의도를 표현하는 코드를 작성할 수 있습니다.

Generics are one of the most powerful features of Swift, and much of the Swift standard library is built with generic code. In fact, you’ve been using generics throughout the Language Guide, even if you didn’t realize it. For example, Swift’s Array and Dictionary types are both generic collections. You can create an array that holds Int values, or an array that holds String values, or indeed an array for any other type that can be created in Swift. Similarly, you can create a dictionary to store values of any specified type, and there are no limitations on what that type can be.
> Generic은 Swift의 가장 강력한 기능 중 하나이며 Swift 표준 라이브러리의 대부분은 Generic으로 빌드됩니다. 사실 몰랐더라도 언어 가이드 전체에서 제네릭을 사용하고 있습니다. 예를 들어 Swift의 Array 및 Dictionary 타입은 모두 일반 컬렉션입니다. Int 값을 보유하는 배열, String 값을 보유하는 배열 또는 실제로 Swift에서 생성 할 수있는 다른 타입에 대한 배열을 생성 할 수 있습니다. 마찬가지로 지정된 유형의 값을 저장하는 딕셔너리를 만들 수 있으며 해당 타입이 될 수있는 것에 대한 제한이 없습니다.

## The Problem That Generics Solve

Here’s a standard, nongeneric function called swapTwoInts(_:_:), which swaps two Int values:
> 다음은 두 개의 Int 값을 바꾸는 swapTwoInts(_ : _ :)라는 표준이 아닌 일반 함수입니다.

```swift
func swapTwoInts(_ a: inout Int, _ b: inout Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

The swapTwoInts(_:_:) function is useful, but it can only be used with Int values. If you want to swap two String values, or two Double values, you have to write more functions, such as the swapTwoStrings(_:_:) and swapTwoDoubles(_:_:) functions shown below:
> swapTwoInts(_ : _ :) 함수는 유용하지만 Int 값에만 사용할 수 있습니다. 두 개의 String 값 또는 두 개의 Double 값을 교체하려면 아래 표시된 swapTwoStrings(_ : _ :) 및 swapTwoDoubles(_ : _ :) 함수와 같은 더 많은 함수를 작성해야합니다.

```swift
func swapTwoStrings(_ a: inout String, _ b: inout String) {
    let temporaryA = a
    a = b
    b = temporaryA
}

func swapTwoDoubles(_ a: inout Double, _ b: inout Double) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

You may have noticed that the bodies of the swapTwoInts(_:_:), swapTwoStrings(_:_:), and swapTwoDoubles(_:_:) functions are identical. The only difference is the type of the values that they accept (Int, String, and Double).
> swapTwoInts(_ : _ :), swapTwoStrings(_ : _ :) 및 swapTwoDoubles(_ : _ :) 함수의 본문이 동일하다는 것을 알 수 있습니다. 유일한 차이점은 허용되는 값의 타입(Int, String 및 Double)입니다.

It’s more useful, and considerably more flexible, to write a single function that swaps two values of any type. Generic code enables you to write such a function. (A generic version of these functions is defined below.)
> 모든 타입의 두 값을 바꾸는 단일 함수를 작성하는 것이 더 유용하고 훨씬 더 유연합니다. Generic을 사용하면 이러한 함수를 작성할 수 있습니다. (이러한 함수의 일반 버전은 아래에 정의되어 있습니다.)

## Generic Functions

Generic functions can work with any type. Here’s a generic version of the swapTwoInts(_:_:) function from above, called swapTwoValues(_:_:):
> Generic은 모든 타입에서 작동 할 수 있습니다. 위의 swapTwoInts (_ : _ :)함수의 일반적인 버전은 swapTwoValues(_ : _ :)입니다.

```swift
func swapTwoValues<T>(_ a: inout T, _ b: inout T) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

The body of the swapTwoValues(_:_:) function is identical to the body of the swapTwoInts(_:_:) function. However, the first line of swapTwoValues(_:_:) is slightly different from swapTwoInts(_:_:). Here’s how the first lines compare:
> swapTwoValues(_ : _ :) 함수의 본문은 swapTwoInts(_ : _ :) 함수의 본문과 동일합니다. 그러나 swapTwoValues(_ : _ :)의 첫 번째 줄은 swapTwoInts(_ : _ :)와 약간 다릅니다. 첫 번째 줄을 비교하는 방법은 다음과 같습니다.

```swift
func swapTwoInts(_ a: inout Int, _ b: inout Int)
func swapTwoValues<T>(_ a: inout T, _ b: inout T)
```

The generic version of the function uses a placeholder type name (called T, in this case) instead of an actual type name (such as Int, String, or Double). The placeholder type name doesn’t say anything about what T must be, but it does say that both a and b must be of the same type T, whatever T represents. The actual type to use in place of T is determined each time the swapTwoValues(_:_:) function is called.
> 함수의 generic 버전은 실제 형식 이름 (예 : Int, String 또는 Double) 대신 플레이스홀더 (이 경우 T라고 함)를 사용합니다. 플레이스홀더 이름은 T가 무엇이어야하는지에 대해 아무 말도하지 않지만, T가 무엇을 나타내는 지에 관계없이 a와 b 모두 동일한 타입 T 여야한다고 말합니다. T 대신 사용할 실제 타입은 swapTwoValues(_ : _ :) 함수가 호출 될 때마다 결정됩니다.

The other difference between a generic function and a nongeneric function is that the generic function’s name (swapTwoValues(_:_:)) is followed by the placeholder type name (T) inside angle brackets (<T>). The brackets tell Swift that T is a placeholder type name within the swapTwoValues(_:_:) function definition. Because T is a placeholder, Swift doesn’t look for an actual type called T.
> generic 함수와 nongeneric 함수의 다른 차이점은 generic 함수의 이름 (swapTwoValues(_ : _ :)) 뒤에 꺾쇠 괄호 (<T>) 안에 플레이스홀더 이름(T)이옵니다. 대괄호는 T가 swapTwoValues(_ : _ :) 함수 정의 내의 플레이스홀더 타입 이름임을 Swift에 알려줍니다. T는 플레이스홀더이므로 Swift는 T라는 실제 타입을 찾지 않습니다. 

The swapTwoValues(_:_:) function can now be called in the same way as swapTwoInts, except that it can be passed two values of any type, as long as both of those values are of the same type as each other. Each time swapTwoValues(_:_:) is called, the type to use for T is inferred from the types of values passed to the function.
> swapTwoValues(_ : _ :) 함수는 이제 swapTwoInts와 동일한 방식으로 호출 될 수 있습니다. 단, 두 값이 서로 동일한 타입인 한 모든 타입의 두 값을 전달할 수 있다는 점이 다릅니다. swapTwoValues(_ : _ :)가 호출 될 때마다 T에 사용할 타입은 함수에 전달 된 값 타입에서 유추됩니다.

In the two examples below, T is inferred to be Int and String respectively:
> 아래 두 예제에서 T는 각각 Int와 String으로 추론됩니다.

```swift
var someInt = 3
var anotherInt = 107
swapTwoValues(&someInt, &anotherInt)
// someInt is now 107, and anotherInt is now 3

var someString = "hello"
var anotherString = "world"
swapTwoValues(&someString, &anotherString)
// someString is now "world", and anotherString is now "hello"
```

## Type Parameters

In the swapTwoValues(_:_:) example above, the placeholder type T is an example of a type parameter. Type parameters specify and name a placeholder type, and are written immediately after the function’s name, between a pair of matching angle brackets (such as <T>).
> 위의 swapTwoValues(_ : _ :) 예에서 플레이스홀더 T는 타입 매개 변수의 예입니다. 타입 매개 변수는 플레이스홀더 타입을 지정하고 이름을 지정하며, 일치하는 꺾쇠 괄호 쌍 (예 : <T>) 사이에 함수 이름 바로 뒤에 기록됩니다.

Once you specify a type parameter, you can use it to define the type of a function’s parameters (such as the a and b parameters of the swapTwoValues(_:_:) function), or as the function’s return type, or as a type annotation within the body of the function. In each case, the type parameter is replaced with an actual type whenever the function is called. (In the swapTwoValues(_:_:) example above, T was replaced with Int the first time the function was called, and was replaced with String the second time it was called.)
> 타입 매개 변수를 지정하면 이를 사용하여 함수의 매개 변수 타입(예 : swapTwoValues(_ : _ :) 함수의 a 및 b 매개 변수)을 정의하거나 함수의 반환 타입으로 사용할 수 있습니다. 각각의 경우 함수가 호출 될 때마다 타입 매개 변수가 실제 타입으로 대체됩니다. (위의 swapTwoValues(_ : _ :) 예제에서 T는 함수가 처음 호출 될 때 Int로 바뀌었고 두 번째로 호출 될 때 String으로 바뀌 었습니다.)

You can provide more than one type parameter by writing multiple type parameter names within the angle brackets, separated by commas.
> 쉼표로 구분하여 꺾쇠 괄호 안에 여러 타입 매개 변수 이름을 작성하여 둘 이상의 타입 매개 변수를 제공 할 수 있습니다.

### Naming Type Parameters

In most cases, type parameters have descriptive names, such as Key and Value in Dictionary<Key, Value> and Element in Array<Element>, which tells the reader about the relationship between the type parameter and the generic type or function it’s used in. However, when there isn’t a meaningful relationship between them, it’s traditional to name them using single letters such as T, U, and V, such as T in the swapTwoValues(_:_:) function above.
> 대부분의 경우 타입 매개 변수에는 Key and Value in Dictionary<Key, Value> 및 Element in Array<Element>와 같은 설명이 포함 된 이름이 있습니다. 이는 타입 매개 변수와 사용되는 제네릭 타입 또는 함수 간의 관계를 독자에게 알려줍니다. 그러나 그들 사이에 의미있는 관계가 없을 때는 위의 swapTwoValues(_ : _ :) 함수의 T와 같이 T, U, V와 같은 단일 문자를 사용하여 이름을 지정하는 것이 일반적입니다.

## Generic Types

In addition to generic functions, Swift enables you to define your own generic types. These are custom classes, structures, and enumerations that can work with any type, in a similar way to Array and Dictionary.
> 제네릭 함수 외에도 Swift를 사용하면 고유 한 제네릭 타입을 정의 할 수 있습니다. 이들은 배열 및 딕셔너리와 유사한 방식으로 모든 타입에서 작동 할 수있는 사용자 정의 클래스, 구조체 및 열거형입니다.

This section shows you how to write a generic collection type called Stack. A stack is an ordered set of values, similar to an array, but with a more restricted set of operations than Swift’s Array type. An array allows new items to be inserted and removed at any location in the array. A stack, however, allows new items to be appended only to the end of the collection (known as pushing a new value on to the stack). Similarly, a stack allows items to be removed only from the end of the collection (known as popping a value off the stack).
> 이 섹션에서는 Stack이라는 일반 컬렉션 타입을 작성하는 방법을 보여줍니다. 스택은 배열과 유사하지만 Swift의 배열 유형보다 더 제한된 작업 집합을 가진 순서가 지정된 값 집합입니다. 배열을 사용하면 배열의 모든 위치에서 새 항목을 삽입하고 제거 할 수 있습니다. 그러나 스택을 사용하면 컬렉션 끝에만 추가 할 수 있습니다 (새 값을 스택에 푸시하는 것으로 알려짐). 마찬가지로 스택을 사용하면 컬렉션의 끝에서만 항목을 제거 할 수 있습니다 (스택에서 값을 팝 하는것으로 알려짐).

Here’s how to write a nongeneric version of a stack, in this case for a stack of Int values:
> 다음은 Int 값 스택에 대해 제네릭이 아닌 스택 버전을 작성하는 방법입니다.

```swift
struct IntStack {
    var items = [Int]()
    mutating func push(_ item: Int) {
        items.append(item)
    }
    mutating func pop() -> Int {
        return items.removeLast()
    }
}
```

Here’s a generic version of the same code:
> 다음은 동일한 코드의 generic 버전입니다.

```swift
struct Stack<Element> {
    var items = [Element]()
    mutating func push(_ item: Element) {
        items.append(item)
    }
    mutating func pop() -> Element {
        return items.removeLast()
    }
}
```

Note how the generic version of Stack is essentially the same as the nongeneric version, but with a type parameter called Element instead of an actual type of Int. This type parameter is written within a pair of angle brackets (<Element>) immediately after the structure’s name.
> Stack의 제네릭 버전은 기본적으로 비 제네릭 버전과 동일하지만 실제 Int 형식 대신 Element라는 타입 매개 변수를 사용합니다. 이 타입 매개 변수는 구조체 이름 바로 뒤의 꺾쇠 괄호 (<Element>) 안에 작성됩니다.


## Extending a Generic Type

When you extend a generic type, you don’t provide a type parameter list as part of the extension’s definition. Instead, the type parameter list from the original type definition is available within the body of the extension, and the original type parameter names are used to refer to the type parameters from the original definition.
> generic 타입을 확장 할 때 확장 정의의 일부로 타입 매개 변수 목록을 제공하지 않습니다. 대신 원래 타입 정의의 타입 매개 변수 목록은 확장의 본문 내에서 사용할 수 있으며 원래 타입 매개 변수 이름은 원래 정의의 타입 매개 변수를 참조하는 데 사용됩니다.

The following example extends the generic Stack type to add a read-only computed property called topItem, which returns the top item on the stack without popping it from the stack
> 다음 예제에서는 일반 스택 타입을 확장하여 topItem이라는 읽기 전용 계산 프로퍼티를 추가합니다.이 프로퍼티는 스택에서 항목을 팝하지 않고 스택의 최상위 항목을 반환합니다.

```swift
extension Stack {
    var topItem: Element? {
        return items.isEmpty ? nil : items[items.count - 1]
    }
}
```

Note that this extension doesn’t define a type parameter list. Instead, the Stack type’s existing type parameter name, Element, is used within the extension to indicate the optional type of the topItem computed property.
> 이 확장은 타입 매개 변수 목록을 정의하지 않습니다. 대신 스택 타입의 기존 타입 매개 변수 이름 인 Element가 확장 내에서 사용되어 topItem 계산 속성의 옵셔널 타입을 나타냅니다.

## Type Constraints

The swapTwoValues(_:_:) function and the Stack type can work with any type. However, it’s sometimes useful to enforce certain type constraints on the types that can be used with generic functions and generic types. Type constraints specify that a type parameter must inherit from a specific class, or conform to a particular protocol or protocol composition.
> swapTwoValues(_ : _ :) 함수와 스택 타입은 모든 타입에서 작동 할 수 있습니다. 그러나 제네릭 함수 및 제네릭 타입과 함께 사용할 수있는 타입에 특정 타입 제약 조건을 적용하는 것이 유용한 경우가 있습니다. 타입 제약 조건은 타입 매개 변수가 특정 클래스에서 상속하거나 특정 프로토콜 또는 프로토콜 구성을 준수해야 함을 지정합니다.

For example, Swift’s Dictionary type places a limitation on the types that can be used as keys for a dictionary. As described in Dictionaries, the type of a dictionary’s keys must be hashable. That is, it must provide a way to make itself uniquely representable. Dictionary needs its keys to be hashable so that it can check whether it already contains a value for a particular key. Without this requirement, Dictionary couldn’t tell whether it should insert or replace a value for a particular key, nor would it be able to find a value for a given key that’s already in the dictionary.
> 예를 들어 Swift의 Dictionary 타입은 딕셔너리의 키로 사용할 수있는 타입에 제한을 둡니다. 딕셔너리에 설명 된대로 딕셔너리의 키 유형은 해시가 가능해야합니다. 즉, 고유하게 표현할 수있는 방법을 제공해야합니다. 사전은 특정 키에 대한 값이 이미 포함되어 있는지 확인할 수 있도록 키를 해시 할 수 있어야합니다. 이 요구 사항이 없으면 사전은 특정 키에 대한 값을 삽입하거나 대체해야하는지 여부를 알 수 없으며 이미 사전에있는 지정된 키의 값을 찾을 수도 없습니다.

This requirement is enforced by a type constraint on the key type for Dictionary, which specifies that the key type must conform to the Hashable protocol, a special protocol defined in the Swift standard library. All of Swift’s basic types (such as String, Int, Double, and Bool) are hashable by default.
> 이 요구 사항은 Dictionary의 키 타입에 대한 타입 제약에 의해 시행되며, 이는 키 타입이 Swift 표준 라이브러리에 정의 된 특수 프로토콜 인 Hashable 프로토콜을 준수해야 함을 지정합니다. Swift의 모든 기본 타입 (예 : String, Int, Double 및 Bool)은 기본적으로 해시 할 수 있습니다. 

### Type Constraint Syntax

You write type constraints by placing a single class or protocol constraint after a type parameter’s name, separated by a colon, as part of the type parameter list. The basic syntax for type constraints on a generic function is shown below (although the syntax is the same for generic types):
> 타입 매개 변수 목록의 일부로 콜론으로 구분 된 타입 매개 변수 이름 뒤에 단일 클래스 또는 프로토콜 제약 조건을 배치하여 타입 제약 조건을 작성합니다. 제네릭 함수의 타입 제약 조건에 대한 기본 구문은 다음과 같습니다 (제네릭 유형의 구문은 동일 함).

```swift
func someFunction<T: SomeClass, U: SomeProtocol>(someT: T, someU: U) {
    // function body goes here
}
```

The hypothetical function above has two type parameters. The first type parameter, T, has a type constraint that requires T to be a subclass of SomeClass. The second type parameter, U, has a type constraint that requires U to conform to the protocol SomeProtocol.
> 위의 가상 함수에는 두 가지 타입 매개 변수가 있습니다. 첫 번째 타입 매개 변수 T에는 T가 SomeClass의 하위 클래스여야하는 타입 제약 조건이 있습니다. 두 번째 유형 매개 변수 U에는 U가 SomeProtocol 프로토콜을 준수해야하는 타입 제약 조건이 있습니다.

### Type Constraints in Action

```swift
func findIndex<T: Equatable>(of valueToFind: T, in array:[T]) -> Int? {
    for (index, value) in array.enumerated() {
        if value == valueToFind {
            return index
        }
    }
    return nil
}

let doubleIndex = findIndex(of: 9.3, in: [3.14159, 0.1, 0.25])
// doubleIndex is an optional Int with no value, because 9.3 isn't in the array
let stringIndex = findIndex(of: "Andrea", in: ["Mike", "Malcolm", "Andrea"])
// stringIndex is an optional Int containing a value of 2
```

## Associated Types

When defining a protocol, it’s sometimes useful to declare one or more associated types as part of the protocol’s definition. An associated type gives a placeholder name to a type that’s used as part of the protocol. The actual type to use for that associated type isn’t specified until the protocol is adopted. Associated types are specified with the associatedtype keyword.
> 프로토콜을 정의 할 때 프로토콜 정의의 일부로 하나 이상의 연관 타입을 선언하는 것이 유용한 경우가 있습니다. 연관 타입은 프로토콜의 일부로 사용되는 타입에 플레이스홀더를 제공합니다. 해당 연결 타입에 사용할 실제 타입은 프로토콜이 채택 될 때까지 지정되지 않습니다. 연관 타입은 associatedtype 키워드로 지정됩니다.

### Associated Types in Action

Here’s an example of a protocol called Container, which declares an associated type called Item:
> 다음은 Item이라는 연관 타입을 선언하는 Container라는 프로토콜의 예입니다.

```swift
protocol Container {
    associatedtype Item
    mutating func append(_ item: Item)
    var count: Int { get }
    subscript(i: Int) -> Item { get }
}
```

## Generic Where Clauses

Type constraints, as described in Type Constraints, enable you to define requirements on the type parameters associated with a generic function, subscript, or type. 
> 타입 제한에 설명 된대로 타입 제한을 사용하면 generic 함수, subscript 또는 타입과 연관된 타입 매개 변수에 대한 요구 사항을 정의 할 수 있습니다.

It can also be useful to define requirements for associated types. You do this by defining a generic where clause. A generic where clause enables you to require that an associated type must conform to a certain protocol, or that certain type parameters and associated types must be the same. A generic where clause starts with the where keyword, followed by constraints for associated types or equality relationships between types and associated types. You write a generic where clause right before the opening curly brace of a type or function’s body.
> 연관 타입에 대한 요구 사항을 정의하는 것도 유용 할 수 있습니다. generic where 절을 정의하여이를 수행합니다. 일반 where 절을 사용하면 연관 타입이 특정 프로토콜을 준수해야하거나 특정 타입 매개 변수 및 연관타입이 동일해야한다고 요구할 수 있습니다. 제네릭 where 절은 where 키워드로 시작하고 그 뒤에 연관된 타입에 대한 제약 조건 또는 타입과 연관 타입 간의 동등 관계가 이어집니다. 타입 또는 함수 본문의 여는 중괄호 바로 앞에 일반 where 절을 작성합니다.

The example below defines a generic function called allItemsMatch, which checks to see if two Container instances contain the same items in the same order. The function returns a Boolean value of true if all items match and a value of false if they don’t.
> 아래 예제는 두 개의 Container 인스턴스가 동일한 순서로 동일한 항목을 포함하는지 확인하는 allItemsMatch라는 일반 함수를 정의합니다. 이 함수는 모든 항목이 일치하면 부울 값 true를 반환하고 일치하지 않으면 false 값을 반환합니다.

The two containers to be checked don’t have to be the same type of container (although they can be), but they do have to hold the same type of items. This requirement is expressed through a combination of type constraints and a generic where clause:
> 검사 할 두 개의 컨테이너가 동일한 타입의 컨테이너일 필요는 없지만 (가능하더라도) 동일한 타입의 항목을 보관해야합니다. 이 요구 사항은 타입 제약 조건과 일반적인 where절의 조합을 통해 표현됩니다.

```swift
func allItemsMatch<C1: Container, C2: Container>
    (_ someContainer: C1, _ anotherContainer: C2) -> Bool
    where C1.Item == C2.Item, C1.Item: Equatable {

        // Check that both containers contain the same number of items.
        if someContainer.count != anotherContainer.count {
            return false
        }

        // Check each pair of items to see if they're equivalent.
        for i in 0..<someContainer.count {
            if someContainer[i] != anotherContainer[i] {
                return false
            }
        }

        // All items match, so return true.
        return true
}
```

This function takes two arguments called someContainer and anotherContainer. The someContainer argument is of type C1, and the anotherContainer argument is of type C2. Both C1 and C2 are type parameters for two container types to be determined when the function is called.
> 이 함수는 someContainer와 anotherContainer라는 두 개의 인수를받습니다. someContainer 인수는 C1 타입이고 anotherContainer 인수는 C2 타입입니다. C1과 C2는 함수가 호출 될 때 결정되는 두 가지 컨테이너 타입에 대한 타입 매개 변수입니다.

The following requirements are placed on the function’s two type parameters:
> 함수의 두 가지 타입 매개 변수에 대한 요구 사항은 다음과 같습니다.

* C1 must conform to the Container protocol (written as C1: Container).
* C2 must also conform to the Container protocol (written as C2: Container).
* The Item for C1 must be the same as the Item for C2 (written as C1.Item == C2.Item).
* The Item for C1 must conform to the Equatable protocol (written as C1.Item: Equatable).

The first and second requirements are defined in the function’s type parameter list, and the third and fourth requirements are defined in the function’s generic where clause.
> 첫 번째 및 두 번째 요구 사항은 함수의 타입 매개 변수 목록에 정의되고 세 번째 및 네 번째 요구 사항은 함수의 일반 where 절에 정의됩니다.

## Extensions with a Generic Where Clause

You can also use a generic where clause as part of an extension. The example below extends the generic Stack structure from the previous examples to add an isTop(_:) method.
> extension의 일부로 일반 where 절을 사용할 수도 있습니다. 아래 예제는 이전 예제의 일반 Stack 구조체를 확장하여 isTop(_ :) 메서드를 추가합니다.

```swift
extension Stack where Element: Equatable {
    func isTop(_ item: Element) -> Bool {
        guard let topItem = items.last else {
            return false
        }
        return topItem == item
    }
}
```

## Contextual Where Clauses

You can write a generic where clause as part of a declaration that doesn’t have its own generic type constraints, when you’re already working in the context of generic types. For example, you can write a generic where clause on a subscript of a generic type or on a method in an extension to a generic type. The Container structure is generic, and the where clauses in the example below specify what type constraints have to be satisfied to make these new methods available on a container.
> 제네릭 타입의 컨텍스트에서 이미 작업중인 경우 고유한 제네릭 타입 제약 조건이없는 선언의 일부로 제네릭 where 절을 작성할 수 있습니다. 예를 들어 제네릭 타입의 subscript 또는 제네릭 타입에 대한 확장의 메서드에 제네릭 where 절을 작성할 수 있습니다. 컨테이너 구조는 일반적이며 아래 예제의 where 절은 컨테이너에서 이러한 새 메서드를 사용할 수 있도록하기 위해 충족해야하는 타입 제약 조건을 지정합니다.

```swift
extension Container {
    func average() -> Double where Item == Int {
        var sum = 0.0
        for index in 0..<count {
            sum += Double(self[index])
        }
        return sum / Double(count)
    }
    func endsWith(_ item: Item) -> Bool where Item: Equatable {
        return count >= 1 && self[count-1] == item
    }
}
let numbers = [1260, 1200, 98, 37]
print(numbers.average())
// Prints "648.75"
print(numbers.endsWith(37))
// Prints "true"
```

This example adds an average() method to Container when the items are integers, and it adds an endsWith(_:) method when the items are equatable. Both functions include a generic where clause that adds type constraints to the generic Item type parameter from the original declaration of Container.
> 이 예제는 항목이 정수인 경우 Container에 average() 메서드를 추가하고 항목이 같을 때 endsWith(_ :) 메서드를 추가합니다. 두 함수 모두 컨테이너의 원래 선언에서 일반 항목 타입 매개 변수에 타입 제약 조건을 추가하는 일반 where 절을 포함합니다.

If you want to write this code without using contextual where clauses, you write two extensions, one for each generic where clause. The example above and the example below have the same behavior.
> 컨텍스트 where 절을 사용하지 않고 이 코드를 작성하려면 각 일반 where절에 대해 하나씩 두 개의 확장을 작성합니다. 위의 예와 아래의 예는 동일한 동작을합니다.

```swift
extension Container where Item == Int {
    func average() -> Double {
        var sum = 0.0
        for index in 0..<count {
            sum += Double(self[index])
        }
        return sum / Double(count)
    }
}
extension Container where Item: Equatable {
    func endsWith(_ item: Item) -> Bool {
        return count >= 1 && self[count-1] == item
    }
}
```

## Associated Types with a Generic Where Clause

You can include a generic where clause on an associated type. For example, suppose you want to make a version of Container that includes an iterator, like what the Sequence protocol uses in the standard library. Here’s how you write that:
> 연관타입에 일반 where 절을 포함 할 수 있습니다. 예를 들어, 표준 라이브러리에서 Sequence 프로토콜이 사용하는 것과 같이 iterator를 포함하는 Container 버전을 만들고 싶다고 가정 해 보겠습니다. 작성 방법은 다음과 같습니다.

```swift
protocol Container {
    associatedtype Item
    mutating func append(_ item: Item)
    var count: Int { get }
    subscript(i: Int) -> Item { get }

    associatedtype Iterator: IteratorProtocol where Iterator.Element == Item
    func makeIterator() -> Iterator
}
```

The generic where clause on Iterator requires that the iterator must traverse over elements of the same item type as the container’s items, regardless of the iterator’s type. The makeIterator() function provides access to a container’s iterator.
> Iterator의 일반 where 절에서는 iterator가 타입에 관계없이 컨테이너 항목과 동일한 항목 타입 요소를 통과해야합니다. makeIterator() 함수는 컨테이너의 iterator에 대한 액세스를 제공합니다.

For a protocol that inherits from another protocol, you add a constraint to an inherited associated type by including the generic where clause in the protocol declaration. For example, the following code declares a ComparableContainer protocol that requires Item to conform to Comparable:
> 다른 프로토콜에서 상속하는 프로토콜의 경우 프로토콜 선언에 일반 where 절을 포함하여 상속 된 관련 형식에 제약 조건을 추가합니다. 예를 들어 다음 코드는 항목이 Comparable을 준수해야하는 ComparableContainer 프로토콜을 선언합니다.

```swift
protocol ComparableContainer: Container where Item: Comparable { }
```

## Generic Subscripts

Subscripts can be generic, and they can include generic where clauses. You write the placeholder type name inside angle brackets after subscript, and you write a generic where clause right before the opening curly brace of the subscript’s body. For example:
> Subscript는 제네릭 일 수 있으며 제네릭 where 절을 포함 할 수 있습니다. Subscript 뒤에 꺾쇠 괄호 안에 플레이스홀더 이름을 쓰고 Subscript의 여는 중괄호 바로 앞에 일반적인 where 절을 작성합니다. 예를 들면 :

```swift
extension Container {
    subscript<Indices: Sequence>(indices: Indices) -> [Item]
        where Indices.Iterator.Element == Int {
            var result = [Item]()
            for index in indices {
                result.append(self[index])
            }
            return result
    }
}
```

This extension to the Container protocol adds a subscript that takes a sequence of indices and returns an array containing the items at each given index. This generic subscript is constrained as follows:
> 컨테이너 프로토콜에 대한 이 확장은 일련의 인덱스를 취하고 주어진 각 인덱스의 항목을 포함하는 배열을 반환하는 subscript를 추가합니다. 이 subscript는 다음과 같이 제한됩니다.

* The generic parameter Indices in angle brackets has to be a type that conforms to the Sequence protocol from the standard library.
*The subscript takes a single parameter, indices, which is an instance of that Indices type.
* The generic where clause requires that the iterator for the sequence must traverse over elements of type Int. This ensures that the indices in the sequence are the same type as the indices used for a container.

Taken together, these constraints mean that the value passed for the indices parameter is a sequence of integers.
> 종합하면 이러한 제약 조건은 indices 매개 변수에 전달 된 값이 정수 시퀀스임을 의미합니다.
