### style 적용

태그에 직접 적용하거나, App.css에 작성하고 import해서 사용할 수 있음

하지만 동일한 class 이름으로 여러개 만들면 여러 컴포넌트에 동일한 style로 적용되기 때문에, 각기 적용하려고 할때는 module.css를 활용함

```jsx
1. {Component이름}.module.css 생성
.box {
    width: 200px;
    height: 50px;
    background-color: blue;
}

2. import후 적용
// component.js 파일
import styles from "./Hello.module.css"

<div className={styles.box}>Hello.modules.css에서 정의한 박스</div>
```

### Handling events

```jsx
// Hello.js
export default function HelloFunction() {

    // 함수 정의
    function showName() {
        console.log("kim");
    }
    function showAge(age) {
        console.log(age)
    }
    **function showText(event) {
        console.log(event.target.value)
    }    
        function showText(txt) {
        console.log(txt)
    }**

        return (
            {/* 버튼 클릭하면 함수 실행되도록 */}
      <button onClick={showName}>버튼</button>
      <button
         onClick={() => {
           showAge(30)
           }}
          >
           버튼
          </button>
      {/* text 입력하는대로 출력되도록(2가지버전) */}
      **<input type="text" onChange={showText} />
            <input type="text" 
                        onChange={event => {
                                const txt = event.target.value;
                                showText(txt);
                        }}
                     />**
```

### props

props(properties)를 활용하여 component간 데이터를 전달할 수 있음

```jsx
// App.js에서 Hello.js의 HelloFunction으로 props 전달

1. App.js
import HelloFunction from './component/Hello'

// 변수에 담아서 props 전달, 변수 이름은 일치하지 않아도됨
return (
    <HelloFunction age={10}/> 
  <HelloFunction age={20}/>
  <HelloFunction age={30}/>

2. Hello.js
export default function HelloFunction(**props**) {

    // props로 전달받은 데이터를 변경하기 위해서는 별도의 state를 생성함
    const [age, setAge] = useState(props.age); 
    const msg = age > 19 ? "성인 입니다" : "미성년자입니다"

    return (
        // props로 전달받은 데이터 그대로 출력할 수 있음
        <h4>{name} {props.age}세</h4>
    // state 변경하는 setAge함수를 통해 state 데이터를 변경함
        <h4>{name} {age}세 : {msg}</h4>
    <button onClick={() => {
        setName(name === "Mike" ? "Jane" : "Mike");
        setAge(age + 1);
        }}>나이/이름 변경</button>
```

### dummy 데이터 활용

json으로 받아온 dummy 데이터를 map method 사용해서 화면에 출력되도록 함

```jsx
// db/data.json
{
  "days": [
    {
      "id": 1,
      "day": 1
    },
    {
      "id": 2,
      "day": 2
    }  
  ],
  "words": [
    {
      "id": 1,
      "day": 1,
      "eng": "book",
      "kor": "책",
      "isDone": false
    },
    {
      "id": 3,
      "day": 2,
      "eng": "car",
      "kor": "자동차",
      "isDone": false
    },
    ]
}

// component/DayList.js
json 파일에 있는 모든 days 데이터 출력
import dummy from "../db/data.json"

export default function DayList() {
    return <ul className="list_day">
        {dummy.days.map(day => (
            <li key={day.id}>DAY {day.day}</li> // key 없으면 에러뜸
        ))}
    </ul>
}

// component/Day.js
json 파일에 있는 모든 words 데이터 중에서 filter 적용하여 일치하는 데이터만 출력
import dummy from "../db/data.json"

export default function Day() {
    // dummy.words 중에 day === 1인 것만 출력되도록
    const day = 1
    const wordList = dummy.words.filter(word => word.day === day); 

    return <>
    <table>
        <tbody>
            {wordList.map(word => (
                <tr key={word.id}>
                    <td>{word.eng}</td>
                    <td>{word.kor}</td>
                </tr>
            ))}
        </tbody>
    </table>
    </>
}
```

### Router

신규 페이지를 불러오지 않는 상황에서 각각의 url에 따라 선택된 데이터를 하나의 페이지에서 렌더링 해주는 라이브러리

```jsx
// 설치 명령어
npm install react-router-dom

// App.js에서 import
import { BrowserRouter, Route, Switch} from "react-router-dom"

return (
  <BrowserRouter> ****// 최상단을 감싸는 부분
    <div className="App">
        <Header /> // 페이지 이동과 상관 없이 항상 표시되는 부분(ex.nav bar)
      <Routes> ****// 페이지 이동하는 부분
        <Route path="/" element={<DayList />} />
        <Route path="/day/:day" element={<Day />} /> // url에 변수 삽입 가능
        <Route path="*" element={<EmptyPage />} /> // 해당 url 조건에 없는 경우
      </Routes>
    </div>
  </BrowserRouter>
  );
```

---

```jsx
// component/DayList.js
import { Link } from "react-router-dom"
import dummy from "../db/data.json"

export default function DayList() {

    return <ul className="list_day">
        {dummy.days.map(day => (
            <li key={day.id}>
                        // 링크는 <a> 대신 <Link> 사용, 백틱에 감싸서 url에 변수 삽입 
                <Link to={`/day/${day.day}`}>DAY {day.day}</Link> 
            </li>
        ))}
    </ul>
}
```

