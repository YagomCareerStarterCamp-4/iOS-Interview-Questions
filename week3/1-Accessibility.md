# ❔ 질문
1. 앱이 foreground에 있을 때와 background에 있을 때 어떤 제약사항이 있나요? <br>
2. 앱이 In-Active 상태가 되는 시나리오를 설명하시오. <br>
3. App의 Not running, Inactive, Active, Background, Suspended에 대해 설명하시오. <br><br>

# ✏️ 작성자
- [제인, 숲재, 예하](#제인-숲재-예하)
- [고사리, 호랭이, 허황](#고사리-호랭이-허황)
<br />

---

# 제인, 숲재, 예하

## 1. 앱이 foreground에 있을 때와 background에 있을 때 어떤 제약사항이 있나요?

- foreground에는 제약사항이 없고, 시스템 리소스에 우선권을 갖고 있습니다.
- background의 경우 메모리와 시스템 리소스에 대한 우선권을 박탈당한 상태가 됩니다.
(아래의 상황 등 몇가지 예외사항 외에는 실행시간을 할당받지 못하게 됩니다.) 

<br><br>

## 1-1. 예외상황에는 어떤게 있나요?

![https://i.imgur.com/I7Oge4A.png](https://i.imgur.com/I7Oge4A.png)

- Audio, AirPlay, PIP(Picture in Picture)
- 사용자 위치 업데이트
- Voice over IP
- 외부 악세서리와의 통신
- 블루투스 LE(Low Energy)와 통신, 혹은 디바이스를 블루투스 LE 악세서리로 변환
- 서버에서의 정기적인 업데이트
- RemoteNotification(Apple Push Notification 지원)
- Background Processing

<br><br>

## 1-2. 앱이 background에 있을 때 메모리에 올라가 있나요?

- 메모리에 올라가 있고, 화면 밖에서 실행되고 있습니다.
- 앱이 background상태가 될 때 가능한 많은 메모리를 해제합니다.
![image](https://user-images.githubusercontent.com/65153742/186077640-4def93fa-7f02-44eb-b372-2f3117bffc55.png)

    
    
<br><br>

## 2. 앱이 In-Active 상태가 되는 시나리오를 설명하시오.

- 앱이 실행중이지만, 아직 아무런 이벤트를 받지 않은 상태입니다.
- 앱 시작 시 시스템은 앱을 Foreground로 전환하기 전에 In-Active상태에서 앱을 시작합니다.
- Foreground 상태에서 전화가 오거나, 잠그거나, 멀티태스킹 스크린 창으로 올리면 (Interrupted) In-Active상태가 됩니다.
<br />
<br />

## 3. App의 Not running, Inactive, Active, Background, Suspended에 대해 설명하시오.
- Not Running
    - 앱이 아직 실행 전인 상태
- Inactive
    - 앱이 실행되고 화면에 표시되는 상태이지만, 이벤트를 받지 못하는 상태
    - Interruption(잠금, 전화, 멀티태스킹 등)으로 발생하거나 background에서 active로 상태 변환 시 이 과정을 거친다. -> 전화가 일부로 가려짐
- Active
    - 앱이 foreground 상태이며, 이벤트를 받는 상태
- Background
    - 앱이 background 상태에서 실행되고 있는 상태
- Suspend
    - 앱이 background 상태에 있으며, 실행을 멈추고 있는 상태
<br />
<br />

## 3-1. UIScene의 state 종류에 대해 설명하시오.
`UIScene` 클래스 안에 `ActivationState` 라는 Enum 타입을 가지고 있다
```swift
    extension UIScene {

        @available(iOS 13.0, *)
        public enum ActivationState : Int {
            case unattached = -1
            case foregroundActive = 0
            case foregroundInactive = 1
            case background = 2
        }
    }

```
- **unattached**
    - `scene`은 처음에 **unattached 상태로 시작**되며 **시스템이 connection notification을 주기 전**까지는 계속 **이 상태를 유지**한다. 또한 scene은 유저가 app switcher로 부터 interface를 dismiss하거나 자원을 재요청 할 때 attached 상태로 다시 돌아오게 된다.
- **foregroundActive**
    - `scene`이 **foreground에서 돌아가고 있으며, 현재 event들을 받고 있는 상태**이다.active scene의 interface는 화면에 있으며 사용자에게 보여지게 된다.
- **forgroundInactive**
    - `scene`이 **foreground에서 돌아가고는 있지만 event를 받지는 않는다.**
    - `scene`이 **다른 상태로 전환되는 동안에 바로 이 foregroundInactive 상태를 통과**하게 된다.
    - 예를 들면, foregroundInactive 상태는 시스템 알람이 오거나 알람 창을 내리거나 app-switching 상태에 있는 경우가 있다.
- **background**
    - `scene`이 **스크린 위에서가 아닌 background에서 돌아가고 있는 상태**이다.
    - background scene은 보여지는 interface가 없다.
- **suspended**
    - 이 상태는 실제로 case 안에는 존재하지는 않는다.`scene`이 **background 상태에 있으며, 아무것도 실행되지 않는 상태**를 의미한다.

<br><br>

## 3-2. 앱이 메모리에서 언제 완전히 해제되나요?.

- 메모리 확보, 사용자의 종료 등의 이유로 suspended 상태에서 not running상태로 이동할 때
- applicationWillTerminate(_:) 메서드 이후

<br><br>

## 3-3. 다음 상황에서의 App LifeCycle State 변화를 설명하시오.

- 실행 중인 앱을 멀티태스킹을 사용하여 Background로 보낸 뒤 다시 멀티태스킹을 사용하여 해당 앱으로 돌아왔을 시
    - active -> inactive -> background -> inactive -> active
![image](https://user-images.githubusercontent.com/65153742/186077879-08312f8a-62f9-4bc6-b09a-f4c3dfcfc893.png)

    - active -> inactive -> background -> inactive -> active
- 사용 중이던 앱을 멀티태스킹 창을 띄워 종료하는 경우
    - sceneWillResignActive -> sceneDidDisconnect -> didDiscardSceneSessions -> applicationWillTerminate


<br><br>

## 3-4. 중요한 데이터를 언제 저장하는 것이 좋을까요?
- (저희가 작성한 답안) 앱을 quit하는 상황(=> background로 전환하는 상황)에서 불리는 sceneWillResignActive에 저장하는 것이 좋습니다. 왜냐하면 applicationWillTerminate(_:)의 경우 background를 지원하는 앱인 경우 background 상태에서 앱을 종료하면 불리지 않습니다.
- 참고
[Preparing Your UI to Run in the Background](https://developer.apple.com/documentation/uikit/app_and_environment/scenes/preparing_your_ui_to_run_in_the_background)
![image](https://user-images.githubusercontent.com/65153742/186078655-0245d761-1794-4376-8ed7-1ffae2de3fc8.png)
- 백그라운드 상태에 있는 앱을 종료하는 경우
    
    ![https://i.imgur.com/bEjlweg.png](https://i.imgur.com/bEjlweg.png)
    
- 실행 중인 앱을 바로 종료하는 경우
    
    ![https://i.imgur.com/v6zV4M0.png](https://i.imgur.com/v6zV4M0.png)


<br />
<br />
<br />

---

# 고사리, 호랭이, 허황
## 1. 앱이 foreground에 있을 때와 background에 있을 때 어떤 제약사항이 있나요? 
Foreground 는 애플리케이션이 클라이언트에게 보여지고 있는 상태로 메모리 및 시스템 리소스에 높은 우선 순위를 가지고 있다.

background 는 앱이 백그라운드에 있지만 여전히 실행되고 있는 상태를 말합니다. 제약사항으로는 가능한 가장 작은 메모리공간을 사용하도록 해야하며 사용자의 이벤트를 받기 어렵다는 특징이 있다.

일단 Background 상태가 되면 추가적인 실행 시간을 할당받지 않는데요, UIKit은 아래의 경우에만 Background 앱에 실행 시간을 할당해 준다고 합니다.

- Audio, AirPlay, PIP
- Location updates(GPS)
- 보이스 오버
- 외부 악세사리
- BLE (블루투스)
- Background Fetch
- 원격 노티피케이션
- Background Processing
![https://i.imgur.com/Ub4vjSA.png](https://i.imgur.com/Ub4vjSA.png)


<br />
<br />

## 2. 앱이 In-Active 상태가 되는 시나리오를 설명하시오.
- 앱이 실행 중이지만 이벤트를 받지 않는 상태
- 다른 상태로 넘어가기 전에 앱은 반드시 이 상태를 거침
- 전화나 메시지 같은 인터럽트가 발생하면 InActive 상태가 됨
- 미리알림 같은 특정 알림창이 화면을 덮어서 앱이 실질적으로 이벤트를 받지 못하는 상황이 여기에 해당함.

<br />
<br />

## 3. App의 Not running, Inactive, Active, Background, Suspended에 대해 설명하시오.
**1) Not Running**

- 앱이 실행되지 않은 상태
- 실행되고 난 후, 시스템은 UI가 화면에 보여야한다면 앱을 inactive 상태에, 아니라면 background 상태에 둔다.
- foreground로 시작할 때 시스템은 자동으로 앱을 active상태로 전환한다.

**2) Inactive - foreground**

- 앱이 foreground에서 실행중이지만 아무런 이벤트를 받지 않고 있는 상태

**3) Active - foreground**

- 앱이 foreground에서 실행중이며 이벤트를 받고 있는 상태

**4) Background - running**

- 앱이 background에 있으며 실행되는 코드가 있는 상태
- 대부분 앱은 suspended상태로 이행하는 도중에 일시적으로 이 상태에 진입하지만 파일 다운로드, 업로드 등 특정 시간동안 이 상태로 남아있게 되는 경우도 있음
- 시스템이 임의로 Background 상태의 앱을 Suspended 상태로 만든다

**5) Suspended - background**

- 앱이 background에 있지만 실행되는 코드가 없는 상태
- 메모리가 부족한 상황이 오면 iOS system은 foreground에 있는 앱의 여유 메모리 공간 확보를 위해 이 상태에 있는 앱들을 특별한 알림없이 정리할 수도 있음
<br />
<br />

## 꼬리 질문


### 개발자는 상태변화를 어떻게 컨트롤 할 수 있는가? 또 상태변화를 감지할 수 있는 변화는 무엇이 있는가? (예시 inactive -> Active)

- AppDelegate, SceneDelegate로 컨트롤 할 수 있다.
- 각각의 메서드에 코드를 작성하면 상태변화가 감지될 때마다 코드가 호출된다.

  - InActive <-> Active
  - InActive <-> Background
  - Background -> Suspended는 알 지 못한다.

<br><br>

### 사용해보거나 기억나는 앱 라이프 사이클 메서드가 있는가? 그렇다면 그 내부에서 어떤 동작들을 해줬나?

- 전체 메서드를 전부 외우고 있을 순 없음.
- iOS13을 기준으로, 코드로만 뷰를 구성할 때 스토리보드를 지우고 루트뷰컨트롤러를 등록해줘야한다. scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions)

<br><br>

### 델리게이트 패턴은 무엇인가?

- 델리게이트 패턴은 프로토콜을 활용해서 인터페이스만 구현한 후 일을 위임하는 패턴이다. 위임 받은 객체가 어떤 객체인지, 어떤 일을 수행하는지는 위임해준 객체가 알 필요는 없다.
- 테이블 뷰, 컬렉션 뷰 등 UI관련 이벤트가 발생했을 때 개발자가 처리하고 싶은 기능을 추가하고 싶을 때 주로 사용된다.

<br><br>

### 앱의 일부만 가리는 알람이나 전화가 왔을 때 어떤 일이 발생하는가?

- 화면 전체를 가리는 전화 등의 interrupt의 경우, active 상태에서 이벤트를 받을 수 없는 inactive 상태로 전환이 되고 이때 `applicationWillResignActive` 메서드가 호출된다.

- 그러나, 앱의 일부만 가리는 interrupt의 경우, 앱은 여전히 이벤트를 받을 수 있는 상태이며, 위의 케이스와는 다르게 active 상태를 유지하고 특별히 메서드를 호출하지 않는다.

<br><br>

### 앱 델리게이트와 씬 델리게이트의 차이는?

- iOS 13 이후 멀티 씬이 도입되면서 씬 델리게이트가 생겼다.

- iOS 13 이전의 `AppDelegate`가 App Life Cycle(launch, foregrounding, backgrounding)과 UI Event 처리를 모두 도맡았다면, 이후에는 `SceneDelegate`가 UI Event를 처리하는 책임을, `AppDelegate`가 App Life Cycle 컨트롤에 대한 책임을 나눠가지게 되었다.

- iOS 13 이후 AppDelegate가 하는 일
  1. 앱의 가장 중요한 데이터 구조를 초기화하는 것
  2. 앱의 scene을 환경설정(Configuration)하는 것
  3. 앱 밖에서 발생한 알림(배터리 부족, 다운로드 완료 등)에 대응하는 것
  4. 특정한 scenes, views, view controllers에 한정되지 않고 앱 자체를 타겟하는 이벤트에 대응하는 것.
  5. 애플 푸쉬 알림 서브스와 같이 실행시 요구되는 모든 서비스를 등록하는것.

- 현재 iOS에서는 멀티 씬을 사용하고 있지 않기 때문에 둘간의 차이는 크지 않지만 iPadOS를 개발하려면 신경써 줘야한다.

<br />
<br />
<br />
