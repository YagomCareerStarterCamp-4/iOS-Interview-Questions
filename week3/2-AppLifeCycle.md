# ❔ 질문
1. 실제 디바이스가 없을 경우 개발 환경에서 할 수 있는 것과 없는 것을 설명하시오. <br>
2. 다크모드를 지원하는 방법에 대해 설명하시오. <br><br>

# ✏️ 작성자
- [지성, 아샌, 줄라이](#지성-아샌-줄라이)
- [릴리, 제리](#릴리-제리)
<br />

---

# 지성, 아샌, 줄라이
## 1. 실제 디바이스가 없을 경우 개발 환경에서 할 수 있는 것과 없는 것을 설명하시오.
- 실제 디바이스가 없을 경우 하드웨어 부분의 테스트가 어려울 수 있다.
- 예를들면 여러 센서를 테스트하지 못하고, 카메라, 전화 등의 기능을 지원하지 않는다.
- 하지만 FaceID의 경우 직접적인 얼굴인식 기능을 테스트하진 못하지만, 성공/실패의 경우를 설정하여 테스트를 진행할 수 있다.
- ARKit도 카메라가 없으니 지원하지 못함
- 또한 로컬푸시의 경우 실기기가 없어도 테스트할 수 있지만, APNs를 사용한 리모트 푸시의 경우 실기기의 identifier가 필요하기에 테스트하지 못한다.
-[https://developer.apple.com/library/archive/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator/TestingontheiOSSimulator.html](https://developer.apple.com/library/archive/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator/TestingontheiOSSimulator.html)

<br>

- **불가능한 것**
    - 하드웨어
        - 광 센서
        - 기압계
        - 블루투스
        - 카메라
        - 모션지원(가속도계, 자이로스코프)
        - 근접센서
    - 프레임워크
        - ARKit
        - External Accessory
        - HomeKit
        - IOSurface
        - Media Player
        - Message UI
    - API
        - Receiving and sending Apple push notifications.
        - The `UIBackgroundModes` key.
        - The `UIVideoEditorController` class in `UIKit`.
        - Support for Handoff.
- **가능한 것**
    - 시뮬레이터로 데이터 전송
    - 위치, 경로 설정
    - 메모리 경고 테스트
    - 아이틀라우드 테스트
    - Touch ID / Face ID 인증 테스트
    - Apple Pay 테스트
    - 통화 중 상태 표시줄 테스트
    - iOS에 연결된 외부 디스플레이 시뮬레이션(ex: CarPlay)
- **경험했던 것**
    
    > DateFormatter의 TimeZone과 Locale의 속성 중 autoupdatingCurrent와 current의 차이를 확인하기 위해 simulator의 설정에서 위치나 시간을 변경한 경우 적용이 되지 않음
    실제 기기에서 앱을 빌드하고 설정을 변경하여 두 차이점을 확인할 수 있었음
    시간대나 지역 설정도 simulator에서 정상적으로 테스트 할 수 없다고 판단함
    >

[Differences between simulated and physical devices](https://help.apple.com/simulator/mac/current/#/devb0244142d)

<br />
<br />

## 2. 다크모드를 지원하는 방법에 대해 설명하시오.

UI Components의 색을 애플에서 지원하는 systemColor를 사용하면 별도의 설정없이 다크모드를 지원할 수 있다.

색을 커스텀하게 설정하려면 Asset의 ColorSet을 설정하여 라이트모드, 다크모드의 색을 지정하여 사용할 수 있다.

또한 userInterfaceStyle에 따라 현재 앱의 다크모드 여부를 확인하여 코드로 색을 지정해줄 수 있다.

- UIColor의 UI ElementColors에 포함되어있는 색상을 사용
    - 따로 명시되어 있지않다면 UIColor 객체를 사용하면 자동적으로 다크모드 변화에 적용됨
    - [Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uicolor/ui_element_colors)
    
- Asset에 Color Set을 추가하여 다크/라이트 모드에 따라 색상을 설정할 수 있음
    - [Apple Developer Documentation](https://developer.apple.com/documentation/uikit/appearance_customization/supporting_dark_mode_in_your_interface)
    
- 코드로 현재 인터페이스 스타일을 확인해서 각 스타일별 설정을 할 수 있음
    - UITraitCollection.UIUserInterfaceStyle == .light / .dark로 상태에 따른 분기가능
    - [Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uiuserinterfacestyle)

<br />
<br />

## 꼬리질문
### 텍스트의 크기를 사용자의 설정에 따라 변환할 수 있는 방법
- Apple이 제공하는 기본 글꼴 사용

  - ![image](https://user-images.githubusercontent.com/65153742/186081074-010c039c-06d2-4e76-b627-b120c9a74dea.png)

  - [Typography - Visual Design - iOS - Human Interface Guidelines - Apple Developer](https://developer.apple.com/design/human-interface-guidelines/foundations/typography/))

- 커스텀 폰트를 사용하는 경우 프로젝트 내부 .ttf / .otf 파일을 추가하여 사용가능
    - 크기에 따라 설정된 폰트를 사용해야 하고 해당 폰트에 dynamic type을 적용해주어야 함
      ```swift
      guard let customFont = UIFont(name: "CustomFont-Light", size: UIFont.labelFontSize) else {
          fatalError("""
              Failed to load the "CustomFont-Light" font.
              Make sure the font file is included in the project and the font name is spelled correctly.
              """
          )
      }
      label.font = UIFontMetrics(forTextStyle: .headline).scaledFont(for: customFont)
      label.adjustsFontForContentSizeCategory = true
      ```
     - [Apple Developer Documentation](https://developer.apple.com/documentation/uikit/text_display_and_fonts/adding_a_custom_font_to_your_app)

     - [Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uifont/scaling_fonts_automatically)

<br><br>
      
      
## VoiceOver를 따로 설정해주려면 어떻게 해야하는지?
1. 스토리보드에서 설정
    
    ![image](https://user-images.githubusercontent.com/65153742/186081679-e813cd1a-ecc2-4b74-adba-b80e1891c120.png)
    
2. Code로 설정
    
    ```swift
    score.isAccessibilityElement = true
    score.accessibilityLabel = "score: \(currentScore)"
    score.accessibilityHint = "Your current score"
    ```
    [Supporting VoiceOver in Your App_Apple Developer Documentation](https://developer.apple.com/documentation/accessibility/supporting_voiceover_in_your_app)

<br>

타겟 → [https://developer.apple.com/library/archive/featuredarticles/XcodeConcepts/Concept-Targets.html#//apple_ref/doc/uid/TP40009328-CH4-SW1](https://developer.apple.com/library/archive/featuredarticles/XcodeConcepts/Concept-Targets.html#//apple_ref/doc/uid/TP40009328-CH4-SW1) 

스킴 → [https://developer.apple.com/library/archive/featuredarticles/XcodeConcepts/Concept-Schemes.html](https://developer.apple.com/library/archive/featuredarticles/XcodeConcepts/Concept-Schemes.html)


<br />
<br />
<br />

---

# 릴리, 제리
## 1. 실제 디바이스가 없을 경우 개발 환경에서 할 수 있는 것과 없는 것을 설명하시오.
= 시뮬레이터로 개발환경에서 할 수 있는 것과 없는 것은? <br>
= 시뮬레이터의 한계 <br>
할 수 있는 것과 없는 것은 시뮬레이터와 기기의 하드웨어적, API적 차이에서 기인함 

<br><br>

### 🙅‍♀️ 할 수 없는 것

---

#### a. 성능, 메모리 사용량, 네트워킹 스피드 테스트

- 실제 디바이스에서 테스트 하는 것이 정확함. 왜냐하면 시뮬레이터는 맥(컴퓨터)의 자원(CPU, memory, networking connection)을 사용하기 때문

#### b. 사용자 테스트

시뮬레이터의 키보드와 터치사용으로는 앱의 실제 사용 경험을 재현하기 어려움

#### c. 디스플레이 정확한 확인 불가

해상도, pixel per point, 색 체계가 맥과 다름

#### d. 시뮬레이터에서 지원하지 않는 하드웨어 사용에 대한 테스트

- 블루투스
- 카메라
- motion 관련 (accelerometer and gyroscope)
- 기압계
- 밝기 감지 센서
- 시리 제외한 오디오 인풋

#### e. 시뮬레이터에서 지원하지 않는 프레임워크의 API 호출, 테스트

- ARKit, HomeKit, IOSurfaces, MediaPlayer, MessageUI 프레임워크 지원X
- ~~푸시 노티피케이션 전송, 수신 (?)~~
    
    [https://www.avanderlee.com/workflow/testing-push-notifications-ios-simulator/](https://www.avanderlee.com/workflow/testing-push-notifications-ios-simulator/)
    
- The `UIBackgroundModes` key.
- The `UIVideoEditorController` class in `UIKit`.
- Support for Handoff.

---

- Metal, OpenGL ES (그래픽 및 연산 API, GPU와 소통)
    - iOS의 그래픽 프로세서와 시뮬레이터의 것은 다름

#### f. 앱 라이프 사이클 관련?

- **Simulator suspends background apps** and processes on iOS 11 and later, tvOS 11 and later, and watchOS 4 and later, however the suspended process can be resumed by the debugger. 

<br><br>

### 🙆🏻‍♀️ 할 수 있는 것

---

할 수 없는 것 빼고 다 가능함

- App accessibility (접근성)
- Localization (지역화)
- Webapp
- iCloud
- 메모리 워닝
- 애플 페이
- debugging log 볼 수 있음

<br />
<br />

## 1-1. IOS 개발하면서 시뮬레이터에 관련된 인상깊은 경험이 있으신가요?
시뮬레이터를 사용했던 프로젝트 경험을 설명하는 대처능력을 보았습니다.

### 📙 출처

---

- 공식문서(오래됨)

  - [Testing and Debugging in Simulator](https://developer.apple.com/library/archive/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator/TestingontheiOSSimulator.html)

- Simulator > Help > Simulator Help
    
  - [Differences between simulated and physical devices](https://www.notion.so/86d31352135a4d48951bc8c55cfa2a86)
<br />
<br />


## 2. 다크모드를 지원하는 방법에 대해 설명하세요 
> 다크 모드일 때 사용할 색상, image를 별도로 설정할 수 있습니다.

<br>

### **시스템이 제공하는 color사용하는 방법**

1. system color
    
    기본적으로 다크모드 지원
    
    ex. `.UIColor.systemPink`
    
2. dynamic system color
    
    색상의 이름을 색상에 대한 설명이 아닌, 사용되는 목적으로 이름을 지어주는 것.
    
    ex. `.UIColor.systemBackground`
    
    iOS13 이상 부터 지원
    

---

### **custom하게 대응하는 방법**

1. Asset Catalog
  - 각 모드에 대응하는 색상 또는 이미지 설정 가능
    - color set
    - ![image](https://user-images.githubusercontent.com/65153742/186082978-6836ba88-cc94-455f-80d6-47d8680336f5.png)
      ```swift
      // iOS
      let aColor = UIColor(named: "customControlColor")

      ```
     - buttons, image views, and custom views and controls에서 이미지가 사용됨. 따라서 각 모드에서 잘 보이는 이미지 제공하기 위해 image asset을 만들어 사용할 수 있음
     - [https://developer.apple.com/documentation/uikit/uiimage/providing_images_for_different_appearances](https://developer.apple.com/documentation/uikit/uiimage/providing_images_for_different_appearances)

2. `UITraitCollection.userInterfaceStyle` 에 따른 커스텀 컬러 생성
    
    - [Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uitraitenvironment/1623516-traitcollectiondidchange)
    
    - system appearance가 변경될 때마다 → traitEnvironment를 변경 → 아래 메서드를 호출, 시스템이 window와 view를 redraw
<br />
<br />

## 2-2. UITraitCollection이 어떤 역할을 하나요?
- IOS 인터페이스 환경을 나타내는 traint의 모음으로 가로, 세로 또는 디스플레이 크기, DynamicType, InterfaceStyle(다크모드), 3D Touch 의 특성들을 나타낼 수 있습니다.
<br />
<br />
<br />
