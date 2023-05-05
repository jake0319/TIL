# Parameters
## 챌린지
### MyParameters<T>를 빌트인 제네릭 Parameters<T>를 사용하지 않고 구현해보세요.



<details>
<summary> 해답 </summary>
<div markdown="1">       

```ts
  
/* _____________ 여기에 코드 입력 _____________ */

type MyParameters<T extends (...args: any[]) => any> = T extends ((...args:infer R) => any) ? R: never 
// const foo는 typeof연산자를 통해 type을 추론해낸다.
// (...args: any[])=>any는 함수타입을 의미한다.(제네릭이 함수를 extends하는 경우 사용)
// 위는 args가 any타입의 튜플임을 의미하고 ,rest문법을 적용한 파라미터임을 의미한다.
// infer키워드는 `추론`의 기능이다. T가 함수타입을 extends함으로부터 R이 any[](튜플)임을 추론


/* _____________ 테스트 케이스 _____________ */
import type { Equal, Expect } from '@type-challenges/utils'

const foo = (arg1: string, arg2: number): void => {}
const bar = (arg1: boolean, arg2: { a: 'A' }): void => {}
const baz = (): void => {}

type cases = [
  Expect<Equal<MyParameters<typeof foo>, [string, number]>>,
  Expect<Equal<MyParameters<typeof bar>, [boolean, { a: 'A' }]>>,
  Expect<Equal<MyParameters<typeof baz>, []>>,
]
```
1. 이 문제를 해결하려면 함수로부터 정보의 일부를 가져올 수 있어야 합니다. 조금 더 자세히 말하자면 함수의 매개변수들을 가져와야 합니다.  
2. `“아직 모르는 타입을 알아내는”` 적절한 방법은 무엇일까요? 바로 `infer 키워드` 입니다. 바로 `infer을 사용하기 전에 먼저 함수와 일치하는 간단한 조건부 타입에 대해서 보겠습니다.`
3. ` type MyParameters<T> = T extends (...args: any[]) => any ? never : never;`  
4. type T가 매개변수와 리턴타입이 있는 함수와 일치하는지 확인합니다. 여기서` any[]를 infer로 `대체할 수 있습니다.  
5. `type MyParameters<T extends (...args: any[]) => any> = T extends (...args : infer U) => any ? U : never;`
 - 제네릭T가 함수타입으로 제한된다면, 조건부타입에서 infer키워드로 타입을 추론해낼 수 있다.
6.  만약 T에 할당되는 매개변수가 [number,string] 일 경우, 타입스크립트 컴파일러는 함수의 매개변수 목록을 추론해서 U에 [number,string] 을 할당해줍니다
  
</div>
</details>
  
- [해설링크](https://ghaiklor.github.io/type-challenges-solutions/ko/easy-parameters.html)
