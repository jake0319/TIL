# Grid
## Grid 기본구조
>flex는 1차원(한방향)
Grid는 두방향(가로-세로)
2차원 레이아웃 시스템
따라서, Flex보다 더 복합적인 레이아웃 표현할 수 있다.

- 전체 페이지 레이아웃 :grid
- 부분적 레이아웃: flex 
섞어서 사용해주면 좋다.

Grid 레이아웃을 만들기 위한 기본적인 HTML 구조는 다음과 같습니다.
Flex와 마찬가지로, 컨테이너와 아이템이 있으면 됩니다.

```html
<div class="container">
	<div class="item">A</div>
	<div class="item">B</div>
	<div class="item">C</div>
	<div class="item">D</div>
	<div class="item">E</div>
	<div class="item">F</div>
	<div class="item">G</div>
	<div class="item">H</div>
	<div class="item">I</div>
</div>
```
- Grid컨테이너에 `.container{ display : grid}` 를 지정하면 그리드 아이템은 그리드의 제어를 받음.
- grid적용시 Item이 block요소 -> 라인 1줄 전부 차지 /  inline요소 -> 컨텐츠영역만 차지

- 그리드는 Grid 컨테이너에 display: grid;를 적용하는게 시작이에요.
- 아이템들이 block 요소라면 이 한 줄 만으로는 딱히 변화는 없습니다.
```css
.container {
	display: grid;
	/* display: inline-grid; */
}
/* 그리고 아무 일도 일어나지 않았다 */
```

>cf) block요소 특성  
기본 너비값이 100% (폼 엘리먼트 제외)  
너비값이 100%를 차지하기 때문에 각 요소가 수직으로 쌓인다.

<br>

## 용어정리

<img width="906" alt="image" src="https://user-images.githubusercontent.com/78556338/221779322-74755640-4983-49db-a668-33b23c5c05da.png">

- 그리드 컨테이너: `ex) div{ display : grid }`로 그리드를 적용시켜주는 영역
- 그리드 아이템: 그리드 컨테이너의 `자식요소`들
- 그리드 라인: 그리드컨테이너 `좌상단`에서 지점에서 1,2,3,4...로 시작
//`ex) grid-column: 1 / 3 , grid-row: 1 / span`처럼 그리드 아이템이 차지하는 셀 영역을 설정해줄 수 있음.
- 그리드 셀: 그리드 라인으로 구분되는 셀 영역
- 그리드 영역: 그리드 셀영역의 집합 
- 그리드 갭: 그리드 셀 사이의 간격 `ex) grid-gap: 1rem 1rem`

<br>


## 그리드의 기본 형태(template-rows&columns)
 
### grid-template-columns
`grid-template-columns : 1fr 1fr;`
- 그리드 `컨테이너에 지정`해주는 속성

- 칼럼(열)의 `가로폭`을 지정한다

- fr:상대적인 비율만큼 가로폭을 나눈다 ex) 1fr 2fr 500px(고정칼럼)
//fr은 꽉 채울 수 있는 (여백)공간이 있으면 그것을 다 채우는 특성이 있다.

- 띄어쓰기는 나눠진 열 갯수의 기준이 된다.

- `repeat( n:반복횟수 , 반복 패턴)` 
ex) grid-template-columns: repeat( 3, 1fr, 2fr ,3fr) 
// 1fr 2fr 3fr 패턴이 3번 반복된다(9줄 컬럼 형성)

- 지정하지 않은 칼럼에 대해서는 자동으로 auto가 지정된다.
// 200px 200px 지정후 남는 아이템에 대한 지정은 Auto
// auto가 지정되면 flex-grow처럼 동작해서 컨테이너 여백을 채운다.

<br>

### grid-template-rows
`grid-template-rows: 1fr 2fr 300px `

- 가로행의 `세로폭` 크기 지정한다.

- 200px 200px (auto);
//배치할 아이템이 더 남는경우 auto가 암묵적으로 지정된다.

- repeat(3,1fr)  
// 컨테이너 height가 지정되지 않는다면 높이가 늘어나지않음
// 컨테이너 height를 지정해준다면 그 때 아이템 높이가 늘어난다.

<br>

