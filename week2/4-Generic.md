# ❔ 질문

Generic에 대해 설명하시오.<br />
Result타입에 대해 설명하시오.

<br />

# ✏️ 작성자

- [릴리, 숲재](#릴리-숲재)
- [토니, 호댕](#토니-호댕)

<br />

---

# 릴리, 숲재

## 1. Generic에 대해 설명하시오.

구체적인 타입 대신 플레이스 홀더 타입을 사용해 유연하고 재사용가능한 함수와 타입을 만들수 있는 문법입니다.

<br />
<br />

## Generic은 어떻게 구현하나요?

타입이나 메서드 구현 시 이름 뒤에 꺽쇠를 사용하여 타입 파라미터를 표시

<br />
<br />

## 타입 파라미터에 제약을 주는 방법에 대해 설명해주세요

타입 파라미터 뒤에 콜론 + 클래스나 프로토콜을 적어주면, 해당 클래스를 상속한 클래스나 프로토콜을 채택한 타입만 사용할 수 있도록 제약을 걸 수 있습니다.

<br />
<br />

## Associated 타입에 대해 설명하시오.

프로토콜 내부에서 사용되는 플레이스 홀더 타입입니다. associated type은 프로토콜을 채택하여 구현할 때 구체적인 타입으로 결정할 수 있습니다. 프로토콜을 제네릭하게 사용할 수 있는 방법입니다.

<br />
<br />

## Associated 타입을 사용한 프로토콜은 타입으로 사용될 수 있나요?(프로퍼티의 타입이나, 메서드의 파라미터의 타입, 메서드의 리턴타입 등)

못해요.

<br />
<br />

## Type constraints 와 where 절의 차이점을 설명해주세요

Type constraints에서는 타입의 제약만 걸 수 있고, where절에서는 타입의 제약 플러스 좀 더 복잡한 제약을 설정할 수 있다.

<br />
<br />

## Result타입에 대해 설명하시오.

제네릭 열거형으로, Success와 Failure 케이스의 연관값이 제네릭 타입으로 구현 되어 있다. Failure는 Error 프로토콜을 채택한 타입만 사용할 수 있다.

<br />
<br />

## Result타입을 사용해 본적이 있나요? 어떨 때 사용했나요?

ex) 네트워킹과 같이 에러처리가 필수적인 로직을 구현할 때 사용

<br />
<br />

## <라이브코딩> Custom Result 타입을 구현해주세요.

```swift
enum MyResult<Success, Failure> where Failure : SomeProtocol {
    case success(Success)
    case failure(Failure)
}
```

<br />
<br />
<br />

---

# 토니, 호댕

## 1. Generic에 대해 설명해주세요.

제네릭은 어떤 타입에도 재사용이 가능한 유연한 함수나 타입을 만들기 위해 사용하는 것이다. 

> *Generic code* enables you to write flexible, reusable functions and types that can work with any type, subject to requirements that you define.
[Swift Programming Guide](https://docs.swift.org/swift-book/LanguageGuide/Generics.html)
> 

<br />
<br />

## Generic을 사용한 경험이 있나요?

일단 기본적으로 `Array`, `Set`, `Dictionary` 같은 Collection들이 제네릭으로 구현되어 있기 때문에 특정 타입의 컬렉션을 만들 때 사용해본 경험이 있습니다. 

이외에는 네트워킹을 위한 `JSONDecoder`를 생성할 때 다양한 타입에 대응할 수 있는 decoder를 만들기 위해 제네릭을 직접 구현해본 경험도 있습니다. 

<br />
<br />

## Generic의 where 절과 Generic의 일반적인 타입 제약의 차이점은 무엇인가요?

일단 두 가지 경우 모두 타입 파라미터로 지정한 타입이 특정 프로토콜을 채택하거나 특정 클래스와 상속관계에 있는 클래스만 사용할 수 있도록 제약 사항을 걸 수 있다는 점은 공통점입니다. 

다만 콜론(:)을 붙이는 **일반적인 타입 제약의 경우 하나의 제약**만 걸 수 있지만 **where절의 경우 2가지 이상의 좀 더 구체적인 제약**을 붙일 수 있다는 점이 차이점입니다. 

<br />
<br />

## 혹시 Generic을 사용하지 않고 동일한 이름의 다양한 메서드를 만들 수 있는 방법에 대해 아는가?

overload를 통해 이름은 동일하나 다른 파라미터를 가진 메서드를 생성할 수 있습니다. 하지만 이 경우 제네릭에 비해 동일한 이름의 메서드를 반복해서 작성해줘야 한다는 단점이 있다고 생각합니다. 

<br />
<br />

## 프로토콜에도 제네릭의 타입 파라미터를 붙일 수 있나요?

안됩니다! 프로토콜의 경우 제네릭처럼 다양한 타입에 대응하기 위해선 `associatedType`을 사용하는 것으로 알고 있습니다. 

<br />
<br />

## 2. Result에 대해 설명해주세요.

Result는 열거형으로 구현이 되어 있으며 success와 failure 2가지 케이스를 가지고 있다고 알고 있습니다. failure의 경우 타입 제약을 통해 Error 타입만 올 수 있습니다. 

따라서 이를 통해 성공했을 경우와 실패했을 경우 코드를 나눠 처리를 할 수 있는 타입으로 알고 있습니다. 

```swift
@frozen public enum Result<Success, Failure> where Failure : Error {

    /// A success, storing a `Success` value.
    case success(Success)

    /// A failure, storing a `Failure` value.
    case failure(Failure)
}
```

<br />
<br />

## 그렇다면 Result 타입을 사용해본 경험이 있으신가요?

네, 네트워킹 관련 코드를 만들면서 데이터를 받아오는 메서드에서 반환 타입을 Result 타입으로 사용해본 경험이 있습니다. 

<br />
<br />

## Result와 이전의 에러 처리 방식의 차이점과 Result가 어떤 점에서 더 나았는지 말씀해주세요.

`Result` 이전에는 에러가 발생하는 메서드에 `throws`를 통해 에러가 발생할 수 있다고 명시해주고 해당 메서드를 사용할 때는 `try` 키워드를 붙이며, 에러 처리의 경우 `do-try`를 통해 해줍니다. 

이런 방식은 에러가 어디서 발생했는지 파악하기 위해 처음 `throws`를 명시한 메서드까지 살펴봐야 어디서 발생한 메서드인지 알 수 있으며 `do-try`를 통해 에러 처리를 해주는 부분도 `Result`에 비해 가독성이 떨어졌습니다. 

`Result`의 경우 `switch`문을 통해 반환 값에 대한 분기 처리를 명확히 해줄 수 있어 조금 더 가독성이 좋다고 느껴졌습니다. 

<br />
<br />

## 에러 처리를 해주는 이유는 무엇일까요?

에러처리를 해주는 이유는 일단 개발자와 사용자 모두에게 에러가 발생했을 경우 어떤 에러가 발생했는지 명시적으로 알려주고 이에 대한 처리를 해주기 위해 해야 합니다.
