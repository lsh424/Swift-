# Strings and Characters

[Swift Docs](https://docs.swift.org/swift-book/LanguageGuide/StringsAndCharacters.html)  


Swift’s String type is a value type. If you create a new String value, that String value is copied when it’s passed to a function or method, or when it’s assigned to a constant or variable.  
> Swift의 문자열 유형은 value type 이다. 새 문자열 값을 만드는 경우 해당 문자열 값은 함수 또는 메서드에 전달되거나 상수 또는 변수에 할당 될 때 복사됩니다.

다음과 같이 문자열간 덧셈 연산을 지원한다. 물론 += 연산자두 지원

```swift
let string1 = "hello"
let string2 = " there"
var welcome = string1 + string2 // welcome now equals "hello there"
```  

