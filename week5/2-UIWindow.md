# ❔ 질문
UIWindow 객체의 역할은 무엇인가?<br />

# ✏️ 작성자
- [허황, 예하](#허황-예하)
- [아샌, 줄라이](#아샌-줄라이)
<br />

---

# 허황, 예하
## UIWindow 객체의 역할은 무엇인가?

- 앱의 UI의 배경인 UIView 하위 클래스입니다.
- 뷰 컨트롤러와 함께 이벤트를 처리하고, 앱을 작동하기 위해 기본적인 다른 많은 작업을 수행하기 위해 뷰 컨트롤러와 함께 작동합니다.
- window를 사용할 수 있는 경우는 아래와 같습니다.
    - 앱 컨텐츠를 표시하기 위해 메인 윈도우를 제공할 때, 추가적인 컨텐츠를 표시하기 위해 추가 윈도우가 필요할 때 윈도우를 사용합니다.
- 다음과 같은 경우에도 사용합니다.
    - (다른 window와 관련이 있는 window의 가시성에 영향을 주는) window의 z축 수준을 설정합니다.
    - window를 표시하고, 키보드 이벤트의 대상으로 지정합니다.
    - 좌표값을 window의 좌표계로 변환하거나, window의 좌표계에서 변환합니다.
    - window의 루트 뷰 컨트롤러를 변경합니다.
    - window가 표시된 화면을 변경합니다.

[https://developer.apple.com/documentation/uikit/uiwindow](https://developer.apple.com/documentation/uikit/uiwindow)
<br />
<br />
## UIWindow를 직접 생성해본적이 있는가? 있다면 언제 사용했고 사용했을 때 주의할점은 무엇인가?

- 스토리보드를 사용하지 않고 코드로만 뷰를 구성할 때 AppDelegate, SecncDelegate에서 루트 뷰컨트롤러를 설정할 때 직접 생성한다.
- iOS 13 미만, AppDelegate만 사용할 경우
    - window가 따로 초기화되지 않기 때문에 AppDelegate의 didFinishWithLaunchingOptions메서드에서 UIWindow()를 생성해야한다.
- iOS 13 이상, SceneDelegate를 사용할 경우
    - willConnectTo 메서드에서 생성해야 한다.
<br />
<br />

## UIView는 무엇인가요?

- 화면의 직사각형 영역에 대한 컨텐츠를 관리하는 클래스입니다.
- 모든 view들의 루트 클래스입니다.
- UIView 클래스를 통해 view들의 공통 동작을 정의합니다.
<br />
<br />

## UIView를 통해 할 수 있는 일(책임)에 대해 설명해주세요.

- Drawing and animation
    - UIKit이나 Core Graphics를 사용해서 직사각형 영역에 콘텐츠를 그립니다.
- Layout and subview management
    - 뷰에는 0개 이상의 subview가 포함될 수 있습니다.
    - subview 에 대한 사이즈와 위치를 조정할 수 있습니다.
    - 오토 레이아웃을 사용하여 뷰 계층 구조의 변경에 대한 응답으로 사이즈 조정, 위치 변경합니다.
- Event handling
    - 뷰는 UIResponder의 subclass며, 터치 및 기타 유형의 이벤트에 응답할 수 있습니다.
    - 뷰는 일반적인 제스처를 처리하기 위해 gesture recognizer를 설치할 수 있습니다.

[https://developer.apple.com/documentation/uikit/uiview](https://developer.apple.com/documentation/uikit/uiview)
<br />
<br />

### UIImageView 내부에 UILabel을 넣어야한다면 어떻게 구현할 것인가요? 또, UIImageView가 사용자의 인터렉션을 받으려면 어떻게 해야하는가?

(custom view)

- Interface Builder: UIView, UIStackView를 최상위에 두고 ImageView, Label을 UIView, UIStackView 내부에 자식뷰로 등록합니다. UIView에 gesture recognizer를 달아서 인터렉션을 처리하면 됩니다.
- code: UIImageView 내부에 addsubview()로 UILabel을 추가할 수 있습니다. UIImageView에 gesture recognizer를 달아서 인터렉션을 처리하면 됩니다.
<br />
<br />

## UIResponder은 무엇인가요?

- 이벤트를 응답하고 처리하기 위한 클래스입니다.
- UIResponder의 인스턴스 (Responder object)는 UIKit 앱의 이벤트 핸들링의 근간(backbone)이 됩니다.
- UIApplication object, UIViewController object, UIWindow를 포함한 모든 UIView object를 포함한 많은 주요 개체도 responder입니다.
- 이벤트가 발생하면 UIKit은 처리를 위해 앱의 responder object에게 이벤트를 보냅니다.

[https://developer.apple.com/documentation/uikit/uiresponder](https://developer.apple.com/documentation/uikit/uiresponder)
<br />
<br />

## ViewController는 View와 Controller 중 어디에 속한다고 생각하시나요?

- MVVM패턴에서는 View라고 생각합니다. 핵심 로직은 ViewModel에서 담당하고 있기 때문에 ViewController은 사용자의 이벤트를 받아 ViewModel에게 전달하고 ViewModel은 로직을 수행한 후에 ViweController(View)에 데이터를 전달하기 때문에 View의 역할만 수행하면 됩니다.
- Cocoa MVC패턴에서 Model은 Controller에게 상태의 변화가 있음을 알립니다. (전통적인 MVC패턴에서는 Observer(로 등록한 View)에게 알립니다.) View는 사용자의 이벤트를 받아서, Controller에게 전달합니다. Controller는 Model, View를 업데이트하도록 시킬 수 있습니다.
- 위 두 경우 모두 역할 분리가 정확하게 이루어졌을 때 View와 Controller를 나눌 수 있습니다. ViewController는 View와 Controller이 밀접하게 관련되어 있기 때문에 면접자에 따라 답변이 달라 질 수 있을 것 같습니다.
<br />
<br />

## 추가) App의 Window에서 컨텐츠를 관리하는 방법

![https://i.imgur.com/Lvatz9i.png](https://i.imgur.com/Lvatz9i.png)
    
- 앱 UI의 각 장면에는 window object와 하나 이상의 view object가 포함되어 있습니다.
- window object는 나머지 UI의 보이지 않는 컨테이너 역할, 뷰의 최상위 컨테이너 역할을 하며, 그들에게 이벤트를 라우팅합니다.
- view object는 사용자가 화면에서 보는 실제 내용입니다.
- window object는 오래 지속되며, 장면의 전체 UI를 없엘 때만 사라집니다.
- 반대로 window의 view는 새로운 컨텐츠나 정보를 업데이트하고 싶을 때마다 자주 변경하게 됩니다.
- UIKit은 뷰를 효과적으로 관리하고, window에서 쉽게 추가/삭제할 수 있도록 뷰 컨트롤러를 제공합니다. 뷰 컨트롤러는 앱의 단일 뷰 집합을 관리하고, 해당 뷰를 최신 상태로 유지합니다.
- 모든 window는 window의 초기 뷰 집합을 지정하는데 사용하는 루트 뷰 컨트롤러가 있습니다. 뷰 집합을 변경하려면 UIKit에 뷰 컨트롤러를 표시하거나 해제하라고 말해야 합니다.
- UIKit은 하나의 뷰 집합에서 다른 뷰 집합으로 전환하는 것을 처리하고, 뷰 컨트롤러 Object를 통해 앱의 모든 인터페이스를 관리합니다. 결과적으로 뷰 컨트롤러는 UI를 구현하는 데 중요한 역할을 합니다.

[https://developer.apple.com/documentation/uikit/view_controllers/managing_content_in_your_app_s_windows](https://developer.apple.com/documentation/uikit/view_controllers/managing_content_in_your_app_s_windows)
(내용)
<br />
<br />
<br />

---

# 아샌, 줄라이
## UIWindow 객체의 역할은 무엇인가?
![Untitled](https://user-images.githubusercontent.com/81469717/188273276-ddf163f2-a674-4802-9ab8-11e846a18720.png)


앱의 UI에 대한 배경 및 view에 이벤트를 전달하는 개체

![Untitled2](https://user-images.githubusercontent.com/81469717/188273284-84bce29b-145d-44da-a6ef-917d99c1028e.png)


- VC와 함께 이벤트를 처리하고 앱 작동에 기본적인 작업들을 수행함
- window에는 고유한 시각적 모양이 없음
- rootVC에서 관리하는 하나 이상의 view를 호스팅 해야함

- 키보드 이벤트의 대상으로 만듦
- rootVC변경
- 표시되는 화면을 변경
<br />
<br />
<br />
