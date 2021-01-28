# The Basics

[Swift Docs](https://docs.swift.org/swift-book/LanguageGuide/TheBasics.html#)

Swift 언어적 특성: 안전하고 빠르고 인터렉티브 하다.

* Variables are always initialized before use. 변수는 사용되기 전에 항상 초기화 되어야 한다. 
* Array indices are checked for out-of-bounds errors. 
* Integers are checked for overflow. 
* Optionals ensure that nil values are handled explicitly.
* Memory is managed automatically. (메모리는 알아서 관리된다. -> ARC)
* Error handling allows controlled recovery from unexpected failures.

objective-c에는 없고 Swift에 도입된것들: tuple, optional 등..

Swift also introduces optional types, which handle the absence of a value.

...

## Swift type-safe한 언어다. 

they’re at the heart of many of Swift’s most powerful features.
> 옵셔널 타입은 Swift의 powerful한 기능들의 핵심이다. 

Swift is a type-safe language which means the language helps you to be clear about the types of values your code can work with. 
If part of your code requires a String, type safety prevents you from passing it an Int by mistake.  
> Swift는 type-safe한 언어로 코드가 작업 할 수있는 값 유형을 명확하게하는 데 도움이됩니다. 코드의 일부에 String이 필요한 경우 type safety가 실수로 Int를 전달하지 못하도록합니다.


Type safety helps you catch and fix errors as early as possible in the development process.  
> 안전성은 개발 프로세스에서 가능한 한 빨리 오류를 포착하고 수정하는 데 도움이됩니다.  


## Type Aliases

Type aliases define an alternative name for an existing type. You define type aliases with the typealias keyword.  
> 유형 별칭은 기존 유형의 대체 이름을 정의합니다. typealias 키워드로 유형 별칭을 정의합니다.

Type aliases are useful when you want to refer to an existing type by a name that is contextually more appropriate, such as when working with data of a specific size from an external source. 
> 유형 별칭은 외부 소스에서 특정 크기의 데이터로 작업 할 때와 같이 상황에 맞는 더 적절한 이름으로 기존 유형을 참조하려는 경우에 유용합니다.

```swift
typealias AudioSample = UInt16
var maxAmplitudeFound = AudioSample.min // UInt16.min와 같음
```  

## Tuples

Tuples group multiple values into a single compound value. The values within a tuple can be of any type and don’t have to be of the same type as each other.  
> 서로 다른 타입끼리 묶을 수 있음  

```swift
let http404Error = (404, "Not Found")

let (statusCode, statusMessage) = http404Error
print("The status code is \(statusCode)") // Prints "The status code is 404"
print("The status message is \(statusMessage)") // Prints "The status message is Not Found"
```  

```swift
print("The status code is \(http404Error.0)") // Prints "The status code is 404"
print("The status message is \(http404Error.1)") // Prints "The status message is Not Found"
```  

```swift
let http200Status = (statusCode: 200, description: "OK")

print("The status code is \(http200Status.statusCode)") // Prints "The status code is 200"
print("The status message is \(http200Status.description)") // Prints "The status message is OK"
```  

Tuples are particularly useful as the return values of functions.  
> 튜플은 함수의 반환 값으로 특히 유용합니다.

notice: Tuples are useful for simple groups of related values. They’re not suited to the creation of complex data structures. If your data structure is likely to be more complex, model it as a class or structure, rather than as a tuple.  
> 주의 : 튜플은 관련 값의 단순한 그룹에 유용합니다. 복잡한 데이터 구조 생성에는 적합하지 않습니다. 데이터 구조가 더 복잡 할 가능성이있는 경우 튜플이 아닌 class 혹은 structure로 모델링하쎄용.


## Optionals

You use optionals in situations where a value may be absent. An optional represents two possibilities: Either there is a value, and you can unwrap the optional to access that value, or there isn’t a value at all.  
> 값이 없을 수있는 상황에서 optional을 사용합니다. optional은 두 가지 가능성을 나타냅니다. 값이 있고 해당 값에 액세스하기 위해 optional을 unwrap 할 수 있거나 값이 전혀 없을 수 있습니다.

#### Optional Binding

You use optional binding to find out whether an optional contains a value, and if so, to make that value available as a temporary constant or variable.
> optional이 값을 갖고 있는지 유무를 확인하고 만약 값이 있다면, 임시 상수 혹은 변수로 사용 가능. 

## Error Handling

You use error handling to respond to error conditions your program may encounter during execution.  
> 오류 처리를 사용해 실행 중에 프로그램에서 발생할 수있는 오류 조건에 대응합니다.

When a function encounters an error condition, it throws an error. That function’s caller can then catch the error and respond appropriately.  
> 함수에 오류 조건이 발생하면 오류가 발생합니다. 그러면 해당 함수의 호출자가 오류를 포착하고 적절하게 응답 할 수 있습니다.  

```swift
func canThrowAnError() throws {
// this function may or may not throw an error
}
```  
A function indicates that it can throw an error by including the throws keyword in its declaration. When you call a function that can throw an error, you prepend the try keyword to the expression.
Swift automatically propagates errors out of their current scope until they’re handled by a catch clause.  
> 함수는 선언에 throws 키워드를 포함하여 오류가 발생할 수 있음을 나타냅니다. 오류가 발생할 수있는 함수를 호출 할 때 표현식 앞에 try 키워드를 추가합니다.
Swift는 catch 절에서 처리 할 때까지 현재 범위에서 오류를 자동으로 전파합니다.

```swift
do {
try canThrowAnError()
// no error was thrown
} catch {
// an error was thrown
}
```  


```swift
func makeASandwich() throws {
// ...
}

do {
try makeASandwich()
eatASandwich()
} catch SandwichError.outOfCleanDishes {
washDishes()
} catch SandwichError.missingIngredients(let ingredients) {
buyGroceries(ingredients)
}
```  
