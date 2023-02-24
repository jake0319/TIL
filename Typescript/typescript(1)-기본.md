# Typescript 기본지식

- 메인 룰: typescript는 최종적으로 javascript로 변환된다. 순전한 typescript 코드를 돌릴 수 있는 것은 deno이나 대중화되지가 않았음. `브라우저, 노드는 모두 js 파일을 실행한다`.
- typescript는 `언어이자 컴파일러(tsc)`이다. 컴파일러는 ts 코드를 js로 바꿔준다.
tsc는 tsconfig.json(`npx tsc --init 시 생성`)에 따라 ts 코드를 js(tsc 시 생성)로 바꿔준다. 인풋인 ts와 아웃풋인 js 모두에 영향을 끼치므로 tsconfig.json 설정을 반드시 봐야한다.
- 단순히 타입 검사만 하고싶다면 `tsc --noEmit` 하면 된다.
- 개인 의견: tsconfig.json에서 그냥 `esModuleInterop: true`, `strict: true` 두 개만 주로 켜놓는 편. `strict: true가 핵심`임.
- ts 파일을 실행하는 게 아니라 `결과물인 js를` 실행해야 한다.
- 에디터가 필수가 됨. VS Code나 웹스톰 반드시 필요. 메모장으로 코딩 불가능한 지경에 이름.
- `간단한 타입은 타입스크립트가 알아서 타입을 추론해준다.`



<br>



# TS 기본 세팅하기

 1.`npm init -y`(:yes)  (node 프로젝트 셋팅)
- 초기 node 프로젝트 세팅(package.json 파일 생성됨)

 2. `npm i(init) (-g:글로벌 설치 1번) typescript`
- 프로젝트 폴더에 ts설치
 3. `npx tsc --init` (ts 설정파일 생성)
- `npm` 아니고 `npx` 주의 

- tsconfig.json 타입스크립트 설정파일 생성

## tsconfig.json - 기본옵션들 (자동으로 켜짐)
> `"esModuleInterop": "true"`, `"strict": "true"`
 (필수옵션)

1. “allowJs” : true, 
- true설정시 , js,ts 동시에 사용할 수 있는 설정
- js->ts 마이그레이션에 유용함.


2. “strict” : true,  (필수)
- 타입검사를 엄격하게 true
- false로 하면 ts쓰는 의미가 많이 줄어듬

3. “target”: “es2016”, 
- 자바스크립트 어떤 버전으로 컴파일할지 옵션
- ("target": “ES5”, : IE에도 호환가능)

4. “module”: “commonjs”,
- 파일 import관련 옵션
- 최신 모듈시스템: "es2015" =~ es 2022
- node 모듈시스템: "commonjs"

5. forceConsistentCastingInfileNames: true 
> import someThing from '파일명' 문법 사용시 파일명의 대소문자를 
엄격히 구분해서 import할 수 있도록,서도 다른 파일로 인식하도록 하는 옵션(=true)
윈도우에서는 구분을 안해서 true를 켜줘야하고, (filename.ts=Filename.ts)
리눅스&맥은 기본적으로 구분해줌.
=>true로 설정해두자

6. skiplibCheck: true 
- 라이브러리 체킹 건너뛰게하는 옵션(=true)
>라이브러리 다운시 각 라이브러리의 타입을 정리해둔 `d.ts`이 존재함,이에대한 타입체크를 건너 뛰어주세요~ 라는 옵션 ( 모든 라이브러리 타입 검사시 컴파일러가 느려질 수 있으므로 타입체킹을 건너 뛴다,실제로 쓰는 파일만 검사하도록 해주는게 좋음 )



<br> 



## 기본명령어

### `npx tsc`
- ts 트랜스파일러(컴파일러) 실행 명령어
//ts파일을 js파일로 바꿔줌 
- typescript 명령어 해설 및 버전 출력

### `npx tsc --noEmit` (컴파일과 출력은 별개의 과정임)
- 타입검사한 내역만 출력해줌 (컴파일한 파일은 생성하지 않음) 
- VScode같은 에디터 사용시 자동으로 계속 돌아가는 옵션

### `tsc -w` 옵션 켜두면 ts파일 저장시마다 자동으로 `npx tsc`


<br>


# TS 기본 문법 
- Typescript는 `변수`,`매개변수`,`리턴값`에 단순히 타입을 지정할 뿐임.


