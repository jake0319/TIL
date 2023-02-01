# TOC
<!-- - [프로미스API](#프로미스-api) -->
- [promise.all](#promise.all)
- [promise.allSettled](#promiseallsettled)
- [promise.race](#promiserace)

<br>

# 프로미스 API
- 프로미스로 구성된 배열을 인자로 받아 새로운 프로미스를 반환하는 API (all, allSettled,race ..)

<br>

## Promise.all
- 요소 전체가 프로미스인 배열을 받고 새로운 프로미스로 반환
- 배열 안 프로미스가 모두 처리되면, 배열 안 프로미스들의 결과값을 담은 배열이 새로운 프로미스의 `result`가 된다.
- 배열 안 프로미스중 하나라도 거부되면 반환되는 프로미스도 거부됨
```js
Promise.all([
  new Promise(resolve => setTimeout(() => resolve(1), 3000)), // 1
  new Promise(resolve => setTimeout(() => resolve(2), 2000)), // 2
  new Promise(resolve => setTimeout(() => resolve(3), 1000))  // 3
]).then(alert); 
```
- 배열 안 프로미스 모두가 처리되는 3초가 지나면 [1,2,3]을 `result`로하는 프로미스 반환
- result배열 안 요소의 순서는 promise.all에 전달한 프로미스 순서와 상응한다.

<br>

```js
let urls = [
  'https://api.github.com/users/iliakan',
  'https://api.github.com/users/Violet-Bora-Lee',
  'https://api.github.com/users/jeresig'
];

// fetch를 사용해 url을 프라미스로 매핑합니다.
let requests = urls.map(url => fetch(url));

// Promise.all은 모든 작업이 이행될 때까지 기다립니다.
let names = ['iliakan', 'Violet-Bora-Lee', 'jeresig'];

let requests = names.map(name => fetch(`https://api.github.com/users/${name}`));

Promise.all(requests)
  .then(responses => {
    // 모든 응답이 성공적으로 이행되었습니다.
    for(let response of responses) {
      alert(`${response.url}: ${response.status}`); // 모든 url의 응답코드가 200입니다.
    }

    return responses;
  })
  // 응답 메시지가 담긴 배열을 response.json()로 매핑해, 내용을 읽습니다.
  .then(responses => Promise.all(responses.map(r => r.json())))
  // JSON 형태의 응답 메시지는 파싱 되어 배열 'users'에 저장됩니다.
  .then(users => users.forEach(user => alert(user.name)));
  ```
- then핸들러 리턴값은 다음 then핸들러에 
- fetch(url)은 응답데이터를 resolve한 프로미스, 이것을 json()으로 역직렬화하면 응답데이터를 객체화한 값을 result로하는  프로미스가 반환됨.

<br>

## Promise.allSettled
- Promise.all과 달리 모든 프로미스의 처리를 기다린다.
- 반환되는 배열은 다음과 같은 `요소`(객체)를 갖는다. 
1. 응답이 성공할 경우 `{status:"fulfilled", value:result}`
2. 에러가 발생한 경우 ` {status:"rejected", reason:error}`
```js
let urls = [
  'https://api.github.com/users/iliakan',
  'https://api.github.com/users/Violet-Bora-Lee',
  'https://no-such-url'
];

Promise.allSettled(urls.map(url => fetch(url)))
  .then(results => { // (*)
    results.forEach((result, num) => {
      if (result.status == "fulfilled") {
        alert(`${urls[num]}: ${result.value.status}`);
      }
      if (result.status == "rejected") {
        alert(`${urls[num]}: ${result.reason}`);
      }
    });
  });
  ```
  - 반환되는 프로미스의 result는 `status`프로퍼티를 가진 객체들을 요소로하는 배열이다. 이를 활용할 수 있다. 
  
  해당 배열은 아래와같다.
```js
[
  {status: 'fulfilled', value: ...응답...},
  {status: 'fulfilled', value: ...응답...},
  {status: 'rejected', reason: ...에러 객체...}
]
```


<br>

## Promise.race
- Promise.race는 Promise.all과 비슷합니다. 다만 가장 먼저 처리되는 프라미스의 결과(혹은 에러)를 반환합니다.
```js
Promise.race([
  new Promise((resolve, reject) => setTimeout(() => resolve(1), 1000)),
  new Promise((resolve, reject) => setTimeout(() => reject(new Error("에러 발생!")), 2000)),
  new Promise((resolve, reject) => setTimeout(() => resolve(3), 3000))
]).then(alert); // 1
```
- 첫 번째 프라미스가 가장 빨리 처리상태가 되기 때문에 `첫 번째 프라미스의 결과가 result 값`이 됩니다. 이렇게 Promise.race를 사용하면 '경주(race)의 승자’가 나타난 순간 `다른 프라미스의 결과 또는 에러는 무시됩니다`.

<br>

## 출처:
[모던자바스크립트튜토리얼- 프로미스API](https://ko.javascript.info/promise-api)
