# 주요개념(메인컨셉)

## jsx

### jsx란?
- 마크업과 javascript로직을 혼용하는 확장문법
//js문법 + html
- jsx는 React.Element를 생성합니다.
>React는 별도의 파일에 마크업과 로직을 넣어 기술을 인위적으로 분리하는 대신, 둘 다 포함하는 “컴포넌트”라고 부르는 느슨하게 연결된 유닛으로 관심사를 분리합니다.

### jsx에 표현식 포함하기
- jsx의 `중괄호{ }` 안에는 `유효한 모든 js표현식(:값)`을 넣을 수 있다.
```jsx
const element = (
  <div>
    <h1>Hello!</h1>
    <img src={user.avatarUrl} />;
  </div>
);

```

### jsx도 표현식이다.
 1. **컴파일이 끝나면 jsx는 정규 자바스크립트 함수 호출이되고,
`자바스크립트 객체로 평가`됩니다.**
 2. **즉, jsx를 
`if문 for`문 안에서 사용하고,
`변수에 할당`하고, 
`인자`로 받아들이고, 
`함수 반환값`으로 사용할 수 있다.**


### jsx속성 정의
- 경고 jsx는 HTML보단 JS에 가까우므로, React DOM은
HTML 어트리뷰트 이름 대신 camelCase 프로퍼티 명명 규칙을 사용합니다.


### jsx로 자식 정의
태그가 비어있다면 XML처럼 \/\> 를 이용해 바로 닫아주어야 합니다.

```jsx
const element = <img src={user.avatarUrl} />;
```
jsx 태그는 자식을 포함할 수 있습니다.

```jsx
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);

```

### React.createElement()
- Babel은 jsx를 React.createElement()호출로 컴파일함
- 버그가 없는 코드 작성에 도움이 되는 검사를 수행
- 이는 리액트 엘리먼트 객체를 생성해 virtual DOM을 구성한다.

### jsx는 XSS(Cross Site Scripting)공격을 방지한다.
- React DOM은 jsx에 삽입된 모든 값을 렌더링하기 전에 이스케이프한다.(XSS공격을 방지)

>
`이스케이프`
특정 문자를 원래의 기능에서 벗어나게 변환하는 행위를 이스케이프(Escape:\\)라고 한다.
&은 &amp;로
<은 &lt;로
>은 &gt;로
"은 &quot;로
'은 &#39로
띄어쓰기는 &nbsp;로
//이를통해 HTML본연의 태그나 스크립트 기능이 제거되어
XSS공격을 방지할 수 있다.

### jsx는 객체(리액트 엘리먼트)_를 표현합니다.

Babel은 jsx를 React.createElement() 호출로 컴파일합니다.

다음 두 예시는 동일합니다.
```jsx
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```
```jsx
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```
React.createElement()는 버그가 없는 코드를 작성하는 데 도움이 되도록 몇 가지 검사를 수행하며, 기본적으로 다음과 같은  단순화된 객체를 생성합니다.
```jsx
// 주의: 다음 구조는 단순화되었습니다
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world!'
  }
};
```
위와 같은 객체를 `React.Element`라고 하고
React는 이 객체를 읽어서, DOM을 구성하고 최신 상태로 유지하는 데 사용합니다.(virtual DOM & diffing argo)

<br>


## 엘리먼트 렌더링
**`Element`는 React 앱의 `가장 작은 단위`입니다.**
> 일반적으로 React 엘리먼트는 1.일반 엘리먼트 2.사용자 정의 컴포넌트 를 의미합니다.

1. DOM 엘리먼트: DOM객체 (일반 plain 객체보다 무겁다)

2. React 엘리먼트: 일반 객체(plain)
- 컴파일과정에서 js객체로 변환(Babel)
- root.render()메서드에 의해 React엘리먼트가 렌더링됨.
- React엘리먼트는 일반 객체이므로 virtual dom을 구성해서 빠른 리렌더를 가능케한다.

`<div id='root'></div>` // 루트DOM 노드
>리액트 앱은 일반적으로 `하나의 루트DOM노드`가 있음
이 안에 들어가는 모든 엘리먼트를 React DOM에서 관리한다.

### React 엘리먼트 렌더링 과정 
 1. DOM엘리먼트 -> ReactDOM.createRoot()에 전달
 2. React 엘리먼트를 -> root.render()에 전달.

