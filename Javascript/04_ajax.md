# ajax

- jQuery에서 비동기적인 통신을 하기 위해 사용

- 동기화 : 모든 페이지가 준비 완료시 보여줌

- 비동기화 : 로딩되는대로 보여줌 (읽어 들이는 작업이 오래 걸리더라도 다른 작업 수행 가능)

  - 사용자가 기다리는 시간 최소화

- 클래스 선택자 : `.`

- ID 선택자 : `#`

  





### ajax 메소드의 두 가지 형태

1. $.ajax(options);
   - URL 적어주는 방법
2. $.ajax(url, options);
   - URL 지정

```html
<script>
        $(document).ready(function() {
            $.ajax({
                url: "./data.html",
                success: function(data) {},
                error: function() {}
            });

            $.ajax("./data.html", {
                success: function(data) {},
                error: function() {}
            });

        });

</script>
```

<br>

<br>

## API로 날씨 정보 가져오기

>URL=	(url)
>	http://api.openweathermap.org/data/2.5/forecast
>PARAMETERS=	(data)
>	?q=Seoul&
>	APPID=643adbd396a1b85d6abb367fc3ad4e60&
>	units=metric

<br>

```html
<!DOCTYPE html>
<html>
<head>
    <!-- <script src="../jquery/jquery.js"></script> -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
    <script>
        $(document).ready(function() {
            $.ajax({
                url: "http://api.openweathermap.org/data/2.5/forecast",
                method: "GET",
                data: {
                    q: "Seoul",
                    APPID: "643adbd396a1b85d6abb367fc3ad4e60",
                    units: "metric"
                },

                success: function(data) {
                    console.log(data);
                },
                error: function(err) {
                    console.log("ERROR!" + err);
                }
            });
        });

    </script>
</head>
<body>
</body>
</html>
```

<br>

<br>

# 날씨 API 폼 추가

### (1) button과 input text 추가

```html
<form>
        <input type="text" name="city" id="city" placeholder="도시를 입력하세요">
        <button id="btnSubmit">검색</button>
</form>
```

- input / button 동일하지만 최근에는 button을 선호

<br>

### (2) 버튼 클릭 전/후 데이터 출력여부

```html
<script>
        $(document).ready(function() {
            $('#btnSubmit').on('click', function(e) {
            let city = $('#city').val();
				if (city != null && city != '') {
            		$.ajax({
							...
            	}
            });
    </script>
```

- let city = $('#city').val();
  - input값은 `val()` 을 사용
- NULL XXX / null O

<br>

#### (3) 선택된값을 city로 지정

```html
let city = $('select > option:selected').val();

//body
<select id="city">
        <option value="seoul">서울</option>
        <option value="london">런던</option>
        <option value="tokyo">도쿄</option>
</select>
```

