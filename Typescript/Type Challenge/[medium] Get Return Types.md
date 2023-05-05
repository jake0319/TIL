# Get Return Type
## 챌린지
## ReturnType<T> 유틸리티 타입을 사용하지 않고 직접 구현해보세요.

__예시:__
```ts
const fn = (v: boolean) => {
  if (v) return 1;
  else return 2;
};

type a = MyReturnType<typeof fn>; // should be "1 | 2"
```
  
<details>
<summary> 솔루션: (여기를 눌러주세요)</summary>
<div markdown="1">       

```ts

/* _____________ 여기에 코드 입력 _____________ */
type MyReturnType<T extends Function> =
  T extends (...args: any[]) => infer R
    ? R
    : never
// type MyReturnType<T extends ((...args:any[])=>{})> = T extends ((...args:any[])=>infer R) ? R :never
// 1. T가 함수타입의 제네릭임을 명시 (제네릭 input을 제한하는것이므로 해도되고 안해도됨)
// 2. infer키워드로 리턴값을 추론해서 true분기에서 반환
// 3. T가 함수타입이 아니면 never(타입을 반환하지않음)

/* _____________ 테스트 케이스 _____________ */
import type { Equal, Expect } from '@type-challenges/utils'

type cases = [
  Expect<Equal<string, MyReturnType<() => string>>>,
  Expect<Equal<123, MyReturnType<() => 123>>>,
  Expect<Equal<ComplexObject, MyReturnType<() => ComplexObject>>>,
  Expect<Equal<Promise<boolean>, MyReturnType<() => Promise<boolean>>>>,
  Expect<Equal<() => 'foo', MyReturnType<() => () => 'foo'>>>,
  Expect<Equal<1 | 2, MyReturnType<typeof fn>>>,
  Expect<Equal<1 | 2, MyReturnType<typeof fn1>>>,
]

type ComplexObject = {
  a: [12, 'foo']
  bar: 'hello'
  prev(): number
}

const fn = (v: boolean) => v ? 1 : 2
const fn1 = (v: boolean, w: any) => v ? 1 : 2
```
1. [조건부 타입에서 타입 추론](https://www.typescriptlang.org/docs/handbook/2/conditional-types.html#inferring-within-conditional-types
)을 사용하는 대부분의 경우는 사용될 타입이 무엇인지 확실하지 않을 때입니다. 함수의 반환 타입이 무엇인지 모르지만 그것을 알아내야 합니다.  
  - 조건부타입에서 infer키워드는 제네릭에 들어오는 타입으로부터 값을 추론해냅니다.
2. () => void와 같은 타입을 갖는 함수를 예로 들겠습니다. void 자리에 무엇이 들 어갈지 정확히 알지 못합니다. 이 경우 그 자리를 infer R로 대체해주겠습니다. 이 것이 풀이의 첫 단계입니다:  
  `type MyReturnType<T> = T extends () => infer R ? R : T;`
3. 주어지는 타입 T가 함수라면 반환 타입을 추론하고 아닌 경우 T를 그대로 반환합 니다. 어렵지 않습니다.  
  함수에 매개변수가 함께 전달될 경우엔 풀이가 제대로 동작하지 않습니다. () => infer R 타입에 할당할 수 없기 때문입니다.
4. 어떤 매개변수도 받을 수 있고 그것에 상관하지 않을 것임을 보여주기 위해 타입의 인 자에 ...args: any[]를 추가합니다:
5. `type MyReturnType<T> = T extends (...args: any[]) => infer R ? R : T;`
- [솔루션참고](https://ghaiklor.github.io/type-challenges-solutions/ko/medium-return-type.html)
  
</div>
</details>
