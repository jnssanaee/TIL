# React 정리

## React
* props는 속성을 나타내는 데이터이다.
* React가 사용자 정의 컴포넌트 발견 시 JSX어트리뷰트를 해당 컴포넌트에 단일 객체로 전달함. 이 객체를 'props'라고 한다.
* 컴포넌트 이름은 항상 대문자로 시작한다.
* 컴포넌트는 자신의 state를 자식 컴포넌트에 props로 전달할 수 있다.
* React의 이벤트는 소문자 대신 카멜케이스를 사용한다.
* React에선 false를 반환해도 기본 동작을 방지할 수 없다. 반드시 preventDefault를 명시적으로 호출해야 한다.
* Key는 어떤 항목을 변경, 추가, 삭제할지 식별하는 것을 돕는다. 

### React 탄생배경
* 상태가 바뀔 때 다 날려버리고, 처음부터 모든 걸 새로 만들어서 보여준다 로 시작
* 실제 DOM을 모두 다시 그리면 속도가 많이 느리기에, 가상DOM을 이용하여 가능케 함
* 가상DOM이란 메모리에 가상으로 존재하는 DOM으로, 그냥 자바스크립트 객체다. 상태 업데이트 시 비교알고리즘을 통해 변경된 곳을 감지 후 실제 DOM에 패치한다.

### 컴포넌트
* input, select, textarea와 같이 state를 거쳐 보여주는 값이 반영되는 컴포넌트를 Controlled Component라 부른다.

### props
```javascript
//App.js
<Hello name="react" color="red" />


//Hello.js
function Hello(props){
    console.log(props); // {name: "react"}, 부모 컴포넌트에 선언한 속성 및 값이 객체형태로 들어있다.
    return <div style={{color: props.color}}>안녕하세요 {props.name}</div> // 중괄호가 2번인 이유는 자바스크립트 값이기 때문
}

//Hello.js (구조분해 할당으로 선언 시)
function Hello({ name, color }){
    return <div style={{color: color}}>안녕하세요 {name}</div>
                    // color 라고만 선언해도 됨
}

//출력화면
안녕하세요 react (빨간색 컬러)


/* 기본값(defaultProps)으로 선언하고 싶을 경우 */
//App.js
<Hello />

//Hello.js
Hello.defaultProps = {
    name: '이름없음'
}

//출력화면
이름없음


/* 컴포넌트 태그 사이 값(children)을 조회하고 싶을 경우 */
//App.js
function App(){
    return (
        <Wrapper>
            <Hello color="pink" />
        </Wrapper>
    )
}

//Wrapper.js
function Wrapper({children}){
    return (
        <div>
            {children}
        </div>
    )
}

```

### 조건부 랜더링
```javascript
//App.js
<Hello isSpecial={true} /> // isSpecial만 선언해도 true로 간주

//Hello.js
Hello() {
    return (
        <div>
            {isSpecial ? <b>하다</b> : <b>안하다<b>} // 삼항연산자 (서로 내용이 다를 경우)
            {isSpecial && <b>*</b>} // &연산자 (단순히 true, false로 처리할 경우)
        </div>
    )
}

//출력화면
하다
*

```

### input 상태 관리
```javascript
//input.js
function input() {
    const [text, setText] = useState('');

    const onChange = (e) => { // e는 이벤트 객체
        console.log(e.target) // e.target은 이벤트가 발생한 input의 정보를 가지고 있다.
        console.log(e.target.value) // e.target.value는 input의 value 값

        setText(e.target.value)
    };

    const onReset = () => {
        setText('');
    };
    return (
        <input onChange={onChange} value={text} /> // value값을 설정해줘야 초기화 시 값이 날아간다.
        <button onClick={onRest}>초기화</button>
    )
}
```

