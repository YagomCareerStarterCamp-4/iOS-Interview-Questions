# ❔ 질문
1. Subscripts에 대해 설명하시오 <br />
2. String은 왜 subscript로 접근이 안되는지 설명하시오 <br />

# ✏️ 작성자
- [앨리](#앨리)
- [릴리, 고사리](#릴리-고사리)
<br />

---

# 앨리
## Subscripts에 대해 설명하시오.
*콜렉션, 리스트, 시퀀스 등 집합의 **특정 member elements에 간단하게 접근할 수 있는 문법***

*추가적인 메소드 없이 특정 값을 할당하거나 가져올 수 있다.*

**클래스, 구조체, 열거형에서 시퀀스의 멤버 요소에 접근하기 위한 바로가기 첨자로, 단일 타입에 여러 서브스크립트를 정의할 수 있다.**
<br />
<br />

## String은 왜 subscript로 접근이 안되는지 설명하시오.
Swift의 String은 Struct타입이고, `characters`의 collection이다. 즉 Array<`Element`>의 element가 Character인 배열이다. 

하지만 Swift에서 String은 subscript, 즉 str[0]같이 Int로 접근하지 못하고, String.index로 접근해야한다.

그 이유는 Swift에서 Character는 1개 이상의 Unicode Scalar로 이루어져 있다. 즉 크기가 **`가변적`**이다.

따라서 String은 subscript를 Int가 아니라 String.Index를 통하여 값을 확인할 수 있다.
<br />
<br />

## Subscript 정의하는 방법?
A. **subscript 키워드로 작성하며 하나 이상의 파라미터와 반환 값을 지정한다.** 

**파라미터나 리턴형을 생략할 수 없고, getter와 setter 모두 구현할 수 있다**

**get-only는 가능하지만, set-only는 불가하다 (getter 생략 불가)**

```swift
subscript(index: Int) -> Int {
    get {
	  }
    set(newValue) {
    }
}
```
<br />
<br />
<br />

---

# 릴리, 고사리
## Subscripts에 대해 설명하시오.
`[]` 를 사용해서 Collection의 요소를 편리하게 접근할 수 있게 하는 문법입니다. 

getter와 setter를 통해 read-only나 read-write 서브스크립트를 정의할 수 있습니다. 

스위프트 Array의 인덱스를 통한 원소 접근과 Dictionary에서 key를 통한 원소 접근도 서브스크립트 문법을 사용합니다.
<br />
<br />

## 동일한 리턴 타입의 서브스크립트를 정의할 수 있나요?
인덱스의 argument label과 argumnet 타입을 달리하면 여러개의 서브스크립트를 정의가능합니다.
<br />
<br />

## String은 왜 subscript로 접근이 안되는지 설명하시오.
String 타입은 내부적으로 Charcater의 배열로 정의되어있습니다. 

그런데 Character는 복수의 Unicode Scalar Value로 이루어져있기 때문에, 크기가 가변적입니다. 

따라서 컴파일 타임에 String을 동일한 크기에 따라 나눌 수없기 때문에 배열의 인덱스로 접근이 어렵습니다.  

따로 extentsion을 구현해줘서 subscript로 접근을 하거나 Sting.Index를 통해 n번재 캐릭터에 접근 할 수 있습니다.
<br />
<br />
<br />