## 변수에 타입지정하기
- const a: string = 'hello';
- const b: number = 5;
- const c: boolean = true;
- const d: undefined = undefined;
- const e: null = null;
- const f: any = true; //any타입: 아무거나 다 넣어도 됨.
- const g: true = true; //값을 그냥 리터럴로 고정해버리기



## 함수에 타입지정하기
- (선언식,표현식,type aliasing ,interface)
// 함수의 파라미터,리턴값에 타입을 지정해주세요 
// (파라미터,리턴값)의 타입위치는 함수 형태마다,타입 선언 방식마다 조금씩 다릅니다.

```typescript
function add(x: number,y: number):number { return x + y } 
// 함수 선언식
// `리턴값타입`의 자리는 `매개변수타입 바로 뒤`

const add1:(x:number, y: number) => number = (x,y) => x + y 
// 함수표현식(화살표함수 할당)
// 함수타입을 변수명 옆에 할당(함수표현식 형태로)
```

## type키워드로 타입용 `변수` 만들기:  `type aliasing`
```typescript 
 type Add = (x:number, y:number) => number;
 const add: Add = (x,y) => x + y;
``
 - Tips) ts는 js로 말이 되는 코드여야한다 (콜론: 뒤의 타입작성부를 지워보자)

## interface로도 타입 지정이 가능
```typescript
 interface Add {
     (x:number,y: number): number;
}
```
- interface 타입변수명 { }중괄호 안에 타입작성, `함수타입(파라미터:리턴값)`



## 객체 타입지정
```typescript
const obj: { lat: number, lon: number} = { lat: 37.6, lon: 127.6 }
``` 
- 객체리터럴

## 배열 타입지정
```typescript
const arr: string[] = ['123','456'] 
// 1.일반적인 배열타입,  `속성[]`
const arr2: Array<number> = [123,456]
 //2.꺽쇠(제네릭) 표기법 
```

## 튜플타입 (: 고정된 길이의 배열 )
`const arr3: [number, number, string] = [123,456,'hello']`


<br>


# 타입추론 팁
- 타입스크립트의 타입추론에 최대한 맡기기
- 타입은 최대한 좁게 작성하기 ( 값 > 자료형 ) 


<br>


# JS로 변환시 사라지는 부분
- 콜론,타입,declare,interface,as, ...

## 타입 선언은 미리 작성해둘 수 있다.
```ts
function add(x:number, y:number): number; ///...타입만 미리 선언
function add(x,y) { 
    return x + y;
} ///... 타입선언부터 하고 함수 선언하기도 가능

```

## as키워드 
- `as`키워드 앞의 타입을 강제로 특정 타입으로 변환시켜준다.
```ts
let a = 123;
a = 'hello' as unknown as number
```
- `unknown`은 잠시 타입 추론을 미루는(알 수 없는 타입임을 명시) 작업이므로 추후 `if문 내로잉` 또는 `as unknown as some_Type` 으로 타입 지정을 다시 해줘야함.


<br> 


# never 타입 & non-null assertion

## non-null assertion
```ts
const head = document.querySelector('#head')!;
```
- `!`: non-null assertion
- head태그가 기본적으론 Element|null 타입인데 null(undefined)이 아님을 보증함 
- 추천하지 않는 방식이므로 `if문으로 타입 내로잉하기`
- 타입체커에 null 또는 undefined가 아님을 알려주는 방식

## never타입
```ts
try { 
	const array:string[] = [];   ///...빈배열[]  할당시 never타입 
	array.push('hello'); ///...string[]으로 타입지정해야 push메서드 사용가능
} catch (error) {
	error;
}
```

[never에 대한 좋은설명 글](#https://ui.toast.com/weekly-pick/ko_20220323)


<br>


# 여러가지 타입 작성방식

## 템플릿 리터럴 타입
- 타입도 자바스크립트처럼 템플릿리터럴이 가능하다.
- 타입을 `명확한 값`으로 할당해서 지정한 '값'으로만 받을 수 있음을 명시

```ts
type World = 'world';
Const a:World = 'world';
///...ctrl+shift : 자동완성추천

type Greeting = `hello ${World}`;
///...템플릿 리터럴 타입

```
## rest 파라미터 타입
```ts
function rest(...args: string[]) { ///args가 string[] 타입임을 의미 
	console.log(args); ///['1','2','3'] 
}