```css
.container {
	/// 컬럼이 3개생성, 각 200,200,500 px의 너비를 갖도록 그리드영역이 생성된다.
	grid-template-columns: 200px 200px 500px;

///... 1:1:1비율로 3줄 
	/* grid-template-columns: 1fr 1fr 1fr */
	/* same as, grid-template-columns: repeat(3, 1fr) */

///... `고정크기와 가변크기를 섞어쓰기` fr:남은여백을 `flex-grow`처럼 전체차지 
///... 1fr , auto속성은 flex-grow 처럼 유연한 설정이다.
  /* grid-template-columns: 200px 1fr; */
	/* grid-template-columns: 100px 200px auto; */

	grid-template-rows: 200px 200px 500px;
	/* grid-template-rows: 1fr 1fr 1fr */
	/* grid-template-rows: repeat(3, 1fr) */
	/* grid-template-rows: 200px 1fr */
	/* grid-template-rows: 100px 200px auto */

	/* grid-template-rows: 70px auto */
	/* (same as above),  grid-template-rows: 70px 1fr */

}
```

<br>

## 자동으로 채우는 방법
### 최대,최솟값 설정하기 minmax(반복횟수,반복값)
`grid-template-columns: repeat(3, minmax(100px,auto));`
- 컬럼의 가로폭이 `최소 100px을 보장하고, 컨텐츠양에 따라 auto로 늘어난다.`, 컬럼 3줄 생성

`grid-template-rows(가로행_세로폭): repeat(3, minmax(100px, auto));`
- 로우의 세로폭이 `최소 100px은 차지하면서 컨텐츠 높이에 따라 자동으로 늘어나도록(auto)`, 로우 3줄 생헝


<br>

### auto-fill과 auto-fit 
__ex) grid-template-rows: repeat(auto-fill, minmax(20%,auto))__
__ex) grid-template-columns: repeat(auto-fit, minmax(25%, auto))__
`repeat()함수 안에서 사용 가능한 특별한 속성`
- auto-fill 속성은 minmax값을 기반으로 `컨테이너에 최대갯수를 채울 수 있는 배치`를 합니다.
- auto-fit 속성은 minmax값을 기반으로 `컨테이너 너비를 fit하게 채울 수 있는 배치`를 합니다.
>auto-fill과 auto-fit은 column및 row의 `개수를 미리 정하지 않고` 자동으로 지정하는 속성임

<br>

**auto-fill**
```css
.contatiner{
grid-template-columns: repeat(auto-fill,minmax(20%, auto));
}
```
- 최소 컨테이너의 20%너비를 차지하는 컬럼들을 컨테이너를 꽉 채울 수 있도록 자동으로 생성한다.(media query안쓰고도 반응형으로 만들어주는 효과도 있음)

<br>

**auto-fit**
```css
.contatiner{
grid-template-columns: repeat(auto-fit,minmax(200px, auto));
}
```
- 최소 200px너비의 컬럼들을 자동으로 생성하고 컨테이너를 꽉 채운다
 
<br>

 
## 셀 간격 만들기
### gap
**그리드 셀 사이의 간격을 설정합니다.**
- row-gap: 가로간격만 지정
- column-gap: 세로간격만 지정
- gap : 1rem 2rem // 축약속성) 순서대로 row , column

<br>

## 그리드 형태를 자동(auto)으로 정의
- grid-auto-columns
- grid-auto-rows
> grid-template-(columns/rows)속성의 통제를 벗어난 위치에 있는
트랙의 크기를 지정하는 속성입니다.
template자리에 auto가 들어간다고 생각하세요.

```css
.container {
	grid-template-columns: repeat(3,1fr);
	grid-auto-rows: minmax(100px, auto);
	gap: 1rem; 
}
```
> 컬럼의 너버는 1:1:1 비율로 3줄을 생성하고, 가로행의 높이는 최소100px유지하면서 컨텐츠양에 따라 자동으로 늘어나는 너비를 갖게끔 자동으로 너비를 설정해줌
> 예시) 계속 로드되서 추가되는 post같은 곳(무한스크롤)에 적용해줄 수 있다  
why?) 컨텐츠 양에 따라 자동으로 높이가 설정되므로..

<br>

## 각 셀의 영역 지정하기
**셀의 시작과 끝 라인넘버를 직접 지정해주기(비추)**
- grid-column-start : 1
- grid-column-end : -2
- grid-row-start : 3
- grid-row-end : 2

<br>

**`start/end 넘버를`를 축약으로 지정하기**
- grid-column: 1 / 3
// span: 차지하는 그리드 셀의 갯수
- grid-column: 1 / span2 
- grid-row: 1 / 4
- grid-row: 2 / span2

<br>

**그리드영역의 그리드라인은 `음수값`도 지정 가능함**
// 3번라인 ~ 끝라인 
- grid-row: 3 / -1(끝라인)  

