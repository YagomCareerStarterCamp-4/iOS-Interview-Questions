# ❔ 질문
1. UIView 에서 Layer 객체는 무엇이고 어떤 역할을 담당하는지 설명하시오<br />

# ✏️ 작성자
- [제인, 숲재](#제인-숲재)
- [지성](#지성)
<br />

---

# 제인, 숲재
## UIView 에서 Layer 객체는 무엇이고 어떤 역할을 담당하는지 설명하시오

- image based 컨텐츠를 관리할 수 있고 애니메이션을 제공한다.
- 먼저 레이어는 주로 뷰의 backing store를 제공하는 용도로 사용하지만 어떤 경우에는 뷰가 없이도 컨텐츠를 표시하기 위해 사용될 수도 있다. 레이어의 주요한 임무는 시각적인 컨텐츠를 관리하는 것이지만 동시에 레이어 스스로가 가지고 있는 시각적인 속성들도 있다.
- 컨텐츠의 크기, 위치, 변형 정보를 보관
- 뷰는 레이어를 만들면서 자동으로 자기 자신을 레이어의 delegate로 할당
<br />
<br />

## Layer-Based Drawing 과정을 설명하시오.

![https://i.imgur.com/PvKkOBi.png](https://i.imgur.com/PvKkOBi.png)

- 대부분의 레이어는 실제로 drawing 작업을 하지 않는다.
- 레이어는 현재 컨텐츠를 캡쳐하고 bitmap으로 캐싱해놓는다. (backing store라고 부름)
- 레이어의 속성이 변경되어 animation을 트리거하면, Core Animation은 레이어의 bitmap과 state information을 그래픽 하드웨어(GPU)로 보내고, 그 하드웨어가 이 bitmap을 렌더링한다.
- 그래서 사실상 레이어는 데이터만 다루기 때문에 모델 객체라고 할 수 있다.
<br />
<br />

## View-Based Drawing 과정과 비교한 Layer-Based Drawing 장점은?

- View-Based의 경우 view의 변경은 `drawRect:`메서드를 호출하게되는데, 이 작업은 main Thread위에서 CPU에 의해 수행되기 때문에 비용이 매우 크다. 반면에 Core Animation은 GPU에 캐싱된 bitmap을 사용하여 이러한 비용을 피할 수 있다.
![https://i.imgur.com/9ImuXvb.png](https://i.imgur.com/9ImuXvb.png)
<br />
<br />

## CoreAnimation이 무엇인가요?

- Layer-Based Drawing을 사용한다.
- 코어 애니메이션은 CPU에 부담을 주거나 앱을 느리게 하지 않고 높은 프레임과 부드러운 애니메이션을 제공
- GPU를 이용해서 렌더링을 가속화하고 부드러운 프레임과 애니메이션을 지원
<br />
<br />

## Layer와 View의 차이점을 설명하시오.

- Layer는 뷰가 가진 컨텐츠와 애니메이션을 표현함에 있어 더 좋은 성능으로 제공하기 위한 보조적인 역할
- View는 이벤트를 핸들링하고, 컨텐츠를 그리고, Responder Chain에 참여할 수 있다.
![https://i.imgur.com/lYZnyzT.png](https://i.imgur.com/lYZnyzT.png)    
<br />
<br />

## CALayer는 어떻게 구성되어 있나요?

UIView는 하나의 CALayer만 가지고 있고 CALayer는 여러개의 subLayer를 가지고 있다

layer는 비용이 저렴해서 여러개의 UIView를 만드는 것보다 여러개의 layer를 만드는 것이 훨씬 가볍다고 한다.

CALayer의 속성으로는 대표적으로 cornerRadius, shadow, border, frame, bounds 등이 있다
<br />
<br />

## CALayer로 애니메이션 등을 만드는 효과를 넣으면 성능이 저하된다. 이럴때 해결할 수 있는 방법이 있나?

- `shouldRasterize`

CALayer를 그릴 때 오직 한번만 랜더링하겠다는 속성, 디폴트는 false

true로 지정시 애니메이션 시 레이어의 컨텐츠를 재활용하여 성능 향상

레이어가 움직이기는 하나, 모양이 변하지 않을때 유용하게 사용 가능

- `drawsAsynchronously`

랜더링을 여러번 하는 상황에서 백그라운드 스레드에서 해당 작업이 일어나도록 설정하는 속성

![https://i.imgur.com/HaDtP7J.png](https://i.imgur.com/HaDtP7J.png)
<br />
<br />

## clipsToBounds, masksToBound를 설명해보세요

둘은 같은 역할을 하지만 프로퍼티가 속한 곳이 다르다. clipsToBounds는 UIView, masksToBounds는 CALayer에 속해 있다.

둘은 SubView나 SubLayer의 내용(글씨 등)이 루트 Layer의 경계를 무시하고 자신을 보여주느냐 경계에 맞추어 잘리느냐를 설정해주는 Bool타입의 메서드이다.
<br />
<br />

## Layer의 좌표 시스템에는 어떤 종류가 있나요?

- Point-Based: Screen 좌표계와 관련이 있거나 다른 레이어와의 관계있는 값을 사용하는 좌표시스템
ex) position 프로퍼티
- Unit-Based: Screen 좌표계와 연관이 없는 값을 사용하는 좌표시스템
ex) anchorPoint 프로퍼티
<br />
<br />

## Layer의 좌표 시스템관련 프로퍼티들을 설명해 보시오.

- Position(Point-Based): UIView의 Bounds와 유사하나, CGPoint 타입으로 width와 height 값을 가지고 있지 않다.
- Bounds: UIView의 Bounds와 동일함
- Frame: Bounds와 Position으로 계산됨
- Anchor Point(Unit-Based):
    - (0,0)에서 (1,1)까지의 좌표로 표현되며, layer의 bounds와의 상대적인 위치 값을 설정하여 Position의 값에 영향을 미친다.
    - 레이어를 회전시켰을 때 anchor point를 기준으로 회전한다.

![https://i.imgur.com/1drOSID.png](https://i.imgur.com/1drOSID.png)

[https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/CoreAnimationBasics/CoreAnimationBasics.html#//apple_ref/doc/uid/TP40004514-CH2-SW17](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/CoreAnimationBasics/CoreAnimationBasics.html#//apple_ref/doc/uid/TP40004514-CH2-SW17)
<br />
<br />
<br />

---

# 지성

## UIView 에서 Layer 객체는 무엇이고 어떤 역할을 담당하는지 설명하시오

UIView는 화면에 그리는 작업과 애니메이션 등의 시각적 행위를 직접 처리하지 않고 `Core Animation`클래스인 `CALayer`에게 위임하는데, 모든 UIView는 해당 타입의 `layer`프로퍼티를 가지고 있다.

디테일하게 `그림자`, `테두리`, `3D 변형`, `마스킹(Masking)`, `애니메이션` 등의 작업을 처리한다. 유연한 커스터마이징이 가능하다는 특징이 있다.

**CALayer란 스크린에 시각적 컨텐츠를 그려내는 사각형의 Class이다.** 

**이 때, 모든 View에 기본적으로 CALayer 클래스를 할당해서 생성된다.**

**CALayer는 실제로 UIView에 속하면서 UIView를 지원해주는 역할**

**각 뷰마다 하나의 루트 Layer가 존재하는데 UIView는 레이아웃과 터치 이벤트 처리 등 작업을 담당하게 되고 실제로 뷰 위의 컨텐츠나 애니메이션을 그리는 행위는 Core Animation 계층에게 양도하게 되는데 이러한 역할들을 CALayer가 담당하게 된다.**

**CALayer는 또한 별도의 스레드에서 동작하면서 GPU에서 그려지게 됩니다. 즉, 그래픽적으로 더욱 많은 요소를 담당하게 됩니다.**
<br />
<br />


## clipsToBounds, masksToBound를 설명해보세요
ClipsToBounds는 뷰의 프로퍼티, masksToBounds는 CALayer의 프로퍼티

ClipsToBounds는 슈퍼뷰의 영역밖으로 나간 서브뷰의 영역은 그리지 않음 (True일시)

masksToBounds는 self의 영역(layer)밖으로 나간 sublayer의 영역은 그리지 않음 (True일시)
<br />
<br />

## CALayer발전 과정

OpenGL → CoreGraphic → CoreAnimation 
<br />
<br />

## CALayer를 통해 많은 애니메이션을 구현하면 성능에 문제가 생기는데 이를 해결 할 수 있는 프로퍼티는?

- `shouldRasterize`

layer의 모양이 더 이상 변하지않을 때 사용하기 적합하며, 해당 프로퍼티 값을 true로 주면, 레이어의 컨텐츠를 한번만 렌더링하고 이후엔 재활용된다.

- `drawsAsynchronously`

layer의 컨텐츠를 반복적으로 그릴 때 사용하기 적합하며 layer를 그리는 데 필요한 작업을 Background스레드에서 수행할 것인지 여부를 지정한다.
<br />
<br />


## clipsToBounds VS masksToBounds

clipsToBounds는 뷰의 프로퍼티, masksToBounds는 CALayer의 프로퍼티

clipsToBounds는 슈퍼뷰의 영역밖으로 나간 서브뷰의 영역은 그리지 않음 (True일시)

masksToBounds는 self의 영역(layer)밖으로 나간 sublayer의 영역은 그리지 않음 (True일시)
<br />
<br />

## Core Animation 발전 과정

OpenGL(현재 Swift는 Metal을 사용함(spriteKit 등)) → CoreGraphic → CoreAnimation -> UIKit

간편하게 만들어지면서 고수준 기능들이 빠지게 된 것
<br />
<br />

## 뷰의 rootlayer와 sublayer에 대해 설명해보세요

각 뷰마다 `root layer`를 가지고 있고, `root layer`는 `sublayer`를 가집니다.

`UIView`의 `layoutIfNeeded()` 를 호출하게 되면 루트 `CALayer` 에게로 forwarding합니다.

`UIView`의 `bounds`가 변경되면 `UIView`는 자신의 `root layer`의 `bounds`를 변경합니다. 하지만 `sublayer`는 자동으로 변경되지 않습니다.

sublayer를 변경 하려면 CALayerDelegate의 layoutSublayers(of:)메서드에서 수정할 수 있습니다.

> var sublayers: [CALayer]? { get set }
> 
<br />
<br />

## layer 객체를 사용해본 적이 있나요?

뷰의 그림자를 구현하기 위해 `layer`의 프로퍼티들을 사용한 경험이 있습니다.
<br />
<br />

## Core Animation이 CPU에 부담을 주지 않는 이유는 무엇인가요?

레이어는 컨텐츠를 캡처하여 레이어의 backing store에 비트맵으로 캐싱합니다. 

이때 레이어의 프로퍼티에 변경이 생기면 Core Animation은 해당 레이어의 비트맵과 상태정보를 GPU로 보내고, GPU가 이 정보를 바탕으로 새로운 비트맵을 그립니다.

View의 draw(_:)가 메인스레드의 CPU를 사용하는 것과는 다르게, CPU에 부담을 주지 않는 결과를 야기합니다.

렌더링단까지만 gpu, 렌더링된 컨텐츠를 그리는 작업은 cpu

<br />
<br />
