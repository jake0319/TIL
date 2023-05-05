# Push
## 챌린지
### Array.push를 타입 시스템 내에서 구현해보세요.

__예시:__
```ts
type Result = Push<[1, 2], "3">; // [1, 2, '3']
```

<details>
<summary>해답</summary>
<div markdown="1">       
  
```ts
/* _____________ 여기에 코드 입력(해답) _____________ */

type Push<T extends any[], U> = [...T,U]
// T가 튜플타입(array type)임을 안다면 rest parameter의 사용이 가능함.

/* _____________ 테스트 케이스 _____________ */
import type { Equal, Expect } from '@type-challenges/utils'

type cases = [
  Expect<Equal<Push<[], 1>, [1]>>,
  Expect<Equal<Push<[1, 2], '3'>, [1, 2, '3']>>,
  Expect<Equal<Push<['1', 2, '3'], boolean>, ['1', 2, '3', boolean]>>,
]
```

</div>
</details>
