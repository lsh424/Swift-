# Enumerations

[Swift Docs](https://docs.swift.org/swift-book/LanguageGuide/Enumerations.html)  

An enumeration defines a common type for a group of related values and enables you to work with those values in a type-safe way within your code.
> 열거형은 관련 값 그룹에 대한 공통 유형을 정의하고 코드 내에서 유형이 안전한 방식으로 해당 값으로 작업 할 수 있도록합니다.


## Enumeration Syntax

```swift
enum SomeEnumeration {
// enumeration definition goes here
}
``` 

Here’s an example for the four main points of a compass:
> 다음은 나침반 예입니다.

```swift
enum CompassPoint {
case north
case south
case east
case west
}
``` 

Multiple cases can appear on a single line, separated by commas:
> 쉼표를 통해 여러개의 케이스를 한 줄에 나타낼 수 있습니다.

```swift
enum Planet {
case mercury, venus, earth, mars, jupiter, saturn, uranus, neptune
}
``` 

Each enumeration definition defines a new type. Like other types in Swift, their names (such as CompassPoint and Planet) start with a capital letter. Give enumeration types singular rather than plural names, so that they read as self-evident:
> 각 열거형 정의는 새 유형을 정의합니다. Swift의 다른 유형과 마찬가지로 이름 (예 : CompassPoint 및 Planet)은 대문자로 시작합니다. 열거형 유형에 복수 이름이 아닌 단수로 지정합니다.

```swift
var directionToHead = CompassPoint.west
directionToHead = .east 
``` 

The type of directionToHead is already known, and so you can drop the type when setting its value. This makes for highly readable code when working with explicitly typed enumeration values.
> directionToHead의 type은 이미 알려져 있으므로 값을 설정할 때 type을 삭제할 수 있습니다. 따라서 열거형으로 작업 할 때 코드를 쉽게 읽을 수 있습니다. (열거형 타입 추론이 가능한경우)


## Iterating over Enumeration Cases

For some enumerations, it’s useful to have a collection of all of that enumeration’s cases. You enable this by writing : CaseIterable after the enumeration’s name. Swift exposes a collection of all the cases as an allCases property of the enumeration type. Here’s an example:
> 일부 열거형의 경우 해당 열거형의 모든 case를 수집하는 것이 유용합니다. 열거형 이름 뒤에 CaseIterable을 작성하여 활성화합니다. Swift는 모든 케이스의 컬렉션을 열거 형의 allCases 속성으로 노출합니다. 예를 들면 다음과 같습니다.

```swift
enum Beverage: CaseIterable {
case coffee, tea, juice
}
let numberOfChoices = Beverage.allCases.count
print("\(numberOfChoices) beverages available")
// Prints "3 beverages available"
``` 

In the example above, you write Beverage.allCases to access a collection that contains all of the cases of the Beverage enumeration. You can use allCases like any other collection—the collection’s elements are instances of the enumeration type, so in this case they’re Beverage values. The example above counts how many cases there are, and the example below uses a for loop to iterate over all the cases.
> 위의 예에서 Beverage.allCases를 작성하여 Beverage 열거형의 모든 case가 포함 된 컬렉션에 액세스합니다. 다른 컬렉션처럼 allCases를 사용할 수 있습니다. 컬렉션의 elements는 열거형 인스턴스이므로 이 경우 Beverage values입니다. 위의 예제는 얼마나 많은 케이스가 있는지 계산하고 아래 예제는 for문을 사용해 모든 케이스를 반복합니다.

```swift
for beverage in Beverage.allCases {
print(beverage)
}
// coffee
// tea
// juice
``` 

The syntax used in the examples above marks the enumeration as conforming to the CaseIterable protocol.
> 위의 예에서 사용 된 구문은 열거형이 CaseIterable 프로토콜을 준수하는 것으로 표시합니다.


## Associated Values

The examples in the previous section show how the cases of an enumeration are a defined (and typed) value in their own right. You can set a constant or variable to Planet.earth, and check for this value later. However, it’s sometimes useful to be able to store values of other types alongside these case values. This additional information is called an associated value, and it varies each time you use that case as a value in your code.
> 이전 섹션의 예에서는 열거형 사례가 자체적으로 정의된 방식을 보여줍니다. 상수 또는 변수를 Planet.earth로 설정하고 나중에 이 값을 확인할 수 있습니다. 하지만 경우에 따라 이러한 케이스 값과 함께 다른 유형의 값을 저장할 수있는 것이 유용합니다. 이 추가 정보를 associated value라고하며 해당 case를 value로 사용할 때마다 달라집니다.

For example, suppose an inventory tracking system needs to track products by two different types of barcode. Some products are labeled with 1D barcodes in UPC format, which uses the numbers 0 to 9. Each barcode has a number system digit, followed by five manufacturer code digits and five product code digits. These are followed by a check digit to verify that the code has been scanned correctly:
> 예를 들어 재고 추적 시스템에서 두 가지 유형의 바코드로 제품을 추적해야한다고 가정해 봅시다. 일부 제품에는 숫자 0 ~ 9를 사용하는 UPC 형식의 1D 바코드 라벨이 부착되어 있습니다. 각 바코드에는 숫자 체계 숫자가 있고 그 뒤에 5 개의 제조업체 코드 숫자와 5 개의 제품 코드 숫자가 있습니다. 다음에는 코드가 올바르게 스캔되었는지 확인하기위한 검사 숫자가 있습니다.

사진
사진

```swift
enum Barcode {
case upc(Int, Int, Int, Int)
case qrCode(String)
}

var productBarcode = Barcode.upc(8, 85909, 51226, 3)
productBarcode = .qrCode("ABCDEFGHIJKLMNOP") // You can assign the same product a different type of barcode 
``` 

You can check the different barcode types using a switch statement, similar to the example in Matching Enumeration Values with a Switch Statement. This time, however, the associated values are extracted as part of the switch statement. You extract each associated value as a constant (with the let prefix) or a variable (with the var prefix) for use within the switch case’s body:
> switch 문을 사용하여 다른 바코드 유형을 확인할 수 있습니다. 그러나 이번에는 관련 값이 switch 문의 일부로 추출됩니다. 스위치 케이스의 본문 내에서 사용하기 위해 각 관련 값을 상수 (let 접두사 포함) 또는 변수 (var 접두사 포함)로 추출합니다.

```swift
switch productBarcode {
case .upc(let numberSystem, let manufacturer, let product, let check):
print("UPC: \(numberSystem), \(manufacturer), \(product), \(check).")
case .qrCode(let productCode):
print("QR code: \(productCode).")
}// Prints "QR code: ABCDEFGHIJKLMNOP." 
``` 

If all of the associated values for an enumeration case are extracted as constants, or if all are extracted as variables, you can place a single var or let annotation before the case name, for brevity:
> 열거형 케이스와 관련된 모든 값이 상수로 추출되거나 모두 변수로 추출되는 경우 간결성을 위해 케이스 이름 앞에 단일 var 또는 let을 배치 할 수 있습니다.

```swift
switch productBarcode {
case let .upc(numberSystem, manufacturer, product, check):
print("UPC : \(numberSystem), \(manufacturer), \(product), \(check).")
case let .qrCode(productCode):
print("QR code: \(productCode).")
}// Prints "QR code: ABCDEFGHIJKLMNOP."
``` 

## Raw Values

enumeration cases can come prepopulated with default values (called raw values), which are all of the same type.
> 열거 형 케이스는 모두 동일한 유형 인 기본값 (원시 값이라고 함)으로 미리 채워질 수 있습니다.

Here’s an example that stores raw ASCII values alongside named enumeration cases:
> 다음은 열거형으로 원시 ASCII 값을 저장하는 예입니다.

```swift
enum ASCIIControlCharacter: Character {
case tab = "\t"
case lineFeed = "\n"
case carriageReturn = "\r"
}
``` 

Raw values can be strings, characters, or any of the integer or floating-point number types. Each raw value must be unique within its enumeration declaration.
> 원시 값은 문자열, 문자 또는 정수 또는 부동 소수점 숫자 유형일 수 있습니다. 각 원시 값은 열거형 선언 내에서 고유해야합니다.


## Recursive Enumerations

A recursive enumeration is an enumeration that has another instance of the enumeration as the associated value for one or more of the enumeration cases. You indicate that an enumeration case is recursive by writing indirect before it, which tells the compiler to insert the necessary layer of indirection.
> case 뒤에 indirect를 명시함으로써 이 enum case가 재귀적이라는것을 나타낼 수 있습니다. 이는 컴파일러에게 간접 계층을 삽입하도록 요청합니다. 

For example, here is an enumeration that stores simple arithmetic expressions:
> 예를 들어 다음은 간단한 산술 표현식을 저장하는 열거입니다.

```swift
enum ArithmeticExpression {
case number(Int)
indirect case addition(ArithmeticExpression, ArithmeticExpression)
indirect case multiplication(ArithmeticExpression, ArithmeticExpression)
}
``` 

You can also write indirect before the beginning of the enumeration to enable indirection for all of the enumeration’s cases that have an associated value:
> 다음과 같이 enum 앞에 indirect를 명시함으로써 모든 열거형 case에 대해 indirect를 활성화 할 수 있습니다.

```swift
indirect enum ArithmeticExpression {
case number(Int)
case addition(ArithmeticExpression, ArithmeticExpression)
case multiplication(ArithmeticExpression, ArithmeticExpression)
}
``` 
This enumeration can store three kinds of arithmetic expressions: a plain number, the addition of two expressions, and the multiplication of two expressions. The addition and multiplication cases have associated values that are also arithmetic expressions—these associated values make it possible to nest expressions. For example, the expression (5 + 4) * 2 has a number on the right-hand side of the multiplication and another expression on the left-hand side of the multiplication. Because the data is nested, the enumeration used to store the data also needs to support nesting—this means the enumeration needs to be recursive. The code below shows the ArithmeticExpression recursive enumeration being created for (5 + 4) * 2:
> ~~ 데이터가 중첩되어 있으므로 데이터를 저장하는 데 사용되는 열거형도 중첩을 지원해야합니다. 즉, 열거형이 재귀적이어야 합니다. 아래 코드는 (5 + 4) * 2에 대해 생성되는 ArithmeticExpression 재귀 열거를 보여줍니다.

```swift
let five = ArithmeticExpression.number(5)
let four = ArithmeticExpression.number(4)
let sum = ArithmeticExpression.addition(five, four)
let product = ArithmeticExpression.multiplication(sum, ArithmeticExpression.number(2))

func evaluate(_ expression: ArithmeticExpression) -> Int {
switch expression {
case let .number(value):
return value
case let .addition(left, right):
return evaluate(left) + evaluate(right)
case let .multiplication(left, right):
return evaluate(left) * evaluate(right)
}
}

print(evaluate(product)) // Prints "18"
``` 
