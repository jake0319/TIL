
# React에서 emotion.js 사용하기

##  `css prop`으로 css를 prop형태로 넣어주기
- 렌더링할 엘리먼트의 css속성값에 `css백틱(css코드)`형태의 `표현식값`을 넣어주면 됨 .
- css코드에는 `scss`문법으로 nesting등의 적용이 가능함. 

<br>

```jsx
//tagged template literal 예시
const style = css`color: hotpink;`
```

```jsx
import { css } from '@emotion/react'

const color = 'white' //color변수 정의

return(
	<div 
	  css={css`
		padding: 32px;
		background-color: hotpink;
		font-size: 24px;
		&:hover { 
			color: ${color}; //정의한 color변수를 ${color}로 사용
		  }
		`}>
		Hover to change color
		</div>
)
```
- `import { css } from '@emotion/react'`
- 리액트엘리먼트에 `css prop`값을 넣어줄 수 있다. 
// css prop값은 `{css`적용시킬 css코드`}`형태로 작성 (js표현식은 {}중괄호로 감싸야한다.)

<br>

## css prop대신 `styled.element`로 정의하기(css prop보다 덜 popular한 방식)
```jsx
import styled from '@emotion/styled'

const Button = styled.button`
	padding:32px;
	background-color: hotpink;
	font-size: 24px;
	border-radius: 4px;
	color: black;
	font-weight: bold;
	&:hover { 
		color: white;
	  }
	}
`	
return( <Button> This my button component</Button> 
)
``` 
- styled component와 작성방식이 동일하다 

<br>

# install
- 리액트를 사용중이라면 가장 쉬운방법:  `npm i --save @emotion/react ` package.
	- 혹은 `yarn add @emotion/react`

## 1. css prop 사용시
- `npm i --save '@emotion/react'`
- `import { css } from '@emotion/react'`

## 2. styled 사용시
- `npm i --save @emotion/styled` | `yard add @emotion/styled`
- `import styled from '@emotion/styled'`


### @emotion/babel-plugin 
> CRA를 사용해 프로젝트를 세팅했다면, `Babel macro`를 사용할 수 있습니다.
> 이모션은 선택적인 Babel 플러그인이 존재하고 더 나은 개발자 경험을 선사합니다.
- `npm install --save-dev @emotion/babel-plugin` | `yarn add --dev @emotion/babel-plugin`
// dev옵션은 빌드시 개발용으로 설치하는 옵션을 의미

```js

//
"emotion" must be the first plugin in your babel config plugins list.
//

{
  "plugins": ["@emotion"]
}

//
If you are using Babel's env option emotion must also be first for each environment.
//
{
  "env": {
    "production": {
      "plugins": ["@emotion", ...otherBabelPlugins]
    }
  },
  "plugins": ["@emotion"]
}
```
- 플러그인에 @emotion을 첫번째로 명시해주어야함

<br>


## css prop 사용법 
- className을 적용시킬 수 있는 모든 컴포넌트|엘리먼트들은 css prop을 사용할 수 있습니다.
// css prop에 적용되는 스타일은 `className` prop형태로 적용되는 class name으로 최종적으로 평가됩니다.
// 특정 엘리먼트에 css prop 값 할당시 이것이 평가되어 동적으로 className이 생성.(className작명을 방지해줌)

### 1.Object Styles
- css prop은 object형태의 값을 허용합니다.
- object 스타일로 선언한 css prop값은 별도의 import구문을 필요로하지 않음.

```jsx
return(
  <div
    css={{
      backgroundColor: 'hotpink',
      '&:hover': {
        color: 'lightgreen'
      }
    }}
  >
    This has a hotpink background.
  </div>
)
```
- css prop의 값으로 `{css 코드}`형태의 표현식을 넣어줌 ( js표현식에 {css code}를 넣으므로 이중괄호 )

<br>

### 2. String Styles
- `tagged template literal`형태로 css prop을 전달할 수도 있습니다.
- 위 방식의 경우 `import { css } from '@emotion/react'` 구문이 필요합니다.

