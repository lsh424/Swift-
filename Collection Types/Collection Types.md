# Collection Types

[Swift Docs](https://docs.swift.org/swift-book/LanguageGuide/CollectionTypes.html)  

Swift provides three primary collection types, known as arrays, sets, and dictionaries, for storing collections of values. Arrays are ordered collections of values. Sets are unordered collections of unique values. Dictionaries are unordered collections of key-value associations.  
> Swift는 값들을 저장하기 위해 배열, 집합 및 사전 세 가지 기본 컬렉션 타입을 제공한다. 배열은 정렬 된 값 모음입니다. 집합은 순서가 지정되지 않은 고유 한 값 모음입니다. 딕셔너리는 순서가 지정되지 않은 키-값 연결 모음입니다.

Creating an Array with a Default Value. 
> 기본값을 포함한 배열 생성

```swift
var threeDoubles = Array(repeating: 0.0, count: 3)  
// threeDoubles is of type [Double], and equals [0.0, 0.0, 0.0]
```  

If you need the integer index of each item as well as its value, use the enumerated() method to iterate over the array instead. For each item in the array, the enumerated() method returns a tuple composed of an integer and the item.
> 각 항목의 정수 인덱스와 해당 값이 필요한 경우 enumerated () 메서드를 사용하여 대신 배열을 반복합니다. 배열의 각 항목에 대해 enumerated () 메서드는 index와 item으로 구성된 튜플을 반환합니다.

```swift
for (index, value) in shoppingList.enumerated() {
print("Item \(index + 1): \(value)")
}
``` 

