# React Day 6

> **Weather API + Search Bar + Weather List**
>
> - npm install --save redux axios
>
> - https://api.openweathermap.org/data/2.5/forecast?appid=키값&q=London,us
> - 특정한 지역도 제한 가능
>
> 1. actions - index.js
> 2. components - chart.js
> 3. containers - search_bar.js, weather_list.js
> 4. reducers - index.js, reducer_weather.js

<br>

<br>

## 01 actions

### API 사용해서 fetch weather data

#### index.js

```react
import axios from 'axios';

const API_KEY = '643adbd396a1b85d6abb367fc3ad4e60';
const ROOT_URL = `https://api.openweathermap.org/data/2.5/forecast?appid=${API_KEY}`;

export const FETCH_WEATHER = 'FETCH_WEATHER';

// 사용자가 Search 버튼 클릭시 호출됨
export function fetchWeather(city) {
    const url = `${ROOT_URL}&q=${city}`;
    const request = axios.get(url);

    return {
        type: FETCH_WEATHER,
        payload: request
    }
}
```

- **axios** 사용
  -  `const request = axios.get(url)`
- 상수를 export해서 외부에서 사용할 수 있게끔 함
  - `export const FETCH_WEATHER = 'FETCH_WEATHER'`
- `return`으로 반환해줌
  - `type`는 필수로 지정해줘야함

<br>

<br>

## 02 reducers

### root_reducer

#### index.js

```react
import { combineReducers } from 'redux';
import WeatherReducer from './reducer_weather';

const rootReducer = combineReducers({
    weater: WeatherReducer
});

export default rootReducer;
```

- **combineReducers** : rootReducer에 새로운 reducer 추가

<br>

#### reducer_weather.js

```react
import { FETCH_WEATHER } from "../actions";

// action.type = FETCH_WEATHER , action.payload(data = weather json)
export default function(state = [], action) {
    switch(action.type) {
        case FETCH_WEATHER:
            // (X) return state.push(action.payload.data);
            // (OLD) return state.concat(action.payload.data);

                    // 날씨 데이터      // 기존 데이터 누적
            return [ action.payload.data, ...state ]
        default:
            return state;
    }
}
```

- **state**가 null이 올 수 있으니 default값을 지정해줌

- **action**은 두 가지가 올 수 있다

  1. action.type
  2. action.payload

- Javascript에서는 state 배열에 데이터를 추가해서 return해줌

  -             // (X) return state.push(action.payload.data);
                // (OLD) return state.concat(action.payload.data);

  - redux는 전개연산자 사용하면 간단히 표현 가능

- 전개연산자 O

  - [Seoul, Tokyo, Newyork, seattle]

- 전개연산자X

  - [Seoul, [Tokyo, Newyork, seattle]]

<br>

<br>



<br>

<br>

## 03 Containers

#### search_bar.js

```react
import React, { Component } from 'react';
import { connect } from 'react-redux'
import { bindActionCreators } from 'redux';
import { fetchWeather } from '../actions/index';

class SearchBar extends Component {
  constructor(props) {
    super(props);

    this.state = { term: ''};

  }

  onInputChange(event) {
    this.setState ({
      term: event.target.value
    });
  }

  onFormSubmit(event) {
    //submit의 default 전송 기능은 막아둠
    event.preventDefault();

    // fetchweather에 data전달하고 원래 term은 초기화하기
    this.props.fetchWeather(this.state.term);
    this.setState({
      term: ''
    })
  }

  render() {
    return (
      <form onSubmit={this.onFormSubmit} className="input-group">
        <input 
          placeholder="Get a five-day forecast in your favorite cities"
          className="form-control"
          value={this.state.term}
          onChange={this.onInputChange}
          />
        <span className="input-group-btn">
          <button type="submit" className="btn btn-secondary">Submit</button>
        </span>
      </form>
    );
  }
}

// mapDispatchToProps function
function mapDispatchToProps(dispatch) {
  //action과 props 연결
  return bindActionCreators({fetchWeather}, dispatch);
}

// connect mapping (만들고자 하는 기능의 함수) (Component의 이름)
// 첫번째 매개변수가 원래 mapStatetoProps인데 없으니까 필요 없으니 전달 X
// mapDispatchToProps는 actions과 props 연결 위해 전달
export default connect(null, mapDispatchToProps)(SearchBar);
```

<br>

<br>

#### weather_list.js

```react
import React, { Component } from 'react';
import { connect } from 'react-redux';
import Chart from '../components/chart';

class WeatherList extends Component {

  render() {
    return (
      <table>
        <thead></thead>
        <tbody></tbody>
      </table>
    );
  }
}

// mapSrtateToProps funciton

// connect mapping
```

