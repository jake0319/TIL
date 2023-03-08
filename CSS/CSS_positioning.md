# CSS position
> CSS의 position 속성은 엘리먼트가 브라우저 화면에 어떻게 배치되는가를 결정합니다.

<br>

## position속성
- 엘리먼트가 브라우저 화면에 어떻게 배치되는지를 결정하는 속성
- `자식 요소에게 inherit되지 않음`

<br>

1. __static(기본값)__
- top,bottom,left,right,z-index 속성의 영향을 받지 않음
- 기본값인 static을 변경해야 이동한다는 의미.

<br>

2. __relative__
- `원래 위치를 기준`으로 top~right 설정값 만큼 이동
- 원래의 공간배치를 유지하는 속성(다른 요소의 배치가 빈공간을 채우지 못한다.)
-  요소의 위치는 이동하지만 `요소가 차지하던 기존 공백의 공간 및 다른 요소들과의 배치상태는 유지된다`
- relative인 부모요소 속에 자식요소가 있다면 부모요소에 포함되어 이동한다. 
<img width="1057" alt="image" src="https://user-images.githubusercontent.com/78556338/223655581-41f44290-d6c0-4c4f-a4b3-10fe77e4de6a.png">


<br>

3. __absolute__
- `static(기본값)이 아닌 첫 부모요소`를
기준으로 배치된다. (부모가 relative,absolute 등 ..일때)
- 타겟요소의 부모를 relative로 변경시켜주면
해당 부모 기준으로 위치를 조정할 수 있음.
- 원래의 공간배치에서 벗어나는 속성(다른 요소의 배치가 빈 공백을 채운다)

<img width="1029" alt="image" src="https://user-images.githubusercontent.com/78556338/223655837-a0f9ab95-2533-4329-8d84-7345cf44d115.png">
<img width="1026" alt="image" src="https://user-images.githubusercontent.com/78556338/223655943-ef775ebb-1c49-4775-8dd9-2f259ed8cfe9.png">



<br>

4. __sticky__
- 요소가 스크롤로 이동할 수 있는 공간을 top~right 속성으로 제한할 수 있음.

<br>

5. __fixed__
> 어떤 엘리먼트의 position 속성을 fixed로 지정해줄 경우, 해당 엘리먼트는 부모 엘리먼트로 부터 완전히 독립되어 브라우저 화면(viewport) 상에서 어디든지 원하는 위치에 자유롭게 배치시킬 수 있게 됩니다.
- 부모로부터 완전히 독립하여 뷰포트상 어디든지 배치가능
- 스크롤시에도 뷰포트상 위치가 변하지 않음.

<br>

__Fixed: 메뉴바를 상단에 고정시키기__
1. 화면의 어디에 고정시킬지를 offset 속성을 이용해서 설정해줍니다. 
2. 화면 상단에 빈틈없이 붙일 것이기 때문에 top:0, left:0, right:0 ; 부여 
  - left & right 대신에 width:100% 도 가능
```css
header {
  position: fixed;
  top: 0;
  /* width: 100% */
  left: 0;
  right: 0;

  /* 생략 */
}
```

<br>

__Fixed: 흔히 발생하는 문제..__
> 메인 영역의 윗 부분이 헤더 영역의 뒤로 들어가서 일부 컨텐츠가 보이지 않습니다.
이 것은 <header> 엘리먼트가 부모인 <body> 엘리먼트로 부터 완전히 독립되면서, <body> 엘리먼트에서 점유하고 있던 <header> 엘리먼트 공간이 사라져버렸기 때문입니다.

- [SOLUTION] 위 문제는 상단 고정 메뉴바 사용할 때 흔히 겪은 문제이며, <body> 엘리먼트의 상단에 메뉴바 높이 만큼 패딩(padding)을 주면 쉽게 해결할 수 있습니다.

```css
body {
  padding-top: 75px;
  /* 생략 */
}
```

<br>

### z-index
- `static이 아닌 요소들간 위 아래 배치 순서`를 지정합니다.

- `auto = 0`과 같으며, 같은 값의 요소들 중에는 나중에 배치된 것이 위로 올라오게 됨.

- 숫자가 클 수록 위로 올라온다`
