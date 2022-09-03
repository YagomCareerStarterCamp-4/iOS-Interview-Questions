# ❔ 질문
- hugging, resistance에 대해서 설명하시오.<br />
- Intrinsic Size에 대해서 설명하시오.<br /><br />
- Safearea에 대해서 설명하시오.<br />
- Left Constraint 와 Leading Constraint 의 차이점을 설명하시오.<br />
- Bounds 와 Frame 의 차이점을 설명하시오.<br />

# ✏️ 작성자
- [아리, 애플사이다](#아리-애플사이다)
- [호댕, 고사리, 토니](#호댕-고사리-토니)
<br />

---

# 아리, 애플사이다
## hugging, resistance에 대해서 설명하시오.

`아리`

- 허깅의 경우 외부에서 압력을 줄 때 늘어나지 않으려고 버티는 힘을 말합니다.
- 리지스턴스는 외부에서 압력을 줄 때 버티는 힘을 말합니다.
- 즉 이 두가지는 자신의 사이즈를 지키려는 힘을 뜻하고 있습니다. 우선순위를 가지고 그것을 기반으로 힘의 세기를 결정하게 됩니다.

`애플사이다`

- Intrinsic Content Size를 활용하여 Layout을 설정하려면 CHCR 조정이 필요하다.
    - Compression-Resistance (압축 저항) : 외부 압력에 의해 콘텐츠가 잘리지 않도록 버티는 힘
    - Content-Hugging (컨텐츠 허깅) : Intrinsic Content Size와 딱맞게 줄어들려고 하는 힘
- 둘은 반대 개념이 아니며, 다른 constraint와 마찬가지로 CHCR에도 우선도가 적용된다.

> hugging 예제로 이해하기
> 
- 예시로 레이블의 Content Hugging을 각각 다른 강도로 주어졌을때 250으로 세팅되어있는 레이블이 늘어난 것을 확인할 수 있다.
    - 1000은 절대 늘어나지 않고, 750은 조금 늘어나도 괜찮고, 250은 늘어나도 상관없다는 뜻
        
        ![https://i.imgur.com/DEvpiY6.png](https://i.imgur.com/DEvpiY6.png)
        

> resistance 예제로 이해하기
> 
- Priority가 가장 낮은 레이블은 아무리 늘어나려고 해도 옆에 Priority가 높은 레이블이 있기 때문에 더 이상 늘어나지 않는 것을 확인할 수 있다.
    
    ![https://i.imgur.com/8dUh3lW.png](https://i.imgur.com/8dUh3lW.png)
<br />
<br />

## Intrinsic Size에 대해서 설명하시오.

`아리`

- 콘텐츠의 기본적인 크기를 뜻합니다. 뷰가 원래 가지고 있었던 크기를 고유 콘텐츠 크기라고 합니다.
- 즉 오토 레이아웃에 의해 변경되기 전 원본 크기를 의미합니다.
- 오토 레이아웃은 기본적으로 뷰의 위치와 크기를 모두 정의하도록 Constraint를 구성해야 합니다. 하지만 어떤 뷰는 콘텐츠에 따라 고유한 크기를 가지곤 합니다. 이 크기를 Intrinsic Content Size라고 합니다. 예를 들어 버튼은 버튼의 타이틀과 마진의 크기를 Intrinsic Content Size로 가집니다.

`애플사이다`

- 원래는 View의 위치 및 Size를 모두 지정해줘야 하는데, 일부 View는 View 내부의 컨텐츠에 따라 자체적으로 Size를 가지도록 되어있다. constraint 개수가 줄어드므로 유용하다. 예를 들어 Button에 텍스트를 넣으면, “해당 텍스트 + default margin”의 크기가 Button 자체 크기가 된다. (font 등의 영향을 받음)
<br />
<br />

## Intrinsic Size를 가지는 뷰, 가지지않는 뷰는 어떤 것들이 있나요?

`아리`

- UIView는 Intrinsic Size를 가지지 않습니다.
- UILabel, UIButton, UISwitch, UITextField는 높이와 너비를 모두 가집니다.
- UITextView, UIImageView는 콘텐츠에 따라 크기가 변합니다.
- UISlider는 이미지의 최소, 최대 이미지의 Intrinsic Size에 따라 높이가 결정됩니다.
    - [https://developer.apple.com/documentation/uikit/uislider](https://developer.apple.com/documentation/uikit/uislider)

`애플사이다`

- UIView : 없음
- Slider : 높이만 가능 (iOS) ← width는 오타인듯
- Label, Button, Switch, Text Field : 높이/너비 가능 ← 위치만 정해주면, 크기는 자동으로 반영됨
- TextView / ImageView : 상황에 따라 다름 (스크롤이 없으면 가능)
<br />
<br />

## SafeArea에 대해서 설명하시오.

`아리`

- 어플리케이션이 상태바, 네비게이션바, 툴바, 탭바를 가리는 것을 방지하는 영역을 뜻합니다.
- 표준 시스템이 제공하는 뷰들은 모두 안전 영역을 준수하고 있습니다.

`애플사이다`

- iOS11 이후 아이폰 상단에 노치가 등장하면서 SafeArea 개념이 생겼다. landscape 모드에서 노치 부분이 View를 가리는 것을 막기 위해 Top/Bottom 외에도 Leading/Trailing에 대한 Margin도 필요해졌다.
<br />
<br />

## Layout Margin이 무엇인가요

Margin은 View와 컨텐츠 (또는 View와 SubViews) 사이의 default spacing이다. 상하좌우 각각 inset 설정 가능하며, 특정 View의 bound 기준으로 내부 영역에 설정된다.
<br />
<br />

## Left Constraint와 Leading Constraint의 차이점을 설명하시오.

`아리`

- 리딩은 텍스트의 시작을 뜻합니다. 영어나 한국어는 텍스트의 시작이 좌측부터 시작하지만, 아랍어 같은 경우 텍스트의 시작이 우측으로 부터 시작하기 때문에 국가에 따라 다른 설정이 됩니다. Left는 말 그대로 왼쪽 제약조건을 뜻합니다.

`애플사이다`

- Left : 항상 기기의 왼쪽 방향을 가르킨다.
- Leading : 언어의 텍스트 진행방향을 고려한 방향이다. 영어, 한국어는 텍스트가 시작하는 방향이 왼쪽인 LTR 언어이고, 아랍어는 반대인 RTL 언어이다. 지역화를 고려하므로 보다 유연한 프로그램을 만들 수 있다.
<br />
<br />

## Bounds 와 Frame 의 차이점을 설명하시오.

`아리`

- 프레임은 슈퍼 뷰 좌표계에서 뷰의 위치와 크기를 나타냅니다. 뷰의 위치 및 크기를 설정할 때 주로 사용하게 됩니다.
- 바운즈는 자신의 좌표계에서 뷰의 위치와 크기를 나타냅니다. 뷰를 회전 한 후 뷰의 실제 크기를 알고 싶을 때 사용합니다.
- 둘의 차이점은 프레임의 사이즈 경우 뷰 영역을 모두 감싸는 사각형으로 나타냅니다. 하지만 바운즈는 뷰 자체의 영역을 나타냅니다. 프레임과 다르게 뷰 영역을 모두 감싸서 만든 사각형이 아니라 뷰 자체의 영역을 나타낸다고 합니다.

`애플사이다`

- Origin의 기준이 차이가 있는데, Frame은 superview의 origin, Bound는 자기 자신 view의 origin을 기준으로 한다. 따라서 View를 회전시켰을 때 Frame과 달리 Bound는 항상 Origin을 유지하며 Size가 같다.
<br />
<br />

## Bounds를 사용하는 예시가 어떤게 있나요?

`아리`

- View 내부에 그림을 그릴 때(drawRect), 혹은 ScrollView에서 스크롤링 할 때 사용합니다.
    - ScrollView의 SubView 중 어떤 영역을 보여줄지 정할 때 사용할 수 있습니다.

`애플사이다`

- ScrollView에서 View (화면에 보이는 View)를 이동시킬 때, ScrollView의 bound를 변경시킨다.
- View를 회전시킨 이후 View 자체의 너비/높이를 파악할 때 사용한다.
<br />
<br />

## 오토레이아웃은 bounds를 다루는 걸까요? frame을 다루는 걸까요?

- frame을 다루는 것이라고 생각합니다. frame은 bounds와는 다르게 뷰의 위치를 설정할 때 사용하기 때문입니다.

<br />
<br />
<br />

---

# 호댕, 고사리, 토니

## hugging, resistance에 대해서 설명하시오.

Hugging은 값이 클수록 이보다 hugging이 작은 것에 비해 더욱 늘어나지 않으며, resistance는 값이 클수록 더욱 줄어들지 않는다고 알고 있습니다.

기본적으로 hugging의 경우 250, resistance의 경우 750이 기준으로 알고 있습니다.

관련 링크: [https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/ViewswithIntrinsicContentSize.html#//apple_ref/doc/uid/TP40010853-CH13-SW1](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/ViewswithIntrinsicContentSize.html#//apple_ref/doc/uid/TP40010853-CH13-SW1)
<br />
<br />

## Intrinsic Size에 대해서 설명하시오.

Intrinsic Size는 콘텐츠의 본질적인 크기입니다.
모든 view가 Intrinsic Size를 갖는 것은 아닙니다.

- `UILabel`, `UIButton`, `Switch`, `UITextField`: width, height 모두 `Intrinsic Content Size`에 정의되어 있다.
- `UILabel`과 `UIButton`의 `Intrinsic Content Size`는 표시되는 text의 양과 사용된 글꼴을 기반으로 한다.
- `UITextView`, `UIImageView`: 콘텐츠 크기에 따라 다를 수 있다.

또한, 각각의 view마다 Intrinsic Size가 적용되는 방식이 다릅니다.

intrinsic size가 정해져 있는 경우 이미 자체로 크기를 알고 있기 때문에 따로 레이아웃(heightAnchor, widthAnchor)을 잡아주지 않아도 됩니다.
<br />
<br />

## UISlider의 Intrinsic Size가 어떻게 정해지는지 알고 있나요?

UISlider의 Intrinsic Size는 슬라이더의 currentThumbImage를 기준으로 높이가 정해집니다.
너비는 따로 Intrinsic Size가 존재하지 않습니다.
<br />
<br />

## Safearea에 대해서 설명하시오.

safearea는 전체 UI의 보이는 부분 내에 view를 배치하는데 도움을 주는 영역입니다. 따라서 navigation controller, TabBar, StatusBar에 컨텐츠가 가리지 않도록 쉽게 정할 수 있습니다.

![https://i.imgur.com/cTh7iRh.png](https://i.imgur.com/cTh7iRh.png)
<br />
<br />

## SafeArea 이전에는 어떤 것을 사용했고 왜 SafeArea가 생기게 됐는지 알고 있나요?

Top / Bottom Layout Guide를 통해 NavigationBar, Status Bar, TapBar에 컨텐츠가 가리지 않도록 했다고 알고 있습니다.
다만 아이폰에 노치가 생기기 시작(iOS 11 / iPhone X)하면서 화면을 가로로 할 경우 leading과 trailing에도 일정 마진이 필요하게 되었고 SafeArea가 생기게 되었다고 알고 있습니다.
<br />
<br />

## Left Constraint 와 Leading Constraint 의 차이점을 설명하시오.

`Left Constraint`는 어떤 객체의 왼쪽을 뜻하고, `Leading Constraint`는 어떤 객체의 앞쪽 가장자리를 뜻합니다.

`Left Constrint`와 `Right Constraint`는 절대적이며 항상 화면 또는 컨트롤의 왼쪽 / 오른쪽을 참조합니다. `Leading Constraint`와 `Trailing Constraint`는 device locale의 영향을 받습니다. (장치별 국가 설정)

읽기 방향이 오른쪽에서 왼쪽인 locale(예: 히브리어, 아랍어)에서는 leading이 오른쪽이 되고 trailing이 왼쪽이 됩니다.
<br />
<br />

## Bounds 와 Frame 의 차이점을 설명하시오.

Bounds와 Frame은 모두 CGRect 타입이며 origin 즉 원점을 나타내는 데이터와 size를 나타내는 데이터를 갖고 있습니다.

frame은 상위뷰 기준, bounds는 자기 자신 기준으로 좌표를 나타냅니다.

### **frame**

[Apple Developer Documentation - frame](https://developer.apple.com/documentation/uikit/uiview/1622621-frame)

The frame rectangle, which describes the view’s location and size in its superview’s coordinate system.

상위뷰의 좌표 시스템에서 뷰의 위치와 사이즈를 나타냅니다.

Frame은 superView(한 단계 상위 뷰)의 좌표시스템안에서 view의 위치와 크기 값이고 주로 View의 위치나 크기를 설정하는 경우 사용합니다.
<br />
<br />

### bounds

[Apple Developer Documentation - bounds](https://developer.apple.com/documentation/uikit/uiview/1622580-bounds)

The bounds rectangle, which describes the view’s location and size in its own coordinate system.

자신의 좌표 시스템에서 뷰의 위치와 크기를 나타냅니다.

Bounds는 자신만의 좌표시스템에서의 View의 위치와 크기 값이고 origin은 디폴트로 (0,0)으로 설정되어 있으며 주로 View내부에 그림을 그릴 때 (drawRect) 사용합니다.
<br />
<br />

## Frame과 Bounds는 각각 언제 사용되나요?

frame은 UIView의 위치나 크기를 설정할 때 사용합니다. 스토리보드에서 우측에 X좌표와 Y좌표가 frame의 좌표입니다.

bounds는 View의 크기를 알고 싶거나 View내부에 그림을 그릴 때 사용합니다. 스크롤뷰를 스크롤할 때에도 contentoffset이 변하며 scrollview의 bounds를 계속 바꿔주는 것으로 알고 있습니다.
<br />
<br />

## 현재 잡혀있는 Layout을 비활성화하는 방법에 대해 아시나요?

현재 잡혀있는 Constraints의 isActive 프로퍼티를 false로 변경해주거나, NSLayoutConstraint.deactivate에 값을 넣어주는 방법으로 Layout을 비활성화할 수 있다고 알고 있습니다.
<br />
<br />

## addSubview를 하고 그 후 Constraints를 잡아줘야 하는 이유는 무엇일까요?

Constraints를 잡기 위해선 서로 같은 Hierarchy에 위치하도록 하여야 하는데 addSubView를 하기 전에는 서로 같은 Hierarchy에 있지 않기 때문입니다
<br />
<br />
<br />
