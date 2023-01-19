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