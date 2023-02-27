
# CSS Flex


## display:flex;
- 컨테이너에 먼저 적용하며 시작 
- `display: inline-flex` 
    - block과 inline-block의 관계처럼 `컨테이너`가 주변 요소들과 어떻게 어우러질지 결정하는 속성
// inline -flex는 inline-block처럼 적용한 컨테이너가 inline요소처럼 컨텐츠영역만 너비를 차지합니다 

<img width="884" alt="image" src="https://user-images.githubusercontent.com/78556338/221488166-98c97979-3123-40b4-b735-64eb77af48c7.png">

<br>

### flex items

<img width="886" alt="image" src="https://user-images.githubusercontent.com/78556338/221488037-abccfd49-b87d-4252-80fd-88395fe560b0.png">

- flex 컨테이너의 자식요소를 말합니다.
- 컨테이너속에 `가로방향`으로 기본배치
- flex Item은 자신이 가진 `내용물의 width`만큼만의 너비를 차지합니다.(마치 inline 요소처럼)
- height는 컨테이너의 높이만큼 늘어납니다.

<br>

### flex-direction

<img width="882" alt="image" src="https://user-images.githubusercontent.com/78556338/221488228-c937f867-4bea-4edb-ae20-5dd8a769db07.png">

- 메인축(Main-Axis)의 방향을 결정하는 속성(기본값:row)
- `flex-direction:row ;`//기본값
- `flex-direction:column ; 세로 
- row-reverse
- column-reverse 

<br>

### flex-wrap(줄넘김 처리 설정)

<img width="882" alt="image" src="https://user-images.githubusercontent.com/78556338/221488605-4e9e4768-bd02-4ef4-b039-0e029b67917b.png">

`flex-wrap: nowrap; (기본값)`
- 컨테이너에 지정합니다.
- nowrap(기본값): 줄넘김처리를 하지 않습니다, 넘치면 빠져나가요
- wrap: 줄넘김처리
- wrap-reverse: 줄넘김시 요소가 위로 빠져나감

## `축 설정`
- flex-direction: row(기본값) | column(세로축을 메인축으로)
// 컨테이너에 메인축의 방향을 설정 
// 메인축은 꼬치 방향, 수직축은 꼬치의 수직방향
- justify-content: 메인축방향의 아이템 정렬 속성
- align-items: 수직축 방향의 아이템 정렬 속성

### flex-flow = direction+wrap
`flex-direction(메인축 결정), flex-wrap(줄넘김처리)`을 한번에 지정하는 단축속성
- `flex-flow: row wrap;` 처럼 속성을 순서대로 나열해서 사용함.

<br>

### justify-content(`메인축` 방향정렬)
```css
.container{
	justify-content: flex-start; 
// flex-start(기본값) 
// flex-end 
// center 중앙정렬 
// space-between 양끝으로 요소 보내고, 남은여백 균등히 
// space-around 요소를 중심으로 around(둘레)에 여백을 균등히해줌(요소간 여백공유X)
// space-evenly 요소를 중심으로 모든 테두리 여백을 균등히(요소간 여백 크기를 공유O)
}
```

<br>

### align-items(`수직축` 방향 정렬)
- 메인축(꼬치)의 세로방향으로 정렬하는 속성 
- 메인축(main-Axis)이 세로(column)로 수정될 경우 `정렬축이 반시계 방향으로 90도` 돌아감

```css
.container{
	align-items: flex-start;
  }
