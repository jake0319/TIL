- 문제: https://school.programmers.co.kr/learn/courses/30/lessons/42578?language=javascript
```js
//얼굴 : 안입음, 동그란 안경, 검정 선글라스
//상의 : 안입음, 파란색 티셔츠
//하의 : 안입음, 청바지

//입지 않을때 1을 더해서 조합을 구하면 구해진 조합 내에서 (안입음, 안입음, 청바지), (안입음, 파란색 티셔츠, 청바지) 이런식으로도 조합이 되기 때문에 하나만 입을때, 두개 입을때, ... 을 다 구해줍니다
//대신 (안입음, 안입음, 안입음) 이런 조합도 나올 수 있기 때문에 1을 빼줌 


// (모든 카테고리에 의상의 수+ (안입는경우의 +1) )로 카운트해서 각각의 곱 -1 의 로직을 사용 => 모든 세부 케이스를 구하는 간단한 방법
function solution(clothes) {
  var answer = 1;
  var obj={};
  for(var i=0;i<clothes.length;i++){
      obj[clothes[i][1]]=(obj[clothes[i][1]] || 1) + 1; // ||연산자는 좌연산자가 falsy인경우 우 연산자를 반환
      console.log(obj)
  }

  for(var key in obj){
      answer *= obj[key];
      console.log(answer)
  }
  
  return answer-1;
}
/* 풀이과정
1. 빈 객체(obj)생성
2. obj에 해당 키가 없으면 값을 1(옷을 입지 않은 경우)로 지정하고 1(옷의 개수)을 더해줌. 
3. obj에 해당 키가 있으면 해당 키의 값을 불러오고 1을 더해줌.
4. for in 구문으로 obj의 키를 반복하여 불러오고 해당 값을 answer에 곱해줌
5. 최소한 1가지 이상의 옷을 입기 떄문에 옷을 입지 않은 경우 -1로 제외. */
```
