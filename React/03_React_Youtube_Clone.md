# :mushroom:React #3 (Youtube):mushroom:

> - React
> - Bootstrap
> - YouTube Data API v3
>
> <br>
>
> ![simple youtube app react component 이미지 검색결과](https://miro.medium.com/max/1255/1*gcu4ppE5zJX-ta4XkR7ziw.png)
>
> - 컴포넌트
>   - search_bar.js
>   - video_detail.js
>   - video_list_item.js
>   - video_list.js
>   - root Component

<br>

<br>

## 1. 준비

- https://github.com/joneconsulting/cloud-computing.git / SimpleStarter
- `npm install`
- `npm start`
- localhost:8080

<br>

### YouTube Data API v3

- https://console.developers.google.com/
- `enable` - `credentials` -  `create credentials` - `API key` 선택 후 key값 복사
- index.js에 `const API_KEY = ' 키값'`
- 추가로 설치

```
npm install --save youtube-api-search
```

- index.js에서 사용

<br>

<br>

## 2. 컴포넌트 작성

### ■ index.html

```react
<!DOCTYPE html>
<html>
  <head>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" href="/style/style.css">
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0-alpha.2/css/bootstrap.min.css">
    <script src="https://maps.googleapis.com/maps/api/js?key=키"></script>
  </head>
  <body>
    <div class="container"></div>
  </body>
  <script src="/bundle.js"></script>
</html>

```

<br>

### ■ index.js (root)

```react
import React, { Component } from 'react';
import ReactDOM from 'react-dom';
import SearchBar from './component/search_bar';
import YTSearch from 'youtube-api-search'
import VideoList from './component/video_list';
import VideoDetail from './component/video_detail';

const API_KEY = '값';

//1.key(객체) 2.term(용어) 3.console 출력


class App extends Component {
  //생성자
  constructor(props){
    super(props); //부모의 생성자 함수를 호출(Component)

    this.state = {
      videos: [],
      selectedVideo: null
    }
    
    YTSearch({ key: API_KEY, term: '커피' }, (data) => {
      this.setState({
        videos: data,
        selectedVideo: data[0]
      })
    });
  }

  videoSearch() {

  }

  handleSelect = (selectedVideo) => {
    //선택된 video를 detail에 표시
    console.log("selectedVideo=" + selectedVideo.snippet.title);

    const { videos } = this.state.videos;

    this.setState({
      ...videos,
      selectedVideo: selectedVideo
    })
  }

  render() {
    return (
      <div>
        <SearchBar />
        <VideoDetail video={this.state.selectedVideo}/>
        <VideoList 
          onVideoSelect={this.handleSelect}
          videos={this.state.videos}/>
      </div>
    );
  }
}

ReactDOM.render(<App/>, document.querySelector('.container'));
```

- **React**는 View를 만들기위한 라이브러리
- **ReactDOM**은 UI를 실제로 브라우저에 렌더링 할 때 사용하는 라이브러리
- `document.querySelector('.container')` : 엘레멘트를 렌더링 후 index.html의 container 영역에 화면을 배포

<br>

#### (1) import Youtube-api-search

```react
import YTSearch from 'youtube-api-search'

//1.key(객체) 2.term(용어) 3.console 출력
YTSearch({ key: API_KEY, term: 'mara' }, function(data) {
  console.log(data);
});
```

- `term`에서 검색한 데이터가 `function(data)` 에 들어옴

<br>

#### (2) VideoList로 데이터 값 보내기

```react
<VideoList videos={this.state.videos}/>
```

<br>

#### (3) 선택한 데이터를 플레이어에 보여주기

- `selectedVideo` 추가 / 초기화
- 데이터를 자식  → 부모 전달
  - Item → List → App : **Function**의 주소값을 전달

```react
<VideoListItem 
    onVideoSelect = {props.onVideoSelect}
    key={v.etag}
    video={v}/>
```

- 부모 컴포넌트(index.js) 의 onVideoSelect Function 사용
- `key` : 선언만 시켜놓음 / react가 아이템을 구별해놓는 유일한 키값

<br>

#### (3) - 1. handleSelect 함수 따로 만들기

```react
handleSelect = (selectedVideo) => {
    //선택된 video를 detail에 표시
    console.log("selectedVideo=" + selectedVideo.snippet.title);

    const { videos } = this.state.videos;

    this.setState({
        ...videos,
        selectedVideo: selectedVideo
    })
}
---
<VideoList 
          onVideoSelect={this.handleSelect}
          videos={this.state.videos}/>
```

<br>

#### (3) - 2. 한줄짜리 코드로 만들기

```react
 <VideoList //같은 이름일 경우 이름 하나 명시 가능
          onVideoSelect={selectedVideo => this.setState({selectedVideo})}      
          videos={this.state.videos}/>
```

<br>

<br>

### ■ video_list.js

```react
import React from 'react';
import VideoListItem from './video_list_item';

const VideoList = (props) => {
    const videoItems = props.videos.map(v => {
        return (
            <VideoListItem 
                onVideoSelect = {props.onVideoSelect}
                key={v.etag}
                video={v}/>
        )
    });

    return (
        // Bootstrap 사용
        <ul className="col-md-4 list-group"> 
            {videoItems}
        </ul>
    )
}

export default VideoList;
```

<br>

#### (1) index.js에서 받은 데이터

```react
const VideoList = (props) => {
    return (
        <ul">
            {props.videos.length}
        </ul>
    )
}
```

- `const VideoList = (props) => ` : 부모 컴포넌트가 호출할 때 받을 수 있는 값을 **props**라고 이름 지음

<br>

#### (2) video_list_item 컴포넌트를 호출

```react
const videoItems = props.videos.map(v => {
        return (
            <VideoListItem 
                onVideoSelect = {props.onVideoSelect}
                key={v.etag}
                video={v}/>
        )
    });
```

- `map`함수를 사용해서 요소들의 개수만큼 `VideoListItem`을 호출

<br>

<br>

### ■ video_list_item.js

```react
import React from 'react';
import VideoList from './video_list';

const VideoListItem = ({video, onVideoSelect}) => {
    const ImageUrl = video.snippet.thumbnails.default.url;

    return (
        <li className="list-group-item">
            <div className="video-list media">
                <div className="media-left">
                    <img className="media-object" src={ImageUrl} />
                </div>
                <div className="media-body">
                    <div className="media-heading">
                        {video.snippet.title}
                    </div>
                </div>
            </div>
        </li>
    )
}

export default VideoListItem;
```

<br>

#### (1) props로 데이터 받기

- 받아온 데이터를 console로 확인하고 ImagUrl 등 요소를 이용해 페이지 생성
- Bootstrap 사용

<br>

#### (2) onVideoSelect 함수 받기

- 함수를 사용해 최상위 컴포넌트 호출 가능
- onClick 이벤트 호출 **직접** 함수 만들기

```react
<li onClick = {() => onVideoSelect(video)} 
        className="list-group-item">
```



<br>

### ■ search_bar.js

```react
import React, { Component } from 'react';

class SearchBar extends Component{
    constructor(props) {
        super(props) //컴포넌트 부모 생성자 호출

        this.state = {
            term: ''
        }
    }

    onInputChange = (event) => {
        this.setState({
            term: event.target.value
        });
    }

    render() {
        return (
            <div className="search-bar">
                <input  onChange={this.onInputChange} />
            </div>
        )
    }
}
export default SearchBar;
```

<br>

- `input` 에서 한번에 콘솔 확인

```react
<input onChange={event => console.log(event.target.value)}/>
```

<br>

<br>

### ■ video_detail.js

```react
import React from 'react';

const VideoDetail = ({video}) => {
    if (!video) {
        return <div>Loading...</div>
    }

    const videoId = video.id.videoId;

    //const url = 'https://www.youtube.com/embed/' + video.id.videoId;
    const url = `https://www.youtube.com/embed/${videoId}`;

    return (
        <div className="video-detail col-md-8">
            <div className="embed-responsive embed-responsive-16by9">
                <iframe className="embed-responsive-item" src={url}/>    
            </div>
            <div className="details">
                <div>{video.snippet.title}</div>
                <div>{video.snippet.description}</div>
            </div>
        </div>
    )
}
export default VideoDetail
```

<br>

#### (1) 첫번째 비디오의 데이터 넘기기

- video player 화면에 리스트의 첫번째 비디오가 재생되도록 함

- **비구조할당**

  ```react
  const VideoDetail = ({video})
  ```

<br>

#### (2) 비디오 플레이

```react
const videoId = video.id.videoId;
//const url = 'https://www.youtube.com/embed/' + video.id.videoId;
const url = `https://www.youtube.com/embed/${videoId}`;
```

- 유튜브 비디오 플레이어만 가져와야하니 https://www.youtube.com/embed/ + videoId
- `백틱 `을 사용하면 `${}`을 이용해직접 값 추가 가능

<br>

#### (3) 초기 데이터값 딜레이에 따른 오류

- `video.id.videoId` : 딜레이로인해 video가 아직 loading중일 수도 있다. 오류 발생!

```react
if (!video) {	return <div>Loading...</div>	}
```

- 따라서 video[0] 데이터가 아직 안 왔을 때 `(!video)`일때 return을 추가

<br>

<br>

<br>

### ■ style.css

```css
.search-bar {
    margin: 20px;
    text-align: center;
}

.search-bar input{
    width: 75%;
}

.video-item img {
    max-width: 64px;
}

.video-detail .details {
    margin-top: 10px;
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 4px;
}

.list-group-item {
    cursor: pointer;
}

.list-group-item:hover {
    background-color: lightsalmon;
}
```

