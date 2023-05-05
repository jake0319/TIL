# If
## 챌린지
### 조건 C를 받아서 참일 경우에는 T를 반환하고 거짓일 경우에는 F를 반환하는 유 틸리티 타입 If를 구현해보세요. C는 true 또는 false일 것이 기대되고, T와 F는 어떤 타입이든 괜찮습니다.

__예시:__

```ts
type A = If<true, "a", "b">; // expected to be 'a'
type B = If<false, "a", "b">; // expected to be 'b'
```

<br>
<br>
<br>
<br>
<br>
<br><br>
<br>

__해답:__
```ts

/* _____________ 여기에 코드 입력 _____________ */

type If<C extends boolean, T, F> = C extends true ? T:F
// 제네릭에 C extends boolean을 추가하지 않는다면 에러케이스의 null이 할당되므로 추가해주어야한다.
// 조건부타입의 C extends true는 C가 true인경우를 나타낸다.

/* _____________ 테스트 케이스 _____________ */
import type { Equal, Expect } from '@type-challenges/utils'

type cases = [
  Expect<Equal<If<true, 'a', 'b'>, 'a'>>,
  Expect<Equal<If<false, 'a', 2>, 2>>,
]

// @ts-expect-error
type error = If<null, 'a', 'b'>
```

1. Typescript에서` 조건부 타입을 써야하는 확실한 경우는 타입 내에서 “if”문을 사용할 때입니다`. 이번 풀이에 사용할 방법이기도 합니다.
2. 만약 주어진 조건이 true로 평가된다면 “true” 분기를 타야하고 그렇지 않을 경우에 는 “false” 분기를 타야합니다.
3. `type If<C, T, F> = C extends true ? T : F;`
4. 여기까지만 진행했다면 컴파일 에러가 생길 것입니다. C의 타입에 대한 제약없이 불 린 타입에 할당하려고 했기 때문입니다. 타입인자 C에 extends boolean을 추가해 주어 이 문제를 해결할 수 있습니다:
5. `type If<C extends boolean, T, F> = C extends true ? T : F;`


