# Javascript 동적 데이터

- 데이터를 실시간으로 갱신 가능하게함
- onload : 페이지가 다 로드된 후에 보이게끔함

<br>

## li ul 태그

```html
<script>
    window.onload = function() {
                let _ul = document.createElement("ul");
                let _li1 = document.createElement("li");
                let _li2 = document.createElement("li");
                let _naver = document.createTextNode("Naver");
                let _daum = document.createTextNode("Daum");

                _li1.appendChild(_naver);
                _li2.appendChild(_daum);

                _ul.appendChild(_li1);
                _ul.appendChild(_li2);

                document.body.appendChild(_ul);
            }
</script>
```

<br>

<br>

## img태그

```html
<script>
            // image Tag 추가
            let myImg = document.createElement("img");
            myImg.src = "mara.jpg";
            myImg.width = 500;
            myImg.height = 350;
            document.body.appendChild(myImg);
</script>
```

1. img에 src, width, height 속성 이용
2. setAttribute 이용

<br>

<br>

## InnerHTML

```html
<script>
    //document.body.innerHTML = "<h1 style='color:  blue;'>Hello, Mara. Hi, there</h1>";
      let div1 = document.getElementById("myDiv1");
      div1.innerHTML = "<h1 style='color:  blue;'>Hello, Mara. Hi, there</h1>";
      div1.innerText = "<h1 style='color:  blue;'>Hello, Mara. Hi, there</h1>";
</script>
```

- **innerHTML** : 기존 것을 빼버리고 이것만 넣겠다

  - 새로운 객체를 만들어서 그 객체에 집어넣는 방법이 주로 사용됨

  - `<div>`태그 사용

  - <h1 style='color: blue;'>Hello, Mara. Hi, there</h1>

- **innerText** : 문자열로만 들어감

  - 적용된 데이터가 반영되지 않고 문자열만 출력됨
  - style='color: blue;'>Hello, Mara. Hi, there 이런식으로 출력됨

<br>

### child 삭제

```
document.body.removeChild(document.getElementById("myH1"));
```

<br>

### getElementById()

태그의 id를 가지고 element를 조사해서 가져오는 메소드

- id 부여 (unique 해야함)

```
<div id="myDiv1"></div>
```

- id가 없으면 null값 반환

<br>

### getelementsByName() / getelementsByTagName()

- 태그의 name 속성이 일치하는 문서 객체를 **배열**로 가져옴
- 배열 / loop를 이용해서 사용

<br>

<br>

## form

```html
<!DOCTYPE html>
<html>
<head>
    <script>
        function saveTemporary() {
            // 선택된 sns 가져오기
            let selectedSns = document.getElementsByName("sns");
            let cnt = 0;

            for (let i = 0; i < selectedSns.length; i++) {
                if (selectedSns[i].checked) {
                    cnt++;
                    console.log(selectedSns[i].value);
                }

                // sns가 한개도 선택이 안된 경우에 alert() 표시
                
            }
            if (cnt == 0) {
                    alert("SNS좀 선택해주세요!!!!");
            }
        }
    </script>
</head>
<body>
    <form action="regist.html" method="POST">
        이름: <input type="text" placeholder="이름을 입력하세요">
        <br>
        여성: <input type="radio" name="gender"><br>
        사용하는 SNS: 
            <input type="checkbox" name="sns" value="Facebook">Facebook
            <input type="checkbox" name="sns" value="Twitter">Twitter
            <input type="checkbox" name="sns" value="Instagram">Instagram
            <input type="checkbox" name="sns" value="Google">Google+ <br><br>
        연령:
            <select name="age"></select> <br>
        <input type="button" value="임시저장" onclick="saveTemporary()">
    </form>
</body>
</html>
```

- checkbox
  - `checked` : 체크 확인
  - `value ` : 값 부여

<br>

<br>

# 날씨 API

>1. 회원가입 후 API Key값 복사
>
>   https://home.openweathermap.org/api_keys
>
>2. API 선택 후 테스트
>
>   - ex) https://openweathermap.org/current
>
>   http://api.openweathermap.org/data/2.5/weather?q=Seoul&APPID= 키값
>
>3. 화씨를 섭씨로 변경
>
>   http://api.openweathermap.org/data/2.5/weather?q=Seoul&APPID=643adbd396a1b85d6abb367fc3ad4e60&units=metric
>
>4. 얻고자하는 정보 정리
>
>   - weather
>     - main, desscription, icon
>   - main
>     - temp, temp_min, temp_max
>   - wind
>     - wind.speed
>   - rain
>     - rain.1h
>   - dt
>
>5. 그림
>
>   http://openweathermap.org/img/wn/04d.png

<br>

