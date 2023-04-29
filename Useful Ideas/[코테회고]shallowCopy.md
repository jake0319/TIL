# 프로그래머스 섬머코딩 인턴십 코딩테스트 1번문항 회고 
- 1번문항을 대략적으로 기억나는대로 비슷하게 구현했습니다
- 주어진문항은 2차원배열 이미지를(nxn) 가로,세로로 2n x 2n으로 복사해서 새로 생성된 이미지의 배열을 출력하는 문제 
- 어려운문제는 아니었고 배열의 복사에 대한 이해만 충분했다면 손쉽게 풀 수 있던 문제 
```js
function solution(image1) { 
let image = [[1,2,3,4,5],
            [5,4,3,2,1],
            [1,3,2,0,5],
            [0,3,1,0,3],
            ]    

var answer = []; //이차원배열
// let temp1 = Array.from(image); 
// let temp2 = Array.from(image); 
let temp1 = image.map(row => Array.from(row));
// 동일한 방식let temp1 = image.map(row => row.slice());  ///내부요소까지 복사본만들기
let temp2 = image.map(row => row.slice()); 



let newMap1 = temp1.map(elem=>[...elem,...elem.reverse()]);  

temp2= temp2.reverse()
let newMap2 = temp2.map(elem=>[...elem,...elem.reverse()]);

answer= [...newMap1,...newMap2] 
console.log(answer);
return answer; 
}
```

<br>

## 문제점 및 해결 방법
- 문제점: 배열의 shallow copy에 대한 이해가 부족했음
1. image배열을 단순히 Array.from(image) 혹은 기타 방법으로 shallow copy한다면 `형태만 같고 내부의 이차원 요소를 동일하게 참조하는 문제`가 발생
- 각각 따로 조작해줄 수 있는 독립적인 temp배열의 생성이 불가능하다.(내부 이차원 요소가 같은 참조를하기때문)
2. 따라서 내부의 이차원 요소까지 독립적인 배열로 복사하는 배열을 만들어내는 코드가 필요 `image.map(row=>row.slice()) 혹은 image.map(row=>Array.from(row)) `
3. 1번의 오류를 범했기 때문에 내부의 이차원 요소가 같은값으로 참조되고,변경되는 문제가 발생함
4. 위 오류만 피해서 잘 설계했다면 빠르게 풀 수 있었던 문제였음 

<br>

### 요약
1. array 원본변경메서드(push(),pop(),unshift(),shift(), reverse(), sort(), splice()) 까먹지 말기 
2. 이차원 배열을 복사할 때는 내부의 이차원 요소까지 map + slice | Array.from() 으로 복사해주기 
