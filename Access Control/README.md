# Access Control

[Swift Docs](https://docs.swift.org/swift-book/LanguageGuide/AccessControl.html#) 

Access control restricts access to parts of your code from code in other source files and modules. This feature enables you to hide the implementation details of your code, and to specify a preferred interface through which that code can be accessed and used.
> Access control은 다른 소스 파일 및 모듈의 코드에서 코드 부분에 대한 액세스를 제한합니다. 이 기능을 사용하면 코드의 구현 세부 정보를 숨기고 해당 코드에 액세스하고 사용할 수있는 기본 인터페이스를 지정할 수 있습니다.

You can assign specific access levels to individual types (classes, structures, and enumerations), as well as to properties, methods, initializers, and subscripts belonging to those types. Protocols can be restricted to a certain context, as can global constants, variables, and functions.
> 특정 액세스 수준을 개별 타입(클래스, 구조체 및 열거형)과 해당 타입에 속하는 프로퍼티, 메서드, 이니셜라이저 및 섭스크립트에 할당 할 수 있습니다. 프로토콜은 전역 상수, 변수 및 함수와 마찬가지로 특정 컨텍스트로 제한 될 수 있습니다.

In addition to offering various levels of access control, Swift reduces the need to specify explicit access control levels by providing default access levels for typical scenarios. Indeed, if you are writing a single-target app, you may not need to specify explicit access control levels at all.
> 다양한 수준의 액세스 제어를 제공하는 것 외에도 Swift는 일반적인 시나리오에 대한 기본 액세스 수준을 제공하여 명시적으로 access control level을 지정할 필요성을 줄여줍니다. 실제로 단일 대상 앱을 작성하는 경우 명시적인 access control level을 지정할 필요가 없습니다.

NOTE  
The various aspects of your code that can have access control applied to them (properties, types, functions, and so on) are referred to as “entities” in the sections below, for brevity.
> access control을 적용 할 수있는 코드의 다양한 측면(프로퍼티, 타입, 함수 등)은 간결성을 위해 아래 섹션에서 "엔터티"라고합니다.

## Modules and Source Files

Swift’s access control model is based on the concept of modules and source files.
> Swift의 access control 모델은 모듈 및 소스 파일의 개념을 기반으로합니다.

A module is a single unit of code distribution—a framework or application that’s built and shipped as a single unit and that can be imported by another module with Swift’s import keyword.
> 모듈은 단일 단위로 빌드 및 제공되고 Swift의 import키워드를 사용하여 다른 모듈에서 가져올 수있는 프레임워크 또는 애플리케이션인 단일 코드 배포 단위입니다.

Each build target (such as an app bundle or framework) in Xcode is treated as a separate module in Swift. If you group together aspects of your app’s code as a stand-alone framework—perhaps to encapsulate and reuse that code across multiple applications—then everything you define within that framework will be part of a separate module when it’s imported and used within an app, or when it’s used within another framework.
> Xcode의 각 빌드 대상(예 : 앱 번들 또는 프레임 워크)은 Swift에서 별도의 모듈로 처리됩니다. 앱 코드의 여러 측면을 독립형 프레임 워크로 그룹화하면(아마도 여러 애플리케이션에서 해당 코드를 캡슐화하고 재사용하기 위해) 해당 프레임워크 내에서 정의한 모든 것은 앱 내에서 가져 와서 사용할 때나 다른 프레임 워크 내에서 사용될 때 별도의 모듈에 속하게됩니다. 

A source file is a single Swift source code file within a module (in effect, a single file within an app or framework). Although it’s common to define individual types in separate source files, a single source file can contain definitions for multiple types, functions, and so on.
> 소스 파일은 모듈 내의 단일 Swift 소스 코드 파일입니다(실제로 앱 또는 프레임워크 내의 단일 파일). 개별 타입을 별도의 소스 파일에 정의하는 것이 일반적이지만 단일 소스 파일에 여러 타입, 함수 등에 대한 정의가 포함될 수 있습니다.

## Access Levels

Swift provides five different access levels for entities within your code. These access levels are relative to the source file in which an entity is defined, and also relative to the module that source file belongs to.
> Swift는 코드 내의 엔터티에 대해 5 가지 액세스 수준을 제공합니다. 이러한 액세스 수준은 엔티티가 정의 된 소스 파일과 관련되며 소스 파일이 속한 모듈과 관련됩니다.

* Open access and public access enable entities to be used within any source file from their defining module, and also in a source file from another module that imports the defining module. You typically use open or public access when specifying the public interface to a framework. The difference between open and public access is described below.
> Open 및 public 액세스를 사용하면 정의 모듈의 모든 소스 파일과 정의 모듈을 가져 오는 다른 모듈의 소스 파일에서 엔티티를 사용할 수 있습니다. 일반적으로 프레임 워크에대한 공용 인터페이스를 지정할 때 open 또는 public 액세스를 사용합니다. open 액세스와 public 액세스의 차이점은 아래에 설명되어 있습니다.

* Internal access enables entities to be used within any source file from their defining module, but not in any source file outside of that module. You typically use internal access when defining an app’s or a framework’s internal structure.
> 내부 액세스를 사용하면 정의 모듈의 모든 소스 파일 내에서 엔티티를 사용할 수 있지만 해당 모듈 외부의 소스 파일에서는 사용할 수 없습니다. 일반적으로 앱 또는 프레임 워크의 내부 구조를 정의 할 때 내부 액세스를 사용합니다.

* File-private access restricts the use of an entity to its own defining source file. Use file-private access to hide the implementation details of a specific piece of functionality when those details are used within an entire file.
> File-private 액세스는 엔티티의 사용을 자체 정의 소스 파일로 제한합니다. File-private 액세스를 사용하여 특정 기능에 대한 구현 세부 정보를 전체 파일 내에서 사용하는 경우 이를 숨깁니다.

* Private access restricts the use of an entity to the enclosing declaration, and to extensions of that declaration that are in the same file. Use private access to hide the implementation details of a specific piece of functionality when those details are used only within a single declaration.
> Private 액세스는 엔터티의 사용을 둘러싸는 선언 및 동일한 파일에있는 해당 선언의 확장으로 제한합니다. 이러한 세부 정보가 단일 선언 내에서만 사용되는 경우 특정 기능의 구현 세부 정보를 숨기려면 Private 액세스를 사용하십시오.

Open access is the highest (least restrictive) access level and private access is the lowest (most restrictive) access level.
> Open 액세스는 가장 높은(최소 제한) 액세스 수준이고 private 액세스는 가장 낮은(가장 제한적인) 액세스 수준입니다.

Open access applies only to classes and class members, and it differs from public access by allowing code outside the module to subclass and override, as discussed below in Subclassing. Marking a class as open explicitly indicates that you’ve considered the impact of code from other modules using that class as a superclass, and that you’ve designed your class’s code accordingly.
> Open 액세스는 클래스와 클래스 멤버에만 적용되며, 아래 서브 클래싱에서 설명하는 것처럼 모듈 외부의 코드를 서브 클래스 및 재정의 할 수 있다는 점에서 공개 액세스와 다릅니다. 클래스를 open으로 표시하면 해당 클래스를 수퍼 클래스로 사용하는 다른 모듈의 코드가 미치는 영향을 고려했으며 이에 따라 클래스 코드를 설계했음을 나타냅니다.

별도 정리 - https://lsh424.tistory.com/40?category=948936
