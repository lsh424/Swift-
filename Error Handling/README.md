# Error Handling

[Swift Docs](https://docs.swift.org/swift-book/LanguageGuide/ErrorHandling.html) 
<br>
<br>

Error handling is the process of responding to and recovering from error conditions in your program. Swift provides first-class support for throwing, catching, propagating, and manipulating recoverable errors at runtime.
> 오류 처리는 프로그램의 오류 조건에 응답하고 복구하는 프로세스입니다. Swift는 런타임에 복구 가능한 오류를 던지고, 포착하고, 전파하고, 조작하기위한 first-class를 제공합니다.

Some operations aren’t guaranteed to always complete execution or produce a useful output. Optionals are used to represent the absence of a value, but when an operation fails, it’s often useful to understand what caused the failure, so that your code can respond accordingly.
> 일부 작업은 항상 실행을 완료하거나 유용한 출력을 생성한다고 보장되지는 않습니다. Optional은 값이 없음을 나타내는 데 사용되지만 작업이 실패 할 경우 코드가 그에 따라 응답 할 수 있도록 오류의 원인을 이해하는데 유용한 경우가 많습니다.

As an example, consider the task of reading and processing data from a file on disk. There are a number of ways this task can fail, including the file not existing at the specified path, the file not having read permissions, or the file not being encoded in a compatible format. Distinguishing among these different situations allows a program to resolve some errors and to communicate to the user any errors it can’t resolve.
> 예를 들어, 디스크의 파일에서 데이터를 읽고 처리하는 작업을 생각해봅시다. 지정된 경로에없는 파일, 읽기 권한이없는 파일 또는 호환 가능한 형식으로 인코딩되지 않은 파일을 포함하여이 작업이 실패 할 수있는 여러 가지 경우가 있습니다. 이러한 다양한 상황을 구분하면 프로그램에서 일부 오류를 해결하고 해결할 수 없는 오류를 사용자에게 알릴 수 있습니다.

<br>

## Representing and Throwing Errors

In Swift, errors are represented by values of types that conform to the Error protocol. This empty protocol indicates that a type can be used for error handling.
> Swift에서 오류는 오류 프로토콜을 준수하는 값 타입으로 표시됩니다. 이 빈 프로토콜은 해당 타입이 오류 처리에 사용할 수 있음을 나타냅니다.

Swift enumerations are particularly well suited to modeling a group of related error conditions, with associated values allowing for additional information about the nature of an error to be communicated. For example, here’s how you might represent the error conditions of operating a vending machine inside a game:
> Swift 열거형은 특히 오류의 특성에 대한 추가 정보를 전달할 수 있도록 관련 값을 사용하여 관련 오류 조건 그룹을 모델링하는 데 적합합니다. 예를 들어 다음은 게임 내에서 자동 판매기를 작동하는 오류 조건을 나타내는 방법입니다.

```swift
enum VendingMachineError: Error {
    case invalidSelection
    case insufficientFunds(coinsNeeded: Int)
    case outOfStock
}
```  
Throwing an error lets you indicate that something unexpected happened and the normal flow of execution can’t continue. You use a throw statement to throw an error. For example, the following code throws an error to indicate that five additional coins are needed by the vending machine:
> 오류가 발생하면 예상치 못한 일이 발생하여 정상적인 실행 흐름을 계속할 수 없음을 나타낼 수 있습니다. throw 문을 사용하여 오류를 발생시킵니다. 예를 들어 다음 코드는 자동 판매기에 5 개의 추가 코인이 필요함을 나타내는 오류를 발생시킵니다.

```swift
throw VendingMachineError.insufficientFunds(coinsNeeded: 5)
```  

<br>

## Handling Errors

When an error is thrown, some surrounding piece of code must be responsible for handling the error—for example, by correcting the problem, trying an alternative approach, or informing the user of the failure.
> 오류가 발생하면 주변 코드 일부가 오류를 처리해야합니다. 예를 들어 문제를 수정하거나 다른 방법을 시도하거나 사용자에게 오류를 알리는 등의 방법으로 오류를 처리해야합니다.

There are four ways to handle errors in Swift. You can propagate the error from a function to the code that calls that function, handle the error using a do-catch statement, handle the error as an optional value, or assert that the error will not occur. Each approach is described in a section below.
> Swift에서 오류를 처리하는 방법에는 네 가지가 있습니다. 함수에서 해당 함수를 호출하는 코드로 오류를 전파하거나 do-catch 문을 사용하여 오류를 처리하거나 오류를 optional value로 처리하거나 오류가 발생하지 않을 것이라고 assert할 수 있습니다. 각 접근 방식은 아래 섹션에 설명되어 있습니다.

### Propagating Errors Using Throwing Functions

To indicate that a function, method, or initializer can throw an error, you write the throws keyword in the function’s declaration after its parameters. A function marked with throws is called a throwing function. If the function specifies a return type, you write the throws keyword before the return arrow (->).
> 함수, 메서드 또는 이니셜라이저에서 오류가 발생할 수 있음을 나타내려면 해당 매개변수 뒤에 함수선언에 throws 키워드를 작성합니다. throws로 표시된 함수를 throwing 함수라고합니다. 함수가 반환 타입을 지정하는 경우 반환 화살표 (->) 앞에 throws 키워드를 씁니다.

```swift
func canThrowErrors() throws -> String
``` 

Note
Only throwing functions can propagate errors. Any errors thrown inside a nonthrowing function must be handled inside the function.
> throwing function만 오류를 전파 할 수 있습니다. nonthrowing function 내에서 발생한 모든 오류는 함수 내에서 처리되어야합니다.

In the example below, the VendingMachine class has a vend(itemNamed:) method that throws an appropriate VendingMachineError if the requested item isn’t available, is out of stock, or has a cost that exceeds the current deposited amount:
> 아래 예에서 VendingMachine 클래스에는 요청 된 항목을 사용할 수 없거나 재고가 없거나 현재 입금액을 초과하는 비용이있는 경우 적절한 VendingMachineError를 발생시키는 vend (itemNamed :) 메서드가 있습니다.

```swift
struct Item {
    var price: Int
    var count: Int
}

class VendingMachine {
    var inventory = [
        "Candy Bar": Item(price: 12, count: 7),
        "Chips": Item(price: 10, count: 4),
        "Pretzels": Item(price: 7, count: 11)
        ]
        var coinsDeposited = 0

    func vend(itemNamed name: String) throws {
        guard let item = inventory[name] else {
            throw VendingMachineError.invalidSelection
        }

        guard item.count > 0 else {
            throw VendingMachineError.outOfStock
            }

        guard item.price <= coinsDeposited else {
            throw VendingMachineError.insufficientFunds(coinsNeeded: item.price - coinsDeposited)
        }

        coinsDeposited -= item.price

        var newItem = item
        newItem.count -= 1
        inventory[name] = newItem

        print("Dispensing \(name)")
    }
}
``` 

Because the vend(itemNamed:) method propagates any errors it throws, any code that calls this method must either handle the errors—using a do-catch statement, try?, or try!—or continue to propagate them. For example, the buyFavoriteSnack(person:vendingMachine:) in the example below is also a throwing function, and any errors that the vend(itemNamed:) method throws will propagate up to the point where the buyFavoriteSnack(person:vendingMachine:) function is called.
> vend(itemNamed :) 메서드는 발생하는 오류를 전파하기 때문에 이 메서드를 호출하는 모든 코드는 do-catch 문, try? 또는 try!를 사용하여 오류를 처리하거나 계속 전파해야합니다. 예를 들어, 아래 예의 buyFavoriteSnack(person : vendingMachine :)도 던지는 함수이며 vend(itemNamed :) 메서드가 던지는 오류는 buyFavoriteSnack(person : vendingMachine :)함수가 호출되는 곳 까지 전파됩니다. 

```swift
let favoriteSnacks = [
    "Alice": "Chips",
    "Bob": "Licorice",
    "Eve": "Pretzels",
]

func buyFavoriteSnack(person: String, vendingMachine: VendingMachine) throws {
    let snackName = favoriteSnacks[person] ?? "Candy Bar"
    try vendingMachine.vend(itemNamed: snackName)
}
``` 

In this example, the buyFavoriteSnack(person: vendingMachine:) function looks up a given person’s favorite snack and tries to buy it for them by calling the vend(itemNamed:) method. Because the vend(itemNamed:) method can throw an error, it’s called with the try keyword in front of it.
> 이 예에서 buyFavoriteSnack(person : vendingMachine :) 함수는 주어진 사람이 좋아하는 간식을 찾아서 vend (itemNamed :) 메서드를 호출하여 구매하려고합니다. vend(itemNamed :) 메서드는 오류를 발생시킬 수 있으므로 앞에 try 키워드를 사용하여 호출됩니다.

Throwing initializers can propagate errors in the same way as throwing functions. For example, the initializer for the PurchasedSnack structure in the listing below calls a throwing function as part of the initialization process, and it handles any errors that it encounters by propagating them to its caller.
> Throwing initializer는 함수가 오류를 던지는 것과 같은 방식으로 오류를 전파 할 수 있습니다. 예를 들어, 아래 목록의 PurchasedSnack 구조체에 대한 이니셜 라이저는 초기화 프로세스의 일부로 throwing 함수를 호출하고 발생하는 오류를 호출자에게 전파하여 처리합니다.

```swift
struct PurchasedSnack {
    let name: String
    init(name: String, vendingMachine: VendingMachine) throws {
        try vendingMachine.vend(itemNamed: name)
        self.name = name
    }
}
``` 

### Handling Errors Using Do-Catch

You use a do-catch statement to handle errors by running a block of code. If an error is thrown by the code in the do clause, it’s matched against the catch clauses to determine which one of them can handle the error.
> do-catch문을 사용하여 코드 블록을 실행하여 오류를 처리합니다. do 절의 코드에서 오류가 발생하면 catch 절과 비교하여 오류를 처리 할 수있는 절을 결정합니다.

Here is the general form of a do-catch statement:
> 다음은 do-catch 문의 일반적인 형식입니다.

```swift
do {
    try expression
    statements
} catch pattern 1 {
    statements
} catch pattern 2 where condition {
    statements
} catch pattern 3, pattern 4 where condition {
    statements
} catch {
    statements
}
``` 

You write a pattern after catch to indicate what errors that clause can handle. If a catch clause doesn’t have a pattern, the clause matches any error and binds the error to a local constant named error. 
> catch 후에 패턴을 작성하여 해당 절이 처리 할 수있는 오류를 표시합니다. catch 절에 패턴이없는 경우 절은 모든 오류를 일치시키고 error라는 로컬 상수에 오류를 바인딩합니다. 

For example, the following code matches against all three cases of the VendingMachineError enumeration.
> 예를 들어 다음 코드는 VendingMachineError 열거의 세 가지 경우 모두와 일치합니다.

```swift
var vendingMachine = VendingMachine()
vendingMachine.coinsDeposited = 8
do {
    try buyFavoriteSnack(person: "Alice", vendingMachine: vendingMachine)
    print("Success! Yum.")
} catch VendingMachineError.invalidSelection {
    print("Invalid Selection.")
} catch VendingMachineError.outOfStock {
    print("Out of Stock.")
} catch VendingMachineError.insufficientFunds(let coinsNeeded) {
    print("Insufficient funds. Please insert an additional \(coinsNeeded) coins.")
} catch {
    print("Unexpected error: \(error).")
}
// Prints "Insufficient funds. Please insert an additional 2 coins."
``` 

In the above example, the buyFavoriteSnack(person:vendingMachine:) function is called in a try expression, because it can throw an error. If an error is thrown, execution immediately transfers to the catch clauses, which decide whether to allow propagation to continue. If no pattern is matched, the error gets caught by the final catch clause and is bound to a local error constant. If no error is thrown, the remaining statements in the do statement are executed.
> 위의 예에서 buyFavoriteSnack(person : vendingMachine :)함수는 오류가 발생할 수 있으므로 try 표현식에서 호출됩니다. 오류가 발생하면 실행은 즉시 전달을 허용할지 여부를 결정하는 catch 절로 전송됩니다. 일치하는 패턴이 없으면 최종 catch 절에서 오류를 포착하고 로컬 오류 상수에 바인딩됩니다. 오류가 발생하지 않으면 do 문의 나머지 문이 실행됩니다.

The catch clauses don’t have to handle every possible error that the code in the do clause can throw. If none of the catch clauses handle the error, the error propagates to the surrounding scope. However, the propagated error must be handled by some surrounding scope. In a nonthrowing function, an enclosing do-catch statement must handle the error. In a throwing function, either an enclosing do-catch statement or the caller must handle the error. If the error propagates to the top-level scope without being handled, you’ll get a runtime error.
> catch 절은 do 절의 코드에서 발생할 수있는 모든 가능한 오류를 처리 할 필요는 없습니다. 오류를 처리하는 catch 절이 없으면 오류가 주변 범위로 전파됩니다. 그러나 전파 된 오류는 일부 주변 범위에서 처리해야합니다. 던지지 않는 함수에서는 둘러싸는 do-catch 문이 오류를 처리해야합니다. 던지는 함수에서 둘러싸는 do-catch 문이나 호출자가 오류를 처리해야합니다. 오류가 처리되지 않고 최상위 범위로 전파되면 런타임 오류가 발생합니다.

### Converting Errors to Optional Values

You use try? to handle an error by converting it to an optional value. If an error is thrown while evaluating the try? expression, the value of the expression is nil. For example, in the following code x and y have the same value and behavior:
> try? 사용해서 오류를 optional value로 변환하여 처리합니다. try? expression를 평가하는 동안 오류가 발생하면 식의 값은 nil입니다. 예를 들어, 다음 코드에서 x와 y는 동일한 값과 동작을 갖습니다.

```swift
func someThrowingFunction() throws -> Int {
    // ...
}

let x = try? someThrowingFunction()

let y: Int?
do {
    y = try someThrowingFunction()
} catch {
    y = nil
}
``` 

If someThrowingFunction() throws an error, the value of x and y is nil. Otherwise, the value of x and y is the value that the function returned. Note that x and y are an optional of whatever type someThrowingFunction() returns. Here the function returns an integer, so x and y are optional integers.
> someThrowingFunction()에서 오류가 발생하면 x와 y의 값은 nil입니다. 그렇지 않으면 x와 y의 값은 함수가 반환 한 값입니다. x와 y는 someThrowingFunction()이 반환하는 모든 optional 타입입니다. 여기서 함수는 정수를 반환하므로 x와 y는 optional integer입니다.

Using try? lets you write concise error handling code when you want to handle all errors in the same way. For example, the following code uses several approaches to fetch data, or returns nil if all of the approaches fail.
> try?를 사용해서 모든 오류를 동일한 방식으로 처리하려는 경우 간결한 오류 처리 코드를 작성할 수 있습니다. 예를 들어, 다음 코드는 여러 접근 방식을 사용하여 데이터를 가져 오거나 모든 접근 방식이 실패하면 nil을 반환합니다.

```swift
func fetchData() -> Data? {
    if let data = try? fetchDataFromDisk() { return data }
    if let data = try? fetchDataFromServer() { return data }
    return nil
}
``` 
