> Q:내장 타입인 Readonly<T> 제네릭을 직접 구현해보세요.  
  T의 모든 프로퍼티가 readonly로 설정된 타입을 만들어야 합니다. 이는 생성된 타 입의 프로퍼티들이 재할당 될 수 없음을 의미합니다.

  <br>
  
```ts


/* _____________ Your Code Here _____________ */

type MyReadonly<T> = { readonly [K in keyof T] : T[K]}
// keyof T는 T의 키를 유니온타입으로 제공 
// [K in keyof T]는 Mapped Types 
// T[K]는 K에 Mapping되는 value타입을 반환
// readonly키워드만 붙여주면 됨

/* _____________ Test Cases _____________ */
import type { Equal, Expect } from '@type-challenges/utils'

type cases = [
  Expect<Equal<MyReadonly<Todo1>, Readonly<Todo1>>>,
]

interface Todo1 {
  title: string
  description: string
  completed: boolean
  meta: {
    author: string
  }
}

```
- 내장 유틸리티타입인 `Readonly<T>` (타입의 모든 키에 readonly키워드를 붙여주는 기능) 을 구현합니다 .
