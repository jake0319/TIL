# Concat
__JavaScript의 Array.concat 함수를 타입 시스템에서 구현하세요. 타입은 두 인수를 받고, 인수를 왼쪽부터 concat한 새로운 배열을 반환해야 합니다.__

예시:
```ts
type Result = Concat<[1], [2]> // expected to be [1, 2]
```

해답:
```ts

/* _____________ Your Code Here _____________ */

type Concat<T extends unknown[], U extends unknown[]> = [...T, ...U];

```
- T,U가 `unknown[]`을 extends하지 않는다면 rest문법에서 T,U가 array임이 보장이 되지 않아 오류가 발생합니다.
  - 따라서 제네릭타입에서 unknown[]를 extends해야합니다.
