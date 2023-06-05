# 숫자문자열과 영단어 
## 나의해답
```js
const table = { 
'zero': 0, // => 문자열 '0'으로 처리해야 falsy취급 받지 않으므로 주의! 
'one': 1,
'two': 2,
'three': 3,
'four': 4,
'five': 5,
'six': 6,
'seven':7,
'eight': 8,
'nine': 9,
}
// s를 순회하면서 테이블의 단어를 만나면 array.push , 숫자는 그대로 추가 
function solution(s) {
    const array =[];
    let arrayedS = s.split('');
    // console.log(arrayedS)
    let tempo = '';

    for (let letter of arrayedS){

        if( !isNaN(Number(letter))){ //letter:숫자면 
            array.push(letter);
            tempo='';
            continue; //letter: 숫자면 push하고 다음 순회로
        } 
        else{ //letter:문자면

            tempo+=letter //템포에 더하면서 체크 
   
            if(table[tempo]){//테이블에서 검색이 되면
                array.push(table[tempo])
                console.log(array)
                tempo=''//초기화
                }
            }
        }
    return Number(array.join(''))
    }
```
- 주의 : table객체의 value가 number이면 0의 케이스에서 falsy값이 되므로 테스트케이스를 모두 통과할 수 없음 주의 
- isNaN메서드로 숫자와 문자를 판별, 숫자인 경우 array정답배열에 push하고 다음 순회로 continue
- letter가 문자인경우 array에 table[tempo] 문자를 추가하고 tempo초기화 
- 정답배열을 join('') 메서드로 string으로 연결한 뒤 숫자로 바꾸어 리턴  

>isNaN() 함수는 몇 가지 예기치 못한 동작을 보일 수 있습니다. 이러한 동작은 주로 인자로 전달된 값의 형태나 타입에 따라 발생할 수 있습니다. 
몇 가지 예기치 못한 오류 상황들은 다음과 같습니다:

1. 빈 문자열('') 또는 공백 문자열(' ')의 경우: isNaN('')와 isNaN(' ')은 모두 false를 반환합니다. 즉, 빈 문자열 또는 공백 문자열은 숫자로 간주되지 않습니다.

2. 공백 문자열을 포함한 숫자 형식의 문자열: isNaN(' 123 ')과 같이 숫자를 나타내는 문자열에 공백 문자가 포함된 경우에도 false를 반환합니다. isNaN() 함수는 문자열의 시작과 끝에서 공백 문자를 자동으로 제거하지 않습니다.

3. 숫자를 나타내는 문자열의 형식: isNaN('123abc')와 같이 숫자 뒤에 알파벳 문자가 포함된 경우에도 false를 반환합니다. isNaN() 함수는 단순히 문자열에 숫자가 포함되어 있는지 여부를 확인하지만, 숫자의 형식을 철저히 검사하지는 않습니다.

4. 특정 객체 또는 배열: isNaN({}) 또는 isNaN([])와 같이 객체나 배열을 전달한 경우에는 항상 true를 반환합니다. isNaN() 함수는 숫자 형태의 값에 대해서만 작동하며, 객체나 배열은 숫자 형태가 아니므로 항상 true를 반환합니다.
- `즉 isNaN함수는 Number()형변환 + NaN인가? 를 판별하는 기능임, Number()형변환함수는 숫자가 아닌 경우 모두 0을 반환하므로 isNaN('') === isNaN(0) === false가 되는 것임` 
- Number.isNaN()함수는 좀 더 엄격하게 NaN인 경우만 true를 반환함. (문자,숫자 구별을 위해서는 isNaN()을 쓰는 것이 타당해보임) 

<br>
<br>

## 솔루션
```js
const table = {
  zero: '0',
  one: '1',
  two: '2',
  three: '3',
  four: '4',
  five: '5',
  six: '6',
  seven: '7',
  eight: '8',
  nine: '9',
};

function solution(s) {
  let answer = '';
  let word = '';

  for (let i = 0; i < s.length; i++) {
    const char = s[i];

    if (!isNaN(Number(char))) {
      // 숫자인 경우
      answer += char;
    } else {
      // 문자인 경우
      word += char;

      if (table[word]) {
        // 영단어가 매칭되는 경우
        answer += table[word];
        word = '';
      }
    }
  }

  return Number(answer);
}

```
