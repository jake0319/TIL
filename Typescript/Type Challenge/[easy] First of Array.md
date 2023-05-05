# First of Array
## 챌린지
### 배열 T를 받아 배열의 첫번째 원소를 반환해 주는 제네릭 First<T>를 구현해보세요.
  
__예시:__
```ts
type arr1 = ["a", "b", "c"];
type arr2 = [3, 2, 1];

type head1 = First<arr1>; // expected to be 'a'
type head2 = First<arr2>; // expected to be 3
```
  
<br>
    
<br>
    
<br>
    
      
<br>
    
<br>
<br>
  
__해답 및 테스트케이스__
```ts
  
/* _____________ 여기에 코드 입력 _____________ */

type First<T extends any[]> = T extends [] ? never : T[0]
// 1. 컨디셔널타입에서 extends : T가 []에 포함되는가 (즉 T가 []인가?) 
// 2. never (참일때) , T[0] (아니면 첫 번째 원소 반환)
  

/* _____________ 테스트 케이스 _____________ */
import type { Equal, Expect } from '@type-challenges/utils'

type cases = [
  Expect<Equal<First<[3, 2, 1]>, 3>>,
  Expect<Equal<First<[() => 123, { a: string }]>, () => 123>>,
  Expect<Equal<First<[]>, never>>,
  Expect<Equal<First<[undefined]>, undefined>>,
]

  ```
__참조개념__
- [Conditional Types](https://www.typescriptlang.org/docs/handbook/2/conditional-types.html)
- [Indexed Types](https://www.typescriptlang.org/docs/handbook/2/indexed-access-types.html)

<br>

- [문제](https://www.typescriptlang.org/play?ssl=37&ssc=1&pln=22&pc=1#code/PQKgUABBCMAsEFoIDECWAnAzgFwgewDMIBBddAQwE9JEE76aAjSkgO2wAs9WXkBXCAAoAAuXYE+ASggBiQDZDgAbHZ5MlTA0ZmiIAnJwBzd6qIAYewC+jgwDlLgGFXpAAwAq1wD6dEQNg9gEVGIgapmIgBbHAMYOANcYhAAYXAUPHAEXGIQAwewA01wA1VwBSmiEAcCcASRsBazohrNCxsAB5bAD5HCEAbWsAQNdjADkHAFLGAOgMIQAgxwB2hgC4G607sTBpsSgAHAFMIFXRoCABeCABtAHJyWYAaCFnGJZWAY1mAXV6B4dGAJkmZgGZlw+XoXb2hiA5B8gATcansnFzR6AKIYGAIQYADyGG2wgyeEGweAgjGG81mt2GD2exzeGA+Rx+fwBwMGoPBkOhsIgpxonWsDR+AHFUNgABJ8RiABdHADiDEEALz2ABjrWvdsNh+phWn9uhsOLUAFaYWp4dAAc2AcGAAGs8GAQMB1KAIAB9XV6-V6iCAG9HOUyIIAVecAOy0QEKAUg6dQbHdqIOr1H07u88rYcWDWE9MCMeNNtj8pt6gb7-TNthAAPwQViDABug3QEB5tmmAAZdtjABdzgBRWwAqg4AERtCrJ9gz9AYzgAAa6NmwA3y4ALVcAGC3xeuCQCRkxBbPXg4APccALuOAFy7pGBsYmU2nBIAOGcAPuOAGVbpMtMzmhK5ABNNI88EEAET2AD8mfL4otFJBOQA6nfqIIAZRcAJUOADqWIIAAeaH96v191Lo1qAAtv0Mq4O6wwAN4QAAogAjnw5AADbLBBuKghAAC+EAEOgeB-iswggQgorwXBVayoMmDAHw2CoHBmAImAIEQBs5CYGRJzTDQSEgnk0GwXBuSerk0znBAlwwCGyynAUBSLBxyHcTB8H8eieTTII0gTD80CHMJ4HkDyODoKgrCymh4lCOpmnaVJMlQJxeLybxSk5IJZlTqm1myVxuQ8YpAnTHwfqDAQRngmZAVPEFIVPB5NwMamWFYGxNDYnhmAIECXHpWQMo0AJsysHg2CkBQlCzNJyX-Kl6VyVlCW5cpuTgVmPLzKolAADKoEqgyzGh5U3FqX43oAJ02ADLjECABdNgAjNZ+X4-heNA-IAqBOAK9NECAK81gAE47E3K8vygrCpgooSlKMryoqYiYAA7qmyqqlAy1rVyPIcHyApCsAIpipK0pygqsDAJgeBwZRqDcD0D0QMNgAenRAuhJIAJy0vW9B2fUd32nX9KpqhqQA)
- [해답](https://ghaiklor.github.io/type-challenges-solutions/ko/easy-first.html)
  
  
  