```jsx
const root = ReactDOM.createRoot(   // 2. 노드 취득 후 root변수에 할당
  document.getElementById('root')   // 1.루트DOM노드(div id='root')취득(모든 하위 React.Elem를포함한다)
);
const element = <h1>Hello, world</h1>;
root.render(element);      //root노드의 render()메서드에 전달후 호출
```

### 렌더링 된 엘리먼트 업데이트하기

- React 엘리먼트는 불변객체
- 엘리먼트를 생성한 이후에는 해당 엘리먼트의 자식이나 속성을 변경할 수 없습니다.
> 따라서,UI를 업데이트하는 유일한 방법은 새로운 엘리먼트를 생성하고 이를 root.render()로 전달하는 것입니다.
`즉, React 엘리먼트(virtual dom을 구성)를 새로이 생성하여 전후 상태를 비교하는 알고리즘으로 UI를 업데이트(리렌더)함을 의미.`



<br>

## 4. Component와 props

### 함수 컴포넌트와 클래스 컴포넌트
- 컴포넌트는 functional(함수선언,ES6 arrow Fn) & Class가 존재한다.
- 컴포넌트명은 대문자로 시작(마크업으로 인식하지않도록)
```jsx
function Welcome(props) {   ///...컴포넌트명은 반드시 시작을 대문자로!
  return <h1>Hello, {props.name}</h1>;
}
```


### 컴포넌트 렌더링
**전까지는 DOM 태그만을 사용해 React 엘리먼트를 나타냈습니다.**
```jsx
const element = <div />; ///... 일반 엘리먼트도 React 엘리먼트입니다.
```
**React 엘리먼트는 사용자 정의 컴포넌트로도 나타낼 수 있습니다.**
```jsx
const element = <Welcome name="Sara" />; ///...사용자 정의 컴포넌트 
```

>React가 사용자 정의 컴포넌트로 작성한 엘리먼트를 발견하면 jsx 어트리뷰트와 자식을 해당 컴포넌트에 단일 객체로 전달합니다. 이 객체를 “props”라고 합니다.
다음은 페이지에 “Hello, Sara”를 렌더링하는 예시입니다.


```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
const root = ReactDOM.createRoot(document.getElementById('root')); //1.루트돔노드(모든React.Elem를 포함한 상위노드)
const element = <Welcome name="Sara" />; 
root.render(element); //2. 루트돔 node의 render메서드에 react elem를 넣어 호출 =>UI렌더링 이뤄짐.
```
위 예시에서는 다음과 같은 일들이 일어납니다.
>
 1. <Welcome name="Sara" /\> (jsx로 작성된 리액트엘리먼트)로 `root.render()를 호출합니다.`
 2. React는 {name: 'Sara'}를 props로 하여 Welcome 컴포넌트를 호출합니다.
 3. Welcome 컴포넌트는 결과적으로 `<h1>Hello, Sara</h1>` 엘리먼트를 반환합니다.
 4. React DOM은 `<h1>Hello, Sara</h1>` 엘리먼트와 일치하도록 DOM을 효율적으로 업데이트합니다.


### props는 (객체)구조분해할당으로 전달할 수 있습니다.
- ` const { 변수명 } //변수패턴  = 분해할 객체`  
const { name } = props     
name = props.name 
//props의 속성명을 { } 중괄호에 담아 파라미터로 전달 ==> ES6구조분해할당.



### props는 읽기 전용입니다. (=수정불가)

```javascript
function sum(a, b) {
  return a + b;
}
```
- 이런 함수들은 `순수함수`라고 호칭하고,
- 입력값(a,b)를 바꾸려하지않고, 항상 동일한 입력값에 동일 결과를 반환하는 특징이 있습니다.

**반면, 다음 함수는 자신의 입력값을 변경하기 때문에 순수 함수가 아닙니다.**

```javascript
function withdraw(account, amount) {
  account.total -= amount;
}
```
**모든 React 컴포넌트는 자신의 props를 다룰 때 반드시 순수 함수처럼 동작해야 합니다.**
- React 컴포넌트는 전달받은 자신의 props 변경시킬 수 없음을 의미합니다.(props: readonly)

>물론 애플리케이션 UI는 동적이며 시간에 따라 변합니다. 이러한 문제를 해결하기위해 `State`가 존재합니다. React 컴포넌트는 state를 통해 위 규칙을 위반하지 않고 사용자 액션, 네트워크 응답 및 다른 요소에 대한 응답으로 시간에 따라 자신의 출력값을 변경할 수 있습니다.