### 여러 개 input 상태 관리하기
```javascript
function input() {
    const [inputs, setInputs] = useState({ // 객체형태의 값을 관리
        name: '',
        nickname: ''
    });
    const { name, nickname } = inputs; // 비구조 할당을 통해 추출

    const onChange = (e) => {
        const { name, value } = e.target; // 이벤트 발생의 name, value 추출
        setInputs({ // 리액트에서 객체 업데이트 시 기존 상태 복사 후 새로운 값을 덮어씌우고 새로운 상태로 설정 (이런걸 불변성을 지킨다고 한다.)
            // 불변성을 지켜줘야만 리액트에서 상태가 변경되었음을 감지할 수 있고, 이에 따라 필요한 렌더링이 발생하게 된다. 
            ...inputs, // spread연산자로 객체 그대로 복사해온다.
            [name]: value // 대괄호로 감쌀 시 name값이 무엇을 가르키냐에 따라 다른 key값이 변경됨, 없을 시 name 문자열 그대로 받아옴
                          // 여기서 [name]은 input의 name, nickname을 가리킨다.
        })
    };

    const onReset = () => {
        setInputs({
            name: '',
            nickname: ''
        })
    };
    return (
        <input name="name" placeholder="이름" onChange={onChange} value={name} />  //value는 inputs로 추출한 값
        <input name="nickname" placeholder="닉네임 " onChange={onChange} value={nickname} /> 
        <button onClick={onRest}>초기화</button>
        <div>
            {name} {nickname}
        </div>
    )
}

``` 

### 배열 항목 추가하기
```javascript
const onCreate = () => {
const user = {
    id: nextId.current,
    username,
    email
};

// 2가지 방법
setUsers([...users, user]); // spread 연산자를 이용하여 복사하고, 뒤에 추가되는 객체를 붙여 추가하는 방식
setUsers(users.concat(user)); // 기존 배열에 concat을 이용한 객체 추가 후 완성된 배열을 set하는 방식
// 주의사항 : users.push(user) push를 이용하여 직접 추가 시 업데이트가 안된다.

};

```

### 배열 항목 제거하기
```javascript
// App.js
const onRemove = id => {
    // filter가 user.id 가 파라미터로 일치하지 않는 원소만 추출해서 새로운 배열을 만듬
    // = user.id 가 id 인 것을 제거함
    setUsers(users.filter(user => user.id !== id));
};

// UserList.js
function User({ user, onRemove }) {
    return (
        <div>
            <b>{user.username}</b> <span>({user.email})</span>
            <button onClick={() => onRemove(user.id)}>삭제</button> 
                    {/* 
                        onclick 시 안에 함수를 호출한다는 의미 
                        이 함수는 id값을 파라미터로 받은 onRemove를 호출한다는 의미
                        onClick={onRemove(user.id)} 이렇게 선언하면 안됨. 컴포넌트 랜더링 시 호출되버림
                    */}
        </div>
    );
}

function UserList({ users, onRemove }) {
    return (
        <div>
            {users.map(user => (
                <User user={user} key={user.id} onRemove={onRemove} />
            ))}
        </div>
    );
}

```

### 배열 항목 수정하기
```javascript

const onToggle = id => {
    setUsers(
        users.map(user => // 불변성을 유지하며 배열을 업데이트 할때도 map함수 이용
            user.id === id ? { ...user, active: !user.active } : user // id값을 비교하여 같으면 active값을 반전
        )
    ); 
};

function User({ user, onRemove, onToggle }) {
  return (
    <div>
      <b
        style={{
          cursor: 'pointer',
          color: user.active ? 'green' : 'black'
        }}
        onClick={() => onToggle(user.id)}
      >
    </div>
  );
}

```

<br><br>

## Redux
리덕스(redux)는 자바스크립트를 위한 상태 관리 프레임워크이다.

### 사용하는 이유 or 장점  
1. 컴포넌트 코드로부터 상태 관리 코드를 분리할 수 있다.
2. 서버 렌더링 시 데이터 전달이 간편하다.
3. 로컬 스토리지에 데이터를 저장하고 불러오는 코드를 쉽게 작성할 수 있다.
4. 같은 상태값을 다수의 컴포넌트에서 필요로 할 때 좋다.
5. 부모 컴포넌트에서 깊은 곳에 있는 자식 컴포넌트에 상태값을 전달할 때 좋다.
6. 알림창과 같은 전역 컴포넌트의 상태값을 관리할 때 좋다.
7. 페이지가 전환되어도 데이터는 살아 있어야 할 때 좋다.

### 3가지 원칙  (설명은 아래)
1. 전체 상태값을 하나의 객체에 저장한다.
2. 상태 값은 불변 객체다.
3. 상태 값은 순수 함수에 의해서만 변경되어야 한다.

### 원칙 설명
- **하나의 객체에 전체 상태값을 저장한다.**  
  하나의 객체를 직렬화해서 서버와 클라이언트가 전체 상태값을 서로 주고 받을 수 있다.  
  최근 상태값을 저장 시 실행 취소(undo)와 다시 실행(redo)을 쉽게 구현할 수 있다.

