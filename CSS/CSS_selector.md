
# CSS선택자

<br>

## * {} 
- HTML문서 내 모든 요소(head포함)

## 태그명 {} 
- scss,emotion에서 css객체 생성시 nesting으로 css타겟팅이 가능



## #id셀렉터명 {}


## .클래스셀렉터명 {}


## 셀렉터[어트리뷰트] {}

```css
a[href] { color :red; }
```

## 셀렉터[어트리뷰트="값"] {}
- 지정된 어트리뷰트를 가지며 지정된 값과 `어트리뷰트 값이 정확히 일치하는 모든 요소 선택`

## 셀렉터[어트리뷰트~="값"] {}
- 지정된 값을 (공백으로 분리된) 단어로 포함하는 요소 선택 

## 셀렉터[어트리뷰트^="값"] {}
- 지정 어트리뷰트 값으로 `시작하는` 요소를 선택

## 셀렉터[어트리뷰트$="값"] {}
- 지정 어트리뷰트 값으로 `끝나는` 요소를 선택

## 셀렉터[어트리뷰트*=”값”] {}
- 지정 어트리뷰트 값을 `단순히 포함만 하면` 선택
- 단어의 일부여도, 공백으로 분리된 값이어도 위 선택자로 선택된다.



## 후손셀렉터
- `셀렉터A 셀렉터B {}` (띄어쓰기로 구분)
- A의 모든 후손 요소인 B를 선택 

## 직계자식 셀렉터
- `A > B {}  `
- 바로 아래 직계 자식요소를 선택 

## 인접(바로뒤)형제 셀렉터 
- `A + B {}`
- A의 바로 뒤에오는 형제요소 선택

## 일반 형제 셀렉터
- `A ~ B {}`
- A의 형제요소중 A의 뒤에오는 모든 B 선택

## 가상클래스 선택자selector:pseudo-class {} 
- :link //방문하지 않은 링크일 때
- :visited //방문한 링크일 때
- :hover // 마우스 올라와 있을 때
- :active  // 클릭된 상태일 때
: :focus // 셀렉터에 포커스가 들어와 있을 때

## UI요소 상태 셀렉터
- :checked //체크 상태일 때 
- :enabled //사용가능 상태일 때
- :disabled //셀렉터가 사용 불가상태일 때 

```html
<!DOCTYPE html>
<html>
<head>
  <style>
    /* input 요소가 사용 가능한 상태일 때,
       input 요소 바로 뒤에 위치하는 인접 형제 span 요소를 선택 */
    input:enabled + span {
      color: blue;
    }
    /* input 요소가 사용 불가능한 상태일 때,
       input 요소 바로 뒤에 위치하는 인접 형제 span 요소를 선택 */
    input:disabled + span {
      color: gray;
      text-decoration: line-through;
    }
    /* input 요소가 체크 상태일 때,
       input 요소 바로 뒤에 위치하는 인접 형제 span 요소를 선택 */
    input:checked + span {
      color: red;
    }
  </style>
</head>
<body>
  <input type="radio" checked="checked" value="male" name="gender"> <span>Male</span><br>
  <input type="radio" value="female" name="gender"> <span>Female</span><br>
  <input type="radio" value="neuter" name="gender" disabled> <span>Neuter</span><hr>

  <input type="checkbox" checked="checked" value="bicycle"> <span>I have a bicycle</span><br>
  <input type="checkbox" value="car"> <span>I have a car</span><br>
  <input type="checkbox" value="motorcycle" disabled> <span>I have a motorcycle</span>
</body>
</html>
```

<br>

## 구조 가상 클래스 선택자
- p:first-child    //p중에서 첫번째 자식인p 요소
- p:last-child    //(부모가존재하는)p요소중 마지막자식인 요소
- :nth-child(n)  //셀렉터중 앞에서 n번째 자식인 요소
- :nth-last-child(n) //셀렉터중 뒤에서 n번째 자식인 요소

<br>

- p:first-of-type //p요소의 부모의 자식요소중 첫 번째 등장하는 p
- p:last-of-type // p요소의 부모의 자식요소중 마지막p
- p::nth-of-type(n) //p요소의 부모의 자식요소인 p중 n번째

### selector:nth-child(n) 와 selector:nth-of-type(n)의 차이 
1. 두 선택자의 차이점은 type 조건의 만족 여부입니다.
2. `nth-child()`의 경우 부모 요소의 모든 자식 중 순서만 맞다면 해당 요소를 선택합니다. 
3. `반면 nth-of-type()`의 경우 부모 요소의 모든 자식 요소 중
`①type 조건`을 만족하고 `②순서`를 만족하는 대상을 선택합니다.

>요약) nth-child는, 해당 선택자의 부모요소중 순서대로 n번째
nth-of-type은 해당 선택자의 부모요소의 해당선택자중에서 n번째를 만족해야 선택된다.

__예시__
 ```html
 <div>
  <h1>Heading1</h1>
  <p>Lorem</p>     //// (1)
  <p>ipsum</p>     //// (2)
  <p>dolor</p>
</div>
```

```css
p:nth-child(2) {
  color: white;
  background-color: gold;
} //// (1)이 선택됩니다. 


p:nth-of-type(2) {
  color: white;
  background-color: gold;
} //// (2)가 선택됩니다. 




```


<br>

## 부정셀렉터
- `:not(셀렉터)` //셀렉터에 해당하지 않는 모든 요소 선택

<br>

## 정합성체크 셀렉터 
- :vaild(셀렉터) //정합성 검증이 성공한 input 또는 form요소 선택
- :invalid(셀렉터) //정합성 검증이 실패한 input 또는 form요소 선택


<br>

## 가상요소셀렉터
- ::first-letter
- ::first-line
- ::after // 컨텐츠 뒤에 위치하는 공간 선택(content프로퍼티와 함께 사용)
- ::before // 컨텐츠 앞에 위치하는 공간 선택(content프로퍼티와 함께 사용)

<br>

### 출처:
- [POIEMAWEB - css셀렉터](https://poiemaweb.com/css3-selector)
