# Omit
## 챌린지
### 내장 타입인 Omit<T, K> 제네릭을 직접 구현해보세요. T에 속한 모든 프로퍼티 중 에서 K에 해당하는 프로퍼티를 지우는 타입을 만들어야 합니다.

__예시:__
```ts
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}

type TodoPreview = MyOmit<Todo, "description" | "title">;

const todo: TodoPreview = {
  completed: false,
};
```

<br>

<br>

<br>

<br>

__솔루션__
```ts

/* _____________ 여기에 코드 입력 _____________ */

type MyOmit<T, K> = { [P in keyof T as P extends K ? never : P]: T[P] }
//1. 문제:  T 프로퍼티에서 K프로퍼티만 제거한 새로운 Omit타입을 다시 리턴해야함
//2. as clause를 통한 Mapped Type에서 Key 리매핑 (as는 T를 새로운 타입으로 리매핑시킨다.)
//3. P in keyof T as P extends K ? never :P   //  P는 T의 키타입이고, P중에서 K에 포함되지 않는 타입만 리맵핑해서 T로 제공 
//4. 결국 P타입은 T-K(여집합)타입인 새로운 Remapped T의 키로 매핑된다. 

/* _____________ 테스트 케이스 _____________ */
import type { Equal, Expect } from '@type-challenges/utils'

type cases = [
  Expect<Equal<Expected1, MyOmit<Todo, 'description'>>>,
  Expect<Equal<Expected2, MyOmit<Todo, 'description' | 'completed'>>>,
]

// @ts-expect-error
type error = MyOmit<Todo, 'description' | 'invalid'>

interface Todo {
  title: string
  description: string
  completed: boolean
}

interface Expected1 {
  title: string
  completed: boolean
}

interface Expected2 {
  title: string
}
```

<br>

### 참고 
- [Key Remapping in Mapped Types](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-1.html#key-remapping-in-mapped-types)
- [솔루션참고](https://ghaiklor.github.io/type-challenges-solutions/ko/medium-omit.html)
- Mapped Types
- Indexed Types
- Conditional Types
