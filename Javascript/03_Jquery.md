# jQuery

<br>

<br>

## jQuery 사용하기

### 1. jQuery 라이브러리 불러오기

#### (1) 파일 사용

```html
<script src="../jquery/jquery.js"></script>
```

#### (2) CDN 사용

```html
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
```

<br>

### 2. jQuery 시작옵션

```html
<script>
        $(document).ready(function() {

        });
    
    // OR
    	$(function() {

        });
</script>
```

- 항상 이걸로 시작하기에 외워두는게 좋다
- `$`표시는 jquery에서만 사용 가능
- 모든 시작함수가 실행되는 부분

<br>

<br>

## jQuery의 장점

- 만들고자하는 코딩의 양을 줄일 수 있음

```html
<!DOCTYPE html>
<html>
<head>
    <!-- <script src="../jquery/jquery.js"></script> -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
    <script>
        $(document).ready(function() {
            $("#btnRed").on('click', function() {
                $("#myH1").css('color', 'red');
            });
            $("#btnBlue").on('click', function() {
                $("#myH1").css('color', 'blue');
            });
        });

    </script>
</head>
<body>
    <h1 id="myH1">Hello, jQuery</h1>
    <button id="btnRed">RED</button>
    <button id="btnBlue">BLUE</button>
</body>
</html>
```

- `$("#아이디값")`

- `on('click')` 이 방법이 좀더 좋음

<br>

<br>

## jQuery의 자손

```html
<!DOCTYPE html>
<html>
<head>
    <!-- <script src="../jquery/jquery.js"></script> -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
    <script>
        $(document).ready(function() {
            $('body > *').css('color', 'red');
            $('div > ul').css('color', 'red');
        });
    </script>
</head>
<body>
    <div>
        
        <ul>
            <li>Apple</li>
            <li>Bag</li>
            <li>Cat</li>
            <li>Dog</li>
            <li>Elephant</li>
        </ul>
    </div>
</body>
</html>
```

### `>` 자손 태그

바로 밑에 있는 자손을 지칭

- $('body > *')` : <body>의 자손
- `$('div > ul')` : <div>의 자손인 <ul>
- ``$('div > li')`` : 바뀌지 않음
  - `$('ul > li')` : 바뀜

<br>

### `공백` 사용

밑의 모든 자손들 중 일치하는 것을 지칭

- `$('div li')` : div밑의 모든 후손들 중에 li 선택하기에 바뀜 

<br>

<br>

## jQuery Value

### `val`

1. $('#name').val();
   
   - 인자 X
   
   - 값을 가져오는 **getter** 함수
   
2. $('#name').val('test');

   - 인자 O

   - 값을 지정하는 **setter** 함수

<br>

```html
<!DOCTYPE html>
<html>
<head>
    <!-- <script src="../jquery/jquery.js"></script> -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
    <script>
        $(document).ready(function() {
            $('input[TYPE="text"]').val('test value');
            $('#btn1').on('click', function() {
                console.log($('#input1').val());
                console.log($('#input2').val());
                console.log($('#input3').val());
                console.log($('#input4').val());
                console.log($('#input5').val());
                console.log($('#input6').val());
            })
        });
    </script>
</head>
<body>
    <div>
        입력필드(일반):     <input type="text" id="input1"> <br/>
        입력필드(일반2):     <input type="text" id="input5"> <br/>
        입력필드(비밀번호): <input type="password" id="input2"> <br/>
        라디오:             <input type="radio" id="input3"> <br/>
        체크박스:           <input type="checkbox" id="input4"> <br/>
        <!-- 파일:               <input type="file" id="input5"> <br/> -->
        히든(숨김):         <input type="hidden" id="input6"> <br/>
        <button type="button" id="btn1">전송</button> <br/>
    </div> 
