# React Hook 

## Hook 개요

### Hook에는 사용규칙이 있습니다
//`Hook은 단순히 Javascript 함수입니다.` 하지만 아래 두 규칙을 준수해야합니다.
// React에선 ESLint 플러그인이 이 규칙을 강제해줍니다.

- `최상위(at the top level)`에서만 Hook을 호출해야 합니다. 반복문(for), 조건문(if), 중첩된 함수 내에서 Hook을 포함할 수 없습니다!
// Hook속에 반복문,조건문,함수를 포함하는 것은 문제없음

- Hook을 사용할 수 있는 위치는 딱 두 곳입니다.
 1.	React 함수컴포넌트 내부
 2. 커스텀 훅 내부


### (함수)컴포넌트의 구조
- ES6 화살표함수를 통해 함수표현식 컴포넌트
- 일반적인 function키워드를 사용한 함수선언식 컴포넌트
- js로직을 작성할 수 있는 부분과 return()문 속 jsx작성부로 나뉜다. 
- State나 Effect Hook이 하나의 컴포넌트 속에서 여러 번 사용될 수 있다.

```jsx
const Example = (props) => {
  // 여기서 Hook을 사용할 수 있습니다!
  return <div />;
}
```
또는 이렇게 생겼습니다.

```jsx
function Example(props) {
  // 여기서 Hook을 사용할 수 있습니다!
  return <div />;
}
```


### Custom Hook이 뭔데?
- 이름이 use로 시작하는 자바스크립트 함수입니다.
- 사용자 Hook은 다른 Hook을 호출할 수 있습니다. 
- 두 개의 JS함수(또는 컴포넌트)에서 같은 로직을 공유하고자 할 때 해당 로직을 또 하나의 함수로 분리하는 것과 같은 맥락.
`즉, 커스텀훅은 그냥 use붙인 함수고 hook을 포함시켜 쓸 수 있다.`

예시)
```jsx
import React, { useState, useEffect } from 'react';

function useFriendStatus(friendID) {
  const [isOnline, setIsOnline] = useState(null);

  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }

  useEffect(() => {
    ChatAPI.subscribeToFriendStatus(friendID, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(friendID, handleStatusChange);
    };
  });

  return isOnline;
}
```

<br>

## 3. useState 훅 사용하기

### useState
- 함수 컴포넌트에서 state를 사용할 수 있게 해주는 훅
- 여러번 사용할 수 있다.
- 초기값으로 객체,숫자,문자 등 아무거나 가능

```jsx
import React, { useState } from 'react';

function Example() {
  const [count, setCount] = useState(0);
...

```

`const [state변수명,state변경함수] = useState(초기값)`
의 형태로 작성되고 배열 구조분해할당으로 값이 할당됨.


### state 가져오기
- 선언된 state변수는 컴포넌트 내부에서 자유롭게 사용됩니다.
`  <p>You clicked {count} times</p>`


### state 갱신하기
- state는 상태변경 함수를 통해서만 갱신됩니다.
- 이후 리렌더링을 발생시킵니다.(state & props의 갱신 특성)
```jsx
  <button onClick={() => setCount(count + 1)}>
    Click me
  </button>

```
<br>

## 4.Effect Hook 완벽정리

### 기본형태
` useEffect( effect함수[,deps])`
- 함수 컴포넌트에서 (side)effect를 수행할 수 있습니다.
- 콜백함수는 return값으로 clean-up함수를 포함할 수 있다.
- 클린업함수는 기명 & 무기명 & 화살표함수여도 상관없다.
- 훅 실행 시점을 조절하기 위한 `의존성배열`을 2nd 인자로 넣어줄 수 있다.


### 컴포넌트 라이프사이클 
- 페이지에 등장(mount)
- 업데이트(update)
- 제거되거나 사라짐(unmount)

> useEffect 훅을 사용하면
컴포넌트 마운트 전, 언마운트 전, 업데이트 후 
각 상황에 맞게 특정 함수를 실행시켜줄 수 있다.

과거 클래스 컴포넌트에서는 아래와 같이 사용했습니다.

```jsx
class Detail2 extends React.Component {
  componentDidMount(){
    //Detail2 컴포넌트가 로드되고나서 실행할 코드
  }
  componentDidUpdate(){
    //Detail2 컴포넌트가 업데이트 되고나서 실행할 코드
  }
  componentWillUnmount(){
    //Detail2 컴포넌트가 삭제되기전에 실행할 코드
  }
}
```
함수형 컴포넌트에서는 다음과 같이 Hook을 사용합니다.

