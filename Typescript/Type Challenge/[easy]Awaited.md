> Q: 만약 Promise 등을 통해 래핑된 타입이 있다면, 그 안에 있는 타입을 어떻게 얻을수 있을까요? 예를 들어 Promise<ExampleType>과 같은 타입이 있다면   
  ExampleType을 어떻게 얻을 수 있을까요?
```ts
[문제]
  type MyAwaited<T> = any
```  
  
 ###  Tips)
 - 조건부타입 
 - 조건부타입 내에서의 타입 추론(infer)
 

```ts
[해답]
  //1 
  type MyAwaited<T> = T extends Promise<infer R> ? R : T
  
  //2  Promise<Promise<string>>과 같은 타입에서 string만 뽑아내는 경우 
  type MyAwaited<T> = T extends Promise<infer R> ? MyAwaited<T> : T

  ```
- Promise< infer R > 형태를 T가 만족하면 R을 추론해서 반환해주시고 아니라면 T를 반환
- infer키워드는 조건부타입의 true 분기에서 사용 가능
- Promise<Promise<string>>과 같은 이중 Promise를 언래핑하기 위해선 재귀적 참조를 활용
- 참고: https://ghaiklor.github.io/type-challenges-solutions/ko/easy-awaited.html
