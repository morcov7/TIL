## :pig2:Flux

- 이전에는 Flux라는 프레임워크
- 단방향 데이터 전달

<br>

### MVC

1. Model

- 비즈니스 로직 - 기능
- 데이터와 직접 연결됨

2. View

- 화면에 보여지는 부분

3. Controller

- 모델과 뷰 연결

- 문제점
  - 비동기적인 변경 요청 대응 어려움
  - 하나의 변경이 다수의 변경을 야기
  - 디버그 어려움

<br>

<br>

<br>

# :pig2:Redux

자바스크립트를 위한 상태 관리 프레임워크

- 컴포넌트 코드와 상태 관리 코드 분리
- Flux에서 일부를 개선

<br>

### 세 가지 원칙

1. 전체 상탯값을 하나의 객체에 저장
2. 상탯값은 불변 객체다
3. 순수 함수에 의해 상탯값이 변경되어야함

<br>

<br>

### (1) Action

- 상태 변경 위해 만드는 포맷
- Action creater = **type** + **payload**(상탯값)
- 생성된 메시지는 고대로 Action에게 돌려줌

<br>

### (2) Store

- 애플리케이션의 모든 **상태**과 관련 **로직** 저장소
- 상태 트리를 유지
- Action이 전달되면서 Reducer라는 개념이 등장

<br>

### (3) Reducer

- Root reducer는 Action으로부터 데이터를 받고 Store를 업데이트해줌

<br>

### (4) View

- Smart Component
  - Action처리, Dumb component 관리
- Dumb Component
  - DOM 요소 관리
  - Action에 직접 의존성 X → 다른 애플리케이션에서 재사용 가능

<br>

### (5) The View Layer Binding

- Store & View 역할 → **react-redux**
- provider component
- connect
- selector

<br>

### (6) The Root Component

- Store 생성, Binding 등 종합적인 역할 수행

<br>

<br>

### React Redux

- 상태관리 로직을 다른 파일과 분리해서 관리
- Action이라는 개념으로 데이터 요청

<br>

<br>

## Redux를 사용한 Application 만들기

> 1. Reducer
>    - Biz Logic (데이터 처리, 상태 처리)
>    - Root reducer에 Reducer를 추가
>    - src/reducers/reducer-books.js
>    - src/reducers/reducer-active-books.js
> 2. src/index.js
>    - reducer를 가지고 store를 생성
>    - App.js 실행시 store 지정
> 3. 사용자의 요청 작업 (이벤트 등)
>    - src/actions/index.js 등록 -> selectBook
>    - Action -> type(BOOK_SELECTED), payload(상태 값)
> 4. 사용자 view (or Contatiner) component
>    - src/containers/book-list.js
>    - src/containers/book-detail.js
> 5. Component하고 Reducer(Store) 연결
>    - mapStatetoProps(state)
>    - mapDispatchToProps(dispatch)
>    - connect() 함수 이용
>      - ex) connect (mapStatetoProps) (mapDispatchToProps)

<br>

```
npm install redux react-redux
```

<br>

## 1. Reducer

#### reducer-books.js

```react
// src/reducers/index.js
export default function() {
    return [
        {title: 'Javacrtipt', page: '157'},
        {title: 'Docker and Kubernetes', page: '72'},
        {title: 'Java Programming', page: '572'},
        {title: 'Microservice Architecture', page: '782'},
        {title: 'Spring Framework', page: '456s'}
    ]   
}
```

- 네 권의 책을 반환시켜주는 로직

<br>

#### Index.js

````react
// src/reducers/index.js
import { combineReducers } from 'redux';
import BookReducer from './reducer-books';
import ActiveBook from './reducer-active-book'

const rootReducer = combineReducers({
    books: BookReducer,
    activeBook: ActiveBook,
});

export default rootReducer;
````

- **combineReducers**
  - 가지고 있는 여러 reducer 들을 관리해주는 Root Reducer

<br>

#### reducer-active-book.js

````react
// src/reducers/reducer-active-book.js

export default function(state = null, action) {
    switch(action.type) {
        case 'BOOK_SELECTED':
            return action.payload;
        default:
            return state;
    }
}
````

- 모든 action중에서 필요한 action만 switch로 분류
- `state` 초기값을 null로 지정해서 원하는 action이 아닐시 null을 반환

<br>

## 2. Containers

#### book-list.js

```react
// src/containers/book-list.js

import React, { Component } from 'react';
import { connect } from 'react-redux';
import { bindActionCreators } from 'redux';
import { selectBook } from '../actions/index';

class BookList extends Component {
    renderList() { // -> bookitem
        return this.props.books.map( book => {
            return ( //Booklist 배열 -> 반복처리
                <li 
                    key={book.title}
                    onClick={() => this.props.selectBook(book)}>
                    {book.title}
                </li>  
            )
        });
    }

    render() {
        return (
            <ul>
                {this.renderList()}
            </ul>
        );
    }
}

//상태값 mapping
function mapStateToProps(state) {
    return {
        books: state.books
    }
}

// 액션 mapping
function mapDispatchToProps(dispatch) {
    return bindActionCreators({
        selectBook: selectBook
    }, dispatch);
}
    
export default connect(mapStateToProps, mapDispatchToProps)(BookList);
```

- **Connect** () ()
  - 갖고 있는 state와 component를 연결 
  - 객체값과 booklist를 연결시켜줌
- ` onClick={() => this.props.selectBook(book)}>`
  - 클릭시 selectBook 실행
- **bindActionCreators**
- **mapDispatchToProps**
  - Action 연결

<br>

#### book-detail.js

````react
// src/containers/book-detail.js

import React, { Component } from 'react';
import { connect } from 'react-redux';

class BookDetail extends Component {
    render() {
        if (!this.props.book) {
            return <div>Select a book to get started.</div>;
        }
        return (
            <div>
                <h3>Deatils for: </h3>
                <div>Title: {this.props.book.title}</div>
                <div>Pages: {this.props.book.page}</div>
            </div>
        );
    }
}

function mapStateToProps(state) {
    return {
        book: state.activeBook
    }
}

export default connect(mapStateToProps)(BookDetail);
````

<br>

<br>

## 3. Actions

#### index.js

````react
// src/sctions/index.js

export function selectBook(book) {
    return {
        type: 'BOOK_SELECTED',
        payload: book
    }
}
````

- **Action 객체**
  1. type
  2. payload

<br>

<br>

## 4. App.js

````react
import React from 'react';
import './App.css';
import BookList from './containers/book-list';
import BookDetail from './containers/book-detail';

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <BookList />
        <BookDetail />
      </header>
    </div>
  );
}

export default App;

````

<br>

<br>

## 5. index.js

````react
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import { createStore } from 'redux';
import { Provider } from 'react-redux';

import reducer from './reducers'; //index.js, reducer-books.js

const store = createStore(reducer);

ReactDOM.render(
    <Provider store={store}>
        <App />
    </Provider>, document.getElementById('root'));
````

- **createStore**
- **Provider**

<br>