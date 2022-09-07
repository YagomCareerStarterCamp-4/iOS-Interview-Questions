# ❔ 질문
Copy On Write는 어떤 방식으로 동작하는지 설명하시오.<br />
Hashable이 무엇이고, Equatable을 왜 상속해야 하는지 설명하시오.<br />

# ✏️ 작성자ㄴ
- [숲재, 나무](#숲재-나무)
- [제인, 호박](#제인-호박)
<br />

---

# 숲재, 나무
## 질문 - **Copy On Write는 어떤 방식으로 동작하는지 설명하시오.**

- Copy-on-Write은 Swift에서 매우 무거운(heavy) 연산인 값 타입의 복사를 최적화(Optimize)하는 방법. Swift의 원시 타입 열거형인 배열, 딕셔너리, 세트의 복사는 이 방식을 활용하여 구현되어 있다.
- `Copy-on-Write`는 Swift에서 `ValueType`이 복사될 때에는 실제로 복사하지 않고 원본 리소스를 공유하다가 `Write` 시점에 실제로 복사하여 불필요한 메모리 사용과 시간을 줄이기 위한 것이다.
- ( = Copy On Write는 A라는 변수에 B라는 변수를 할당해주었을 때, 새로 메모리에 할당하는 것이 아니라, B의 메모리를 A가 공유하는 형태로 구성됩니다. 그러다가 A가 값이 수정될 때 새로 메모리에 할당이 되는 식으로 동작합니다.)
- 사용자 정의 구조체 같은 값 타입의 경우 Copy-on-Write이 반영되어있지 않다.
<br />
<br />

## Swift에서 COW가 기본적으로 구현되어 있는 타입은?

- Array, Dictionary, Set, String -> STD에 정의된 가변 길이를 가진 콜렉션 타입
<br />
<br />

## 사용자 정의 타입에서 COW를 직접 구현하려면 어떻게 해야 하는지 간단히 설명하시오

- struct 내부에 class 인스턴스를 저장프로퍼티로 소유하고, 그 내부에 프로퍼티로 원본 데이터를 저장한다.
- getter에서는 참조의 데이터를 그대로 반환하고,, setter에는 UniquelyReferenced를 체크한 뒤 true면 mutate하고 false면 새로운 참조 인스턴스를 생성하여 반환합니다.

```swift
final class Ref<T> {
    var val : T
    init(_ v : T) {val = v}
}

struct Box<T> {
    var ref : Ref<T>
    init(_ x : T) { ref = Ref(x) }

    var value: T {
        get { return ref.val }
        set {
          if (!isUniquelyReferencedNonObjC(&ref)) {
            ref = Ref(newValue)
            return
          }
          ref.val = newValue
        }
    }
}
```

<details>
<summary>Array, Dictionary, Set에 COW가 구현되어있다는 근거</summary>
<div markdown="1">  

[https://developer.apple.com/documentation/swift/array#:~:text=Arrays%2C](https://developer.apple.com/documentation/swift/array#:~:text=Arrays%2C) like all,the original storage.

> Arrays, like all variable-size collections in the standard library, use copy-on-write optimization. Multiple copies of an array share the same storage until you modify one of the copies. When that happens, the array being modified replaces its storage with a uniquely owned copy of itself, which is then modified in place. Optimizations are sometimes applied that can reduce the amount of copying.
> 
> 
> This means that if an array is sharing storage with other copies, the first mutating operation on that array incurs the cost of copying the array. An array that is the sole owner of its storage can perform mutating operations in place.
> 

[https://developer.apple.com/documentation/swift/dictionary/#:~:text=Bridging Between Dictionary,Dictionary share buffer](https://developer.apple.com/documentation/swift/dictionary/#:~:text=Bridging%20Between%20Dictionary,Dictionary%20share%20buffer).

> Bridging Between Dictionary and NSDictionary
You can bridge between Dictionary and NSDictionary using the as operator. For bridging to be possible, the Key and Value types of a dictionary must be classes, @objc protocols, or types that bridge to Foundation types.
> 
> 
> Bridging from Dictionary to NSDictionary always takes O(1) time and space. When the dictionary’s Key and Value types are neither classes nor @objc protocols, any required bridging of elements occurs at the first access of each element. For this reason, the first operation that uses the contents of the dictionary may take O(n).
> 
> Bridging from NSDictionary to Dictionary first calls the copy(with:) method (- copyWithZone: in Objective-C) on the dictionary to get an immutable copy and then performs additional Swift bookkeeping work that takes O(1) time. For instances of NSDictionary that are already immutable, copy(with:) usually returns the same dictionary in O(1) time; otherwise, the copying performance is unspecified. The instances of NSDictionary and Dictionary share buffer using the same copy-on-write optimization that is used when two instances of Dictionary share buffer.
> 

[https://developer.apple.com/documentation/swift/set/#:~:text=Bridging Between Set,Set share buffer](https://developer.apple.com/documentation/swift/set/#:~:text=Bridging%20Between%20Set,Set%20share%20buffer).

> Bridging Between Set and NSSet
You can bridge between Set and NSSet using the as operator. For bridging to be possible, the Element type of a set must be a class, an @objc protocol (a protocol imported from Objective-C or marked with the @objc attribute), or a type that bridges to a Foundation type.
> 
> 
> Bridging from Set to NSSet always takes O(1) time and space. When the set’s Element type is neither a class nor an @objc protocol, any required bridging of elements occurs at the first access of each element, so the first operation that uses the contents of the set (for example, a membership test) can take O(n).
> 
> Bridging from NSSet to Set first calls the copy(with:) method (- copyWithZone: in Objective-C) on the set to get an immutable copy and then performs additional Swift bookkeeping work that takes O(1) time. For instances of NSSet that are already immutable, copy(with:) returns the same set in constant time; otherwise, the copying performance is unspecified. The instances of NSSet and Set share buffer using the same copy-on-write optimization that is used when two instances of Set share buffer.
>
</div>
</details>


<details>
<summary>사용자 정의 값타입에서 COW를 구현하지 않아도 컴파일러가 최적화를 위해서 COW를 하는 경우도 있다</summary>
<div markdown="1">  
[value type - Does swift copy on write for all structs? - Stack Overflow](https://stackoverflow.com/questions/43486408/does-swift-copy-on-write-for-all-structs)

> with your custom structure – you're not implementing copy-on-write at a language level. Of course, as @Alexander says, this doesn't stop the compiler from performing all sorts of optimisations to minimise the cost of copying whole structures about. These optimisations needn't follow the exact behaviour of copy-on-write though – the compiler is simply free to do whatever it wishes, as long as the program runs according to the language specification.
> 

Language level에서 구현되어있지 않더라도, 컴파일러가 Optimization을 위해 COW를 수행하는 경우도 있다. 링크의 지역 변수 값 복사하는 경우와 같을 때...
</div>
</details>

<br />
<br />

## 질문 - Hashable이 무엇이고, Equatable을 왜 상속해야 하는지 설명하시오.

1. Hashable이란?
정수 타입의 해시값을 제공할 수 있는 프로토콜. 
임의의 크기를 갖는 데이터를 고정 크기 값으로 맵핑하기 위해 해시값(HashValue)을 사용함
딕셔너리와 세트에서 값에 유일하게 접근하기 위해 사용하는 수단이다.
    
    > A type that can be hashed into a Hasher to produce an integer hash value.
    > 
2. Equatable을 상속해야 하는 이유?
동일한 해시값에 대해 같은 값이 할당되어있는 경우를 해시 충돌이라고 하는데, 해시 충돌 시에 값을 직접 비교하여 찾기 위해 `==` 비교 연산이 필요하다. 이를 위해 `Equatable`을 상속해야 한다.
<br />
<br />

## Swift 기본 컬렉션 중에서 요소가 Hasable을 준수해야 하는것은?

- Set, Dictionary의 Key
<br />
<br />

## Hash 충돌 문제를 해결하는 방법을 설명하시오.

- 개방주소법/개별 체이닝
<br />
<br />

## Swift에서는 어떤 방식으로 충돌을 해결하나요?

- 개방주소법 - 선형탐색
<br />
<br />

## 선형탐색의 한계점은?

- 데이터가 뭉치는 Clustering 현상이 발생하여 해싱 효율 저하 가능성 있음
<br />
<br />

## Reference

- [https://nsios.tistory.com/81](https://nsios.tistory.com/81)
- [http://ankit.im/swift/2016/01/19/exploring-swift-dictionary-implementation/](http://ankit.im/swift/2016/01/19/exploring-swift-dictionary-implementation/)
<br />
<br />

---

# 제인, 호박
## 질문 - Copy On Write는 어떤 방식으로 동작하는지 설명하시오.
## Copy On Write를 사용하는 이유

값타입의 경우 변수에 할당하거나 함수의 파라미터로 넘길 때 할당된 값이 복사가 된다.

그러나 만약 아주 큰 값타입을 다룰때는 복사를 한다면 비용이 매우 많이 들 것이다.

따라서 이러한 이슈를 최소화하기 위해 특정 값타입의 경우 수정이 일어나거나, 참조가 한번 이상 될때만 복사가 되도록하여 성능을 개선하였다.
<br />

다시 말해서 수정 (쓰기)이 일어날 때 복사한다는 뜻으로,

단순히 값을 전달할 때는 복사가 아니라 기존 값의 참조를 통해 불필요한 복사를 줄여서 메모리를 절약할 수 있다.
<br />

작동 예시를 보자.

변수 array에 담긴 배열을 array2에 할당할때는 주소값이 같은데,

array2를 수정하니 그제서야 array2의 주소값이 달라진다.

```swift
var array = [1,2,3] 
var array2 = array
address(o1: &array) //0x100517000 
address(o1: &array2) //0x100517000 

array2.append(4) // array2가 변경 
address(o1: &array) //0x100517000 
address(o1: &array2) //0x102b04330
```
<details>
<summary>주소 구하는 함수 adress</summary>
<div markdown="1">  

```swift
func address(o1: UnsafeRawPointer) {
    let address = String(format: "%p", Int(bitPattern: o1))
    print(address)

}
func address<T: AnyObject>(o2: UnsafePointer<T>) {
    let address = String(format: "%p", Int(bitPattern: o2))
    print(o2.pointee)
    print(address)

    withUnsafePointer(to: T.self) { pointer in
        pointer.pointee
        pointer
    }
}
```

</div>
</details>
<br />
<br />

## 모든 값타입의 경우 Copy On Write를 지원하나요?

NO

Swift Standard Library에 정의된 값타입인 **컬렉션타입 (Array, Dictionary, Set)**의 경우에만 COW가 구현되어있다. 사용자 정의 값타입의 경우에는 지원하지 않는데, 이때 사용자가 직접 COW를 구현할 수 있다.
<br />
<br />

## 사용자 정의 구조체에서 Copy On Write를 구현하는 방법에 대해 설명해보세요.

```swift
final class Ref<T> {
  var val : T
  init(_ v : T) {val = v}
}

struct Box<T> {
    var ref : Ref<T>
    init(_ x : T) { ref = Ref(x) }

    var value: T {
        get {
            return ref.val
        }
        set {
            if (!isKnownUniquelyReferenced(&ref)) {
                //참조카운트 1이 아니라면 (=여러곳에서 참조되고 있다면)
                print(isKnownUniquelyReferenced(&ref))
            ref = Ref(newValue)
                print("새로운 인스턴스 생성")
            return
          }
          ref.val = newValue
        }
    }
}

//주소 구하는 함수
func address(o1: UnsafeRawPointer) {
    let address = String(format: "%p", Int(bitPattern: o1))
    print(address)

}
```

```swift
var box = Box<[Int]>([1,2,3])
var copy = box
address(o1: &box.value) //0x10072bda0
address(o1: &copy.value) //0x10072bda0

box.value = [3]
address(o1: &box.value) //**0x10072c1c0**
address(o1: &copy.value) //0x10072bda0
```

1. 제네릭 타입의 프로퍼티를 가진 class 타입을 정의한다.
2. 위에서 정의한 class 타입의 프로퍼티를 가진 struct를 정의한다.
3. 그리고 struct에 value라는 계산 프로퍼티를 정의하여 getter과 setter을 가지게 한다.
4. getter이 불린 경우에는 그대로 값을 전달하고
5. setter이 불린 경우에는 내장 전역 함수인 isKnownUniquelyReferenced 함수를 사용하여
    
    해당 객체의 참조 카운트가 1이면 true, 1보다 크면 false를 반환한다.
    
    - 객체의 참조 카운트가 2개 이상이면 다른 곳에서 사용되고 있다는 뜻이니까 새로운 복사본을 만들어서 수정도 같이 진행한다.
    - 객체의 참조 카운트가 1이면 그냥 수정한다.
<br />
<br />

## COW 한계

첫 수정 작업을 할 때는 두 세번째보다 실행 시간이 더 걸린다 → 오버헤드가 발생함

<img alt="image" src="https://user-images.githubusercontent.com/73867548/188710276-b72a6f68-89ff-4bd4-823e-d04141771aa5.png">
<br />
<br />

## 오버헤드가 무엇인지 아시나요?

- 어떤 처리를 하기 위해 들어가는 간접적인 시간
- 기본적인 처리시간 말고 추가로 들어가는 시간
<br />
<br />

## 값타입과 참조타입의 차이가 무엇인가?

- 값타입 (Enum, String, Dictionary, Set Tuple)
    - 변수에 할당하면 스택 영역에 값이 저장됨
    - 변수를 다른 변수에 복사한 후 복사본을 변경하더라도 원본에 영향을 주지 않는다.
- 참조 타입
    - 스택 영역에는 포인터(레퍼런스)만 할당되고 실제 데이터는 힙 영역에 저장됨
    - 변수를 복사하더라도 둘다 하나의 값을 가리키고 있어서 복사본과 원본이 모두 같은 값을 갖는다
    - 변수를 복사하면 레퍼런스 카운트만 +1이 되고 실제 값이 복사되지는 않는다.
<br />
<br />

## **메모리 구조에 대해 아시나요?**

- Code
    - 우리가 작성한 소스코드가 기계어 형태로 저장
    - 컴파일 타임에 저장되고, Read-Only로 저장
- Data
    - 전역변수, static 변수가 저장
    - 프로그램 시작과 동시에 할당되고, 프로그램이 종료되어야 메모리 해제
    - Read-Write 로 지정 (실행중 변수값이 변경될 수 있으니)
- Heap
    - 클래스 인스턴스, 클로저같은 **참조타입**이 저장
    - 프로그래머가 할당/해제하는 메모리 영역 → 스위프트에서는 직접 할당하지는 않음(ARC가 해줌)
    - 사용후 반드시 메모리 해제를 해야함 → 안하면 메모리 누수 발생
    - 코드, 데이터, 스택 영역과 다르게 유일하게 `런타임` 시 결정되어서 데이터의 크기가 확실치 않을 때 사용
    
    <img alt="image" src="https://user-images.githubusercontent.com/73867548/188710885-b8f911cd-f173-4335-b825-d56a7597b748.png">

    
- Stack
    - 함수 호출시 함수의 지역변수, 매개변수, 리턴 값 등이 저장
    - 함수가 종료되면 저장된 메모리도 해제됨 (자동으로 해서 신경 안써도 됨)
    - 컴파일 타임에 결정
    - 메모리 크기에 대한 제한이 있어서 메모리를 무한히 할당할 수 없는 단점,,
    - LIFO 먼저 생성된 변수가 가장 나중에 해제됨
    
    <img alt="image" src="https://user-images.githubusercontent.com/73867548/188710973-0970817c-8665-41e8-b8e3-95fabaf8c85e.png">
<br />
<br /> 

## **힙 vs 스택 언제 사용하나요?**

- 데이터의 크기를 모르거나, 스택에 저장하기엔 큰 데이터의 경우는 힙에 할당
- 그 외에는 스택에 할당
    - 스택에 너무 많은 메모리 할당시 스택오버플로우 일어나서 앱이 죽음
    - 힙 오버플로우도 있음
<br />
<br />

## **컴파일 타임과 런타임의 차이가 무엇인가?**

- 컴파일이란?
    - 사람이 작성한 언어를 기계가 읽을 수 있도록 번역하는 것
        - 컴파일 타임 에러
            - 프로그램이 성공적으로 컴파일링되는 것을 방해하는 신택스 에러나 파일 참조 오류와 같은 문제를 말하며, 일반적으로 컴파일러는 문제를 일으킨 소스코드 라인을 알려준다
- 런타임 에러
    - 이미 실행가능한 프로그램이긴 한데 프로그램이 실행 중에 발생하는 형태의 예상치 못한 오류
    - 0 나누기, nil 참조, 메모리 부족 오류 등이 있다.
<br />
<br />

## Reference

- [https://medium.com/@lucianoalmeida1/understanding-swift-copy-on-write-mechanisms-52ac31d68f2f](https://medium.com/@lucianoalmeida1/understanding-swift-copy-on-write-mechanisms-52ac31d68f2f)
- [https://github.com/apple/swift/blob/main/docs/OptimizationTips.rst#id28](https://github.com/apple/swift/blob/main/docs/OptimizationTips.rst#id28)
<br />
<br />

## 질문 - Hashable이 무엇이고, Equatable을 왜 상속해야 하는지 설명하시오.
**Hashable이란**

- 정수 해시값을 제공하여 그 자체로 유일하게 표현이 가능한 방법을 제공하는 프로토콜

**Equatable이란**

- 값의 비교가 가능함을 보장해주는 프로토콜
<br />
<br />

## **Hashable은 왜 Equatable을 상속해야하는가**

- key 값을 해싱함수를 통해 해시값으로 만든다. 해시값은 해시주소값(해시 테이블 배열의 index)이다.
- key값이 달라도 해싱함수를 통해 생성된 해시값은 같을 수 있다. 그런 경우를 해시 충돌이라고 한다.
- 해시 충돌이 발생했을 때 해시값이 아닌 다른 값으로 같은지를 비교한다. 그래서 이 때 Equatable을 통해 정의한 `==` operation에 따라서 같은 객체인지 비교한다.
<br />
<br />

## **해시 충돌이 뭔가요?**

- 다른 key의 해시 주소 값(해시테이블의 index)이 동일 할 경우를 해시충돌이라고 한다.
<br />
<br />

## **해시테이블이 뭔가요?**

- key&Value 형식처럼 보인다. 해시테이블은 배열이다.
- key를 가지고 해싱함수를 통해 해시값으로 만든다. 해시값을 통해 해시테이블에 있는 value의 index값을 알아낸다. → key 값으로 index값을 알아낸다.
- index를 통해 해시테이블에 접근해 값을 가져오거나 저장한다.

<img alt="image" src="https://user-images.githubusercontent.com/73867548/188710753-45b63712-ca57-4bea-ab12-50eea7ccc72f.png">

<br />
<br />

## **해시충돌의 해결 방법이 뭔가요?**

- Chaining 기법
    - 개방 해슁 또는 open Hashing이라고 부르는데, 해시 테이블 저장 공간 외의 공간을 활용하는 방법
    - 충돌이 일어날 경우, 연결리스트를 이용하여 데이터를 추가로 뒤에 연결 시켜서 저장한다.
    - 하나의 해시주소값에 두 개 이상의 value가 있기 때문에 key값도 같이 저장된다.
    
    <img alt="image" src="https://user-images.githubusercontent.com/73867548/188712418-2f5b7575-1d91-473f-b78a-cba222737ddf.png">

    
- Linear Probing 기법
    - 폐쇄 해싱 또는 Close Hashing이라고 부른다.
    - 해시 테이블 저장 공간 안에서 충돌 문제를 해결하는 방법이다.
    - 충돌이 일어날 경우, 해당 해쉬 주소값(index부터) 순회하며 가장 처음 나오는 빈공간에 저장하는 기법
    
<br />
<br />

## **해시테이블의 용도 및 장단점은 뭔가요?**

- 장점
    - 데이터 저장/읽기 속도가 빠르다.
    - 키에 대한 데이터의 중복 확인이 쉽다.
- 단점
    - 일반적으로 저장 공간이 많이 필요하다. (충돌 문제를 해결하기 위해 저장공간을 많이 쓴다.)
    - 충돌 발생 시 해결하기 위한 별도의 자료구조가 필요하다.
- 정리
    - 해시 테이블은 시간 복잡도와 공간 복잡도를 맞바꿨다라고 표현하기도 한다.
        - 속도는 빨라졌지만 낭비되는 저장공간이 늘어났기 때문이다.
- 용도
    - 저장, 검색, 삭제 등 탐색을 많이 하는 경우나 캐쉬를 구현할 때 사용하면 좋다고 한다.

<br />
<br /> 

## **Hashable을 사용한 경험이 있나요?**

- Set에 직접 정의한 타입을 넣었을 때 해당 타입을 hashable하게 만들어준 경헝이 있다.(Hashable을 채택했다.)
<br />
<br />

## **Hashable을 따르는 타입은 무엇이 있을까요?**

- 표준 라이브러리의 많은 타입들 또한 hashable을 준수한다.
- string, integer, floating-point, boolean values, set 등
<br />
<br />
