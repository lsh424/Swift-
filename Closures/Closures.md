# Closures

[Swift Docs](https://docs.swift.org/swift-book/LanguageGuide/Closures.html)

Closures can capture and store references to any constants and variables from the context in which they are defined. This is known as closing over those constants and variables. Swift handles all of the memory management of capturing for you.  
> 클로저는 정의 된 컨텍스트에서 모든 상수 및 변수에 대한 참조를 캡처하고 저장할 수 있습니다. 이를 이러한 상수 및 변수에 대한 closing이라고합니다. Swift는 캡처의 모든 메모리 관리를 처리합니다.

Here’s how you can use the map(_:) method with a trailing closure to convert an array of Int values into an array of String values. The array [16, 58, 510] is used to create the new array ["OneSix", "FiveEight", "FiveOneZero"]:
> 다음은 후행 클로저와 함께 map (_ :) 메서드를 사용하여 Int 값 배열을 문자열 값 배열로 변환하는 방법입니다. 배열 [16, 58, 510]은 새 배열 [ "OneSix", "FiveEight", "FiveOneZero"]를 만드는 데 사용됩니다.  

```swift
let digitNames = [
0: "Zero", 1: "One", 2: "Two",   3: "Three", 4: "Four",
5: "Five", 6: "Six", 7: "Seven", 8: "Eight", 9: "Nine"
]
let numbers = [16, 58, 510]
``` 

The code above creates a dictionary of mappings between the integer digits and English-language versions of their names. It also defines an array of integers, ready to be converted into strings. You can now use the numbers array to create an array of String values, by passing a closure expression to the array’s map(_:) method as a trailing closure:
> 위의 코드는 정수 숫자와 영어 버전 이름 간의 매핑 dictionary를 만듭니다. 또한 문자열로 변환 할 준비가 된 정수 배열을 정의합니다. 이제 배열의 map (_ :) 메서드에 클로저 표현식을 후행 클로저로 전달하여 numbers 배열을 사용하여 String 값의 배열을 만들 수 있습니다.  

```swift
let strings = numbers.map { (number) -> String in
var number = number
var output = ""
repeat {
output = digitNames[number % 10]! + output
number /= 10
} while number > 0
return output
}
// strings is inferred to be of type [String]
// its value is ["OneSix", "FiveEight", "FiveOneZero"]
``` 

The map(_:) method calls the closure expression once for each item in the array. You don’t need to specify the type of the closure’s input parameter, number, because the type can be inferred from the values in the array to be mapped.
> map (_ :) 메서드는 배열의 각 항목에 대해 한 번씩 클로저 표현식을 호출합니다. 매핑 할 배열의 값에서 유형을 유추 할 수 있으므로 클로저의 입력 매개 변수 type 을 명시 할 필요가 없습니다.  

In this example, the variable number is initialized with the value of the closure’s number parameter, so that the value can be modified within the closure body. (The parameters to functions and closures are always constants.) The closure expression also specifies a return type of String, to indicate the type that will be stored in the mapped output array.  
> 이 예에서 변수 number는 클로저의 number 매개 변수 값으로 초기화되어 값이 클로저 본문 내에서 수정 될 수 있습니다. (함수 및 클로저에 대한 매개 변수는 항상 상수입니다.) 클로저 표현식은 매핑 된 출력 배열에 저장 될 유형을 나타 내기 위해 반환 type을 String으로 지정합니다.  

The closure expression builds a string called output each time it is called. It calculates the last digit of number by using the remainder operator (number % 10), and uses this digit to look up an appropriate string in the digitNames dictionary. The closure can be used to create a string representation of any integer greater than zero.  
> 클로저 표현식은 호출 될 때마다 output이라는 문자열을 만듭니다. 나머지 연산자 (number % 10)를 사용하여 숫자의 마지막 숫자를 계산하고이 숫자를 사용하여 digitNames 사전에서 적절한 문자열을 찾습니다. 클로저는 0보다 큰 정수의 문자열 표현을 만드는 데 사용할 수 있습니다.  


## Capturing Values

A closure can capture constants and variables from the surrounding context in which it is defined. The closure can then refer to and modify the values of those constants and variables from within its body, even if the original scope that defined the constants and variables no longer exists.
> 클로저는 정의 된 컨텍스트에서 상수와 변수를 캡처 할 수 있습니다. 그러면 클로저는 상수와 변수를 정의한 원래 범위가 더 이상 존재하지 않더라도 본문 내에서 해당 상수와 변수의 값을 참조하고 수정할 수 있습니다.  

In Swift, the simplest form of a closure that can capture values is a nested function, written within the body of another function. A nested function can capture any of its outer function’s arguments and can also capture any constants and variables defined within the outer function.
> Swift에서 값을 캡처 할 수있는 가장 간단한 형태의 클로저는 다른 함수의 본문 내에 작성된 중첩 함수입니다. 중첩 함수는 외부 함수의 인수를 캡처 할 수 있으며 외부 함수 내에 정의 된 모든 상수 및 변수를 캡처 할 수도 있습니다.  

Here’s an example of a function called makeIncrementer, which contains a nested function called incrementer. The nested incrementer() function captures two values, runningTotal and amount, from its surrounding context. After capturing these values, incrementer is returned by makeIncrementer as a closure that increments runningTotal by amount each time it is called.
> 다음은 incrementer라는 중첩 함수를 포함하는 makeIncrementer라는 함수의 예입니다. 중첩 된 incrementer() 함수는 주변 컨텍스트에서 runningTotal 및 amount의 두 값을 캡처합니다. 이러한 값을 캡처 한 후에는 호출 될 때마다 runningTotal을 양만큼 증가시키는 클로저로 incrementer를 makeIncrementer에서 반환합니다.  

```swift
func makeIncrementer(forIncrement amount: Int) -> () -> Int {
var runningTotal = 0

func incrementer() -> Int {
runningTotal += amount
return runningTotal
}
return incrementer
}
``` 

The incrementer() function doesn’t have any parameters, and yet it refers to runningTotal and amount from within its function body. It does this by capturing a reference to runningTotal and amount from the surrounding function and using them within its own function body. Capturing by reference ensures that runningTotal and amount don’t disappear when the call to makeIncrementer ends, and also ensures that runningTotal is available the next time the incrementer function is called.
> incrementer() 함수에는 매개 변수가 없지만 함수 본문 내에서 runningTotal 및 양을 참조합니다. 주변 함수에서 runningTotal 및 amount에 대한 참조를 캡처하고 자체 함수 본문 내에서 사용하여이를 수행합니다. 참조로 캡처하면 makeIncrementer 호출이 종료 될 때 runningTotal 및 amount가 사라지지 않고 다음에 incrementer 함수가 호출 될 때 runningTotal을 사용할 수 있습니다.  

```swift
let incrementByTen = makeIncrementer(forIncrement: 10)
```

This example sets a constant called incrementByTen to refer to an incrementer function that adds 10 to its runningTotal variable each time it is called. Calling the function multiple times shows this behavior in action:
> 이 예제에서는 호출 될 때마다 runningTotal 변수에 10을 더하는 incrementer 함수를 참조하도록 incrementByTen이라는 상수를 설정합니다. 함수를 여러 번 호출하면이 동작이 실제로 표시됩니다.  

```swift
incrementByTen()
// returns a value of 10
incrementByTen()
// returns a value of 20
incrementByTen()
// returns a value of 30
```

If you create a second incrementer, it will have its own stored reference to a new, separate runningTotal variable:
> 두 번째 incrementer를 만들면 별도의 새 runningTotal 변수에 대한 stored referenc를 갖게 됩니다.  

```swift
let incrementBySeven = makeIncrementer(forIncrement: 7)
incrementBySeven() // returns a value of 7
```

Calling the original incrementer (incrementByTen) again continues to increment its own runningTotal variable, and does not affect the variable captured by incrementBySeven:
> original incrementer (incrementByTen)를 다시 호출하면 자체 runningTotal 변수가 계속 증가하고 incrementBySeven에 캡처 된 변수에는 영향을주지 않습니다.  

```swift
incrementByTen()
// returns a value of 40
```

#### *Note* 

If you assign a closure to a property of a class instance, and the closure captures that instance by referring to the instance or its members, you will create a strong reference cycle between the closure and the instance. Swift uses capture lists to break these strong reference cycles. For more information, see Strong Reference Cycles for Closures.
> 클래스 인스턴스의 속성에 클로저를 할당하고 클로저가 인스턴스 또는 해당 멤버를 참조하여 해당 인스턴스를 캡처하면 클로저와 인스턴스 사이에 강력한 참조주기가 생성됩니다. Swift는 캡처 목록을 사용하여 이러한 강력한 참조주기를 깨뜨립니다. [별도 정리](https://lsh424.tistory.com/77). 

## Closures Are Reference Types

In the example above, incrementBySeven and incrementByTen are constants, but the closures these constants refer to are still able to increment the runningTotal variables that they have captured. This is because functions and closures are reference types. 
> 위의 예에서 incrementBySeven 및 incrementByTen은 상수이지만 이러한 상수가 참조하는 클로저는 캡처 한 runningTotal 변수를 계속 증가시킬 수 있습니다. 이는 함수와 클로저가 참조 유형이기 때문입니다.

Whenever you assign a function or a closure to a constant or a variable, you are actually setting that constant or variable to be a reference to the function or closure.
> 함수 또는 클로저를 상수 또는 변수에 할당 할 때마다 실제로 해당 상수 또는 변수를 함수 또는 클로저에 대한 참조로 설정합니다.   

This also means that if you assign a closure to two different constants or variables, both of those constants or variables refer to the same closure. 
> 이것은 또한 두 개의 다른 상수 또는 변수에 클로저를 할당하면 해당 상수 또는 변수가 모두 동일한 클로저를 참조 함을 의미합니다.  

```swift
let alsoIncrementByTen = incrementByTen
alsoIncrementByTen()
// returns a value of 50

incrementByTen()
// returns a value of 60
```

## Escaping Closures

A closure is said to escape a function when the closure is passed as an argument to the function, but is called after the function returns. When you declare a function that takes a closure as one of its parameters, you can write @escaping before the parameter’s type to indicate that the closure is allowed to escape.
> 클로저는 클로저가 함수에 인수로 전달 될 때 함수를 escape(탈출)한다고 말하지만 함수가 반환 된 후에 호출됩니다. 클로저를 매개 변수 중 하나로 사용하는 함수를 선언 할 때 매개 변수 type 앞에 @escaping을 작성하여 클로저가 escape(탈출)될 수 있음을 나타낼 수 있습니다.  

One way that a closure can escape is by being stored in a variable that’s defined outside the function. As an example, many functions that start an asynchronous operation take a closure argument as a completion handler. The function returns after it starts the operation, but the closure isn’t called until the operation is completed—the closure needs to escape, to be called later. For example:
> 클로저가 탈출 할 수있는 한 가지 방법은 함수 외부에 정의 된 변수에 저장하는 것입니다. 예를 들어, 비동기 작업을 시작하는 많은 함수는 completion handler로 클로저 인수를 사용합니다. 이 함수는 작업을 시작한 후에 반환되지만 작업이 완료 될 때까지 클로저가 호출되지 않습니다. 클로저는 나중에 호출하려면 이스케이프해야합니다. 예를 들면 :  

```swift
var completionHandlers = [() -> Void]()
func someFunctionWithEscapingClosure(completionHandler: @escaping () -> Void) {
completionHandlers.append(completionHandler)
}
```

The someFunctionWithEscapingClosure(_:) function takes a closure as its argument and adds it to an array that’s declared outside the function. If you didn’t mark the parameter of this function with @escaping, you would get a compile-time error.
> someFunctionWithEscapingClosure (_:) 함수는 클로저를 인수로 사용하여 함수 외부에서 선언 된 배열에 추가합니다. 이 함수의 매개 변수를 @escaping으로 표시하지 않으면 컴파일 타임 오류가 발생합니다.  

An escaping closure that refers to self needs special consideration if self refers to an instance of a class. Capturing self in an escaping closure makes it easy to accidentally create a strong reference cycle. For information about reference cycles, see Automatic Reference Counting.
> self를 참조하는 이스케이프 클로저는 self가 클래스의 인스턴스를 참조하는 경우 특별한 고려가 필요합니다. 이스케이프 클로저에서 자신을 캡처하면 실수로 강력한 참조주기를 쉽게 만들 수 있습니다. 참조주기에 대한 자세한 내용은 ARC를 참조하세요. [별도 정리](https://lsh424.tistory.com/77)

Normally, a closure captures variables implicitly by using them in the body of the closure, but in this case you need to be explicit. If you want to capture self, write self explicitly when you use it, or include self in the closure’s capture list. Writing self explicitly lets you express your intent, and reminds you to confirm that there isn’t a reference cycle. For example, in the code below, the closure passed to someFunctionWithEscapingClosure(_:) refers to self explicitly. In contrast, the closure passed to someFunctionWithNonescapingClosure(_:) is a nonescaping closure, which means it can refer to self implicitly.
> 일반적으로 클로저는 클로저 본문에서 변수를 사용하여 암시 적으로 변수를 캡처하지만 이 경우 명시 적이어야합니다. self를 캡처하려면 self를 사용할 때 명시 적으로 작성하거나 클로저의 캡처 목록에 self를 포함합니다. self를 명시 적으로 작성하면 의도를 표현할 수 있으며 참조주기가 없음을 확인하도록 상기시켜줍니다. 예를 들어 아래 코드에서 someFunctionWithEscapingClosure (_:)에 전달 된 클로저는 명시 적으로 self를 참조합니다. 반대로 someFunctionWithNonescapingClosure (_:)에 전달 된 클로저는 nonescaping 클로저입니다. 즉, self를 암시 적으로 참조 할 수 있습니다.  

```swift
func someFunctionWithNonescapingClosure(closure: () -> Void) {
closure()
}

class SomeClass {
var x = 10
func doSomething() {
someFunctionWithEscapingClosure { self.x = 100 }
someFunctionWithNonescapingClosure { x = 200 }
}
}

let instance = SomeClass()
instance.doSomething()
print(instance.x)
// Prints "200"

completionHandlers.first?()
print(instance.x)
// Prints "100"
```

Here’s a version of doSomething() that captures self by including it in the closure’s capture list, and then refers to self implicitly:
> 다음은 클로저의 캡처 목록에 포함하여 자신을 캡처 한 다음 암시 적으로 self를 참조하는 doSomething() 입니다.

```swift
class SomeOtherClass {
var x = 10
func doSomething() {
someFunctionWithEscapingClosure { [self] in x = 100 }
someFunctionWithNonescapingClosure { x = 200 }
}
}
```

If self is an instance of a structure or an enumeration, you can always refer to self implicitly. However, an escaping closure can’t capture a mutable reference to self when self is an instance of a structure or an enumeration. Structures and enumerations don’t allow shared mutability, as discussed in Structures and Enumerations Are Value Types.
> self가 구조체의 인스턴스이거나 열거형이면 항상 암시 적으로 self를 참조 할 수 있습니다. 하지만 escaping closure는 self가 구조체 또는 열거형의 인스턴스일 때 self에 대한 mutable reference를 캡처 할 수 없습니다. 구조체 및 열거형은 공유 변경을 허용하지 않습니다.

```swift
struct SomeStruct {
var x = 10
mutating func doSomething() {
someFunctionWithNonescapingClosure { x = 200 }  // Ok
someFunctionWithEscapingClosure { x = 100 }     // Error
}
}
```
