# ❔ 질문
자신만의 Custom View를 만들려면 어떻게 해야하는지 설명하시오.<br />
ViewController의 생명주기를 설명하시오.<br />

# ✏️ 작성자
- [예거, 앨리, 나무](#예거-앨리-나무)
- [숲재, 릴리, 예하](#숲재-릴리-예하)
<br />

---

# 예거, 앨리, 나무
## 자신만의 Custom View를 만들려면 어떻게 해야하는지 설명하시오.

***- programmatically***

init(frame:) 함수가 실행 된다.

VC에서 불러올 때는 커스텀뷰의 인스턴스를 만들고 addSubView로 추가해줘야 한다.

***- xib (Xcode Interface Builder)***

Nib 파일을 XML형식으로 변환한 파일이다.

같은 이름으로 xib, swift 파일을 만들고 스토리보드에서 만들듯이 뷰를 디자인하고 class를 지정해주면 된다.

<img src = "https://user-images.githubusercontent.com/81469717/188274068-3855a75c-1319-4512-93fb-19ac97b77861.png" width = 500>

xib 파일 생성

`.xib` 파일을 열고 Placeholders 아래에 있는 `File's Owner`를 클릭하고 Xcode의 오른쪽 탭을 보면Attributes Inspector 탭의 File's Owner에 Class를 지정하는 곳이 있는데 여기에 CustomView를 입력해주면 화면과 소스가 연결된 것이다.

`CustomView`는 우리가 만든 View이므로 당연히 초기화도 직접 해줘야한다. 

초기화하기 전에 생성했던 View를 IBOutlet으로 추가해준다. (?)

```swift
class CustomView: UIView {
	
	override init(frame: CGRect) {
		super.init(frame: frame)
		loadView() 
	}

	required init?(coder: NSCoder) {
		super.init(coder: coder)
		loadView() 
	}

	private func loadView() {
		let view = Bundle.main.loadNibNamed("CustomView", 
																	owner: self, 
																	options: nil)?.first as! UIView 
		view.frame = bounds 
		addSubview(view) 
	} 
}
```
<br />
<br />

## ViewController의 생명주기를 설명하시오.

### **1. init**

ViewController 객체가 생성됩니다.

### **2. loadView**

View를 메모리에 로드합니다.

### **3. viewDidLoad**

View의 Controller가 메모리에 로드된 뒤 호출된다.

보통 화면이 표시되기 전 초기화하는 작업을 추가한다.

1회 호출되며 메모리 경고로 뷰가 사라지지 않는 이상 다시 호출되지 않음.

### **4. viewWillAppear / viewDidAppear**

**viewWillAppear** : View가 표시되기 직전에 호출.

View1 > View2 > View1 형태로 View가 전환된다면 viewDidLoad는 1회만 호출되지만, viewWillAppear은 View1이 2번 나타나므로 2회 호출된다. (화면이 새로 올라올 때마다 수행)

그때마다 수행해야 할 작업을 담당하기에 유용하다.

**viewDidAppear** : View가 표시된 후 호출.

View에 적용할 애니메이션 작업을 추가하는 것이 좋다.

뷰를 보여줄 때 필요한 추가적인 작업을 할때 유용하다.

### **5. viewWillDisappear / viewDidDisappear**

**viewWillDisappear :** View가 사라지기 직전 호출.

뷰를 생성하고나서 했던 행동들을 되돌리는 작업할 때,

작성 또는 선택된 정보들을 삭제되기 전에 저장해두는 작업할 때 사용한다.

**viewDidDisappear :** View가 사라진 직후 호출.

뷰를 숨기는 것과 관련된 추가적인 작업을 할때 사용한다.

View1 > View2 > View1 형태로 View가 전환된다면 다음 순서로 진행된다.

View2가 표시할 준비가 되면 이후 View1이 사라진다.

### **6. viewDidUnload**

View가 메모리에서 해제된 뒤 호출됩니다.

<img src = "https://user-images.githubusercontent.com/81469717/188274766-dc87bcbd-0888-4592-8b1b-04c98caf2a7f.png" width = 600>

<br />
<br />

## 코드와 xib의 장단점?

### code

장점
- 동적인 레이아웃으로 개발자가 마음대로 제어할 수 있다.
- 충돌시 해결이 쉽다.
- 재사용성이 높다.

단점
- 개발기간이 xib에 비해 오래걸린다.
- 에러를 찾기가 어렵다.
- 오래된 코드 리팩토링이 어렵다.

### xib

장점
- 눈으로 보면서 확인할 수 있다.
- VC에 추가적인 코드작업이 필요없다.
- 레이아웃 관련 에러를 찾기 쉽다.
- 재사용성이 높다.

단점
- 충돌시 골치아프다.
- 코드로 작성한 UI에 비해 오버헤드가 크다.
<br />
<br />

## 두 개의 UIVIewController간 화면 전환이 이루어질 때의 시나리오를 UIVIewController life cycle과 관련해서 설명해주세요.

1. UIVIewController A를 띄운다.
    - loadView : viewController의 최상위 view를 로드합니다. 이 메소드를 override하면 기본 뷰를 교체할 수 있습니다. 만약 전체 뷰가 웹뷰여야 한다면 이 메소드에서 교체할 수 있겠죠!?
    - viewDidLoad
    - viewWillAppear
    - viewDidAppear


2. UIVIewController B로 화면을 이동한다.
    - A.viewWillDisappear : 사라질꺼야
    - B.viewDidLoad : 뷰 로드했어
    - B.viewWillAppear : 뷰 보일 준비 됐어
    - A.viewdidDisappear : 뷰 사라졌어
    - B.viewDidAppear : 뷰 떴어!

**사라질거야 > 로드했어 > 보일거야 > 사라졌어 > 보였다!**
<br />
<br />

## 추가 정보) full screen modal, modal 불리는 메서드가 다르다

full → mainVC가 완전히 가려져서 모달뷰가 띄워지면 disappear() 메서드가 불린다.

modal → mainVC가 완전히 가려진게 아니라서 main에서 viewWillDisappear()과 같은 함수는 호출되지 않는다.

<img src = "https://user-images.githubusercontent.com/81469717/188274699-5411c856-9b96-40d2-bbae-fd2facbc3cac.png" width = 600>


모달뷰를 닫으면, ModalViewController는 완전히 사라지게 된다. 그러니깐 소멸자까지 호출된다.
<img src = "https://user-images.githubusercontent.com/81469717/188274693-113a092c-64d5-42f3-a7f5-d834a6e90613.png" width = 600>

<br />
<br />
<br />

---

# 숲재, 릴리, 예하

## 자신만의 Custom View를 만들려면 어떻게 해야하는지 설명하시오.

- UIView를 상속받는 클래스를 만들어야 합니다. 만든 클래스를 xib 파일과 연결하거나 코드로 구성한 UI 요소와 연결하는 방법이 있습니다.

### Xib 사용하는 방법
- Nib파일을 XML형식으로 변환한 파일
- Nib파일은 바이너리 형식의 파일이며, Xib파일을 빌드하면 Nib파일로 컴파일 된다.

장점
  - 눈으로 쉽게 보면서 레이아웃 작업이 가능하다.
  - 레이아웃 에러를 찾기 쉽다.
  - 뷰컨에 추가적인 코드작업이 필요없다.
  - 재사용성이 좋다.
  
단점
  - 충돌시에 해결이 복잡함
  - 코드로 작성한 UI에 비해서 오버헤드가 크다. (필요 시 메모리로드 -> 파싱과정)
        
### 코드로만 작성하는 방법
- 코드로만 UI를 작성한다.
- `UIView` 의 서브클래스를 생성하여, `draw` 메서드를 오버라이드 하여 원하는 그림을 구현하면 됩니다.

장점
  - 오버헤드가 적다
  - 충돌시 해결이 비교적 쉽다.
  - 개발자 마음대로 제어가 가능하다.(동적인 레이아웃)
  - 재사용성이 좋다.
단점
  - 개발시간이 Xib에 비해 시간이 더 소요된다.
  - 레이아웃 에러를 찾기 어렵다.
<br />
<br />

## nib 파일이 무엇인가요?

- Next Interface Builder의 약자로, Interface Builder 파일입니다.
- 바이너리 형식의 파일입니다
- nib 파일을 통해 iOS나 Mac 앱의 UI를 저장할 수 있습니다.
<br />
<br />

## Custom View를 만들 때 사용하는 이니셜라이저에 대해 설명해주세요.

- `init(frame:)`
    - 애플에서 구현하길 권장하는 메서드입니다. 이 메서드 대신 사용자 정의 초기화를 할수도 있습니다.
    - 사용자 정의 초기화를 하게 되면 구현 시작 시 `super` 를 호출해야 합니다.
[https://developer.apple.com/documentation/uikit/uiview/1622488-init](https://developer.apple.com/documentation/uikit/uiview/1622488-init)
        
![https://i.imgur.com/AS5Pr35.png](https://i.imgur.com/AS5Pr35.png)
        
- `init(coder:)`
    - storyboard나 nib파일로 뷰를 로드하고, 사용자 정의 초기화가 필요할 때 이 메서드를 구현합니다.
    
    [https://developer.apple.com/documentation/uikit/uiview](https://developer.apple.com/documentation/uikit/uiview)
<br />
<br /> 

## ViewController를 초기화하는 **init(nibName:bundle:) 메서드를 설명해주세요.**

- 지정된 번들의 nib 파일로 뷰 컨트롤러를 생성하는 메서드입니다.
    - 지정한 nib파일이 바로 로드되지는 않고, 뷰 컨트롤러의 뷰에 처음 액세스할 때 로드됩니다.
    - 만약 nib 파일이 로드된 후 추가적인 초기화를 수행하려면, `viewDidLoad` 메서드를 오버라이드하고, 그 곳에서 수행합니다.
- 스토리보드에서 뷰 컨트롤러를 인스턴스화할 때, iOS는 `init(coder:)` 메서드를 호출하여 새 뷰 컨트롤러를 초기화하고, nibName 속성을 스토리보드 내부에 저장된 nib파일로 설정합니다.
- 만약 nibName 파라미터에 nil을 주고, `loadView` 메서드를 오버라이드하지 않는다면, 뷰 컨트롤러는 nib 파일을 찾습니다.
- 스토리보드를 사용할 때, 뷰 컨트롤러 클래스를 직접 초기화하지 않는 대신 segue가 트리거될 때 자동으로 혹은 스토리보드의 `instantiateViewController(withIdentifier:)` 메서드를 통해서 코드로 인스턴스화됩니다.
[https://developer.apple.com/documentation/uikit/uiviewcontroller/1621359-init](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621359-init)
<br />
<br />

## ViewController의 생명주기를 설명하시오.

ViewManagement

<img src = "https://i.imgur.com/SJQxaCE.png](https://i.imgur.com/SJQxaCE.png" width = 600>

- ViewController가 생성되면 앱은 view를 생성해야 하는 시점에 loadView noti를 ViewController에 전달하고, ViewController는 view를 생성해서 root view로 할당합니다. Root view의 메모리 할당이 완료되면 ViewController는 viewDidLoad noti를 통해 메모리 할당이 완료되었음을 알림받습니다.

<img src = "https://i.imgur.com/17YDqJO.png](https://i.imgur.com/17YDqJO.png" width = 600>

- View hierarchy의 메모리 할당이 완료되면(viewDidLoad 후) ViewController는 view가 화면에 나타나기 시작하는 시점에 viewWillAppear noti를 받고, subview들을 생성하고 렌더링하는 과정을 거칩니다. 이 과정이 다 끝나면 viewDidAppear noti를 통해 화면에 view들이 모두 그려졌음을 알림받습니다.
- 화면 전환이 일어나면 현재 ViewController의 view는 사라지고 다른 ViewController의 view가 화면에 나타날 것입니다. 이 때, ViewController는 viewWillDisappear noti를 받고 현재 그려진 view들을 화면에서 없애는 작업을 거칩니다. 이 과정이 다 끝나면 viewDidDisappear noti를 통해 새로운 view가 화면에 나타나고 기존 view들이 화면에서 모두 사라졌음을 알림받습니다.
- 뷰컨트롤러는 초기화되고, 사용자에게 보여지고, 메모리에서 해제되기까지 여러 가지 상태를 거치게됩니다.
- 이니셜라이저로 인스턴스가 생성 된 이후에, `loadView` 가 호출됩니다.
- `loadView` 로 뷰 컨트롤러의 루트뷰를 만든 다음, 메모리에 로드 시킨 후 `viewDidLoad` 가 호출됩니다.
- 뷰가 화면에 나타나기 직전에는`viewWillAppear` , 뷰가 화면에 나타난 직후엔 `viewDidAppear` 가 호출됩니다.
- 그리고 뷰가 화면에서 사라지기 직전엔 `viewWillDisappear` , 뷰가 화면에서 사라진 직후엔 `viewDidDisappear` 가 호출됩니다.

<img src = "https://i.imgur.com/bqCgQRH.png](https://i.imgur.com/bqCgQRH.png" width = 600>

1. `loadView`
- View가 만들어지고 메모리에 올라가는 시점에 호출되는 메서드
- 사용자가 직접 호출하면 안됨
- 만약 루트 뷰를 직접 생성한다면 이 메소드를 override해서 꼭 view property에 assign 해야 한다.
- 만약 interface builder를 통해 뷰를 만들고 있다면 loadView를 override 하면 안된다
- view에 대한 추가적인 초기화를 진행하고 싶다면 viewDidLoad에서 하는 것이 권장됨
1. `viewDidLoad`
- view가 메모리에 올라온 직후 불리는 메서드
- 특별한 경우가 아니라면 화면이 처음 만들어질 때 한번만 실행이 보장되므로, 초기화 코드를 작성하는데에 좋다
1. `viewWillAppear`
- view가 Window의 view hierarchy 계층에 추가되기 직전에 불리는 메서드
- 화면에 보여지기 직전에 불리는 메서드
- 화면에 보여질때마다 호출되므로, 다른 화면에 의해 가려졌다가 다시 되돌아 왔을 때 수행해줄 작업을 작성하기에 용이
- viewWillAppear <-> viewDidAppear 구간에서 constraint, layout, draw가 수행됨
1. `viewDidAppear`
- view가 view hierarchy에 추가된 직후에 불리는 메서드
1. `viewWillDisappear`
- view가 View hierarchy에서 제거되기 직전 불리는 메서드
1. `viewDidDisappear`
- view가 View hierarchy에서 제거된 직후 호출되는 메서드
<br />
<br />

## 화면이 나타났을 때 동영상을 자동재생시키려면, 동영상을 재생하는 코드를 어떤 메서드에서 호출해야할까요?

- ViewWillAppear : 동영상 로딩
- ViewDidAppear: 동영상 재생 트리거
<br />
<br />

## 여러 화면 계층이 존재하는 앱에서 키보드 Notification을 활용할 때 Notification 등록 및 제거를 어느 메서드에서 구현해야 할까요?

- 등록: ViewWillAppear
- 삭제: ViewWillDissappear
<br />
<br />

## 뷰컨트롤러는 어떤 역할을 하나요?

- 뷰의 레이아웃과 표시를 관리합니다.
- 뷰를 통해 들어온 사용자 액션에 응답합니다. (gesture recognizer를 심거나, `touchesBegan` 오버라이드, IBAction)
- 다른 뷰 컨트롤러와 함께 협업합니다
<br />
<br />
<br />
