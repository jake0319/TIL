
# 1.  || 연산자 (for react조건부렌더링)

`const a = 0;
const b = a || 3 `
- `||`연산자는 **falsy값**`(0,’’,false,NaN,null,undefined)`
(느슨한 falsy값 ) 
이 평가값으로 오면 뒤로 넘어감(3)

## React -조건부 렌더링
```jsx
count =0;
count || <div>123</div> ; 
```
- count가 null과 undefined일때만 뒤로 넘어가고싶은데..? 
- => nullish coalescing 사용




## Q. 널병합(nullish coallescing)/옵셔널체이닝 왜 사용하나요?
A. 자바스크립트 코드를 간결하게 만들어주면서,
안전하게 사용할 수 있게 해줌.
- if 문 남발 줄여줌
- try{}..catch(e){} 문 줄여줌
- 예상치 못한 에러 발생 줄여줌


# 2.   null병합연산자 ?? 
- (falsy값중에서) null과 undefined일 때만 뒤에 값 참조로 넘어감.
- `||` 연산자 대용으로 사용(좀 더 세세한 조건을 맞추기위한)
- falsy한 값(0, ‘’ , false , NaN , null, undefined)중 null과 undefined만 따로 구분함.
```js
const c = 0;
const d = c ?? 3;   // d는 0
// ??(nullish coalescing)는 null과 
undefined일 때만 오른쪽 참조로 넘어감 
```

# 3. 옵셔널체이닝 (널병합은 null,undefined일때만 뒤로 참조하고, 옵셔널은 null,undefined아닐때만 뒤로참조 )
- null / undefined가 아닐 때만 뒤로 참조를 이어감.
    -  null / undefined일때는 참조를 멈추고 undefined를 반환
- null이나 undefined의 속성을 조회하는 경우 에러가 발생하는 것을 막는다.

>//TypeError: Cannot read properties of null
(reading ‘d’);
// 비동기데이터 페칭시, 값이 존재하지 않는 값의
(쿼리요청에러 발생한) 참조가 불가능한
에러 발생 방지

### ex1)
```js
const a ={};
a.b; // a가 빈 객체이므로 값이 존재함 (문제없음)
```

### ex2)
```js
const c = null;
try { 
  c.d;
} catch (e) {
 console.error(e)
}

//TypeError: Cannot read properties of null
(reading ‘d’);
```
- `try { …} catch (e) { …} `로 도배하면 좋지않음
    - `?.`로 간단히 대체 가능!



ex3) 객체나 배열 요소 접근에 대해서도 사용가능
- c?.f()  // c가 참조가능값일때,객체 메서드 참조
- c?.[0]   // c가 참조가능값일때, 배열 인덱스를 통한 참조

ex4)옵셔널체이닝(null,undefined가 아닌 경우 속성값 참조) 
- null병합연산자(앞에오는 평가값이 null,undefined(엄격한 falsy)일 때만  뒤로 넘어감,)
```js
const c = null;
c?.[0] ?? 123  // null ?? 123 
=> 123 
// c= null이므로 c?.[0] //null
// null ?? 123   
// 123  (null이 앞에오면 뒤로 넘어감)
```




