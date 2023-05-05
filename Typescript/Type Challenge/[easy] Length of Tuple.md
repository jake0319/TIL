# Length of Tuple
## Challenge
### 주어지는 튜플의 길이를 반환해주는 제네릭 Length를 만들어보세요.

__예시:__
```ts
type tesla = ["tesla", "model 3", "model X", "model Y"];
type spaceX = [
  "FALCON 9",
  "FALCON HEAVY",
  "DRAGON",
  "STARSHIP",
  "HUMAN SPACEFLIGHT"
];

type teslaLength = Length<tesla>; // expected 4
type spaceXLength = Length<spaceX>; // expected 5
```

<br>
<br>
<br>
<br>

__해답:__

```ts
/* _____________ 여기에 코드 입력 _____________ */

// 초기 제시문: type Length<T> = any
type Length<T extends { length: number }> = T["length"]; //해답
// 1. length는 타입T를 인덱싱하기 위해 사용될 수 없습니다.
// 2. 따라서 T가 length프로퍼티를 갖고있다는 것을 미리 타입스크립트 컴파일러에 인식시켜줘야합니다.
// 3. T extends {length: number} 를 인식시키면 length프로퍼티를 사용할 수 있습니다.


/* _____________ 테스트 케이스 _____________ */
import type { Equal, Expect } from '@type-challenges/utils'

const tesla = ['tesla', 'model 3', 'model X', 'model Y'] as const

const spaceX = ['FALCON 9', 'FALCON HEAVY', 'DRAGON', 'STARSHIP', 'HUMAN SPACEFLIGHT'] as const

type cases = [
  Expect<Equal<Length<typeof tesla>, 4>>,
  Expect<Equal<Length<typeof spaceX>, 5>>,
  // @ts-expect-error
  Length<5>,
  // @ts-expect-error
  Length<'hello world'>,
]
```
- JavaScript에서는 length 프로퍼티를 사용하여 배열의 길이에 접근할 수 있습니다. 타입 내에서도 똑같이 사용할 수 있습니다
  - `type Length<T extends any> = T["length"];`
- `하지만 이렇게만 할 경우 “Type ‘length’ cannot be used to index type ‘T’.”라는 컴파일 에러를 얻습니다. `
- 따라서 TypeScript에게 입력으로 주어진 타입변수가 해당 프로퍼티를 가지고 있음을 알려주어야 합니다
  - `type Length<T extends { length: number }> = T["length"];`

<br>

### 요약
- JS와달리 TS의 타입에서 length프로퍼티를 사용하려면 `{length:number}` 프로퍼티가 존재함을 미리 인식시켜야한다.

- [해답](https://ghaiklor.github.io/type-challenges-solutions/ko/easy-tuple-length.html)
- [문제](CMAcEFoIBkCmA7A5gFwBYQHsAzCAFQFcAHAG1UkQUafoCMBPCAZwEt0CD0EABQABHnwEBKCAGJANkOABsdnYqtMPRmaIgCcnAHN3qogBh7AL6NDAOUuAYVcmARcYiBsHsAioxEAcdYBdxwD6dEQBg9gDTXAGquAKU0QgDgTgCSNgLWdEAAGaFh4ADykAHxRnoA2tYAga36AHIOAKWMAdAYQgBBjgDtDAFxFUdXYnPTYbJSoENionNQAhhAAvBAA2gDkre0dAwA0EAMAtgQAJqjUEADM45Mz84sAGqvTcwsQAJoDALr1jc2clB0AxqibPf0DAGIAgsgAwgDyAHIQAJw7V4fH4QAASAFEXgA1I4TAYAEQASi8AOI-HYAZVIL0RGNBAEkAAo7UEAVQAsi9fhjCS93uCnsh8SjQaQTkUGk0Wm1OrEcPhenyEsNOkkoMBgBBUAAPJrXVqzCAAFjOXMuNzuQoFKAw-Pi6tumzFEqlstQ8tQioArPRqlEimKUdxsKDyCxAAujgBxBiCAF57AAx15QguGw2EonHKEtq11w+QAVpx8gQAE6YYBwYAAawIYBAwHUoAgAH1iyXSyWIIAb0b97oggBV5wA7LRBAKHjgFIOotljuFiC5-OSwAQs36QqVdIHOc0tYkxb0Oug2GAxzq4rhEqbWuhZpwIABvCC0JeB9DkKYsVBJiAAXynZD6ACI9-yb8cANwQCWAF1XAK9NYBN0Hyu91eBBIAAwtNqQniAB7jgCOzYAj0N+IOgAg42+ECADUDgCVY4ABC0QIAGEMQIAY6OAK1DgATTYAJ02FCaABMf6AAytgA+7d6pCAAA1-5LoAIKuADodgA+y4AD0ueIAaDWAAc1gAR48RQSAME1tiAB-dgA1nRAIGACVDgA2C4AvZ2AB1LECAC9zgAwy4APuOADftNbgYAu0OlIAOPOABiTgA+o4AlqskWRkpLH+pCrhgG7bveeAHkeJ5JueEAQcZgAlC4ALl3Mfy7HcZ4aGAAar2EQIJRGkeoSUFp2HYQIAMotyWpgAA864cntql5Y9twUyUMm2AtOc24QOCACO5AdNQEzgma8oXhARBJgQUyTCIY4INGjX3m0wDkNg3DUJwAzqNcAicBVIpdL0gyLTs6z7CscLrVsa17IsRzHBAHSbrN6DzTNc0VQadwPIMQJfL8AJwvdIIQtCsKTEiqLonCWI4niRIkhSVIQDSdIMkyLJsodx0QKd53zlV1zHW0t30C1crYPEdUNdQ8QTmOxDciMSQTEqSSk+jrVYzjjX4wBy6EyQ11GhMVoU2M9Amn1nAIDKmN80mXVJvQE7s5z4qSjzfPU4Lwuiwz8QDLgCzUAQEAAO7JtQswDJTpzfiABWFV2xGADLjECABdNgAjNcbhXdnmoD0GKgCoEx+ECAK81gAE43BgbBqG4aRpw0ZxgmyapumM6cBrp6ZtmUCu+7-p+yGYYRsAUYxvGiYpmmsDAJwBDUGN3Bzc7EDEYAHp0QLowSACctKcB+nmehznqZZjmeZAA)

