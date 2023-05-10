## 완전탐색 - [lv2] 모음사전

```js
let idx = 0;
const result = {};
const vowels = [..."AEIOU"];

function solution(word) {
  dfs("", 0);
  return result[word];
}

const dfs = (word, length) => { //찾는단어 , 단어길이 
  if (length > 5) return; //6자리 dfs부터는 모두 종료 
  // console.log(word,idx);  
  result[word] = idx++; 
  vowels.forEach((vowel) => { //조건에 맞는 인덱스 객체 생성
    dfs(word + vowel, length + 1);      
  });
};
```
- dfs 재귀호출하며 vowels배열로부터 word : index 프로퍼티를 가진 객체를 생성
- 생성한 객체의 word를 키값으로하는 index value를 리턴
- forEach문으로 vowels배열에서 dfs를 재귀적으로 호출하는 구조가 문제의 word생성 구조와 같다는 것을 인식해야함.

### word 생성구조
```js
`` 0
A 1
AA 2
AAA 3
AAAA 4

AAAAA 5
AAAAE 6
AAAAI 7
AAAAO 8
AAAAU 9

AAAE 10

AAAEA 11
AAAEE 12
AAAEI 13
AAAEO 14
AAAEU 15

AAAI 16

AAAIA 17
AAAIE 18
AAAII 19
AAAIO 20
AAAIU 21

AAAO 22

AAAOA 23
AAAOE 24
AAAOI 25
AAAOO 26
AAAOU 27

AAAU 28

AAAUA 29
AAAUE 30 ...
```
