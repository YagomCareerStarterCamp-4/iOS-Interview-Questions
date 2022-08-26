# ❔ 질문
오토레이아웃을 코드로 작성하는 방법은 무엇인가? (3가지)<br />
스토리보드를 이용했을때의 장단점을 설명하시오.<br />

# ✏️ 작성자
- [고사리, 허황](#고사리-허황)
- [제인, 호랭이](#제인-호랭이)
<br />

---

# 고사리, 허황
## 오토레이아웃을 코드로 작성하는 방법은 무엇인가? (3가지)
1. `NSLayoutConstaint`
    
    인터페이스 객체간에 레이아웃 관계를 나타냅니다. 가독성이 떨어지고 휴먼에러 발생률이 높아 추천되지 않습니다.
    
    ```
    NSLayoutConstraint(
    		item: myView,
    		attribute: .leading,
    		relatedBy: .Equal,
    		toItem: view,
    		attribute: .leadingMargin,
    		multiplier: 1.0,
    		constant: 0.0
    ).isActive = true
    
    ```
    
2. `NSLayoutAnchor`
    
    NSLayoutConstraint가 복잡하고 사용법이 어려워서 새로나온 클래스입니다.
    
    NSLayoutConstraint 객체를 만들어내는 팩토리 클래스이며, 가독성이 좋아 추천됩니다.
    
    ```
    let margins = view.layoutMarginsGuide
    
    myView.leadingAnchor.constraint(
    		equalTo: margins.leadingAnchor
    ).active = true
    myView.trailingAnchor.constraint(
    		equalTo: margins.trailingAnchor
    ).active = true
    myView.heightAnchor.constraint(
    		equalTo: myView.widthAnchor,
    		multiplier: 2.0
    ).active = true
    
    ```
    
    다음과 같이 배열을 통해 전체를 active 시켜주는 방법이 있다.
    
    ```
    NSLayoutConstraint.activate([
    label.widthAnchor.constraint(equalTo: Button.widthAnchor, multiplier: 2.0),
    ...])
    
    ```
    
3. `Visual Format Language`
    
    설명하고자 하는 레이아웃의 시각적인 표현을 제공하는 방식입니다. 읽을 수 있도록 설계되어 있으며 뷰는 대괄호로 표시되고 뷰간의 연결은 하이픈(또는 뷰들을 떨어뜨리는 숫자에 의해 두개의 분리된 하이픈)을 사용합니다.
    
    ```
    let views = ["redView": redView,
                 "blueView": blueView,
                 "greenView": greenView]
    let format1 = "V:|-[redView]-8-[greenView]-|"
    let format2 = "H:|-[redView]-8-[blueView(==redView)]-|"
    let format3 = "H:|-[greenView]-|"
    
    var constraints = NSConstraint.constraints(withVisualFormat: format1,
                        options: alignAllLeft,
                        matrics: nil,
                        views: views)
    constraints += NSConstraint.constraints(withVisualFormat: format2,
                        options: alignAllTop,
                        matrics: nil,
                        views: views)
    constraints += NSConstraint.constraints(withVisualFormat: format3,
                        options: []
                        matrics: nil,
                        views: views)
    
    NSConstraint.activateConstraints(constraints)
    
    ```
    

참고링크: [Documentation Archive- Visual Format Language](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH27-SW1)
<br />
<br />

## 스토리보드를 이용했을때의 장단점을 설명하시오.
**장점**

- 코드를 몰라도 뷰를 구성할 수 있다.(낮은 진입 장벽)
- 시각화 (뷰를 구성하면서 바로 볼 수 있다.)
- 개발이 빠르다.

**단점**

- 생산성 앱이 커지면 스토리보드를 로드하는데 시간이 오래 걸린다.
- 가독성 스토리보드로 제약을 걸게 되면 제약을 한눈에 파악하기도 힘들고 뷰의 구조를 파악하는데 힘들다.
- 협업 어려움(별개) 협업할 때 스토리보드를 여러사람이 만지게 되면 깃 충돌이 발생할 가능성이 많아 짐.
- 재사용성 스토리보드로 만든 뷰는 재사용하기 어렵다(xib파일로 분리해서 만들면 어느정도 해소됨)
- 관리 용이성 뷰의 Identifier를 관리해야하며 IBOutlet, IBAction 등 매번 연결해줘야한다.
- 커스텀하기 힘들다. (스토리보드로 구성할 수 있는 뷰의 한계가 있음) 예를 들어 텍스트필드의 자식으로 레이블을 등록한다던지 등 코드로 가능한 작업을 할 수 없음

코드, 스토리보드 모두 장단점을 가지고 있음. 어디가 좋다 나쁘다 할 순 없다. 현재 프로젝트에서는 스토리보드로 뷰를 구현했으며 커스텀해야할 경우 코드로 구현했다. 깃 충돌이 발생한다 하더라도 충돌을 해결할 수 있다면 스토리보드를 쓰는데 충돌은 문제가 되지 않느다.

이번 프로젝트에서 스토리보드를 채택한 이유는 빠른 개발 및 출시를 위함으로 뷰를 코드로 구현하는것보다 개발속도, 시각화 등에서 이점이 있다고 판단하여 스토리보드로 구현하였다.

스토리보드, 코드는 팀의 성향 차이이며 둘간의 장단점을 명확이 알 수 있다면 두 방식 모두 개발하는데 장단점을 가지고 있다.

### 꼬리 질문

Q1. 뷰를 구성할 때 스토리보드, 코드 어떤 방식을 더 선호하는가?
<br />
<br />
<br />

---

# 제인, 호랭이
## 오토레이아웃을 코드로 작성하는 방법은 무엇인가? (3가지)
오토리사이징 뷰 - 뷰 - 슈퍼뷰간 간계

1. **NSLayoutConstraint**

NSLayoutConstraint class의 convenience 메서드를 이용해 직접 객체를 생성하는 방법이다.

객체간 레이아웃 관계를 나타내고, 객체 생성시 파라미터에 모든 값을 넣어줘서 복잡하다.

아래와 같은 단점때문에 Apple에서도 iOS 8이하에서 지원되어야 하지 않는 이상 NSLayoutAnchor를 사용하도록 권장한다.

- 레이아웃에 관여하지 않는 파라미터도 모두 값을 지정해줘야해서 가독성이 떨어짐
- layout anchor 방식과는 다르게, 특정 제약사항의 조건들을 확인시켜 주지 않아 오류에 취약함 (runtime이 되어서야 알 수 있음)

```swift
NSLayoutConstraint(item: myView, 
								   attribute: .leading, 
                   relatedBy: .equal, 
                   toItem: view, 
                   attribute: .leadingMargin, 
                   multiplier: 1.0, 
                   constant: 0.0).isActive = true
```

2. **NSLayoutAnchor**

NSLayoutConstraint 객체를 만드는 팩토리 클래스이다. 

제약을 주고 싶은 아이템의 anchor property에 접근해서 제약을 정의하는 방식이다.

레이아웃 잡을때 대부분 사용하는 방법으로 객체간 관계를 잘 표현하며 가독성이 우수하다.

```swift
aView.leadingAnchor.constraint(equalTo: view.leadingAnchor,constant: 8).isActive = true
aView.trailingAnchor.constraint(equalTo: view.trailingAnchor,constant: -8).isActive = true
aView.topAnchor.constraint(equalTo: view.topAnchor,constant: 20).isActive = true
aView.heightAnchor.constraint(equalToConstant: 100).isActive = true

bView.leadingAnchor.constraint(equalTo: view.leadingAnchor,constant: 30).isActive = true
bView.trailingAnchor.constraint(equalTo: view.trailingAnchor,constant: -30).isActive = true
bView.topAnchor.constraint(equalTo: aView.bottomAnchor,constant: 20).isActive = true
bView.heightAnchor.constraint(equalToConstant: 100).isActive = true
```

3. ****Visual Format Language****

레이아웃의 시각적 표현을 코드로 할때 사용한다. 뷰는 `[]` 대괄호를 사용하고, 뷰 간 연결은 `-` 하이픈을 사용한다.

코드가 보다 짧고 간결한 표현을 가지고 있어 시각적으로 쉽게 관계를 파악할 수 있고 한번에 여러개의 제약을 정의할 수 있다. 

하지만, 표현성을 강조한 언어라 모든 제약사항을 정의할 수 없는 단점이 있다. 

예를 들어, A view의 `가로:세로 = 1:2` 를 정의할때 사용하는 aspect ratio를 표현할 수 있는 방법이 없다.

```swift
let views: [String : Any] = ["a": aView, "b": bView]
let format1 = "H:|-[a]-|"
let folet format4 = "V:[a]-20-[b(100)]"
        
var constraint = NSLayoutConstraint.constraints(withVisualFormat: format1, options: [], metrics: nil, views: views)
constraint += NSLayoutConstraint.constraints(withVisualFormat: format2, options: [],metrics: nil, views: views)
constraint += NSLayoutConstraint.constraints(withVisualFormat: format3, options: [], metrics: nil, views: views)
constraint += NSLayoutConstraint.constraints(withVisualFormat: format4, options: [], metrics: nil, views: views)
view.addConstraints(constraint)
```

**NSLayoutAnchor, Visual Format Language** 모두 결국 **NSLayoutConstraint**를 사용한다.
<br />
<br />

## 스토리보드를 이용했을때의 장단점을 설명하시오.
### [storyboard](https://linux-studying.tistory.com/13#storyboard)

장점

- 결과물을 예측하기 쉬움
- 속성을 쉽게 확인가능
- 소스코드를 일일히 파악하지 않아도 UI 확인 가능
- 개발 속도가 빠르다.

단점

- 무겁다 ( 스토리 보드 분리로 해결 가능 )
- 링크가 끊어졌을 때 파악하기 힘들다 ( IBOutlet, IBAction )
- 협업 문제 ( Diff 로 알기 힘들고, Conflict 나면 해결 어려움 )
- **코드리뷰 불가능**
    - 스토리보드나 xib 파일은 코드리뷰가 불가능합니다. xml 파일을 코드리뷰 할 수 있는 개발자는 아마 많지 않을거에요ㅠ.
- **Storyboard 와 코드간 연결**
    - 스토리보드와 swift 파일간 연결이 끊겨도 빌드가 됩니다. 빌드타임이 아니라 런타임에 체크하기 때문이죠.
- 뷰를 재사용하기 힘들다
- 커스텀 불가능 - 텍스트필드에 레이블 올리기 어렵다

### [Code Base](https://linux-studying.tistory.com/13#Code%--Base)

장점

- 가볍다 ( 코드만 나와 있으므로 )
- Diff 만 보고 파악하기 쉽다
- Conflict 발생 가능성이 낮아진다

단점

- 해당 컴포넌트를 숙지하고 있어야 한다.
- 어떤 화면이 만들어질지 파악하기 힘들다.
- 코드가 상당히 길어진다
<br />
<br />
<br />
