### 개념

web-app 만드는 라이브러리 (새로고침 없이도 페이지가 변함)

web-app의 장점 : 모바일앱으로 발행이 쉬움, 앱처럼 뛰어난 UX,

### 기본 명령어

```jsx
// 프로젝트 생성
npm install -g create-react-app
create-react-app {앱이름}

npx create-react-app {앱이름}

// 디렉토리 이동 및 실행
cd {앱이름}
npm start // index.js를 실행하게됨

// 기타
/* eslint-disable */ : 터미널창에 노란색 waring message 안뜸
```

### 초기 구조

- node_modules : 라이브러리들 집합소
- public : static 파일 보관소
- src : 소스코드 보관

```jsx
// index.js (index.js를 실행하여 페이지 렌더링함)
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App'; // App.js파일에서 App 컴포넌트 import
import reportWebVitals from './reportWebVitals';

// import된 App 컴포넌트 실행
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
// public/index.html 파일 내의 <div id="root">에 렌더링한다는 의미
  <React.StrictMode>
    <App /> 
  </React.StrictMode>
);
// React.StrictMode : 앱 내의 잠재적인 문제를 알아내기 위한 도구
// JSX(자바스크립트의 확장 문법) : <App/> 처럼 html 태그처럼 생겼지만 자바스크립트 형태의 코드로 변환함

// **App.js(메인페이지)**
import logo from './logo.svg';
import './App.css';

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p>
          Edit <code>src/App.js</code> and save to reload.
        </p>
        <a
          className="App-link"
          href="<https://reactjs.org>"
          target="_blank"
          rel="noopener noreferrer"
        >
          Learn React
        </a>
      </header>
    </div>
  );
}

export default App;
```

### 작동 원리

리액트는 view에서 상태 변화가 생기면 바뀐 상태에 따라 DOM을 어떻게 업데이트할지 정하는 것이 아니라 **가상의 DOM(Virtual DOM)**을 사용함

가상의 DOM(Virtual DOM) : 브라우저에 실제로 보여지는 DOM이 아니라 메모리에 가상으로 존재하는 DOM, 단순한 JS객체이기 때문에 브라우저에서 DOM을 보여주는 것보다 속도가 빠름

view에서 상태 변화가 생기면 업데이트가 필요한 곳의 UI를 가상의 DOM을 통해 렌더링하고, 리엑트 내부의 엔진을 통해 실제 브라우저에서 보여지고 있는 DOM과 비교한 후 차이가 있는 곳을 감지해서 실제 DOM에 패치를 시킴

### JSX 문법

```jsx
**1. 태그에 class를 부여할때 <div className="클래스이름"> 으로 정의함**

**2. 서버에서 받아오는 데이터를 쉽게 바인딩함 {변수명}, {함수}
     src, id, href 등의 속성에도 {변수명, 함수} 가능**

// 변수, 함수 불러오기
function App() {  
  const title = '변수로 정한 제목'; // let
    function 함수() {
    return 100
  }

  return (
    <div className="App">
      <h4>{ title }</h4>
            <h4> { 함수() } </h4>
    </div>

// 이미지 불러오기
import logo from './logo.svg'; // 동일 depth에 위치한 logo.svg파일을 logo라는 이름으로 import

function App() {

  return (
    <div className="App">
      <img src={ logo }/>
    </div>

**3. style 속성 넣을때 style = { object 자료형 } 으로 정의함, style 속성명은 camelCase로**
<div style={ { color : 'blue', fontSize : '90px' } }>네비게이션바</div>
```

### state

정의 : 변수 대신 사용하는 데이터 저장 공간

state에 변수 저장하면 state 변경되어도 HTML이 자동으로 재렌더링되어서 app처럼 보여짐 >> 자주 바뀌는 데이터 state에 넣기

동일한 component여도 state는 개별적으로 관리됨

```jsx
// state로 데이터 바인딩하기
import React, { useState } from 'react';

function App() {

  let [글제목, 글제목변경] = useState('글제목입니다'); // [데이터, 데이터 변경 함수] 형식, 데이터는 array, object도 가능

    return (
        <h4> { 글제목 } </h4>

cf. ES6 destructuring 문법 : var [a, b] = [10, 100]; a=10, b=100으로 저장됨
array나 object에 있던 자료를 변수에 쉽게 담고 싶을 때 활용

// state로 '좋아요' 기능 구현
import React, { useState } from 'react';

function App() {
    let [좋아요, 좋아요변경] = useState(0); // 좋아요 초기값 0이기 때문에

    return (
        <h4> { 글제목[0] } <span onClick={ ()=>{ 좋아요변경(좋아요 + 1) } }>👍</span> {좋아요} </h4>
        // onClike = {함수명}으로 작성하고 함수를 따로 정의해도 됨

// button 클릭했을때 state 변경되도록 만들기
1. 하드코딩
function App() {
    function 제목바꾸기(){
    글제목변경( ['글제목4', '글제목5', '글제목6']);
  }
    return (
        <button onClick={ 제목바꾸기 }> 버튼 </button>

2. deep copy 활용

function 제목바꾸기(){
    var newArray = [...글제목];  // var newArray = 글제목 : 복사가 아니라 값 공유
    newArray[0] = '글제목4';
    글제목변경( newArray ); // 새로 만들어진 리스트를 state의 변경함수에 인자로 넣어줌
  }
```

### Component

어떠한 속성(props)들을 입력으로 받아서 그에 맞는 리액트 element를 생성해 return함

ex. 붕어빵 기계에서 붕어빵 틀(component), 재료(props), 완성된 붕어빵(element)

return 안에 html 태그 넣을때 코드의 가독성을 위해서 component로 따로 만들고, 이를 return 안에 작성하면 됨, return안에 있는 태그는 하나로 묶여야함(<>, </>)

component로 만들면 좋은 것 : 반복 출현하는 html 덩어리들, 자주 변경되는 html UI들(재렌더링이 많이 요구되는 것들)

component 이름은 항상 대문자로 시작해야함(소문자인 경우 html 태그로 인식함)

```jsx
return
        <Modal></Modal> // 이름 첫글자는 대문자로
    </div>
  );
}

function Modal(){
  return (
    <div className="modal">
      <h4>제목</h4>
      <p>날짜</p>
      <p>상세내용</p>
    </div>
  )
}

// 1. 함수 component
function Welcome(props) {
    return <h1>안녕, {props.name}</h1>
}

// 2. 클래스 component
class Welcome extends React.Component {
    render() {
        return <h1>안녕, {this.props.name}</h1>
    }
}
```