```jsx
import {useState, useEffect} from 'react';

function Detail(){

  useEffect(()=>{
    //여기적은 코드는 컴포넌트 로드 & 업데이트 마다 실행됨
    console.log('안녕')
  });
  
  return (생략)
}
```


### 주요개념
- effect함수는 컴포넌트 mount & update(리렌더)시마다 호출된다. 

> 어떤 상황에서 update?
 1. state(상태변경 함수를 통한 변경) 
 2. 새로운 props가 들어올 때 (read-only,부모컴포넌트에서만 변경가능)
 3. 부모컴포넌트 리렌더링시 자식 컴포넌트 리렌더링된다.

### 의존성배열[deps]**  (a.k.a dependency arrays)**

 1.  **아무것도 넣지 않을 때( 2nd 인자를 주지 않음 )**
 - 모든 업데이트 상황에서 useEffect호출 
 2. **[] 공배열**을 줄 때
 - 첫 마운트시에만 useEffect호출
 3.  **[값]**을 줄 때 
 - 해당 값이 변경될 때마다 호출 


### React가 effect를 정리(clean-up)하는 시점은 정확히 언제일까요? 
`클린업함수: effect의 return값으로 반환되는 effect정리 목적의 함수`

>React는 컴포넌트가 마운트 해제되는 때에 정리(clean-up)를 실행합니다. 
하지만 위의 예시에서 보았듯이 effect는 한번이 아니라 렌더링이 실행되는 때마다 실행됩니다.
기존 effect가 실행핰 작업들을 정리해주기 위해
다음 effect 실행 전 기존 작업을 취소하거나 정리할 수 있도록
Clean-up 함수가 실행됩니다. 
React가 다음 차례의 effect를 실행하기 전에 이전의 렌더링에서 파생된 effect 또한 정리하는 이유가 바로 이 때문입니다.
 
- 컴포넌트 언마운트 될 때(사라질 때)
- (컴포넌트 업데이트를 통해) 다음 useEffect가 실행되기 전

### useEffect는 렌더링 이후 매번 수행되는 걸까요?

>네, 기본적으로 첫번째 렌더링과 이후의 모든 업데이트에서 수행됩니다.
마운팅과 업데이트라는 방식으로 생각하는 `대신 effect를 렌더링 이후에 발생하는 것으로 생각하는 것이 더 쉬울 것입니다.`
React는 effect가 수행되는 시점에 이미 DOM이 업데이트되었음을 보장합니다.




### 왜 useEffect를 써야하는가?

- 우선, useEffect 안에 적은 코드는 일반 함수와 다르게 `html 렌더링 이후`에 동작한다는 사실을 알고 있어야합니다.
- 컴포넌트의 생명주기에 따라 적절한 시점에 특정 기능을 동작하게 할 수 있습니다.

```jsx
function Detail(){

  (반복문 10억번 돌리는 코드)
  return (생략)
}
```
//위 코드는 반복문을 돌리고나서 React.Element를 렌더링,
위에서 아래로 동작하는 일반 함수의 동작방식과 같다.
Cf.)컴포넌트가 리턴하는 엘리먼트요소를 React.Element 라고한다.

```jsx
function Detail(){

  useEffect(()=>{
    (반복문 10억번 돌리는 코드)
  });
  
  return (생략)
}
```
//위 코드는 렌더링 이후에 effect함수를 호출시킴.


 
### 관심사의 구분: Multiple Effect
- State Hook을 여러 번 사용할 수 있는 것처럼 effect 또한 여러 번 사용할 수 있습니다. Effect를 이용하여 서로 관련이 없는 로직들을 갈라놓을 수 있습니다. 
-  React는 컴포넌트에 사용된 모든 `effect를 지정된 순서에` 맞춰 적용합니다.

### effect가 업데이트 시마다 실행되는 이유

>React 애플리케이션의 흔한 버그 중의 하나가 componentDidUpdate를 제대로 다루지 않는 것입니다.
이번에는 Hook을 사용하는 컴포넌트를 생각해봅시다.

```jsx
function FriendStatus(props) {
  // ...
  useEffect(() => {
    // ...
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });
```

위 컴포넌트의 동작을 가시화하면 아래와 같습니다.