// 3번~ 끝에서 두 번째 라인
- grid-column: 3 / -2(끝에서 두 번째 라인)   

// end넘버를 생략하면 3 / 4 와 동일
- grid-column: 3

<br>

**grid는 겹칠 수 있다.**
- 그리드 영역을 조절하면 셀D가 셀C를 덮을 수 있고 그리드는 이것이 가능하다
- 겹쳐진 아이템의 투명도(opacity:)를 조정해서 창조적인 디자인을 만들어 낼 수도 있다.

<br>

## 통제,미통제 영역을 지정해주기
>`grid-auto-columns`는 `grid-template-columns`의 `통제를 받지 않는 column`들의 배치를 결정하는 규칙이다.

```css
.container {
	grid-template-columns: 50px;  ///...처음 열의 폭만 50px 강제지정
	grid-auto-columns: 1fr 2fr; ///...나머지 통제받지 않는 영역을 세팅
.item:nth-child(1) { grid-column: 2 ; } ///... 2 / 3 : 1칸차지
.item:nth-child(2) { grid-column: 3 ; } ///... 3 / 4 : 1칸차지
.item:nth-child(3) { grid-column: 4; }
.item:nth-child(4) { grid-column: 5; }
.item:nth-child(5) { grid-column: 6; }
.item:nth-child(6) { grid-column: 7; }
}
```
<img width="905" alt="image" src="https://user-images.githubusercontent.com/78556338/221784520-e23e7e38-b465-4fc0-9d79-309d881469e6.png">

<br>


### '이름'으로 그리드 영역을 정의하기
- 컨테이너에 `grid-template-areas: " grid-area이름 설정 "` 

<img width="905" alt="image" src="https://user-images.githubusercontent.com/78556338/221785368-f199ff70-8294-4783-bd80-47e0b39eec11.png">

```css
	.container{ 
		grid-template-areas: 
			" header header header" ///.header{grid-columns: 1 / 4} 와 같음
			" a b c "
			" . . . "
			" footer footer footer"  ///.footer{ grid-columns: 1 / 4} 와 같음
			}
			```
- `grid-area 설정이름`은 영역을 띄어쓰기로 구분하고, 간격은 굳이 맞출 필요 없음
- `마침표(.)`는 빈 셀을 의미한다

<br>

### 그리드 아이템과 grid-area이름 매칭시키기
1. 컨테이너에 `grid-template-areas`속성으로 grid-area영역을 지정해줬다면  
2. 각 그리드 아이템별로 grid-area 이름을 매칭시켜줘야한다.

```css
.header{ grid-area: header;}
.sidebar-a { grid-area: a;}
.main-content{ grid-area: main;}
.sidebar-b { grid-area: b;}
.footer{ grid-area: footer;}
```
- 이름 값 매칭시에는 따옴표를 붙이지 않는 것에 주의한다!

<br>

### grid-template-areas 설정 예시) 
```html
<head>
<style>
.grid-container{
  display: grid;
  gap: 1rem;
  grid-template-columns: 1fr 3fr 1fr; ///...각 셀의 폭 지정하기
  grid-template-areas:  ///...이름으로 각 셀의 배치영역 지정해주기(밑에 지정되므로 설정이 우선된다)
	'header header header' ///... 1 / 4
	'sidebar-a main sidebar-b' ///... 1: 3: 1
	'footer footer footer';  ///... 1 / 4
}
.header { grid-area: header;}
.sidebar-a { grid-area: sidebar-a;}
.sidebar-b { grid-area: sidebar-b;}
.main { grid-area: main;}
.footer{ grid-area: footer}
</style>
</head>
<body>
	<div class="grid-container">
		<div class="header grid-item">Header</div>
		<div class="sidebar-a grid-item">Sidebar A</div>
		<div class="sidebar-b grid-item">Sidebar B</div>
	</div>
```

<br>

## 자동 배치 속성 설정하기
`grid-auto-flow`
- 컨테이너에 그리드 아이템의 자동 배치 알고리즘을 정하는 속성입니다.

```css
.container {
	display: grid;
	grid-template-columns: repeat(auto-fill, minmax(25%, auto));
	grid-template-rows: repeat(5, minmax(50px,auto));
	// 셀이 비지 않도록 빽빽히 아이템을 배치한다
	grid-auto-flow: dense;
}
///... 어디서 시작할지 모르지만(auto),셀 3칸 차지     
item:nth-child(2) { grid-column: auto / span 3; } 

item:nth-child(5) { grid-column: auto / span 3; }
item:nth-child(7) { grid-column: auto / span 2; }
```
- row(좌에서 우로 순서대로 배치, `기본값`)
- column(위에서 아래로 순서대로 배치)
- dense(배치간 여백을 꽉 채워 배치)
- row dense
- column dense 

