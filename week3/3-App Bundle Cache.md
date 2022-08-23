# ❔ 질문
1. App Bundle의 구조와 역할에 대해 설명하시오. <br>
2. NSCache와 딕셔너리로 캐시를 구성했을때의 차이를 설명하시오. <br><br>

# ✏️ 작성자
- [애플사이다, 아리](#애플사이다-아리)
- [줄라이, 예거](#줄라이-예거)
<br />

---

# 애플사이다, 아리
## 1. App Bundle의 구조와 역할에 대해 설명하시오.
키워드: 실행가능한 코드 관련, 리소스, 파일 시스템, 디렉토리 모음

- 번들이란 실행 가능한 코드와 관련 리소스(앱 아이콘, 이미지 등)을 한 공간에 묶는 파일 시스템에 있는 디렉토리 모음입니다.
- 즉 번들이란 실행가능한 코드와 코드에 의해 사용되는 리소스를 갖는 standardized, hierarchical 디렉토리 구조입니다.
- 애플리케이션 번들은 개발자들에 의해 흔히 생성되는 가장 흔한 번들입니다.
    - 앱이 정상적으로 작동하기 위해 필요한 모든 것들이 저장됩니다.
    - 플랫폼에 따라 상세한 구조는 다르지만 번들을 사용하는 방법은 동일합니다.
<br />
<br />

## 1-1. Bundle 구조에 꼭 포함되어야 하는 것들은 어떤 것들이 있나요? 
- Info.plist, Executable 이 두가지는 필수로 있어야 합니다.
<br />
<br />

## 1-2. App Bundle을 사용해본 경험이 있나요?
- Asset에 이미지를 넣은 경우, UIImage에 init(named: “image1”) 형태로 간단히 불러온다.
- Bundle에 Mock 데이터로 JSON 파일을 넣은 경우, bundle path를 통해 불러온다.
- info.plist에서 기기의 지원방향을 설정...
- 지역화 기능 구현하는 경우, Localization 이라는 파일을 만들고, 그 안에 localize할 데이터를 입력한다. 이때도 번들로 접근
<br />
<br />

## 1-3. Bundle과 Package는 같은 의미인가요?
- 둘의 개념을 혼용해서 사용하는 경우가 많은데, 많은 Bundle이 Package이기도 하기 때문입니다. application 같은 경우는 Finder에서 사용자에게 단일한 파일로 노출되는 Package이면서, 실행 코드와 리소스를 포함하여 Bundle이기도 한 대표적인 예시라고 볼 수 있다. <br>

### [App Bundle의 파일 유형]

- Info.plist `필수`
응용 프로그램에 대한 구성 정보를 포함하는 구조화된 파일. 시스템은 이 파일에 존재에 의존하여 애플리케이션 및 모든 관련 파일에 대한 관련 정보를 식별함
- Executable `필수`
모든 응용 프로그램에는 실행 파일이 있어야 함. 이 파일에는 애플리케이션의 기본 진입점과 애플리케이션 대상에 정적으로 링크된 모든 코드가 포함되어있음
- Resource files
애플리케이션의 실행 파일 외부에 있는 데이터 파일. 일반적으로 이미지, 아이콘, 사운드, nib 파일, 문자열 파일, 구성 파일(configuration files), 데이터 파일 등으로 구성됨. 대부분의 리소스 파일은 특정 언어 또는 지역에 대해 현지화하거나 모든 지역에서 공유할 수 있음
- Other support files
iOS 애플리케이션 번들에 사용자 정의 프레임워크 또는 플러그인은 포함할 수 없음

### [iOS App Bundle의 구조]

- MyApp `필수`
애플리케이션의 코드를 포함하는 실행 파일
- 응용 프로그램 아이콘 `필수/권장`
- Info.plist `필수`
번들 ID, 버전 번호 및 앱 표시 이름과 같은 응용 프로그램의 구성 정보가 포함되어 있음
- Launch images `권장`
- MainWindow.nib `권장`
응용 프로그램 시작 시 로드 할 기본 인터페이스 객체가 포함되어 있음. 일반적으로 nib 파일에는 응용 프로그램의 기본 창 객체와 응용 프로그램 delegate 객체의 인스턴스가 포함됨
- Settings.bundle
설정 애플리케이션에 추가하려는 애플리케이션 별 환경 설정을 포함하는 특수 유형의 플러그인
- 사용자 지정 리소스 파일
지역화되지 않은 리소스는 최상위 디렉토리에 배치되고 지역화된 리소스는 애플리케이션 번들의 언어별 하위 디렉토리에 배치
### 참고링크
  - [https://melod-it.gitbook.io/sagwa/documentation-archive/bundle-programming-guide/bundle-structures](https://melod-it.gitbook.io/sagwa/documentation-archive/bundle-programming-guide/bundle-structures)
  - [https://github.com/jwonyLee/TIL/blob/master/iOS/Interview/AppBundle.md](https://github.com/jwonyLee/TIL/blob/master/iOS/Interview/AppBundle.md)
  - [https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFBundles/BundleTypes/BundleTypes.html#//apple_ref/doc/uid/10000123i-CH101-SW1](https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFBundles/BundleTypes/BundleTypes.html#//apple_ref/doc/uid/10000123i-CH101-SW1)

<br><br>

## 2. NSCache와 딕셔너리로 캐시를 구성했을때의 차이를 설명하시오.
키워드: 메모리 관리, 쓰레드 세이프, 리테인 카운트 증가

- 딕셔너리는 메모리가 부족하면 값을 삭제하는 코드를 별도로 작성해주어야 합니다.
- 하지만 NSCache는 자동 제거 정책을 가지고 있어, 메모리가 자동적으로 관리됩니다.
- 또한 NSCache는 딕셔너리와 다르게 Thread-safe 합니다. 데이터를 쓸 때마다 lock을 걸어줄 필요가 없습니다.
    
    > You can add, remove, and query items in the cache from different threads without having to lock the cache yourself.
    > 
- 딕셔너리는 Key에서 사용되는 모든 객체가 NSCopying 프로토콜을 지원하기 때문에 Key값을 복사하지만 NSCache는 retain 카운트만 증가시킵니다. 따라서 복사를 지원하지 않는 객체까지 포용합니다.

<br><br>

## 2-1. NSCache는 무엇인가요?

- 저장 컨테이너로 리소스가 부족할 때 제거될 수 있는 Key-Value 쌍을 저장하고, 메모리 관리가 기본적으로 제공됩니다. 다른 앱에서 메모리를 더 사용하려고 하면 자동으로 캐시 되어 있던 데이터를 지우고 메모리를 해제합니다.

<br><br>

## 2-2. 캐시를 처리하는 이유는 무엇인가요?
- 고용량, 고화질 이미지를 계속해서 다운로드하면 사용자의 네트워크 리소스를 계속 소모하게 되며, 다운로드가 완료되는데까지 오랜 시간이 걸리게 됩니다. 그래서 다운받은 이미지를 캐싱해서 저장해두고 그 이미지가 사용되면 리소스 소모 없이 빠르게 보여줄 수 있게 됩니다.


<br><br>

## 2-3. NSCache의 자동 제거 정책에 대해서 설명해주세요.
- 자동 제거 정책은 totalCostLimit 혹은 countLimit을 설정하여 수동적으로 관리할 수 있게 되어있습니다. 여기에 NSCache에는 setObject(__:forkey:cost:) 메소드를 이용해 개발자가 임의로 cost를 부여하여 삭제의 우선순위를 정할 수 있습니다.

<br><br>

## 2-4. 메모리 캐시와 디스크 캐시가 뭔가요?
캐시 : 리소스를 매번 새롭게 다운받을 필요 없이 어떤 공간에 임시로 저장해두는 것 (ex. 로그인 정보, 첫번쨰 페이지 내용, 썸네일/이미지 등)

- 메모리 캐시 : 애플리케이션의 메모리 영역의 일부분을 Caching에 사용하는 것입니다. 그 말인즉 애플리케이션이 종료되어 메모리에서 해제되면 이 영역에 있던 리소스들은 OS에 반환되면서 Memory Caching되어 있던 리소스들은 사라지게 됩니다.
- 디스크 캐시 : Disk Caching은 데이터를 파일 형태로 디스크에 저장하는 것입니다. Disk Caching이 반복적으로 발생하면 애플리케이션이 차지하는 용량이 커지지만, 앱을 껐다 켠다고 해서 데이터가 사라지지는 않습니다. Disk Caching의 가장 가까운 예로는 카카오톡
- L1 등 캐시 메모리 : 메모리와 CPU 중간에서 임시로 데이터를 저장 (CPU가 RAM보다 너무 처리속도가 빨라서 발생하는 병목현상을 방지하기 위해 RAM의 데이터를 미리 CPU에 옮겨둠)하여 컴퓨터 성능이 향상됨5년 또는 10년 후 나의 모습
- 참고 링크
  - [https://developer.apple.com/documentation/foundation/nscache](https://developer.apple.com/documentation/foundation/nscache)


<br />
<br />
<br />

---

# 줄라이, 예거
## 1. App Bundle의 구조와 역할에 대해 설명하시오.
- 앱 번들은 앱을 실행시키기 위한 코드와 리소스(이미지, 사운드 등)를 한 공간에 모아둔 디렉토리입니다.
    
    > A representation of the code and resources stored in a bundle directory on disk.
    > 
- **.app 확장자**로 끝나는 패키지가 **메인 번들**입니다.
- 메인 번들 안에는 실행 가능한 코드(executable code)와 images, sound, nib 파일과 같은 리소스와 info.plist 가 들어있습니다.
- iOS 는 앱이 모두 SandBox 화 되어있다. 앱 규모의 접근제어라고 볼 수 있다. 앱의 외부 데이터(연락처, 사진첩 등)에 접근하려면 접근 권한 허용이 필요합니다.
    - [제드 블로그 - Bundle](https://zeddios.tistory.com/1309)
    - [공식문서 - Bundle Structures](https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFBundles/BundleTypes/BundleTypes.html#//apple_ref/doc/uid/10000123i-CH101-SW1)
    - [iOS) iOS 앱 번들 구조와 샌드박스 방식](https://sihyungyou.github.io/iOS-app-bundle/)
<br />
<br />


## 1-1. Info.plist 는 뭐하는 파일인가요?
- 앱의 **필수 설정 정보(configuration)**가 저장된 XML 파일입니다. 프로젝트를 생성하면 자동으로 생성됩니다. 반드시 포함되어야 합니다.
- 이 파일 안에는 번들 identifier, 배포 버전 등 앱의 필수 설정 정보가 들어있습니다.
- UIKit 에서는 Xcode 상의 프로젝트 네비게이터에서 바로 Info.plist 파일을 찾을 수 있지만, 만약 **SwiftUI** 로 프로젝트를 만들면 Info.plist 파일이 보이지 않는데요, TARGETS 내의 [Info] 탭에 내재화되어 있습니다.
<br />
<br />

## 2. NSCache와 딕셔너리로 캐시를 구성했을때의 차이를 설명하시오.
- NSCache 는 디바이스의 메모리가 부족해지면 **자동으로 캐시된 데이터를 삭제**합니다. 그리고 이러한 삭제 정책을 개발자가 선택할 수 있습니다. 디바이스의 메모리를 너무 많이 사용하지 않기 위한 다양한 auto-eviction policies 가 존재합니다.
    - 어떤 정책이 있는지 까지 알아보기. 2개 정도 있다.
- NSCache 는 **Thread-safe** 합니다. 다른 스레드에서 접근해, 안전하게 캐시의 항목을 추가하거나 삭제할 수 있습니다.
    
    > Unlike an `[NSMutableDictionary](https://developer.apple.com/documentation/foundation/nsmutabledictionary)` object, a cache does not copy the key objects that are put into it.
    > 
- 딕셔너리(NSMutableDictionary)로 캐시를 구성해본 적은 없지만, 자동화된 캐시 삭제 기능이 없고, Thread-safe 하지 않습니다.
- 마지막으로, NSCache 는 저장된 Key 객체(key objects)를 복사하지 않지만, 딕셔너리는 Key 가 변경되면, 기존의 Key 가 변경되는 게 아니라, 새롭게 복사된 Key 가 새로 생성됩니다.
- 이렇게 동작하는 이유는, NSCache 는 ‘복사(NSCopying 프로토콜)’를 지원하지 않는 객체와도 동작할 수 있기 때문입니다.
    - [https://developer.apple.com/documentation/foundation/nscache](https://developer.apple.com/documentation/foundation/nscache)
    - [https://aroundck.tistory.com/4717](https://aroundck.tistory.com/4717)
    - [https://dev200ok.blogspot.com/2021/11/dictionary-nscache-thread-safe.html](https://dev200ok.blogspot.com/2021/11/dictionary-nscache-thread-safe.html)
    - [https://jryoun1.github.io/swift/Cache/](https://jryoun1.github.io/swift/Cache/)
    - [https://inuplace.tistory.com/1050](https://inuplace.tistory.com/1050)

<br><br>

## 2-1. Cache 기능이란 무엇이고 왜 사용하나요?
- 자주 사용하는 데이터를 메모리나 디스크에 미리 복사해두고, 해당 데이터가 필요할 때, 바로 꺼내 사용하는 것입니다.
- 대표적인 예로는, 다운받는 데에 시간이 좀 걸리는 이미지가 있다고 할 때, 그 이미지를 캐시해두고, 필요할 때 재사용하면 **앱의 사용성**을 끌어올릴 수 있고, 불필요하게 반복적인 통신을 할 필요가 없어집니다.
- 스위프트에서는 **NSCache** 라는 메모리 캐싱 클래스를 제공합니다.

<br><br>

## 2-2. 캐시정책은 무엇이 있을까요? 
(내용없음)

<br />
<br />

## 2-3. Thread-safe 란 무엇인가요?
- **멀티-스레딩 환경**에서는 2개 이상의 Thread 가 하나의 **공유 자원**에 접근해서, 읽고/쓰는 작업을 동시에 수행하는, Thread 간의 **Race Condition** 이 발생할 수 있습니다.
- Race Condition 이 발생하면, 공유 자원에 대한 접근이 어떤 순서로 이루어졌는 지에 따라, 실행 결과가 매번 달라질 수 있습니다.
- 이런 Race Condition 을 예방할 수 있게 만들어진 코드가 **Thread-safe** 하다고 말할 수 있습니다.
    - [https://sihyungyou.github.io/iOS-race-condition-in-swift/](https://sihyungyou.github.io/iOS-race-condition-in-swift/)

<br><br>

## 2-4. 경험 상, 같이 일하기 힘든 유형의 개발자를 만나보신 적 있나요? 어떤 이유에서 였나요?
(개인답변)

<br />
<br />
<br />