```
// { friend: { id: 100 } } state을 사용하여 마운트(1st_Render)합니다.  (`초기값`)
ChatAPI.subscribeToFriendStatus(100, handleStatusChange);     // 첫번째 effect가 작동합니다.

// { friend: { id: 200 } } state로 업데이트합니다.
ChatAPI.unsubscribeFromFriendStatus(100, handleStatusChange); // 이전의 effect를 정리(clean-up)합니다.
ChatAPI.subscribeToFriendStatus(200, handleStatusChange);     // 다음 effect가 작동합니다.

// { friend: { id: 300 } } state로 업데이트합니다.
ChatAPI.unsubscribeFromFriendStatus(200, handleStatusChange); // 이전의 effect를 정리(clean-up)합니다.
ChatAPI.subscribeToFriendStatus(300, handleStatusChange);     // 다음 effect가 작동합니다.

// 마운트를 해제합니다.
ChatAPI.unsubscribeFromFriendStatus(300, handleStatusChange); // 마지막 effect를 정리(clean-up)합니다.
```
정리)
- effect는 매 렌더링마다 실행
- 다음 effect의 실행 전 clean-up이 동작하고,컴포넌트가 마운트 해제될 때도 동작한다.

>useEffect는 매 렌더링순간에 실행되고,(= 컴포넌트 생명주기에따르면 마운트와 업데이트 순간마다 실행)
clean-up함수는 다음 effect의 실행 직전에 동작한다.(why?, 직전에 실행한 effect를 정리해주기위해) 
또는 마운트해제 후 동작한다.


### Effect를 건너 뛰어 성능 최적화하기 
모든 렌더링 이후에 effect를 정리(clean-up)하거나 적용하는 것이 때때로 성능 저하를 발생시키는 경우도 있습니다.  

이러한 문제는 의존성배열[deps]에 값을 주입하므로써 해결합니다.  

```jsx
useEffect(() => {
  document.title = `You clicked ${count} times`;
}, [count]); // count가 바뀔 때만 effect를 재실행합니다.
```  

>`주의` 모든 객체는 참조값이 변수에 할당됩니다.
의존성배열에 원시값이 아닌 객체(배열)자료형이 들어간다면
모든 리렌더링 상황에서 새로운 참조값으로 초기화되어 effect가 실행됩니다ㅣ
이러한 문제를 해결하기 위해선 useCallback이나 ,useMemo와 같은 캐싱 Hook을 사용해야합니다.

`위 동작은 정리(clean-up)를 사용하는 effect의 경우에도 동일하게 작용합니다.`  

```jsx
useEffect(() => {
  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }

  ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
  return () => {
    ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
  };
}, [props.friend.id]); // props.friend.id가 바뀔 때만 재구독합니다.
``` 

>effect를 실행하고 이를 정리(clean-up)하는 과정을 (마운트와 마운트 해제 시에)딱 한 번씩만 실행하고 싶다면  빈 배열([])을 두 번째 인수로 넘기면 됩니다. 이렇게 함으로써 React로 하여금 여러분의 effect가 prop이나 state의 그 어떤 값에도 의존하지 않으며 따라서 재실행되어야 할 필요가 없음을 알게 하는 것입니다.

<br>



## 5.Hook의 규칙 

### 최상위(at the Top Level)에서만 Hook을 호출해야 합니다
<b> 반복문, 조건문 혹은 중첩된 함수 내에서 Hook을 호출하지 마세요. <b>
이 규칙을 따르면 컴포넌트가 렌더링 될 때마다 항상 동일한 순서로 Hook이 호출되는 것이 보장됩니다.
이러한 점은 React가 useState 와 useEffect 가 여러 번 호출되는 중에도 Hook의 상태를 올바르게 유지할 수 있도록 해줍니다. 
> 조건문 속에 Hook 포함시켜 실행 순서가 패스되거나 밀리게 된다면 예기치 못한 버그를 발생시킵니다. 이것이 컴포넌트 최상위(the top of level)에서 Hook이 호출되어야만 하는 이유입니다.   



```jsx
  // 🔴 조건문에 Hook을 사용함으로써 첫 번째 규칙을 깼습니다
  if (name !== '') {
    useEffect(function persistForm() {
      localStorage.setItem('formData', name);
    });
  }

///

useState('Mary')           // 1. name state 변수를 읽습니다. (인자는 무시됩니다)
// useEffect(persistForm)  // 🔴 Hook을 건너뛰었습니다!
useState('Poppins')        // 🔴 2 (3이었던). surname state 변수를 읽는 데 실패했습니다.
useEffect(updateTitle)     // 🔴 3 (4였던). 제목을 업데이트하기 위한 effect가 대체되는 데 실패했습니다.

```

