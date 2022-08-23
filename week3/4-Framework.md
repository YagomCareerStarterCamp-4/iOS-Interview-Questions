# ❔ 질문
1. Foundation Kit은 무엇이고 포함되어 있는 클래스들은 어떤 것이 있는지 설명하시오.  <br>
2. iOS 앱을 만들고, User Interface를 구성하는 데 필수적인 프레임워크 이름은 무엇인가? <br><br>

# ✏️ 작성자
- [숲재, 릴리, 호댕](#숲재-릴리-호댕)
- [고사리, 앨리, 토니](#고사리-앨리-토니)
<br />

---

# 숲재, 릴리, 호댕
## 1. Foundation Kit은 무엇이고 포함되어 있는 클래스들은 어떤 것이 있는지 설명하시오.
문자열 처리, 날짜 처리, 런 루프, GCD,  쓰레드, 네트워킹 등과 관련된 기능을 제공하게 된다. Foundation에 정의된 클래스, 프로토콜, 데이터 타입 등은 MacOS, iOS, tvOS, watchOS 전체에서 다양하게 사용된다. 즉, 스위프트로 개발을 하기 위해 필수적인 기능을 제공하는 프레임워크라고 볼 수 있다.

여기에는 NSArray, NSString, NSDate, NSData 등과 같은 Objective-C와 관련된 클래스들이 구현되어 있다.

- Fundamentals
    - 컬렉션, 원시타입, Date, 측정등에 대한 클래스를 제공
- App Support
    - 노티피케이션, GCD, Operation, Error, Bundle
- Files and Data Persistence
    - FileSystem, iCloud
- Networking
    - URL
- Low-Level Utilities
    - XPC, RunLoop, Thread
<br />
<br />

## 1-1. Foundation이 포함되어 있는 최상위 계층 프레임워크를 알고있나요?
![image](https://user-images.githubusercontent.com/65153742/186086362-31c9cc23-242b-4a28-8721-0cc1144adcc6.png)

- Cocoa touch framework입니다. 그중에서도 Foundation은 Core Servieces Layer에 속해있습니다.
https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/Cocoa.html
<br />
<br />

## 1-2. OS X Layer에 대해 알고계신가요?
- Cocoa touch Layer
- Media Layer
- Core Services Layer
- Core OS Layer <- low level

시스템의 하위 계층은 모든 소프트웨어가 의존하는 기본 서비스를 제공합니다. 후속 계층에는 아래 계층을 기반으로 구축(또는 보완)하는 보다 정교한 서비스와 기술이 포함됩니다.

[https://developer.apple.com/library/archive/documentation/MacOSX/Conceptual/OSX_Technology_Overview/About/About.html#//apple_ref/doc/uid/TP40001067-CH204-TPXREF101](https://developer.apple.com/library/archive/documentation/MacOSX/Conceptual/OSX_Technology_Overview/About/About.html#//apple_ref/doc/uid/TP40001067-CH204-TPXREF101)

<br />
<br />

## 1-3. 지금까지 사용해본 프레임워크는 어떤 것이 있나요?
(내용없음)

<br><br>

## 1-4. 라이브러리와 프레임워크의 차이는 무엇인가요?
- 프레임워크는 앱을 구현할 때 필수적으로 구현해야 하는 것들을 의미한다. 따라서 특정 기능을 구현하기 위해선 반드시 프레임워크를 import해야 한다. 라이브러리의 경우 앱을 구현할 때 필수적이진 않으며 사용하기 쉽게 특정 기능을 모아 구현해놓은 것으로 볼 수 있다. 라이브러리 또한 프레임워크를 기반으로 만들어졌다고 볼 수 있다.

- 프레임워크와 라이브러리의 차이를 제어 흐름의 주도권을 누가 가지느냐에 따라서도 볼 수 있다. 프레임워크는 사용자 코드를 호출할 수 있기 때문에, 앱의 제어 흐름을 프레임워크가 가진다. 반면, 라이브러리는 프로그래머의 코드가 라이브러리의 코드를 호출하며, 프로그래머가 흐름을 만들며 라이브러리를 호출한다.

- 예를 들어, UIKit의 TableView를 사용할 때, 저희는 UIKit이 미리 정해둔 tableViewDidSelect안에 사용자 코드를 등록해두고, 이벤트를 감지한 프레임워크가 해당 메서드를 호출한다. 이렇게 프레임워크가 사용자의 코드를 호출할 수 있다. 프로그래머는 프레임워크가 짜놓은 제어의 흐름에 탑승해서 원하는 기능을 사용한다. <br>

- 라이브러리는 재사용이 필요한 반복적인 기능을 모듈화 한것이다. 
- 프레임워크는 프로그램을 구동하는데 필요한 패키지의 묶음이다.

<br><br>

## 1-5. 어떤 라이브러리를 사용해봤나요? 라이브러리르 선택하는 기준이 있는지, 해당 라이브러리를 왜 사용했는지
(내용없음)

<br><br>

## 1-6. Dynamic Framework와 Static Framework의 차이점
- Xcode에서 Framework를 만들면 기본적으로 Dynamic Framework으로 만들어집니다. Dynamic Framework는 실행 파일 외부에 분리된 파일로 존재하게 되고, 동시에 여러 프레임워크 또는 어플리케이션 코드에서 참조할 수 있습니다. ( Static Linker를 통해 Dynamic Library Reference가 어플리케이션 코드에 들어가고, 런타임에 동적으로 프레임워크를 링크해줍니다.) 따라서 메모리를 효율적으로 사용할 수 있고, 런타임에 동적으로 연결되므로, 전체 빌드를 다시 하지 않아도 새로운 프레임워크 사용이 가능하고, 빌드시간이 Static Framework에 비해 빠릅니다. 하지만 파일 외부에 동적 라이브러리가 존재하기 때문에 파일이 손상되면 실행 파일이 더 이상 작동할 수 없다는 문제가 있고, 런타임 실행속도는 Static Framework보다 느립니다. 

- Static Framework의 경우 빌드시 Static Linker에 의해 앱의 실행파일에 프레임워크 코드가 복사되어 Heap에 상주합니다. 따라서  Static Framework의 경우 컴파일을 다시 하지 않는 이상 수정을 할 수 없으며, Static Framework를 여러 Framework에서 사용하게 되면 코드 중복이 발생하게 됩니다. Dynamic Framework에 비해 메모리를 많이 사용하며, 빌드시간이 깁니다. 하지만 런타임시 실행 속도가 빠르고, 안정적이라는 장점이 있습니다. <br><br>
#### **Dynamic Framework**
![image](https://user-images.githubusercontent.com/65153742/186086940-522ac7c9-887d-4e7b-8d04-70077b9560a6.png)
#### **Static Framework**
![image](https://user-images.githubusercontent.com/65153742/186086980-84345d30-aaa0-4315-883c-93321ab30c80.png)
#### 참고 링크
  - [https://minsone.github.io/ios/mac/ios-framework-part-1-static-framework-dynamic-framework](https://minsone.github.io/ios/mac/ios-framework-part-1-static-framework-dynamic-framework)
  - [https://medium.com/@StueyGK/static-libraries-vs-dynamic-libraries-af78f0b5f1e4](https://medium.com/@StueyGK/static-libraries-vs-dynamic-libraries-af78f0b5f1e4)

<br><br>

## 2. iOS 앱을 만들고, User Interface를 구성하는 데 필수적인 프레임워크 이름은 무엇인가?
- UIKit입니다.

<br><br>

## 2-1. UIKit의 상속 구조를 설명해보시오.
![image](https://user-images.githubusercontent.com/65153742/186087204-4889dcf9-49d7-4d7c-89da-9bf9d2ec559c.png)

<br><br>

## 2-2. UIKit은 어떤 스레드에서 사용되어야하나요?
- main thread입니다.
<br><br>

## 2-3. 왜 main thread에서 사용해야되나요?
- 뷰의 드로잉 사이클을 메인 스레드의 메인 런루
<br><br>

## 2-4. 왜 그렇게 설계되었을까요?
- UIKit이 만약 멀티 스레드에서 처리된다면, 스레드 안전성을 보장하기 위한 블락이 필요하게 될 것이고 이는 성능 저하를 야기하게 됩니다. 그리고 UIKit과 같은 거대한 프레임워크를 애초에 스레드세이프하게  디자인하기엔 현실적으로 어려웠기 때문에 단일 스레드만을 사용하는 전략을 취했다고 알고 있습니다.

- [https://velog.io/@rnfxl92/왜-UI는-Main-Queue에서-업데이트-해야-할까](https://velog.io/@rnfxl92/%EC%99%9C-UI%EB%8A%94-Main-Queue%EC%97%90%EC%84%9C-%EC%97%85%EB%8D%B0%EC%9D%B4%ED%8A%B8-%ED%95%B4%EC%95%BC-%ED%95%A0%EA%B9%8C)

<br />
<br />
<br />

---

# 고사리, 앨리, 토니
## 1. iOS 앱을 만들고, User Interface를 구성하는 데 필수적인 프레임워크 이름은 무엇인가?
- UIKit - iOS나 tvOS 앱용 User Interface를 구성하고 관리한다.
<br />
<br />

## 1-1. framework vs Library (프레임워크와 라이브러리의 차이)
- 프레임워크는 틀이고 그 안에서 재사용이 가능하도록 만들어진 도구들을 라이브러리라고 한다.


||framework|Library|
|------|---|---|
|정의|동적 라이브러리의 단점으로는 <br> 첫째, 외부 의존도가 발생하기 때문에 이식성이 어렵다. <br> 둘째, 성능저하로 인해 속도가 느려질수있다|개발을 하기 위해 필요한 것들을 미리 구현 해놓은 도구들의 집합.|
|장점|1. 기본적인 설계와 라이브러리를 제공하여 개발 속도를 향상시킨다. <br> 2. 오류의 폭을 좁힐 수 있다. <br> 3. 어느정도의 코드 품질을 보장한다. <br> 4. 코드의 재사용성이 높고 확장성이 좋다.|1. 코드를 재사용하기 쉽다. <br> 2. 코드의 내용을 숨겨 기술 유출을 방지할 수 있다. <br> 3. 이미 구현되어 있어 개발시간 단축. <br> 4. 컴파일 시간을 단축할 수 있다.(미리 컴파일 되어 있음)|
|단점|1. 프레임워크의 의존도가 늘어나 개발 능력이 저할 될 수 있다. <br> 2. 개발자의 자유도가 떨어진다.|동적 라이브러리의 단점으로는 첫째, 외부 의존도가 발생하기 때문에 이식성이 어렵다. 둘째, 성능저하로 인해 속도가 느려질수있다|

<br><br>

## 1-2. Dynamic Framework vs Static Framework
- 두 개의 차이는 **"컴파일된 코드를 참조하는 방식"**에 있다.

**Dynamic** → 런타임 상에서 Mach-O 파일과 연결(**참조**관계)

**Static** → 앱이 사용하는 프레임워크의 코드는 컴파일 시간동안 ‘Static Linker’에 의해 앱의 실행파일 안에 **복사**

<br><br>

## 1-3. 코코아 터치(Cocoa Touch)?
- 코코아 터치 프레임워크란 iOS 개발 환경을 구축하기 위한 최상위 프레임워크다. 파운데이션도 여기 포함되어 있음! `코코아(Cocoa)`라는 단어는, `NSObject`를 상속받는 모든 클래스, 모든 객체를 가리킬 때 사용하는 단어다.

<br><br>


## 2. Foundation Kit은 무엇이고 포함되어 있는 클래스들은 어떤 것이 있는지 설명하시오.
- 필수 데이터 타입, 컬렉션, 운영체제 서비스를 이용하여 앱의 기본 계층을 정의한다
<br />
<br />

## 2-1. UIKit을 사용할 때 필수요소?
- 어플리케이션 아이콘, LaunchScreen.storyboard
![image](https://user-images.githubusercontent.com/65153742/186089126-e0e36197-b9bb-4b4c-a6bd-bd40785c99be.png)

<br />
<br />

## 2-2. UIKit 앱의 구조?
- MVC(Model- View - Controller) 디자인 패턴을 기반으로 한다.
![image](https://user-images.githubusercontent.com/65153742/186089201-048ac910-e82a-4a6b-9746-33ebcef4aa1a.png)

<br><br>

## 2-3. SwiftUI?
- iOS 13이상 지원. SwiftUI는 UIKit를 기반으로 동작한다.

<br />
<br />
<br />