```jsx
import { css } from '@emotion/react'

const color = 'darkgreen'

render(
  <div
    css={css`
      background-color: hotpink;
      &:hover {
        color: ${color}; //전역으로 선언한 변수도 받을 수 있음.
      }
    `}
  >
    This has a hotpink background.
  </div>
)
```
- `css키워드로 시작하는 tagged template literal`로 css prop 속성을 지정합니다.
> Note: css from @emotion/react는 계산된 class name 문자열을 제공하지 않음.

<br>

## Styled components
> style이 첨부된 리액트 컴포넌트를만드는 방법임.

```jsx
import styled from '@emotion/styled'

const Button = styled.button`
  color: turquoise;
`
return(
	<Button>This my button component.</Button>
	)
```

<br>

### prop을 기반으로 바뀌는 styled-component
- styled안에서의 사용되는 `함수형태의 인자들은 반드시 props와 함께 호출`된다.
// 이것이 props기반으로 컴포넌트의 스타일을 변경시키도록 허용한다.

```jsx
import styled from '@emotion/styled'

const Button = styled.button`
  color: ${props => (props.primary ? 'hotpink' : 'turquoise')};
`

const Container = styled.div(props => ({
  display: 'flex',
  flexDirection: props.column && 'column'
}))

render(
  <Container column>  //column값이 props로 전달 
    <Button>This is a regular button.</Button>
    <Button primary>This is a primary button.</Button>
  </Container>
)
```
- `styled.생성할엘리먼트 + tagged template literal with CSS code => 컴포넌트`
// 함수형태의 인자는 반드시 props를 갖는 형태로 작성해야함
// 위 방식으로 컴포넌트 생성시 컴포넌트의 인자로 넣어서 호출해주는 값은 props값으로 전달됨.

<br>

### 아무 컴포넌트 스타일링하기 
- className prop를 갖는 컴포넌트라면 모두 스타일링이 가능
```jsx
import styled from '@emotion/styled'

const Basic = ({ className }) => <div className={className}>Some text</div>

const Fancy = styled(Basic)`
  color: hotpink;
`
render(<Fancy />)
```
- __cosnt `새로 스타일링할 컴포넌트`  = style(`스타일링할 컴포넌트`) + TTL(css)__
- 스타일링할 컴포넌트는 className props를 가져야함.

>const { className } = 분해할객체(props)  
//jsx표현식 값으로 사용할 변수명 : className  
// 컴포넌트는 props 인자만을 받을 수 있음.  
// className 변수 = props.className 값이 할당됨
// {className} 구조분해시 -> `className` props의 값으로 `{className}`할당가능

<br>

### withComponent메서드로 렌더링된 태그 변경하기
- section태그로 만들어준 컴포넌트를 다른 태그로 변경할 수 있음.
```jsx
import styled from '@emotion/styled'

const Section = styled.section`
  background: #333;
  color: #fff;
`

// Section컴포넌트와 동일한 style을 갖고있지만 aside태그로 렌더링됨.
const Aside = Section.withComponent('aside')

render(
  <div>
    <Section>This is a section</Section>
    <Aside>This is an aside</Aside>
)
```

<br>

### 다른 emotion컴포넌트를 타켓팅해서 styling하기
- `@emotion/babel-plugin`사용시 일반 css선택자 처럼 다른 컴포넌트를 타겟팅해서 스타일링이 가능하다.

```jsx
import styled from '@emotion/styled'

const Child = styled.div`
  color: red;
`

// Parent컴포넌트의 자식요소인 Child컴포넌트에만 적용되는 css속성
const Parent = styled.div`
  ${Child} {
    color: green;
  }
`

render(
  <div>
    <Parent>
      <Child>Green because I am inside a Parent</Child>
    </Parent>
				// Parent 바깥에 존재하는 컴포넌트이므로 green이 적용되지 않음
    <Child>Red because I am not inside a Parent</Child>
  </div>
)
```

