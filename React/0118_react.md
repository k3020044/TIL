# 1/18 리액트 정리(hooks)

### Hooks

리액트의 state와 생명주기 기능에 갈고리(hook)를 걸어 원하는 시점에 정해진 함수를 실행되도록 하는 것

최상위 레벨에서만 호출해야함(ex. 반복문, 조건문 안에서는 안쓰도록함)

리액트 함수 컴포넌트에서만 호출해야함

1. **useState** : state를 활용하기 위한 훅
   
    state를 사용해서 값이 바뀔때마다 재렌더링이 되도록 구현할 수 있음
   
    const [변수명, set함수명] = useState(초기값)

2. **useEffect** : side effect(ex. 서버에서 데이터를 받아오거나 수동으로 DOM을 변경하는 등의 작업)를 수행하기 위한 훅
   
    클래스 컴포넌트에서 제공하는 생명주기 함수(componentDidMount, componentDidUpdate, componentWillUnmount)와 동일한 기능을 하나로 통합해서 제공함
   
    useEffect(effect 함수, 의존성 배열), 의존성 배열 안에 있는 변수 중 하나라도 값이 변경될때 effect 함수가 실행됨

3. **useMemo** : memoized value를 리턴하는 훅, 연산량이 높은 작업을 반복하는 것을 방지하기 위해 사용
   
    파라미터로 create 함수와 의존성 배열을 받음
   
    렌더링이 일어나는 동안 실행됨, 의존성 배열을 넣지 않을 경우 렌더링이 일어날 때마다 함수가 실행되기 때문에 의존성 배열을 넣어야함
   
   ```jsx
   const memoizedValue = useMemo(
       () => {
       // 연산량이 높은 create 함수 작업
       return computeExpensiveValue(의존성 변수1, 의존성 변수2)
    }, [의존성 변수1, 의존성 변수2] // 의존성 변수가 변할 때 다시 함수 호출 되는것임
   )
   ```

4. **useCallback** : useMemo 훅과 유사한 역할을 하지만 값이 아닌 함수를 반환한다는 차이가 있음
   
    컴포넌트 내에 함수를 정의한다면 매번 렌더링이 일어날 때마다 함수가 새로 정의되지만, useCallback 훅을 사용하여 특정 변수의 값이 변한 경우에만 함수를 다시 정의하도록 해서 불필요한 반복 작업을 없애줌
   
    useCallback(function, dependencies) = useMemo(() ⇒ function, dependencies)

5. **useRef** : 레퍼런스(특정 컴포넌트에 접근할 수 있는 객체)를 사용하기 위한 훅, 레퍼런스 객체를 반환
   
    현재 레퍼런스하고 있는 엘리먼트를 의미하는 .current 속성을 가짐
   
    렌더링될 때마다 항상 같은 ref 객체를 반환하고, 컴포넌트가 마운트 해제되기 전까지는 계속 유지됨

---

### Material UI

리액트 UI 라이브러리 중 하나

```jsx
// 설치 명령어
npm install @material-ui/core @material-ui/icons (--save --legacy-peer-deps)

// HomeScreen.js
import React from 'react';
import { Box, Card, CardActionArea, Typography } from '@material-ui/core';
import TouchAppIcon from '@material-ui/icons/TouchApp';
import { useStyles } from '../styles';
import Logo from '../components/Logo';
export default function HomeScreen(props) {
  const styles = useStyles();
  return (
    <Card>
      <CardActionArea onClick={() => props.history.push('/choose')}>
        <Box className={[styles.root, styles.red]}>
          <Box className={[styles.main, styles.center]}>
            <Typography variant="h6" component="h6">
              Fast & Easy
            </Typography>
            <Typography variant="h1" component="h1" className={styles.bold}>
              Order <br />
              & pay
              <br />
              here
            </Typography>
            <TouchAppIcon fontSize="large"></TouchAppIcon>
          </Box>
          <Box className={[styles.center, styles.green]}>
            <Logo large />
            <Typography variant="h5" component="h5" className={styles.footer}>
              Touch to start
            </Typography>
          </Box>
        </Box>
      </CardActionArea>
    </Card>
  );
}
```

