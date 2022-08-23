# ❔ 질문
접근 제어자의 종류엔 어떤게 있는지 설명하시오.<br />
Codable에 대하여 설명하시오.<br />

# ✏️ 작성자
- [나무, 제인](#나무-제인)
- [예거, 고사리](#예거-고사리)
<br />

---

# 나무, 제인

## 접근 제어자의 종류엔 어떤게 있는지 설명하시오

- 접근 제어
    - 다른 소스 파일 및 모듈의 코드에서 코드의 일부에 대한 접근을 제한하는 것 (불필요한 접근 방지)
    - 특정 코드의 세부적인 구현을 감추고 딱 필요한 만큼만 공개해 다른 곳에서 사용할 수 있도록 한다.
    - 개별 타입 (class, struct, enum) 뿐 만 아니라 해당 타입에 속하는 프로퍼티, 메소드, 이니셜라이저 및 첨자 (subscripts) 에 대해 특정 접근 레벨을 지정할 수 있다.
    - 프로토콜은 전역 상수, 변수 및 함수처럼 특정 컨텍스트로 제한될 수 있다.
    - Swift 는 코드 내의 개체에 대해 **5가지 접근 레벨** **(open / public / internal / fileprivate / private)** 을 제공한다.
    - open -> public -> internal -> fileprivate -> private 순으로 더 범위가 제한적이다.
- 접근 레벨 원칙
    - 더 낮은 레벨을 가진 엔티티를 특정 엔티티에 선언해 사용할 수 없음
        - public변수는 internal, private, fileprivate 타입에서 정의될 수 없음
        - 함수는 함수의 파라미터 타입이나 리턴 값 타입보다 더 높은 접근 레벨을 가질 수 없음

<br />
<br />

## 꼬리 질문

### 모듈이 뭔지 설명해주세요.

모듈 및 소스 파일

Swift의 접근 제어 모델은 모듈과 소스 파일의 개념을 기반으로 합니다.

*모듈* 은 코드 배포 의 단일 단위입니다. 단일 단위로 빌드 및 배송되며 Swift의 `import`키워드 를 사용하여 다른 모듈에서 가져올 수 있는 프레임워크 또는 애플리케이션입니다 .

Xcode의 각 빌드 대상(예: 앱 번들 또는 프레임워크)은 Swift에서 별도의 모듈로 처리됩니다. 앱 코드의 측면을 독립형 프레임워크로 그룹화하면(아마도 여러 애플리케이션에서 해당 코드를 캡슐화하고 재사용하기 위해), 해당 프레임워크 내에서 정의한 모든 것은 앱 내에서 가져와서 사용할 때 별도의 모듈의 일부가 됩니다. 또는 다른 프레임워크 내에서 사용될 때.

*소스 파일* 은 모듈 내의 단일 Swift 소스 코드 파일입니다(사실상 앱 또는 프레임워크 내의 단일 파일). 별도의 소스 파일에 개별 유형을 정의하는 것이 일반적이지만 단일 소스 파일에는 여러 유형, 기능 등에 대한 정의가 포함될 수 있습니다.

<br />
<br />

### internal 타입은 프로퍼티로 public 프로퍼티를 가질 수 있나요?

아니요


<br />
<br />

### @testable 키워드를 사용하는 이유는 무엇인가요?

open, public의 경우에만 다른 모듈에서 접근이 가능한데, 유닛 테스트를 할때는 다른 모듈의 internal 엔티티에 접근할 수 있도록 해당 키워드를 사용한다. 

### 사용자 정의 타입의 프로퍼티, 메서드, 이니셜라이저, 서브스크립트 등의 접근 레벨은 명시를 하지 않았을 경우 기본적으로 어떻게 정의되나요?

타입의 접근 레벨을 따라간다.

private → private

fileprivate → fileprivate

<br />
<br />

### 항상 그런가요?

아니요

- public 사용자 정의 타입 안에 프로퍼티를 정의했을 때 프로퍼티의 기본 접근레벨이 무엇인가요?
    - internal이다.

> A public type defaults to having internal members, not public members. If you want a type member to be public, you must explicitly mark it as such. This requirement ensures that the public-facing API for a type is something you opt in to publishing, and avoids presenting the internal workings of a type as public API by mistake.
> 

<br />
<br />

### open 과 public의 차이는 무엇인가

> Open access applies only to classes and class members, and it differs from public access by allowing code outside the module to subclass and override, as discussed below in [Subclassing](https://docs.swift.org/swift-book/LanguageGuide/AccessControl.html#ID16). Marking a class as open explicitly indicates that you’ve considered the impact of code from other modules using that class as a superclass, and that you’ve designed your class’s code accordingly.
> 

open: 모듈 외부에서도 subclassing, override가 가능한 접근수준

public: 사용은 가능하지만, 모듈 외부에서 subclassing, override은 불가능한 접근수준

<br />
<br />

### private 변수나 함수를 extension에서 항상 사용이 가능한가?

→ 같은 소스파일 내에서만 가능

<br />
<br />

## 1-1. Codable이 무엇이고, 언제 사용하나요?

Encodable과 Decodable 프로토콜의 합성 프로토콜이며, JSON 및 프로퍼티 리스트 형식을 따르는 Data를 디코딩하거나, 반대로 인코딩하기 위해 커스텀 타입에 채택하여 사용합니다.

<br />
<br />

### 1-2. Codable을 채택하기 위한 조건을 설명해 주세요.

Codable을 채택하기 위해서는 해당 타입의 모든 프로퍼티가 Codable한 타입이어야 하는데, 

- standard library types like `[String](https://developer.apple.com/documentation/swift/string)`, `[Int](https://developer.apple.com/documentation/swift/int)`, and `[Double](https://developer.apple.com/documentation/swift/double)` and Foundation types like `[Date](https://developer.apple.com/documentation/foundation/date)`, `[Data](https://developer.apple.com/documentation/foundation/data)`, and `[URL](https://developer.apple.com/documentation/foundation/url)` 은 Codable한 타입입니다.
- Array, Dictionary, Optional 또한 내부 요소의 타입이 Codable이면 Codable합니다.
- 커스텀 타입 프로퍼티를 가진 경우에도 만약 그 타입이 Codable이면 Codable합니다.

<br />
<br />

### JSON이 뭔가요?

- JavaScript Object Notation라는 의미의 축약어로 데이터를 저장하거나 전송할 때 많이 사용되는 **경량의 DATA 교환 형식**
- Javascript에서 객체를 만들 때 사용하는 표현식을 의미한다.
- JSON 표현식은 사람과 기계 모두 이해하기 쉬우며 용량이 작아서, 최근에는 JSON이 XML을 대체해서 데이터 전송 등에 많이 사용한다.
- JSON은 데이터 포맷일 뿐이며 어떠한 통신 방법도, 프로그래밍 문법도 아닌 단순히 데이터를 표시하는 표현 방법일 뿐이다.

<br />
<br />

### 디코딩할 데이터에 불필요한 값이 있다면 어떻게 해야 할까요?

→ 해당 불필요한 값을 제외하고 타입을 정의하여 디코딩하면 됩니다.

```swift
let json =
"""
{
    "name": "식빵",
    "family": "웰시코기",
    "age": 1,
    "weight": 2.14
}
"""

struct Dog: Codable {
    let name: String
    let family: String
    let age: Int
    var weight: Double = 10.0

    enum CodingKeys: CodingKey {
        case name, family, age
    }
}

print(try! JSONDecoder().decode(Dog.self, from: json.data(using: .utf8)!))
```

<br />
<br />

---

# 예거, 고사리

### 스위프트에서 접근 제어(Access Control)는 어떤 역할을 하나요?

- 소스 파일과 파일 간, 혹은 모듈 간의 **코드 접근을 제한(restrict) 하는 것**을 말합니다.
- 이를 통해 디테일한 코드 구현을 **은닉화**할 수 있고, 동시에 **개발자의 의도대로 코드의 인터페이스를 설정**할 수 있습니다.
- 클래스, 구조체, 열거형과 같은 개별 타입들, 그리고 프로퍼티, 메서드, 이니셜라이저에도 설정할 수 있습니다.
- 출처 → [스위프트 공식 문서](https://docs.swift.org/swift-book/LanguageGuide/AccessControl.html#ID4)

<br />
<br />

### 접근 제어자의 종류엔 어떤 게 있을까요?

- 가장 제한되는 것부터 순서대로 **private → fileprivate → internal → public → open** 이렇게 5가지가 있습니다.
    - **private** → 선언한 scope 내부에서만 쓸 수 있습니다.
    예를 들어, 특정 구조체 내에서 private 메서드를 만들었다면, 그 타입 선언 내부에서만 쓸 수 있습니다. 구조체의 인스턴스를 생성하더라도 호출할 수 없습니다.
    - **fileprivate** → 선언한 스위프트 파일 내에서만 사용할 수 있습니다.
    - **internal** → 디폴트 접근 수준입니다. **모듈** 내에서만 사용할 수 있습니다.
    접근제어를 명시하지 않으면, 기본적으로 internal 이 걸려있습니다.
    - **public** → 선언한 외부 모듈에서 접근 가능해집니다. 하지만 외부 모듈에서 **오버라이드와 서브클래싱이 불가능**합니다.
    public 부터는 single-target 앱 수준을 벗어난, **프레임워크나 라이브러리 개발 영역**입니다.
    - **open** → public 처럼, 외부 모듈에서 접근 가능하면서, 오버라이드와 서브클래싱도 가능합니다. 완전 열려 있는 것이죠. 😃
    단, open 은 상속 가능하거나, 오버라이드 가능한 **클래스(overridable class) 에만** 붙일 수 있습니다. 구조체나 열거형, 프로토콜 앞에 open 붙이면 컴파일 에러 납니다.
        - Only classes and overridable class members can be declared 'open';
    - (얘기할 거) 타입 “정의"가 맞나.. “선언"이 맞나? 무슨 표현이 좋을까? 🤔
        - (고사리) “구현" 은 ‘기능' → 추상화된 거를 표현할 때만 쓴다. 개별적인 타입 보다는, 종합적인 기능을 언급할 때 표현하는 편이다.
        - ex) 커밋 메시지 올릴 때 → **“feat: 네트워크 모델 타입 OO”**
        - 이 타입을 정의했습니다. → definition → 야곰 질문 게시판에 올려볼까 고민도 듦.
        - 이 타입을 선언했습니다. → declaration. → 문서에서 많이 본 듯한ㅋㅋ

![image](https://user-images.githubusercontent.com/75905803/186046975-62b25518-af9d-47ff-9ac0-f5c4a3c70a79.png)

<br />
<br />

### 프로토콜에도 접근제어를 설정할 수 있을까요?

- 프로토콜도 타입이기 때문에, 접근제어 설정이 가능합니다.
- 기본적으로 프로토콜도 정의하면 internal 접근제어가 설정되어 있는 거고, private 하게 걸 수도 있습니다. (private extension 이 가능하듯이)
    - [참고] private extension → fileprivate 처럼 작동한다. 스위프트 4 부터 작동한다.
    - private protocol → 얘도 fileprivate 처럼 작동한다.
- 단 open 은 클래스에만 걸 수 있으니까 public 까지만 가능하고, 이때 프로토콜 내부에서 정의한 프로퍼티도 전부 같은 수준이거나 그 이상의 접근제어가 걸려있어야 합니다.
- 프로토콜 끼리 상속(Protocol Inheritance) 하기 위해선, 서로가 같은 접근수준 이어야 합니다.
    - 확실하지 않음, 프로토콜 부분 더 읽어보기!

![image](https://user-images.githubusercontent.com/75905803/186046999-8f06a205-5ebb-4a57-a9b6-ecc2be93cbd1.png)

<br />
<br />

### Codable 에 대해 설명해주세요.

- Codable 은 프로토콜 2개(**Decodable & Encodable**)이 묶인 **typealias** 입니다. [공식 문서](https://developer.apple.com/documentation/swift/codable)
- 특정 타입에 Codable 프로토콜 채택하면, 그 타입은 인코딩, 디코딩이 둘 다 가능해집니다. 단, 해당 타입이 갖는 모든 프로퍼티 또한 Codable 해야만 합니다.
- String, Int 같은 기본 타입들은 전부 Codable 하지만, 개발자가 만든 커스텀 타입의 프로퍼티가 포함되어 있다면, 그 커스텀 타입도 Codable 해야만 컴파일 됩니다.

<br />
<br />

### 인코딩, 디코딩이 뭔데요?

- 데이터를 **외부(모듈 외)와 소통**할 수 있게 JSON 과 같은 표현 방식으로 변환하는 것입니다.
- 예를 들어, 어떤 모델 타입을 내장된 JSONEncoder 클래스의 encode 메서드를 통해 **JSON data** 로 만들어낼 수 있습니다. (바이너리)
- 디코딩은 → 외부의 JSON data 를 내부에서 정의해둔 모델 타입으로 변환하는 것입니다.
- 외부 JSON 파일을 디코딩해서 → 내부에서 정의해둔 타입으로 변환할 수 있습니다.

<br />
<br />

### 인코딩, 디코딩에서 쓰이는 CodingKeys 열거형에 대해 설명해주세요.

- 클라이언트와 서버가 JSON 과 같은 외부 표현(external representation)을 통해 데이터를 주고 받을 때, 둘의 네이밍 방식이 다를 수 있잖아요?
- 이때, 이 서로 다른 **key name** 을 호환 가능하게(compatiblity) 만들기 위해 사용되는 특별한 열거형입니다.
