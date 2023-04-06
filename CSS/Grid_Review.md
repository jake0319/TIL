# CSS GRID 보충개념

## auto-fill과 auto-fit의 차이 

- auto-fill (가능한 많이 채우기, 항목의 사이즈 작아질 수 있음)
:auto-fill은 가능한 한 많은 항목을 그리드에 채우기 위해 항목의 크기를 줄이는 것을 선호합니다. 그러나 그리드의 크기가 충분하지 않으면 빈 공간이 남아 있을 수 있습니다. 

- auto-fit (가능한 많이 채우되, 항목의 사이즈는 유지)
:auto-fit은 가능한 한 많은 항목을 그리드에 채우되 항목의 크기를 유지하는 것을 선호합니다. 

<br>


## grid-template-columns/rows: options
- columns: 컬럼의 가로폭을 지정합니다
- rows: 로우(행)의 세로폭을 지정합니다

<br>


## grid-template-columns/rows: `options`
1.options는 직접 행과 px값을 지정할 수 있고
// ` 200px 200px auto` ,`minmax(100px,1fr)` (패턴)
// auto와 1fr은 컨테이너의 나머지 영역에 반응하는 유연한 값이다.

2. repeat( 반복수, 반복패턴) 
- `반복수`: `auto-fit/fill` 옵션이나 `정수`값이 들어감 
- `반복패턴`: `px값` 혹은 `minmax(60px,1fr)`처럼 가변적인 값이 들어갈 수 있음

<br>


## grid-auto-columns/rows: option
- grid-template-(columns/rows)속성의 `통제를 벗어난 위치에 있는 트랙의 크기를 지정`하는 속성입니다. template자리에 auto가 들어간다고 생가하기.

```css
.container {
	grid-template-columns: repeat(3,1fr);
	grid-auto-rows: minmax(100px, auto);
	gap: 1rem; 
}
```
- 위 코드는 그리드 rows를 자동으로 정하겠다는 의미이며, 무한스크롤로 포스트가 업데이트 되는 UI등에 적용가능하다.
(컨텐츠에 따라 그리드 row 수를 가변적으로 변경시키겠다는 의미)
// 무한스크롤 영역에 grid적용시 활용하자...

- `grid-auto와 grid-template이 함께 있을 시 적용 순서는 grid-template => grid-auto이다(grid-template의 지배를 받는 영역 이외의 영역에 grid-auto가 적용됨!`

### tips)
- `grid-auto-rows: minmax(150px,auto); ` 지정시  
__그리드 아이템의 컨텐츠 길이(lorem n으로 테스트가능)에따라 최소150px이상의 height를 확보하게 만들 수 있다.__

<br>

## 그리드 셀의 영역을 직접 지정하기 (grid-column, grid-row)
- 그리드 template-columns/rows 을 지정했으면, 그리드 아이템이 차지하는 셀의 영역또한 조정해줄 수 있다.
- 특정 그리드 아이템을 셀렉터로 선택하고 `grid-column: m/n`,\
`grid-row: m/n` 을 지정해주면 해당 아이템의 그리드 셀 영역이 지정된다.

1. grid-column은 셀의 가로 영역을 지정한다.
2. grid-row는 셀의 세로 영역을 지정한다.
3. -1 숫자는 끝 지점을 의미  (ex: grid-column: 1 / -1 )
4. 값을 m/n이 아니라 span n 형태로 `몇 칸을 차지하는지`지정해줄 수 있음.

<br>

## 그리드 template과 auto를 함께 사용하기
> grid-auto-columns는 grid-template-columns의 통제를 받지 않는 column들의 배치를 결정하는 규칙이다.

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
- 첫번째 컬럼의 가로폭만 50px로 고정한다.
- grid-auto-columns로 첫번째 50px컬럼을 제외한 미통제 영역의 폭에 대해 설정한다. 


<br>

## '이름'으로 그리드 영역을 정의하기 
1. `그리드 컨테이너`에 grid-template-areas: `템플릿`; 설정 
```
.container{ 
		grid-template-areas: 
			" header header header" 
///.header{grid-columns: 1 / 4} 와 같음
			" a b c "
			" . . . "
			" footer footer footer"  
///.footer{ grid-columns: 1 / 4} 와 같음
			}
```

2. `그리드 아이템에` grid-area: `이름` 매칭시키기


<br>

## 정렬속성 
> flex에서는 flex-direction이 존재하여 메인축이 가로,세로로 유동적으로 바뀌지만, Grid는 축이 존재하지 않으므로 오로지 가로-세로 개념임.

1. justify-items: 모든 그리드 아이템들을 각자의 셀 위치에서 가로(row축)방향으로 정렬합니다.
  - 그리드컨테이너가 아닌 각각의 아이템이 포함된 `셀`관점에서 가로정렬 (컨테이너에 지정하는 속성이며, 모든 셀에 동시 적용)

2. align-items:
모든 그리드 아이템들을 (각자의 셀 위치에서) 세로(column축)방향으로 정렬하는 속성.
// 그리드컨테이너가 아닌 각각의 아이템이 포함된 `셀`관점에서 세로정렬 (컨테이너에 지정하는 속성이며, 모든 셀에 동시 적용)

3. align-contents: 그리드 아이템을 그룹지어 세로정렬
  - justify-content: 그리드 아이템그룹을 그리드 컨테이너에서 어디에 위치할지 가로방향의 지정 속성
  - 묶어서 위치를 조정한다고 생각하면 쉽다 !

4. 특정 아이템에 세로정렬 align-self (특정 셀 속의 아이템에만 적용)
5. 특정 아이템에 가로정렬 justify-self (특정 셀 속의 아이템에만 적용)
6. z-index z축 정렬 : 숫자가 클 수록 위로 올라온다.

<br>

### 정렬속성 - 정리 
> 즉, 그리드에서 정렬속성은 (가로-세로)관점에서 정렬하고 flex와 다르게 별도의 축이 없다.(오로지 가로-세로 방향만존재)  
1. justify(align)-conents는 그리드 컨테이너에서 모든 셀을 묶어서 이동시키는 속성이며,  
2. justify(align)-items는 그리드 컨테이너가 아닌 각각의 셀 관점에서 아이템들을 이동시키는 속성이고,
3. justify(align)-self는 특정 그리드 셀속의 아이템 하나만의 위치를 이동시키는 속성이다.

<br>

### 참고하면 좋은 영상
- [GRID](https://www.youtube.com/watch?v=9zBsdzdE4sM)
