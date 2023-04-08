# react-responsive 라이브러리
> 미디어 쿼리를 사용하여 반응형 웹 앱 개발을 도와주는 라이브러리

__install__
1. npm install react-responsive
2. npm install @types/react-responsive (`타입스크립트 사용시`)

<br>



## react-responsive 사용예
` const isMobile = useMediaQuery({ maxWidth: DeviceSize.mobile });`  

위와같이 useMediaQuery()훅을 설정객체(break point등을 명시)와 함께 호출하면
명시한 bp 구간에 해당하는 경우 해당한다면(true),아니라면(false)의 boolean값을 리턴해준다.

<br> 

### 예시
```jsx
import React from "react";
import { useMediaQuery } from "react-responsive";
import styled from "styled-components";
import { Logo } from "../logo";
import { Accessibility } from "./accessibility";
import { NavLinks } from "./navLinks";
import { DeviceSize } from "../responsive";
import { MobileNavLinks } from "./mobileNavLinks";

const NavbarContainer = styled.div`
  width: 100%;
  height: 60px;
  box-shadow: 0 1px 3px rgba(15, 15, 15, 0.13);
  display: flex;
  align-items: center;
  padding: 0 1.5em;
`;

const LeftSection = styled.div`
  display: flex;
`;

const MiddleSection = styled.div`
  display: flex;
  flex: 2;
  height: 100%;
  justify-content: center;
`;

const RightSection = styled.div`
  display: flex;
`;

export function Navbar(props) {
  const isMobile = useMediaQuery({ maxWidth: DeviceSize.mobile });
// 뷰포트 width가 DeviceSize.mobile 이하에서 true값을 리턴

  return (
    <NavbarContainer>
      <LeftSection>
        <Logo />
      </LeftSection>
      <MiddleSection>{!isMobile && <NavLinks />}</MiddleSection>
      <RightSection>
        {!isMobile && <Accessibility />}
        {isMobile && <MobileNavLinks />}
      </RightSection>
    </NavbarContainer>
  );
}
```
- useMediaQuery훅을 호출하여 BP에 따라 리턴되는 boolean값으로 조건부렌더링을 구현한다.

- 디렉토리구조: `pages > page > components ( index.ts 및 구성요소 ) `


<br>

### 출처:
- [codesandbox](https://codesandbox.io/s/condescending-rgb-x4br4n?file=/src/components/navbar/index.jsx)
- [react-resposive-공식문서](https://www.npmjs.com/package/react-responsive)

