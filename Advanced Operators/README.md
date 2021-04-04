# Advanced Operators

[Swift Docs](https://docs.swift.org/swift-book/LanguageGuide/AdvancedOperators.html)  

## Bitwise Operators

Bitwise operators enable you to manipulate the individual raw data bits within a data structure. They’re often used in low-level programming, such as graphics programming and device driver creation. Bitwise operators can also be useful when you work with raw data from external sources, such as encoding and decoding data for communication over a custom protocol.
> 비트 연산자를 사용하면 데이터 구조 내에서 개별 원시 데이터 비트를 조작 할 수 있습니다. 그래픽 프로그래밍 및 장치 드라이버 생성과 같은 저수준 프로그래밍에 자주 사용됩니다. 비트 연산자는 사용자 지정 프로토콜을 통한 통신을위한 데이터 인코딩 및 디코딩과 같은 외부 소스의 원시 데이터로 작업 할 때도 유용 할 수 있습니다.

Swift supports all of the bitwise operators found in C, as described below.
> Swift는 아래에 설명 된대로 C에서 발견되는 모든 비트 연산자를 지원합니다.

### Bitwise NOT Operator

The bitwise NOT operator (~) inverts all bits in a number:
> 비트 NOT 연산자 (~)는 숫자의 모든 비트를 반전합니다.

```swift
let initialBits: UInt8 = 0b00001111
let invertedBits = ~initialBits  // equals 11110000
``` 

### Bitwise AND Operator

The bitwise AND operator (&) combines the bits of two numbers. It returns a new number whose bits are set to 1 only if the bits were equal to 1 in both input numbers:
> 비트 AND 연산자 (&)는 두 숫자의 비트를 결합합니다. 두 입력 숫자에서 비트가 1 인 경우에만 비트가 1로 설정된 새 숫자를 반환합니다.

```swift
let firstSixBits: UInt8 = 0b11111100
let lastSixBits: UInt8  = 0b00111111
let middleFourBits = firstSixBits & lastSixBits  // equals 00111100
``` 

### Bitwise OR Operator

The bitwise OR operator (|) compares the bits of two numbers. The operator returns a new number whose bits are set to 1 if the bits are equal to 1 in either input number:
> 비트 OR 연산자 (|)는 두 숫자의 비트를 비교합니다. 연산자는 두 입력 숫자 중 하나에서 비트가 1이면 비트가 1로 설정된 새 숫자를 반환합니다.

```swift
let someBits: UInt8 = 0b10110010
let moreBits: UInt8 = 0b01011110
let combinedbits = someBits | moreBits  // equals 11111110
``` 

### Bitwise XOR Operator

The bitwise XOR operator, or “exclusive OR operator” (^), compares the bits of two numbers. The operator returns a new number whose bits are set to 1 where the input bits are different and are set to 0 where the input bits are the same:
> 비트 XOR 연산자 또는 "배타적 OR 연산자"(^)는 두 숫자의 비트를 비교합니다. 연산자는 입력 비트가 다른 경우 비트가 1로 설정되고 입력 비트가 동일한 경우 0으로 설정된 새 숫자를 반환합니다.

```swift
let firstBits: UInt8 = 0b00010100
let otherBits: UInt8 = 0b00000101
let outputBits = firstBits ^ otherBits  // equals 00010001
``` 

### Bitwise Left and Right Shift Operators

The bitwise left shift operator (<<) and bitwise right shift operator (>>) move all bits in a number to the left or the right by a certain number of places, according to the rules defined below.
> 비트 왼쪽 시프트 연산자 (<<) 및 비트 오른쪽 시프트 연산자 (>>)는 아래 정의 된 규칙에 따라 숫자의 모든 비트를 특정 자리 수만큼 왼쪽 또는 오른쪽으로 이동합니다.

Bitwise left and right shifts have the effect of multiplying or dividing an integer by a factor of two. Shifting an integer’s bits to the left by one position doubles its value, whereas shifting it to the right by one position halves its value.
> 비트 왼쪽 및 오른쪽 시프트는 정수를 2 배로 곱하거나 나누는 효과가 있습니다. 정수의 비트를 왼쪽으로 한 위치 이동하면 값이 두 배가되고 오른쪽으로 한 위치 이동하면 값이 반으로 줄어 듭니다.

```swift
let shiftBits: UInt8 = 4   // 00000100 in binary
shiftBits << 1             // 00001000
shiftBits << 2             // 00010000
shiftBits << 5             // 10000000
shiftBits << 6             // 00000000
shiftBits >> 2             // 00000001
``` 

## Operator Methods

Classes and structures can provide their own implementations of existing operators. This is known as overloading the existing operators.
> 클래스와 구조체는 기존 연산자의 자체 구현을 제공 할 수 있습니다. 이를 기존 연산자 오버로드라고합니다.

The example below shows how to implement the arithmetic addition operator (+) for a custom structure. The arithmetic addition operator is a binary operator because it operates on two targets and is said to be infix because it appears in between those two targets.
> 아래 예제는 사용자 정의 구조체에 대해 산술 더하기 연산자 (+)를 구현하는 방법을 보여줍니다. 산술 더하기 연산자는 두 대상에서 작동하기 때문에 이항 연산자이며 두 대상 사이에 나타나기 때문에 중위라고합니다.

The example defines a Vector2D structure for a two-dimensional position vector (x, y), followed by a definition of an operator method to add together instances of the Vector2D structure:
> 이 예제에서는 2 차원 위치 벡터 (x, y)에 대한 Vector2D 구조체를 정의한 다음 Vector2D 구조체의 인스턴스를 함께 추가하는 연산자 메서드 정의가 이어집니다.

```swift
struct Vector2D {
    var x = 0.0, y = 0.0
}

extension Vector2D {
    static func + (left: Vector2D, right: Vector2D) -> Vector2D {
        return Vector2D(x: left.x + right.x, y: left.y + right.y)
    }
}
``` 

The operator method is defined as a type method on Vector2D, with a method name that matches the operator to be overloaded (+). Because addition isn’t part of the essential behavior for a vector, the type method is defined in an extension of Vector2D rather than in the main structure declaration of Vector2D. Because the arithmetic addition operator is a binary operator, this operator method takes two input parameters of type Vector2D and returns a single output value, also of type Vector2D.
> 연산자 메서드는 Vector2D에서 오버로드 할 연산자 (+)와 일치하는 메서드 이름을 가진 형식 메서드로 정의됩니다. 더하기는 벡터의 필수 동작의 일부가 아니기 때문에 타입 메서드는 Vector2D의 기본 구조 선언이 아닌 Vector2D의 확장에서 정의됩니다. 산술 더하기 연산자는 이항 연산자이므로이 연산자 메서드는 Vector2D 유형의 두 입력 매개 변수를 취하고 Vector2D 유형의 단일 출력 값을 반환합니다.

In this implementation, the input parameters are named left and right to represent the Vector2D instances that will be on the left side and right side of the + operator. The method returns a new Vector2D instance, whose x and y properties are initialized with the sum of the x and y properties from the two Vector2D instances that are added together.
> 이 구현에서 입력 매개 변수의 이름은 + 연산자의 왼쪽과 오른쪽에있는 Vector2D 인스턴스를 나타 내기 위해 left와 right으로 지정됩니다. 이 메서드는 함께 추가 된 두 Vector2D 인스턴스의 x 및 y 프로퍼티의 합으로 x 및 y 속성이 초기화되는 새 Vector2D 인스턴스를 반환합니다.

The type method can be used as an infix operator between existing Vector2D instances:
> 타입 메서드는 기존 Vector2D 인스턴스 사이에서 중위 연산자로 사용할 수 있습니다.

```swift
let vector = Vector2D(x: 3.0, y: 1.0)
let anotherVector = Vector2D(x: 2.0, y: 4.0)
let combinedVector = vector + anotherVector
// combinedVector is a Vector2D instance with values of (5.0, 5.0)
``` 

### Prefix and Postfix Operators

The example shown above demonstrates a custom implementation of a binary infix operator. Classes and structures can also provide implementations of the standard unary operators. Unary operators operate on a single target. They’re prefix if they precede their target (such as -a) and postfix operators if they follow their target (such as b!).
> 위에 표시된 예제는 이진 중위 연산자의 사용자 정의 구현을 보여줍니다. 클래스와 구조체는 표준 단항 연산자의 구현을 제공 할 수도 있습니다. 단항 연산자는 단일 대상에서 작동합니다. 대상 앞에 오는 경우 접두사 (예 : -a)이고 대상을 따르는 경우 접미사 연산자 (예 : b!)입니다.

You implement a prefix or postfix unary operator by writing the prefix or postfix modifier before the func keyword when declaring the operator method:
> 연산자 메서드를 선언 할 때 func 키워드 앞에 접두사 또는 접미사 수정자를 작성하여 접두사 또는 접미사 단항 연산자를 구현합니다.

```swift
extension Vector2D {
    static prefix func - (vector: Vector2D) -> Vector2D {
        return Vector2D(x: -vector.x, y: -vector.y)
    }
}
``` 

```swift
let positive = Vector2D(x: 3.0, y: 4.0)
let negative = -positive
// negative is a Vector2D instance with values of (-3.0, -4.0)
let alsoPositive = -negative
// alsoPositive is a Vector2D instance with values of (3.0, 4.0)
``` 

### Compound Assignment Operators

Compound assignment operators combine assignment (=) with another operation. For example, the addition assignment operator (+=) combines addition and assignment into a single operation. You mark a compound assignment operator’s left input parameter type as inout, because the parameter’s value will be modified directly from within the operator method.
> 복합 할당 연산자는 할당 (=)을 다른 연산과 결합합니다. 예를 들어 더하기 할당 연산자 (+ =)는 더하기와 할당을 단일 연산으로 결합합니다. 매개 변수의 값이 연산자 메소드 내에서 직접 수정되므로 복합 할당 연산자의 왼쪽 입력 매개 변수 유형을 inout으로 표시합니다.

```swift
extension Vector2D {
    static func += (left: inout Vector2D, right: Vector2D) {
        left = left + right
    }
}
``` 

Because an addition operator was defined earlier, you don’t need to reimplement the addition process here. Instead, the addition assignment operator method takes advantage of the existing addition operator method, and uses it to set the left value to be the left value plus the right value:
> 더하기 연산자가 이전에 정의되었으므로 여기에서 더하기 프로세스를 다시 구현할 필요가 없습니다. 대신 더하기 할당 연산자 방법은 기존 더하기 연산자 방법을 활용하고 왼쪽 값을 왼쪽 값에 오른쪽 값을 더한 값으로 설정하는 데 사용합니다.

```swift
var original = Vector2D(x: 1.0, y: 2.0)
let vectorToAdd = Vector2D(x: 3.0, y: 4.0)
original += vectorToAdd
// original now has values of (4.0, 6.0)
``` 

### Equivalence Operators

By default, custom classes and structures don’t have an implementation of the equivalence operators, known as the equal to operator (==) and not equal to operator (!=). You usually implement the == operator, and use the standard library’s default implementation of the != operator that negates the result of the == operator. There are two ways to implement the == operator: You can implement it yourself, or for many types, you can ask Swift to synthesize an implementation for you. In both cases, you add conformance to the standard library’s Equatable protocol.
> 기본적으로 사용자 정의 클래스 및 구조에는 같음 연산자 (==) 및 같지 않음 연산자 (! =)라고하는 등가 연산자가 구현되어 있지 않습니다. 일반적으로 == 연산자를 구현하고 == 연산자의 결과를 부정하는! = 연산자의 표준 라이브러리 기본 구현을 사용합니다. == 연산자를 구현하는 방법에는 두 가지가 있습니다. 직접 구현하거나 여러 유형의 경우 Swift에 구현을 합성하도록 요청할 수 있습니다. 두 경우 모두 표준 라이브러리의 Equatable 프로토콜에 대한 적합성을 추가합니다.

```swift
extension Vector2D: Equatable {
    static func == (left: Vector2D, right: Vector2D) -> Bool {
        return (left.x == right.x) && (left.y == right.y)
    }
}
``` 

```swift
let twoThree = Vector2D(x: 2.0, y: 3.0)
let anotherTwoThree = Vector2D(x: 2.0, y: 3.0)
if twoThree == anotherTwoThree {
    print("These two vectors are equivalent.")
}
// Prints "These two vectors are equivalent."
``` 

