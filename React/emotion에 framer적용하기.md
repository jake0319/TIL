
# emotion + framer-motion 적용하기 
- emotion은 css-in-js방식의 css library
- framer-motion은 css animation 라이브러리
__이 둘을 함께 사용하려면 어떻게 해야할까?__

<br>

## 불만족스러웠던 코드 
```jsx
const StyledButton = styled('div')`
  height: 48px;
  margin: 0;
  border: none;
  cursor: pointer;
  display: inline-flex;
  justify-content: center;
  align-items: center;
  position: relative;
  font-weight: 600;
  outline: none;
  padding: 0 30px;
  border-radius: 4px;
  background-color: #5184f9;
  color: white;
  min-width: 150px;
`;

render(
  <motion.button
    whileHover={{ scale: 0.85 }}
    transition={{ duration: 0.5 }}
    style={{ background: 'transparent', border: 'none' }}
  >
    <StyledButton>Hello There</StyledButton>
  </motion.button>
);
```
- `StyledButton`컴포넌트의 style을 지정하고,options객체(whileHover,transition,style..) 를 prop으로으로 전달한 framer-motion제공 컴포넌트
`motion.button`로 다시 래핑해주는 불필요한 이중래핑 발생..

<br>

## 개선한 코드
```jsx
const StyledButton = styled(motion.button)`
  height: 48px;
  margin: 0;
  border: none;
  cursor: pointer;
  display: inline-flex;
  justify-content: center;
  align-items: center;
  position: relative;
  font-weight: 600;
  outline: none;
  padding: 0 30px;
  border-radius: 4px;
  background-color: #5184f9;
  color: white;
  min-width: 150px;
`;

render(
  <StyledButton whileHover={{ scale: 0.85 }} transition={{ duration: 0.5 }}>
    Hello There
  </StyledButton>
);
```

- styled() function에는 컴포넌트를 전달할 수 있다.
  - 그렇다면 `styled(motion.button)`형태로 전달해볼까? => 성공

- styled로 컴포넌트 생성시 motion컴포넌트를 할당하여 생성하면 불필요한 래핑과정이 발생하지 않는다

<br>

### 출처:
- [emotion + framer-motion 적용하기](https://blog.maximeheckel.com/posts/framer-motion-emotion/)
