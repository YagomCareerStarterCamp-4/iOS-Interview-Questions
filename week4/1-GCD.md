# ❔ 질문
1. NSOperationQueue 와 GCD Queue 의 차이점을 설명하시오. <br />
2. GCD API 동작방식과 필요성에 대해 설명하시오. <br />
3. UIKit클래스들을 다룰 때 꼭 처리해야하는 스레드 이름은? <br />

# ✏️ 작성자
- [지성, 릴리, 예하](#지성-릴리-예하)
- [아샌, 제리, 호랭이](#아샌-제리-호랭이)
<br />

---

# 지성, 릴리, 예하
## NSOperationQueue 와 GCD Queue 의 차이점을 설명하시오.
- NSOperationQueue는 objc로 이뤄진 고수준 API입니다. GCD에 비해 상대적으로 느리고, 오버헤드를 더 발생시킵니다. 고수준 API이므로 GCD엔 없는 기능(예를 들면 작업 취소, 일시 중지, 재사용, KVO)을 사용하기 더 편합니다. (GCD는 따로 구현을 해줘야 합니다.)
- GCD는 큐 자체에 우선순위를 매깁니다. NSOperationQueue는 큐 내부의 오퍼레이션에 우선순위를 매기거나, 오퍼레이션끼리의 의존성을 추가하여 작업 순서를 지정해줄 수 있습니다.
- GCD의 디스패치 큐 기본 동작 방식은 Serial(직렬)입니다. NSOperationQueue는의 기본 동작 방식은 Concurrent로(병렬)이다.
<br />
<br />

## NSOperationQueue가 오버헤드가 더 발생하는 이유는 무엇인가요?
- objc는 Static Dispatch 없이 Dynamic Dispatch로 동작하기 때문에 오버헤드가 더 발생한다고 생각합니다.
<br />
<br />

## GCD API 동작 방식과 필요성에 대해 설명하시오.
- GCD는 FIFO 방식의 작업 대기 Queue를 제공하고 관리합니다. DispatchQueue는 Serial, Concurrent로 구분됩니다. Serial은 큐에 추가된 순서 대로 작업을 처리합니다. Concurrent는 큐에 추가된 순서 대로 작업을 처리하지만, 동시에 하나 이상의 작업을 처리합니다.
- 작업은 Async, Sync 방식으로 처리됩니다. Async는 큐에서 작업이 나온 후 나간 작업이 완료되는 것을 기다리지 않고 다음 작업을 내보내고, Sync는 나간 작업이 완료되길 기다립니다.
- 앱에서 통신과 같이 시간이 오래 소요되는 작업을 동기로 처리하게 되면 응답이 오기 전까지 앱이 멈춘 상태로 있게 되고, 사용성을 저하시킵니다. 이 때 GCD 등을 통해 UI 처리 등의 여러 작업을 동시적으로 처리할 수 있습니다.
- GCD 이전에는 멀티 스레딩을 위해서 직접 스레드 락을 거는 등의 관리가 필요했지만, GCD가 제공되어 멀티 스레드 사용시 운영체제가 관리해주어 간편하게 작업을 할 수 있습니다.
<br />
<br />

## Global DispatchQueue 의 Qos 에는 어떤 종류가 있는지, 각각 어떤 의미인지 설명하시오.
- Quality of Service는 Dispatch Queue에서 수행할 작업을 분류하기 위해 사용합니다.
- 시스템은 QoS를 사용하여 스케줄링, CPU, I/O 처리량, 타이머 지연 시간 등 우선 순위를 조정합니다.
- 수행하는 작업의 중요도에 적합한 QoS를 지정하면, CPU가 작업의 우선순위를 스케줄링하고 시스템 자원을 효율적으로 사용할 수 있습니다. 그래서 수행된 작업은 성능과 에너지 효율성 사이에서 균형을 유지하게 됩니다.
- 사용자와 다른 작업에 미치는 영향을 고려해서 지정해야 합니다. [참고 URL](https://developer.apple.com/documentation/dispatch/dispatchqos/qosclass)
    
    ![https://i.imgur.com/H6gzJEt.png](https://i.imgur.com/H6gzJEt.png)
    
    ![https://i.imgur.com/vd5Y4Zu.png](https://i.imgur.com/vd5Y4Zu.png)
    
    [참고 URL](https://developer.apple.com/library/archive/documentation/Performance/Conceptual/EnergyGuide-iOS/PrioritizeWorkWithQoS.html)
    
- `user interactive`
    - main 스레드에서 작동하거나 UI 업데이트하거나 animation을 수행하는 등 사용자와 상호작용하는 작업입니다.
    - 작업이 빨리 수행되지 않으면 UI가 멈춘 것처럼 보일 수 있습니다.
    - 사용자의 행동에 대한 즉각적인 반응이 기대되지만, 메인 스레드에서 진행하기엔 많은 로드가 걸리는 작업을 `user interactive` 에서 처리해서 바로 동작하듯이 보이도록할 수 있습니다.
    - 대응력과 성능에 초점을 맞춥니다.
- `user initiated`
    - 몇 초 이하, 즉각적인 반응을 요구하는 작업을 의미합니다.
    - 사용자와 상호작용을 지속하기 위해 필요한 작업입니다.
    - 저장된 문서를 열기, UI에서 클릭해서 열기 등 시간이 좀 걸릴 수 있다는 사실을 사용자가 인지하고 있는 작업을 할당을 할당할 때 적합합니다.
    - 대응력과 성능에 초점을 맞춥니다.
- `default`
    - (특수 QoS) 작업을 분류하기 위해 사용하는 것이 아닙니다. 구체적인 분류를 지정하지 않은 작업은 default로 처리됩니다.
    - GCD global queue는 이 수준에서 실행됩니다.
- `utility`
    - 데이터 다운로드, 사용자가 볼 수 있는 진행률 표시줄 등 몇 초 ~ 몇 분이 걸리며, 즉각적인 결과가 필요하지 않은 작업에 사용합니다.
    - 반응성, 성능, 에너지 효율성 간의 균형에 중점을 둡니다.
- `background`
    - 몇 분 ~ 몇 시간, 상당한 시간이 걸리며 사용자가 직접적으로 인지하지 않는 작업을 처리할 때 사용합니다.
    - 인덱싱, 동기화, 백업 등 백그라운드에서 작동하는 작업에 적합합니다.
    - 에너지 효율성에 중점을 둡니다.
- `unspecified`
    - (특수 QoS) QoS 정보가 없음을 나타냅니다. 시스템에서 추론되어야 합니다. 이제 더 이상 사용하지 않는 레거시 API를 사용하고 있을 때, 이 QoS를 가집니다.

[참고 URL](https://developer.apple.com/library/archive/documentation/Performance/Conceptual/EnergyGuide-iOS/PrioritizeWorkWithQoS.html)
<br />
<br />

## UIKit 클래스들을 다룰 때 꼭 처리해야하는 애플리케이션 쓰레드 이름은 무엇인가? 
- main thread 입니다.
- UI 업데이트와 같은 사용자에게 직접적인 영향을 미치는 작업은 매우 중요하며, 백그라운드에서 발생할 수 있는 다른 작업보다 우선순위가 높습니다.
[참고 URL](https://developer.apple.com/documentation/uikit)
[참고 URL](https://developer.apple.com/library/archive/documentation/Performance/Conceptual/EnergyGuide-iOS/PrioritizeWorkWithQoS.html)
    
    ![https://i.imgur.com/ceF0eCs.png](https://i.imgur.com/ceF0eCs.png)
    
    ![https://i.imgur.com/wFtUrlv.png](https://i.imgur.com/wFtUrlv.png)
<br />
<br />

## 왜 main 쓰레드에서 처리되어야 하나요?
1. 

![https://i.imgur.com/LcvtO0w.png](https://i.imgur.com/LcvtO0w.png)

- 사용자로부터 발생한 터치, 줌 등의 event는 UIApplication에서 Window, View 순의 responder chain을 따라 최종적으로 UIResponder로 전달된다.
- Responder는 다양한 이벤트(터치, 줌, 핀치 등)를 처리하여 UI에서 변경사항으로 변환된다.
- 기본적으로 AppDelegate `(@main) UIApplicationMain(_:_:_:_:)`을 사용해 UIApplication 인스턴스를 메인 스레드에서 생성하는데, 이 때 생성된 UIApplication은 앱의 run loop를 포함하여 event loop 설정과 event 처리를 맡는다.
- 이러한 Responder Chain 이벤트가 메인 스레드에서 발생하므로 UIResponder에 관련한 것들은 메인 스레드에서 처리되야 한다.

[참고 URL](https://stackoverflow.com/questions/18467114/why-must-uikit-operations-be-performed-on-the-main-thread)

2. UIKit의 요소들이 Thread-Safe하지 않다고 알고 있습니다. (Thread-Safe하게 만들려면 atomic으로 하거나 NSLock 등으로 처리해야 한다고 합니다.) iOS의 Graphics Rendering이 궁극적으로 동기로 처리되기 때문에 성능 향상을 위해서 main thread에서 serial하게 처리하는 것으로 이해하고 있습니다.

Advanced Graphics and Animations for iOS Apps (2014 WWDC 인데, 애플 공홈에서는 영상이 안보입니다.)
[참고 URL](https://docs.huihoo.com/apple/wwdc/2014/419_advanced_graphics_and_animation_performance.pdf)
    
![https://i.imgur.com/apPw0Ot.png](https://i.imgur.com/apPw0Ot.png)
    

3. 메인 스레드의 `Main RunLoop`에 따라 뷰 드로잉 사이클도 함께 일어나기 때문입니다.

UI작업을 꼭 메인 스레드에서 작업해야하는 이유는 메인 스레드에는 `Main RunLoop`가 동작하고 있기 때문입니다. 메인 스레드에서는 RunLoop가 일정한 주기를 유지하며 계속 동작하고 있습니다. 이 주기에 맞추어서 사용자의 입력을 받아서 UI를 그리게 됩니다. 이러한 주기를 `View Drawing Cycle`이라고 합니다.
하지만 사실 모든 스레드는 각자의 RunLoop를 가질 수 있는데요, 왜 꼭 Main RunLoop에 의존해야할까요? 그 이유는 모든 스레드의 RunLoop에 따라 각자가 UI를 그리게 된다면 UI가 그려지는 시점이 모두 제각각이 되기 때문입니다. 그렇게 되면 비효율적일 뿐더러 위에서 언급한대로 Race Condition이 발생하게 되지요. 따라서 기준을 하나로, Main RunLoop의 기준에 따르는 것으로 문제를 해결할 수 있습니다. 사실 위의 내용과 맥락이 비슷한 이유라고 할 수 있습니다.
<br />
<br />
<br />

---

# 아샌, 제리, 호랭이
## NSOperationQueue 와 GCD Queue 의 차이점을 설명하시오.
- NSOperationQueue
    
    - 우선 순위와 준비상태에 따라 대기열에 있는 개체를 호출
    대기열에 작업을 추가하면 작업이 완료될 때까지 대기열에 남아있고, 작업을 추가한 후에는 대기열에서 직접 제거할 수 없음
    작업 대기열은 작업이 완료될 때까지 작업을 유지하고 대기열 또한 모든 작업이 완료될 때까지 유지됨
    완료되지 않은 작업이 있는 대기열을 중단하면 메모리 누수가 발생할 수 있음
    ![Untitled](https://user-images.githubusercontent.com/83864058/187872329-c1c0b358-558d-4f2b-81cb-daf5d1908f17.png)
    
    - KVO 사용이 가능함
    maxConcurrentOperationCount, Suspended, name key path사용 가능
    (현재 operations/operationCount는 iOS13부터 사용불가)
    ![Untitled](https://user-images.githubusercontent.com/83864058/187872554-08045018-f4d0-4807-b439-876feb0e587e.png)
    [Apple Developer Documentation](https://developer.apple.com/documentation/foundation/nsoperationqueue)
    
    - 취소, 중지 등의 작업을 할 수 있다.
    ![Untitled](https://user-images.githubusercontent.com/83864058/187872653-39ac0a60-3245-452a-9dd8-e678a1848e0a.png)
    [Apple Developer Documentation](https://developer.apple.com/documentation/foundation/nsoperation)
    
- GCD(Grand Central Dispatch)
    
    - DispatchQueue의 장점은 사용이 더 간단하고 작업을 실행하는데 훨씬 더 효율적임
    선입선출 데이터 구조, 대기열에 추가하는 작업은 항상 추가된 순서대로 시작
    Main Thread를 제외하고 작업을 실행하는데 사용하는 스레드에 대해 보장하지 않음?
    (스레드가 어떻게 관리되는지 알 수 없다는 뜻인가?)
    작업들을 동기/비동기로 실행하도록 할 수 있음
    [Apple Developer Documentation](https://developer.apple.com/documentation/dispatch/dispatchqueue)
    ![Untitled](https://user-images.githubusercontent.com/83864058/187872782-b1ce5280-4482-4cd1-b919-6d9019647fd6.png)
    ![Untitled](https://user-images.githubusercontent.com/83864058/187872821-5eec7bf7-ecc3-497e-b2d1-3abfc23dd9d1.png)
<br />
<br />

## GCD API 동작 방식과 필요성에 대해 설명하시오.
- 시스템에서 관리하는 디스패치 대기열에 작업을 제출하고 멀티코어 하드웨어에서 동시에 코드를 실행한다.
![Untitled](https://user-images.githubusercontent.com/83864058/187873111-2ef28bf3-dd50-43ce-b557-4b76d26528d5.png)

- 시스템에서 스레드를 관리해주기 직접 스레드를 생성하고 각 스레드에 작업을 할당하지 않아도 되는 간편함이 있다.
그리고 공식문서에도 나와있듯이 실행중인 모든 응용 프로그램의 요구사항을 더 잘 수용해 균형잡힌 방식으로 사용가능한 시스템 리소스와 일치시킴
![Untitled](https://user-images.githubusercontent.com/83864058/187873241-e51bd0d1-b72c-4f27-a25d-5da71a5350b6.png)
[Apple Developer Documentation](https://developer.apple.com/documentation/DISPATCH)
<br />
<br />

## Global DispatchQueue 의 Qos 에는 어떤 종류가 있는지, 각각 어떤 의미인지 설명하시오.
- 우선순위가 높은 작업은 우선순위가 낮은 작업보다 더 많은 리소스를 사용하여 더 빠르게 수행되기 때문에 일반적으로 더 많은 에너지가 필요하다.
작업을 예약할 때 시스템은 등급이 더 높은 작업에 우선순위를 지정
![Untitled](https://user-images.githubusercontent.com/83864058/187873761-f8db560b-8ddf-46d2-b773-5189a81b06b7.png)
![Untitled](https://user-images.githubusercontent.com/83864058/187873538-4224e34c-4e96-4a2d-b584-446a22df5509.png)
[Apple Developer Documentation](https://developer.apple.com/documentation/dispatch/dispatchqos)

1. **userInteractive**: 가장 높음, 사용자와 직접 상호작용을 하는 작업을 의미하기 때문에 사용자의 요청에 곧바로 응답이 있어야함(애니메이션 처리, 이벤트 처리, UI 업데이트 등의 경우) 
2. **userInitiated**: 사용자가 어떤 요청을 했을 때 그 결과를 곧바로 받아야하거나 사용자가 앱을 사용하는 것을 순간적으로 막기 위한 경우(사용자에게 보여줄 이메일 내용을 로드할 때 등)
3. **default**: 기본값으로 일반적인 작업의 경우입니다.
4. **utility**: 사용자가 앱을 계속 사용하지 않도록 막지 않는 작업에 사용할 수 있습니다.(사용자와 상호작용하지 않으면서 오랜시간동안 어떤 작업을 진행해야할 때)
5. **background**: 가장 낮은 우선순위를 가지는 qos이고, 앱이 백그라운드에서 실행중일 때 진행하는 작업에 이 qos를 사용할 수 있습니다.
<br />
<br />

## UIKit 클래스들을 다룰 때 꼭 처리해야하는 애플리케이션 쓰레드 이름은 무엇인가?
- MainThread
![Untitled](https://user-images.githubusercontent.com/83864058/187874074-14bee7b8-0c6c-4246-87bc-ec0e68a9c2f7.png)
[Apple Developer Documentation](https://developer.apple.com/documentation/uikit)
<br />
<br />

## DispatchQueue를 사용할 때 serial/concurrent Queue를 생성하는 방법
- DispatchQueue의 init의 attributes 파라미터의 값을 통해 설정이 가능하다.
생략하게 되면 해당 queue는 serial하게 동작하고 concurrent하게 동작하게 하고 싶다면 concurrent를 값으로 넣어주어 생성하면 된다.
[Apple Developer Documentation](https://developer.apple.com/documentation/dispatch/dispatchqueue/2300059-init)
![Untitled](https://user-images.githubusercontent.com/83864058/187874258-029cd4bf-2a29-40af-be25-68b42df00516.png)
<br />
<br />

## sync를 사용할 때 dead lock이 발생하는 경우는?
- main에 sync로 작업을 실행하는 경우
![Untitled](https://user-images.githubusercontent.com/83864058/187874510-6ec71aab-0a83-4fb9-b35b-1e7f80a80bbe.png)
[Apple Developer Documentation](https://developer.apple.com/documentation/dispatch/dispatchqueue)

- 현재 queue(같은 queue)에 sync로 작업을 실행하는 경우
![Untitled](https://user-images.githubusercontent.com/83864058/187874592-cdf752b5-31d4-4959-b908-f4a83d8b9192.png)
[Apple Developer Documentation](https://developer.apple.com/documentation/dispatch/dispatchqueue/1452870-sync)
<br />
<br />
<br />
