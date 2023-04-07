# React의 `lazy()와 <Suspense>`

1. lazy()는 리소스가 많이 드는 무거운 컴포넌트의 비동기적 렌더링을 위해 사용


2. Suspense 컴포넌트로 lazy컴포넌트를 wrapping하는 경우
fallback={ lazy렌더링 전까지 보여줄 컴포넌트 } 를 제공할 수 있음 .

```jsx
import {lazy , Suspense} from 'react';
const Admin = lazy( ()=> import('./components/Admin');
//Admin컴포넌트는 lazy컴포넌트가 되었다!

<Suspense fallback={<TemporalComp/>}>
	<Admin/>
</Suspense>
```

- 리턴값이 import구문인 콜백함수를 넣어 lazy()함수를 호출하면 
해당 컴포넌트는 lazy컴포넌트가 되어 비동기적으로 렌더링된다.
- 비동기적으로 렌더링되는 경우 폴백컴포넌트가 필요하므로 아래와 같이 Suspense 컴포넌트로 래핑해준 뒤, fallback컴포넌트의 제공이 필요하다.
- lazy로 불러오는 컴포넌트는 lazy외부에서 별도의 import할 필요가없음(내부에서 import)


<br>

### Code Splitting
>React의 코드 스플리팅은 큰 규모의 애플리케이션에서 페이지 로드 속도를 향상시키기 위해 사용하는 기술임. 코드 스플리팅을 사용하면 애플리케이션의 모든 코드를 초기 로드 시점에 불러오는 것이 아니라, 
필요한 코드만 불러와 사용할 수 있으며 ,이는 초기 로드 시간을 단축시켜 애플리케이션의 성능을 향상시키는 데 도움이 됨.
React에서 코드 스플리팅을 구현하는 방법은 여러 가지가 있지만 React.lazy()와 Suspense를 사용하는 방법이 대표적임.

<br>

### 코드스플리팅의 단점
>코드 스플리팅은 애플리케이션의 성능을 향상시키는 데 중요한 역할을 합니다, 그러나 코드 스플리팅을 지나치게 사용하면 코드의 유지보수성이 떨어질 수 있습니다.
따라서 코드 스플리팅을 사용할 때는 적절한 수준에서 사용하는 것이 중요합니다.


<br>

### react-error-boundary 라이브러리
- 코드스플리팅 과정에서 발생하는 예외처리는 `react-error-boundary` 라이브러리로 손쉽게 처리가 가능하다.

1. import ErrorBoundary from 'react-error-boundary';
2. ErrorBoundary컴포넌트로 suspense컴포넌트를 감싸준 후 
FallbackComponent prop에 에러 발생시 보여줄 컴포넌트를 넣어준다. 

### ErrorBoundary컴포넌트의 prop
1. FallbackComponenp prop : 에러발생시 렌더링할 컴포넌트
2. `onReset` prop: ErrorBoundary컴포넌트의 prop으로 오류가 해결되어 컴포넌트를 다시 렌더링 할 때 호출되는 함수
3. `onError`prop: ErrorBoundary컴포넌트의 prop으로 에러 발생시 실행될 함수를 넣어줌 
  - 해당 prop에 할당되는 함수는 resetErrorBoundary를 호출할 수 있음.
4. `resetErrorBoundary 함수`: (오류가 발생한 상태를 초기화하고) FallbackComponent가 아닌 ErrorBoundary의 자식컴포넌트를 다시 렌더링합니다.
  - 에러 초기화후 원래 감쌋던 Suspense컴포넌트를 다시 렌더링(원래보여주려던거 보여줌)
  - onReset prop에 할당되는 에러해결후 함수속에서 호출될 수 있음

```jsx
<Route path="admin" element={
  <ErrorBoundary 
	FallbackComponent={ErrorFallback} //사용자 정의 
	onReset={ ()=>navigate('/')} //에러 해결시 동작하는 함수로  '/'라우트로 이동
  >
	<Suspense fallabck={<SkeletonAdmin/>}>
	  <Admin/>   // lazy로 불러오는 컴포넌트 
	</Suspense>
  </ErrorBoundary>
/>
```


- 코드스플리팅시 에러바운더리 처리가 필요(에러발생시 보여줄 컴포넌트 정의 및 후속처리) ex) 에러발생시 호출할 함수 및 동작 정의 
- 위코드는 코드스플리팅 도중 에러 발생시 '/'라우트로 리다이렉트시키는 코드.

<br> 

>위 방식의 코드스플리팅+에러바운더리처리는 코드 재사용성을 감소시키므로 모든 부분에 적용하지 말고 무거운 컴포넌트의 렌더링에만 부분적으로 적용하는 것이 좋다.

<br>
<br>

### 출처:
- [React Lazy Load Code to Load Faster | React Code Splitting Tutorial](https://www.youtube.com/watch?v=nS5qbSJLGx8)
