# Functions

[Swift Docs](https://docs.swift.org/swift-book/LanguageGuide/Functions.html)


## Optional Tuple Return Types

To handle an empty array safely, write the minMax(array:) function with an optional tuple return type and return a value of nil when the array is empty:  
> 빈 배열을 안전하게 처리하려면 optional tuple 반환 type으로 minMax (array :) 함수를 작성하고 배열이 비어있을 때 nil 값을 반환합니다.  

```swift
func minMax(array: [Int]) -> (min: Int, max: Int)? {
if array.isEmpty { return nil }
var currentMin = array[0]
var currentMax = array[0]
for value in array[1..<array.count] {
if value < currentMin {
currentMin = value
} else if value > currentMax {
currentMax = value
}
}
return (currentMin, currentMax)
}
```  

You can use optional binding to check whether this version of the minMax(array:) function returns an actual tuple value or nil:  
> optional binding을 사용하여이 버전의 minMax (array :) 함수가 실제 튜플 값 또는 nil을 반환하는지 여부를 확인할 수 있습니다.

```swift
if let bounds = minMax(array: [8, -6, 2, 109, 3, 71]) {
print("min is \(bounds.min) and max is \(bounds.max)")
} // Prints "min is -6 and max is 109"
``` 

## Default Parameter Values

You can define a default value for any parameter in a function by assigning a value to the parameter after that parameter’s type. If a default value is defined, you can omit that parameter when calling the function.  
> 매개 변수 type 뒤에 값을 할당하여 함수의 모든 매개 변수에 대한 기본값을 정의 할 수 있습니다. 기본값이 정의 된 경우 함수를 호출 할 때 해당 매개 변수를 생략 할 수 있습니다.

```swift
func someFunction(parameterWithoutDefault: Int, parameterWithDefault: Int = 12) {
// If you omit the second argument when calling this function, then
// the value of parameterWithDefault is 12 inside the function body.
}
someFunction(parameterWithoutDefault: 3, parameterWithDefault: 6) // parameterWithDefault is 6
someFunction(parameterWithoutDefault: 4) // parameterWithDefault is 12
``` 

## In-Out Parameters

Function parameters are constants by default. Trying to change the value of a function parameter from within the body of that function results in a compile-time error. This means that you can’t change the value of a parameter by mistake. If you want a function to modify a parameter’s value, and you want those changes to persist after the function call has ended, define that parameter as an in-out parameter instead.  
> 함수 매개 변수는 기본적으로 상수입니다. 해당 함수의 본문 내에서 함수 매개 변수의 값을 변경하려고하면 컴파일 타임 오류가 발생합니다. 이는 실수로 매개 변수 값을 변경할 수 없음을 의미합니다. 함수가 매개 변수 값을 수정하도록하고 함수 호출이 종료 된 후에도 이러한 변경 사항이 유지되도록하려면 해당 매개 변수를 대신 in-out 매개 변수로 정의하세요.

You write an in-out parameter by placing the inout keyword right before a parameter’s type. An in-out parameter has a value that is passed in to the function, is modified by the function, and is passed back out of the function to replace the original value.  
> 매개 변수 type 바로 앞에 inout 키워드를 배치하여 in-out 매개 변수를 작성합니다. in-out 매개 변수에는 함수에 전달되고 함수에 의해 수정되고 원래 값을 대체하기 위해 함수에서 다시 전달되는 값이 있습니다.

```swift
func swapTwoInts(_ a: inout Int, _ b: inout Int) {
let temporaryA = a
a = b
b = temporaryA
}

var someInt = 3
var anotherInt = 107
swapTwoInts(&someInt, &anotherInt)
print("someInt is now \(someInt), and anotherInt is now \(anotherInt)")
// Prints "someInt is now 107, and anotherInt is now 3"
``` 