### 컴포넌트 합성
**컴포넌트는 자신의 출력에 다른 컴포넌트를 참조할 수 있습니다. **
- = 컴포넌트 속에는 다른 컴포넌트가 들어갈 수 있다.

예를 들어 Welcome을 여러 번 렌더링하는 App 컴포넌트를 만들 수 있습니다.
```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}
```

### 컴포넌트 추출
너무 heavy한 컴포넌트는 여러 개의 작은 컴포넌트로 나누는 것이 가능합니다.



<br>

## 5. State와 생명주기 
**컴포넌트는 마운트,업데이트,언마운트의 생명주기를 가집니다.**

- State는 마운트시 초기값으로 초기화되고,
- set함수를통해 변경시 컴포넌트의 리렌더링을 유발합니다.
 
<br>

## 6. 이벤트 처리하기
- React의 이벤트는 camelCase를 사용합니다.
- jsx를 사용하여 문자열이 아닌 함수로 이벤트 핸들러(함수)를 전달합니다.
//이벤트가 발생하면 이벤트핸들러에 `e 합성이벤트 객체`가 인자로 전달됩니다.

```jsx
[HTML]
<button onclick="activateLasers()">
  Activate Lasers
</button>

[jsx]
<button onClick={activateLasers}>  //camelCase , {함수, not 문자열 }
  Activate Lasers
</button>
```

### React는 false를 반환해도 기본 동작을 방지할 수 없습니다.
- `e.preventDefault()`로 `onSubmit` 이벤트의 기본동작을 명시적으로 방지해줘야합니다.
```jsx
function Form() {
  function handleSubmit(e) {
    e.preventDefault();
    console.log('You clicked submit.');
  }

  return (
    <form onSubmit={handleSubmit}>
      <button type="submit">Submit</button>
    </form>
  );
}
```


<br>


## 7. 조건부렌더링 

### 엘리먼트
- 일반 엘리먼트 
- 사용자정의 컴포넌트

### 엘리먼트변수
- 엘리먼트를 저장하기 위해 변수를 사용할 수 있습니다.
`const 변수  = React엘리먼트`

```jsx
class LoginControl extends React.Component {
  constructor(props) {
    super(props);
    this.handleLoginClick = this.handleLoginClick.bind(this);
    this.handleLogoutClick = this.handleLogoutClick.bind(this);
    this.state = {isLoggedIn: false};
  }

  handleLoginClick() {
    this.setState({isLoggedIn: true});
  }

  handleLogoutClick() {
    this.setState({isLoggedIn: false});
  }

  render() {
    const isLoggedIn = this.state.isLoggedIn;
    let button;
    if (isLoggedIn) {
      button = <LogoutButton onClick={this.handleLogoutClick} />;  //엘리먼트 변수
    } else {
      button = <LoginButton onClick={this.handleLoginClick} />;
    }

    return (
      <div>
        <Greeting isLoggedIn={isLoggedIn} />
        {button}
      </div>
    );
  }
}

const root = ReactDOM.createRoot(document.getElementById('root')); 
root.render(<LoginControl />);
```

### 논리연산자(&&)로 if를 인라인으로 표현하기( 조건부 렌더링 )
jsx 안에는 if문을 사용할 수 없지만 대신 중괄호에 js표현식을 포함 할 수 있습니다.
- `true && expression` 은 항상 `expression`으로 평가된다. (둘 다 참이여야 참)
- `false && expression`은 항상 `false`
// `true &&` 뒤에오는 표현식으로 평가되고, false는 false
// React에서 false는 아무것도 렌더시키지 않는다.(falsy표현식은 그대로 falsy한 값이 반환되므로 주의하자)

```jsx
function Mailbox(props) {
  const unreadMessages = props.unreadMessages;
  return (
    <div>
      <h1>Hello!</h1>
      {unreadMessages.length > 0 && ///...앞에값이 true일 때, 뒤에 출력
        <h2>
          You have {unreadMessages.length} unread messages.
        </h2>
      }
    </div>
  );
}
const messages = ['React', 'Re: React', 'Re:Re: React'];

const root = ReactDOM.createRoot(document.getElementById('root')); 
root.render(<Mailbox unreadMessages={messages} />);
```

- falsy 표현식을 반환하면 여전히 && 뒤에 있는 표현식은 건너뛰지만 falsy 표현식이 반환된다는 것에 주의해주세요, 아래 예시에서, <div>0</div>이 render 메서드에서 반환됩니다.
```jsx
render() {
  const count = 0;
  return (
    <div>
      {count && <h1>Messages: {count}</h1>}
    </div>
  );
}
```