- **상태값을 불변 객체(액션 객체)로 관리한다.**  
  리덕스의 상태값은 액션 객체와 dispatch 메서드를 호출하는 방법만 가능하다.  
  상태값은 dispatch 메서드가 호출된 순서대로 리덕스 내부에서 변경된다.  
  목적이 상태값 수정만이라면 상태값 직접 수정이 더 빠르지만,  
  이전 상태값과 비교하여 변경 여부를 파악할 땐 불변 객체가 훨씬 유리하다.  

- **순수 함수에 의해서만 상태값을 변경해야한다.**  
  리덕스에서 상태값을 변경하는 함수를 리듀서(reducer)라고 한다.  
  리듀서는 이전 상태값과 액션 객체를 입력받아 새로운 상태값을 만드는 순수 함수다.  
  > 순수 함수 : 외부의 상태를 변경하거나 인자의 상태를 직접 변경하는 부수 효과가 없는 함수  


### 주요 개념
상태값이 변경되는 과정은 아래와 같다. 
![image info](../images/react_redux.png)  
  
1. **액션(action)**  
   액션은 type속성값을 가진 자바스크립트 객체다. dispatch 메서드에 넣어 호출 시  
   리덕스는 위 그림의 과정을 수행한다. 
   액션 객체는 type 속성외 다른 원하는 속성값도 넣을 수 있다.
   ```javascript
    store.dispatch({ type: 'REMOVE_ALL' });  
    store.dispatch({ type: 'REMOVE', id: 123 })
   ```  
   <br>

    각 액션은 고유한 type 속성값을 사용해야하며, 충돌을 피하기 위해 아래와 같이 접두사를 붙이는 방법을 많이 사용한다.  
    ```javascript
    store.dispatch({ type: 'todo/REMOVE' })
    ```

2. **리듀서(reducer)**  
* 현재상태와 액션객체를 받아 새로운 상태를 반환한다.
* 리듀서는 액션이 발생했을 때 새로운 상태값을 만드는 함수다.
* 작성 예
   ```javascript
   function reducer(state = INIT_STATE, action) {
       switch (action.type) {
            case ADD: 
                return {
                    //...
                };
            case REMOVE: 
                return {
                    //...
                };
            default:
                return state;
       }
   }

   const INIT_STATE = { todos: [] };
   ```

3. **스토어**  
   스토어는 리덕스의 상태값을 갖는 객체다. 액션의 발생은 스토어의 dispatch 메서드로 시작된다.  
   스토어는 액션이 발생하면 미들웨어 함수를 실행하고, 리듀서를 실행해서 상태값을 새로운 값으로 변경한다.  
   그리고 사전에 등록된 모든 이벤트 처리 함수에게 액션의 처리가 끝났음을 알린다.  

<br><br>

## create-react-app
* create-react-app은 웹 어플리케이션을 만들기 위한 환경을 제공한다.  
* 바벨, 웹팩도 포함과 있으며, 테스트 시스템, HRM(hot-module-replacement), ES6+, CSS 후처리 등 거의 필수라고 할 수 있는 개발 환경도 구축해준다.
* index.html, index.js파일은 빌드 시 예약된 파일이므로 지우면 안된다.
* index.js로부터 연결된 모든 JS, CSS 파일은 src 폴더 밑에 있어야 한다.
  (src 폴더 바깥에 있는 파일 import 시 실패함)
* 이미지, 폰트 파일도 src폴더 밑에서 import 해야 브라우저 캐싱 효과를 볼 수 있다.
* npm run eject 선언 시 숨겨져 있던 내부 설정파일이 노출된다. (바벨, 웹팩 등의 설정을 변경할 수 있다.)

<br><br>

## Hook  
Hook은 함수 컴포넌트에서 state와 생명주기 기능을 '연동'할 수 있게 해주는 함수이다.

### Hook 2가지 원칙
* 최상위에서만 Hook 호출. 반복문, 조건문, 중첩된 함수 내에서 실행 X
* 리액트 함수 컴포넌트 내에서만 Hook 호출. (custom Hook내에선 가능)

### Context API
* 전역적으로 사용할 수 있는 값을 관리할 수 있다. (값은 함수, 라이브러리 인스턴스, DOM일 수도 있다.)
* createContext의 파라미터는 Context의 기본값을 설정할 수 있다.
    ```javascript
    const UserDispatch = React.createContext(null);
    ```
* 앱 제작 시 state를 위한 Context, dispatch를 위한 Context를 따로 생성하는 게 최적화에 좋다.