<br>

- `__styled(Object)__`형태로도 작성 가능

```jsx 
import styled from '@emotion/styled'

const Child = styled.div({
  color: 'red'
})

// object형태로 작성시 [key(컴포넌트명)]: { css code } 형태로 작성한다 .
const Parent = styled.div({
  [Child]: {
    color: 'green'
  }
})

render(
  <div>
    <Parent>
      <Child>green</Child>
    </Parent>
    <Child>red</Child>
  </div>
)
```
1. styled.element(TTL)방식으로 정의한 컴포넌트생성
2. TTL방식으로 부모컴포넌트 스타일 정의시 `${자식컴포넌트명}``{css}`형태로 자식컴포넌트의 스타일 지정이 가능
3. (object key-value방식)부모컴포넌트 스타일 정의시 자식컴포넌트명을 key값으로하고 value에 css객체를 넣어주면 자식컴포넌트 스타일링이 가능 

<br>

### Object styles로 작성하기
 ```jsx
 import styled from '@emotion/styled'

const H1 = styled.h1(
  {
    fontSize: 20  // h1태그에 적용하는 css묶음, 
  },
  props => ({ color: props.color })  // props형태로 작성하는 css묶음: 화살표함수 리턴값 객체가 남는다.
)

render(<H1 color="lightgreen">This is lightgreen.</H1>)
```
- `styled.element()`에 복수의 style객체를 넣어줄 수 있음!
// 1. 일반 style객체
// 2. 컴포넌트에 props전달을 통해 동적으로 생성되는 style객체(arrow-function형태)

<br>

### Customizing prop forwarding
- html속성으로 유효한 props들만 필터링해서 전달해주는 방식에 사용
```jsx
import isPropValid from '@emotion/is-prop-valid'
import styled from '@emotion/styled'

const H1 = styled('h1', {
  shouldForwardProp: prop => isPropValid(prop) && prop !== 'color'
})(props => ({
  color: props.color
}))

render(<H1 color="lightgreen">This is lightgreen.</H1>)
```

<br>

### Composing dynamic styles(스타일합성: style변수로 정의해서 활용하기)
```jsx
import styled from '@emotion/styled'
import { css } from '@emotion/react'

//1. Tagged Template Literal로 작성된 css코드를 변수처럼 활용 
const dynamicStyle = props =>
  css`
    color: ${props.color};
  `
//2. props를 인자로 받는 함수로 css코드 정의 (object형태의 style객체 리턴)
const dynamicStyle = props =>({color: props.color});

//아래 코드는 위에서 정의한 dynamicStyle 코드가 적용됨.
const Container = styled.div`
  ${dynamicStyle};
`
render(<Container color="lightgreen">This is lightgreen.</Container>)
```
- style객체는 1.`태그드 템플릿 리터럴 방식`(1.인라인 방식은 css키워드 | 생성자방식은 2.styled.element) 으로 정의하거나 또는 2.object형태로 정의할 수 있음.
 그리고 이 객체를 변수에 할당해서 재활용 할 수 있는데, TTL방식에서는 `${변수명}`으로 활용한다.

<br>

### Nesting components
- `&`:부모선택자 사용을 통해 네스팅이 가능
```jsx
import styled from '@emotion/styled'

const Example = styled(span')`
  color: lightgreen;
  & > strong { // <Example>컴포넌트의 직계자식중 strong태그(엘리먼트)
    color: hotpink;
  }
`

render(
  <Example>
    This is <strong>nested</strong>.
  </Example>
)
```

<br>

## Composition(합성)
- 합성은emotion의 가장 강력한 사용패턴중 하나입니다.
```jsx
import { css } from '@emotion/react'

const base = css`
  color: hotpink;
