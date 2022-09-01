# ❔ 질문
1. TableView를 동작 방식과 화면에 Cell을 출력하기 위해 최소한 구현해야 하는 DataSource 메서드를 설명하시오. <br />
2. 하나의 View Controller 코드에서 여러 TableView Controller 역할을 해야 할 경우 어떻게 구분해서 구현해야 하는지 설명하시오. <br />

# ✏️ 작성자
- [제인, 숲재](#제인-숲재)
- [고사리, 줄라이, 허황](#고사리-줄라이-허황)
<br />

---

# 제인, 숲재
## TableView를 동작 방식과 화면에 Cell을 출력하기 위해 최소한 구현해야 하는 DataSource 메서드를 설명하시오. 
- tableView(_:numberOfRowsInSection:)
- tableView(_:cellForRowAt:)
<br />
<br />

## 하나의 View Controller 코드에서 여러 TableView Controller 역할을 해야 할 경우 어떻게 구분해서 구현해야 하는지 설명하시오.
우선 테이블뷰를 사용하기 위해서는 아래의 작업들이 필요하다.

- cell 등록 (register)
- datasource, delegate 지정

cell register는 각각의 tableView 마다 register를 해주면 되기에 문제가 되지 않는다. (스토리보드를 사용한다면 cell을 자동으로 등록해주기 때문에 register cell을 직접적으로 해주지 않아도 된다.)

하나의 뷰 컨트롤러에서 여러개의 tableView를 쓸 때 가장 문제인건 datasource와 delegate 메서드를 공유한다는 점이다. 이를 해결하기 위해선 아래와 같이 메서드 내에서의 분기를 통해 해결할 수 있다.

```swift
let tableView1: TableView
let tableView2: TableView
func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
	if tableView == tableView1 {
		// ...
	} else if tableView == tableView2 {
		// ...
	}
}

```
<br />
<br />

## prepareForReuse에 대해서 설명하시오. 
- cell이 reusableDeque배열로 들어갈 때 불리는 메서드.
- 성능을 높이기 위해서 테이블뷰의 데이터소스는 `tableView(_:cellForRowAt:)` 메서드에서 셀을 row에 할당할 때 보통 UITableViewCell을 재사용한다. 이런 셀의 재사용 구조가 큐로 되어있다.
- 테이블뷰를 위한 셀을 만들때 `dequeueReusableCell(withIdentifier:)` 메서드를 사용한다.
- 이 메서드는 기존에 존재하는 셀을 디큐하거나, 만약 없다면 등록한 커스텀 셀 클래스나 nib 파일을 이용하여 새로운 셀을 만든다.
- UITableViewCell이 reuse identifier가 있다면 dequeueReusableCell 메서드 내에서 prepareForReuse이 불린다.
- 재사용을 위해 셀을 초기화하는 코드를 여기다 작성하면 된다.
<br />
<br />

## 셀의 속성이 초기화되지 않은 경우 어떻게 해결해야하나요?
- 이 현상은 셀의 속성을 초기화시켜주지 않은 채 재사용하기 때문에 발생한다. 따라서 재사용되는 셀의 속성을 초기화시켜주면 된다.
- 예를 들어 셀에 체크박스 속성이 있는데 체크된 셀을 재사용하면 셀의 속성을 초기화해주지 않아서 새롭게 생성된 셀 중에도 체크된 셀이 있을 수 있다. 따라서 prepareForReuse를 오버라이드해서 속성을 초기화해주어야 한다.
- 그런데 모든 셀을 초기화하면 선택된 셀도 선택이 풀리기 때문에 cellForRowAt 메서드에서 선택된 셀을 체크하여 다시 선택되어있도록 바꿔야한다.
<br />
<br />

## TableView와 CollectionView의 차이점을 설명하시오.
- row versus item(row, column -> Grid)
- 테이블뷰에서는 **하나의 열에 여러 행의 cell**을 표현하는 방식이기에, cell의 모양은 항상 row에 맞춰 디자인 해야한다.
- 하지만 컬렉션뷰는 **행과 열을 모두 만들 수 있**기 때문에 cell의 모양을 자유롭게 디자인이 가능하다. 그렇기 때문에 tableView에서는 cell을 수직으로만 스크롤이 가능했다면, collectionView에서는 수직/수평 스크롤을 모두 할 수 있다.
- 또 다른 차이점은 컬렉션 뷰는 **레이아웃 객체**가 있다. 기존에 제공하는 flow layout을 사용해도 되지만, UICollectionViewDelegateFlowLayout 프로토콜을 통해 본인이 원하는 모양으로 레이아웃을 커스텀해서 사용할 수도 있다. 구현해야할 목록이나 보여주어야 할 데이터가 얼마나 복잡한지에 따라 달라지는 것 같다.
- 간단한 데이터만 보여줘도 된다면 테이블뷰, 셀의 모양을 커스텀하거나 디자인 수정 가능성이 높다면 컬렉션 뷰 사용
<br />
<br />

## 네트워킹을 통해서 셀이 이미지 표현해야 할때 고려해야 할 사항을 설명하시오.
- 네트워킹해서 이미지를 생성하는 작업을 비동기로 진행이 되어야 합니다.
- deque 시점에서 indexPath 검증

```swift
DispatchQueue.main.async {
    if indexPath == self.gridCollectionView.indexPath(for: cell) {
        cell.configureContent(product: identifier)
    }
}

```

- 이미지 캐싱
<br />
<br />

## 셀 아이템의 크기를 랜덤하게 표현하는 컬렉션뷰를 구현하는 방법을 설명하시오.
- 동적으로 사이즈를 지정하고 싶다면 UICollectionViewDelegateFlowLayout를 준수하여 메서드를 구현해야 합니다.

```swift
    func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, sizeForItemAt indexPath: IndexPath) -> CGSize {
        return CGSize(width: 10 * Int.random(in: 5...10) , height: 10 * Int.random(in: 5...10))
    }

```
<br />
<br />
<br />

---

# 고사리, 줄라이, 허황
## TableView를 동작 방식과 화면에 Cell을 출력하기 위해 최소한 구현해야 하는 DataSource 메서드를 설명하시오.
테이블 뷰를 뷰컨트롤러에서 사용하기 위해서는 데이터소스, 델리게이트를 뷰컨 자신의 인스턴스로 등록해줘야한다.

데이터 소스는 테이블뷰의 데이터를 관리하는 역할을 하고, 델리게이트는 뷰의 이벤트나 뷰의 모양을 결정하는 역할을 한다.

최소한으로 구현해야하는 데이터 소스 메서드는 넘버오브로우인섹션 메서드가 있으며 이 메서드는 섹션에 셀이 몇개가 들어갈지 정하는 메서드이다.

- func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int
    - cell의 갯수를 정의한다.

다른 메서드로 셀포로우엣 이라는 메서드는 테이블뷰나 컬렉션뷰가 셀을 정의할 때 호출되는 메서드로 셀에 표현하고 싶은 데이터가 있을때 이 메서드 내부에서 셀에 데이터를 넘겨주면 셀을 개발자가 원하는 방향으로 데이터를 넣어줄 수 있다.

- func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell
    - 셀을 정의한다.
<br />
<br />

## 하나의 View Controller 코드에서 여러 TableView Controller 역할을 해야 할 경우 어떻게 구분해서 구현해야 하는지 설명하시오. 
테이블 뷰마다 셀의 모양이 다를 경우 register 함수를 이용해서 셀을 등록해야한다.

- func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell

하나의 뷰 컨트롤러에서 여러 테이블뷰를 관리할 경우 테이블뷰들의 데이터소스메서드가 동일하게 동작하기 때문에 위 메서드 내부에서 테이블 뷰를 구분할 필요가 있다. 예시 테이블 뷰마다 테그를 이용하는 방법도 있음.
<br />
<br />

## prepareForReuse에 대해서 설명하시오. 
> tableview 의 delegate가 셀을 재사용하도록 준비합니다. UITableViewCell 객체가 재사용 가능한 경우 이 메서드는 UITableView 에dequeueReusableCell(withIdentifier:) 메서드에서 객체가 반환되기 직전에 호출됩니다.
> 

테이블 뷰는 셀을 매번 생성하는 것이 아니라 재사용한다.

cellForRowAt메서드에서 셀을 재사용하기 때문에 셀의 배경색을 바꾼거나, 데이터를 바꾸는 상황이 생기면 아래쪽에 있는 셀이 재사용되어 화면에 표시될 때 그래로 유지할 경우가 생긴다.

prepareForReuse 메서드를 셀 내부에서 오버라이드 해주면 셀이 재사용되기 전에 호출되는 메서드이기 때문에 위와 같은 상황을 해결할 수 있다.
<br />
<br />

## TableView와 CollectionView의 차이점을 설명하시오.
둘다 스크롤 뷰를 상속받고 있기 때문에 많은 양의 정보를 제공할 때 이점이 있다.

간단한 데이터만 보여줘도 된다면 테이블 뷰를 사용해도 좋다.

테이블 뷰는 수직으로만 스크롤 할 수 있다.

하지만 셀을 커스텀 하거나 뷰를 원하는 디자인으로 구성하고 싶다면 컬렉션 뷰를 사용하는 것이 좋다.

iOS 14 부터 사용할 수 있는 UICollectionLayoutListConfiguration를 사용하면 컬렉션 뷰를 사용하더라도 테이블 뷰 형태의 뷰를 구성할 수 있다.
<br />
<br />

## 비동기 작업이나 무거운 작업을 할 때 Cell을 빠르게 넘겼을 때 데이터가 누락되거나 다른 데이터가 셀의 이미지에 들어가는 경우가 있는데 해결하는 방법은 무엇이 있을까? 
- 허황: 무거운 작업을 다시 수행하지 않도록 캐시를 이용하는 방법도 있을 수 있다.
cellForRowAt 메서드 내부에서 비동기 작업(escaping closer)를 할 경우 클로져의 정의시점과 호출시점이 다르기 때문에 indexPath를 비교하는 방법도 있다.

- 고사리: Cell이 재사용될 때 기존에 진행하고 있던 비동기 요청이 문제가 되는 것이라고 보았을 때, 셀을 초기화하는 메서드에서 비동기 요청을 취소하고 이미지를 비워주는 것으로 누락이나 꼬임이 해결될 수 있다고 생각된다.

- 줄라이: indexPath를 통해 각 tableView, collectionView에 indexPath와 현재 indexPath를 검증하면 된다.
<br />
<br />

## Cell을 재사용할 때, 컨텐츠와 관련된 셀의 초기화는 어디에서 진행되어야 하나? 그리고 그 이유는 무엇인가?
컨텐츠와 관련없는 항목(alpha, editing, selection state 등)은 `prepareForReuse`, 컨텐츠와 관련된 항목은 `cellForRowAt`에서 초기화를 진행해야 한다.

> To avoid potential performance issues, you should only reset attributes of the cell that are not related to content, for example, alpha, editing, and selection state. The table view's delegate in tableView(_:cellForRowAt:) should always reset all content when reusing a cell.
>
<br />
<br />

## CollectionViewFlowLayout 과 flowlayoutDelegate의 차이는 무엇입니까?
- flowLayout은 코드가 간결하다. flowLayoutDelegate는 해당하는 viewController에 채택해주어야한다. delegate의 메서드들은 모두 선택사항이며, 셀의 크기를 반환하거나 셀 사이의 최소간격을 설정하거나 지정된 섹션의 헤더뷰의 크기를 반환하는 등 디테일한 작업들을 해줄 수 있다.
<br />
<br />

## DiffableDataSource의 사용 방법에 대해서 알고 계신가요?
- DiffableDataSource는 snapshot을 통해 데이터가 다른 부분만 바꿔주어서 reloadData보다 성능이 좋다. dataSource를 정의하고 snapshot의 원하는 section에 데이터를 append 해주고 apply snapshot하면 된다.
<br />
<br />

## DiffableDataSource를 쓸 때 주의할점이 무엇이 있을까? 
- section과 data 모두 hashable을 채택하여야 한다.
<br />
<br />

## 테이블 뷰 컨트롤러와 뷰 컨트롤러의 차이?
- 테이블 뷰 컨트롤러를 사용하면 데이터소스, 델리게이트를 테이블 뷰 컨트롤러가 수행하기 때문에 자기 자신(self)를 넘겨줄 필요는 없다.
하지만 테이블 뷰 컨트롤러는 테이블 뷰 이외에 다른 뷰들을 컨트롤하는데 어려움이 있기 때문에 확장성을 고려한다면 뷰 컨트롤러를 사용하는게 좋다고 생각한다.
<br />
<br />

## 테이블 뷰의 데이터 소스 역할을 할 수 있는 객체는 뷰컨트롤러 뿐인가? 뷰컨트롤러말고 다른 객체를 사용해본 경험이 있거나, 없다면 다른 객체를 써야하는 이유는 무엇인가?
뷰 컨트롤러에서 테이블 뷰, 컬렉션 뷰의 데이터소스 역할까지 하게 되면 뷰 컨트롤러가 비대해질 수 있다. 별도의 객체를 만들고 역할을 분리하면 뷰 컨트롤러가 비대해지는 현상을 방지할 수 있다.
<br />
<br />

## 기능을 구현할 때 테이블 뷰나 컬렉션 뷰를 사용해야할 경우가 있다면 어떤 기준을 두고 사용하는가?
- 표현해야할 뷰의 모양
- 스크롤 방향(테이블 뷰는 수직 스크롤만 지원)
- 최소 iOS 버전(모던 셀, 일반 컬렉션 뷰 등)
<br />
<br />

## collectionView, tableView reloadData 완료 시점
`reloadData()`가 실행되면 아래와 같은 순서로 진행된다.

1. 아이템의 갯수에 따라 셀의 높이나 너비를 결정한다.
2. 화면에 표시될 만큼의, 혹은 다음에 노출될 만큼의 셀을 만든다.

그 이후 호출되는 메서드 `layoutSubviews()`를 통해 데이터 리로딩이 모두 끝났음을 알 수 있다.
<br />
<br />
<br />
