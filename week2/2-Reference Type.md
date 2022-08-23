# ❔ 질문

Strong 과 Weak 참조 방식에 대해 설명하시오. 순환 참조에 대하여 설명하시오.<br />
강한 순환 참조 (Strong Reference Cycle) 는 어떤 경우에 발생하는지 설명하시오.

<br />

# ✏️ 작성자

- [지성, 숲재](#지성-숲재)
- [아샌, 줄라이](#아샌-줄라이)

<br />

---

# 지성, 숲재

## Strong 과 Weak 참조 방식에 대해 설명하시오.

ARC는 인스턴스의 Reference Count를 관리하여 Count가 0이 되는 시점에 해당 인스턴스를 Deallocate 시킨다.
클래스 인스턴스를 참조하는 변수를 선언할 때 별도의 키워드를 붙이지 않는다면 `강한참조`를 하게 되고, 참조 시점에 retain되어 Retain Count가 1이 오르고, 참조가 종료되는 시점에 release되어 Retain Count가 1이 내려간다. 약한 참조의 경우, 클래스 인스턴스를 참조할 때 retain을 하지않아 retain count가 오르지 않고, release도 되지 않는다. 클래스 인스턴스가 해제될 경우 해당 클래스 인스턴스가 담긴 변수에 자동으로 nil로 초기화 해준다. 또한 자동으로 nil로 초기화해주므로 nil값으로 초기화될수 있는 옵셔널 타입이여야만 한다.

<br />
<br />

## 순환 참조에 대하여 설명하시오.

여러 클래스 인스턴스가 서로를 순환하여 참조하고 있는 상태를 순환참조라고 한다.

<br />
<br />

## 강한 순환 참조 (Strong Reference Cycle) 는 어떤 경우에 발생하는지 설명하시오.

여러 클래스 인스턴스가 서로간에 강한 참조를 할 경우 발생하고, 순환 참조가 발생하게 되면 서로간의 참조가 해제되지않기 때문에 메모리 누수의 발생 가능성이 있다.

또한 클래스 인스턴스를 소유하고 있던 변수에 nil을 할당하여 해당 클래스 인스턴스에 대한 메모리 주소가 유실되어 메모리 누수가 발생하고 있는 클래스 인스턴스에 접근조차 하지 못해 프로그램 종료 전까지 영원히 메모리에서 해제되지 못할 수 있다.

<br />
<br />

## 꼬리질문

- unowned
- unowned와 weak의 차이
    - 어떨 때 사용하는지, 주의점
- 클로저 순환참조 문제
    - 어떤 상황에 발생하는지, 왜 발생하는지
    - 어떻게 방지하는지
- 강한 순환 참조 방지 방법

<br />
<br />

## 강한 순환 참조를 방지하는 방법

강한 순환 참조를 방지하기 위해 weak, unowned 참조를 활용하는 방법이 있다.

<br />
<br />

## unowned

unowned 참조는 weak 참조와 동일하게 클래스 인스턴스를 참조할 때 retain을 하지않아 retain count가 오르지 않고, release도 되지 않는다.

<br />
<br />

## unowned와 weak의 차이

약한 참조는 참조할 클래스 인스턴스가 수명이 더 짧은 경우(먼저 nil, 메모리에서 내려갈 경우)에 사용하고, 미소유 참조는 참조할 클래스 인스턴스와 수명이 같거나 더 짧은 경우(먼저 nil, 메모리에서 내려갈 경우)에 사용한다.

<br />
<br />

또한 약한 참조는 클래스 인스턴스를 트래킹하여 nil을 할당하여 초기화할 수 있는 반면, 미소유 참조는 클래스 인스턴스의 주소값은 알고있지만 트래킹하지않아 nil을 할당하여 초기화 할 수 없고, 이로 인해 클래스 인스턴스는 메모리에서 이미 내려갔지만 클래스 인스턴스가 위치했던 주소값을 그대로 들고있어(댕글링 포인터) 해당 주소에 접근할 시 크래시가 일어나게 된다.

## 클로저 순환참조 문제

![https://i.imgur.com/TY1txxB.png](https://i.imgur.com/TY1txxB.png)

클로저는 클래스와 동일하게 레퍼런스 타입이므로 내부에서 self를 캡쳐시 참조가 이뤄지기 때문에 self의 retain count가 1이 올라가게 된다. 이로 인해 클래스 인스턴스가 할당된 변수에 nil을 할당하여도 retain count가 남아있어 메모리에 남아있는 문제가 발생하게 된다.

<br />
<br />

## 어떻게 방지하는지?

캡쳐리스트에서 weak이나 unowned 키워드를 사용하여 retain count를 증가시키지 않는 방식으로 값을 캡쳐 할 수 있다.

<br />
<br />

## 일반 함수에서 강한순환참조 문제를 걱정하지 않아도 되는 이유는?

func 키워드로 정의되는 `전역 함수`는 일반적으로 우리가 클로저라고 부르는 `익명 함수`와는 달리 값을 캡쳐하지 않는다. 그래서 강한 참조 문제가 일어나지 않는다.

<br />
<br />

### Reference

[Automatic Reference Counting - Swift Language Guidline](https://docs.swift.org/swift-book/LanguageGuide/AutomaticReferenceCounting.html)[CaptureList - Swift Reference Manual](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#ID544)

<br />
<br />

---

<br />
<br />

# 아샌, 줄라이

## 1. Strong과 Weak참조 방식에 대해 설명하시오.

> default가 strong, strong 참조 카운트를 올리고 메모리에 올라가있는 상태이다.
>
![image](https://user-images.githubusercontent.com/75905803/186047575-1f5fb969-49a3-48b2-a8bf-6e9724abe543.png)

> ARC관련 공식문서에 보면 프로퍼티, 상수, 변수등에 클래스 인스턴스를 할당할 때마다 인스턴스에 대한 강한 참조를 만들고 강한 참조가 남아있는 한 할당 해제를 허용하지 않는다고 되어있다.
그리고 예시 코드를 확인해봤을 때 다른 키워드를 사용하지 않고 일반적으로 class를 생성하고 사용하는 경우에 강한 참조 상태이고 이 모든 참조가 끊어질 때까지 인스턴스 할당을 해제하지 않는다는 표현이 나와 있으므로 default는 strong인 것을 확인할 수 있다.
> 

> weak는 약한 참조로 참조 카운트를 올리지 않고 참조된 인스턴스가 사라지면 nil이 된다.
따라서 런타임 시점에 값을 변경할 수 있어야 하기 때문에 항상 옵셔널 타입의 변수로 선언을 해야한다.
> 

![image](https://user-images.githubusercontent.com/75905803/186047597-a7aa90de-d575-4f2d-84ce-74ae7abe8988.png)

<br />
<br />

## 2. 순환 참조

> 두 개의 클래스 인스턴스가 서로 강한 참조를 유지하여 각 인스턴스가 다른 인스턴스의 참조 카운트를 0으로 만들지 않아 계속 활성상태로 유지하는 것
> 

![image](https://user-images.githubusercontent.com/75905803/186047623-c1d14eaa-5cdd-4c88-8357-43210ab12fd8.png)

![image](https://user-images.githubusercontent.com/75905803/186047637-cd50736e-94f7-4717-aae2-da1db63e680b.png)

<br />
<br />

## 3. 강한 순환 참조 (Strong Reference Cycle) 는 어떤 경우에 발생하는지 설명하시오.

> 순환 참조가 발생한 상황에서 하나의 인스턴스 또는 모두가 nil(해제)가 된 상태가 되더라도 서로 강한 참조를 유지하고 있기 때문에 메모리에서 해제가 되지 않음
> 

![image](https://user-images.githubusercontent.com/75905803/186047653-c8370e6e-18a5-41ed-b7c4-571d0422ee94.png)

<br />
<br />

# 꼬리질문

## 1. ARC는 어떻게 동작하고 다른 언어에서의 참조 처리방식과 다른점이 무엇인지?

> 다른 언어에서의 대표적인 참조 처리방식인 GC(Garbage Collection)는 런타임 시점에 처리가 결정되는 반면 ARC는 컴파일 시점에 작동하기 때문에 메모리 관리를 위한 시스템 자원을 추가할 필요가 없음
> 

![image](https://user-images.githubusercontent.com/75905803/186047682-cf102b3b-b66c-4d3c-9f9a-2c68f68c9689.png)

- 출처: 야곰 스위프트 프로그래밍

<br />
<br />

## 2. weak와 unowned의 차이

> 둘다 참조 카운트를 올리지 않는 공통점이 있다.
weak의 경우 인스턴스가 사라지면 nil이 되지만 unowned는 인스턴스가 사라지면 런타임 시점에 에러가 발생한다.
> 

![image](https://user-images.githubusercontent.com/75905803/186047702-0355d0ab-986f-45b1-be8a-181642dad198.png)

> 이러한 이유로 인해 두가지를 설정하는 기준은 인스턴스의 생명주기와 관련이 있다.
인스턴스의 생명주기가 더 짧다면 `weak`, 같거나 더 길다면 `unowned`를 채택하면 된다.
> 

![image](https://user-images.githubusercontent.com/75905803/186047716-5728d1c1-57a4-4b69-b30e-9adebd72f60f.png)

> 하지만 개인적인 의견으로 Forced Unwrapping과 비슷한 느낌으로 차라리 unowned 보다는 weak를 채택하는 것이 어떨까 하는 입장이다.
Forced Unwrapping의 경우에도 우리가 코드를 작성하는 시점에서는 nil이 절대 나오지 않을 것 또는 이전 과정에서 확실히 검증이 되었다고 판단하기 때문에 Forced Unwrapping를 사용할 수 있다.
하지만 사용자의 예기치 못한 작동, 코드가 추가되고 로직이 변경되는 과정에서 나오는 human error로 인해 nil이 될 가능성이 충분히 있기 때문에 옵셔널 바인딩을 통해 안전하게 처리를 하는 것이 좋다.
이러한 것과 비슷하게 unowned의 경우에 참조 카운트를 올리지 않지만 인스턴스가 먼저 사라지게 된다면 런타임 시점에 치명적 에러를 발생시키기 때문에 weak를 채택하는 것이 더 안정성이 확보된 것이 아닌가 하는 생각이 든다.
> 

<br />
<br />

## 3. closure에서 weak self를 반드시 써야하는 상황은 언제인가?

> escaping closure의 경우 클래스의 인스턴스인 self를 참조하는 경우 self를 캡쳐하면 강한 참조 사이클이 발생할 수 있다.
특히 escaping closure는 반환된 후 호출되는 클로저이다.
클로저가 탈출할 수 있는 방법은 밖에 정의된 변수에 저장되는 것이고 작업을 시작한 후 반환되지만 작업이 완료될 때까지 클로저가 호출되지 않는다.
그렇기 때문에 내부 인스턴스를 사용하기 위해선 self를 참조해야하는데 weak self를 사용하지않으면 강한 참조사이클이 발생할 수 있다.
> 
- weak self를 사용할 때 VC가 dismiss가 된다던지, 해당 인스턴스가 메모리에서 해제가 되더라도 클로저의 역할(기능)을 보장할 수 있는 방법이 있는지?
    - 아래 링크에 나와 있는 방법들을 해석해보면 옵셔널 바인딩을 통해 캡쳐한 self를 담아두고 일시적인 강한 참조를 생성하는 것이다.
    이렇게 코드를 작성한 경우 클로저가 살아있는동안 self가 해제되는 것을 방지하여 self가 유지되도록한다.
    - 옵셔널 체이닝을 사용하는 경우에 클로저가 실행중에 self가 nil이 된다면 그 시점부터 정상적인 동작을 하지 않기 때문에 해당 클로저의 역할을 모두 수행하지 못한다.
    1. [https://velog.io/@haanwave/Article-You-dont-always-need-weak-self](https://velog.io/@haanwave/Article-You-dont-always-need-weak-self)
    2. [https://chrisdownie.net/software/2022/04/10/the-golden-rules-of-weak-self/](https://chrisdownie.net/software/2022/04/10/the-golden-rules-of-weak-self/)
