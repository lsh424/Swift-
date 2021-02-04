# Basic Operators

[Swift Docs](https://docs.swift.org/swift-book/LanguageGuide/BasicOperators.html)  


## Ternary Conditional Operator

Here’s an example, which calculates the height for a table row. The row height should be 50 points taller than the content height if the row has a header, and 20 points taller if the row doesn’t have a header:  
> 다음은 행의 높이를 계산하는 3항 연산자 사용 예시입니다. 행에 헤더가있는 경우 행 높이는 콘텐츠 높이보다 50 포인트 더 커야하며 행에 헤더가 없으면 20 포인트 더 높아야합니다.  

```swift
let contentHeight = 40
let hasHeader = true
let rowHeight: Int

if hasHeader {
 rowHeight = contentHeight + 50
} else {
 rowHeight = contentHeight + 20
}
// rowHeight is equal to 90
```  

3항 연산자를 사용하면 위의 코드를 아래와 같이 간결하게 표현 가능하다.

```swift
let contentHeight = 40
let hasHeader = true
let rowHeight = contentHeight + (hasHeader ? 50 : 20) // rowHeight is equal to 90
```  

The ternary conditional operator provides an efficient shorthand for deciding which of two expressions to consider. Use the ternary conditional operator with care, however. Its conciseness can lead to hard-to-read code if overused. Avoid combining multiple instances of the ternary conditional operator into one compound statement.  
> 삼항 조건 연산자는 주의해서 사용해야한다. 과도한 축약은 읽기 어려운 코드로 이어질 수 있다. 삼항 조건 연산자의 여러 인스턴스를 하나의 복합 명령문으로 결합하지 마세용.


## Nil-Coalescing Operator

The nil-coalescing operator (a ?? b) is shorthand for the code below:  

```swift
a != nil ? a! : b    // (a ?? b)와 같음
``` 

The example below uses the nil-coalescing operator to choose between a default color name and an optional user-defined color name:  
> 아래 예제는 nil-coalescing 연산자를 사용하여 기본 색상 이름과 선택적 사용자 정의 색상 이름 중에서 선택합니다.

```swift
let defaultColorName = "red"
var userDefinedColorName: String?   // defaults to nil

var colorNameToUse = userDefinedColorName ?? defaultColorName
// userDefinedColorName is nil, so colorNameToUse is set to the default of "red" (userDefinedColorName가 nil이기 때문에 "red"로 설정 됨)
``` 

