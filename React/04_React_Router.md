# :baby_chick:React #4

> - React Router에 대해 배우기
> - 3 pages 간의 이동
> - Plugin (rcc : class / rsc : function)

<br>

### shouldComponentUpdate(nextProps, nextState)

- 렌더링 직전에 실행됨
- 렌더링 : 새로운 속성이나 상태가 전달된 후에 이뤄짐
- shouldComponentUpdate()에서 false를 반환하도록 구현하면 React가 다시 렌더링되지 않도록 할 수 있음
  - 변경된 부분 X
  - 불필요한 성능 저하를 피하고자 할 때(컴포넌트 多) 유용

<br>

<br>

### WHY super(props)?

- javascript에서 `super` = 부모 생성자
- super(props) 선언 전까지는 `.this` 키워드 사용 불가

<br>

<br>

## 1. Single Page Application(SPA)

- 하나의 페이지로 동작하는 Application
- 단점 : 히스토리 X / 이전페이지 X
- 단점을 보완하기 위해 React Router와 같은 추가적인 라이브러리가 필요하다

<br>

<br>

## 2. Native App VS Mobile- Web

![native mobile web hybrid 이미지 검색결과](https://lh3.googleusercontent.com/proxy/EyEyONFemgpWEcl1k5BSCPjazkutNjk6tMNfe2FOjUYGOZOWENwl4iuNzO3flTgYv_LMdku4Gtr6CCo4_FNohxvJAKll12rjEM_VCn76Ktk19kdQemOl8VgSDrQNckGArcJoU4wXb1DAGg9ErkXT9QQf_WfUVq5j8sLL9tdytP9-tGeXiaetnTFB8VQC)

- Mobile Web
  - Java / Kotlin / Swift 등
  - PhoneGap / React / Angular

- Native App
  - CSS / Javascript / Html
  - 단점 : 카메라 등 하드웨어의 기능 사용 불가능
- **Hybrid App**
  - Native + Web App

<br>

<br>

## 3. React Router  라이브러리

- SPA에서 라우팅 문제를 해결하기 위해 사용되는 **네비게이션 라이브러리**

- 요청을 받았을 때 페이지를 어떤 뷰, 컴포넌트로 갈지 정해줌
- 브라우저 **내장객체** 사용 가능
  - location
  - history (브라우저의 뒤로가기 등)

<br>

#### **react-router-dom**

- V4에서 새롭게 나온 개념
- 간편한 페이지 렌더링 라이브러리
- `npm install ---save react-router-dom`

<br>

#### 사용법

```react
import { BrowserRouter as Router, Route } from 'react-router-dom';
```

<br><br>

## 4. 첫번째 예제

### header.js

```react
import React from 'react';
import { Link } from 'react-router-dom';

export default () => {
    return (
        <div>
            <ul>
                <li><Link to="/my_note1">Page1</Link></li>
                <li><Link to="/my_note2">Page2</Link></li>
                <li><Link to="/my_note3">Page3</Link></li>
            </ul>
            
        </div>
    );
};
```

#### Link to=""

- <a>링크와 유사한 기능의 컴포넌트
  - href의 속성의 의미로 `to`속성 사용

<br>

### App.js

```react
import React from 'react';
import { BrowserRouter as Router, Route } from 'react-router-dom';

import Header from './components/header';
import Page1 from './routes/page1';
import Page2 from './routes/page2';
import Page3 from './routes/page3';

const App = () => {
  return (   
      <Router>
        <Header />
            <Route path="/my_note1" component={Page1}/>
            <Route path="/my_note2" component={Page2}/>
            <Route path="/my_note3" component={Page3}/>
        </Router>
  );
};

export default App;
```

- `Router` : 사용자들이 요청할 수 있는 요청 경로 모아둠
- `Route` : `path`, 
  - Router안의 세부 요소들
  - 개별적인 링크로 연결됨
  - `component`  : 어디로 갈지 지정

<br>

### Page.js

```react
import React from 'react';

const Page1 = () => {
    return (
        <div>
            <h1>Page1 입니담ㅎㅅㅎ</h1>
        </div>
    );
};

export default Page1;
```

<br>

<br>

```react
<Router>
<div>
    <ul>
    <li><Link to="/my_note1">Page1</Link></li>
    <li><Link to="/my_note2">Page2</Link></li>
    <li><Link to="/my_note3">Page3</Link></li>
    </ul>

</div>
    <Route path="/my_note1" component={Page1}/>
    <Route path="/my_note2" component={Page2}/>
    <Route path="/my_note3" component={Page3}/>
</Router>
```

- <Link>의 to와 <Route>의 path가 서로 연결됨
- <Router> 안에 <Route>
- 

<br>

<br>



## 5. 두번째 예제

### App.js

```react
import React from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import About from './routes/About';
import Home from './routes/Home';
import Header from './components/header';
import Posts from './routes/Posts';
import Login from './routes/Login';
import MyProfile from './routes/MyProfile';
import Search from './routes/Search';
import NotFound from './NotFound';

const App = () => {
  return (
    <Router>
      <Header />
      <Switch>
        <Route exact path="/" component={Home} />
        <Route path="/about/:userid" component={About} />
        <Route path="/posts" component={Posts} />
        <Route path="/search" component={Search} />
        <Route path="/login" component={Login} />
        <Route path="/myprofile" component={MyProfile} />
        <Route component={NotFound} />
      </Switch>
    </Router>
  );
};

export default App;
```

- **exact** path : 정확한 링크만 지정

  ```react
  <Route exact path="/" component={Home} />
  ```

- `Switch`를 사용해 존재하지 않는 페이지 요청이 들어오면 NotFound를 띄움



<br>

- **url parameter**

  - 컴포넌트 내에서 분리되어 데이터로 사용 가능
  - 블로그나 사용자의 페이지를 재활용하고 싶을 때 사용
  - `:` 사용

  ```react
  //App.js
  <Route path="/about/:userid" component={About} />
  
  //header.js
  <Link to="/about/dong" className="item">About</Link>
  
  //About.js
  {match.params.userid} About
  ```

<br>

<br>

### header.js

```react
import React from 'react';
import { Link } from 'react-router-dom';
import './Header.css'

const Header = () => {
    return (
        <div className="header">
            <Link to="/" className="item">Home </Link>
            <Link to="/about" className="item">About</Link>
        </div>
    );
};

export default Header;
```

<br>

<br>

### About.js

```react
import React from 'react';

const About = ({match}) => {
    return (
        <div>
            {match.params.userid}'s Profile
        </div>
    );
};

export default About;
```

- 위에서 값이 match로 전달되어짐
- `match.params.값` 

<br>

<br>

### Posts.js

```react
import React from 'react';
import { Link, Route } from 'react-router-dom';

const Post = ({match}) => {
    return (
        <h2>{match.params.title}</h2>
    )
}

const Posts = () => {
    return (
        <div>
            <h2>Posts</h2>
            <Link to="/posts/java">Java Programming</Link>
            <Link to="/posts/react">React Programming</Link>
            <Link to="/posts/js">Javascript Programming</Link>
            <Link to="/posts/msa">Microservice Architecture</Link>

            <Route path="/posts/:title" component={Post} />
        </div>
    );
};

export default Posts;
```

<br>

<br>

### MyProfile.js

```react
import React from 'react';
import {Redirect} from 'react-router-dom';

const isLogged = true;

const MyProfile = () => {
    return (
        <div>
            {
                !isLogged && <Redirect to="/login" />
            }

            MyProfile
        </div>
    );
};

export default MyProfile;
```

- <Redirect> : 강제로 가게끔 만든다

<br>

<br>

### search.js

```react
import React from 'react';

const Search = ({location}) => {
    return (
        <div>
            Search Keyword : {new URLSearchParams(location.search).get("keyword")}
        </div>
    );
};

export default Search;
```

- [http://localhost:3000/search?keyword=리액트

<br>

<br>

### header.css

```css
.header {
    background: lightskyblue;
    display: table;
    table-layout: fixed;
    width: 100%;
}

.item {
    text-align: center;
    padding-top: 1rem;
    padding-bottom: 1rem;
    display: table-cell;
    color: indigo;
    text-decoration: none;
    font-size: 1.1rem;
}

.item:hover {
    background: slateblue;
}

.item:active, .item.active {
    background: white;
    color: tomato;
}
```

- `.item:active` : 마우스 이벤트에 대한 css
- `.item.active` : 아이템에 대한 css

<br>

<br>

