- 문제: https://school.programmers.co.kr/learn/courses/30/lessons/42576
```js
function solution(participant, completion) {
  participant = participant.sort(); //유니코드 오름차순(사전순) 정렬 
  completion = completion.sort();
  for (let i = 0; i < completion.length; i++) { //불일치 지점 찾아내기 
      if (participant[i] !== completion[i]) return participant[i]; 
  }
  return participant[participant.length - 1]; //가장 마지막에 찾고자하는 동명이인2명이 있는 경우
}
```
