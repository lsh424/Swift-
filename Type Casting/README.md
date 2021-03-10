# Type Casting

[Swift Docs](https://docs.swift.org/swift-book/LanguageGuide/TypeCasting.html)  

Type casting is a way to check the type of an instance, or to treat that instance as a different superclass or subclass from somewhere else in its own class hierarchy.
> 타입 캐스팅은 인스턴스의 타입을 확인하거나 해당 인스턴스를 자체 클래스 계층 구조의 다른 위치에서 다른 수퍼 클래스 또는 하위 클래스로 취급하는 방법입니다.

Type casting in Swift is implemented with the is and as operators. These two operators provide a simple and expressive way to check the type of a value or cast a value to a different type.
> Swift의 타입 캐스팅은 is 및 as 연산자로 구현됩니다. 이 두 연산자는 값의 타입을 확인하거나 값을 다른 타입으로 캐스트하는 간단하고 표현적인 방법을 제공합니다.

```swift
class MediaItem {
    var name: String
    init(name: String) {
        self.name = name
    }
}

class Movie: MediaItem {
    var director: String
    init(name: String, director: String) {
        self.director = director
        super.init(name: name)
    }
}

class Song: MediaItem {
    var artist: String
    init(name: String, artist: String) {
        self.artist = artist
        super.init(name: name)
    }
}
```  

The final snippet creates a constant array called library, which contains two Movie instances and three Song instances. The type of the library array is inferred by initializing it with the contents of an array literal. Swift’s type checker is able to deduce that Movie and Song have a common superclass of MediaItem, and so it infers a type of [MediaItem] for the library array:
> 마지막 스니펫은 두 개의 Movie 인스턴스와 세 개의 Song 인스턴스를 포함하는 라이브러리라는 상수 배열을 만듭니다. 라이브러리 배열의 유형은 배열 리터럴의 내용으로 초기화하여 유추됩니다. Swift의 유형 검사기는 Movie 및 Song에 MediaItem의 공통 수퍼 클래스가 있음을 추론 할 수 있으므로 라이브러리 배열에 대해 [MediaItem] 유형을 추론합니다.

```swift
let library = [
    Movie(name: "Casablanca", director: "Michael Curtiz"),
    Song(name: "Blue Suede Shoes", artist: "Elvis Presley"),
    Movie(name: "Citizen Kane", director: "Orson Welles"),
    Song(name: "The One And Only", artist: "Chesney Hawkes"),
    Song(name: "Never Gonna Give You Up", artist: "Rick Astley")
]
// the type of "library" is inferred to be [MediaItem]
``` 

The items stored in library are still Movie and Song instances behind the scenes. However, if you iterate over the contents of this array, the items you receive back are typed as MediaItem, and not as Movie or Song. In order to work with them as their native type, you need to check their type, or downcast them to a different type, as described below.
> 라이브러리에 저장된 항목은 영화 및 노래 인스턴스입니다. 그러나 이 배열의 내용을 순회하는 경우 다시받은 항목은 Movie 또는 Song이 아닌 MediaItem으로 입력됩니다. 원래 타입으로 작업하려면 아래에 설명 된대로 유형을 확인하거나 다른 유형으로 다운 캐스트해야합니다.

## Checking Type

Use the type check operator (is) to check whether an instance is of a certain subclass type. The type check operator returns true if the instance is of that subclass type and false if it’s not.
> 타입 검사 연산자 (is)를 사용하여 인스턴스가 특정 하위 클래스 타입인지 확인합니다. 타입 검사 연산자는 인스턴스가 해당 하위 클래스 유형이면 true를 반환하고 그렇지 않으면 false를 반환합니다.

```swift
var movieCount = 0
var songCount = 0

for item in library {
    if item is Movie {
        movieCount += 1
    } else if item is Song {
        songCount += 1
    }
}

print("Media library contains \(movieCount) movies and \(songCount) songs")
// Prints "Media library contains 2 movies and 3 songs"
``` 

## Downcasting

A constant or variable of a certain class type may actually refer to an instance of a subclass behind the scenes. Where you believe this is the case, you can try to downcast to the subclass type with a type cast operator (as? or as!).
> 특정 클래스 타입의 상수 또는 변수는 하위 클래스 인스턴스를 참조 할 수 있습니다. 이것이 사실이라고 생각하는 경우 타입 캐스트 연산자 (as? 또는 as!)를 사용하여 하위 클래스 타입으로 다운 캐스트 할 수 있습니다.

Because downcasting can fail, the type cast operator comes in two different forms. The conditional form, as?, returns an optional value of the type you are trying to downcast to. The forced form, as!, attempts the downcast and force-unwraps the result as a single compound action.
> 다운 캐스트가 실패 할 수 있기 때문에 타입 캐스트 연산자는 두 가지 다른 형태로 제공됩니다. 조건부 형식 as?는 다운 캐스트하려는 유형의 optional 값을 반환합니다. 강제 형식 as!는 다운 캐스트를 시도하고 결과를 강제 해제합니다.

Use the conditional form of the type cast operator (as?) when you aren’t sure if the downcast will succeed. This form of the operator will always return an optional value, and the value will be nil if the downcast was not possible. This enables you to check for a successful downcast.
> 다운 캐스트가 성공할지 확실하지 않은 경우 타입 캐스트 연산자 (as?)의 조건부 형식을 사용합니다. 이 형식의 연산자는 항상 optional 값을 반환하며 다운 캐스트가 불가능한 경우 값은 nil이됩니다. 이를 통해 성공적인 다운 캐스트를 확인할 수 있습니다.

Use the forced form of the type cast operator (as!) only when you are sure that the downcast will always succeed. This form of the operator will trigger a runtime error if you try to downcast to an incorrect class type.
> 다운 캐스트가 항상 성공할 것이라고 확신하는 경우에만 타입 캐스트 연산자 (as!)의 강제 형식을 사용하십시오. 이 형식의 연산자는 잘못된 클래스 타입으로 다운 캐스트하려고하면 런타임 오류를 트리거합니다.

The example below iterates over each MediaItem in library, and prints an appropriate description for each item. To do this, it needs to access each item as a true Movie or Song, and not just as a MediaItem. This is necessary in order for it to be able to access the director or artist property of a Movie or Song for use in the description.
> 아래 예제는 라이브러리의 각 MediaItem을 반복하고 각 항목에 대한 적절한 설명을 인쇄합니다. 이렇게하려면 MediaItem이 아닌 실제 영화 또는 노래로 각 항목에 액세스해야합니다. 이는 설명에 사용하기 위해 영화 또는 노래의 감독 또는 아티스트 자산에 액세스 할 수 있도록하기 위해 필요합니다.

In this example, each item in the array might be a Movie, or it might be a Song. You don’t know in advance which actual class to use for each item, and so it’s appropriate to use the conditional form of the type cast operator (as?) to check the downcast each time through the loop:
> 이 예에서 배열의 각 항목은 영화이거나 노래 일 수 있습니다. 각 항목에 사용할 실제 클래스를 미리 알지 못하므로 루프를 통해 매번 다운 캐스트를 확인하려면 타입 캐스트 연산자 (as?)의 조건부 형식을 사용하는 것이 적절합니다.

```swift
for item in library {
    if let movie = item as? Movie {
        print("Movie: \(movie.name), dir. \(movie.director)")
    } else if let song = item as? Song {
        print("Song: \(song.name), by \(song.artist)")
    }
}

// Movie: Casablanca, dir. Michael Curtiz
// Song: Blue Suede Shoes, by Elvis Presley
// Movie: Citizen Kane, dir. Orson Welles
// Song: The One And Only, by Chesney Hawkes
// Song: Never Gonna Give You Up, by Rick Astley
``` 

## Type Casting for Any and AnyObject

Swift provides two special types for working with nonspecific types:
> Swift는 비특정 타입 작업을 위해 두 가지 특수 타입을 제공합니다.

* Any can represent an instance of any type at all, including function types. (Any는 함수 타입을 포함하여 모든 타입의 인스턴스를 나타낼 수 있습니다.)
* AnyObject can represent an instance of any class type.(AnyObject는 모든 클래스 타입의 인스턴스를 나타낼 수 있습니다.)

Use Any and AnyObject only when you explicitly need the behavior and capabilities they provide. It’s always better to be specific about the types you expect to work with in your code.
> 제공하는 동작과 기능이 명시적으로 필요한 경우에만 Any 및 AnyObject를 사용하십시오. 코드에서 작업 할 것으로 예상되는 타입을 구체적으로 지정하는 것이 항상 좋습니다.

Here’s an example of using Any to work with a mix of different types, including function types and nonclass types. The example creates an array called things, which can store values of type Any:
> 다음은 Any를 사용하여 함수 타입 및 비클래스 타입을 포함하여 다양한 타입을 혼합하여 작업하는 예입니다. 이 예제에서는 Any 타입의 값을 저장할 수있는 things라는 배열을 만듭니다.

```swift
var things = [Any]()

things.append(0)
things.append(0.0)
things.append(42)
things.append(3.14159)
things.append("hello")
things.append((3.0, 5.0))
things.append(Movie(name: "Ghostbusters", director: "Ivan Reitman"))
things.append({ (name: String) -> String in "Hello, \(name)" })

for thing in things {
    switch thing {
    case 0 as Int:
        print("zero as an Int")
    case 0 as Double:
        print("zero as a Double")
    case let someInt as Int:
        print("an integer value of \(someInt)")
    case let someDouble as Double where someDouble > 0:
        print("a positive double value of \(someDouble)")
    case is Double:
        print("some other double value that I don't want to print")
    case let someString as String:
        print("a string value of \"\(someString)\"")
    case let (x, y) as (Double, Double):
        print("an (x, y) point at \(x), \(y)")
    case let movie as Movie:
        print("a movie called \(movie.name), dir. \(movie.director)")
    case let stringConverter as (String) -> String:
        print(stringConverter("Michael"))
    default:
        print("something else")
    }
}

// zero as an Int
// zero as a Double
// an integer value of 42
// a positive double value of 3.14159
// a string value of "hello"
// an (x, y) point at 3.0, 5.0
// a movie called Ghostbusters, dir. Ivan Reitman
// Hello, Michael
``` 
