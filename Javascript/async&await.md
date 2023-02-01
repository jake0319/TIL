
# TOC
- [async 키워드](#async-키워드)
- [await 키워드](#await-키워드)


# Async & Await 
## async 키워드

- `async`키워드는 function 앞에 위치합니다.
- async가 붙은 함수는 `항상 프로미스를 반환`합니다.
- 프로미스가 아닌 값을 반환하더라도 항상 resolved promise로 래핑해 이행된 프로미스로 반환함.

<br>

```js
async function f() {
  return 1;
}

f().then(alert); // 1
```
- async함수는 항상 `이행된 프로미스를 반환하므로` then메서드 체이닝이 가능

```js
async function f() {
  return Promise.resolve(1);
}

f().then(alert); // 1
```
- async함수는 반드시 프로미스를 반환하고, 프로미스가 아닌 것도 프로미스로 래핑해 반환

<br>
<br>

## await 키워드
- 반드시 async함수 내부에서만 사용가능
- 자바스크립트는 await키워드를 만나면 프로미스가 처리될(이행,거절) 때가지 기다립니다.
```js
async function f() {

  let promise = new Promise((resolve, reject) => {
    setTimeout(() => resolve("완료!"), 1000)
  });

  let result = await promise; // 프라미스가 이행될 때까지 기다림 (*)

  alert(result); // "완료!"
}

f();
```
- 생성된 promise는 1초뒤 resolve되고, await은 이를 기다리게만든다.
- await키워드가 붙은 프로미스는 result값을 변수에 할당할 수 있다.
- then메서드를 통해 프로미스result값을 핸들러의 내부적으로 활용하는 것의 편의문법임.
//await는 promise.then보다 좀 더 세련되게 프라미스의 result 값을 얻을 수 있도록 해주는 문법입니다. promise.then보다 가독성 좋고 쓰기도 쉽습니다.

<br>

```js
async function showAvatar() {

  // JSON 읽기
  let response = await fetch('/article/promise-chaining/user.json');
  // 프로미스가 처리되길 기다리고, api데이터(json형식)를 response에 할당
  let user = await response.json();
  // response를 역직렬화

  // github 사용자 정보 읽기
  let githubResponse = await fetch(`https://api.github.com/users/${user.name}`);
  let githubUser = await githubResponse.json();

  // 아바타 보여주기
  let img = document.createElement('img');
  img.src = githubUser.avatar_url;
  img.className = "promise-avatar-example";
  document.body.append(img);

  // 3초 대기, 값을 활용하지 않는 promise 
  await new Promise((resolve, reject) => setTimeout(resolve, 3000));

  img.remove();

  return githubUser;
}

showAvatar();
```

## 출처:
[모던자바스크립트 튜토리얼- async&await](https://ko.javascript.info/async-await)

