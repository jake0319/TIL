

# React Course - Typescript - Intro 

## 1. Intro 

- ts쓰는 이유1 : 타입에러 체크 
- 객체 프로퍼티 참조시 인텔리센스 


```jsx 
import { Address } from "./interfaces/interfaces";

interface InlineDocumentationTsProps { 
 address: Address;
}


export default function InlineDocumentation({
address,} : InlineDocumentationTSProps ) {
return ()
```
- prop 구조분해할당시 타입 지정해주는방법 

<br>

### Install Typescript
- npx create-react-app my-app --template typescript 


<br>

## 2. Props

### 컴포넌트 Props 타입 정의하기

타입스크립트는 변수,리턴값,함수에 타입지정한다.
```jsx
// interface: class느낌으로 타입작성하기(주로객체) 
interface AppProps {
  headerText: string;
}

//구조분해할당 옆에 interface타입명시 
// <App headerText="some strings"> //headerText = props.headerText
// headerText를 {}로 감쌋으므로 interface타입 지정
export default function App({headerText}: AppProps) {
  return( 
    <>
    <h1>{headerText}</h1>
    </>
  )
}
```
- 구조분해할당하는 props는 객체형태의 타입을 지정하는 interface타입을 지정하자

<br>

### 옵셔널 props
- prop은 컴포넌트에 전달하는 과정에서 정확한 타입을 할당시켜줘야함.

```jsx

// interface: class느낌으로 타입작성하기(주로객체) 
interface AppProps {
  headerText: string;
  extraText?: string; //optional type 
}

//구조분해할당 옆에 interface타입명시 
// <App headerText="some strings"> //headerText = props.headerText
export default function App({headerText,extraText}: AppProps) {
  return( 
    <>
    <h1>{ extraText && <p>{headerText}</p>}</h1>
    </>
  )
}

```
- 값이 있을 수도 없을 수도 있는 경우 옵셔널연산자`?`를 type에 명시해야한다. 

<br>

### Default props

- prop값에 default value를 적용하는 경우 값이 없다면 default값이 할당된다.
// (컴포넌트내부 prop정의)extraProp='extra value' -> (실제 전달시)extraProp={333}  : Error 
- 이렇게 default값이 지정된 prop에 default값의 타입이 아닌 다른 타입의 값을 할당하면 에러가 발생한다. 

<br>

## 3.useState

- 기본추론: useState초기값에 원시형값을 넣으면 type추론을 자동으로해줌

- setState함수로 user값을 null-> user object로 변경시켜주면 
타입 inference를 하지못해 error발생: 별도의 type Annotation이 필요하다.

- 중첩객체는 객체속에 interface타입을 nesting하자(better than type 하드코딩)
```ts
interface Address{
  street: string;
  number: number;
  zip: string;
}

interface User{
  name:string;
  age: number;
  country: string;
  address: Address; // 중첩객체는 위에서 정의한 Address interface타입 할당 
  admin: boolean;
}
```

<br>

### setState()로 변경되는 state의 타입 define
```tsx
export default function App(){
  const [user,setUser]=useState<User|null>(null);
  const fetchUser = ()=> setUser({
    name: "Mitchel" ,
    age: 23 ,
    country: 'the Netherlands' ,
    address: {
      street: "Main st.",
      number: 88,
      zip: "21345",
    },
    admin: false,
    })

return ( 
  <>
  <button onClick={fetchUser}>Fetch User on CLick</button>
  {user && <p>{`welcome ${user.name}`}</p>}
  </>
)
}
```
- user State는 `useState<generic>`형태로 변경 가능한 값의 타입을 `union type으로 명시해주어야 한다.

- tsx파일의 interface들은 `inferfaces.ts`파일로 별도로 분리한 뒤 import해서 사용하는 것이 좋음.(export 명시 필수)

<br>

## 4. Forrms and Events 

### (before get in)_타입스크립트 함수타입
```
//함수타입(type,interface)은 파라미터를 꼭 명시해주어야함
// 간단한 함수type 정의시에는 `=>`화살표 를 사용하자
type FnType = (arg: ArgType) => ReturnType;

// 다른 모든 경우는 :를 쓰라.(type)
type FnAsObjType = {
  (arg: ArgType): ReturnType;
};

//interface에서도 :
interface InterfaceWithFn {
  (arg: ArgType): ReturnType;
}

const fnImplementation = (arg: ArgType): ReturnType => {
  /* 구현부 */
};
```

<br>

```tsx
import { ChangeEventHandler, useState } from 'react';

const defaultFormData = { 
  title: "",
  body: "",
};

