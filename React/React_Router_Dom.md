


#리액트 라우터1: 셋팅이랑 기본 라우팅

__바닐라js에서 페이지 나누는 법__
1. html파일 만들어서 상세페이지내용 채움
2. 누가/detail로 접속하면 html파일 보내줌

<br> 

__리액트 SPA 방식__
`index.html`하나만 사용함
1. 컴포넌트 만들어서 상세페이지 내용 채움
2. 누가/detail접속하면 그 컴포넌트 보여줌
=> 좀 더 편하게 짜기 위해 react-router-dom 라이브러리 사용..

<br>


## React-router-dom사용법

1. `index.js`에서 <App>을 <BrowserRouter>로 래핑

2. 위 작업을 거치면 App의 하위 컴포넌트에서 Routes,Route 컴포넌트 적용가능

// `import {BrowserRouter} from 'react-router-dom';`
// `import { Routes ,Route, Link }  from 'react-router-dom';`

- __`Route`: `Routes컴포넌트 속에 존재하는` 페이지단위 보면 됨__

```jsx
<Routes>
 <Route path="/" element={ } /> //메인페이지 경로.. 
 <Route path="/detail" elememt={보여주려는 html요소 혹은 컴포넌트} />
 <Route path="/home" element={<Home/>} />
</Routes>
```
//유의: 라우트도 컴포넌트이기 때문에 배치 순서에 따라 위에서 아래로 배치.


### Link컴포넌트로 페이지 이동하기
`<Link to="/detail">상세페이지</Link>`
//a태그 역할의 버튼임 

<br>

### useNavigate Hook

__1.useNavigate__
- 페이지 이동을 도와주는 훅
- `<Link>는` `<a>태그` 기반이여서 태그속에 태그로 중첩되는 문제 발생
-  훅을 사용하면 onClick이벤트 기반 이벤트핸들러로 등록가능!

```jsx
let navigate = useNavigate(); //네비게이트 함수 생성 
         <Nav.Link onClick={()=>navigate('detail')}
>Pricing</Nav.Link>
```

- navigate(1) :앞으로 한 페이지 이동해주세요
- navigate(-1): 이전 페이지로 이동해주세요
- navigate('/~~'): 해당 주소로 이동해주세요
- navigate(-2): 두 페이지 전도 가능!

<br>

__2.404페이지 만들기__

`<Route path="*" element={<div>없는페이지요</div>} ></Route>`
//*는 와일드카드 문자: 위에 경로 외에 모든 것에 해당

<br>

__.Nested Routes(중첩라우트)__

- 특정 라우트경로 이동시 중첩된 또다른 라우트가 있는 경우
```jsx
<Routes>
 <Route path="/about" element={<About/>}>
  <Route path="member" element={<Member/>} />
  <Route path="location" element={<Location/>} />
 </Route>
</Routes>
```
- 누가 /about/member 들어가면 , Member컴포넌트 보여주세요 라는 뜻 
- 위와 아래는 같은 표현방식임 

```jsx
<Route path="about/member" element={<About/>}/>
<Route path="about/location" element={<Location/>}/>
```

<br> 

__중첩라우트와 outlet컴포넌트__

```jsx
> layouts - App.js

<Route path="/about" element={ <About/> } >  
  <Route path="member" element={ <div>멤버들</div> } />
  <Route path="location" element={ <div>회사위치</div> } />
</Route>
```

```jsx
> pages - About.js

function About(){
  return (
    <div>
      <h4>about페이지임</h4>
      <Outlet></Outlet>
    </div>
  )
}
```

- outlet컴포넌트는 중첩라우트에서 부모 엘리먼트에 지정해준다.
- 지정된 outlet컴포넌트는 중첩라우트에서 자식엘리먼트를 부모엘리먼트의 outlet컴포넌트 위치에 렌더링시킨다.
- 즉 /about/member 경로로 About컴포넌트를 보여주며, 그속에 outlet위치에 중첩라우트member의 엘리먼트를 보여줌

__요약__ 
1. 부모에 outlet세팅 
2. 중첩라우트로 이동시 부모 컴포넌트도 보여주고, 부모의 outlet에 중첩된 자식라우트의 엘리먼트까지 렌더링

<br>

# 리액트 라우터3. URL파라미터로 상세페이지 100개 만들기

1. import { useParams } from 'react-router-dom';
//Detail컴포넌트에서 사용할 id변수 선언(URL값으로부터 동적으로 생성되는 변수)
`let id = useParams(); `

2. `path="/detail/:id" `로 경로 설정시
입력해준 url을 입력할 때,입력한값( 0,1,2..)값에 따라 id변수를 생성해준다.
`<Route path="/detail/:id" element={ <Detail shoes={shoes}/ > }/>`
- 이제 Detail컴포넌트 내부에서 동적으로 받아온 id변수를 구조분해할당 `let { id } = useParams()`로 활용할 수 있음.

<br>
