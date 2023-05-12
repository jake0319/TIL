# [정렬 : lv2] 가장 큰 수
```js

// sort함수: 리턴값이 양수면 b가 a보다 먼저온다 .

function solution(numbers) {
    var answer = '';
    numbers.sort(sortFunc)
    answer = numbers.join('')
    if (answer[0] === '0') return '0' 
    return answer;
}

const sortFunc = (a, b) => {
    const compareA = parseInt(a.toString() + b.toString())
    const compareB = parseInt(b.toString() + a.toString())
    return compareB - compareA
}
```

- `answer[0] = '0'인 경우는 원소가 모두 0인경우밖에 없음,왜냐면 sortFunc에 의해 0은 가장 오른쪽 끝으로 보내짐 `
- `sort함수 : Return값이 0보다 크면 (b,a) 배치로 바뀐다. `
- `parseInt()는 문자열로 된 부분에서 숫자(정수)만 뽑아서 변환해주는것이 특징이고, Number()은 문자열 전체가 숫자일때 소수점까지 전체 다 숫자타입으로 가져올 수 있음`


<br>

## 또다른  풀이 
```js
function solution(numbers) {
    let stringNum = 
      numbers.map((el) => el + '').sort((a,b) => (b+a) - (a+b));
  
    return stringNum[0] === '0' ? '0' : stringNum.join('');
}
```
- javascript에서 숫자+문자는 문자로 형변환
- sortFunction정의 : b+a - a+b 가 0보다 크면 배치는 (b,a)순서로 바뀐다  (`자바스크립트에서 문자열끼리 뺄셈 연산을 하면 숫자로 형변환됩니다. `)
- 정렬배열의 첫번째 원소가 0인경우는 원소가 모두 0인 경우밖에 없다. 


- 솔루션 : https://codeisneverodd-home.vercel.app/solution-pass
- 문제: https://school.programmers.co.kr/learn/courses/30/lessons/42746
