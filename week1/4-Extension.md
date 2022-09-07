# ❔ 질문
Extension에 대해 설명하시오. <br />
Extension 내부에서 함수를 override할 수 있는지 설명하시오.<br />

# ✏️ 작성자
- [릴리, 호댕](#릴리-호댕)
- [호랭이, 나무](#호랭이-나무)
<br />

---

# 릴리, 호댕
## Extension에 대해 설명하시오.

Extension은 이미 존재하는 클래스, 구조체, 열거형, 프로토콜 타입에 **새로운 기능을 추가**하기 위해 존재합니다. 또한 원본 타입에는 접근하지 못하더라도 Extension을 통해 추가로 기능을 추가할 때 Extension을 사용합니다. 
<br />
<br />

## Extension 내부에서 함수를 override할 수 있는지 설명하시오.

Objective-C와의 호환성을 위해 @objc 키워드를 붙이면 override가 가능하긴 하나, Swift에선 공식적으로 Extension에선 override를 금지하고 있습니다.
<br />
<br />

## Extension에 뭐가 들어갈 수 있는지 설명해주세요.

- 인스턴스 연산 프로퍼티 & 타입 연산 프로퍼티 추가
- 인스턴스 메서드 & 타입 메서드를 정의
- 새로운 이니셜라이져 제공
- subscript 정의
- 새로운 중첩타입 정의와 사용
- 기존 타입이 프로토콜을 구현하는데 사용
<br />
<br />

## Extension에서 구현하지 못하는 것은 무엇이 있는지 설명해주세요

저장 프로퍼티, property observer는 안됩니다.
<br />
<br />

## 클래스에서 Extension으로 이니셜라이저를 추가하려면 어떤 형태로 추가를 해야 하는지 아시나요?

클래스의 경우 convenience init으로 추가를 해줘야 합니다. 지정 이니셜라이저는 새롭게 추가할 수 없습니다.
<br />
<br />

## 구조체에서 커스텀 이니셜라이저를 사용하면서 디폴트 이니셜라이저, 멤버와이즈 이니셜라이저를 사용할 수 있는 방법에 대해 알고 있나요?

익스텐션에 커스텀 이니셜라이져를 정의해주면 됩니다. 그럼 자동으로 생성된 디폴트 이니셜라이져와 멤버와이즈 이니셜라이져를 사용하면서 커스텀 이니셜라이져도 사용할 수 있습니다.
<br />
<br />

---

# 고사리, 나무
## Extension에 대해 설명하시오.

- add new functionality to an existing class, structure, enumeration, or protocol type
존재하는 타입에 새로운 기능을 추가할 때 사용하는 기능입니다.
- 익스텐션에서 제공하는 기능은?
    - Add computed instance properties and computed type properties
    → 연산 프로퍼티 추가
    - Define instance methods and type methods
    → 메서드 추가
    - Provide new initializers
    → 이니셜라이저 제공
    - Define subscripts
    → 서브스크립트 정의
    - Define and use new nested types
    → 중첩 타입 정의 및 사용
    - Make an existing type conform to a protocol
    → 프로토콜 채택 및 준수

[Extensions — The Swift Programming Language (Swift 5.6)](https://docs.swift.org/swift-book/LanguageGuide/Extensions.html)
<br />
<br />

## Extension 내부에서 함수를 override할 수 있는지 설명하시오.

[https://docs.swift.org/swift-book/LanguageGuide/Extensions.html#:~:text=Extensions can add new functionality to a type%2C but they can’t override existing functionality](https://docs.swift.org/swift-book/LanguageGuide/Extensions.html#:~:text=Extensions%20can%20add%20new%20functionality%20to%20a%20type%2C%20but%20they%20can%E2%80%99t%20override%20existing%20functionality).

→ 공식 문서에 따르면, 익스텐션에서 기능의 오버라이드는 불가능합니다.

메서드가 다이내믹하고 `@objc` 속성을 가질 때, 예외적으로 override가 가능한데 이는 컴파일러가 Objective-C와의 호환성을 위해 Extension에서 Override를 허용하기 때문입니다. 

그럼에도 Extension 내의 override를 하는 것은 두 가지의 이유로 피하는 것이 좋습니다.

1. Extension의 기능상 의도와 불일치한다. (기능의 추가)
2. Objective-C 런타임 중, NSObject 상속 메서드와 Extension에 override로 정의된 메서드 중 어떤 것으로 초기화될 것인지 보장할 수 없다.

<img alt="image" src="https://user-images.githubusercontent.com/73867548/188714400-bc377280-25ed-41af-87dc-8e769aee3d22.png">

```swift
class Superclass {
    @objc dynamic func foo() {
        print("print it")
    }
}

class SubClass: Superclass {

}

extension SubClass {
    @objc override dynamic func foo() {
        print("Does it work?")
    }
}

let subClass = SubClass()
subClass.foo()

// Does it work?
```

참고링크: https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/CustomizingExistingClasses/CustomizingExistingClasses.html#//apple_ref/doc/uid/TP40011210-CH6-SW4

<br />
<br />


## 익스텐션을 활용하여 class의 상속을 대체하는 방법은 무엇이 있을까요?

프로토콜을 채택하고 익스텐션에서 프로토콜 기본 구현을 작성하는 방법이 있습니다.
<br />
<br />

## 대체하는 것에는 어떤 장점이 있나요?

1. 값타입의 장점(적은 비용, Thread Safe 등)을 가지며 공통된 기능을 구현할 수 있습니다.
2. 단일 상속에 비해 다중 채택이 가능하고, 수평적인 관계를 만들 수 있습니다.
<br />
<br />

## dynamic 키워드의 기능을 설명해주세요

항상 동적으로 디스패치되게 강제합니다. → 컴파일타임에 가상화되거나 인라인되지 않게 합니다.
→ 어떤 메서드를 실행할지를 런타임에 결정하도록 강제합니다.
<br />
<br />

## dynamic dispatch와 static dispatch의 차이점을 설명해 주세요

- dynamic dispatch: 어떤 메서드를 실행할지를 런타임에 결정합니다.
- static dispatch: 컴파일타임에 미리 어떤 메서드를 실행할지 결정합니다.

위의 메서드가 dynamic해야 하는 이유: Objective-C의 Class는 override의 가능성 때문에 dynamic dispatch를 사용합니다. 해당 메서드와 Objective-C의 호환성을 위해 dynamic하게 만들 필요가 있습니다.
<br />
<br />