rest('1','2','3');
```
- `...args`는 args배열을 리스트화
- args배열은 함수 호출시 넘긴 `arguments`들의 배열이 된다.

## 튜플 타입
- 배열 요소의 `갯수 및 타입이 정해진` type
```ts
const tuple: [ string, number] = ['1',1];
tuple[2] = 'hello';  ///... Error

tuple.push('hello') ///... 이건 ts가 못막아줌
```

<br>


# keyof,typeof 연산자와 enum

## enum
- `const enum 변수` 형태로 선언
```ts
const enum EDirection { 
 Up = 123,
 Down = 'How are you',
 Left = 'say hi',
 Right= 'enum',
}
const a = EDirection.Up;
const c = EDirection.Left;
```
- const `enum 변수명`으로 선언
- `enum`: 임의의 숫자나 문자열을 할당할 수 있으며, 하나의 그룹으로 묶고싶을 때 사용한다.
[Enum에 대한 설명글](#https://joshua1988.github.io/ts/guide/enums.html#%EC%9D%B4%EB%84%98-enums)
- 숫자형(:auto incrementing), 문자형(:리버스맵핑) , 혼합형 enum 등이 가능 

> BUT! enum은 ts의 문법이므로 트리쉐이킹이 되지 않는다 -> 번들링시 포함되므로 쓰지 않는 것이 좋다.
 대신 Union type 사용을 권장 ( Union Type > const enum > enum )
[Enum을 지양해야하는 이유](#https://engineering.linecorp.com/ko/blog/typescript-enum-tree-shaking)



```ts
const ODirenction = {
 Up: 0, ///... key값은 문자열 오버라이딩
 Down: 1, ///...`as const`없다면 number로 기본 타입추론
 Left: 2,
 Right: 3,
} as const; 

--아래와 같다--

