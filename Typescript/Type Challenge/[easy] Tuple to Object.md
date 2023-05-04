

### Q: 배열이 주어질 때, 배열에 있는 원소들을 키/값으로 가지는 객체로 변환해보세요.

```ts
문제 예시:
const tuple = ["tesla", "model 3", "model X", "model Y"] as const;
// expected { tesla: 'tesla', 'model 3': 'model 3', 'model X': 'model X', 'model Y': 'model Y'}
const result: TupleToObject<typeof tuple>;
```

<br>

### 해답
>배열에 있는 모든 값들을 얻어 새 객체의 키와 값으로 만들어야합니다.  
이 경우 인덱스 타입을 이용하면 쉽습니다. 배열에 있는 값들은 T[number]로 얻을수 있습니다.  
mapped type을 사용하면, T[number]로 얻은 값들을 순회하며 기존의원소 를 키와 값으로 하는 새로운 타입을 만들 수 있습니다:

```ts
/* _____________ Your Code Here _____________ */

type TupleToObject<T extends readonly any[]> = { [K in T[number]] : K }
// 인덱스타입: 배열리터럴의 값들을 뽑아내기 위해 사용함 (배열은 숫자인덱스를 key값으로 갖는 일종의 객체라고 생각)
// typeof연산자와 함께 인덱스타입을 사용하면 객체를 element로 갖는 배열의 요소 타입을 뽑아낼 수 있다.
// 맵드타입: [K in 인덱스드타입] : K

/* _____________ Test Cases _____________ */
import type { Equal, Expect } from '@type-challenges/utils'

const tuple = ['tesla', 'model 3', 'model X', 'model Y'] as const
const tupleNumber = [1, 2, 3, 4] as const
const tupleMix = [1, '2', 3, '4'] as const

type cases = [
  Expect<Equal<TupleToObject<typeof tuple>, { tesla: 'tesla'; 'model 3': 'model 3'; 'model X': 'model X'; 'model Y': 'model Y' }>>,
  Expect<Equal<TupleToObject<typeof tupleNumber>, { 1: 1; 2: 2; 3: 3; 4: 4 }>>,
  Expect<Equal<TupleToObject<typeof tupleMix>, { 1: 1; '2': '2'; 3: 3; '4': '4' }>>,
]
```


### 추가개념: as const , 인덱스타입과 typeof 연산자
- `as const` : 객체리터럴,배열 등에 as const를 붙이면 readonly가 붙은 타입화 기능을 제공한다. (타입이 아닌 요소를 readonly타입으로 만들어주는 키워드)
- Indexed type: Array[number]는 배열의 인덱스 타입으로 배열의 모든 요소를 뽑아낸다.(타입으로 뽑아내진 않으므로 타입으로 뽑고싶다면 typeof키워드 필요함)


<br>

### 참고 
- [Indexed types](https://www.typescriptlang.org/docs/handbook/2/indexed-access-types.html)
- [Mapped types](https://www.typescriptlang.org/docs/handbook/2/mapped-types.html)
- [해설](https://ghaiklor.github.io/type-challenges-solutions/ko/easy-tuple-to-object.html)
