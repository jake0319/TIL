## 1.Variables and nesting 

### Variables
`_variables.scss`파일 생성, 파일명에 `_(언더바)`를 붙이면 해당파일은 컴파일(scss->css)하지않는다. 

```scss
$bg: #e7473c; 

```
- `$(달러싸인)+변수명 : value;` 로 선언
- `@import "_variables"`로 전역 styles.scss에 import한다.
- import시킨 _variables 파일 속 변수는 $변수명 으로 참조가능

<br>

### Nesting
- css코드구문{}속에 `선택자 {코드구문}`을 중첩시켜 정확한 element타겟팅이 가능
- 동일 선택자에 대해 중첩문 속에서`& {} `으로 네스팅이 가능 // `&:hover {}`

```scss

.box{ 
  margin-top: 20px;
  &:hover {
	background-clor: green;
	}
  h2 { 
	color: blue;
	}
  button { 
	color: red;
	}
}
```  
```scss
a { text-decoration: none;}
a { color: red;}

// 위 코드를 nesting적용하면? 
a { text-decoration: none;
	& { color: red;}
}
```



## 2.Mixins
- __`상황에 따라 다르게 css를 작성하고 싶을 때 사용하는 functionality문법`__

<br>
 
### css의 함수형 작성
`> src/scss/_mixins.scss파일 생성`
```scss
@mixins sexyTitle {
	color: blue;
	font-size: 30px;
	margin-bottom: 12px;
}
```
- style.scss파일에=>적용
```scss
@import "mixins"; 로 import

h1 { 
	@include sexyTitle();
}
```
// h1선택자에 {css구문}적용시 안에서 @include 믹스인함수(); 적어주면 위 믹스인css코드가 적용됨.
// css의 함수형작성을 도와줌

<br>

### mixin에 파라미터넣기
```scss
@mixin link($color){
	text-decoration: none;
	display: block;
	color: $color;
}
```
- $color라는 변수로 함수에 '구멍'을 뚫어줌

```scss
// styles.scss파일에서..


// nesting으로 작성하기
a { 
  &:nth-child(odd) {
	@include link(blue);
 }
  &:nth-child(even) {
	@include link(red);
 }
}

```

<br>

### mixin에 `@if{},@else{}문`적용하기

```scss
@mixin link($word) {
	text-decoration: none;
	display: block;
	@if $word == "odd" {
		color: blue;
	} @else {
		color: red;
    }
}

```

<br> 

__Mixin문법 요약__
- mixin은 일종의 {css코드}묶음을 함수처럼 적용시켜주는 functionality
- 사용방법:  `적용선택자{ @include mixin(params);}`
- mixin내부적으로 `if-else` 조건문 사용가능, 조건식에 따라 mixin내부에서 실행시킬 css부분적으로 적용가능


## Extends
- 공통적인 css코드를 적용시킬때(재사용) 사용한다. 

```scss
%button {
  font-family: inherit; //부모요소 폰트 물려받기
  border-radius: 7px;
  font-size: 12px;
  text-transformP uppercase; //text대문자로 변경 
  padding: 5px 10px;
  color: white;
  background-color: peru;
  font-weight: 500;
}
```
- 이후 `%변수명`으로 정의한 extends를 `_buttons.scss`파일에서 @import "_button" 후 사용 
- 사용하려는 선택자에서 {` @extends %변수명; `} 으로 재사용할 코드묶음을 적용시킨다.

<br>


## mixin에 @content지정하기

```scss
@mixin responsive($device) {
  @if $device == "iphone" {
	@media screen and (min-width: $minIphone) and (max-width: $maxIphone) {
	  @content;
	}

@mixin responsive($device) {
  @if $device == "ipad" {
	@media screen and (min-width: $minIpad) and (max-width: $maxIpad) {
	  @content;
	}
}
```

<br>

```scss
@import "_mixins";

h1 { 
  color: red;
  @include responive("iphone") {
	color: yellow;  //...@content로 결정되는 부분
  }
  @include responsive("iphone-l") {
	color: green;
  }
}
``` 
- mixin에 설정된 @content부분은 선택자 {@include 믹스인함수(인자){ @content해당코드} }로 호출시 적어주는 값에 의해 동적으로 결정된다.
	   