// stretch(기본값): 아이템들이 수직축 방향으로 컨테이너 끝까지 쭉 늘어남
// flex-start(위,왼쪽): `메인축(flex-direction)이 가로(row)일때는 위`, `메인축이 세로(column)일때는 왼쪽`
// flex-end: `메인축 가로 -> 아래`, `메인축 세로: 오른쪽` 
// center
// baseline :텍스트 베이스라인 기준
```

<br>

>flex-(start->end) :    main축(가로): (justify-content)좌->우   main축(가로): (align-items)위->아래  
>flex-(start->end) :    main축(세로): (justify-content)위->아래  /  main(세로): (align-items)좌->우



<br>

### align-content(여러 행 정렬)
- align-items와의 차이: `flex-wrap:wrap;(줄바꿈O)`이 설정된 상태에서, `아이템들의 행이 2줄이상 되었을 때`의
수직축 방향 정렬을 결정하는 속성
// stretch: 아이템들이 줄바꿈 된 상태로 stretch됨
// flex-start : 줄바꿈된 아이템들이 컨테이너의 위쪽에 위치
// flex-end : 줄바꿈된 아이템들이 컨테이너 아래쪽에 위치 
// center: 
// space-between: 세로축으로 정렬되며 일부는 위쪽, 줄바꿈된 아이템은 아래쪽에 위치하고 여백을 균등히
// space-around: 세로축으로 정렬되며 여백을 균등히 갖고, 각 아이템이 여백을 공유하지 않음
// space-evenly: 세로축정렬, 여백을 균등히갖고, 각 아이템이 여백을 공유함 

<br>

## flex-basis(아이템의 `기본`크기를 직접지정, `기본값auto`)
- flex 아이템의 `기본 크기(좌우 너비)`를 설정하는 속성(메인축이 세로일때는 상하 높이)
// 메인축이 row일때는 `너비`, 메인축이 column일때는 `높이`
- flex-basis로 설정되는 값 : `auto(기본값) , 0, 50%, 300px, 10rem, content`
- `flex-basis:auto;` : flex-basis를 따로 설정하지 않으면 기본값(auto)로 셋팅되고 해당 아이템의 width값으로 설정됩니다. width값을 따로 주지 않았다면 `컨텐츠의 크기`가 됨.
// auto -> `width값 | 컨텐츠 크기`
`__즉, basis를 따로 주지않으면 기본값auto = width이고  만약 width 속성도 없다면 내부 컨텐츠 크기만큼 따름.__`

<br>

### 아이템 너비로 flex-basis값 vs width 
- __flex-basis는 `유연한 값`이다__
1. width가 100px이 안되는 AAA와 CCC는 100px로 늘어난다.
2. width가 100px이 넘는 BBB(150px)는 그대로 그 값을 유지한다
- __반면 width는 `고정된 절대적 값`이다__

> **word-wrap: break-word 설정을 해주면 아이템속의 문자 컨텐츠가 아이템을 삐져나가지 않고 줄바꿈처리됩니다.

<br>


### flex-grow(유연하게 늘리기): `기본값0`
- flex-grow는 아이템이 `flex-basis의 값보다 커질 수 있는지`를 결정하는 속성입니다.
// 기본값인(0)보다 큰 값이면 flex-basis보다 커지며 여백을 메운다.
- 값에는 `숫자값`이 지정되며 `기본값(0)보다 크면 해당 아이템이 유연한 박스로 변하고 원래의 크기보다 커지며 빈 공간을 메우게 된다`.
```css
.item{
flex-grow: 1;
//flex-grow:0; 기본값
```

- 비율로 세팅하면 ex) grow값을 비율로 1:2:1 처럼 설정하면 아이템들의 컨텐츠를 제외한 나머지 여백을 해당 비율로 나눠갖는다.

<img width="882" alt="image" src="https://user-images.githubusercontent.com/78556338/221501593-e2ddabe9-498f-4271-ab20-c2af65a72e2a.png">

### 유연하게 줄이기(유연하게 줄이기): `기본값1`
- flex-basis보다 작아질 수 있는지 결정합니다.
- 몇이든 `일단 0보다 큰 값이 세팅되면` 해당 아이템이 유연한(Flexible)박스로 변하고 flex-basis보다 작아집니다.
- 기본값이 1이기 때문에 따로 세팅을 하지 않아도 아이템이 flex-basis보다 작아질 수 있습니다.
- flex-shrink가 0으로 세팅되면 아이템 크기가 flex-basis보다 작아지지 않기 때문에 고정폭의 컬럼을 쉽게 만들 수 있습니다.

<br>

### flex (grow-shrink-basis)순서
- grow, shrink, basis 축약속성 
```css
.item{
	flex: 1;      // 1 1 0
	flex: 1 1 auto; // 1 1 auto 
	flex: 1 500px;   // 1 1 500px
```
- `flex:1;`의 basis값은 0이다.
- `basis가 0일경우`: 기본점유 크기가 0이므로 `전체 영역크기를 flex-grow비율로 나눠갖는다.`

<br>

### align-self(수직축으로 아이템 정렬)
- 기본값은auto , 기본적으로 align-items설정을 상속받음
- align-items보다 우선권이 있음 (align-content는 줄바꿈 관련속성)
- 아이템 하나만 해당 위치를 수직으로 정렬하는것임.

<br>

### 배치순서 Order
- 작은 수일수록 먼저 배치 ( 1 -> 2 -> 3 )
- html구조를 바꾸는 것은 아니므로 접근성측면 고려

<br>

### z-index
- Z축 정렬속성
- 숫자가 클 수록 위로 올라옴(기본값0이므로 1만 설정해도 나머지 아이템보다 위로 올라옴)

<br>

### 출처:
[이번에야말로 CSS Flex를 익혀보자_1분코딩](https://studiomeal.com/archives/197)
