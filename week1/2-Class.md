# ❔ 질문
class의 성능을 향상시킬수 있는 방법들을 나열해보시오.<br />
Convenience init에 대해 설명하시오.<br />

# ✏️ 작성자
- [지성, 예하](#지성-예하)
- [허황, 줄라이](#허황-줄라이)
<br />

---

# 지성, 예하
## class의 성능을 향상시킬수 있는 방법들을 나열해보시오. 
클래스는 Dynamic Dispatch로 동작합니다. 상속과 override를 할 수 있기 때문에 프로그램이 클래스가 어떤 메서드나 프로퍼티를 참조하는지 런타임에 결정해야 합니다. 메서드 테이블에서 메서드를 찾은 후 간접 호출을 통해 동작하도록 구현되어 있습니다. 간접 호출은 직접 호출보다 느리고, 비용이 더 듭니다. Dynamic Dispatch를 Static Dispatch로 바꿔주어 런타임 오버헤드(어떤 처리를 하기 위해 들어가는 간접적인 처리 시간, 메모리)를 줄이면 클래스의 성능이 향상됩니다. 

동적인 동작이 필요하지 않을 때, Dynamic Dispatch의 사용을 줄여 클래스의 성능을 향상시키는 방법에는 크게 세 가지가 있습니다. 

1. `final`
    - `final` 키워드를 붙이면 메서드, 프로퍼티를 override할 수 없습니다. 클래스 정의 앞에 붙이면 서브클래싱(상속) 할 수 없고, 모든 메서드와 프로퍼티가 `final`임을 암시합니다. `final` 키워드를 붙이면 컴파일러가 dynamic dispatch 간접 호출을 제거합니다.
    - [https://docs.swift.org/swift-book/LanguageGuide/Inheritance.html](https://docs.swift.org/swift-book/LanguageGuide/Inheritance.html)
    - <img width="934" alt="image" src="https://user-images.githubusercontent.com/60090790/185962988-86aecd1e-6099-4a00-82d5-f366fa088d84.png">
    - <img width="936" alt="image" src="https://user-images.githubusercontent.com/60090790/185963053-d47fbaca-acd5-4934-a72d-c532b88faa70.png">
2. `private`
    - `private` 키워드 붙인 메서드나 프로퍼티는 한 파일 내에서만 참조되도록 제한됩니다.
    - 컴파일러는 `private` 키워드가 붙은 파일에서 오버라이드가 되지 않았다면 직접 호출로 바꾸어 Static Dispatch로 동작하게 됩니다.
3. Whole Module Optimization
    - 클래스를 `internal` (아무 것도 지정하지 않았을 때 기본값) 접근으로 선언하면 선언된 모듈 내부에서만 볼 수 있습니다.
    - Swift는 기본적으로 컴파일시 모듈 내 파일들을 각각 컴파일합니다.
    - 파일 하나하나 컴파일할 경우 각 파일만 검사하므로 `internal` level의 클래스가 다른 파일에서 오버라이딩되는지를 파악할 수 없어서 dynamic dispatch로 동작하게 됩니다.
    - WMO를 활성화하면 모듈 전체를 동시에 컴파일하여(검사 범위: 모듈 전체) `internal` level의 클래스들이 오버라이딩 되지 않는 경우 내부적으로 `final`로 추론해 static dispatch로 동작하게 하여 성능을 향상시킬 수 있습니다.
    - `pubilc` 키워드가 추가된 경우 `final` 추론이 불가합니다.

**Increasing Performance by Reducing Dynamic Dispatch** [https://developer.apple.com/swift/blog/?id=27](https://developer.apple.com/swift/blog/?id=27)

(번역) [https://mildwhale.github.io/2020-01-31-increasing-performance-by-reducing-dynamic-dispatch/](https://mildwhale.github.io/2020-01-31-increasing-performance-by-reducing-dynamic-dispatch/)

<br />
<br />

## Convenience init에 대해 설명하시오.

- Class가 정의할 수 있는 보조 이니셜라이저입니다.
- 스위프트에서는 클래스가 모든 저장 프로퍼티가 초기값을 받게 하기 위해 designated initializer와 convenience initializer를 정의합니다.
    
<img width="919" alt="image" src="https://user-images.githubusercontent.com/60090790/185963352-320b7bbb-23ef-4c60-855b-c1439bb5c820.png">
    
- 필요하지 않으면 정의하지 않아도 됩니다.
    
<img width="934" alt="image" src="https://user-images.githubusercontent.com/60090790/185963379-a86a9e8f-4ea3-4214-83c8-041c42c26d8f.png">
<br />
<br />

## Static Dispatch, Dynamic Dispatch에 대해 설명해주세요.
- dispatch란 어떤 메서드를 호출할 것인지를 결정하고 결정한 것을 실행하는 메커니즘입니다.
- Swift엔 Static Dispatch와 Dynamic Dispatch가 존재합니다.
- Static Dispatch는 컴파일 타임에 어떤 메서드를 호출할 것인지 실제 코드 위치를 파악하고 실행하는 방식입니다.
    - 메서드 인라이닝 (메서드를 호출할 때 해당 메서드로 이동하지 않고 메서드의 결과값을 바로 반환, 성능 향상)
- Dynamic Dispatch는 런타임에 메서드 테이블에서 어떤 메서드를 호출할 것인지 결정하고 실행하는 방식입니다.

****Understanding Swift Performance**** [https://developer.apple.com/videos/play/wwdc2016/416/](https://developer.apple.com/videos/play/wwdc2016/416/)
<br />
<br />

## protocol에선 method dispatch가 어떤식으로 동작하나요? 
- protocol에 메서드를 정의하고 extension에 해당 메서드를 기본 구현하면 (Witness Table을 활용한) Dynamic Dispatch를 사용합니다. 런타임에 구현체를 참조합니다.
- protocol에 메서드를 정의하지 않고, extension에 메서드가 기본 구현돼있는 경우 extension은 메서드 오버라이딩이 불가능하기 때문에 Static Dispatch를 사용합니다.

****Understanding Swift Performance**** [https://developer.apple.com/videos/play/wwdc2016/416/](https://developer.apple.com/videos/play/wwdc2016/416/)
<br />
<br />

## WMO와 기존 컴파일 방식의 차이는 무엇인가요? 
- Xcode가 컴파일하는 방식은 다음과 같습니다
    - 파일을 개별적으로 컴파일합니다
    - 이 과정이 병렬적으로 실행되어 속도가 향상될 수 있다
    - 필요한 파일만 다시 컴파일할 수 있다
    - <img width="915" alt="image" src="https://user-images.githubusercontent.com/60090790/185963695-233d51de-28e3-4321-a418-e1689a083491.png">
    - WMO는 기존의 방식보다 빌드 시간은 오래 걸릴 수 있습니다. (이 부분도 Swift2에서 속도 개선이 있었다고 합니다.)
    - 하지만, 빌드된 바이너리는(빌드가 된 후에 실행은) 더 빠릅니다.
    - <img width="921" alt="image" src="https://user-images.githubusercontent.com/60090790/185963793-27140c9a-5f66-431e-8db5-2364bf8abf98.png">
    - <img width="924" alt="image" src="https://user-images.githubusercontent.com/60090790/185963829-fedc371d-c6bb-4f2f-a69e-ce62675ff602.png">
****Optimizing Swift Performance**** [https://developer.apple.com/videos/play/wwdc2015/409/?time=433](https://developer.apple.com/videos/play/wwdc2015/409/?time=433)
<br />
<br />

## designated initializer 는 무엇인가요?
Class의 기본적인 이니셜라이저입니다. 
<br />
<br />

## designated initializer와 convenience initializer를 사용할 때 규칙은 무엇인가요?
- designated initializer는 직계 슈퍼클래스의 designated initializer를 호출해야 합니다.
- convenience initializer는 동일한 클래스의 다른 이니셜라이저를 호출해야 합니다.
- convenience initializer는 최종적으로 designated initializer를 호출해야 합니다.
<img width="927" alt="image" src="https://user-images.githubusercontent.com/60090790/185963969-b5af4359-3799-45b1-a78c-249a482a9a58.png">
https://docs.swift.org/swift-book/LanguageGuide/Initialization.html
<br />
<br />
<br />

---

# 허황, 줄라이 
## class의 성능을 향상 시킬수 있는 방법들을 나열해보시오.
참고:  h[ttps://developer.apple.com/videos/play/wwdc2016/416/](https://developer.apple.com/videos/play/wwdc2016/416/)

클래스는 상속을 통해 메서드와 속성 프로퍼티을 재정의 할 수 있다.

클래스의 성능 향상에 대해 설명하기 앞서 Dispatch의 개념에 대해 먼저 알아보자면 Dispatch는 어떤 메소드가 호출될 것인지 결정하고 실행하는 과정이다.

Dispatch의 종류에는 Dynamic Dispatch, Static Dispatch 두가지가 있다

Static Dispatch는 컴파일 시점에서 메서드가 호출될 지 명확하게 결정되는 것을 말하고,

Dynamic Dispatch의 경우 런타임 시점에서 메서드가 호출될 지 결정되는 것을 말한다.

즉, 컴파일 시점에 어떤 함수가 호출될 지 알 수 없기 때문에 런타임 시 메서드를 찾기 위한 오버헤드가 발생한다.

클래스의 경우 상속을 통해 메서드를 재정의 될 수 있기 때문에 Dynamic Dispatch가 일어난다.

클래스의 Dynamic Dispatch를 Static Dispatch로 바꿔 클래스의 성능을 향상시킬 수 있는 방법은 3가지가 있다.

1. final 키워드

final 키워드는 클래스가 더 이상 상속을 하지 않겠다는 키워드로서 final 키워드가 붙은 클래스의 경우 Static Dispatch가 일어나기 때문에 런타임 시 오버헤드를 줄일 수 있어 성능을 향상 시킬 수 있다.

2. private 

접근 제어 private를 사용하면 현재 파일에 존재하는 타입만 참조할 수 있다.

컴파일 시점에서 private 키워드가 붙은 타입의 재정의 선언이 없으면 final 키워드를 자동으로 판단하여 Static Dispatch로 동작하게 된다.

3. Whole Module Optimization

internal acssess level은 정의된 모듈 내에서만 접근이 가능하다. 스위프트는 기본적으로 모듈별로 컴파일이 되기 때문에 서로 다른 모듈에서 override 되었는지 확인이 불가능하다. 

whole module optimization을 사용하면 모든 모듈을 한번에 컴파일하게 된다.

서로 다른 모듈의 클래스가 재정의 되었는지 추론할 수 있게되고 그렇지 않은 경우 내부적으로 final 키워드를 붙여 Static Dispatch로 동작하게 된다.

[https://developer.apple.com/swift/blog/?id=27](https://developer.apple.com/swift/blog/?id=27)
<br />
<br />

## 프로토콜을 사용한 구조체의 경우는 어떤 Dispatch가 일어나는가? 
PWT(Protocol Withness Table)를 사용해서 Dynamic Dispatch가 일어난다.


❗ https://developer.apple.com/videos/play/wwdc2016/416/ 참고 WWDC PDF 137p ~ 140p

<br />
<br />

## 프로토콜을 사용한 구조체가 Dynamic Dispatch가 일어난다면 어떻게 성능을 향상 시킬 수 있는가?
제네릭을 사용하여 컴파일 시점에 타입을 유추할 수 있다.(Static Polymorphism)

PWT를 사용하긴 하지만 특정 타입을 찾아갈 필요없이 PWT자체를 사용하게 된다.

컴파일 시점에 타입을 유추할 수 있기 때문에 컴파일러는 특정 타입 버전의 메서드를 만든다.

❗ https://developer.apple.com/videos/play/wwdc2016/416/ 참고 WWDC PDF 209p ~ 240p

<br />
<br />

## Convenience init에 대해 설명하시오.
클래스는 모든 프로퍼티가 초기화 시점에 기본값을 가져야 한다.

Designated Initializer, Convenience Initializer 두가지 종류의 이니셜라이저가 있다. 

Designated Initializer는 클래스의 모든 프로퍼티가 초기화 될 수 있도록 하는 역할을 한다.

Convenience Initializer는 보조 이니셜라이저로 Designated Initializer를 도와주는 역할을 한다.

Convenience Initializer는 사용할 때는 반드시 Designated Initializer를 호출해야 한다.
<br />
<br />

## 초기화 규칙은 무엇인가?
모든 멤버가 초기화 되어야 한다.

**규칙 1**

지정 이니셜라이저는 상속 받은 부모 클래스의 지정 이니셜라이저를 호출해야 한다.

**규칙 2**

편의 이니셜라이저는 동일한 클래스의 다른 이니셜라이저를 호출해야 한다.

**규칙 3**

편의 이니셜라이져는 최종적으로 지정 이니셜라이져를 반드시 호출해야한다.
<br />
<br />

## 편의 이니셜라이저는 언제 사용하는가?
이니셜라이저를 개발자의 의도에 맞게 커스터마이징 하고 싶을 때 사용한다.
<br />
<br />
<br />
