# :cherries:React App

> - React를 활용해 기본적인 CRUD가 가능한 웹 페이지를 구현
> - 여러 개의 자식 Component와 하나의 부모 Component 사이의 데이터 교환

<br>

# React의 Form Component

```react
import React, { Component, Fragment } from 'react';

class PhoneForm extends Component {
    state = {
        name: '',
        phone: ''
    }

    handleChange = (event) => {
        this.setState({
            [event.target.name] : event.target.value
        })
    }

    render() {
        return(
            <form>
                <input
                    placeholder="이름을 입력하세요"
                    onChange={this.handleChange} 
                    name='name'/> <br></br>
                <input
                    placeholder="전화번호를 입력하세요"
                    onChange={this.handleChange}
                    name='phone' />
                <div>
                    <h1>{this.state.name} / {this.state.phone}</h1>
                </div>
            </form>
            
        );
    }
}

export default PhoneForm;
```

- `[event.target.name]` : input 태그의 name을 각각 동적으로 가져옴

<br>

<br>

# 자식 → 부모 컴포넌트 전달

### App.js

```react
import React, { Component } from 'react';
import PhoneForm from './phone_form';


class App extends Component {
  id = 1;
  state = {
    contacts: [
      { 
        id: 0,
        name: '관리자',
        phone: '010-2222-3333'
      }
    ]
  }

  handleCreate = (data) => {
    console.log(data);
    // data -> contacts 배열에 추가
    // 기존 데이터 가져오기
    const { contacts } = this.state;
    this.setState ({
      contacts: contacts.concat({id: this.id++, ...data})
    })
  }

  render() {
    const { contacts } = this.state;

    return (
      <div>
        <PhoneForm 
          onCreate = {this.handleCreate}/> <br></br>
        
        {JSON.stringify(contacts)}
      </div>
    );
  }
}
export default App;
```

- `contacts`에 데이터를 추가

- `{}` 를 사용한 **비구조 할당**

  ```react
  // 비구조 할당
  const { contacts } = this.state;
  console.log(this.state);  // { contacts: [...]}
  console.log(this.state.contacts); // [...]
  console.log(contacts);  // [...]
  
  // 원래
  const temp = this.state
  {JSON.stringify(contacts.contacts)}
  ```

  - 비구조 할당을 함으로써 데이터를 바로 사용 가능

<br>

<br>

### phone_form.js

```react
import React, { Component, Fragment } from 'react';

class PhoneForm extends Component {
    state = {
        name: '',
        phone: ''
    }
    handleSubmit = (e) => {
        e.preventDefault();
        this.props.onCreate(this.state);
	
        //입력값 초기화
        this.setState({
            name: '',
            phone: ''
        })
    }

    handleChange = (event) => {
        this.setState({
            [event.target.name] : event.target.value
        })
    }

    render() {
        return(
            <form onSubmit={this.handleSubmit}>
                <input
                    value={this.state.name}
                    placeholder="이름을 입력하세요"
                    onChange={this.handleChange} 
                    name='name'/> <br></br>
                <input
                    value={this.state.phone}
                    placeholder="전화번호를 입력하세요"
                    onChange={this.handleChange}
                    name='phone' />
                <button type="submit">등록</button>
            </form>
            
        );
    }
}
export default PhoneForm;
```

- `e.preventDefault();` : 리프레싱 되는 현상 없이 데이터가 전달됨

- 전송 후 데이터 초기화
  - this.setState({
                name: '',
                phone: ''
            })
  - value={this.state.name}
  - value={this.state.phone}
- `JSON.stringify( )` : 자바스크립트의 값을 JSON 문자열로 변환

<br>

#### 기존 배열 데이터에 자식 데이터 추가하기

```react
handleCreate = (data) => {
    // 기존 데이터 가져오기
    const { contacts } = this.state; 
    
     // data -> contacts 배열에 추가
    this.setState ({
        contacts: contacts.concat({id: this.id++, ...data})
    })
}
```

- `concat`: 문자열 연결해주는 함수
- `...data` : 기존 데이터 유지

<br>

<br>

# :four_leaf_clover:2 자식 1 부모 컴포넌트 사이 Data 전달

> 1. phone_form 컴포넌트  : 사용자에게 입력 받음
> 2. phone_list 컴포넌트 : 데이터 출력
> 3. App 루트 컴포넌트

<br>

### phone_list.js

```react
import React, { Component } from 'react';

class PhoneList extends Component {
    render() {
        //const data = this.props;    //contacts -> data.contacts 번거로움 ㅠㅠ
        const {data} = this.props;  //비구조할당! data -> contacts
        const list = data.map(value => 
            <div>{value.name} / {value.phone}</div>
        );

        return (
            <div>
                {list}
            </div>
        )
    }
}

export default PhoneList;
```

