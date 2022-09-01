# ❔ 질문
1. 앱이 시작할 때 main.c에 있는 UIApplicationMain 함수에 의해 생성되는 객체는 무엇인가? <br />
2. @main에 대해 설명하시오 <br />
3. 상태 변화에 따라 다른 동작을 처리하기 위한 AppDelegate 메서드들을 설명하시오 <br />
4. SceneDelegate에 대해 설명하시오 <br />

# ✏️ 작성자
- [호댕, 릴리, 토니](#호댕-릴리-토니)
- [예거](#예거)
<br />

---

# 호댕, 릴리, 토니
## 앱이 시작할 때 main.c에 있는 UIApplicationMain 함수에 의해 생성되는 객체는 무엇인가?
`UIApplicationMain` 함수는 코코아 터치 프레임워크에서 앱 라이프 사이클을 시작하는 함수로 `UIApplication` 객체의 인스턴스를 생성하고 해당 객체의 앱으로서 기능을 하기 위해 기반을 마련한다. 런루프를 포함한 메인 이벤트 루프를 설정하며 이벤트 처리를 시작한다.

파라미터

- `argc` : argv 내 인자의 수를 의미. 일반적으로 main에 해당하는 매개변수이다.
- `argv` : 인자의 리스트. 일반적으로 main에 해당하는 매개변수이다.
- `principalClassName` : UIApplication의 클래스 혹은 하위 클래스의 이름이다. 만약 nil을 할당하면 UIApplication으로 추정된다. => 앱의 라이프사이클 관리
- `delegateClassName` : application delegate 인스턴스를 생성할 때 클래스의 이름. 만약 UIApplication의 하위 클래스로 설정하면, 하위 클래스를 delegate로 설정할 수 있다. 어플리케이션의 main nib 파일에서 delegate 객체를 불러오면 nil을 지정하면 된다.

만약 반환 타입이 Int로 지정되어 있더라도 이 함수는 반환되지 않는다. 사용자가 홈버튼을 눌러 앱을 종료하면 애플리케이션이 백그라운드로 이동한다.

이 메서드는 주요한 클래스에서 애플리케이션 객체의 인스턴스를 생성하며, 지정된 클래스에서 delegate가 있는 경우 인스턴스를 생성하며 애플리케이션의 delegate로 설정한다. 애플리케이션의 런 루프를 포함한 main event loop를 설정하며 이벤트의 처리를 시작한다. 만약 Info.plist에서 불러올 main nib 파일을 지정한 경우 이 함수는 해당 nib 파일을 불러온다.

```
func UIApplicationMain(_ argc: Int32,
                     _ argv: UnsafeMutablePointer<UnsafeMutablePointer<CChar>?>,
                     _ principalClassName: String?,
                     _ delegateClassName: String?) -> Int32

```

> Creates the application object and the application delegate and sets up the event cycle.
> 

[https://developer.apple.com/documentation/uikit/1622933-uiapplicationmain](https://developer.apple.com/documentation/uikit/1622933-uiapplicationmain)
<br />
<br />

## @main에 대해 설명하시오
Swift 5.3부터 나온 Attribute로 이전에는 `@UIApplicationMain`을 사용했다. 처음 앱이 실행되게 되면 해당 어트리뷰트를 찾아 UIApplicationMain 메서드를 호출하고 앱이 실행되며, 만약 해당 어트리뷰트가 없다면 시작 지점을 찾지 못해서 컴파일 에러가 발생한다.

이렇게 어트리뷰트로 앱의 진입점을 표시하기 때문에 따로 main 파일은 존재하지 않는다.

모든 C 기반 언어들은 메인에서 실행되도록 구현이 되어 있다. `Swift`도 `Objective-C`를 계승한 만큼 Main이 존재한다.

```
func main(argc: Int32, argv: UnsafeMutablePointer<UnsafeMutablePointer<Int8>?>) -> Int32 {
    return UIApplicationMain(argc, argv, nil, "AppDelegate")
}

```

[https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#:~:text=with](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#:~:text=with) this attribute.-,main,-Apply this attribute
<br />
<br />

## iOS 어플리케이션의 실행 순서
[https://developer.apple.com/documentation/uikit/app_and_environment/responding_to_the_launch_of_your_app/about_the_app_launch_sequence](https://developer.apple.com/documentation/uikit/app_and_environment/responding_to_the_launch_of_your_app/about_the_app_launch_sequence)

1. @main을 통해 시작점을 찾아 main 함수 실행
2. main 함수에서 UIApplicationMain 함수 실행 (UIApplication 객체 생성, AppDelegate 생성)
3. Info.plist 파일을 읽어들여 해당 파일에 기록된 정보를 참고하여 데이터 로드 (main nib 파일 사용할 경우 이 때 로드됨)
4. Main nib 파일이 없거나 앱 델리게이트가 없는 경우 앱 델리게이트 객체를 생성하고 앱 객체와 연결
5. 런루프 만드는 등 실행에 필요한 준비
[https://jeongupark-study-house.tistory.com/164](https://jeongupark-study-house.tistory.com/164)
6. 준비 완료를 앞두고 앱 델리게이트에 해당 메서드로 전달.

```
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
    return true
}

```

![https://cdn.discordapp.com/attachments/973392117202321518/973865290330959912/2022-05-11_5.32.37.png](https://cdn.discordapp.com/attachments/973392117202321518/973865290330959912/2022-05-11_5.32.37.png)

![https://cdn.discordapp.com/attachments/973392117202321518/973865525824339978/2022-05-11_11.11.31.png](https://cdn.discordapp.com/attachments/973392117202321518/973865525824339978/2022-05-11_11.11.31.png)

UIApplicationDelegate에 정의된 메서드

반환 값은 무시된다.
<br />
<br />

## 상태 변화에 따라 다른 동작을 처리하기 위한 AppDelegate 메서드들을 설명하시오
[https://developer.apple.com/documentation/uikit/uiapplicationdelegate](https://developer.apple.com/documentation/uikit/uiapplicationdelegate)

`AppDelegate` 클래스의 경우 `UIApplicationDelegate` 프로토콜을 채택해야 함

App을 위한 공통의 행동을 관리한다.

- 앱의 중앙 데이터 구조 초기화
- 앱의 scene을 구성 (X)
- 메모리 경고나 다운로드 완료 알림 등 앱 외부에서 발생하는 알림에 응답
- 앱 자체를 대상으로 하는 이벤트에 반응
- Apple Push Notification 같은 앱의 launch time에서 필요한 서비스 등록

apple은 앱딜리게이트에 기본적으로 중요하다고 여기는 3개의 메서드들을 구현해놓았는데,

1. `func application(: didFinishLaunchingWithOptions:) → Bool`
ios12이하 버전은 multiple window를 지원하지 않았어서 UIWindow객체에 대한 configuration도 진행했었음.
2. `func application(: configurationForConnecting:options: ) → UISceneConfiguration`
application이 새로운 scene/window를 제공하려고 할때 불리는 메서드. 최초 launch때 불리는 건 아님
3. `func application(_: didDiscardSceneSessions: )`
사용자가 scene을 버릴때 불리운다. (앱 스위쳐를 통해 앱을 종료할 경우 호출됨 - 안될 때도 있는데 왜 그러지...)

app life-cycle event와 관련된 메서드

- `application:willFinishLaunchingWithOptions`: 어플리케이션이 최초 실행될 때 호출되는 메소드
- `application:didFinishLaunchingWithOptions`: 어플리케이션이 실행된 직후 사용자의 화면에 보여지기 직전에 호출
- `applicationDidBecomeActive`: 어플리케이션이 Active 상태로 전환된 직후 호출
- `applicationWillResignActive`: 어플리케이션이 Inactive 상태로 전환되기 직전에 호출
- `applicationDidEnterBackground`: 어플리케이션이 백그라운드 상태로 전환된 직후 호출
- `applicationWillEnterForeground`: 어플리케이션이 Active 상태가 되기 직전에, 화면에 보여지기 직전의 시점에 호출.
- `applicationWillTerminate`: 어플리케이션이 종료되기 직전에 호출
<br />
<br />

## SceneDelegate에 대해 설명하시오
iOS 13 이후 multi window가 만들어지면서 새롭게 생긴 것. scene 내에서 발생하는 life-cycle에 응답하는데 사용됨. 앱의 UI 인스턴스 내 생명주기를 관리하는데 사용이 된다.

Appdelegate는 2가지 역할을 했는데, application에게 1. process level의 이벤트 발생을 알려주었고, 2. UI 상태변화를 알려주었음.

그러나 iOS 13으로 넘어오면서 1 process & multiple user interface를 지원하게 되었음. AppDelegate의 일부 역할을 SceneDelegate에게 넘겨주었고, AppDelegate는 새로운 역할을 하나 더맡게됨. 1. process level의 이벤트발생은 그대로 알려주고, 2. session life-cycle을 application에게 알려주게 됨. 기존의 UI 상태 변화를 알려주는 것은 SceneDelgate가 책임지게 되었고, AppDelegate는 새로운 Scene Session이 생성되거나 버려질때 알리는 역할을 새로 맡게 됨.
<br />
<br />

## Multi window와 multi tasking의 차이
- Process level의 이벤트 발생
[https://velog.io/@minni/iOS-Application-Life-Cycle](https://velog.io/@minni/iOS-Application-Life-Cycle)
<br />
<br />

## UIApplication 객체의 컨트롤러 역할은 어디에 구현해야 하는가
UIApplication 에서 앱의 이벤트에 대한 핸들링하는 UIApplicationDelegate 라는 프로토콜이 있습니다. AppDelegate 는 이 프로토콜을 채택하여, UIApplication 의 컨트롤러 역할을 수행합니다.
<br />
<br />

## SceneDelegate나 AppDelegate 사용한 경험이 있나요? 그 때 실행 매커니즘을 소개해주실 수 있나요?
UIWindow 객체를 생성하고 rootViewController를 지정한다.

[https://developer.apple.com/documentation/uikit/app_and_environment/managing_your_app_s_life_cycle](https://developer.apple.com/documentation/uikit/app_and_environment/managing_your_app_s_life_cycle)

[https://blog.naver.com/PostView.naver?blogId=soojin_2604&logNo=222423840595&parentCategoryNo=&categoryNo=&viewDate=&isShowPopularPosts=false&from=postView](https://blog.naver.com/PostView.naver?blogId=soojin_2604&logNo=222423840595&parentCategoryNo=&categoryNo=&viewDate=&isShowPopularPosts=false&from=postView)
<br />
<br />
<br />

---

# 예거
## @Main 에 대해서 설명해주세요.
- main 키워드는 **프로그램의 Entry-point**, 즉 시작점을 의미합니다.
    - @main 키워드는 Swift 5.3 에서 나왔고, 그 전에는 @UIApplicationMain 어트리뷰트를 사용했다.
- 구조체, 클래스, 열거형 같은 커스텀 타입의 앞에 붙여줄 수 있는 어트리뷰트 이구요.
프로그램 흐름의 the top-level entry point 라는 걸 지정해줄 수 있습니다.
- 해당 타입은 파라미터도, 리턴값도 없는(Void) 함수를 가지고 있어야 합니다.

    ![@main 어트리뷰트를 달 수 있는 타입의 자격.png](https://user-images.githubusercontent.com/83864058/187879264-0c43127e-76ff-4000-b4f9-6d3d6330a99d.png)
    
- 실행 가능한 스위프트 코드는 **최대 1개의 top-level entry point** 를 가져야 합니다.
- Xcode 의 Command Line Tool 에서는 **main.swift** 라는 파일이 있고, 프로그램은 이 파일의 첫 줄부터 순서대로 실행됩니다.
- **UIKit** 프레임워크에서는 AppDelegate 클래스가 @main 어트리뷰트를 달고 있습니다.
- **SwiftUI** 프레임워크에서는 App 프로토콜을 채택한 구조체가 @main 어트리뷰트를 달고 있습니다.
    
    ![Untitled](https://user-images.githubusercontent.com/83864058/187879412-ab122787-3978-437a-8255-a6f21d8c888f.png)
    
    - [공식 문서 - Attributes - main](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html)
    - [제드 블로그](https://zeddios.tistory.com/69)
<br />
<br />

## 최상위 레벨 오류를 보신 적 있나요? 왜 발생하는 걸까요? Top-Level Code 란 무엇인가요?
- 탑 레벨 코드란? → **스위프트의 소스 파일**입니다. statements, declarations, expressions 들이 여러 개 존재할 수도, 하나도 없을 수도 있습니다.

> **The top-level code in a Swift source file** consists of zero or more statements, declarations, and expressions.
> 
- **탑 레벨 코드에는 2가지 종류**가 있습니다.
하나는 top-level declarations(선언문), 하나는 실행 가능한 top-level 코드입니다.
    - main 함수 내부만이 실행 가능한 top-level 코드라고 볼 수 있다.
- 탑레벨 선언문은 오직 declarations 로만 구성되어 있습니다. 모든 스위프트 소스 파일에서 가능합니다.
- 실행 가능한 탑레벨 코드는 선언문 뿐 아니라, **statements 와 expressions 로 구성**되어 있습니다. 오직 프로그램의 top-level 엔트리 포인트에서만 허용됩니다.
- 정리하자면, 우리가 @main 어트리뷰트가 달리지 않은 타입에서, 캡슐화되지 않은 statements 또는 expressions 를 작성하면 → **“Statements/Expressions are not allowed at the top level”** 에러가 출력되는 이유는 → **그 장소도 top-level 은 맞지만, 오직 선언(declarations)만 가능한 곳이라서** 그런 것입니다.
    - [공식 문서 - Declarations - Top-Level Code](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#ID352)
    - [제드 블로그](https://zeddios.tistory.com/69)
<br />
<br />

## expression 하고 statement 차이를 아시나요? 한국어로는 뭐라 부를까요?
- **expressions** 은 **“표현/표현식"** 이라고 칭하고, 하나의 값으로 수렴하는 코드 조각을 의미합니다.
    - 1 + 2, “문자열", true && false, **리턴 타입이 Void 인 메서드 호출 (print 등) 등이 표현식**의 예시입니다.

    ![Untitled](https://user-images.githubusercontent.com/83864058/187879681-d2a3886a-75b4-4a14-b82d-38c0a432f8c3.png)
    
- **statement** 는 **“구문/구문식"** 이라고 칭하고, 실행 가능한(executable) 최소의 독립적 코드 조각을 의미합니다.
    - if, switch, for, guard문 등이 예시입니다.
    - [출처 블로그](https://mindock.github.io/knowledge/expression-statement/)
<br />
<br />

## 앱이 시작할 때 main.c 에 있는 UIApplicationMain 함수에 의해 생성되는 객체는 무엇일까요?
- 결론 → **UIApplication 싱글턴 객체(shared)**가 생성됩니다.
- 모든 iOS 앱은 단 하나의 UIApplication 인스턴스를 갖습니다. (아주 드물게 서브 클래스)
- 앱이 실행될 때, 시스템은 UIApplicationMain 함수를 호출하고, **이 함수는 UIApplication 클래스의 싱글턴인 shared 를 생성**합니다.

> Every iOS app has exactly one instance of UIApplication(or, very rarely, a subclass of UIApplication).
When an app launches, the system calls the [UIApplicationMain(_:_:_:_:)](https://developer.apple.com/documentation/uikit/1622933-uiapplicationmain) function.
Among its other tasks, this function creates **a singleton UIApplication object** that you access using [shared](https://developer.apple.com/documentation/uikit/uiapplication/1622975-shared).
> 
- [iOS Application Lifecycle - 무지성 블로그](https://limjs-dev.tistory.com/58)
- [공식 문서 - UIApplication](https://developer.apple.com/documentation/uikit/uiapplication)
- [공식 문서 - UIApplicationMain(_:_:_:_:)](https://developer.apple.com/documentation/uikit/1622933-uiapplicationmain)
- [[iOS] 앱의 생명주기(App Life Cycle)와 앱의 구조(App Structure)](https://jinshine.github.io/2018/05/28/iOS/%EC%95%B1%EC%9D%98%20%EC%83%9D%EB%AA%85%EC%A3%BC%EA%B8%B0(App%20Life%20Cycle)%EC%99%80%20%EC%95%B1%EC%9D%98%20%EA%B5%AC%EC%A1%B0(App%20Structure)/)
<br />
<br />

## UIApplication 싱글턴 객체의 역할은 무엇인가요?
- 입력되는 **유저 이벤트의 초기 라우팅을 처리**합니다.
- 컨트롤 객체(UIControl 클래스의 인스턴스)에 의해 전달된 작업 메시지를 적절한 대상 객체에 전달합니다.
- 애플리케이션 객체는 open windows (UIWindow 객체) 목록을 유지하며 이를 통해 앱의 UIView 객체를 검색 할 수 있습니다.

> Your app’s application object handles the initial routing of incoming user events.

It dispatches action messages forwarded to it by control objects (instances of the UIControl class) to appropriate target objects.

The application object maintains a list of open windows (UIWindow objects), which it can use to retrieve any of the app’s UIView objects.
> 
- [공식 문서 - UIApplication](https://developer.apple.com/documentation/uikit/uiapplication)
- [출처](https://github.com/jwonyLee/TIL/blob/master/iOS/Interview/UIApplicationMain.md)
<br />
<br />

## SceneDelegate, AppDelegate 의 차이점은 무엇이고 왜 SceneDelegate 가 만들어졌을까요?
- iOS 12 까지는 AppDelegate 클래스가 App Life Cycle 및 UI Life Cycle 을 모두 관리했습니다.
- 그러다 **iOS 13 버전부터, AppDelegate 의 책임이 AppDelegate 과 SceneDelegate 으로 분리**됐습니다.
- 책임이 분리된 이유는, **iPadOS 에 도입된 Multiple Window 기능 때문**입니다.
- **AppDelegate** → 앱의 생명주기 및 설정 담당
    - App Life Cycle
    - Session Life Cycle
- **SceneDelegate** → 화면에 보이는 내용(Windows or Scenes)을 처리하고 앱이 표시되는 방식을 관리
    - UI Life Cycle

💡 [https://github.com/jwonyLee/TIL/blob/master/iOS/Interview/Scene-Delegate.md](https://github.com/jwonyLee/TIL/blob/master/iOS/Interview/Scene-Delegate.md)

[https://huniroom.tistory.com/87](https://huniroom.tistory.com/87)
<br />
<br />

## 아이패드에서 Multitasking 하고 Multiple Windows 차이는 뭘까요?
- **멀티태스킹** → 한 화면에, 동시에 여러 종류의 앱을 실행할 수 있는 기능을 말합니다.
    - 예를 들면, 아이패드에서 한 쪽엔 카톡을 놓고, 한 쪽엔 유튜브를 켜두는 기능 처럼요.
- **멀티플 윈도우** → 한 화면에, 하나의 앱을 2개의 분할된 window 로 실행할 수 있는 기능입니다.
    - 예를 들면, 캘린더를 2개 띄우는데, 하나는 연도를 기준으로, 하나는 월 기준으로 보는 식으루요.

💡 [HIG - Multitasking and Multiple Windows](https://developer.apple.com/design/human-interface-guidelines/ios/system-capabilities/multitasking/)
<br />
<br />

## SceneDelegate 에 대해 설명해주세요.
- AppDelegate 의 UIWindow 와 관련된 것이 SceneDelegate 의 **UIScene** 으로 넘어왔습니다.
- **화면에 보이는 내용을 처리하고, 앱이 표시되는 방식을 관리하는 클래스**입니다.
- 여러 메서드가 있는데요, 재밌는 건 처음 프로젝트를 만들면 SceneDelegate 클래스에 이미 들어와있는 메서드들이 전부 optional methods 들이라는 것입니다. 즉, 구현을 완성하지 않아도 상관없습니다.
- 가장 많이 다뤄본 건, 역시 **scene 메서드** 인데요.
새로운 UIWindow 를 만들고, rootViewController 를 설정해서 이 창을 표시할 Key window 로 만듭니다.
- 그 외에도 sceneDidDisconnect, sceneDidBecomeActive, sceneWillResignActive, sceneWillEnterForeground, sceneDidEnterBackground 메서드가 있습니다. 전부 옵셔널이라, 반드시 구현하지 않아도 됩니다.

💡 [공식 문서 - UISceneDelegate 프로토콜](https://developer.apple.com/documentation/uikit/uiscenedelegate)

[도미닉 - AppDelegate 에 있는 메소스들 정리](https://appleceo.github.io/2019/09/08/MethodInAppDelegate/)

<br />
<br />

## AppDelegate 클래스의 첫 번째 메서드인 application(_:didFinishLaunchingWithOptions:) 에 대해 설명해주세요. 사용해본 경험이 있나요?
- 가장 상위에 있고 사용해본 경험이 있는 것은, ****[application(_:didFinishLaunchingWithOptions:)](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1622921-application)**** 메서드입니다.
(자매품으로 did → will 도 있음. 근데 did 만 기본으로 적혀있음. 좀 더 권장되는 듯.)
- 첫 번째 파라미터는 **앱의 싱글턴 객체**입니다. (UIApplication.shared 인 듯)
- 두 번째 파리미터는 **launchOptions 는 딕셔너리**입니다.
앱이 실행된 이유(reason)를 집어넣을 수 있습니다. 만약 유저가 그냥 앱을 눌러서 실행한 거라면, 딕셔너리는 비어있을(empty) 수도 있습니다.
- 앱을 처음 실행할 때, launch process 가 거의 끝나가고(almost done) 앱이 실행될 준비가 됐을 때(app is almost ready to run) 이 메서드가 호출됩니다.
- 제가 진행했던 프로젝트에서는, 보통 이 메서드에 **Firebase, Dropbox 와 같은 원격 서버와 연동해주는 코드**를 넣었습니다.

💡 [https://velog.io/@minni/iOS-Application-Life-Cycle#appdelegate-와-scenedelegate](https://velog.io/@minni/iOS-Application-Life-Cycle#appdelegate-%EC%99%80-scenedelegate)
<br />
<br />

## 상태 변화에 따라, 다른 동작을 처리하기 위한 AppDelegate 메서드들을 설명해주세요.
- 위에서 얘기한 메서드 말고, AppDelegate 에는 2개의 메서드가 더 있죠.
Xcode 에서 기본적으로 **MARK: UISceneSession Lifecycle** 라는 주석을 붙여놓기도 했습니다.
- 먼저, [application(_:configurationForConnecting:options:)](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/3197905-application) 메서드에는 미리 주석이 적혀있는데요.
    - **새로운 SceneSession 이 만들어질 때 이 메서드가 호출됩니다.**
    - 그러니까, 새로운 SceneSession 을 만들 때, 구성(configuration) 옵션을 바꾸고 싶다면, 이 메서드를 이용하세요.
    
    > // Called when a new scene session is being created.
    // Use this method to select a configuration to create the new scene with.
    > 
    - 그런 옵션은 [UIScene.ConnectionOptions](https://developer.apple.com/documentation/uikit/uiscene/connectionoptions) 클래스에 타입 프로퍼티(static let)들로 구현되어 있습니다.
- 다음으로, [application(_:didDiscardSceneSessions:)](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/3197906-application) 메서드에도 주석이 있습니다.
    - **유저가 SceneSession 을 영구적으로 버리면(discards) 이 메서드가 호출됩니다.**
    - 이 메서드는 앱의 데이터 구조가 scene 과 관련된 리소스를 release 해야할 때 사용하세요.
    - UIKit 은 오직 scenes 를 영구적으로 dismissing 할 때만, 이 메서드를 호출합니다.
        - 시스템이 메모리를 청소하기 위해, scene 과의 연결을 해제(disconnects)하는 경우엔 이 메서드가 호출되지 않습니다.
        - 메모리 반환은 scene 객체를 삭제하지만, SceneSessions 는 보존(preserves)합니다.
    
    > // Called when the user discards a scene session.
    // If any sessions were discarded while the application was not running, this will be called shortly after application:didFinishLaunchingWithOptions.
    // Use this method to release any resources that were specific to the discarded scenes, as they will not return.
    >
<br />
<br />

## 멀티플 윈도우에서는 앱의 라이프 사이클이 2개 돌아갈까요?
- 만약 앱의 라이프 사이클을 2개 돌릴 수 있다면, iOS 13 에서 굳이 AppDelegate 의 역할을 SceneDelegate 에 나눠줄 필요가 없지 않았을까요?
- (애플사이다 말로는) 앱의 라이프 사이클은 하나고, 화면의 라이프 사이클이 2개 돌아간다고 함.

💡 [공식 문서 - Managing Your App's Life Cycle](https://developer.apple.com/documentation/uikit/app_and_environment/managing_your_app_s_life_cycle)
<br />
<br />
<br />