### `삼항연산자 (조건식,값) ? A : B `로 If-Else 구문 인라인으로 표현하기
- 리액트 엘리먼트도 `표현식`이기 때문에 `삼항연산자의 반환값`이 될 수 있음.
```jsx
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      {isLoggedIn 
        ? <LogoutButton onClick={this.handleLogoutClick} />
        : <LoginButton onClick={this.handleLoginClick} />
      }
    </div>
  );
}
```

### 컴포넌트가 렌더링하는 것을 막기 (with `null`)
- 가끔 다른 컴포넌트에 의해 렌더링될 때 컴포넌트 자체를 숨기고 싶을 때가 있을 수 있습니다.  
이때는` 렌더링 결과를 출력하는 대신 null을 반환`하면 해결할 수 있습니다.





## 8.리스트와 키(고유하고 안정적인)
- **map이 적용되는 콜백의 리턴값에 직접 key를 부여해줘야한다.**

### Key
```jsx
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li>{number}</li>
);
```
- React elem은 `표현식` 이므로 자바스크립트 로직의 값으로 사용된다.
- React에서 List(<li>)에는 `고유하고(unique) &  안정적인(stable,변하지않는)` key속성값을 필요로한다.
- Key를 선택하는 가장 좋은 방법:  리스트의 항목들 사이에서 각각을 고유하게 식별할 수 있는 `문자열`을 사용하는 것입니다.

### Key속성을 부여하는 위치 (:map 콜백의 반환값 위치에)
- 실제 map콜백의 반환값으로 생성되는 List에 부착해준다.
- ListItem컴포넌트의 return값인 <li>태그 에는 key를 부여하지 않고,
- 실제 map 콜백의 반환값인 <ListItem>컴포넌트에 직접 props로 key를 부여한다.

```jsx
function ListItem(props) {
  // 맞습니다! 여기에는 key를 지정할 필요가 없습니다.
  return <li>{props.value}</li>;
}

function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    // 맞습니다! 배열 안에 key를 지정해야 합니다.
    <ListItem key={number.toString()} value={number} />
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}
```



### Key는 형제 사이에서만 고유한 값이어야 한다.
- 하나의 배열에서 생성되는 형제(list아이템)끼리 사이에만 고유해야하고, 전체 범위에서 고유할 필요는 없습니다.
- 서로 다른 배열간에 생성되는 리스트는 동일한 Key를 사용할 수 있다.

```jsx
function Blog(props) {
  const sidebar = (
    <ul>
      {props.posts.map((post) =>
        <li key={post.id}>
          {post.title}
        </li>
      )}
    </ul>
  );
  const content = props.posts.map((post) =>
    <div key={post.id}>
      <h3>{post.title}</h3>
      <p>{post.content}</p>
    </div>
  );
  return (
    <div>
      {sidebar}
      <hr />
      {content}
    </div>
  );
}

const posts = [
  {id: 1, title: 'Hello World', content: 'Welcome to learning React!'},
  {id: 2, title: 'Installation', content: 'You can install React from npm.'}
];
```

### Key는 힌트를 제공하지만 실제 전달되는 값은 아니다.
- 컴포넌트에 key와 동일한 값이 필요하면 다른 이름의 prop으로 명시적으로 전달할 필요가 있음.

```jsx
const content = posts.map((post) =>
  <Post
    key={post.id}     //힌트로만 사용되는 값
    id={post.id}       //실제 명시적으로 전달되는 값
    title={post.title} />
);
```
- 위 예시에서 Post 컴포넌트는 props.id를 읽을 수 있지만 props.key는 읽을 수 없습니다.



### jsx에선 {중괄호}에 모든 `표현식(:값)`을 포함시킬 수 있다.
- map() 함수의 결과를 inline으로 처리해보자
- 단, map()이 너무 중첩된다면 컴포넌트로 빼는 것이 낫다.
```jsx
function NumberList(props) {
  const numbers = props.numbers;
  return (
    <ul>
      {numbers.map((number) =>
        <ListItem key={number.toString()}
                  value={number} />       //map()은 배열을 반환하므로 표현식이다.
      )}
    </ul>
  );
}
```

<br>