</body>
</html>
```

- `btn1` 이라는 id를 가진 버튼에 click `이벤트` 발생

```
$('#btn1').on('click', function() {
                console.log($('#input1').val());
}
```

- input 태그 중에 type에 text인 것만 value 지정
  - `[TYPE="text"]`

```
$('input[TYPE="text"]').val('test value');
```

<br>

<br>

## jQuery 속성 선택자

| 선택자 형태    | 설명                                               |
| -------------- | -------------------------------------------------- |
| 요소[속성^=값] | 속성 안의 값이 특정 값으로 시작하는 문자 객체 선택 |
| 요소[속성$=값] | 속성 안의 값이 특정 값으로 끝나는 문자 객체 선택   |
| 요소[속성*=값] | 속성 안의 값이 특정 값 포함하는 문자 객체 선택     |

<br>

<br>

## select 태그 

### setTimeout()

지정한 시간 후 기능을 한번 실행

1. 어떤 기능을 할 것인지 function
2. 몇초 뒤에 실행할 것인지 시간

<br>

```html
<script>
        setTimeout(function() {
            // let selectedItem = $('#mySelect').val();
            // let selectedItem = $('select').val();
            let selectedItem = $('select > option:selected').val();
            console.log(selectedItem);
        }, 5000);
    
    	setInterval(function() {
            // let selectedItem = $('#mySelect').val();
            // let selectedItem = $('select').val();
            let selectedItem = $('select > option:selected').val();
            console.log(selectedItem);
        }, 5000);
</script>
```

<br>

<br>

## input 값 가져오기

```html
<!DOCTYPE html>
<html>
<head>
    <!-- <script src="../jquery/jquery.js"></script> -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
    <script>
        $(document).ready(function() {
            $('input[TYPE="text"]').val('test value');
            $('#btn1').on('click', function() {
                console.log($('#input1').val());
                console.log($('#input2').val());
                console.log($('#input3').val());
                console.log($('#input4').val());
                console.log($('#input5').val());
                console.log($('#input6').val());
            })
        });

        setInterval(function() {
            // let selectedItem = $('#mySelect').val();
            // let selectedItem = $('select').val();
            let selectedItem = $('select > option:selected').val();

            // 선택된 라디오 버튼의 값, 체크박스의 값 출력
            let checkedRadio = $('#input3:checked').val();
            let checkedCheckbox = $('#input4:checked').val();
            
            console.log($('input[name="gender"]:checked').val());

            console.log(selectedItem + "/" + checkedRadio + "/" + checkedCheckbox);
        }, 5000);

    </script>
</head>
<body>
    <div>
        입력필드(일반):     <input type="text" id="input1"> <br/>
        입력필드(일반2):     <input type="text" id="input5"> <br/>
        입력필드(비밀번호): <input type="password" id="input2"> <br/>
        라디오(남성):       <input type="radio" id="input3" name="gender" value="m"> <br/>
        라디오(여성):       <input type="radio" id="input3" name="gender" value="f"> <br/>
        체크박스:           <input type="checkbox" id="input4" name="food" value="pizza">피자 
                            <input type="checkbox" id="input4" name="food" value="mara">마라 <br/>
        <!-- 파일:               <input type="file" id="input5"> <br/> -->
        히든(숨김):         <input type="hidden" id="input6"> <br/>

        <select id="mySelect">
            <option value="apple">사과</option>
            <option value="banana">바나나</option>
            <option value="orange">오렌지</option>
            <option value="키위">kiwi</option>
        </select>

        <button type="button" id="btn1">전송</button> <br/>

    </div> 
</body>
</html>
```

- 지정하기
  1. 아이디 사용
     - $('#mySelect').val();
  2. 태그 사용
     - $('select').val();
- 자손 태그 사용

```
console.log($('input[name="gender"]:checked').val());
```

<br>

<br>

## jQuery 배열

### `each()` 메서드 이용

jQuery에서 배열을 관리할 때 each() 메서드를 사용한다.

1. $.each(object, function(index, item) {})
2. $(selector).each(function(index,item) {})
   - html 태그의 요소들을 가져올때 많이 쓰임

<br>

```html
<script>
        $(document).ready(function() {
            let array = [
            {name:'Naver', link:'https://www.naver.com'},
            {name:'Google', link:'https://www.google.com'},
            {name:'Daum', link:'https://www.daum.net'},
            {name:'Multicampus', link:'https://www.multicampus.com'},
            ];
            
            for (let i = 0; i < array.length; i++) {
                console.log(array[i].name);
            }
            
              $.each(array, function(index, item){
                console.log(item);
                console.log(item.name + "/" + item.link);
            	});
        });
</script>
```

<br>

<br>