### 조건부로 effect를 실행하고싶다면? (조건문 -> Hook의 내부에)

```jsx
  useEffect(function persistForm() {
    // 👍 더 이상 첫 번째 규칙을 어기지 않습니다
    if (name !== '') {
      localStorage.setItem('formData', name);
    }
  });
```


### 오직 React 함수 내에서 Hook을 호출해야 합니다
Hook을 일반적인 JavaScript 함수에서 호출하지 마세요.
- ✅ React 함수형 컴포넌트에서 Hook을 호출하세요.
- ✅ Custom Hook에서 Hook을 호출하세요. (use키워드를 붙인 모듈함수)

  <br>

## 6. 커스텀 훅 만들기
- 특성1)사용자 정의 Hook은 단순히 이름이 `use로 시작하는 일반 자바스크립트 함수`입니다. 
- 사용자 Hook은 `다른 Hook을 내부에서 호출`할 수 있습니다.
- 특정 로직을 커스텀훅으로 분리해 모듈처럼 사용할 수 있고, 각각의 (커스텀)Hook에 대한 호출은 서로 독립된 state를 받습니다.
<b> 아래 코드로부터 커스텀훅을 추출해봅시다.</b>
```jsx
import React, { useState, useEffect } from 'react';

function FriendListItem(props) {
  const [isOnline, setIsOnline] = useState(null);
  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });

  return (
    <li style={{ color: isOnline ? 'green' : 'black' }}>
      {props.friend.name}
    </li>
  );
}
```

<b>추출 후...</b>
```jsx
import { useState, useEffect } from 'react';

function useFriendStatus(friendID) {
  const [isOnline, setIsOnline] = useState(null); ///...온라인상태값 선언

  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    } 	    ///...사용자 Status를 변경시킬 함수 정의 

    ChatAPI.subscribeToFriendStatus(friendID, handleStatusChange);
 			///... props로 받은friendID와 정의한 함수를 인자로 넘겨 호출(온라인 여부 상태반환) 
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(friendID, handleStatusChange);
			///... 사용자 상태 구독을 취소하는 클린업함수
    };
  });

  return isOnline;  ///... 온라인 여부(isOnline)를 반환하는 커스텀훅의 반환값
}
```
>React 컴포넌트와는 다르게 `사용자 정의 Hook은 특정한 시그니처가 필요 없습니다`. 무엇을 인수로 받아야 하며 필요하다면 무엇을 반환해야 하는 지를 사용자가 결정할 수 있습니다. 다시 말하지만, `보통의 함수와 마찬가지입니다`.이름은 반드시 use로 시작`해야 하는데 그래야만 한눈에 보아도 `Hook 규칙이 적용되는지를 파악`할 수 있기 때문입니다.



### 사용자 정의 Hook 이용하기
//사용자 로그인 상태를 반환하는 로직을 useFriendStatus 커스텀hook으로 뽑아냈습니다.
//이제 이 커스텀훅은 FriendStatus,FriendListItem 컴포넌트에서 사용됩니다.

```jsx
function FriendStatus(props) {
  const isOnline = useFriendStatus(props.friend.id);

  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}

///


function FriendListItem(props) {
  const isOnline = useFriendStatus(props.friend.id);

  return (
    <li style={{ color: isOnline ? 'green' : 'black' }}>
      {props.friend.name}
    </li>
  );
}

```

- **본래의 코드와 동일한 코드일까요?**
> **`YES`** 커스텀훅으로 로직을 분리하더라도 정확히 같은 방식으로 작동합니다.바뀐 것은 오로지 공통의 코드를 뽑아내 새로운 함수로 만든 것뿐입니다. 사용자 정의 Hook은 React의 특별한 기능이라기보다 기본적으로 Hook의 디자인을 따르는 관습입니다.

- **사용자 정의 Hook의 이름은 `use`로 시작되어야 하나요?**
> **`YES`** 이 관습은 아주 중요합니다. 이를 따르지 않으면 특정한 함수가 그 안에서 Hook을 호출하는지를 알 수 없기 때문에 `Hook 규칙의 위반 여부를 자동으로 체크`할 수 없습니다.

- **같은 Hook을 사용하는 두 개의 컴포넌트는 state를 공유하나요?** 
> **`NO`** Custom Hook을 사용할 때마다 그 안의 state와 effect는 `완전히 독립적`입니다.

- **Custom Hook은 어떻게 독립된 state를 갖는 건가요?**
> **`각각의 Hook에 대한 호출은 서로 독립된 state를 갖습니다`** : useFriendStatus훅을 직접적으로 호출하기 때문에 React의 관점에서 이 컴포넌트는 useState와 useEffect를 새로 호출한 것과 다름없습니다. 


### TIPs) Hook에서 Hook으로 정보 전달하기
// Hook은 함수이기 때문에 Hook 사이에서도 정보를 전달할 수 있습니다.
// 아래 예시코드는 현재 선택된 친구가 온라인 상태인지를 표시하는 채팅 수신자 선택기입니다.

```jsx
const friendList = [
  { id: 1, name: 'Phoebe' },
  { id: 2, name: 'Rachel' },
  { id: 3, name: 'Ross' },
]; ///...참조할 친구리스트 배열 