`
render(
  <div
    css={css`
      ${base};
      background-color: #eee;
    `}
  >
    This is hotpink.
  </div>
)
```
- const css객체변수명 = `css tagged template literal` 문법은 `css객체`를 생성해줍니다.
1. css객체가 들어가야 할 위치에 해당 css ttl문법이 완벽히 대체할 수 있습니다.
2. 이는 자바스크립트 표현식이되고, jsx에서 inline css어트리뷰트의 값이 될 수 있습니다.
3. (사용예) `<div css={css객체변수} ></div>`
4. ttl속에서 css객체변수를 참조할 경우 `${변수명}`형태로 참조해야합니다.

<br>

### Legacy Css Vs emotion composition
__Legacy__
```jsx
render(
  <div>
    <style>
      {`
        .danger {
          color: red;
        }
        .base {
          background-color: lightgray;
          color: turquoise;
        }
      `}
      >
    </style>
    <p className="base danger">What color will this be?</p>
  </div>
)
```
- `CSS cascade rules`에 따라 .danger는 .base에 의해 `override됩니다.`

<br>

__Emotion Composition__
```jsx
import { css } from '@emotion/react'

const danger = css`
  color: red;
`
const base = css`
  background-color: darkgreen;
  color: turquoise;
`
render(
  <div>
    <div css={base}>This will be turquoise</div>
    <div css={[danger, base]}> 
      This will be also be turquoise since the base styles overwrite the danger
      styles.
    </div>
    <div css={[base, danger]}>This will be red</div>
  </div>
)
```
- emotion을 사용할 경우 `base & danger` 스타일블록을 따로 생성한 뒤 css어트리뷰트에 개별적으로 전달할 수 있습니다.
- [danger,base]와 같이 배열값을 넣어줘도 되며 순서에 의해 base가 danger을 오버라이딩합니다.

<br>

## Object styles
- `Object styles`에서 css property는 `camelCase`로 작성합니다. 
// background-color => backgroundColor 

### Examples
__With the css prop__
```jsx
render(
  <div
    css={{
      color: 'darkorchid',
      backgroundColor: 'lightgray'
    }}
  >
    This is darkorchid.
  </div>
)
```
- `css프로퍼티: value` 는 `camelCase_Key : 'string_value'`로 작성한다.

<br>

__With `styled`__
```jsx 
import styled from '@emotion/styled'

const Button = styled.button(
  {
    color: 'darkorchid'
  },
  props => ({
    fontSize: props.fontSize
  })
)

render(<Button fontSize={16}>This is a darkorchid button.</Button>)
```
- `styled`방식으로 작성시 파라미터에는 1.css객체 2.props함수 형태로 넣어준다.

<br>

__Child Selectors__
```jsx
render(
  <div
    css={{
      color: 'darkorchid',
      '& .name': {
        color: 'orange'
      }
    }}
  >
    This is darkorchid.
    <div className="name">This is orange</div>
  </div>
)
```
- 콤마(,)로 구분해서 `'선택자':{css객체} (key-value)형태로` nesting 할 수 있음
// 주의) 선택자 중첩시 선택자는 `'문자열'`형태로 작성한다 .

<br>

### Object로 Media Queries 정의하기 
```jsx
render(
  <div
    css={{
      color: 'darkorchid',
      '@media(min-width: 420px)': {
        color: 'orange'
      }
    }}
  >
    This is orange on a big screen and darkorchid on a small screen.
  </div>
)
```
- 문자열 형식으로 작성하며, `@media(min-width: 420px)' : {css code }`형태로 작성 

<br>

### Numbers 
```jsx
render(
  <div
    css={{
      padding: 8,
      zIndex: 200
    }}
  >
    This has 8px of padding and a z-index of 200.
  </div>
)
```

<br>

### Numbers
``` jsx
render(
  <div
    css={{
      padding: 8,
      zIndex: 200
    }}
  >
    This has 8px of padding and a z-index of 200.
  </div>
)
```
- `단위가 없는 css속성`이 아닌이상 number에 `px`단위가 자동으로 추가됩니다.

<br>