---

### 조건부 렌더링(conditional rendering)

```jsx
// 로그인/로그아웃 여부에 따라 다른 메세지 보이도록

// 1. 메세지 function 정의
function UserGreeting(props) {
    return <h1>다시 오셨군요</h1>
}

function GuestGreeting(props) {
    return <h1>회원가입을 해주세요</h1>
}

// 2. 조건에 따라 렌더링되로록 function 정의
function Greeting(props) {
    const isloggedIn = props.isLoggedIn
    if (isLiggedIn) {
            return <UserGreeting />
    }
        return <GuestGreeting />
    }

// 로그인/로그아웃 여부에 따라 다른 버튼이 보이도록

// 1. button component 생성
function LoginButton(props) {
    return (
        <button onClick={props.onClick}> 로그인 </button>
    )
}

function LogoutButton(props) {
    return (
        <button onClick={props.onClick}> 로그아웃 </button>
    )
}

// 2. 로그인 여부 정보 담을 state 및 정보 변경하는 함수 생성
function LoginControl(props) {
    const [ isLoggedIn, setIsLoggedIn ] = useState(false) // 기본 default 값은 false    

    const handleLoginClick = () => { // isLoggedIn 값을 true로 바꿔줌
        setIsLoggedIn(true)
    }

    const handleLogoutClick = () => { // isLoggedIn 값을 false로 바꿔줌
        setIsLoggedIn(false)
    }
} 

    let button // 변수 생성
    if (isLoggedIn) { // isLoggedIn 값이 true이면 button 변수에 LogoutButton 컴포넌트 넣어줌
        button = <LogoutButton onClick={handleLogoutClick} /> // 엘리먼트 변수: 변수에 컴포넌트를 대입
    }  else {
    button = <LoginButton onClick={handleLoginClick} />
    }

    return (
        <div>
            <Greeting isLoggedIn={isLoggedIn} />
            {button} // 엘리먼트 변수에 담은 컴포넌트가 출력됨
        <div/>
    )
}
```

- truthy : true, {}, [](empty object,array), “0”, “false”
- falsy : false, 0, -0, 0n, ‘’, “”(empty string), null, undefined, NaN

---

### Modal 구현

```jsx
// components/Modal.js
import React from "react";
import '../App.css'; 

function Modal(props) {

function closeModal() {
    props.closeModal();
    }

    return (
      <div className="Modal" onClick={closeModal}>
        <div className="modalBody" onClick={(e) => e.stopPropagation()}>
          <button id="modalCloseBtn" onClick={closeModal}>
            ❌
          </button>
          {props.children}
        </div>
      </div>
    );
  }     
export default Modal;

// App.js
function App() {
  // 매장식사, 포장 여부 모달
  const [modalIsOpen, setModalIsOpen] = useState(false);

    return (
  <div>
    <Header/>
    <EasyImage/>
    <div>
          {/* 매장식사, 포장 여부 모달 */}
      <input type="button" value="결제" onClick={() => setModalIsOpen(!modalIsOpen)}/>
      {modalIsOpen && (
        <Modal closeModal={() => setModalIsOpen(!modalIsOpen)}>
          <div id="rectangle">
            <div className="box green"> 먹고 가기 </div> 
            <div className="box green"> 가져 가기 </div>
          </div>
        </Modal>
      )}

// index.css
.modalBody {
  position: absolute;
  width: 100%;
  height: 60%;
  padding: 40px;
  text-align: center;
  background-color: rgb(255, 255, 255);
  border-radius: 10px;
  box-shadow: 0 2px 3px 0 rgba(34, 36, 38, 0.15);
}

#modalCloseBtn {
  position: absolute;
  top: 15px;
  right: 15px;
  border: none;
  color: rgba(0, 0, 0, 0.7);
  background-color: transparent;
  font-size: 20px;
}

#modalCloseBtn:hover {
  cursor: pointer;
}
```