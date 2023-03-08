# Framer-motion 정리
- framer자체 편의속성으로 css animation효과를 편리하게 부여할 수 있다.
- `element -> <motion.element>`로 변경하고 옵션으로 `props만` 전달하면 되어 사용이 편리하다.

<br>

## 설치 
`npm i --save framer-motion`| `yarn add framer-motion`

<br>

## 3. initial prop (:starting point state)
- `요소가 dom에 렌더링될 때 동작하는 애니메이션 시작 속성`
- initial prop은 `animate 종료시점` 속성이고, 
- animate prop은 `animate 시작시점` 속성임.

```jsx
return(<div>
     {pizza.base && (
        <motion.div className="next" initial={{x:'-10vw'}} animate={{x: 0}}>
          <Link to="/toppings">
            <button>Next</button>
          </Link>
        </motion.div>
      )}  
    </div>
    //&&: falsy값 아니면 맨 뒤에값 리턴(:pizzabase있으면 뒤에리턴)
)
```

## 4. animate props (:end point)
- 요소의 dom렌더링 후 동작하는 애니메이션의 endpoint 속성 

```jsx
import { motion } from 'framer-motion';
return ( <div>
<motion.h2 animate={{ fontSize: 50, color: '#ff2994'}}>
</div>
)
```
- h2태그에 옵션은 `객체` 형태로 전달하며 속성key값은 `camelCase로 작성한다`.
- `x:100  ,y: -100` 속성 : 현재 위치에서의 (+,-)좌표로의 이동
- `ratateZ: 180`, `scale`...등 적용가능

<br>

## 5. transition options
> 애니메이션 효과는 initial,animate로 설정하고  
`(duration:2 ,delay: 3, ease: easeInOut ... 등 )` 의 애니메이션을 컨트롤할 수 있는 효과는 `transition`에 옵션으로 부여한다.

<br>

```jsx
return(<div>
     {pizza.base && (
       <motion.div className="home container" 
    initial={{opacity: 0}}
    animate={{opacity: 1}}
    transition={{delay: 0.5, duration: 1.5}}>
      <h2>
        Welcome to Pizza Joint
      </h2>
      <Link to="/base">
        <button>
          Create Your Pizza
        </button>
      </Link>
    </motion.div>
      )}  
    </div>
    //&&: falsy값 아니면 맨 뒤에값 리턴(:pizzabase있으면 뒤에리턴)
)
```
- delay: initial효과 발현까지 최초 delay시간
- duration: initial~animate 애니메이션 동작 시간

<br>

### transition속성: type prop
- 애니메이션 동작 특수효과 동작 방식을 설정한다.
- type: 'tween'  // 'spring' (스프링효과)...

<br>

### transition속성: stiffness prop
__stiffness(스프링상수)__
- stiffness `값이 클 수록 탄성이 증가함`(default:100, no unit)
- 값이 클 수록 spring이 많이 통통 튄다고 보면 됨.

<br>

## 6.Hover animation(마우스 hover시 애니메이션)
```jsx
<motion.button
whileHover= {{ scale: 1.1,
  textShadow: "0px 0px 8px rgb(255,255,255)",
  boxShadow: "0px 0px 80x rgb(255,255,255)",
}}>Create Your Pizza
<motion.button>
```
- `whileHover` 옵션은 마우스hovering시 발동할 animate속성이라고 생각하면 됨.
- 따라서 hovering시 동작할 animation의  transition속성 역시 별개로 부여할 수 있음

```jsx
<motion.li  
    whileHover={{ scale: 1.3 , originX: 0, color: '#f8e112'}}
    transition={{type: 'spring', stiffness:" 300"}}>
    <span className={spanClass}>{ base } </span>
</motion.li>
```
- scale옵션으로 인해 text가 왼쪽으로 이동하면서 커지는 문제는 `originX: 0`로 원래 위치에서 움직이지 않도록 해결할 수 있음.

<br>

## 7. Variants변수로 animation객체 분리하기
1. `Variant`는 우리의 initial,animate,transition 등의 prop 객체들을 
별도의 참조가능한 객체로 분리할 수 있게 해준다. (일종의 변수라고 보면 되고
변수로 분리하여 좀 더 가독성 좋은 코드로 만들어줌)