## 9. 폼(FORM)
HTML 폼 엘리먼트는 폼 엘리먼트 자체가 `내부 상태`를 가지기 때문 React의 다른 DOM 엘리먼트와 다르게 동작합니다.
예를 들어, 순수한 HTML에서 아래의 폼은 `name`을 입력받습니다.
```jsx
<form>
  <label>
    Name:
    <input type="text" name="name" />
  </label>
  <input type="submit" value="Submit" />
</form>
```
위 방식보다는 Javascript 함수로 폼의 제출을 처리하고, 사용자가 폼에 입력한 데이터에 접근하도록
하는 것이 편리합니다. => 이를 `제어 컴포넌트` 라고합니다.

### 제어컴포넌트( state속성에 의해 제어되는 컴포넌트 )
> HTML에서 `<input>, <textarea>, <select>`와 같은 폼 엘리먼트는 일반적으로 사용자의 입력을 기반으로 자신의 state를 관리하고 업데이트합니다. React에서는 변경할 수 있는 state가 일반적으로 컴포넌트의 state 속성에 유지되며 setState()에 의해 업데이트됩니다.



```jsx
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: ''};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('A name was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

- 컴포넌트 메서드(handle(),render() 의 this는 메서드호출객체: NameForm에 바인딩.
- 이벤트핸들러(this.handleChange)내부의 this는 컴포넌트 메서드를 간접 참조하는 형태로 이벤트핸들러에 부착되므로 NameForm(this)에 대한 바인딩을 잃어버린다(window객체를 참조하게된다). 따라서 this.handleChange = this.handleChange.bind(this) 와같이 명시적으로
this.handleChange 이벤트핸들러속 this가 NameForm을 참조하도록 바인딩 시키는 과정이 필요하다.
- 함수형 컴포넌트에서는 위와 같은 문제를 걱정할 필요가 없음.

### textarea 태그 
**HTML에서 `<textarea>` 엘리먼트는 텍스트를 자식으로 정의합니다.**
```jsx
<textarea>
  Hello there, this is some text in a text area
</textarea>
```
**React에서 `<textarea>`는 value 어트리뷰트를 대신 사용합니다. 이렇게하면 `<textarea>`를 사용하는 폼은 한 줄 입력을 사용하는 폼과 비슷하게 작성할 수 있습니다.**
```jsx
class EssayForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: 'Please write an essay about your favorite DOM element.'
    };

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('An essay was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Essay:
          <textarea value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}

```
- React에서 textarea는 children요소가 아닌 `<textarea value={valueState}/>` 형태로 value속성으로 관리한다.

### select 태그
**기존 HTML방식**
```jsx
<select>
  <option value="grapefruit">Grapefruit</option>
  <option value="lime">Lime</option>
  <option selected value="coconut">Coconut</option>
  <option value="mango">Mango</option>
</select>
```
- option태그에 selected value 속성값 부여

**React 제어컴포넌트 방식**
```jsx
 <form onSubmit={this.handleSubmit}>
        <label>
          Pick your favorite flavor:
          <select value={this.state.value} onChange={this.handleChange}>
            <option value="grapefruit">Grapefruit</option>
            <option value="lime">Lime</option>
            <option value="coconut">Coconut</option>
            <option value="mango">Mango</option>
          </select>
        </label>
        <input type="submit" value="Submit" />
      </form>
```
- option태그가 아닌 select태그에 value속성값으로 선택된 옵션값을 관리

### 제어되는 Input Null값 
> 제어 컴포넌트에 `value prop을 지정하면 의도하지 않는 한 사용자가 변경할 수 없습니다.`
value를 설정했는데 여전히 수정할 수 있다면? 실수로 undefined, null로 설정되었을 경우 뿐입니다.
```jsx
ReactDOM.createRoot(mountNode).render(<input value="hi" />);
   ///... input value prop이 'hi'로 고정되어 변경이 불가능합니다.
setTimeout(function() {
  ReactDOM.createRoot(mountNode).render(<input value={null} />);
}, 1000);  ///... 1초 뒤 input 입력이 활성화됩니다.
```




출처: 

[리액트공식문서](https://ko.reactjs.org)
  
[3.엘리먼트렌더링](https://ko.reactjs.org/docs/rendering-elements.html)

[4.Component와Props](https://ko.reactjs.org/docs/components-and-props.html#rendering-a-component)

[6.이벤트 처리하기](https://ko.reactjs.org/docs/handling-events.html)

[7. 조건부렌더링](https://ko.reactjs.org/docs/conditional-rendering.html)

[8. 리스트와 Key](https://ko.reactjs.org/docs/lists-and-keys.html)

[9. Form](https://ko.reactjs.org/docs/forms.html)

\+ 나의 작은 견해 
