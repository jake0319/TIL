# TOC
[상태관리](#상태관리)

- [자식컴포넌트가 부모 컴포넌트의 state를 바꾸는 방법?](#자식컴포넌트가-부모-컴포넌트의-state를-바꾸는-방법)

[Recoil 시작하기](#recoil-시작하기)

[atom이란?](#atom이란?)

- [atom의 기본 형태](#atom의-기본-형태)

- [useRecoilState](#userecoilstate)

- [전역상태 관련 Hooks](#전역상태-관련-hooks)

[Selector](#selector)

- [selector의 `set메서드`](#selector의-set)

- [비동기selector와 suspense](#비동기selector와-suspense)

- [비동기처리의 또다른 대안 Loadable](#비동기처리의-또다른-대안-loadable)

- [selector와 캐싱](#selector와-캐싱)

[atomFamily로 복수의 atom 관리하기](#atomfamily로-복수의-atom-관리하기)

- [key 관리의 필요성](#key-관리의-필요성)

[selectorFamily API](#selectorfamily-api)

[Recoil의 장단점](#recoil의-장단점)

<br>
<br>

# Recoil
## 상태관리

- React는 단방향으로 바인딩을하는 라이브러리
- 리액트에서 props는 부모->자식 방향으로만 단방향으로 전달이 가능함.
- props드릴링을 통해 state를 부모->자식 전달가능

<br>

## 자식컴포넌트가 부모 컴포넌트의 state를 바꾸는 방법?
1. 부모state를 변경할 수 있는 `setState함수를 `자식 컴포넌트에 `props로 전달하며 드릴링`. (depth가 깊어질 경우 비효율적) ,자식 컴포넌트에서 전달받은 setState함수는 부모컴포넌트의 state를 변경시킬 수 있다.

2. prop드릴링을 방지를 위한, `State management tool`( redux, recoil ..)사용

> **atom은 결국 컴포넌트의 state를 `manageable`하게 `원격`으로 공유하기 위해 사용하는 것.**


<br>

## Recoil 시작하기
- 프로젝트 세팅: `npx create-react-app [디렉토리명]`
- 리코일 설치: `npm i recoil 또는 yarn add recoil`
- `<RecoilRoot>` 컴포넌트는 Recoil의 hooks를 사용하는 `모든 구성 요소의 조상`이어야 한다.
> store를 별도로 생성해줘야 하는 **Redux**와 달리 리코일은 `RecoilRoot만 제공해도 자동으로 store가 생성됩니다`.
```jsx
import React from 'react';
import {
  RecoilRoot,
  atom,
  selector,
  useRecoilState,
  useRecoilValue,
} from 'recoil';

function App() {
  return (
    <RecoilRoot>
      <CharacterCounter />
    </RecoilRoot>
  );
}
```
- 리코일을 적용하기 위해서는 `<RecoilRoot>`컴포넌트로 래핑해주는 과정이 필요하다.
- App컴포넌트를 감싸주어도 좋음

<br>

## atom이란?

- atom 은 기존의 redux에서 쓰이는 `store(state저장소)` 와 유사한 개념으로, 상태의 단위
- atom은 unique한 id인 `key`로 구분됨.
- atom은 어떤 컴포넌트에서나 읽고 쓸 수 있다(useRecoilState등의 훅 사용을 통해)
- atom의 값을 `읽는` 컴포넌트들은 `암묵적으로 atom을 구독`하고, 해당 atom에 변화가 생긴다면 이를 구독하는 모든 컴포넌트들이 `리렌더링`된다.


## atom의 기본 형태

**`atom({key: 'unique key' ,default: 'default state' })`로 생성**
- 아톰은 일종의 저장소(store), 그 안에 `default state`를 포함한다.
- 아톰은 `atom({객체세팅})`으로 선언
- `key`와 `default` 프로퍼티는 필수로 선언해야한다.
- `원시형데이터` 타입과 더불어 `객체나 배열`같은 complex타입도 atom으로 사용할 수 있다.{key: , default: `초기값`}
>참고: 현재 atom을 설정할 때 `Promise`을 지정할 수 없다는 점에 유의해야 한다. 비동기 함수를 사용하기 위해서는 `selector()`를 사용한다.

**state.js (atom 선언파일)**
```js
export const cookieState = atom({
  key: 'cookieState',
  default: []
});
```

**Cookie.js**
```js
import React from 'react'
import { cookieState } from '../../state';
import { useRecoilState } from 'recoil';

const Cookie = () => {
	const [cookies, setCookies] = useRecoilState(cookieState);
  //atom의 key값으로 호출: 1.아톰state값, 2.상태변경함수 구조분해할당
  //이제 cookies, setCookies라는 변수를 활용할 수 있다.
  return(
    <div>
        {cookie.map(cookie => (
          <Card
            cookies={cookie}
            key={cookie.id}
            idx={cookie.id}
           />
       ))}
      </div>
  );
}
export default Cookie;
```

## useRecoilState
`
const [cookies, setCookies] = useRecoilState(cookieState);
`

- React의 기본 hook인 useState와 굉장히 유사한 형태를 가지고 있다.
- `구조분해할당`으로 atom의 state와 state를 `set`하는 함수를 각각 받아올 수 있다.
- atom의 state가 변경되면 이를 구독하는 모든 컴포넌트의 리렌더링이 일어난다.

<br>

## 전역상태 관련 Hooks
>`전역상태(Atoms, Selector)`를 get/set 하기 위해 Recoil에서 제공하는 Hooks들을 사용한다. 기본적으로 아래 4가지가 크게 사용된다.
`Hook의 인자에는 전역상태인 atom(혹은 Selector)를 넣어준다.(: Recoil에서 atom과 selector는 동일한 훅으로 다뤄준다.`

- `useRecoilState()` : useState() 와 유사하다.
전역상태의 state값,setter함수를 반환한다. 

    `const [state, setState] = useRecoilState(atom|selector)` 형태로 구조분해 할당하여 사용한다.

- `useRecoilValue()` : 전역상태의 state상태값만을 반환한다.

- `useSetRecoilState()` : 전역상태의 setter 함수만을 반환한다.

- `useResetRecoilState()` : 전역상태를 default(초기값)으로 Reset 하기 위해 사용된다.
reset한 default값이 반환된다.

<br>

```jsx
import { useRecoilValue, useSetRecoilState } from 'recoil';
import { cookieState } from '../../state';

const cookies = useRecoilValue(cookieState);
//state값 
const setCookies = 
useSetRecoilState(cookieState);
//setState함수
const resetCookies = useResetRecoilState(cookieState);
//state초기화후 초기값
```
<br>


## Selector
- Selector는 다음 두 가지 의미를 가진다.(둘중 한 개만 만족할 수도 있음)
1. 아톰에서 `파생된`(atom데이터를 재활용한) 데이터 조각
    - `파생 데이터를 만들어주는 역할`
2. 특정 데이터를 반환하는 `순수함수`
    - 순수함수: 동일input -> 동일 output을 산출하는 함수 
    - `atom이 처리 불가능한 promise같은 비동기데이터 반환가능`
> selector는 `1.`아톰의 데이터를 활용해 (getter)파생된 데이터를 반환하거나 `2.`아톰의 데이터를 활용하지 않더라도 특정 데이터를 반환할 수 있다. 
이 `두 가지 용도로 사용된다`.

**예제1)**
```jsx
import "./App.css";
import {
  RecoilRoot,
  atom,
  selector,
  useRecoilState,
  DefaultValue,
  useResetRecoilState,
} from "recoil";

const tempFahrenheit = atom({
  key: "tempFahrenheit",
  default: 32,
});
//아톰

const tempCelcius = selector({
  key: "tempCelcius",
  // get: ({ get }) => ((get(tempFahrenheit) - 32) * 5) / 9,
  get: ()=> 30, ///...selector의 get메서드 리턴값이 selector의 상태값
  set: ({ set }, newValue) =>
    set(
      tempFahrenheit, //아톰 state
      // newValue instanceof DefaultValue ? newValue : (newValue * 9) / 5 + 32
      3 //아톰state값을 고정값 3으로 set해주고, 추후 구조분해할당된 setter호출시 무조건적으로 3으로 변경된다.
    ),
});
// 아톰을참조하는 셀렉터
function TempCelcius() {
  const [tempF, setTempF] = useRecoilState(tempFahrenheit);
  const [tempC, setTempC] = useRecoilState(tempCelcius);
  const resetTemp = useResetRecoilState(tempCelcius);

  const addTenCelcius = () => setTempC(tempC + 10);
  const addTenFahrenheit = () => setTempF(tempF + 10);
  const reset = () => resetTemp();

  return (
    <div>
      Temp (Celcius): {tempC}
      <br />
      Temp (Fahrenheit): {tempF}
      <br />
      <button onClick={addTenCelcius}>Add 10 Celcius</button>
      <br />
      <button onClick={addTenFahrenheit}>Add 10 Fahrenheit</button>
      <br />
      <button onClick={reset}>Reset</button>
    </div>
  );
}
function App() {
  return (
    <div className="App">
      <RecoilRoot>
        <TempCelcius></TempCelcius>
      </RecoilRoot>
    </div>
  );
}

export default App;

```
- selector도 atom처럼 state값이 존재한다(:get메서드의 리턴값)
- 셀렉터도 atom 처럼 unique한 `key`값이 필요하다.
- 셀렉터의 get메서드 리턴값이` useRecoilState(selector) state값이 된다`.
- 셀렉터의 set메서드가 `useRecoilState(selector)`의 구조분해할당한 `setter함수`가 된다.
- 추가) 셀렉터의 get메서드의 인자에는
상태값을 조작할 때 사용하는 `특정객체`가 전달되는데,`{get}`을 파라미터에 넣어주어 `구조분해할당`으로 전역상태(atom,selector)의 값을 참조하는 `get()`함수를 사용할수 있다.
- 추가) set메서드에는 `( {set} , newValue )` 두개의 인자가 전달되며, set: ({set},newValue)=>set(prevAtomState,newValue)로 형태로 set해준다.

<br>

### selector의 set
```jsx
const proxySelector = selector({
  key: 'ProxySelector',
  get: ({get}) => ({...get(myAtom), extraField: 'hi'}),
  set: ({set}, newValue) => set(myAtom, newValue),
  //myAtom -> newValue
});
```
- set메서드는 구조분해시킬 객체와,newValue를 인자로 받는다.
- useRecoilState로 위 셀렉터 호출시 setter함수가 위 set()메서드로 동작한다.
<br>

**예제2**
```jsx
import { atom, selector, useRecoilState, useRecoilValue} from 'recoil';
export default function Counter() {
  const [count,setCount] = useRecoilState(countState);
  //countState아톰의 state 참조
  const oddEven = useRecoilValue(oddEvenState);
  // oddEvenState셀렉터의 derived state참조

  return(
    <div className='counter'>
      Count: {count} / 홀짝: {oddEven}
       <br />
       <button onClick={()=> setCount(count - 1)}> 1 감소</button>
       <button onClick={()=> setCount(count + 1)}> 1 증가</button>
    </div>
  )
}
```
- `atom과 selector는 구분없이 동일한 Hook으로 다뤄준다!`
- Read-only데이터에 사용하는 Hook이 따로 있다.(useRecoilValue)

<br>

## 비동기selector와 suspense

- selector로 비동기처리시 비동기데이터 fetching이 완료되기 전까지 렌더링을 멈춰줄 `suspense`와 함께 사용해야 에러가 발생하지 않는다.
- `<React.Suspense>`: 컴포넌트를 완전히 렌더링할 수 있을 때까지 렌더링을 멈춰두는 기능`(fallback prop으로 임시 렌더링컴포넌트를 전달한다)`
```jsx
export defualt function App(){
  return(
    <div className="App">
    <h1>Random Cat</h1>
    <p>페이지를 새로 고침할 때마다 랜덤한 고양이 사진을 보여줘야옹</p>
    <RecoilRoot>
      <React.Suspense fallback={null}>
        <RandomCat/>
        //비동기 selector포함 컴포넌트
      </React.Suspense>
    </RecoilRoot>
    </div>
  )
}
```
- `비동기처리가 이뤄지는 selector가 있는 컴포넌트`를 React.Suspense로 감싸줍니다.
- `fallback prop`에는 비동기처리가 완료되기까지 보여줄 컴포넌트를 전달합니다.
>위와같이 비동기selector에 suspense를 이용하면 비동기 상태처리를 간단하게 할 수 있다.
- 어떤컴포넌트를 언제 렌더링할지 리액트컴포넌트가 최적화시켜줌.(컨커런트모드),
리액트 서스펜스는 이 컨커런트모드 기능의 일부.

## 비동기처리의 또다른 대안 Loadable
- suspense대신 `useRecoilValueLoadable`훅 활용하기 (`switch(Loadable.state)로 렌더링할 컴포넌트를 세밀하게 조정해줄 수 있음`)
    - `비동기 셀렉터의 값 참조`
```jsx
export defualt function App(){
  return(
    <div className="App">
    <h1>Random Cat</h1>
    <p>페이지를 새로 고침할 때마다 랜덤한 고양이 사진을 보여줘야옹</p>
    <RecoilRoot>
        <RandomCat/>
        //비동기 selector포함 컴포넌트
    </RecoilRoot>
    </div>
  )
}
```
- 위 코드에서 로더블 활용을 위해 `useRecoilValue`  => `useRecoilValueLoadable` 훅으로 바꿔줍니다.
- use...Loadble훅은 `Loadable객체`를 반환합니다.

<br>

### Loadable객체는..
- `state`프로퍼티를 통해 데이터처리 상태를 확인하고
    - Loadable.state:`<string>`
   1. "hasValue" 
   2. "loading"
   3. "hasError" 
- `contents`프로퍼티를 통해 실제 콘텐츠를 가져올 수 있습니다.
```jsx
export default function RandomCat(){
  //비동기처리 selector를 포함한 컴포넌트

  const photoUrlLoadable = useRecoilValueLoadable(randomCat);
  //비동기 셀렉터로 호출 -> 로더블객체 생성, 로더블 state & contents활용가능

  switch (photoUrlLoadable.state){
    case 'hasValue':
      content = <img src={phtoUrlLoadable.contents}> alt="random cat" />; //리액트 엘리먼트 변수에 할당
      break; //탈출 
    case 'hasError':
      content = '데이터를 불러오는 중 에러가 발생했습니다';
      break;
    case 'loading':
      default:
        content = 'loading...';
  }
  return <div className="random-cat">{content}</div>;
}
```
<br>

### selector와 캐싱
>selector란 구독하고 있는 atom에 변화가 생길 때마다 새로운 값을 리턴해주는 순수 함수이다. 즉, get을 통해 가져온 값은 의존성을 갖고 있어, `구독하는 값이 변할 때마다 새로운 값을 갱신한다`, 위와 반대로 get메서드에 async와 await을 추가함으로서 비동기 처리시` 구독하는 atom 값이 변하지 않을 경우, 동일한 값을 리턴한다`. 즉, get()의 파라미터가 동일하다면, 캐시된 값을 리턴한다.

- 셀렉터는 참조한 아톰에 자동으로 의존성이 걸린다
- selector는 동일한 atom state 참조에 대해 memoize된 리턴값을 반환한다.
- 같은 `get참조값`에 대해 캐싱된 값을 반환해준다.
```jsx
export cosnt keywordState = atom({
  key: 'keywordState',
  default: '',
})

export const animeList = selector({
  key: 'animeList',
  get: async ({get})=>{
    const keyword = get(keywordState)
    ///...사용한 아톰에 자동으로 의존성이 걸린다.(값을 캐싱한다)
    if(!keyword || keyword.length < 3) {
      return [];
    }
  }
})
```



<br>

## atomFamily로 복수의 atom 관리하기
- atomFamily의 호출 인자값은 atom과 유사하다.
- atomFamily의 default값은 atom과 달리 특정한 파라미터를 받는 함수형태가 될 수 있다.
- atomFamily API는 호출할 때마다 지정한 형식의 atom을 생성하는 팩토리함수를 리턴한다.
- atomFamily호출시 넣어준 인자는 default: 메서드의 인자가된다.

```typescript
const modalsAtomFamily = atomFamily<ModalInfo, ModalId>({
  key: "modalsAtomFamily",
  default: (id) => ({
    id,
    isOpen: false,
    title: "",
  }),
})
```
**아래와 같이 atomFamily함수에 id를 전달하여 호출하면 해당 id를 활용한 atom이 생성된다.**

```ts
const [myModal, setMyModal] = useRecoilState(modalsAtomFamily("myModal"))
const [yourModal, setYourModal] = useRecoilState(modalsAtomFamily("yourModal"))
```
<br>

### key 관리의 필요성
- atomFamily는 팩토리함수를 만들어내는 API기 때문에 어떤 atom을 만들었는지 알 수 없기 때문에 atom생성시 별도의 key값에 대한 관리가 필요하다.
>예를 들어, 현재 열려 있는 모달들을 한 번에 닫는 등의 작업 등은 atomFamily 만으로는 수행할 수 없을 것입니다. 따라서, atomFamily와 동시에 atomFamily를 통해 생성된 Atom의 key를 별도로 관리해주는 작업이 필요합니다

```jsx
export const modalIdsAtom = atom<ModalId[]>({
  key: "modalIdsAtom",
  default: [],
})
//결국 새 모달을 생성하고자 할 때에는 아래와 같이 두 단계의 작업이 필요하다는 의미입니다.

const setModalIdsAtom = useSetRecoilState(modalIdsAtom) //setter함수 취득 

/* 1. atomFamily로 모달 Atom 생성 */
const myModal = modalsAtomFamily("myModal")

/* 2. 생성한 Atom의 key를 별도의 배열에 넣기 */
setModalIdsAtom((prev) => [...prev, modalIdsAtom])
```

<br>

## selectorFamily API
> Recoil의 selector는 set 값을 이용해 쓰기 가능한 상태(writable state)를 정의할 수 있습니다. set은 특정한 타입의 파라미터를 받는데, 이 타입의 파라미터를 이용해 다른 Recoil Atom을 업데이트하는 용도로 사용할 수 있습니다.
- selector와 selectorFamily <-> atom , atomFamily관계와 동일하다.
- 즉, selectorFamily는 한 파라미터(위 예시에서는 ModalId)를 받아 이 파라미터를 이용해 작업을 수행하는 selector를 리턴하는 팩토리 함수를 리턴합니다.
- selectorFamily로 만든 셀렉터생성함수는 호출 인자값으로 get,set메서드에 인자를 전달할 수 있다.

```typescript
export const modalsSelectorFamily = selectorFamily<ModalInfo, ModalId>({
  key: "modalsSelectorFamily",

  get: (modalId) => ({ get }) => get(modalsAtomFamily(modalId)),

  set: (modalId) => ({ get, set, reset }, modalInfo) => {
    if (modalInfo instanceof DefaultValue) {
      reset(modalsAtomFamily(modalId))
      set(modalIdsAtom, (prevValue) => prevValue.filter((item) => item !== modalId))

      return
    }

    set(modalsAtomFamily(modalId), modalInfo)
    set(modalIdsAtom, (prev) => Array.from(new Set([...prev, modalInfo.id])))
  },
})
```
- selectorFamily에서 get,set메서드에는 기존 selector의 메서드와 달리 `(호출값) =>` 을 전달하는 과정이 추가된다.

- `get` 내부의 syntax를 살펴보면 생기는 의문이 있습니다. Recoil은 동일한 atomFamily로 생성된 Atom들을 구분하기 위해 atomFamily에 넘겨준 파라미터를 내부적인 ID로 이용합니다

```ts
const myModal = useRecoilValue(modalsAtomFamily("myModal"))
const notMyModal = useRecoilValue(modalsAtomFamily("myModal"))
// myModal과 notMyModal은 같은 Atom의 값을 가리키게 될 것입니다.
```
- `set` 값에서는 첫 번째 파라미터로 set 내부에서 다른 Recoil 상태를 읽거나 업데이트할 때 사용하는 get, set, reset 함수를 가져옵니다. 두 번째 파라미터는 특정한 값을 받는데, 이 값을 이용해 다른 상태를 읽거나 업데이트 할 수 있습니다.

<br>

## Recoil의 장단점
### Recoil장점
- React와 굉장히 잘 연계된다.
- 사용법이 직관적이고 단순하다.

### Recoil단점
- Hooks를 통해서만 사용할 수 있다.
- 프로덕션 레벨에서 사용하기엔 아직 약간의 부담이 있음.
- 현재는 디버깅 도구의 지원이 미비하다.

<br>

<br>

<br>

## 출처:
[Recoil 공식문서](https://recoiljs.org/ko/docs/introduction/getting-started)

[Recoil: 왕위를 계승하는 중입니다](https://tv.naver.com/v/16970954)

[Recoil 정복기](https://abangpa1ace.tistory.com/m/212)

[Recoil:React를 위한 상태 관리 라이브러리](https://wit.nts-corp.com/2022/10/13/6586)

[[Recoil] Recoil 200% 활용하기](https://velog.io/@juno7803/Recoil-Recoil-200-%ED%99%9C%EC%9A%A9%ED%95%98%EA%B8%B0#3%EF%B8%8F%E2%83%A3---recoil%EC%9D%98-%EA%B8%B0%EB%B3%B8-atom)

[Recoil atomFamily를 통해 여러 개의 Atom 관리하기](https://junglast.com/blog/recoil-atomfamily-atom)