<br>

2. Variant객체의 initial,animate속성은 alias로 작성 가능하며, motion컴포넌트에 적용시 반드시 별도로 명시해야함( initial='hidden' animate='visible', exit='exit 등..)
// `alias propagation to the child components`

<br>

3. transition은 visible속성에 nesting한 객체로 부여한

<br>

```jsx
const containerVariants = {
  hidden: { //hidden, initial등.. 자유로운 작명 가능
    opacity: 0,
    x: '100vw'
  },
  visible: { //animate효과를 visible로 작명
    opacity:1,
    x:0
    transition: {
      type: 'sping',
      delay: 0.5
    }
  }
}
```
- 위에서 작명해준 hidden(initial),visible(animate)는 declare해야 인식가능.
- transition은 visible객체의 하위 프로퍼티로 이동시킬 수 있음.

<br>

4. parent element에서 Variants 사용시
`initial='hidden' animate='visible'`설정값은 자동으로 자식요소에 전파(propagate)된다. (따라서 자식요소에 variants를 적용할 때 이미 설정한 alias설정은 무시할 수 있음.)
// cf.설정하지 않은 alias는 명시해야함

<br

## 8.Variant(part2)

```jsx
import React from 'react';
import { motion } from 'framer-motion';
const Order = ({ pizza }) => {

  const containerVariants = {
    hidden: { //hidden, initial등.. 자유로운 작명 가능
      opacity: 0,
      x: '100vw'
    },
    visible: { //animate효과를 visible로 작명
      opacity:1,
      x:0,
      transition: {
        type: 'spring',
        delay: 0.5,
        when: "beforeChildren"
        // child 컴포넌트의 animation보다 부모 animation을 보다 먼저 일으킨다.
      }
    }
  }

  const childVariant = {
    hidden: {
      opacity: 0 ,
    }
    visible: {
      opacity: 1,
    }
  }

  return (
    <motion.div className="container order"
    variants={containerVariants}
    initial="hidden"
    animate="visible"
    >
      <h2>Thank you for your order :)   
      </h2>
      <p>You ordered a {pizza.base} pizza with:</p>
      <motion.div variants={childVariant}>
      {pizza.toppings.map(topping => <div key={topping}>{topping}
      </div>)}
      </motion.div>
    </motion.div>
}
export default Order;
```
- 부모요소에 Variant의 alias delaration이 선행됐으므로 자식요소에는 필요치 않다.


### orchestration property(transition 세부속성) 선후관계 지정
 
```jsx
const containerVariants = {
    hidden: { //hidden, initial등.. 자유로운 작명 가능
      opacity: 0,
      x: '100vw'
    },
    visible: { //animate효과를 visible로 작명
      opacity:1,
      x:0,
      transition: {
        type: 'spring',
        mass: 20,  //spring옵션 : 질량(클수록느리다)
        damping: 8, //spring옵션: 클수록( )
        // childVariant의 transition보다 먼저 일으킨다.
        when: "beforeChildren",
        // when: 'afterChildren' 자식 애니메이션 이후에 동작 
        staggerChildren: 0.4
      }
    }}
```
__transition Orchestration 조화속성__
1. when(in transition): 자식요소와의 관계에서 언제 애니메이션이 시작될지 정하는 옵션 
  - 'beforeChildren' | 'afterChildren'
2. staggerChildren(in transition): 자식요소 애니메이션 발동 전 딜레이두기
  - 0 ~ 1

<br>

__transtion type:'spring'; 하위옵션__
1. transition_ mass: 0 ~ 1  //spring옵션 : 질량(값이 클수록 ocillation하는 속도가 느리다.)
2. transition_ damping: 9, //spring옵션: 작을수록 진동을 많이한다. (0 :무한진동 )

<br>

## 9. keyframes 
```jsx

const Home = () => {

  const buttonVariants = {
    visible: {
      x: [0,-20,20,-20,20,0], //keyframes를 배열로 전달 
      transition: {delay: 2}
    },
    hover: { //whileHover대신사용하는 variants속성 
        scale: [1,1.1 , 1 , 1.1 , 1, 1.1 ,1]
        textShadow: "0px 0px 8px rgb(255,255,255)",
        boxShadow: "0px 0px 80x rgb(255,255,255)",
    }
  }
  return (
    <motion.div
      className="home container"
      initial={{ opacity: 0 }}
      animate={{ opacity: 1 }}
      transition={{ delay: 0.5, duration: 1.5 }}
    >
      <h2>Welcome to Pizza Joint</h2>
      <Link to="/base">
        <motion.button
          variants={buttonVariants}
          initial="hidden"
          animate="visible"
          whileHover="hover" //적용시 declare 필수 
        >
          Create Your Pizza
        </motion.button>
      </Link>
    </motion.div>
  );
};

export default Home;
```
- x좌표를 배열형식의 `keyframes [0,-20,20,-20,0]`처럼 전달하므로써 좌우진동효과를 줄수 있음.

<br>
 
## 10. Repeating Animations
> keyframes는 결국 지정한 값만큼만 반복하고 멈춘다는 문제가 있음, 이것을
계속해서 반복시키려면 어떡해야할까? : `transition: {yoyo: n | yoyo: Infinity}`

<br>

```jsx
const buttonVariants = {
    hover: { //whileHover대신사용하는 variants alias속성 
        scale: 1.1,
        textShadow: "0px 0px 8px rgb(255,255,255)",
        boxShadow: "0px 0px 80x rgb(255,255,255)",
        transition: {
          duration: 0.3,
          yoyo: 10 , //hover시 1=>1.1 10회 반복 
          //Infinity: 무한반복
        }
    }
  }
```
- transition하위속성의 `yoyo를 이용하자.`
  - yoyo: n(반복횟수) | Infinity(무한반복)

<br>

## 11. Animate Presence (요소 존재여부 조작하기)
- 단순히 컴포넌트를 useState와 setTimeout을 활용한 조건부렌더링으로 Dom에서 지울 수 도(pop out) 있지만,
- 화면 바깥으로 사라질 시 동작하는 애니메이션은  `AnimatePresence` 컴포넌트를 활용하자
  - smooth하게 뷰포트에서 slide out하는 등의 효과를 부여할 수 있음 

```jsx
const Home = () => {
  //state선언 후  조건부렌더링
  let [showTitle, setShowTitle] = useState(true);
  //4초 뒤에 true-> false
  setTimeout(() => {
    setShowTitle(false);
  }, 4000);

  render(
    //exit모션을 정의할 motion컴포넌트를 래핑한다.
    <AnimatePresence>
      { showTitle && (<motion.h2 transition={{duration:1}}
      exit={{y : -1000}}>Thank you for your order
      </motion.h2>)
      }</AnimatePresence>
      )}
```
1. `Animate Presence`로 exit모션(나갈 때 동작 애니메이션)을 정의한 motion.h2컴포넌트를 래핑한다.
2. `motion.h2` 컴포넌트에 exit프롭 값으로 설정객체{y: -1000}를 전달한다.
3. transition dureation을 주어 천천히 out시킬 수도 있음.

<br>


## 12. Animation Routes (라우팅 변화에 따라 exit 애니메이션 발동시키기 :라우트 변화시 렌더링엘리먼트가 dom에서 사라질 때 exit animation이 발동 )
1. App.js에서 Switch(Routes)를 AnimatePresence로 감싸준다.
- AnimatePresence는 wrapping한 dom element의 존재유무를 감지해 exit animation을 발동시켜주는 역할을 한다.
> Dom요소가 아닌 Switch(Routes)라우터 컴포넌트를 감싸주는 경우 AnimatePresence는 exit애니메이션이 언제 발동해야하는지 
알 수 없다(라우트가 언제 변하는지 알 수 없다), 따라서 우리는 라우트가 변하는 것을 AnimatePresence가 알 수 있도록 아래와같이 처리해야함. 

2. App.js의 `<Switch>`(`<Routes>` at .v6) 에 `location={location} key={location.key}` prop값을 제공하면 AnimatePresence는 라우트의 변화를 감지할 수 있음.
  - `const location = useLocation()` //useLocation: 현재 페이지 경로를 가지고 있는 함수

3. 이제 dom에서 해당 라우트의 element가 사라질 때 exit animation이 발동된다.

4. `<AnimatePresence>`의 `exitBeforeEnter` 속성은 직전의 컴포넌트의 exit animation이 완료 되어야 다음 컴포넌트가 enter될 수 있도록 한다.(step2->order로 이동시 order의 animation이 중간에 시작되는 것을 방지: 애니메이션 동작 전후 관계를 설정해 매끄러운 ux를 만들어줌) 

```jsx
import React from 'react';
import { Link } from 'react-router-dom';
import { motion , AnimatePresence } from 'framer-motion';

const backdrop = {
  hidden: { opacity: 0},
  visible: {opacity: 1 }, //안보이다가->보이는설정
}

//showModal prop이 참일때만 아래 motion.div를 렌더링하고 false로 바뀌면 //안보이다가 보이는 설정 (opacity 0->1 )
const Modal = ({showModal}) => {
  return(
    <AnimatePresence exitBeforeEnter>
      { showModal && (
        <motion.div className='backdrop'
        variants={backdrop}
        initial="hidden"
        animate="visible"
        >
        </motion.div>
      )}
    </AnimatePresence>
  )
 }

 export default Modal;
 ```
 - 라우트 변경에 따른 exit animation발동 뿐만아니라, 단일 컴포넌트의 presence 감지에도 사용할 수 있음.


## 13. Modal Animation(part1): `on-off모달 만들기`
```jsx
const Order = ({ pizza, setShowModal}) => {

  // setShowModal값이 바뀔 때마다 useEffect발동
  useEffect( ()=> {
    setTimeout(()=> {setShowModal(true)},5000)}
    ,[ setShowModal ])
    
  return (
    <motion.div
      className="container order"
      variants={containerVariants}
      initial="hidden"
      animate="visible"
      exit="exit"
    >
      <h2>Thank you for your order </h2>

      <motion.div variants={childVariant}>
        <p>You ordered a {pizza.base} pizza with:</p>
        {pizza.toppings.map((topping) => (
          <div key={topping}>{topping}</div>
        ))}
      </motion.div>
    </motion.div>
  );
};

```
- App.js에서 setShowModal prop을 Order로 전달.
- useEffect + setTimeout으로 setShowModal(true) 발동 
- 5초후 화면 modal효과 발동 

<br>

# 14. Modal Animation(part2)

- `<AnimatePresence onExitComplete={()=>{setShowModal(false)}}`
- `onExitComplete`옵션은 `exit animation` 종료시마다 콜백함수를 동작시켜주는 옵션`임. (모든 라우트 전환 과정에서 렌더링 element dom에서 사라질때 exit 애니메이션효과 발동 -> 애니메이션 종료 후 callback실행)

- 즉, `start again` 버튼 클릭후 `"/"`path로 돌아갈 때 라우트 전환이 이뤄지고, exit animation이 발동 된 뒤 setShowModal(false)호출로 modal을 종료시켜준다.



<br>


## 15. Animating SVG's

> svg는 path(fill,d속성)로 구성된다. 하나의 path는 하나의 연결을 말함.
- d: path를 만들어주는 좌표 
- fill: 면의 색상을 지정해주는주는 속성
- pathLength: must be between `0 ~ 1`, svg의 progress를 말함.
```jsx
import React from 'react';
import { motion } from 'framer-motion';

const pathVariants = {
  hidden: { 
    opacity: 0,
    pathLength: 0  //svg progress속성 (0~1)
  },
  visible: {
    opacity:1,
    pathLength:1,
    transition: { duration: 2 , ease: 'easeInOut'}
  }
 }
const Header = () => {
  return (
    <header>
      <div className="logo">
        <motion.svg className="pizza-svg" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"variants={svgVariants}
        initial="hidden"
        animate="visible"
        >
          <motion.path
            fill="none"
            d="M40 40 L80 40 C80 40 80 80 40 80 C40 80 0 80 0 40 C0 40 0 0 40 0Z"
            variants={pathVariants}
          />
          <motion.path
            fill="none"
            d="M50 30 L50 -10 C50 -10 90 -10 90 30 Z"
            variants={pathVariants}
          />
        </motion.svg>
      </div>
      <motion.div className="title" initial={{ y: -250,}} animate={{ y: -10, }}>
        <h1>Pizza Joint</h1>
      </motion.div>
    </header>
  )
}
```

<br>

# 16. Creating a Loader(로더만들기)
```jsx
import React from 'react';
import { motion } from 'framer-motion';

const loaderVariants = {
  animationOne: {
    x: [-20,20],
    y: [0,-30],
    transition: {  //x,y속성에 따로 transition적용하기 
    x: { 
      yoyo: Infinity,
      duration: 0.5,
    },
    y: {
      yoyo: Infinity,
      duration: 0.25,
    }
  }
  }
}
const Loader = () => {

  return(
    <>
    <motion.div className='loader'
    variants={loaderVariants}
    animate="animationOne"
    >
    </motion.div>
    </>
  )
}

export default Loader;
```
- transition을 적용하고 싶은 `개별 속성을 nesting한 key값으로 다시 명시`해주면된다. 그러면 별개의 속성에 별개의 transition을 적용할 수 있음.

- 해당 loader는 애니메이션의 종점에서 cordinate가 변하지 않고 계속 반복동작하는 속성이기때문에 initial속성이 필요 없음

<br>

# 17. useCycle Hook
> framer-motion에서 사용가능한 hook으로서 ,user-interaction에 따라  animation type전환 및 다중 애니메이션 적용에 효과적임.

```jsx
// Variants에 다중 애니메이션 설정
const loaderVariants = {
  animationOne: {
    x: [-20,20],
    y: [0,-30],
    transition: { 
    x: { 
      yoyo: Infinity,
      duration: 0.5,
    },
    y: {
      yoyo: Infinity,
      duration: 0.25,
    }}
  },
  animationTwo: {
    y: [0,-40],
    x: 0, 
    transition: {
      y: { 
        yoyo : Infinity,
        duration: 0.25,
        ease: 'easeOut'
      }
    }
  }
  }
const Loader = () => {
  // useCycle에 적용할 애니메이션(1,2,3..)들을 문자열로 할당
  // cycleAnimation()호출시 animation효과를 1->2->3 전환시켜준다..
  const[animation,cycleAnimation]= useCycle("animationOne","animationTwo")
  return(
    <>
    <motion.div className='loader'
    variants={loaderVariants}
    // animate="animationOne"
    animate={animation}
    >
    </motion.div>
    <div onClick={()=>cycleAnimation()}>Change Loader</div>
    </>
  )
}
export default Loader;
```

<br>

# 18. Dragging Items & Wrap up (아이템을 draggable하게 바꾸기)
- `<motion.element>` 에 단순히 `drag`속성만 추가하면 해당 요소는 드래그해서 이동 가능한 요소가 됨. 
- `dragConstraints={{left:0,top:0,right:0,bottom:0}}` drag요소의 원복 위치를 지정해줄 수 있음
- `dragElastic={0~1}` drag탄성계수: 클수록 탄성이 커져서 원복이 빨라짐(고무줄효과)

<br>
<br>

### What I Learned
- `animate` prop( starting point animation )
- `initial` prop( ending point animation )
- `whileHover` (animate settings in hovering action)
- `transition` prop(ex: delay,duration, stiffness,type )
- `variants`prop settings (: transition(type,mass,damping | when,staggerChildren ect..) & alias(hidden,visible,exit..) propagation To child motion components)
- (`<AnimatePresence exitBeforeEnter>` & useLocation & exit prop ) using with Router-Dom
- Modal Animations (`<AnimatePresence onExitComplete(callback)>`)
- animating svg's (pathLength: 0~1)
- create loader  (cordinte keyframes, transition object keys nesting )
- useCycle hook(multi-animation) & cycleAnimation function
- drag options

<br>

### 출처:
[The net ninja: framer-motion_course](https://www.youtube.com/watch?v=2V1WK-3HQNk&list=PL4cUxeGkcC9iHDnQfTHEVVceOEBsOf07i)