- `map` 함수

  - 집합 값의 모든 요소를 끄집어 내서 value에 넣어줌

    ```react
    const list = data.map(value => 
    	<div key={value.id}>{value.name} / {value.phone}</div>
    );
    ```

- `key` : key값을 사용하지 않으면 만약 데이터가 삭제/수정/추가 되었을 때 전체를 다시 만든다.
  - key값이 있으면 해당 key값의 데이터만 삭제/수정/추가하면 되기 때문에 조금 더 **효율적**이다.

<br>

#### App.js

```react
import React, { Component } from 'react';
import PhoneForm from './phone_form';


class App extends Component {
  id = 1;
  state = {
    contacts: [
      { 
        id: 0,
        name: '관리자',
        phone: '010-2222-3333'
      }
    ]
  }

  handleCreate = (data) => {
    console.log(data);
    // data -> contacts 배열에 추가
    // 기존 데이터 가져오기
    const { contacts } = this.state;
    this.setState ({
      contacts: contacts.concat({id: this.id++, ...data})
    })
  }

  render() {
    const { contacts } = this.state;
    console.log(this.state);  // { contacts: [...]}
    console.log(this.state.contacts); // [...]
    console.log(contacts);  // [...]

    return (
      <div>
        <PhoneForm 
          onCreate = {this.handleCreate}/> <br></br>
        
        <PhoneList data={this.state.contacts}/>
      </div>
    );
  }
}
export default App;
```

- `<PhoneList />` 추가

<br>

<br>

# 3 자식 1 부모 Component

### phone_form.js

```react
import React, { Component, Fragment } from 'react';

class PhoneForm extends Component {
    state = {
        name: '',
        phone: ''
    }
    handleSubmit = (e) => {
        e.preventDefault();
        this.props.onCreate(this.state);

        this.setState({
            name: '',
            phone: ''
        })
    }

    handleChange = (event) => {
        this.setState({
            [event.target.name] : event.target.value
        })
    }

    render() {
        return(
            <form onSubmit={this.handleSubmit}>
                <input
                    value={this.state.name}
                    placeholder="이름을 입력하세요"
                    onChange={this.handleChange} 
                    name='name'/> <br></br>
                <input
                    value={this.state.phone}
                    placeholder="전화번호를 입력하세요"
                    onChange={this.handleChange}
                    name='phone' />
                <button type="submit">등록</button>

            </form>
            
        );
    }
}

export default PhoneForm;
```

<br>

### phone_item.js

```react
import React, { Component, Fragment } from 'react';

class PhoneItem extends Component {
    render() {
        const css = {
            border: '1px solid green',
            padding: '10px',
            margin: '',
        };
        const {name, phone, id } = this.props.info;

        return(
            <div style={css}>
                <div><b>{name}</b></div>
                <div>{phone}</div>
            </div>
            
        );
    }
}

export default PhoneItem;
```

- `margin` : 다른 요소들과의 간격 조절
- `padding` : 영역 안쪽의 간격 조절



<br>

### phone_list.js

```react
import React, { Component } from 'react';
import PhoneItem from './phone_item';

class PhoneList extends Component {
    render() {

        const {data} = this.props; 
        const list = data.map(value => (
                <PhoneItem key={value.id}
                info={value} />
            ) 
        );

        return (
            <div>
                {list}
            </div>
        )
    }
}

export default PhoneList;
```

<br>

<br>

## Javascript 문자열관련 함수

```javascript
const myArray = [1,2,3,4,5];

myArray.slice(0,2)
[ 1, 2 ]

myArray.slice(0,2).concat(myArray.slice(3,5))
[ 1, 2, 4, 5 ]

[...myArray.slice(0,2), ...myArray.slice(3,5)]
[ 1, 2, 4, 5 ]

myArray.map(v => v**2);
[ 1, 4, 9, 16, 25 ]

myArray.map(v => "<div>" + v + "</div>")
[
  '<div>1</div>',
  '<div>2</div>',
  '<div>3</div>',
  '<div>4</div>',
  '<div>5</div>'
]

myArray.filter(n => n == 3);
[ 3 ]

myArray.filter(n => n % 2 == 0);
[ 2, 4 ]
```

- `concat` : 합치기
- `slice` : 범위 내의 값들만 남기기
- `map` : 가지고 온 모든 요소에 공통적으로 기능을 적용할 때
  - 요소의 개수는 그대로
