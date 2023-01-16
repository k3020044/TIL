# 1/13 리액트 정리

### 데이터 UPDATE & DELETE & POST

기존에는 데이터를 변경해도 새로고침을 하거나 페이지를 이동하면 바뀐 값이 저장되지 않고 새로 렌더링됐지만, state에 담긴 데이터를 REST API 통신을 통해서 변경할 수 있음

```jsx
**1. update**
// component/Word.js

export default function Word({ word }) {
		const [isDone, setIsDone] = useState(word.isDone)
// toggleDone 호출하면 isDone 값을 반대로 바꿔주는 함수가 실행됨
    function toggleDone() {
        // setIsDone(!isDone)
        fetch(`http://localhost:3001/words/${word.id}`, {
            method : 'PUT',
            headers : {
                'Content-Type' : 'application/json', // 보내는 데이터 타입이 json
            },
            body : JSON.stringify({ // 수정을 위한 정보 입력, json 형태로 변환
                ...word, // 기존 데이터에 isDone 값을 바꿔서 담음
                isDone : !isDone,
            }), 
        }).then(res => {
            if (res.ok) { // 응답 정상이면
                setIsDone(!isDone) // state 바꾸는 함수 실행
            }
        })
    }

**2. delete**
// component/Word.js

// word 변수 중복되기때문에 word를 'w'로 받아오고, 이를 word 변수에 저장
export default function Word({ word: w }) {
    const [word, setWord] = useState(w)

		// 삭제 버튼 누르면 데이터 del 함수 실행
    function del() {
        if(window.confirm("삭제하시겠습니까?")) { // 윈도우 창으로 메세지 나오고 확인 누르면 이후 실행
            fetch(`http://localhost:3001/words/${word.id}`, {
                method: "DELETE",
            }).then(res => {
                if (res.ok) { // 응답 정상이면 해당 word의 id 속성값을 setWord 함수 통해 0으로 변경
                    setWord({ id: 0 })
                }
            })
        }
    }
    // id값이 0이면 null을 return하고 db 파일에서 데이터 사라짐
    if (word.id === 0){
        return null
    }

**3. post
3-1. word 데이터 추가하기**
// component/CreateWord.js 생성

import { useRef } from "react"
import { useNavigate } from "react-router" 명령어 : npm install react-router-dom
import useFetch from "../hooks/useFetch"

export default function CreateWord() {
    const days = useFetch("http://localhost:3001/days")
    const navigate = useNavigate() // 화면 이동할때 사용
    
    // submit은 누르면 새로고침되기 때문에 preventDefault
    function onSubmit(e) {
        e.preventDefault()
        console.log(engRef.current.value) // 입력한 데이터 출력됨

        fetch(`http://localhost:3001/words/`, {
            method : 'POST',
            headers : {
                'Content-Type' : 'application/json', // 보내는 데이터 타입이 json
            },
            body : JSON.stringify({ // 수정을 위한 정보 입력, json 형태로 변환
                day : dayRef.current.value,
                eng : engRef.current.value,
                kor : korRef.current.value,
                isDone : false // isDone 속성 값은 전부 false로 
            }), 
        }).then(res => {
            if (res.ok) { // 응답 정상이면
                alert("생성이 완료됐습니다") // 경고창 알림  
                navigate(`/day/${dayRef.current.value}`) // 생성완료되면 해당 day의 단어페이지로 이동함       
            }
        })
    }
    
    // useRef = DOM에 접근하기 위해 사용하는 hook
    const engRef = useRef(null)
    const korRef = useRef(null)
    const dayRef = useRef(null)

    return (
        <form onSubmit={onSubmit}>
            <div className="input_area">
                <label>Eng</label>
                {/* input 받는 값을 useRef와 연결함 */}
                <input type="text" placeholder="초기값" ref={engRef} /> 
            </div>
            <div className="input_area">
                <label>Kor</label>
                <input type="text" placeholder="초기값" ref={korRef} />
            </div>
            <div className="input_area">
                <label>Day</label>
                <select ref={dayRef}>
                    {days.map(day => (
                        <option key={day.id} value={day.day}>
                            {day.day}
                        </option>
                    ))}
                </select>
            </div>
            <button>저장</button>
        </form>
    )
}

// component/Header.js 링크 연결
<Link to="/create_word" className="link">단어 추가</Link>

// App.js에 등록
<Routes>
   <Route path="/" element={<DayList />} />
   <Route path="/day/:day" element={<Day />} />
   <Route path="/create_word" element={<CreateWord />} />

3-2. day 데이터 추가하기
// component/CreateDay.js
import useFetch from "../hooks/useFetch"
import { useNavigate } from "react-router"

export default function CreateDay() {
    const days = useFetch("http://localhost:3001/days")
    const navigate = useNavigate() // 화면 이동할때 사용

    function addDay() {
        fetch(`http://localhost:3001/days/`, {
            method : 'POST',
            headers : {
                'Content-Type' : 'application/json', // 보내는 데이터 타입이 json
            },
            body : JSON.stringify({ // 추가 날짜는 현재 있는 개수 + 1, json 형태로 변환
                day : days.length + 1
            }), 
        }).then(res => {
            if (res.ok) { // 응답 정상이면
                alert("생성이 완료됐습니다") // 경고창 알림  
                navigate(`/`) // 첫페이지로 가도록       
            }
        })   
    }
    return (
        <div>
            <h3> 현재 일수 : {days.length}일 </h3>
            <button onClick={addDay}>날짜 추가</button>
        </div>
    )
}
```

### 기타 기능

```jsx
1. 데이터 렌더링 중일때 '로딩중' 메세지 화면에 뜨도록 구현
// component/DayList.js

export default function DayList() {
	// days 데이터를 받아오는 페이지인데 개수가 0이라는 것은 아직 렌더링 중이라는 의미
	if (days.length === 0) { 
        return <span>로딩중..</span>
    }

2. 새로운 데이터 추가하고 아직 저장 중일때 계속 클릭 못하도록 방지 & '저장중' 뜨도록 구현
// component/CreateWord.js
export default function CreateWord() {
	const [isLoading, setIsLoading] = useState(false) // state 새로 생성하고 기본값 false
	
	function onSubmit(e) {
        e.preventDefault()
				// isLoaging == false이면, true로 바꾸고 함수 실행(클릭 계속하는거 방지)
				if (!isLoading) {
            setIsLoading(true)
            fetch(`http://localhost:3001/words/`, {
						...
				}).then(res => {
            if (res.ok) { // 응답 정상이면
                alert("생성이 완료됐습니다") // 경고창 알림  
                navigate(`/day/${dayRef.current.value}`) 
                setIsLoading(false) // 완료되면 다시 초기값으로 변경       
            }
	return (
		<button
       style={{
          opacity : isLoading ? 0.3 : 1, // 로딩중이면 버튼 흐리게 출력되도록
          }}
         >
          {isLoading ? "저장중.." : "저장"} // 로딩중이면 button의 value가 저장중으로 구현
    </button>
```