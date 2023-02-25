# 리액트에서 자주 쓰는 if문 작성패턴



## 1.컴포넌트 안에서 쓰는 if/else

```jsx
function Component(){
	if( true ) {
 		return <p>참이면 보여줄 HTML</p>;
 	} else {
		return null;
	}
}
```
- JSX에는 자바스크립트 표현식만 가능(일반 js코드 불가)


## 2.JSX안에서 쓰는 삼항연산자
`조건문 ? 참일 때 실행할 표현식: 거짓일 때 실행할 표현식`
```jsx
function Component() {
  return (
    <div>
      {
        1 === 1
        ? <p>참이면 보여줄 HTML</p>
        : null
      }
    </div>
  )
} 
```
- JSX return()문 안에서는 삼항연산자를 쓸 수 있습니다.
// 삼항연산자 리턴값 = 표현식(값)이므로..
- 삼항연산자의 리턴값은 자바스크립트 표현식이어야 합니다.
- 삼항연산자는 중첩될 수 있습니다.
```jsx
function Component() {
  return (
    <div>
      {
        1 === 1
        ? <p>참이면 보여줄 HTML</p>
        : ( 2 === 2 
            ? <p>안녕</p> 
            : <p>반갑</p> 
          )
      }
    </div>
  )
} 
```
- 조건문 참이면 -> 참/거짓을 다시 한번 판별 후 리턴(이중조건문)

<br>

## 3.`&& 연산자로 if역할 대신하기`
`&&연산자`: 왼쪽 오른쪽 둘 다 true면 전체를 true로 바꿔주세요
```js 
true && false; //false
true && true; //true
```

근데 자바스크립트는 && 기호로 비교할 때 true와 false를 넣는게 아니라 자료형을 넣으면
```js
true && '안녕'; //안녕
false && '안녕'; //false 
true && false && '안녕'; //false 
```
`자바스크립트는 그냥 &&로 연결된 값들 중에 처음 등장하는 falsy 값을 찾아주고 그게 아니면 [마지막값]을 남겨준다"고 이해하면 됨 .`
- 우선적으로 나오는 falsy값(0,null,undefined,false ...) 리턴
- 그게아니라면 마지막 값 리턴 

<br>

### 응용)
```jsx
function Component() {
  return (
    <div>
      {
        1 === 1
        ? <p>참이면 보여줄 HTML</p>
        : null
      }
    </div>
  )
} 

//위 코드를 아래로 축약가능 

function Component() {
  return (
    <div>
      { 1 === 1 && <p>참이면 보여줄 HTML</p> }
    </div>
  )
//먼저나오는 falsy값이 없으므로 <p>태그 리턴 
}
```


## 4. switch/case조건문 
```jsx
function Component2(){
  var user = 'seller';
  if (user === 'seller'){
    return <h4>판매자 로그인</h4>
  } else if (user === 'customer'){
    return <h4>구매자 로그인</h4>
  } else {
    return <h4>그냥 로그인</h4>
  }
}
```

<br>

- 위와같이 if문을 연달아 쓰는 경우 switch/case로 대체가능
```jsx
function Component2(){
  var user = 'seller';
  switch (user){
    case 'seller' :
      return <h4>판매자 로그인</h4>
    case 'customer' :
      return <h4>구매자 로그인</h4>
    default : 
      return <h4>그냥 로그인</h4>
  }
}
```
1. `switch(검사변수) {} `부터 작성
2.  switch 본문{}안에 case 검사변수랑일치하는값 : 를 넣어줌.
3.  일치하면 실행할 코드를 case: 밑에넣어줌
4.  default: 는 그 외의 모든값일 때 코드 실행 
//단점: 조건식에서 하나의 변수만 검사할 수 있음

<br>

## 5. object/array 자료형 응용
- `element는 자바스크립트 표현식`

<br>

```jsx
function Component() {
  const [newVal,setNewVal] = useState('info');
  return (
    <div>
      {
        { 
           info : <p>상품정보</p>,
           shipping : <p>배송관련</p>,
           refund : <p>환불약관</p>
        }[newVal값]
      }

    </div>
  )
} 
```
- state를 key값으로 사용해서 객체를 computed property 방식으로참조하는 렌더링

<br>
//아래와 같이 렌더링할 객체를 변수로 미리 만들어도 됨.

```jsx
const 탭UI = { 
  info : <p>상품정보</p>,
  shipping : <p>배송관련</p>,
  refund : <p>환불약관</p>
}

function Component() {
  var 현재상태 = 'info';
  return (
    <div>
      {
        탭UI[현재상태]
      }
    </div>
  )
} 
```

	

