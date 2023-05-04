# Exclude
__T에서 U에 할당할 수 있는 타입을 제외하는 내장 제네릭 Exclude<T, U>를 이를 사용하지 않고 구현하세요.__

예시:
`type Result = MyExclude<'a' | 'b' | 'c', 'a'>    // 'b' | 'c'`
-  유틸타입 Exclude<T,U>의 구현 문제 (T에서 U를 제외한 타입의 반환)

<br>

```ts
/* _____________ Your Code Here _____________ */

type MyExclude<T, U> = T extends U ? never : T 

```

<br>

1. 분산조건부타입 기능에 의해 유니온타입인T의 멤버는 조건부타입을 순회한다.( 제네릭에 유니온타입이 할당될 경우 분산적으로 동작한다.) 
2. 순회하며 리턴된 결과는 다시 Union 타입으로 합쳐진다.
3. 조건부타입의 extends키워드는 T가 U에 포함되는가? 를 의미한다.
4. 유니온타입T의 멤버가 U에 포함되면 never, 포함되지않으면 해당 멤버를 리턴 후 합쳐줌.
5. 결과적으로 유틸리티타입 Exclude<T,U>이 구현된다.

<br>

- 해답: https://ghaiklor.github.io/type-challenges-solutions/ko/easy-exclude.html