export default function App() {
  const [ formData ,setFormData ] = useState(defaultFormData);
  const { title ,body } = formData;

  //onChange이벤트 객체의 type명시해야함.
  const onChange= (e: React.ChangeEvent<HTMLInputElement>) => {
    setFormData((prevState)=> ({
      ...prevState,
      [e.target.id]: e.target.value,
    }));
  };
  
  // submit이벤트 객체의 type명시해야함.
  const onSubmit = (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    console.log(formData);
    setFormData(defaultFormData);
    };
  return (
    <>
    <h1>Form</h1>
    <p>Create a Post</p>

    <form onSubmit={onSubmit}>
    <label htmlFor="title">Title</label>
    <br/>
    <input type="text" id="title" value={title} onChange={onChange}></input>
    <br/>
    <br/>
    <label htmlFor="body">Body</label>
    <br/>
    <input type="text" id="body" value={body} onChange={onChange}/>
    <br></br>
    <br></br>
    <button type="submit">Upload post</button>
    </form>
    </>
  )

}
```
- form 에는 onSubmit(preventDefault)이벤트 달기 
- label(htmlFor속성:input의 id와 연결) 과 input태그는 짝궁(label: input태그의 제목)
- input의 속성은 ( type , id , value , e.handler)
- form태그에는 하나의 submit타입의 button 
- submit,change이벤트 객체에 type꼭 명시하기
  - 호출부에서 `onChange={(e)=>onChagne(e)}`형태로 작성해보면 인텔리센스로 확인가능 

<br>

# 5. Generic Components 

__Generic.tsx__
```tsx
//DataGridProps는 id:number속성을 가지고 있어야하므로 T extends Item
interface Item {
  id: number;
}

interface DataGridProps<T> {
  items: T[]; //T타입 요소가들어간 배열 
}

// { items },, 구조분해하려는 items의 타입을 : DataGridProps<T> 제네릭타입으로 정의 
export default function DataGrid<T extends Item>({ items }: DataGridProps<T>){
  return(
    <>
    {items.map((item) => (
      <li key={item.id}>{JSON.stringify(item)}</li>
    ))}
    </>
  );
}
```
- items의 요소 item이 id속성을 갖는것을 `<T extends Item>`으로 명시해주어야함
- `<T>[]`제네릭은 동적으로 요소 타입이 결정되는 배열로서 => `T타입이 item의 타입이 됨.`
- `JSON.stringify(obj)` :직렬화(암호화=JSON객체화)
- 


<br>

__App.tsx__
```tsx
import DataGrid from './Generic';

interface User {
  id: number;
  name: string;
  age: number;
}

export default function App() {
  const users: User[] = [
    {id:1 , name: "John", age: 55 },
    {id:2 , name: "Mitchel", age: 23 },
    {id:3 , name: "Mike",  age: 50 }
  ]
return ( 
<>
  <DataGrid items={users}></DataGrid>
</>)
}
```
- DataGrid컴포넌트에는 items prop으로 users배열 할당 
- users배열은 여러 데이터타입을 요소로 가지므로 <T> 제네릭을 활용한다. 
- li를 map으로 렌더링할 때 key프롭 반드시 정의하기 .

<br>

## 6. Type Narrowing with React

### `in` operator Narrowing 
- `if("title" in item){ }`으로 내로잉

<br>

### equlity(===) Narrowing (based on a "type" property)
- `if(item.type === " imageItem"){ }` 으로 내로잉하기 
- 단점: 백엔드에서 페칭하는 데이터에 type 프로퍼티가 존재해야 ===연산자로 내로잉이 가능

<br>


__Component.tsx__
```tsx
interface ImageItem {
  id: number;
  title: string;
  imageUrl: string;
}

interface QuoteItem {
  id: number;
  quote: string;
}

export type Item = ImageItem | QuoteItem;

//Item양식에 따라 타입 세분화시키기
// export interface Item { 
//   id: number;
//   title?: string;
//   imageUrl?: string;
//   quote?: string;
// }

//객체리터럴의 형태를정의할 수도, items객체의 타입을 정의할 수도 있음.
interface ComponentProps {
  items: Item[]; //Item타입의 객체를 요소로 갖는 배열 = items의 형태 
 }

export default function Component({items}: ComponentProps){
  return(
    <>
    <ul>
      {items.map((item)=> {
        // title속성이 item에 있는경우 아래 코드를 실행(타입내로잉))
        if("title" in item)
        return(
          <li key={item.id}>
            {item.title && <p>{item.title}</p>}
            {item.imageUrl && (
              <img
              style={{maxWidth: "15rem"}} //인라인스타일
              src={item.imageUrl}//src= url
              alt={item.title}></img>
            )}
          </li>)

            {item.quote && <p style={{fontStyle: " italic"}}>{item.quote}</p>}
        
      })}
    </ul>
    </>
  )
}
```

<br>

__App.tsx__
```tsx
import Component, { Item } from "./Components"

export default function App(){
  const items: Item[] = [
    {
      id:1,
      title: "A nice sunset",
      imageUrl:
      "https://imgaes.unsplash.com/photo-1231231231221313-d23913812312",
    },
    {
      id:2,
      quote:
        "Lorem ipsum dolor sit amet consectetur adipisicing elit. Error recusandae perspiciatis fuga, illum quos molestias esse rerum doloribus. Illum at laudantium a aut numquam autem, eveniet ratione distinctio eum adipisci.",
    },
  ];

  return <Component items={items}/>;
}
```

<br>

### 출처:
- [TechBase_React Course with TS](https://www.youtube.com/watch?v=vbHpjdu7N9s&list=PLG-Mk4wQm9_LyKE5EwoZz2_GGXR-zJ5Ml&index=8)