---

```jsx
// component/Day.js

import dummy from "../db/data.json"
import { useParams } from "react-router-dom";

export default function Day() {
    // useParams console에 찍으면 url 부분의 변수 {day : '1'} 형식으로 출력됨
    const day = useParams().day
    const wordList = dummy.words.filter(word => word.day === Number(day)); //day 값 문자열이므로 숫자로 변경

    return <>
    <h3>Day {day}</h3>
    <table>
        <tbody>
            {wordList.map(word => (
                <tr key={word.id}>
                    <td>{word.eng}</td>
                    <td>{word.kor}</td>
                </tr>
            ))}
        </tbody>
    </table>
    </>
}
```

---

```jsx
1. 뜻보기 버튼을 클릭하면 한국어 뜻이 보이면서, 버튼명이 "뜻 숨기기"로 변경되도록 구현
단어마다 속성을 관리해야하기 때문에, 새로운 Word component를 만들고 데이터를 담을 state를 사용함

2. 체크박스 선택하면 해당 단어(word)는 외운(json의 isDone 값) 단어로 처리되고, 해당 행이 회색으로 표시되도록 구현

// component/Day.js

// word 변수의 값을 word에 담아서 Word component로 전달
   <Word word={word} key={word.id} />

// component/Word.js

import { useState } from "react";

// props를 word라는 변수로 가져옴
export default function Word({ word }) {
    // 뜻을 보여줄지 말지 결정할 isShow 속성을 만들고, 초기값은 false로 설정
    const [isShow, setIsShow] = useState(false)
    // 단어 외웠는지 여부를 체크하는 변수를 props에 담긴 속성으로 가져옴
    const [isDone, setIsDone] = useState(word.isDone)

    // toggleShow 호출하면 isShow 값을 반대로 바꿔주는 함수가 실행됨
    function toggleShow() {
        setIsShow(!isShow)
    }
    // toggleDone 호출하면 isDone 값을 반대로 바꿔주는 함수가 실행됨
    function toggleDone() {
        setIsDone(!isDone)
    }

    return (
        // isDone 값이 true(외운 경우)이면 미리 정한 off td class(회색으로 변경)가 적용되도록
        <tr className={isDone ? "off" : ""}>
            <td>
                {/* isDone의 값이 true이면 체크표시가 되고, 체크박스 선택하면 isDone 속성값을 바꿔줌 */}
                <input type="checkbox" checked={word.isDone} onChange={toggleDone}/>
            </td>
            <td>{word.eng}</td>
            {/* isShow 값이 true인 경우만 단어뜻 노출함 */}
            <td>{isShow && word.kor}</td>
            <td>
                {/* 뜻 버튼을 클릭하면 isShow 속성을 반대로 바꿔주고, 값에 따라 버튼에 표시되는 이름 정해짐 */}
                <button onClick={toggleShow}>뜻 {isShow ? "숨기기" : "보기"}</button>
                <button className="btn_del">삭제</button>
            </td>
        </tr>
    )
}
```

### REST API

json 데이터를 method 활용하여 CRUD 하고, 이 데이터를 REST API를 통해 전달받음(useEffect 활용)

(Create : POST, Read : GET, Update : PUT, Delete : DELETE)

```jsx
// 설치 명령어
npm install -g json-server

// 사용 명령어
json-server --watch { 경로 ./src/db/data.json} --port {3000 포트번호}

// REST API로 데이터 받아오기

// component/DayList.js
import { useEffect, useState } from "react";

export default function DayList() {

    // API로 days 데이터 가져오기 위해 state 생성함, 초기데이터는 빈배열
    const [days, setDays] = useState([])

    // useEffect의 매개변수는 (함수, 변수)
    // useEffect는 DOM의 상태 변화가 있어서 렌더링이 된 이후에 매개변수의 해당 함수가 실행되도록 하는것
    // 이때 매개변수의 변수값이 변할때만 해당함수가 실행되고, 이 변수에 빈 배열을 넣으면 최초 한번만 실행됨
    useEffect(() => {
        fetch("<http://localhost:3000/days>")
        .then(res => {
            return res.json() //결과값을 json 형태로 반환
        })
        .then(data => {
            setDays(data) // 이 값을 days에 저장함
        })
    }, [])

// component/Day.js

export default function Day() {
    // url에 담긴 데이터를 day 변수에 담아옴
    const day = useParams().day
    // words state를 생성하고 초기값을 빈배열로 함
    const [words, setWords] = useState([])

    // const wordList = dummy.words.filter(word => word.day === Number(day)); 
    // 특정 day값에 맞는 데이터만 받아오도록 설정함
    useEffect(() => {
        fetch(`http://localhost:3000/words?day=${day}`)
        .then(res => {
            return res.json() //결과값을 json 형태로 반환
        })
        .then(data => {
            setWords(data) // 이 값을 words에 저장함
        })
    }, [day]) // url에 변수가 있어서 계속 바뀔때 해당 변수를 넣어줘야함
```
