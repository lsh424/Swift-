# Inheritance

[Swift Docs](https://docs.swift.org/swift-book/LanguageGuide/Inheritance.html)  


A class can inherit methods, properties, and other characteristics from another class. When one class inherits from another, the inheriting class is known as a subclass, and the class it inherits from is known as its superclass. Inheritance is a fundamental behavior that differentiates classes from other types in Swift.
> 클래스는 다른 클래스에서 메서드, 속성 및 기타 특성을 상속 할 수 있습니다. 한 클래스가 다른 클래스에서 상속 될 때 상속하는 클래스를 subclass라고하고 상속 된 클래스를 superclass라고합니다. 상속은 Swift의 다른 타입과 클래스를 구별하는 기본적인 동작입니다.

Classes in Swift can call and access methods, properties, and subscripts belonging to their superclass and can provide their own overriding versions of those methods, properties, and subscripts to refine or modify their behavior. Swift helps to ensure your overrides are correct by checking that the override definition has a matching superclass definition.
> Swift의 클래스는 수퍼 클래스에 속하는 메서드, 프로퍼티 및 subscript를 호출하고 액세스 할 수 있으며 해당 메서드, 프로퍼티 및 subscript의 overriding version을 제공하여 동작을 재정의하거나 수정할 수 있습니다. Swift는 override에 일치하는 슈퍼 클래스 정의가 있는지 확인하여 override가 올바른지 확인하는 데 도움이됩니다.

Classes can also add property observers to inherited properties in order to be notified when the value of a property changes. Property observers can be added to any property, regardless of whether it was originally defined as a stored or computed property.
> 클래스는 프로퍼티 값이 변경 될 때 알림을 받기 위해 상속 된 프로퍼티에 프로퍼티 옵저버를 추가 할 수도 있습니다. 프로퍼티 옵저버는 저장 프로퍼티인지 계산 프로퍼티인지 여부에 관계없이 모든 프로퍼티에 추가 할 수 있습니다.

```swift
class Vehicle {
    var currentSpeed = 0.0
    var description: String {
        return "traveling at \(currentSpeed) miles per hour"
    }
    func makeNoise() {
        // do nothing - an arbitrary vehicle doesn't necessarily make a noise
    }
}

```  

```swift
let someVehicle = Vehicle()

print("Vehicle: \(someVehicle.description)")
// Vehicle: traveling at 0.0 miles per hour
```  

```swift
// Subclassing
class Bicycle: Vehicle {
    var hasBasket = false
}
```  

```swift
let bicycle = Bicycle()
bicycle.hasBasket = true

bicycle.currentSpeed = 15.0
print("Bicycle: \(bicycle.description)")
// Bicycle: traveling at 15.0 miles per hour
```  

```swift
class Tandem: Bicycle {
    var currentNumberOfPassengers = 0
}
```  

```swift
let tandem = Tandem()
tandem.hasBasket = true
tandem.currentNumberOfPassengers = 2
tandem.currentSpeed = 22.0
print("Tandem: \(tandem.description)")
// Tandem: traveling at 22.0 miles per hour
```

```swift
class Train: Vehicle {
    // Overriding Methods
    override func makeNoise() {
        print("Choo Choo")
    }
}

let train = Train()
train.makeNoise()
// Prints "Choo Choo"
```

```swift
class Car: Vehicle {
    var gear = 1
    // Overriding Property
    override var description: String {
        return super.description + " in gear \(gear)"
    }
}

let car = Car()
car.currentSpeed = 25.0
car.gear = 3
print("Car: \(car.description)")
// Car: traveling at 25.0 miles per hour in gear 3
```


```swift
class AutomaticCar: Car {
    // Overriding Property Observers
    override var currentSpeed: Double {
        didSet {
            gear = Int(currentSpeed / 10.0) + 1
        }
    }
}

let automatic = AutomaticCar()
automatic.currentSpeed = 35.0
print("AutomaticCar: \(automatic.description)")
// AutomaticCar: traveling at 35.0 miles per hour in gear 4
```

## Preventing Overrides

You can prevent a method, property, or subscript from being overridden by marking it as final. Do this by writing the final modifier before the method, property, or subscript’s introducer keyword (such as final var, final func, final class func, and final subscript).
> 메서드, 프로퍼티 또는 subscript를 final로 표시하여 override 되는 것을 방지 할 수 있습니다. (예 : final var, final func, final class func 및 final subscript) 

Any attempt to override a final method, property, or subscript in a subclass is reported as a compile-time error. Methods, properties, or subscripts that you add to a class in an extension can also be marked as final within the extension’s definition.
> 하위 클래스에서 final 메서드, 프로퍼티 또는 subscript를 override하려는 모든 시도는 컴파일 타임 오류가 발생합니다. 클래스의 extension에 추가하는 메서드, 프로퍼티 또는 subscript는 extension 내에서 final로 표시 될 수도 있습니다.

You can mark an entire class as final by writing the final modifier before the class keyword in its class definition (final class). Any attempt to subclass a final class is reported as a compile-time error.
> 클래스 정의 (최종 클래스)에서 class 키워드 앞에 final을 작성하여 전체 클래스를 final로 표시 할 수 있습니다. 최종 클래스를 서브클래싱하려는 모든 시도는 컴파일 타임 오류가 발생합니다. 

