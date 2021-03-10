# Nested Types

[Swift Docs](https://docs.swift.org/swift-book/LanguageGuide/NestedTypes.html)  

Enumerations are often created to support a specific class or structure’s functionality. Similarly, it can be convenient to define utility classes and structures purely for use within the context of a more complex type. To accomplish this, Swift enables you to define nested types, whereby you nest supporting enumerations, classes, and structures within the definition of the type they support.
> 열거형은 종종 특정 클래스 또는 구조체의 기능을 지원하기 위해 생성됩니다. 마찬가지로, 더 복잡한 타입의 컨텍스트 내에서 사용하기 위해 순수하게 유틸리티 클래스 및 구조체를 정의하는 것이 편리 할 수 있습니다. 이를 수행하기 위해 Swift는 중첩된 타입을 정의 할 수 있도록하여 지원하는 타입의 정의 내에서 지원하는 열거형, 클래스 및 구조체를 중첩합니다.

To nest a type within another type, write its definition within the outer braces of the type it supports. Types can be nested to as many levels as are required.
> 다른 타입 내에 타입을 중첩하려면 지원하는 타입의 외부 중괄호 안에 정의를 작성하십시오. 타입은 필요한만큼의 수준으로 중첩 될 수 있습니다.

```swift
struct BlackjackCard {

    // nested Suit enumeration
    enum Suit: Character {
        case spades = "♠", hearts = "♡", diamonds = "♢", clubs = "♣"
    }

    // nested Rank enumeration
    enum Rank: Int {
        case two = 2, three, four, five, six, seven, eight, nine, ten
        case jack, queen, king, ace
        struct Values {
            let first: Int, second: Int?
        }
        var values: Values {
            switch self {
            case .ace:
                return Values(first: 1, second: 11)
            case .jack, .queen, .king:
                return Values(first: 10, second: nil)
            default:
                return Values(first: self.rawValue, second: nil)
            }
        }
    }

    // BlackjackCard properties and methods
    let rank: Rank, suit: Suit
    var description: String {
        var output = "suit is \(suit.rawValue),"
        output += " value is \(rank.values.first)"
        if let second = rank.values.second {
            output += " or \(second)"
        }
        return output
    }
}
```  