### Arrays
```jsx
render(
  <div
    css={[
      { color: 'darkorchid' },
      { backgroundColor: 'hotpink' },
      { padding: 8 }
    ]}
  >
    This is darkorchid with a hotpink background and 8px of padding.
  </div>
)
```
- 중첩된 배열은 평탄화됩니다.
- `[css1,css2,css3] => {css1,css2,css3}``

<br>

### Fallbacks(대안)
```jsx
render(
  <div
    css={{
      background: ['red', 'linear-gradient(#e66465, #9198e5)'],
      height: 100
    }}
  >
    This has a gradient background in browsers that support gradients and is red
    in browsers that don't support gradients
  </div>
)
```
- 지원하지 않는 css속성에 대해 배열로 fallback 인자를 넣어줄 수 있음.
// fallback(동작하지 않는 속성에 대한 대안 속성)

<br>

### with css키워드 
```jsx
import { css } from '@emotion/react'

const hotpink = css({
  color: 'hotpink'
})

render(
  <div>
    <p css={hotpink}>This is hotpink</p>
  </div>
)
```
- css와 object 스타일을 함께 사용할 수 있습니다.
- cssTTL형식으로 작성시 `'hotpink'`는 string형식이 아닌 css코드형식으로 작성해야함.

<br>

### Composition(합성)
```jsx
import { css } from '@emotion/react'

const hotpink = css`
  color: hotpink;
`

const hotpinkHoverOrFocus = css({
  '&:hover,&:focus': hotpink
})//다중선택자 적용 

const hotpinkWithBlackBackground = css(
  {
    backgroundColor: 'black',
    color: 'green'
  },
  hotpink
)

render(
  <div>
    <p css={hotpink}>This is hotpink</p>
    <button css={hotpinkHoverOrFocus}>This is hotpink on hover or focus</button>
    <p css={hotpinkWithBlackBackground}>
      This has a black background and is hotpink. Try moving where hotpink is in
      the css call and see if the color changes.
    </p>
  </div>
)
```
> css 컴포지션에 대한 정리
1. css키워드는 1.object Style, 2. Tagged Template Literal 방식을 지원하고,
2. 결과적으로 css객체를 생성해주는 방법이며
3. jsx에서 React.element의 css prop에 생성한 css객체값을 전달할 수 있다.(css객체는 js표현식이므로 중괄호로 감싸주어야함)

> object스타일 
1. `css({css코드})`형태로 작성 `(css함수에 css객체를 전달하는 형태로)`
2. css코드는 key-value형태로 작성하며, key값에 선택자가 들어올 때는 string type을 전달한다.
3. value는 기본적으로 string type으로 작성
4. css함수에는 여러 css객체를 파라미터로 전달할 수 있고, 변수로 정의한 css객체또한 할당 가능하다.

> Tagged Template Litral 스타일
1. 전통적인 css코드를 css키워드+백틱 사이에 작성한다.
2. `백틱`속에서 외부의 css객체로 정의한 `변수는 ${변수}`형태로 참조할 수 있음.

<br>

## Nested Selectors 
```jsx
import { css } from '@emotion/react'

const paragraph = css`
  color: turquoise;

  a {
    border-bottom: 1px solid currentColor;
    cursor: pointer;
  }
`
// p태그의 style 속에 a앵커태그의 style을 nesting으로 작성 후, p태그의 css prop으로 전달.

render(
  <p css={paragraph}>
    Some text. <a>A link with a bottom border.</a>
  </p>
)
```
- css nesting은 특정 엘리먼트를 타겟팅하는 데에 효과적인 방법 

<br>

```jsx 
import { css } from '@emotion/react'

const paragraph = css`
  color: turquoise;

  header & {
    color: green;
  }
`
//header태그의 자손중 모든 현재선택자(&: p태그)에 color:green; 속성 부여

render(
  <div>
    <header>
      <p css={paragraph}>This is green since it's inside a header</p>
    </header>
    <p css={paragraph}>This is turquoise since it's not inside a header.</p>
  </div>
)
```
- `&`선택자는 `현재선택자(current class)`를 의미합니다.

__Refactor__
```jsx
import { css } from '@emotion/react'

