# :yum:React

- React는 페이스북에서 개발, 관리하는 UI 라이브러리
- 장점 : 가상 돔을 통해 UI 자동 업데이트
  - *UI = render(state)*

<br>

## create-react-app 

리액트로 웹 애플리케이션을 만들기 위한 환경 제공

```
npx create-react-app my-app
cd my-app
npm start
```

- http://localhost:3000
- `npm start` : 개발자 모드
- `npm run` : 사용자 모드

<br>

<br>

## React Template code

```react
import React, { Component } from 'react';

class 이름 extends Component {
    render() {
        return (
        	<div></div>
        );
    }
}

export default MyIntro;
```

- 모든 클래스는 `render()` 함수가 있어야 사용자에게 어떻게 그려질지 알려줌
- `export` : 외부의 App등에서 myintro 상속받아 사용 가능함
- **Class로 선언**하면 새로운 함수를 선언하는게 용이해서 이 방법을 더 추천
- 모든 return 키워드에는 단하나의 element가 와야함

<br>

```react
function MyIntro() {
    return (
        <div>

        </div>
    );
```

- function의 형태로 선언 가능

<br>

<bR>

## JSX

- JavaScript를 확장한 문법

<br>

### 규칙

1. JSX를 사용하는 스크립트 파일은 상단에서 React 라이브러리 import 해야함

   ```react
   import React from 'react';
   ```

2. 컴포넌트 생성

   ```react
   function App() {
   
   }
   
   class App extends Component() {
   
   }
   ```

   - 내부에서 JSX를 반환

3. 최상위 태그는 꼭 1개여야 함

4. `Fragment`빈 태그

<br>

<br>

## React 스타일 적용

```react
import React, { Component } from 'react';
import './App.css';
import {Fragment} from 'react';

class App extends Component {
  render() {
    const css = {
      color: 'white',
      background: 'purple',
      padding: '20px',
      fontSize: '45px'
    };
    const name = "Dongsun";
    return (
      <div className='App-header'>	//1.
        <div style={css}>Hello, {name}</div>	//2.
      </div>
    );
  }
}
export default App;
```

#### Style 지정하는 두가지 방법

1. 외부로부터 지정 : `className`
2. 내부에서 변수로 지정 : `style`

<br>

<br>

# 전통적인 방식 VS 단일 페이지 애플리케이션

