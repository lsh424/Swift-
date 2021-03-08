# Deinitialization

[Swift Docs](https://docs.swift.org/swift-book/LanguageGuide/Deinitialization.html)  

A deinitializer is called immediately before a class instance is deallocated. You write deinitializers with the deinit keyword, similar to how initializers are written with the init keyword. Deinitializers are only available on class types.
> deinitializer는 클래스 인스턴스가 할당 해제되기 직전에 호출됩니다. 이니셜 라이저가 init 키워드로 작성되는 방식과 유사하게 deinit 키워드로 deinitizers를 작성합니다. Deinitializer는 클래스 유형에서만 사용할 수 있습니다.


## How Deinitialization Works

Swift automatically deallocates your instances when they’re no longer needed, to free up resources. Swift handles the memory management of instances through automatic reference counting (ARC), as described in Automatic Reference Counting. Typically you don’t need to perform manual cleanup when your instances are deallocated. However, when you are working with your own resources, you might need to perform some additional cleanup yourself. For example, if you create a custom class to open a file and write some data to it, you might need to close the file before the class instance is deallocated.
> Swift는 더 이상 필요하지 않은 인스턴스를 자동으로 할당 해제하여 리소스를 확보합니다. Swift는 자동 참조 계산에 설명 된대로 자동 참조 계산 (ARC)을 통해 인스턴스의 메모리 관리를 처리합니다. 일반적으로 인스턴스 할당이 취소 될 때 수동 정리를 수행 할 필요가 없습니다. 그러나 자체 리소스로 작업하는 경우 몇 가지 추가 정리를 직접 수행해야 할 수 있습니다. 예를 들어 사용자 정의 클래스를 만들어 파일을 열고 일부 데이터를 쓰는 경우 클래스 인스턴스가 할당 해제되기 전에 파일을 닫아야 할 수 있습니다.

Class definitions can have at most one deinitializer per class. The deinitializer doesn’t take any parameters and is written without parentheses:
> 클래스 정의에는 클래스 당 최대 하나의 deinitializer가 있을 수 있습니다. deinitializer는 매개 변수를 사용하지 않으며 괄호없이 작성됩니다.

```swift
deinit {
    // perform the deinitialization
}
```  

Deinitializers are called automatically, just before instance deallocation takes place. You aren’t allowed to call a deinitializer yourself. Superclass deinitializers are inherited by their subclasses, and the superclass deinitializer is called automatically at the end of a subclass deinitializer implementation. Superclass deinitializers are always called, even if a subclass doesn’t provide its own deinitializer.
> Deinitializer는 인스턴스 할당 해제가 발생하기 직전에 자동으로 호출됩니다. 직접 deinitializer를 호출 할 수 없습니다. 수퍼 클래스 deinitializer는 해당 서브 클래스에 의해 상속되며, 수퍼 클래스 deinitializer는 서브 클래스 deinitializer 구현이 끝날 때 자동으로 호출됩니다. 수퍼 클래스 deinitializer는 하위 클래스가 자체적 인 deinitializer를 제공하지 않더라도 항상 호출됩니다.

Because an instance isn’t deallocated until after its deinitializer is called, a deinitializer can access all properties of the instance it’s called on and can modify its behavior based on those properties (such as looking up the name of a file that needs to be closed).
> 인스턴스는 deinitializer가 호출 될 때까지 할당 해제되지 않기 때문에 deinitializer는 호출 된 인스턴스의 모든 속성에 액세스 할 수 있으며 해당 속성을 기반으로 동작을 수정할 수 있습니다 (예 : 닫아야하는 파일의 이름 조회).
