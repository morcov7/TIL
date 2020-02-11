# :tropical_fish:Javascript

- 웹 브라우저(클라이언트)에서 많이 사용하는 프로그래밍 언어
- **ECMAScript**, Jscript는 모두 Javascript를 의미

<br>

<br>

## HTML5 기본

```html
<!DOCTYPE html>
<html>
    <head>
        <!-- 자바스크립트 예제 -->
        <script>
            var name = "마라샹궈"; 
            alert('Hello\' ' + name); 
            console.log("Hungry");  
        </script>

    </head>
    <body>
        <img />
        <input type="text" />

        <h1>제목입니다.</h1>
        <!-- <h1>제목입니다.</h1> -->
    </body>
</html>
```

- 가능하면 모든 태그는 Closer를 갖는다고 생각하자
- `<head>` : 외부에서 참조되는 작업
- `<body`> : 화면에 보여지는 모든 작업
- `<script>` : javascript

<br>

- `var` : 변수 선언
- `()` 
  1. function (javascript)
     - 함수를 직접 만들기 or 호출
  2.  method (java)
- `+` : 덧셈, 문자열 결합
- `alert()` : 경고창 띄우기
- `console.log()` : 개발자 도구의 콘솔창 화면 출력됨
  - 디버깅 용도나 결과값 확인에 많이 쓰인다

- 오버 로딩 : 가지고 있는 함수를 재정의

<br>

### 개발자도구  Storage

- 쿠키 등이 보관됨

<br>

<br>

## 기본 용어

### 식별자

- 자바스크립트에서 이름을 붙일 때 사용
  - 변수명, 함수명
- 키워드로 변수명 사용 X (ex. int, public)

<br>

### 주석

- `<!-- -->` : html 주석
- `//` : 라인 주석 (javascrispt)
- `/* */`  : 블록 주석  (javascript)s

<br>

### 비교연산자

- `===` : 값, 데이터 타입도 같다
- `==` : 값이 같다
- `!==` : 값은 같고 데이터 타입은 다르단

<br>

### 함수

- alert()
- confirm() : 데이터값을 화면에 보여줌 / 버튼 2개
- propmt() : 값을 입력 받게끔 창을 띄움

<br>

### 논리연산자

- `!` : not
- `&&` : and
- `||` : or

<br>

<br>

## javascript 변수 선언

### (1) var

- 가변의 값
- 일반변수 선언
- ECMA6에서는 var를 사용하지 않도록 권장

### (2) let

- 블록 안 로컬 변수 선언
- ECMA6에서는 let을 사용하도록 권장
  - 같은 변수명으로 재선언시 오류뜸

### (3) const

- 상수값 선언
- 불변의 값

<br>

<br>

## 연산자 활용

```html
<!DOCTYPE html>
<html>
<head>
    <script>
        window.onload = function() {
            //변수선언
            let list = "";

            //연산자 사용
            list += "<ul>\n";
            list += "    <li>Hello</li>\n";
            list += "    <li>JavaScrpit..!</li>\n";
            list += "</ul>";

            let a = 10;
            console.log(a);
            console.log(a++);
            console.log(++a);
            console.log(a);
            
            let b = "10";
            console.log(typeof a);
            console.log(typeof b);
            console.log(a === b);

            //출력
            document.body.innerHTML = list;
        }
    </script>
</head>
<body>
</body>
</html>
```

- `window.onload` : 화면에 웹 페이지가 처음 뜨면 자동으로 실행되는 이벤트
- `ul` `li` : bulletin
- `documnet.body.innerHTML` : list를 화면에 HTML 형태로 넣어달라

- `typeof` : 데이터의 자료형 확인

<br>

<br>

## Array

```html
<!DOCTYPE html>
<html>
<head>
    <script>
        let myArray1 = ["apple", "banana", "orange"];
        let myArray2 = new Array();
        myArray2[0] = "grape";
        myArray2[1] = "lemon";

        console.log(myArray1);
        console.log(myArray2);

    </script>
</head>
<body>

</body>
</html>
```

<br>

<br>



## Function

```html
<!DOCTYPE html>
<html>
<head>
    <script>
        let func = function() {
            let output = prompt("숫자를 입력해 주세요.");
            alert(output);
        }
        //alert(func);
        func();

        function func2(name) {
            alert("Hello" + name);
        }
        func2("Dongsun");
</script>
</head>
<body></body>
</html>
```

<br>

<br>

### 타이머함수

- setTimeout()
  - 특정한 시간 후에 함수 한번실행
- setInterval()
  - 특정한 시간마다 함수를 실행
- 시간 단위는 milliseconds (1000 = 1초)

```html
<script>
    setInterval(function() {
        console.log("외부 서비스에서 데이터 호출" + new Date());
    }, 3000);  
    
    setTimeout(function() {
            alert('Works!');
        }, 3000);
</script>
```

<br>

<br>

### eval 함수

문자열을 실제 코드처럼  동적으로 실행시켜줌

```html
<script>
    let willEval = "";
    willEval += "var number=10;";
    willEval += "console.log(number);";

    eval(willEval);
</script>
```

<br>

<br>

### parseInt

- parseInt는 숫자로 변환 가능할 때까지 변환시켜준다.

```html
<script>
let a = parseInt("123");
let a = parseInt("123원");	//OK
let a = parseInt("$123원");	//오류
    
let b = parseInt("3.14");

console.log(typeof a);
console.log(typeof b);
</script>
```

<br>

<br>

### 화살표 함수 - ECMAScript 6

- `=>` 를 `function()` 대신 사용
- 함수를 좀더 간략하게 사용하기 위해

```html
<script>
    let func = function() {
    	console.log("hi there");
    }
    func();

    let func2 = () => {
    	console.log("hi there");
    }
    func2();
</script>
```