const ODirection: {
    readonly Up: 0;
    readonly Down: 1;
    readonly Left: 2;
    readonly Right: 3;
}
```
- 객체선언 뒤에 `as const`붙이면 각 속성이 `readonly`인 `리터럴타입`을 생성해줌. 
// 각 속성에 값 자체로 타입이 할당됨.



## typeof
- `값`으로부터 타입을 만들고싶을 때 사용하는 연산자

## keyof
- `타입`의 key속성값만 뽑아서 새로운 타입으로 만들고싶을 때 사용

## keyof & typeof

```ts
type Direction = typeof ODirection[keyof typeof ODirection]
```
1. ODirection객체(리터럴값)의 '타입'을 typeof 으로 뽑아내고
2. 해당 타입의 key속성값들을 keyof으로 뽑아내서  
3. 해당 객체의 키값들을 유니온 타입으로 만들고 =>  `'Up'|'Down'|'Left'| 'Right'`(type)
4. 이것을 ODirection객체의 [computed property]형태로 참조
//( ODirection[keyof typeof ODirection];
// 1 | 2 | 3 | 4  (아직은 객체의 참조값에 불과, 타입이아님)
이것을 다시 typeof키워드로 타입으로 만들면 
5. type Direction =  `1 | 2 | 3 | 4` ( 유니온타입 생성)



<br>

# Union(|)과 Intersection(&)

## 간단하게 타입을 선언하고싶다면? Type Aliasing
- type키워드로 선언한 타입은 변수처럼 할당할 수 있다.
```ts
type A = { a :string };
const a: A = { a : 'hello' };
// const a:{ a: string } = { a : 'hello' };
```

## 상속(extends),구현(implement)와 같은 객체지향 프로그래밍을 하고싶다면? Interface
- interface는 class처럼 {}중괄호로 선언한다.
- interface는 타입과 마찬가지로 `객체의 타입의 이름을 지정`하는 또 다른 방법이다.
```ts 
interface B { a : string };
const b: B = { a: 'hello' };
```

## Union(|)타입 
- A 또는 B 의 의미
- 타입범위를 넓혀준다 


## Intersection(&) 
- 타입의 교집합타입을 만들어준다.
// 모든 속성이 다 있어야 함.
`type A = stirng & number` 
///string,number를 모두 만족하는 타입은 존재할 수 없으므로 never타입  

- 객체의 경우 속성을 합해준다
`type B = { hello: 'world; } & { name : 'jake' }`
const b: B = { hello: 'world', name: 'jake' };
> Cf) { hello: 'world; } & { name : 'jake' } 와
{ hello: 'world; } | { name : 'jake' }는 다르다! 
Intersection타입의경우 두 객체중 하나만 존재해도 만족이지만
Union타입의 경우 두 객체를 합친 타입이어야지만 만족한다.



<br>


# Type Alias와 Interface의 extends(상속)

- type은 &로 합쳐진다.
- interface는 extends상속으로 합쳐진다.

```ts
type Animal = { breath : true };
type Mammal = Animal & { breed: true};
type Human = Mammal & { think: true };

interface A {
  breath: true,
}

interface B extends A {
  breed: true
} ///... A를 상속해서 B는 breath,breed 모든 속성을 갖는다.
const typeB: B = { breath: true, breed: true};
const jake: Human = { breath: true, breed:true, think: true}
```

## interface는 선언시마다 합쳐진다(오버라이딩)
- 이러한 특성으로 라이브러리의 타입은 interface로 작성된다.(확장성)
```ts
interface newA {
  talk: () => void;
}
interface newA {
  eat: () => void;
}
interface newA {
  shit: () => void;
}
const newA: newA = {
 talk(){}, 
 eat(){},
 shit(){},
}
```

<br>


# 타입을 집합으로 생각하기
- 객체는 상세할 수록 좁은 타입이다.
```ts
type A = { name: string };
type B = { age: number };

type newAB = A | B ;
// type newAB는 A와B보다 넓은 타입
type C = A & B ; 
// intersection(&)은 객체type을 합쳐준다 -> A,B타입의 속성을 모두 만족하는 객체타입 생성
// type C는 A와,B보다 좁은 타입

const newC : C = { name : 'jake', age: 29};
// name,age키를 포함한 C는 더 상세하므로 좁은 타입
// 좁은타입은 넓은타입에 포함될 수 있다.
const ab: newAB = { name : 'jake' };
// const abc: C = { name: 'jake'};  (불가)
```

- 객체 리터럴을 할당시(좁은타입 객체를 넓은타입 변수로)
타입 포함 검사뿐 아니라 `잉여속성검사`까지 진행하므로 에러가 뜰 수 있다. 
- (객체 리터럴은 알려진 속성만 지정할 수 있음)
```ts
type C = { name: string } & { age: number };
const c: C = { name: string , age: number, married: false}
/// C타입에 존재하지 않는 married: false가 잉여속성 검사로인해 에러 발생.
```

<br>


# void 사용법

## 함수선언문의 void 
- 리턴타입이 `void`면 리턴값이 존재하면안된다!
- (null: X , undefined: O)

## interface로 선언한 메서드의 () => `void`
- 존재하지 않음이 아니라 사용하지 않는다의 의미
```ts
interface A {
   talk: () => void;
}
const a: A ={
   talk() { return 3; } ///에러가나지 않음, return을 무시 
   // 리턴값이 void이므로 존재하더라도 return값을 무시해서 사용하지 않는다.
}
```

## 콜백함수의 () => `void`
- 존재하지 않음이 아니라 사용하지 않는다의 의미(리턴값이 있어도 봐줌)
- 호출부에 넣는 콜백함수의 리턴값이 존재해도 에러 발생하지 않음.
```ts
declare function forEach(arr: number[], callback: (el: number)=> void ): void;
// 함수 구현부 없이 타입만 작성하고 싶을 때(declare), forEach같은 내장메서드에 적용
// forEach는 리턴값이 없이 배열요소에 특정작업(콜백)을 순회하며 반복하기만 하므로 void 리턴

let target: number[] = [];
forEach([1,2,3], el => { target.push(el)});
forEach([1,2,3], el => target.push(el));
```

<br>

## declare 키워드
> 이는 TypeScript로 작성하지 않은 코드의 타입 정보를 컴파일러에게 알려주는 선언이다.  
대게 외부 사용자에게 내가 만든 라이브러리의 타입 정보를 알려줄 목적으로 d.ts 파일을 정의할 때 Ambient Declaration을 이용한다.  
Ambient Declaration을 작성할 때 declare 키워드를 사용한다.
- 다른 ts파일에서 선언된 타입임을 보장하는 키워드
- 선언하지 않은 함수를 사용하기위해 붙여준다.
> 원래 `function 변수명(파라미터타입):리턴타입`형태로 타입만 선언해두면
바로 밑에 함수선언을 해줘야 에러가 나지 않음.
하지만 declare키워드를 쓰면 타입선언만 해도 에러를 방지.

<br>

### 타입스크립트 선언 파일
> 타입스크립트 선언 파일 `d.ts`는 타입스크립트 코드의 타입 추론을 돕는 파일입니다.   
예를 들어, 전역 변수로 선언한 변수를 특정 파일에서 import 구문 없이 사용하는 경우 해당 변수를 인식하지 못합니다.    
그럴 때 아래와 같이 해당 변수를 선언해서 에러가 나지 않게 할 수 있습니다.

  `declare const global = 'sth';`
// declare로 선언한 변수는 해당 파일에서 전역변수 처럼 인식한다 .

<br>

### 전역 변수와 전역 함수에 대한 타입 선언
- 해당 타입스크립트 파일에서 사용할 순 있지만 선언되어 있지 않은 전역 변수나 전역 함수는 아래와 같이 타입을 선언할 수 있다.

// **전역 변수**  

`declare const pi = 3.14;`

// __전역 함수__
```ts
declare namespace myLib {
  function greet(person: string): string;
  let name: string;
}
myLib.greet('캡틴');
myLib.name = '타노스';
```
TIP
> Use declare namespace to describe types or values accessed by dotted notation.


<br>
<br>


# unknown vs any 
##  unknown 
- 타입을 당장을 모르니 나중에 정의하겠다는 의미 
```ts
try{
 
} catch(error) { ///ts는 error를 unknown으로 기본 처리한다.
  (error as AxiosError)
```
- try...catch문에서 error는 무엇이 발생할지 모르므로 unknown처리후 
- 사용자가 에러를 정의해준다.

## any
- 타입정의를 포기 



<br>


# 타입가드(타입 narrowing)
## 원시형타입의 union타입 타입가드
- 함수의 파라미터 타입이 Union일 경우 narrowing필요
- typeof연산자를 이용해 내로잉

```ts
function numOrStr(a: number | string) {
  if (typeof a === "number") {
    a.toFixed(1);
  } else {
    a.charAt(3);
  }
  if(typeof a === 'string'){
    a.charAt(3);
  }
  if(typeof a ==='boolean'){
    a.toString();
  }
}
numOrStr("123");
numOrStr(1);
```

## 배열타입의 타입가드
- 배열타입은 `Array.isArray(확인할배열값)`으로 타입가드작성
```ts
function numOrNumArray(a: number | number[]){
  if(Array.isArray(a)){ //number[] }
  a.concat(4)
} else {  //number 
  a.toFixed(3);
}}
```

## class의 타입가드
- 클래스 ( {} 내부 상태를 복사한 객체 생성해주는 생성자함수
new class명() 으로 인스턴스 생성) 
// static 키워드에 따라 복사하지않는 상황도 있음
클래스명은 그 자체로 `인스턴스의` 타입이된다.
//new className()으로 만들어준 객체 : 인스턴스

```ts 
class A {
  aaa()
}
class B {
  bbb()
}

function aOr B(param: A | B){ 
///...param은 A or B의 인스턴스이므로 클래스를 타입으로 가질 수 있음
	if( param instanceof A){ ///... param이 A의 인스턴스일 때
      param.aaa(); ///A메서드 호출가능.
  }
}
aOrB(new A());
aOrB(new B());
```

## 객체의 타입가드
- 객체 안 `속성의 값`으로도 타입을 체크
```ts
type B = { type: 'b', bbb: string};
type C = { type: 'c', ccc: string};
type D = [ type: 'd', ddd: string};
function typeCheck( a: B | C | D){
if(a.type === 'b'){
	a.bbb;
} else if (a.type === 'c'){
     a.ccc;
 } else {
	a.ddd;
}
```

## `in` 연산자: 객체속성명으로 타입 구분하기
```ts
type B = { type: 'b', bbb: string};
type C = { type: 'c', ccc: string};
type D = [ type: 'd', ddd: string};
function typeCheck( a: B | C | D){
if( bbb in a ){ /// bbb속성이 a파라미터에 있다면? => B로추론
	a.type;
} else if (a.type === 'c'){
     a.ccc;
 } else {
	a.ddd;
}

# 커스텀 타입 가드
- 타입을 구분해주는 `커스텀 타입가드 함수`를 작성 가능. 
```ts
function catOrDog(a: Cat | Dog): a is Dog{
  if((a as Cat).meow { return false } 
  return true;
}
```
- return값에 is가들어가 있다면 커스텀타입가드함수다.
- 커스텀타입가드 함수는 if문안에서 사용하며,ts에 정확한 타입을 알려준다.
- `typeof`,`instanceof`,`in`,`Array.isArray`로 구분하기 힘든 경우 사용
```ts
function pet(a: Cat | Dog){
  if(catOrDog(a)){
 console.log(a.bow);
  }
  if('meow' in a) {
  console.log(a.meow);
  }
}

# ts 4.8ver 업데이트 내용
```ts
const x: {} = 'hello';
const y: Object = 'hi'; //대문자Object
// {}, Object는 모든 타입 할당 가능을 의미(null,undefined제외)

const yy: object = { hello:'world' };
//하지만 object는 지양, interface나type,class 사용 권장 
const z: unknown = 'hi'  
//unknown: {}(모든타입)| null | undefined의 합성
```

# readonly,indexed signature, mapped types
## readonly속성
- 타입에 readonly속성 부여해서 재할당 불가 만들기
```ts
interface A {
  readonly a: string;
  b: string
}
const aaaa: A = { a:'hello', b: 'world'};
aaaa.a = '123'; ///readonly로 인해 재할당 불가
```


## Indexed Signature

```ts
 type A = { a: string , b: string ,c: string ,d : string}; //너무 길다..
 
 //위,아래는 동치 

 type indexedA = { [key: string]: string}
 // 객체의 어떤key값이던 전부 string이고 그 값도 string이다.를 의미한다. 
 // 모든 key값과 value값의 타입형태를 대략적으로 지정한다.

```


## Mapped Types
```ts 
 type BB = 'Human' | 'Mammal' | 'Animal';
 type newB = { [key in BB]: number } 
 // key값은 BB타입의 key값중에서 ,값은 number로 ..
const newB = { Human: 123, Mammal: 5, Animal: 7}
type newC = { [key in BB]: B };
```
- cf.) interface로는 | (또는)문법을 사용할 수 없다.
// Interface문법은 유니온타입이 불가(대신 여러번 선언을 통한 오버라이딩 또는 extends상속으로 확장가능함.)




<br>



# 타입스크립트와 Class
## 객체지향문법
- 객체지향문법을 위한 private,proteted,static 등의 키워드 사용가능
- constructor에 선언한 변수는 생성자메서드 이전에 타입 초기화가 필요하다.

```ts
class A {
  a: string;
  b: number;  //인스턴스의 프로퍼티가 되는 값은 타입초기화 필요 
  constructor(a: string, b:number =123){ // 123: default value
	this.a = a;
	this.b = b;
  }
```
- 그렇지않으면 타입 및 변수 초기화를 같이 진행해줘야함.
- new class명()를 통한 속성의 동적인 생성을 원하지 않는다면 constructor 생략 가능.
- //cf.클래스는 객체 복사기
```ts
class A {
  a: string = '123'
  b: number = 123 ///... 인스턴스에 복제된다.
  }
```
- `priviate`키워드가 붙은 메서드/프로퍼티는 클래스명을 통한 속성의 참조/호출 가능, 인스턴스로 참조/호출 불가능 함.
- `protected`는 extends한 클래스에서도 부모클래스 속성의 참조/호출을 가능하게해줌
-  `static`키워드는 특정 속성을 자식요소에 물려줄지 말지를 결정한다.
```ts
class newA2{
  public method(){ const aaaa = 'bb'}
  public props = 123;
}

const insA2 = new newA2();
insA2.method()
insA2.props
```

## implements, abstract
`class B implements A(인터페이스)` //B는 A를 구현한다(모양을 따른다).
- 클래스B는 인터페이스A를 따라야한다.(내부 속성의 타입을 따라야함.)
- 클래스의 모양을 interface로 통제할 수있다.


>			public		protected 	private
클래스내부사용       O 		   O			    O
인스턴스사용			O			X			X
상속클래스사용 		O			O			X

```ts

abstract class DB { ///... 클래스의 모양만 구현해놓는 abtract 추상클래스
  private readonly a: string = '123';
  b: string = 'world';
  c: string = 'wow';
  
  abstract method(): void; ///...반드시 구현해줘야하는 method(abstract)
  method2(){
  return '3';
  }
}
class DBD extends DB {
  method(){}; ///...abtract가 붙은 method는 필수로 구현
}
```
- abtract 추상클래스는 클래스의 모양만 구현해놓을 수 있다.(extends해서 사용)
- abstract method는 반드시 구현부에서 구현해줘야함.

<br>

# Optional ,Generic
## optional 연산자 :`?`
- 함수 파라미터나,객체 속성값이 옵션인 경우 붙여준다.

```ts
function abc( a: number, b?: number, c?:number){}
abc(1)
abc(1,2)
abc(1,2,3)

let obj: { a: string, b?:string } = { a: 'hello', b:'world', c:'wow' }
obj = { a: 'hello' }
```
## Generic
- 호출시 타입이 결정되는 문법(선언시 결정되지않음)
- 타입을 변수처럼 만드는 것
- 제네릭은 `변수명 뒤 < >`로 표현한다.
```ts
function add<T>(x: T, y:T):T {
  return x + y;
} 
add(1,2) /// T: number로 확정
add('1','2') ///T: string으로 확정
```

## 제네릭에서 extends는 왼쪽을 오른쪽으로 '제한'한다는 의미 (=왼쪽이 오른쪽에 할당가능하다는 의미)
> 제네릭은 사용하는 시점에 타입을 결정해줌으로써 사실상 아무 타입이나 집어넣어도 상관 없다.  
이렇게 입력값에 대한 유연성을 확보했지만 각 함수에 대해 사용처에 따라서 입력값을 제한 할 필요가 생긴다. 
//cf.) 클래스에서 extends가 왼쪽(자식)클래스가 오른쪽(부모)클래스 상속한 확장클래스라는 것과 대비된다


- 제네릭의 extends는 `왼쪽을 오른쪽으로 제한`의 의미를 갖는다.
`function add<T extends number>()`
//제네릭함수 호출시 넣을 수 있는 T값의 타입은 number로 제한된다.
//만약 <T extends K> 형태의 제네릭이 있다면, T가 K에 할당 가능해야 한다 라고 정의하면 된다.

## 여러 제네릭을 각각 제한할 수 있다.
`<T extends number, K extends string>`
- 제네릭의 타입을 오른쪽으로 제한

## 제네릭 제한의 여러 형태
>
1. 콜백함수의 형태를 제한 
 `function add<T extends (a:string) => number>`
  add(a => +a)

2. T를 (모든)함수로 제한 
 `add<T extends (...args:any => any)>  `
  add(Fn)
3. T를 생성자(클래스,생성자함수,컨스트럭터메서드)로 제한
 `add<T extends abstract new (...args: any)=> any>`
  add(A)

### 제네릭문법의 위치
>꺾쇠 <> 기호를 변수명, 함수명 `앞에다 쓰면 '타입 단언'` 이 되게 된다.    
따라서 `제네릭을 구현하려면 변수명, 함수명 뒤에다가`꺾쇠 괄호를 적어주어야 한다.  
그리고 제네릭명은 꼭 T가 아니어도 된다.  
내마음대로 붙일수는 있지만 관습적으로 대문자 알파벳 한글자로 처리하는 편이다. (T, U, K ...)  

<br>

// __인수들을 받아서 배열로 만들어주는 메소드__
```ts
function toArray<T>(a: T, b: T): T[] {
   return [a, b];
}
```

// __만약 화살표 함수로 제네릭을 표현한다면 다음과 같이 된다.__
```ts
const toArray2 = <T>(a: T, b: T): T[] => { ... }
```
- 변수명 뒤에 꺾쇠를 적어준다, 다만 함수표현식형태이므로 부등호를 넘어서 파라미터 앞에 `<T>`를 적어준다.

<br>

### 제네릭 주의사항
- 인수를 배열로 받을 경우 `T[],  Array[T]`와 같은 형태로 처리한다.
//  T[]와 Array<T>는 같다.
```ts
function loggingIdentity<T>(arg: T[]): T[] {
   console.log(arg.length); // 배열은 .length를 가지고 있다. 따라서 오류는 없다.
   return arg; // 배열을 반환
}

function loggingIdentity2<T>(arg: Array<T>): Array<T> {
   console.log(arg.length);
   return arg;
}

loggingIdentity([1, 2, 3]);
loggingIdentity2([1, 2, 3]);
```

<br>

### 제네릭과 interface & type
- __inteface with Generic__
```ts
interface Mobile<T> { 
   name: string;
   price: number;
   option: T; // 제네릭 타입 - option 속성에는 다양한 데이터 자료가 들어온다고 가정
}

// 제네릭 자체에 리터럴 객테 타입도 할당 할 수 있다.
const m1: Mobile<{ color: string; coupon: boolean }> = {
   name: 's21',
   price: 1000,
   option: { color: 'read', coupon: false }, // 제네릭 타입의 의해서 option 속성이 유연하게 타입이 할당됨
};

const m2: Mobile<string> = {
   name: 's20',
   price: 900,
   option: 'good', // 제네릭 타입의 의해서 option 속성이 유연하게 타입이 할당됨
};
```

<br>

- __type alias with Generic__
```ts
type TG<T> = T[] | T;

const number_arr: TG<number> = [1, 2, 3, 4, 5];
const number_arr2: TG<number> = 12345;

const string_arr: TG<string> = ['1', '2', '3', '4', '5'];
const string_arr2: TG<string> = '12345';
```

<br>
<br>

### 제네릭 함수타입 
- __인터페이스로 함수 타입을 지정__
```ts
interface Add {
   (x: number, y: number): number;
}

let myFunc: Add = (x, y) => {
   return x + y;
};
```

- __다른 방식으로 아예 함수를 할당할때 제네릭을 결정하는 방식__
```ts
interface GenericIdentityFn<T> {
   (arg: T): T;
}

function identity<T>(arg: T): T {
   return arg;
}

let myIdentity: GenericIdentityFn<number> = identity;
let myIdentity2: GenericIdentityFn<string> = identity;

myIdentity(100);
myIdentity2('100');
```

<br>

### 제네릭 클래스 타입
- 클래스는 그자체로 인스턴스의 타입이 될 수 있다.
```ts
class GenericNumber<T> {
   zeroValue: T;
   add: (x: T, y: T) => T;

   constructor(v: T, cb: (x: T, y: T) => T) {
      this.zeroValue = v;
      this.add = cb;
   }
}

let myGenericNumber = new GenericNumber<number>(0, (x, y) => {
   return x + y;
});

let myGenericString = new GenericNumber<string>('0', (x, y) => {
   return x + y;
});

myGenericNumber.zeroValue; // 0
myGenericNumber.add(1, 2); // 3

myGenericString.zeroValue; // '0'
myGenericString.add('hello ', 'world'); // 'hello world'
```


<br>



# 기본값 타이핑 

## 디폴트파라미터 ( 파라미터변수 = 값 )
```ts
cosnt a = (b: number = 3, c:number =4) => { return '3';}
const a = (b: { children: string} = { children: 'zerocho'}) => {  }
```

- 헷갈린다면 타입자리 생략하고 생각해보기
## 화살표함수 제네릭과 JSX
` const add = <T = unknown>(x: T, y: T) => ({ x, y }) `
- 함수선언식에서는 파라미터 오른쪽에 적지만, 화살표함수는 익명함수이므로 위와같다.
- tsconfig.json의 JSX 설정을 none으로 하지않으면 <>엘리먼트로 인식하여 제네릭에 linting이 된다. ->none설정 필요 
- 제네릭변수 T에 `1.기본값부여 T = unknown`, `2. T extends unknown` `3. <T,> (의도가 불분명해서 비권장)` 로 에러체크를 피한다.




<br>
<br>

### Reference: 
1. [zerocho - 타입스크립트 올인원](https://www.inflearn.com/course/%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%98%AC%EC%9D%B8%EC%9B%90-1)
2. [타입스크립트핸드북](https://joshua1988.github.io/ts/usage/declaration.html#%EC%A0%84%EC%97%AD-%EB%B3%80%EC%88%98%EC%99%80-%EC%A0%84%EC%97%AD-%ED%95%A8%EC%88%98%EC%97%90-%EB%8C%80%ED%95%9C-%ED%83%80%EC%9E%85-%EC%84%A0%EC%96%B8)

3. [Inpa_tisroy](https://inpa.tistory.com/entry/TS-%F0%9F%93%98-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-Generic-%ED%83%80%EC%9E%85-%EC%A0%95%EB%B3%B5%ED%95%98%EA%B8%B0#%EC%A0%9C%EB%84%A4%EB%A6%ADgenerics_%EC%86%8C%EA%B0%9C)
