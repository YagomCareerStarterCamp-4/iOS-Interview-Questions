# ❔ 질문
1. defer란 무엇인지 설명하시오 <br />
2. defer가 호출되는 순서는 어떻게 되고, defer가 호출되지 않는 경우를 설명하시오 <br />
3. property wrapper에 대해서 설명하시오 <br />

# ✏️ 작성자
- [숲재, 제리](#숲재-제리)
- [줄라이, 지성](#줄라이-지성)
<br />

---

# 숲재, 제리
## defer란 무엇인지 설명하시오.
작성된 위치와 상관 없이 함수 종료 직전에 실행되는 구문으로 현재 있는 코드 블록을 빠져나가기 전에 꼭 실행해야하는 코드를 작성할 수 있습니다.

defer를 중첩하여 작성도 가능하며, 이경우 가장 내부에 있는 defer가 가장 마지막에 실행됩니다.
<br />
<br />

## defer를 사용했던 경우가 있나요?
오류처리나 메모리를 해제하는 코드를 작성하는 경우에 defer를 사용해본적 있습니다
<br />
<br />

## defer가 호출되는 순서는 어떻게 되고, defer가 호출되지 않는 경우를 설명하시오.
여러 개의 defer가 블록 내부에 있을 시에 맨 마지막에 작성된 구문부터 역순으로 실행되고

- defer 구문을 읽기 전에 함수가 종료되면 defer 구문이 호출이 안됨. throw를 이용해서 에러를 던지는 경우나 guard문을 이용해서 throw, return으로 처리되는 경우 defer가 처리되지 않습니다.
<br />
<br />

## property wrapper에 대해서 설명하시오.
swift5.1에서 소개된 문법으로 반복되는 로직을 프로퍼티 자체에 연결할 수 있습니다.

property wrapper를 사용하는 방법은 enum, struct, class 앞에 @propertyWrapper를 붙혀 어떠한 프로퍼티에 하고 싶은 행동을 정의하는 타입으로 선언합니다.

그리고 타입 내부에 wrappedValue 프로퍼티를 만들어 원하는 로직을 작성합니다.

해당 로직을 적용하기 위한 프로퍼티 앞에 wrapper name을 작성하여 wrapper를 적용할 수 있습니다.
<br />
<br />

## property wrapper내 initialize를 사용하는 이유나 어느 경우에 사용할 수 있을까요?
wrapped property에 초기값을 설정해주는 경우 init 메서드를 이용하고, 로직에 필요한 값을 인수로 받고 싶은 경우 init 파라미터를 이용하여 값을 가져올 수도 있습니다.

```Swfit
@Uppercase(wrappedValue: "default", index: 2) var name: String

```
<br />
<br />

## property wrapper를 이용해서 string 프로퍼티 값을 대문자로 나타내는 로직을 작성해주세요!
```Swift
@propertyWrapper
struct Uppercase {
  private var value: String = ""

  var wrappedValue: String {

      get { self.value }
      set { self.value = newValue.uppercased() }
  }

  init(wrappedValue initialValue: String) {
      self.wrappedValue = initialValue
  }
}

struct City {
    @Uppercase var name: String
}

var city = City()
city.name = "seoul"
print(city.name)

```
<br />
<br />
<br />

---

# 줄라이, 지성
## defer란 무엇인지 설명하시오.
함수가 종료되기 전 실행해야하는 코드를 작성하는 코드 블럭
<br />
<br />

## defer가 호출되는 순서는 어떻게 되고, defer가 호출되지 않는 경우를 설명하시오.
```swift
func test() {
    defer {
        print("test 1")
    }

    do {
        defer {
            print("test 2")
        }

        print("test 3")
    }

    for i in 0..<2 {
        defer {
            print("test 4")
        }

        if i % 2 == 0 {
            defer {
                print("test 5")
            }

            print("test 6")
        }
    }

    defer {
        print("test 7")
    }

    print("test 8")
}
```

- defer는 Stack으로 동작하여(LIFO) 선언된 역순으로 호출된다.

defer가 호출되지 않는 경우는 throw나 guard를 사용하여 함수를 도중에 탈출해버리는 경우엔 defer가 호출되지 않으며, 오류가 발생하면 함수를 반환하지 않고 프로그램을 종료시키는 비반환함수 또한 defer가 호출되지 않는다.
<br />
<br />

## property wrapper에 대해서 설명하시오.
Property Wrapper는 반복되는 로직들을 프로퍼티 자체에 연결해 보일러 플레이트 코드를 줄일 수 있으며, 코드의 재사용성을 높여준다.

특정 타입을 정의하여 property wrapper 어노테이션을 붙이고, wrapperValue에 반복되는 로직을 넣어 사용한다.
<br />
<br />

## 사용함으로써 얻을 수 있는 장점은?
보일러 플레이트 코드를 줄일 수 있으며, 코드의 재사용성을 높여준다.
<br />
<br />
<br />