- `filter` : 조건에 true인 요소들만 추림
  - 요소의 개수가 달라짐
- `reduce`

<br>

<br>

# 삭제 이벤트 추가

> **이벤트 처리**
>
> 1. 이벤트 주체를 결정 : <삭제>버튼
> 2. 이벤트 종류를 결정 : <Click 이벤트>
> 3. 이벤트 핸들러를 구현 : 이벤트가 작동되면 어떤 동작을 할지 결정하는 함수 구현
> 4. 이벤트 주체 <-> 핸들러 연결 : 함수~버튼

<br>

### (1) App.js

```react
import React, { Component } from 'react';
import PhoneForm from './components/phone_form';
import PhoneList from './components/phone_list';


class App extends Component {
  id = 1;
  state = {
    contacts: [
      { 
        id: 0,
        name: '관리자',
        phone: '010-2222-3333'
      }
    ]
  }

  handleCreate = (data) => {
    console.log(data);
    // data -> contacts 배열에 추가
    // 기존 데이터 가져오기
    const { contacts } = this.state;
    this.setState ({
      contacts: contacts.concat({id: this.id++, ...data})
    })
  }

  // state의 contacts에서 해당 ID 값을 삭제
  handleRemove = (selected_id) => {    
    console.log('App handleRemove=' + selected_id)
    const { contacts } = this.state;

    this.setState({
      contacts : contacts.filter(
        info => info.id !== selected_id
      )
    });
  }

  render() {
    const { contacts } = this.state;

    return (
      <div>
        <PhoneForm 
          onCreate = {this.handleCreate}/> <br></br>
        
        <PhoneList data={this.state.contacts}
          onRemove = {this.handleRemove} />
      </div>
    );
  }
}
export default App;
```

<br>

#### handleRemove()

```react
// state의 contacts에서 해당 ID 값을 삭제
handleRemove = (selected_id) => {    
    const { contacts } = this.state;
    // const obj = thist.state; => obj.contacts.filter

    this.setState({
        contacts : contacts.filter(
            info => info.id !== selected_id
        )
    });
}
```

- const obj = this.state

  - 이렇게 할당하면 귀찮아짐
    - obj.contacts.filter

- `filter`를 통해서 삭제버튼이 클릭되지 않은 id값만 남는다

  - filtering된 데이터는 다른 Array에 값을 저장시켜놔야함

    => setState() 필요함

<br>

### (2) phone_list.js

```react
import React, { Component } from 'react';
import PhoneItem from './phone_item';

class PhoneList extends Component {
    render() {

        const {data, onRemove} = this.props; 
        const list = data.map(value => (
                <PhoneItem 
                    key={value.id}
                    info={value}
                    onRemove = {onRemove}
                />
            ) 
        );

        return (
            <div>
                {list}
            </div>
        )
    }
}

export default PhoneList;
```

- `const {data} = this.props;` 이렇게 할당하면 아래와같이 사용해야함
  - data -> data.data
  - onRemove -> data.onRemove

<br>

### (3) phone_item.js

```react
import React, { Component, Fragment } from 'react';

class PhoneItem extends Component {
    handleRemove = (e) => {
        const { info, onRemove } = this.props;

        onRemove(info.id);  //부모의 Remove (List의 Remove) 호출
    }

    render() {
        const css = {
            border: '2px solid skyblue',
            padding: '10px',
            margin: '7px',
        };

        const {name, phone, id } = this.props.info;

        return(
            <div style={css}>
                <div><b>{name}</b></div>
                <div>{phone}</div>
                <button onClick={this.handleRemove}>삭제</button>
            </div>
            
        );
    }
}

export default PhoneItem;
```

- `onRemove(info.id)`
  - item → list → App

<br>

<br>

# 수정 이벤트 추가

> 1. 수정 버튼 추가
> 2. 수정 클릭시 해당하는 ITEM의 Name, phone값을 콘솔에 출력

<br>

```javascript
> const myTag = [
... {id: 0, text: 'Hello', tag: 'A'},
... {id: 1, text: 'World', tag: 'B'},
... {id: 2, text: 'Bye', tag: 'C'},
... ];
undefined
> myTag
[
  { id: 0, text: 'Hello', tag: 'A' },
  { id: 1, text: 'World', tag: 'B' },
  { id: 2, text: 'Bye', tag: 'C' }
]

> const modifiedArray = myTag.map(v => v.id === 1 ? ({text: 'React'}): v);
undefined
> modifiedArray
[
  { id: 0, text: 'Hello', tag: 'A' },
  { text: 'React' },
  { id: 2, text: 'Bye', tag: 'C' }
]

> const modifiedArray2 = myTag.map(v => v.id === 1 ? ({...v, text: 'React'}): v);
undefined
> modifiedArray2
[
  { id: 0, text: 'Hello', tag: 'A' },
  { id: 1, text: 'React', tag: 'B' },
  { id: 2, text: 'Bye', tag: 'C' }
]
```

