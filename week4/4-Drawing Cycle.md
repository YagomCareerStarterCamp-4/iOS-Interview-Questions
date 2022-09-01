# ❔ 질문
1. setNeedsLayout와 setNeedsDisplay의 차이에 대해 설명하시오. <br />

# ✏️ 작성자
- [줄라이, 숲재, 앨리](#줄라이-숲재-앨리)
<br />

---

# 줄라이, 숲재, 앨리
## setNeedsLayout와 setNeedsDisplay의 차이에 대해 설명하시오.
A. 업데이트 하는 대상이 다르다.

***- setNeedsLayout() : 사각형 자체의 크기와 위치를 layout 시키는 의미***

`layoutSubviews`를 예약하는 행위 중 가장 비용이 적게 드는 방법이 `setNeedsLayout`을 호출하는 것이다. 이 메소드를 호출한 View는 재계산되어야 하는 View라고 수동으로 체크가 되며 update cycle에서 `layoutSubviews`가 호출되게 된다. 이 메소드는 비동기적으로 작동하기 때문에 호출되면 바로 반환되고, View의 보여지는 모습은 update cycle에 들어갔을 때 바뀌게 된다.

***- setNeedsDisplay() : 사각형 내의 요소들을 draw해주는 의미***

View의 실제 컨텐츠가 변경될 때, `setNeedsDisplay()` 또는 setNeedsDisplay(_ :) 메소드를 호출해서 시스템에 **view를 다시 그려야한다**고 알릴 때 사용한다. 

이 메소드는**다음 드로잉 사이클(next drawing cycle)동안 View를 업데이트해야 함을 시스템에 알린다.**

![Untitled](https://user-images.githubusercontent.com/83864058/187881202-bc339f10-9870-49a1-a2bb-0851aceb40e9.png)
<br />
<br />

## Main Run Loop?
A. 어플리케이션이 실행되면 iOS의 `UIApplication`이 매인 스레드에서 main run loop를 실행시킨다. main run loop는 돌아가면서 터치 이벤트, 위치의 변화, 디바이스의 회전 등의 각종 이벤트들을 처리하게 되고, 이러한 처리 과정은 각 이벤트들에 알맞는 핸들러를 찾아 그들에게 처리 권한을 위임하며 진행된다.

이렇게 발생한 이벤트들을 모두 처리하고 권한이 다시 main run loop로 돌아오게 되고 이 시점을 **update cycle**이라고 한다.

**postion이나 layout 값을 변경하는 코드와 실제로 변경된 값이 반영되는 시점에는 시간차가 존재한다**
<br />
<br />

## setNeedsLayout vs layoutIfNeeded
A. 

***- layoutIfNeeded()***

이 메소드는 `setNeedsLayout`과 같이 수동으로 `layoutSubviews`를 예약하는 행위이지만 해당 예약을 바로 실행시키는 동기적으로 작동하는 메소드다. update cycle이 올 때까지 기다려 `layoutSubviews`
를 호출시키는 것이 아니라 그 즉시 `layoutSubviews`를 발동시킨다.

만일 main run loop에서 하나의 View가 `setNeedsLayout`을 호출하고 그 다음 `layoutIfNeeded`
를 호출한다면 `layoutIfNeeded`는 그 즉시 View의 값이 재계산되고 화면에 반영하기 때문에 `setNeedsLayout`이 예약한 `layoutSubviews` 메소드는 update cycle에서 반영해야할 변경된 값이 존재하지 않기 때문에 호출되지 않는다. 이러한 동작 원리로 `layoutIfNeeded`는 그 즉시 값이 변경되어야 하는 애니매이션에서 많이 사용된다.

→ `setNeedsLayout`과 `layoutIfNeeded`의 차이점은 동기적으로 동작하느냐 비동기적으로 동작하느냐의 차이!
<br />
<br />

## Drawing Cycle?
A. 

: 뷰가 로드 되거나 변경이 있을때 화면에 시각적으로 표현되어 그려지는 일종의 사이클

1. 뷰 로드 시 시스템이 UIView에게 draw 메서드를 통해 드로잉을 요청
2. 뷰의 스냅샷을 캡쳐하여 UIView에게 전달
3. 뷰의 컨텐츠 변경 시 관련 메서드 (SetNeedsDisplay, SetNeedsLayout 등등..) 호출하여 시스템에 업데이트 요청
4. Next Drawing Cycle에서 업데이트 요청 받은 뷰를 업데이트

→ 뷰의 스냅샷을 캡쳐하고 뿌려주는 프로세스를 반복하는 과정! ****`draw(_:)`****메소드를 호출해서 View를 업데이트
<br />
<br />

## View 컨텐츠 변경 관련한 업데이트 트리거 종류
A.

1. View를 부분적으로 가린 타 View의 이동 및 제거
2. 히든 뷰 노출 ( isHidden이 false로 할당.)
3. 뷰를 화면 밖과 안으로 이동 및 스크롤
4. 뷰 드로잉 사이클 관련 메서드 호출 (명시적, UIView의 setNeedsDisplay 호출)
<br />
<br />

## View Constraints 와 Layout이 그려지는 시점은 언제인가요? (ViewController의 LifeCycle메서드를 기준으로)
A. viewWillAppear 이후 viewDidAppear 이전

*view와 viewController의 layout관련 메소드의 호출 순서*

![Untitled](https://user-images.githubusercontent.com/83864058/187881539-f14df35c-63bd-440b-b038-2e3b4229cb87.png)
<br />
<br />

## Cell 이 reloadData 했을때 후처리를 하고 싶으면 어디다가 해야 될까요 ? 
답 : layoutSubviews 

![스크린샷 2022-05-11 오후 7.00.02.png](https://user-images.githubusercontent.com/83864058/187881722-f1448868-6135-4796-bdc1-606d4ac4c30e.png)

[Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uiview)
<br />
<br />
<br />

---
