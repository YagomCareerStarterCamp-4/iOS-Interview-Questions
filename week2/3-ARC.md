# ❔ 질문

ARC란 무엇인지 설명하시오. <br />
Retain Count 방식에 대해 설명하시오.

<br />

# ✏️ 작성자

- [호랭이, 예하](#호랭이-예하)
- [호박, 앨리](#호박-앨리)

<br />

---

# 호랭이, 예하

## ARC란 무엇인지 설명하시오.

- Swift에서 메모리를 관리하기 위해 Auto Reference Counting, ARC를 사용합니다.
- 기존에 사용하던 manual reference counting과 동일한 메모리 관리 규칙을 따릅니다.
    ![image](https://user-images.githubusercontent.com/75905803/186048357-4e61d077-3779-4753-9cec-349ecf6da769.png)
    [https://developer.apple.com/library/archive/releasenotes/ObjectiveC/RN-TransitioningToARC/Introduction/Introduction.html#//apple_ref/doc/uid/TP40011226](https://developer.apple.com/library/archive/releasenotes/ObjectiveC/RN-TransitioningToARC/Introduction/Introduction.html#//apple_ref/doc/uid/TP40011226)
- 객체에 대한 레퍼런스 카운트를 관리하고, 0이 되면 자동으로 메모리를 해제`dealloc` 합니다.
- 코드를 분석해 적절한 위치에 retain, release 메서드를 삽입하여 count를 증감시키는 Reference Count(Retain Cont) 방식으로 관리합니다.
    - NSObject retain
        - [https://developer.apple.com/documentation/objectivec/1418956-nsobject/1571946-retain](https://developer.apple.com/documentation/objectivec/1418956-nsobject/1571946-retain)
    - NSObject release
        - [https://developer.apple.com/documentation/objectivec/1418956-nsobject/1571957-release?language=objc](https://developer.apple.com/documentation/objectivec/1418956-nsobject/1571957-release?language=objc)

<br />
<br />

## Retain Count 방식에 대해 설명하시오.

- 컴파일 타임에 retain, release 메서드의 삽입이 이루어지고, 참조 카운트가 0이 되면 메모리가 해제되는 방식으로 메모리를 관리합니다.
- 클래스가 새 인스턴스를 만들면 ARC가 인스턴스의 정보를 담는 메모리를 할당합니다. ARC가 사용 중인 인스턴스를 해지하지 않기 위해 (크래시가 발생할 수 있기에) 인스턴스에 대한 참조를 갖고 있는 프로퍼티를 추적합니다.
    ![image](https://user-images.githubusercontent.com/75905803/186048432-a7b70039-ebca-4e9d-a9b0-4c385c54f646.png)
    [https://docs.swift.org/swift-book/LanguageGuide/AutomaticReferenceCounting.html](https://docs.swift.org/swift-book/LanguageGuide/AutomaticReferenceCounting.html)
    ![image](https://user-images.githubusercontent.com/75905803/186048493-24448cce-8a9a-4b92-8451-360dcf34dc93.png)
    [https://developer.apple.com/videos/play/wwdc2016/416/?time=439](https://developer.apple.com/videos/play/wwdc2016/416/?time=439)
    
<br />
<br />

## ARC가 필요한 이유는?

- 힙은 참조 타입이 저장되고, 개발자가 동적으로 할당하는 메모리 공간이기 때문에 메모리의 관리를 위해서 ARC가 필요합니다.
- 참조형 자료들이 얼마나 참조되고 있는지 카운팅하고, 이에 따라 메모리를 할당 또는 제거해야 하는데, 이를 자동으로 수행해주는 것이 ARC입니다.

<br />
<br />

## ARC가 모든 타입에 적용됩니까?

- 참조 타입에 적용되며, struct, enum 등 값 타입에는 적용되지 않습니다
    ![image](https://user-images.githubusercontent.com/75905803/186048531-2921d785-58c7-4537-9fe8-4a6f071ba49f.png)
    [https://docs.swift.org/swift-book/LanguageGuide/AutomaticReferenceCounting.html](https://docs.swift.org/swift-book/LanguageGuide/AutomaticReferenceCounting.html)

<br />
<br />

## MRR에 대해서 설명하시오.

- Manual Retain-Release, MRR는 Objective-C에서 사용하던 메모리 관리 방법입니다.
- 런타임에 실행됩니다.
- NSObject 클래스 내의 reference counting이라고 알려진 model을 이용해서 구현된다.
- retain, release메서드를 직접 호출해서 메모리 관리를 했습니다..
    ![image](https://user-images.githubusercontent.com/75905803/186048552-60a06cfe-6f09-46e7-8706-28abe0050a38.png)
    [https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html)
    

<br />
<br />

## MRR과 비교해서 ARC의 장점은 무엇입니까?

- 코드의 양이 줄어들고, 컴파일러가 자동으로 메서드를 삽입하고 메모리를 해제하기 때문에 메모리 오류 가능성이 낮아져서 프로그램 안정성이 높아집니다.

<br />
<br />

## 컴파일 타임에 메모리를 관리하는 것의 이점이 무엇입니까?

- 프로그램이 동작할 때 해제(deinit) 시점을 아는 것(가비지 컬렉션)보다 상대적으로 성능이 좋다고 할 수 있습니다. (프로그램이 동작할 때는 감시하기 위해 추가 자원이 필요할테니)
- 참고: 가비지 컬렉션은 맥에서도 있었습니다
    ![image](https://user-images.githubusercontent.com/75905803/186048566-2f0c3500-2f9b-41e3-befa-8b90897f17be.png)
    [https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html)

<br />
<br />
<br />

---

# 호박, 앨리

## ARC란 무엇인지 설명하시오.

`Automatic Reference Counting`의 약자로 자동으로 앱의 메모리 사용량을 추적하고 관리한다. 객체에 대한 **참조 카운트를 관리**하고 **0이 되면 자동으로 메모리를 해제**한다.

<br />
<br />

## Retain Count 방식에 대해 설명하시오.

compile time에 자동으로 `retain`, `release`를 적절한 위치에 삽입하는 방식(런타임에 삽입된 코드가 실행되면서 retain, release에 의해 RC를 증감하고 count가 0이 되면 메모리에서 제거한다)

<br />
<br />

## Strong, weak, Unowned가 무엇인가요?

- Strong
    - 강한참조를 하게 되면 자신이 참조하는 인스턴스의 소유권을 가지게 됩니다.
    - 강한참조를 하게 되면 RC가 증가하게 됩니다.
    - 우리가 사용할 떄 아무것도 적어주지 않으면 강한참조로 선언이 됩니다.
- weak
    - 약한참조는 RC를 증가시키지 않습니다.
    - weak을 사용할 때는 소유자에 비해서 짧은 생명주기를 가진 인스턴스를 참조할 때 사용해야합니다.
    - 대상 인스턴스를 참고하고 있는 중에 언제든지 메모리에서 해제될 수 있기 때문입니다!
- unowned
    - 미소유 참조도 해당 인스턴스의 소유권을 가지지 않으며 자신이 참조하는 인스턴스의 RC를 증가시키지 않습니다.
    - 약한참조는 객체를 계속 추적하면서 객체가 사라지게 되면 nil로 바꾸게 됩니다.
    - 하지만 미소유참조는 객체가 사라지게 되면 댕글링 포인터가 남게 됩니다.
    - 댕글링 포인터를 참조하게 되면 크래쉬가 나면서 앱이 종료될것입니다!
    - 그렇기 때문에 미소유참조는 사라지지 않음이 보장되는 객체에만 사용해야합니다!
    - 댕글링 포인터(Dangling Pointer)
        - 이미 해제된 메모리를 가리키고 있는 포인터를 말합니다.

<br />
<br />

## 힙 영역 메모리 어떤 것인가요?

- 메모리의 힙(heap) 영역은 사용자가 직접 관리 해야만 하는 메모리 영역입니다.
- 힙 영역은 사용자에 의해 메모리 공간이 동적으로 할당되고 해제됩니다.
- 힙 영역은 메모리의 낮은 주소에서 높은 주소의 방향으로 할당됩니다.

<br />
<br />

## 왜 메모리 관리를 해야할까?

- 앱의 성능을 유지하기 위해.
- 메모리에는 크게 스택, 힙이 있는데 스택은 값타입의 데이터가 저장되어 자동으로 제거되기 때문에 특별한 관리가 필요없다.
- 반면 힙에 저장된 데이터는 참조 타입으로, 개발자가 동적으로 관리해주어야한다. 참조 타입의 경우 인스턴스가 참조를 통해 여러 곳에서 접근하므로 언제 메모리가 해제되는지 매우 중요하다. 적절한 시점에 해제되지 않으면 한정적인 메모리 자원을 낭비하게 되고, 성능 저하로 이어지기 때문이다.

<br />
<br />

## GC와 ARC의 차이가 무엇인가요?

힙 메모리를 관리하는 방법은 두 가지가 있습니다.GC(Garbage Collection)와 RC(Reference Count)가 있습니다.JAVA(AOS)에서는 GC를 사용하고 Swift(iOS)에서 RC를 사용합니다.

GC와 RC 모두 사용하지 않는 객체를 해제하기 위한 수단입니다. 하지만 둘의 동작방식에 차이가 있습니다.

<br />
<br />

## GC (Garbage Collection)

- GC는 Mark-and-sweep 알고리즘을 기반으로 동작합니다. Garbage Collector가 GC root를 순회하면서 참조할 수 있는 모든 객체를 마킹합니다.
- 이후 불특정한 시간에 마킹되지 않은(더 이상 필요없는) 모든 객체를 찾아 지웁니다.
- **런타임 시점에서 주기적으로 참조를 추적하여 사용하지 않는 인스턴스를 해제합니다.**

## 장점

- RC만큼 메모리 누수가 되는 순환 참조에 대해서 고려하지 않아도 됩니다. (물론 발생은 하지만 .. RC만큼은 아닙니다..)

## 단점

- 참조해제 되는 시점을 예측할 수 없습니다.
- 런타임 때 참조를 추적하는데에 리소스가 사용되기 때문에 성능저하가 있을 수 있습니다.

<br />
<br />

## RC (Reference Count)

- RC는 ARC의 RC입니다.
- 각 오브젝트의 Reference Counting을 통해서 카운트가 0이 되면 메모리를 해제하는 방식입니다.
- **컴파일 시점에 참조 시점과 해제 시점이 결정되고 런타임 때 그대로 실행됩니다.**

## 장점

- 참조해제 되는 시점을 예측할 수 있습니다.
- 런타임 시점에 추가적인 리소스가 필요하지 않습니다.

## 단점

- 순환참조가 발생할 확률이 GC에 비해 많습니다.
- 순환참조가 발생하면 영구적으로 메모리가 해제 되지 않게 됩니다.