### useRef
* DOM 접근 시 사용하는 Hook
* 사용 예
    ```javascript
    // useRef 호출
    import React, { useRef } from 'react';

    // 객체 선언
    const nameInput = useRef();

    // 선택하고 싶은 DOM에 선언
    <input ref={nameInput} />

    // DOM 접근 및 이벤트
    nameInput.current.focus()
    ```

### useState
* 함수형 컴포넌트에서 상태를 관리할 수 있는 Hook
* 사용 예
    ```javascript
    // useState 호출
    import React, { useState } from 'react';

    // useState 선언
    const [number, setNumber] = useState(0);
    // [상태, 상태를 바꿔주는 함수] = useState(상태 초기값);

    // 함수 선언
    const onIncrease = () => {
        setNumber(number + 1); // 다음 상태는 선언하는 방식 : '현재 상태를 가져와서 +1 한 값을 넣겠다' 라는 의미
        setNumber(prevNumber => prevNumber + 1); // updater 함수(함수형 업데이트) 컴포넌트 최적화와 관계있음, 어떻게 업데이트 할 것이다 라는 로직의 함수가 들어갈 수도 있다. prevNumber는 관행상 쓰이는 단어이며, 이전상태를 의미한다.
    }

    //컴포넌트에 선언
    return (
        <>
            <h1>{number}</h1>
            <button onClick={onIncrease}+1</button>
        </>
    )
    ```

### useReducer
* reducer는 '상태를 업데이트 하는 함수'
* '액션'이라는 객체를 기반으로 상태를 업데이트한다. (액션객체란 업데이트할 때 참조하는 객체)
* 상태 업데이트 로직을 컴포넌트 밖으로 분리 가능 (다른 파일에서 작성 후 불러와서도 사용 가능)
* 사용 예
    ```javascript
    // useReducer 호출
    import React, { useReducer } from 'react';

    // 리듀서 함수 생성 (상태업데이트 로직이 컴포넌트 밖에 있다.)
    fucntion reducer(state, action) {
        switch (action.type) {
            case 'INCEREMENT': 
                return state + 1;
            case 'DECEREMENT': 
                return state - 1;
            default:
                return state;
       }
    }

    // 리듀서 사용
    function Counter() {
        const [number, dispatch] = useReducer(reducer, 0);
        //const [현재상태, 액션을 발생시키는 함수] = useReducer(리듀서 함수, 초기값);

        const onIncrease = () => {
            dispatch({
                type: 'INCEREMENT'
            })
        }

        const onDecrease = () => {
            dispatch({
                type: 'DECEREMENT'
            })
        }
    }

    //컴포넌트에 선언
    return (
        <>
            <h1>{number}</h1>
            <button onClick={onIncrease}+1</button>
            <button onClick={onDecrease}-1</button>
        </>
    )
    ```
### useCallback
* 함수를 재사용 시 사용하는 Hook
* 사용 예
    ```javascript
    // useCallback 호출
    import React, { useCallback } from 'react';

    // useCallback 선언 (기존함수를 userCallback으로 감싼다.)
    const onRemove = userCallback(id => {
        setUsers(users.filter(user => user.id ! == id));
    }, [users]) 
    // users가 바뀔 때만 함수가 생성됨
    // 두번째 인자는 꼭 선언! 미선언 시 최신상태가 아닌 초기랜더링 시 상태를 참조한다.
    ```

### useMemo
* 성능 최적화를 위해 사용하는 Hook, 의존성이 변경됐을때만 다시 계산
* 사용 예
    ```javascript
    // useCallback 호출
    import React, { useMemo } from 'react';

    // useMemo 선언
    const count = useMemo(() => countActiveUsers(users), [users]);
    //첫번째 함수는 users가 바뀔때에만 연산한다. 그렇지 않을 경우 이전에 계산된 값을 사용
    ```


### customHook
정리하자 (쉽게사용하기 위함?)

### dispatch 
정리하자

<br><br>

## React 최상위 API

### React.memo
* 컴포넌트의 리랜더링 성능을 최적화할 수 있다.
    ```javascript
    // React.memo 선언
    export default React.memo(CreateUser); // props가 바뀐 경우에만 리랜더링 발생!
    ```

<br><br>

## 기타

### 컴포넌트 리랜더링 확인 방법
* React Developer Tools 설치  (크롬 확장도구)
* 개발자도구 - react 탭 - 톱니바퀴 - 'Highlight Updates' 체크
![image info](../images/react_hightlight.gif)  