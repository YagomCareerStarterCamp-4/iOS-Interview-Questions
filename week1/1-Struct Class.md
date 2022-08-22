# ❔ 질문

struct와 class와 enum의 차이를 설명하시오.<br />
struct가 무엇이고 어떻게 사용하는지 설명하시오.<br />
mutating 키워드에 대해 설명하시오.<br />
<br />

# ✏️ 작성자
- [아샌, 호랭이](#아샌-호랭이)
- [고사리, 제리](#고사리-제리)
<br />

---

# 아샌, 호랭이
## struct와 class와 enum의 차이를 설명하시오.
class는 참조타입이기 때문에 새로 만들어진 변수나 상수가 원본의 값이 아닌 주소를 가리키게 됩니다. 메모리 영역은 힙에 저장이 되고, 힙의 메모리 주소는 힙에 저장이 됩니다.  
struct는 값타입이어서 변수나 상수에 값이 복사되어서 할당이 됩니다. 값은 스택영역에 저장됩니다. 상속이 불가합니다.  
enum은 값타입이며, 관련 값을 하나의 유형으로 묶어서 정의한 타입입니다. 열거형은 위의 두 타입과 다르게 이니셜라이저를 갖지 않으며, 저장 프로퍼티를 가질 수 없고 상속이 불가합니다.  
<br />
<br />

## struct와 class의 공통점은?
- 값을 저장할 속성 정의
- 기능을 제공하는 메소드 정의
- 첨자 구문을 사용하여 값에 대한 액세스를 제공하도록 첨자를 정의합니다.
- 초기 상태를 설정하기 위한 이니셜라이저 정의
- 기본 구현 이상으로 기능을 확장하도록 확장
- 특정 종류의 표준 기능을 제공하는 프로토콜 준수
<br />
<br />   

## 값을 묶어서 정의하는 건 struct, array도 가능한데 enum를 주로 쓰는 이유는?

switch를 사용할 때 간결하게 사용할 수 있고, 코드의 가독성이 높아집니다.  

배열의 경우 nil값이 들어있는 등 런타임 에러가 발생할 수 있지만, enum은 이런 오류를 컴파일러가 잡아주기 떄문에 컴파일타임에 에러를 확인할 수 있습니다.  

구조체로 열거형의 기능적인 요소는 대응할 수 있지만 열거형의 경우 인스턴스를 생성하지 않고 상수로 접근할 수 있도록 만들어져 있기 때문에 실수의 여지가 줄어듭니다.  

열거형의 각 case는 비트 단위의 고유값을 가지기 때문에 비교문에서 비교가 빠르다는 장점을 가집니다.  
<br />
<br />

### Class와 Struct

[Understanding Swift Performance - WWDC16 - Videos - Apple Developer](https://developer.apple.com/videos/play/wwdc2016/416/)

<img width="949" alt="image" src="https://user-images.githubusercontent.com/60090790/185947796-c7e8eb98-ddbe-44b7-84c6-67c6eeb5cce8.png">

<img width="953" alt="image" src="https://user-images.githubusercontent.com/60090790/185947853-d461c39b-bb77-4e8c-bbf8-b9eec0db23ba.png">
<br />
<br />

### enum이 값 타입인 근거

[Structures and Classes - The Swift Programming Language (Swift 5.6)](https://docs.swift.org/swift-book/LanguageGuide/ClassesAndStructures.html)

[Memory Safety - The Swift Programming Language (Swift 5.6)](https://docs.swift.org/swift-book/LanguageGuide/MemorySafety.html)
<br />
<br />

## struct가 무엇이고 어떻게 사용하는지 설명하시오.

Struct는 Class와 동일하게 값을 저장할 타입을 정의하고 기능을 제공하는데, Class에서만 제공하는 상속, 참조 등의 기능이 필요 없을 때 Struct를 사용합니다. 

Class와 다르게 멤버와이즈 이니셜라이저를 제공하고 있기 때문에 별도로 이니셜라이징을 하지 않아도 간편하게 사용할 수 있습니다. Class에서만 사용 가능한 편의 이니셜라이저가 필요할 경우에는, extension을 사용한 이니셜라이징으로 동일한 기능을 구현할 수 있습니다. 

[Structures and Classes - The Swift Programming Language (Swift 5.6)](https://docs.swift.org/swift-book/LanguageGuide/ClassesAndStructures.html)
<br />
<br />

## class에는 왜 멤버와이즈 이니셜라이즈가 없을까요?

class는 상속 가능성이 있기 때문에, 멤버와이즈 이니셜라이즈가 존재한다면 상위 클래스 내부에 변경이 있을 경우 자동화되어 있는 이니셜라이저 중 수정이 필요한 하위 클래스를 찾아서 작업하는 과정에서 오히려 더 번거로워질 수 있다. 이렇게 상속의 특성으로 멤버와이즈 이니셜라이즈의 기능이 문제 여지가 있어서 직접 구현하게 만들어놓은 것이 아닐까.........?

[[스위프트(Swift) 프로그래밍] - 왜 Swift의 클래스(Class)에는 멤버와이즈 초기화(Memberwise Init)가 없을까?](https://jayb-log.tistory.com/258)

mutating을 통한 값을 변경할때, 프로퍼티만 바뀔까? 인스턴스가 새로 만들어져서 덮어써지는 걸까.
<br />
<br />

## mutating 키워드에 대해 설명하시오.

기본적으로 값타입의 프로퍼티는 해당 인스턴스 메서드 내에서 수정할 수 없습니다. 그러나 Struct의 프로퍼티를 메서드를 통해 수정해야하는 경우 mutating 키워드를 사용해야합니다. 이 변경사항은 메서드가 종료될 때 프로퍼티에 적용됩니다. self에 새로운 인스턴스도 할당할 수 있으며, 메서드가 종료되면 새 인스턴스가 기존 인스턴스를 대체합니다.

값타입은 선언 시에 바로 복사를 하지 않고 변경이 이루어졌을 때 실제 복사를 하는 COW로 최적화를 하는데, mutating이 없으면 언제 실제 복사가 이루어질지 알 수 없다. mutating 키워드를 통해서 이 메서드가 호출된다면 실제 복사를 해야한다고 알려준다.

[Methods - The Swift Programming Language (Swift 5.6)](https://docs.swift.org/swift-book/LanguageGuide/Methods.html)
<br />
<br />

## 내부 프로퍼티가 var로 선언된 struct를 let 인스턴스로 선언했습니다. 이 경우 외부에서 mutating키워드를 가진 메서드를 사용해서 프로퍼티를 변경할 수 있을까요?

struct는 값타입이고, 값이 let으로 선언된 것이기 때문에 변경할 수 없습니다. 
<br />
<br />
<br />

---

# 고사리, 제리

## struct와 class와 enum의 차이를 설명하시오.

struct와 enum은 값타입이고 class는 참조타입으로 struct, enum은 상속이 불가능하며 class는 단일 상속이 가능합니다.

initialize의 차이도 있습니다. struct는 초기화하는 경우 customize initailizer, memberwise initializer 사용하고, class는 designated initializer를 이용하여 초기화합니다. 여기서 convenience init을 통해 매개변수 일부를 지정값을 넣어주어 최소한의 입력으로 초기화 가능하게 할수도 있습니다.
<br />
<br />

## struct가 무엇이고 어떻게 사용하는지 설명하시오.

프로퍼티를 저장하고 메서드를 제공하여 이를 캡슐화하는 타입입니다.

구조체는 struct라는 키워드를 이용하여 구조체명과 클로저 내부에 프로퍼티와 메서드를 정의합니다.

Swift에서는 구조체의 인스턴스를 생성하고 초기화하고자 할 때 멤버와이즈 이니셜라이저를 제공합니다.

구조체 내부에 선언된 프로퍼티의 이름이 멤버와이즈 이니셜라이즈의 매개변수로 지정되어 프로퍼티 값에 접근 할 수 있습니다.
<br />
<br />

## mutating 키워드에 대해 설명하시오.

struct, enum과 같은 값타입의 프로퍼티는 인스턴스 메서드내에서는 수정할 수 없는데요.

인스턴스 메서드에 mutating이라는 키워드를 붙여 프로퍼티를 수정할 수 있습니다.
<br />
<br />

## 값타입과 참조타입이 무엇을 의미하나요?

값타입은 변수를 할당하면 스택 영역에 값이 저장되고 변수를 복사하게 되면 복사본을 변경하더라도 원본에 영향을 주지 않습니다. 

참조타입은 주소값만 스택영역에 할당되고 데이터는 힙영역에 할당됩니다.

변수가 참조되거나 복사될 때 레퍼런스 카운트가 추가됩니다.
<br />
<br />

## ARC는 무엇일까요?

Swift에서 자동으로 메모리를 관리해주는 시스템으로 **클래스 인스턴스**가 생성되면 참조카운트를 추가하고 0이 되면 자동으로 메모리가 해제됩니다.

두개의 객체가 서로를 참조하고 있어 메모리가 해제되지 않는 순환참조가 발생하는 경우도 있습니다.
<br />
<br />

## 순환 참조가 무엇인가요?

이 경우 weak, unowmed를 이용하여 reference count를 증가하지 않게 방지할 수 있습니다.

[참고 URL](https://babbab2.tistory.com/27)
<br />
<br />

## enum의 장점은 무엇이 있을까요?

첫번째로 열거형이 타입으로 사용한다면 점을 이용해서 내가 선언한 case에서만 접근 할 수 있어 오타의 가능성이 줄어들고 가독성이 높아집니다.

또, 프로토콜을 채택할 수 있어서 Error, CustomStringConvertable 등 다양한 프로토콜을 활용하여 사용할 수 있습니다.

인스턴스를 만들지 않는 다는 측면에서 장점이 있습니다. 어떤 경우? → enum내 static 프로퍼티나 메소드를 선언하여 관리하는 경우, magic number
<br />
<br />

## 프로젝트를 진행하면서 class와 struct를 선택한 기준 혹은 이유를 공유해주세요

class는 고유한 데이터를 공유하거나 관리하는 경우에 사용합니다. 예를 들면 싱글턴, 파일 관리 매니저 타입을 정의하거나 API 네트워킹할 때 주로 사용했었습니다.

struct는  잘 변경되지 않는 데이터 타입을 정의할 때 주로 사용하고, 값을 저장하기보다 인스턴스 생성이 더 많은 경우 struct를 사용합니다.
<br />
<br />

## class는 왜 멤버와이즈 이니셜라이즈를 사용할 수 없나요?

클래스는 상속이 가능하기 때문에 서브클래스에서 초기화하는 경우가 발생할때 슈퍼클래스의 초기화를 수정해주어야하는 경우가 나타날수있습니다

이처럼 자동화된 맴버와이즈 이니셜라이즈를 사용하게 되면 상속의 가능성에 대한 오류를 직접 수정해주어야하기 때문에 클래스는 멤버와이즈 이니셜라이즈를 제공하지 않습니다
<br />
<br />

## 왜 값타입의 프로퍼티는 수정할 수 없는 것일까요?

인스턴스 메서드내에서 인스턴스 프로퍼티에 접근하는 경우 값타입의 인스턴스 자체를 복사하여 접근하게 되는데 복사본의 프로퍼티가 수정되어도 원본의 인스턴스는 변화하지 않기 때문에 수정할 수 없습니다.

따라서 mutating 키워드를 붙이게 되면 수정된 프로퍼티를 가진 인스턴스를 만들고 인스턴스 메서드가 종료되면 새로 만들어진 인스턴스가 기존의 인스턴스를 대체하게 됩니다.

[참고 url](https://sweetfood-dev.github.io/swift/method/)
<br />
<br />