<br>

## 정렬하기~ flex와 거의비슷
> flex에서는 flex-direction이 존재하여 메인축이 가로,세로로 유동적으로 바뀌지만, Grid는 축이 존재하지 않으므로 오로지 가로-세로 개념임.

<br>

### align-items:
`모든 그리드 아이템들을 (각자의 셀 위치에서) 세로(column축)방향으로 정렬하는 속성.`

<br>

<img width="910" alt="image" src="https://user-images.githubusercontent.com/78556338/221787047-273643d8-62f8-47c8-adae-d0375d5e3e21.png">

- stretch (`기본값`,컨테이너 높이설정시 아이템이 컨테이너 끝까지 늘어난다)
- start (셀의 위)
- center (셀의 중앙)
- end (셀의 끝))

<br>

### justify-items:
`모든 그리드 아이템들을 각자의 셀 위치에서 가로(row축)방향으로 정렬합니다.`
- stretch (기본값,컨테이너 높이에 따라 알아서 끝까지 늘어난다)
- start
- center
- end

<br>

### place-items 
`align-items & justify-items` 함께 쓰는 축약속성
- align-items(세로) justify-items(가로) 순서로 작성한다.
```css
.container{
 place-items: center start; ///...순서: 세로 가로 
} 
 ```

<br>

### 그리드아이템을 그룹지어 세로정렬(align-content)
- __`align-content: start;`__
<img width="912" alt="image" src="https://user-images.githubusercontent.com/78556338/221787778-c2d5be57-7798-4782-a895-29faf19c301e.png">

- 아이템을 (컨테이너 단위)에서 그룹으로 묶어서 __`세로`__ 정렬합니다.
- Grid아이템들의 높이를 모두 합한 값이 Grid컨테이너 높이보다 작을 때 Grid 아이템들을 통째로 정렬합니다.
- stretch(`기본값`,컨테이너를 끝까지 채움)
- start
- center
- end
- space-between(양끝 배치 후, 사이 공간을 동등히)
- space-around(각 아이템별 좌우 마진을 동등히 부여, 각각의 마진을 보장한다(겹치지않음)
- space-evenly(전체 공간에서 모든 여백을 동등히 나눠갖는다(마진공유)

<br>

### 아이템그룹 가로정렬(justify-conent)
- 아이템을 (컨테이너 단위)에서 그룹으로 묶어서 __`가로`__ 정렬합니다.
- stretch
- start
- center
- end
- space-between(양끝 배치 후, 사이 공간을 동등히)
- space-around(각 아이템별 좌우 마진을 동등히 부여, 각각의 마진을 보장한다(겹치지않음)
- space-evenly(전체 공간에서 모든 여백을 동등히 나눠갖는다(마진공유)

<br>

### 축약속성 place-content(세로,가로순서)
`place-content: space-between center;` (컨테이너속성, align세로&justify가로)

<br>

### 그리드 아이템 하나만 세로 정렬 align-self
- `특정 아이템 하나에만 적용하는 속성이다.`
- 특정 아이템 하나만 `셀 영역에서 세로 정렬을 위치를 지정`한다.
- stretch(기본)
- start
- center
- end

<br>

### 개별아이템 가로 정렬 justify-self
- `특정 아이템에만 적용하는 속성이다.`
- 특정 아이템 하나만 셀 영역에서 가로 정렬을 위치를 지정한다.
- stretch(기본)
- start
- center
- end

<br>

### 축약 place-self
`.아이템{ place-self: (align) (justify)}`

<br>

### 아이템 배치 순서 order
- 그리드아이템에 설정한 order값이 낮을수록 먼저 배치된다.
- 시각적인 순서만 바꿀 뿐이지 html구조를 바꾸진 않으므로 접근성측면 고려가 필요하다.

<br>

### Z축 정렬  z-index 
`숫자가 클수록 위로 올라온다`
- 기본값은 0이므로 1만 설정해도 다른 아이템보다 올라온다.
- position: abosulte와 같은 설정이 필요 없음 (flex&grid가 자체로 배치해줌)

<br> 
<br> 

### 출처:   
- [1분코딩-CSS Flex와 Grid 제대로 익히기](https://studiomeal.com/archives/533)


 