- `...v` : 전개 연산자 사용시 나머지 데이터 유지됨

<br>

### App.js

```react
handleUpdate = (selected_id, data) => {
    const { contacts } = this.state;
    // this.state.contacts -> 반영 (this.setState)

    this.setState({
        contacts : contacts.map(
            item => item.id === selected_id 
            ? {...item, ...data}
            : item //기존 값 유지
        )
    })
}
```

- `{...item, ...data}` : 기존데이터 + 넘어온 데이터

<br>

### Phone_list.js

```react
import React, { Component, Fragment } from 'react';

class PhoneItem extends Component {
    state = {
        editable: false,
        name: '',
        phone: ''
    }

    handleUpdate = (e) => {
        const { editable } = this.state;
        // const editable = this.state.editable;

        this.setState({
            editable: !editable //true
        });
    }

    handleRemove = (e) => {
        const { info, onRemove } = this.props;

        onRemove(info.id);  //부모의 Remove (List의 Remove) 호출
    }

    render() {
        const css = {
            border: '2px solid skyblue',
            padding: '10px',
            margin: '7px',
        };
        const {editable} = this.state;
        if (editable) {
            console.log('수정모드');
            return (
                <div>
                    수정모드
                </div>
            );
        }

        const {name, phone} = this.props.info;

        return(
            <div style={css}>
                <div><b>{name}</b></div>
                <div>{phone}</div>
                <button onClick={this.handleRemove}>삭제</button> &nbsp;
                <button onClick={this.handleUpdate}>수정</button>
            </div>
            
        );
    }
}

export default PhoneItem;
```

- `this.setState({ editable: !editable});`
  - 한번 클릭시 수정 / 한번 더 클릭시 일반 

<br>

### phone_item.js

```react
import React, { Component, Fragment } from 'react';

class PhoneItem extends Component {
    state = {
        editable: false,
        name: '',
        phone: ''
    }

    componentDidUpdate(preProps, prevState) {
        const { info, onUpdate } = this.props;
        console.log(info.name, info.phone);
        console.log(prevState.editable + "/" + this.state.editable);

        if (!prevState.editable && this.state.editable) {
            this.setState ({
                name: info.name,
                phone: info.phone
            })
        }
        //update
        if (prevState.editable && !this.state.editable) {
            onUpdate(info.id, {
                name: this.state.name,
                phone: this.state.phone});            
        }
    }

    handleUpdate = (e) => {
        const { editable } = this.state;
        // const editable = this.state.editable;

        this.setState({
            editable: !editable //true
        });
    }

    handleRemove = (e) => {
        const { info, onRemove } = this.props;

        onRemove(info.id);  //부모의 Remove (List의 Remove) 호출
    }

    handleChange = (e) => {
        // name : name/phone , value: 값
        const {name, value} = e.target;
        this.setState({
            [name]: value
        });
    }


    render() {
        const css = {
            border: '2px solid skyblue',
            padding: '10px',
            margin: '7px',
        };
        const {editable} = this.state;
        if (editable) {
            return (
                <div style={css}>
                    <div>
                        <input value={this.state.name} onChange={this.handleChange} name="name" 
                                placeholder="이름을 입력하세용" />
                    </div>
                    <div>
                        <input value={this.state.phone} onChange={this.handleChange} name="phone" 
                                placeholder="연락처를 입력하세용"/>
                    </div>
                    
                    <button onClick={this.handleRemove}>삭제</button> &nbsp;
                    <button onClick={this.handleUpdate}>적용</button>
                </div>
            );
        }

        const {name, phone} = this.props.info;

        return(
            <div style={css}>
                <div><b>{name}</b></div>
                <div>{phone}</div>
                <button onClick={this.handleRemove}>삭제</button> &nbsp;
                <button onClick={this.handleUpdate}>수정</button>
            </div>
            
        );
    }
}

export default PhoneItem;
```

<br>

#### componentDidUpdate 

- Component가 update될 때마다 실행되는 메소드

<br>

#### handleChange

- `<input value={this.state.name} onChange={this.handleChange} name="name" `
  - onChange에 새로운 함수 handleChange 추가해줘야지 값 새로 입력해 적용 가능



## Redux 컴포넌트

상태관리를 하기 위해 사용되는 라이브러리