```html
<!DOCTYPE html>
<html>
<head>
    <script>    //weather의 json 데이터
        let weather_json = `
            {
                "weather": [{
                    "main": "Clouds",
                    "description": "overcast clouds",
                    "icon": "04d"
                }],
                "main": {
                    "temp": 9.27,
                    "temp_min": 9,
                    "temp_max": 10,
                    "pressure": 1015,
                    "humidity": 45
                },
                    "wind": {
                "speed": 1.5
                },
                "dt": 1580272499
            }
        `;

        window.onload = function() {
            let _img = document.getElementById("_img");
            let _temp = document.getElementById("_temp");
            // let _temp_min_max = document.getElementById("_temp_min_max");
            let _wind = document.getElementById("_wind");
            let _temp_min = document.getElementById("_temp_min");
            let _temp_max = document.getElementById("_temp_max");

            // json 파일의 값을 각 tag 값에 저장
            // 1. text(json) -> object(json) : JSON.parse()
            //    object -> text : JSON.stringify()
            // 2. sample.name / sample['address'] (가변데이터 사용 가능 key = address / sample[key])
            
            let parsedJson = JSON.parse(weather_json);
            let _image = document.createElement("img");
            _image.src = "http://openweathermap.org/img/wn/" + parsedJson.weather[0].icon + "@2x.png";
            // _img에 추가
            _img.appendChild(_image);

            _temp.innerText = "현재온도: " + parsedJson.main.temp + "도" + ", " + parsedJson.weather[0].main;
            // _temp_min_max.innerText = parsedJson.main.temp_min 
            // + "/" + parsedJson.main.temp_max;
            _temp_max.innerText = parsedJson.main.temp_max;
            _temp_min.innerText = parsedJson.main.temp_min;
            
            _wind.innerText = "풍속: " + parsedJson.wind.speed + ", 습도: " + parsedJson.main.humidity;

        }
    </script>
</head>
<body>
    <table>
        <tr>
            <td rowspan="2" id="_img"><img></td>
            <td colspan="2" id="_temp">현재온도: 10도, 맑음</td>
            <!-- <td></td> -->
        </tr>
        <tr>
            <!-- <td></td> -->
            <td>
                <span id="_temp_min" style="color: blue";>10</span> /
                <span id="_temp_max" style="color: red";>20</span>
            </td>
            <td id="_wind">풍량</td>
        </tr>
    </table>
</body>
</html>

```

<br>

<br>

# 시계

```html
<!DOCTYPE html>
<html>
<head>
    <script>
        window.onload = () => {
            let clock = document.getElementById("clock");

            setInterval(function() {
                clock.innerHTML = new Date().toString();
            }, 1000);   
        }
    </script>
</head>
<body>
    <h1 id="clock"></h1>
</body>
</html>
```

- setInterval

  : 일정한 시간 후 반복적으로 호출됨

<br>

<br>

# Javascsript Event

이벤트 : 마우스 또는 키보드등에 의해 발생됨



### 시계

```html
<!DOCTYPE html>
<html>
<head>
    <script>
        window.onload = () => {
            let clock = document.getElementById("clock");

            setInterval(function() {
                clock.innerHTML = new Date().toString();
            }, 1000);   

            clock.onclick = function() {
                clock.style = "color: blue;";
                clock.style.backgroundColor = "pink";
            };
        }
    </script>
</head>
<body>
    <h1 id="clock"></h1>
</body>
</html>
```

- 시계 클릭시 색상 변경
  - onclick() 사용

<br>

<br>

### 카운터

```html
<!DOCTYPE html>
<html>
<head>
    <script>
        window.onload = () => {
            let counter = document.getElementById("counter");

            let plusBtn = document.getElementById("plus");
            plusBtn.onclick = function() {
                counter.innerText++;
            }

            let minusBtn = document.getElementById("minus");
            minusBtn.onclick = function() {
                counter.innerText--;
            }
        }
    </script>
</head>
<body>
    <h1 id="counter">0</h1>
    <button id="plus">+</button>
    <button id="minus">-</button>
</body>
</html>
```

<br>

<br>

### form 전송

```html
<script>
window.onload = function() {
            let btnNext = document.getElementById("btnNext");
            btnNext.onclick = function() {
                let name = document.getElementById("txtName");
                let id = document.getElementById("txtId");
                let pwd = document.getElementById("txtPwd");

                if (name.value == "") {
                    alert("이름은 필수 입력입니다.");
                    return;
                }
                if (id.value == "") {
                    alert("아이디는 필수 입력입니다.");
                    return;
                }
                if (pwd.value == "") {
                    alert("비밀번호는 필수 입력입니다.");
                    return;
                }
                // 다음 단계 or form.action 페이지로 이동
                let frm = document.getElementById("registForm");
                frm.action = "next.html";
                frm.submit();
            }
        }
</scstip>
```