function ChatRecipientPicker() {
  const [recipientID, setRecipientID] = useState(1); ///... 수신자id 상태값
  const isRecipientOnline = useFriendStatus(recipientID);

  return (
    <> ///...React.Fragment
      <Circle color={isRecipientOnline ? 'green' : 'red'} /> ///...JSX는 js표현식을 허용합니다.
      <select
        value={recipientID} ///...select box의 초기값
        onChange={e => setRecipientID(Number(e.target.value))} ///...셀렉트 이벤트 발생시
선택된 박스의 값(문자열)->Number로 전환 후 RecipientID값으로 상태를 갱신합니다.
      >
        {friendList.map(friend => (
          <option key={friend.id} value={friend.id}>
            {friend.name}
          </option> ///... friendList배열에서 option엘리먼트 리스트를 map으로 생성합니다.
        ))}
      </select>
    </>
  );
}
```
>현재 선택된 친구의 ID를 recipientID state 변수에 저장하고 사용자가 `<select>` 선택기에 있는 다른 친구를 선택하면 이를 업데이트합니다. 그리고 useState Hook 호출은 recipientID state 변수의 최신값을 돌려주기 때문에 이를 useFriendStatus Hook에 인수로 보낼 수 있습니다.

```jsx
  const [recipientID, setRecipientID] = useState(1);
  const isRecipientOnline = useFriendStatus(recipientID);
```

>이를 통해 지금 선택되어있는 친구의 온라인 상태 여부를 알 수 있습니다. 다른 친구를 선택하고 recipientID state 변수를 업데이트하면 useFriendStatus Hook은 이미 선택되어있는 친구의 구독을 해지하고 새로이 선택된 친구의 상태를 구독할 것입니다.
>해설) useFriendStatus커스텀훅을 포함한 컴포넌트의 friendId state가 셀렉트박스 선택으로 갱신되므로 
컴포넌트는 업데이트(리렌더,재호출)되며 useFriendStatus커스텀 훅 내부의 useEffect또한 다시 호출됩니다.
위 과정으로 클린업함수는 기존 선택된 친구의 구독을 취소하고, 다음 effect가 셀렉트된 친구의 온라인상태를 구독할 것입니다.


### useYourImagination()
** 사용자정의 훅(Custom Hook)은 이전 React 컴포넌트에서는 불가능했던 로직공유의 유연성을 제공합니다.**
- Hook을 만들어 폼 다루기
- 애니메이션
- 선언현 구독
- 타이머
그 외에 생각하지 않은 부분까지 훨씬 다양한 쓰임새에 적용할 수 있습니다. 

>너무 이른 단계에서 로직을 뽑아내려고 하지는 않는 게 좋습니다. 함수 컴포넌트가 할 수 있는 일이 더 다양해졌기 때문에 여러분의 코드에 있는 함수 컴포넌트의 길이도 길어졌을 것입니다. 이는 지극히 평범한 일이며 지금 바로 Hook으로 분리해야만 한다고 느낄 필요는 없습니다. 하지만 동시에 사용자 정의 Hook이 복잡한 로직을 단순한 인터페이스 속에 숨길 수 있도록 하거나 복잡하게 뒤엉킨 컴포넌트를 풀어내도록 돕는 경우들을 찾아내는 것을 권장합니다.

<br>

리액트에는 이외에도 더 많은 Additional Hook들이 존재합니다.
이에 대한 논의는 다음 문서를 참고해주세요:)

<br> 

**출처**: [리액트공식문서_Hook](#https://ko.reactjs.org/docs/hooks-custom.html)
\+ 나의 작은 견해
ㅌㄴ
