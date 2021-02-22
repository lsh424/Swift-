# Subscripts

[Swift Docs](https://docs.swift.org/swift-book/LanguageGuide/Subscripts.html)

Classes, structures, and enumerations can define subscripts, which are shortcuts for accessing the member elements of a collection, list, or sequence. You use subscripts to set and retrieve values by index without needing separate methods for setting and retrieval. For example, you access elements in an Array instance as someArray[index] and elements in a Dictionary instance as someDictionary[key].
> 클래스, 구조체 및 열거형은 컬렉션, 목록 또는 시퀀스의 구성요소에 액세스하기위해 바로 접근할 수 있는 subscripts를 정의 할 수 있습니다. subscripts를 사용하여 설정 및 검색을위한 별도의 방법없이 인덱스별로 값을 설정하고 검색합니다. 예를 들어 Array 인스턴스의 요소에 someArray [index]로 액세스하고 Dictionary 인스턴스의 요소에 someDictionary [key]로 액세스합니다.

## Subscript Syntax

```swift
subscript(index: Int) -> Int {
    get {
        // Return an appropriate subscript value here.
    }
    set(newValue) {
        // Perform a suitable setting action here.
    }
}
``` 

Here’s an example of a read-only subscript implementation
> 다음은 읽기 전용 subscript 구현의 예입니다.

```swift
struct TimesTable {
    let multiplier: Int
    subscript(index: Int) -> Int {
        return multiplier * index
    }
}

let threeTimesTable = TimesTable(multiplier: 3)
print("six times three is \(threeTimesTable[6])") // Prints "six times three is 18"
``` 

## Subscript Usage

The exact meaning of “subscript” depends on the context in which it’s used. Subscripts are typically used as a shortcut for accessing the member elements in a collection, list, or sequence. You are free to implement subscripts in the most appropriate way for your particular class or structure’s functionality.
> “subscript”의 정확한 의미는 사용되는 컨텍스트에 따라 다릅니다. 일반적으로 subscript는 컬렉션, 리스트 또는 시퀀스의 멤버 요소에 액세스하기위한 바로가기로 사용됩니다. 특정 클래스 또는 구조체의 기능에 가장 적합한 방식으로 subscript를 자유롭게 구현할 수 있습니다.

For example, Swift’s Dictionary type implements a subscript to set and retrieve the values stored in a Dictionary instance. You can set a value in a dictionary by providing a key of the dictionary’s key type within subscript brackets, and assigning a value of the dictionary’s value type to the subscript:
> 예를 들어 Swift의 Dictionary 타입은 Dictionary 인스턴스에 저장된 값을 설정하고 검색하기 위해 subscript를 구현합니다. subscript 안에 사전 키 유형의 키를 제공하고 subscript에 사전 값 타입의 값을 할당하여 사전에 값을 설정할 수 있습니다.

```swift
var numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
numberOfLegs["bird"] = 2
``` 

The example above defines a variable called numberOfLegs and initializes it with a dictionary literal containing three key-value pairs. The type of the numberOfLegs dictionary is inferred to be [String: Int]. After creating the dictionary, this example uses subscript assignment to add a String key of "bird" and an Int value of 2 to the dictionary.
> 위의 예는 numberOfLegs라는 변수를 정의하고 세 개의 키-값 쌍을 포함하는 dictionary literal로 초기화합니다. numberOfLegs 딕셔너리는 [String : Int]로 추론됩니다. 딕셔너리를 만든 후이 예제에서는 subscript를 사용하여 문자열 키 "bird"와 Int 값 2를 사전에 추가합니다.


## Subscript Options

Subscripts can take any number of input parameters, and these input parameters can be of any type. Subscripts can also return a value of any type.
> Subscript는 여러 입력 매개 변수를 사용할 수 있으며 이러한 입력 매개 변수는 모든 타입이 될 수 있습니다. 아래 첨자는 모든 타입의 값을 반환 할 수도 있습니다.

Like functions, subscripts can take a varying number of parameters and provide default values for their parameters, as discussed in Variadic Parameters and Default Parameter Values. However, unlike functions, subscripts can’t use in-out parameters.
> 함수와 마찬가지로 subscript는 가변 매개 변수 및 기본 매개 변수 값에 설명 된대로 다양한 수의 매개 변수를 사용하고 매개 변수에 대한 기본값을 제공 할 수 있습니다. 그러나 함수와 달리 아래 첨자는 in-out 매개 변수를 사용할 수 없습니다.

While it’s most common for a subscript to take a single parameter, you can also define a subscript with multiple parameters if it’s appropriate for your type. The following example defines a Matrix structure, which represents a two-dimensional matrix of Double values. The Matrix structure’s subscript takes two integer parameters:
> subscript가 단일 매개 변수를 사용하는 것이 가장 일반적이지만 타입에 적합한 경우 여러 매개 변수로 subscript를 정의 할 수도 있습니다. 다음 예제에서는 Double 값의 2차원 행렬을 나타내는 Matrix 구조체를 정의합니다. Matrix 구조체의 subscript는 두 개의 정수 매개 변수를 사용합니다.

```swift
struct Matrix {
    let rows: Int, columns: Int
    var grid: [Double]
    init(rows: Int, columns: Int) {
        self.rows = rows
        self.columns = columns
        grid = Array(repeating: 0.0, count: rows * columns)
    }
    func indexIsValid(row: Int, column: Int) -> Bool {
        return row >= 0 && row < rows && column >= 0 && column < columns
    }
    subscript(row: Int, column: Int) -> Double {
        get {
            assert(indexIsValid(row: row, column: column), "Index out of range")
            return grid[(row * columns) + column]
        }
        set {
            assert(indexIsValid(row: row, column: column), "Index out of range")
            grid[(row * columns) + column] = newValue
        }
    }
}
``` 

Matrix provides an initializer that takes two parameters called rows and columns, and creates an array that’s large enough to store rows * columns values of type Double. Each position in the matrix is given an initial value of 0.0. To achieve this, the array’s size, and an initial cell value of 0.0, are passed to an array initializer that creates and initializes a new array of the correct size. This initializer is described in more detail in Creating an Array with a Default Value.
> Matrix는 행과 열이라는 두 개의 매개 변수를 사용하는 이니셜 라이저를 제공하고 Double 타입의 행 * 열 값을 저장할 수있을만큼 큰 배열을 만듭니다. 행렬의 각 위치에는 초기 값 0.0이 지정됩니다. 이를 위해 배열의 크기와 초기 셀 값 0.0이 올바른 크기의 새 배열을 만들고 초기화하는 배열 이니셜 라이저에 전달됩니다. 이 이니셜 라이저는 기본값으로 배열 만들기에 자세히 설명되어 있습니다.

## Type Subscripts
Instance subscripts, as described above, are subscripts that you call on an instance of a particular type. You can also define subscripts that are called on the type itself. This kind of subscript is called a type subscript. You indicate a type subscript by writing the static keyword before the subscript keyword. Classes can use the class keyword instead, to allow subclasses to override the superclass’s implementation of that subscript. The example below shows how you define and call a type subscript:
> 위에서 설명한대로 인스턴스 subscript는 특정 유형의 인스턴스에서 호출하는 subscript입니다. 타입 자체에서 호출되는 subscript를 정의 할 수도 있습니다. 이러한 종류의 subscript를 type subscript라고합니다. subscript 키워드 앞에 static 키워드를 작성하여 type subscript를 나타냅니다. 클래스는 대신 class 키워드를 사용하여 하위 클래스가 해당 subscript의 수퍼 클래스 구현을 override 할 수 있습니다. 아래 예제는 type subscript를 정의하고 호출하는 방법을 보여줍니다.

```swift
enum Planet: Int {
    case mercury = 1, venus, earth, mars, jupiter, saturn, uranus, neptune
    static subscript(n: Int) -> Planet {
        return Planet(rawValue: n)! 
    }
}

let mars = Planet[4]
print(mars)
``` 