// 선택자를 key로 사용시 string type으로 작성 
const headerCss = css({
'header &':{color:'green'}
 }
)

// css()에는 여러 css객체를 전달할 수 있습니다. 
// 여러 css객체를 배열로 전달할 경우 flattened(평탄화)됩니다.
const paragraph = css([
{
  color: 'turquoise'},
headerCss]
)
render(
  <div>
    <header>
      <p css={paragraph}>This is green since it's inside a header</p>
    </header>
    <p css={paragraph}>This is turquoise since it's not inside a header.</p>
  </div>
)
```

<br>

## Media Queries
```jsx
import { css } from '@emotion/react'

render(
	//최소 420px 이상의 뷰포트 영역에서 적용합니다.
  <p
    css={css`
      font-size: 30px;
      @media (min-width: 420px) {
        font-size: 50px;
      }
    `}
  >
    Some text!
  </p>
)
```
- emotion에서 `Media Queries`는 일단 css작성과 동일하게 작성한다.

<br>

```jsx
import { css } from '@emotion/react'

//전환점(breakpoints) 정의 
const breakpoints = [576, 768, 992, 1200]

//map메서드로 문자열매핑 배열 생성, 이후 mq[n]으로 참조
const mq = breakpoints.map(bp => `@media (min-width: ${bp}px)`)

render(
  <div>
    <div
      css={{
        color: 'green',
        [mq[0]]: {
          color: 'gray'
        },
        [mq[1]]: {
          color: 'hotpink'
        }
      }}
    >
      Some text!
    </div>
    <p
      css={css`
        color: green;
        ${mq[0]} {
          color: gray;
        }
        ${mq[1]} {
          color: hotpink;
        }
      `}
    >
      Some other text!
    </p>
  </div>
)
```
> div태그에 css prop으로 작성한 style코드는 `576px, 768px, 992px , 1200px` 영역에서   
순서대로 media-query가 적용되며, cascade rule에 의해 동작한다.  
(1200px영역 이상에선 1200px이상 영역에 해당하는 미디어 쿼리만 적용됨.)

<br>

## Global styles
- `css reset | font face`와 같은 글로벌스타일 적용시 사용
- Global컴포넌트 정의 후 styles prop의 값으로 글로벌 스타일을 작성

```jsx
import { Global, css } from '@emotion/react'

render(
  <div>
    <Global
      styles={css`
        .some-class {
          color: hotpink !important;
        }
      `}
    />
    <Global
      styles={{
        '.some-class': {
          fontSize: 50,
          textAlign: 'center'
        }
      }}
    />
    <div className="some-class">This is hotpink now!</div>
  </div>
)
```

<br>

## Package Summaries
 __`@emotion/react`__
- react로 개발시 사용하는 패키지
` yarn add @emotion/react | npm i --save @emotion/react`

<br>

# ADVANCED
## Best Practices

### 타입스크립트와 오브젝트스타일
- css string(ttl)을 사용한 스타일 작성시 타입스크립트 인텔리센스의 도움을 얻을 수 없으므로 
Obejct 스타일로 작성을 권장합니다(object style작성시 정적타입체크 지원).

```jsx
const myCss = css({
  color: 'blue',
  grid: 1 // Error: Type 'number' is not assignable to type 'Grid | Grid[] | undefined'
})
```

<br>

### Colocate styles with components(컴포넌트와 style의 동시배치)
> 일반적인 css사용시 컴포넌트와 스타일코드는 별개의 파일에 정의되며
이것은 유지보수를 어렵게 만들고,컴포넌트 수정 후 관련 css의 코드의 업데이트를 망각하게 만듭니다.   
이를 극복하는 emotion의 가장 큰 장점 중 하나는 컴포넌트와 style코드의 동시 작성입니다.

<br>

### Consider How you will share styles(스타일 쉐어 방법)
__방법1. Export CSS object__
```jsx
export const errorCss = css({
  color: 'red',
  fontWeight: 'bold'
})