![single page application multi page application에 대한 이미지 검색결과](https://dotcms.com/blog/images/spa-vs-mpa-graphic.png?language_id=1)

### SPA (Single page Application)

- 최초 요청시 **서버**에서 첫 페이지를 처리하고 그 후 라우팅을 **클라이언트**에서 처리
- 전통적인 방식은 페이지 전환 때마다 렌더링 결과를 서버에서 받아서 화면이 깜빡거림
- 단일 페이지 애플리케이션은 렌더링을 클라이언트에서 처리하기에 **자연스럽게 동작**함

<br>

<br>

## React 기능

![리액트 부모 컴포넌트 props에 대한 이미지 검색결과](https://user-images.githubusercontent.com/6733004/45532511-48d9b100-b82f-11e8-8f3d-6611900edc02.png)

### Props

- 부모 컴포넌트가 자식 컴포넌트에게 데이터를 전달할 때 호출
- `app.js` : 부모 컴포넌트 
- props 값은 `this.`키워드를 이용해 사용
  - Class가 아니라 Function으로 선언시 사용 불가능! this 자체가 존재하지 않기 때문

### State

- 하나의 컴포넌트 내부에서 상태 정보를 저장하기 위해 사용
- 동적인 데이터 처리

<br>

<br>

## Props 사용하기

### 1. 외부 컴포넌트 만들기 (MyIntro.js)

```react
import React, { Component } from 'react';

class MyIntro extends Component {
    render() {
        return (
            <div>
                안녕하세요, 제 이름은 React 입니다.
            </div>
        );
    }
}
export default MyIntro;
```

<br>

### 2. 부모 컴포넌트 지정 (App.js)

- index.html에 렌더링되어 보여짐

```react
import React, { Component } from 'react';

class MyIntro extends Component {
    render() {
        const css = {
            color: 'purple',
            fontSize : '40px'
        };
        return (
            <div style={css}>
                안녕하세요, 제 이름은 React 입니다.
            </div>
        );
    }
}
export default MyIntro;
```

- `<MyIntro />` : 태그화시켜서 MyIntro에서 렌더링된 결과가 삽입됨

<br>

### 3. Props 사용

#### (1) Class 사용

#### App.js

```react
import React, { Component } from 'react';
import MyIntro from './Myintro';


class App extends Component {
  render() {
    const a = '이동선';
      
   const card = {
      name: 'React',
      email: 'email@naver.com',
      phone: '010-2222-3333'
    };
      
    return (
      <MyIntro b={a} card={card}/>
    );
  }
}
export default App;
```

- `b`이라는 변수를 props에 담아 전달

<br>

#### MyIntro.js

```react
import React, { Component } from 'react';

class MyIntro extends Component {


    render() {
        const css = {
            color: 'purple',
            fontSize : '40px'
        };
        return (
            <div style={css}>
                안녕하세요, 제 이름은 <b>{this.props.b}</b> 입니다.
            </div>

			<div style={css}>
                안녕하세요, 제 이름은 <b>{this.props.card.name}</b> 입니다. <br>
                </br>
                제 이메일은 <b>{this.props.card.email}</b>이며
                제 전화번호는 <b>{this.props.card.phone}</b>입니다.<br></br>
                
            </div>
        );
    }
}
export default MyIntro;
```

<br>

<br>

#### (2) Function 사용

#### MyIntro2.js

```react
import React, { Component } from 'react';

const MyIntro2 = ({card}) => {
    const css = {
        color: 'chocolate',
        fontSize : '50px'
    };
    return (
        <div style={css}>
                안녕하세요, 제 이름은 <b>{card.name}</b> 입니다. <br>
                </br>
                제 이메일은 <b>{card.email}</b>이며<br></br>
                제 전화번호는 <b>{card.phone}</b>입니다.<br></br>
                
        </div>
    )
}

export default MyIntro2;
```

<br>

<br>

## State 사용하기 (Counter 만들기)

### 함수 만들기

- 보통 `handle`이란 이름을 붙임

- function / 화살표 연산자 사용

  ```react
  Home = ({match}) => {
  
  }
  ```


<br>

### Counter.js

```react
import React, { Component } from 'react';

class Counter extends Component {
    state = {
        count: 100
    }

    //handleClick = function() {
    handleIncrease = () => {
        this.setState({
            count: this.state.count + 1
        });
    }

    handleDecrease = () => {
        this.setState({
            count:  this.state.count - 1
        });
    }

    render() {
        return (
            <div>
                <h1>Counter</h1>
                <h2>{this.state.count}</h2>
                <button onClick={this.handleIncrease}>+</button>
                <button onClick={this.handleDecrease}>-</button>
            </div>
        );
    }
}

export default Counter;
```

- **state** : 
  - 컴포넌트 내에서 지속적으로 사용 가능
- **setState()**
  - State 고정값 바꾸는 함수

<br>

### App.js

```javascript
import React, { Component } from 'react';
import Counter from './Counter';


class App extends Component {
  render() {

    return (
      <div>
        <Counter />
      </div>
    );
  }
}
export default App;
```

<br>

<br>

## setState()

```react
handleChangeinfo = () => {
    // 1. this.state.info의 name을 변경
    this.setState({
        info: {
            name: 'Changed'
        }
    });

    // 2. this.state.info의 name을 변경 (전개 연산자)
    this.setState({
        info: {
            ...this.state.info,
            name: '2_Changed'

        }
    });
}
```

- 1번 : info의 데이터가 리셋됨
- 2번 : 전개 연산자를 사용해 나머지 데이터를 유지시킴

<br>

### 전개연산자 ...

```html
const array = [1,2,3,4,5];
undefined
const newArray = [6,7,8,9,10];
undefined
newArray.push(array);
6
newArray
(6) [6, 7, 8, 9, 10, Array(5)]
const newArray2 = [6,7,8,9,10];
undefined
newArray2.push(...array);
10
newArray2
(10) [6, 7, 8, 9, 10, 1, 2, 3, 4, 5]
```

-  **전개 연산자** : 가지고 있는 데이터를 풀어서 저장시켜줌
- 해당하는 값은 바꾸고 나머지는 **유지시키고 싶을 때 사용**

<br>

<br>

## props 사용해 Counter 제작

```react
 constructor(props) {
        super(props);
        console.log(this.props.init);
        this.state.count = this.props.init;
        console.log('call constructor');
    }
```

- Constructor 추가해 count 초기값을 init으로 변경 가능



<br>

<br>

## Component 생명 주기 메서드

모든 컴포넌트는 세 단계를 거친다. 각 단계에서 순차적으로 호출되는 메서드를 생명 주기 메서드라고 한다.

1. 초기화 단계
2. 업데이트 단계
3. 소멸 단계

<br>

```react
constructor(props) {
        super(props);
        console.log(this.props.init);
        // this.state.count = this.props.init;
        console.log('call constructor');
}

componentDidMount() {
    console.log('componentDidMount')
}

shouldComponentUpdate(nextProps, nextState) {
    console.log('shouldComponentUpdate');
    if (nextState.count % 5 === 0) return false;
    return true;
}

componentWillUpdate(nextProps, nextState) {
    console.log('componentWillUpdate');
}

componentDidUpdate(prevProps, prevState) {
    console.log('componentDidUpdate');
}
```

<br>

### 1. 초기화단계

- constructor
  - 제일 먼저 출력됨
- componentDidMount
  - 초기단계 마지막 함순



### 2. 업데이트 단계

클릭 등 이벤트 발생시

- componentWillUpdate
- componentDidUpdate

- **shouldComponentUpdate**
  - 렌더링 속도를 높여줌
  - 화면에 데이터를 그려주거나 안 그려주고 싶을 때 사용

<br>

<br>

## 에러 함수

```react
class Counter extends Component {
    state = {
        count: 100,
        info: {
            name: 'React',
            age: 10
        }
    }



{this.state.count == 3 && <ErrorObject />}
```

- componentDidCatch
  - 에러 발생시 호출되는 함수

<br>

#### Counter.js

```react
import React, { Component } from 'react';

const ErrorObject = () => {
    throw (new Error('에러 발생쓰'));       
}

class Counter extends Component {
    state = {
        count: 0,
        error: false,
        info: {
            name: 'React',
            age: 10
        }
        
    }

    componentDidCatch(error, info) {
        this.setState({
            error: true
        });
        console.log(error);
    }


    handleIncrease = () => { 
        this.setState({
            count: this.state.count + 1
        });
    }

    handleDecrease = () => {
        this.setState({
            count:  this.state.count - 1
        });
    }

    handleChangeinfo = () => {
        // 1. this.state.info의 name을 변경
        this.setState({
            info: {
                name: 'Changed'
            }
        });

        // 2. this.state.info의 name을 변경 (전개 연산자)
        this.setState({
            info: {
                ...this.state.info,
                name: '2_Changed'
                
            }
        });
    }

    render() {
        if (this.state.error) return (<h1>에러가 발생되었심덩.</h1>)

        return (
            <div>
                <h1>Counter</h1>
                <h2>{this.state.count}</h2>

                {this.state.count == 3 && <ErrorObject />}

                <button onClick={this.handleIncrease}>+ </button>
                <button onClick={this.handleDecrease}>- </button> 
                <button onClick={this.handleChangeinfo}>Changeinfo</button>
                <br></br>
                <h1>{this.state.info.name}/{this.state.info.age}</h1>
            </div>
        );
    }
}

export default Counter;
```