// Use arrays to compose styles
export const largeErrorCss = css([errorCss, { fontSize: '1.5rem' }])
```
- Use arrays to compose styles: `스타일합성을 위해 array를 사용합니다.`
- object style로 정의한 css객체를 `css(배열[css객체1,css객체2])`형태로 다시 전달

<br>

__방법2.Share styels via component reuse__
```jsx
export function ErrorMessage({ className, children }) {
  return (
    <p css={{ color: 'red', fontWeight: 'bold' }} className={className}>
      {children}
    </p>
  )
}

// `fontSize: '1.5rem'` is passed down to the ErrorMessage component via the
// className prop, so ErrorMessage must accept a className prop for this to
// work!
export function LargeErrorMessage({ className, children }) {
  return (
    <ErrorMessage css={{ fontSize: '1.5rem' }} className={className}>
      {children}
    </ErrorMessage>
  )
}
```
1. css prop의 값을 설정시 동적으로 이 선택자를 타겟팅하는 className이 생성된다.
2. 이 className은 ( ErrorMessage 컴포넌트에서 생성 -> prop으로 전달 -> p태그의 className으로 전달 )
3. 결과적으로 ErrorMessage 컴포넌트에 적용한 style은 className을 공유하는 p에도 전달된다.
4. p태그에 fontSize: '1.5rem' style이 적용된다.

<br>

## Keyframes
> css Keyframes사용시 `keyframes 헬퍼`를 사용할 수 있습니다.  
css keyframes헬퍼는 style로 사용 가능한 객체를 리턴합니다.

<br>

__1.Normal CSS keyframes__
```css
@keyframes roll-and-round {
  /* 시작 */
  from {
    left: 36px;
    border-radius: 0;
    transform: scale(1) rotate(0deg);
    opacity: 1;
  }

  /* 끝 */
  to {
    left: 600px;
    border-radius: 100%;
    transform: scale(0.25) rotate(1080deg) ;
    opacity: 0;
    
  }

  /* 중간과정 추가 */
  /* 67% {
    transform: scale(2) rotate(540deg);
    border-radius: 10%;
    opacity: 1;
  } */
}
```

<br>

__2.Using emotion keyframes helper__
```jsx
import { css, keyframes } from '@emotion/react'

const bounce = keyframes`
// 0 20 53 80 100 지점에서 css
  from, 20%, 53%, 80%, to {
    transform: translate3d(0,0,0);
  }

// 40,43 지점에서 css
  40%, 43% {
    transform: translate3d(0, -60px, 0);
  }

//70지점
  70% {
    transform: translate3d(0, -15px, 0);
  }

//90지점
  90% {
    transform: translate3d(0,-4px,0);
  }
`
render(
  <div
    css={css`
      animation: ${bounce} 1s ease infinite;
    `}
  >
    some bouncing text!
  </div>
)
```
- animation: `keyframes설정 변수` 시간 시간함수 반복횟수; 로 설정한다.


<br>

# Typescript 
> emotion은 @emotion/react , @emotion/styled 를 위한 타입스크립트 정의가 있습니다, 이 정의는 css 프로퍼티들의 타입을 추론할 수 있게 합니다.

## @emotion/react
__Typescript에서 css prop사용을 위한 tsconfig.json 세팅__
```jsx
"jsx": "react-jsx",
"jsxImportSource":@emotion/react"
```
- 위 세팅만으로 타입스크립트에서 object style, template literal방식의 css prop설정이 가능합니다.
- 또한 ts에선 인텔리센스 활용 및 에러체크를 위해 object style이 권유됩니다.
```jsx

import { css } from '@emotion/react';

const titleStyle = css({
                       ^ Argument of type 'boxSizing: 'bordre-box';' is not assignable [...]
  boxSizing: 'bordre-box', // Oops, there's a typo!
  width: 300,
  height: 200,
});
```


### 출처:
[Emotion 공식문서](https://emotion.sh/docs/introduction)
