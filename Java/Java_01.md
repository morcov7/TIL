# **:cloud_with_rain:Java #01**

- 컴파일러 언어
- 주로 웹, 모바일의 Server Side 개발에 사용됨
- 오라클 라이선스 / OpenJDK
- 특징
  - 모든 운영체제에서 실행 가능 (WORA : Write Once Run Anyway)
  - 객체 지향 프로그래밍(OOP : Object Oriented Programming)
  - 메모리 자동 정리 by Garbage Collector

<br>

#### Java 개발 도구 (JDK : Java Development Kit)

- JDK : 개발 도구
  - SE / ME / EE
- JRE (Java Runtime Environment) : 실행 환경

<br>

<br>

## 1. HelloWorld 출력

Java는 대소문자를 철저히 구분하기에 주의해야함

<br>

```java
import java.lang.*;		//1.

public class HelloWorld {	//2.
    public static void main(String[] args) {	//3.
        System.out.println("Hello World!");		//4.
    }
}
```

1. `import` : java.lang의 라이브러리를 사용 선언
2. `public class` : 외부에서 사용 가능한 Class 생성
   - class : 자바의 기본 단위
3. Method 선언
   - **method** : 객체 지향 언어에서 function을 의미
   - `System.out.println` : System 패키지 안에 포함되어있는 out 안의 println을 호출
4. `println` 메소드 호출해서 사용

<br>

### Compile 후 실행

```powershell
C:\Users\HPE\class\java\day1>javac HelloWorld.java	//컴파일

2020-02-17  오후 03:42               426 HelloWorld.class
2020-02-17  오후 03:40               149 HelloWorld.java

C:\Users\HPE\class\java\day1>java HelloWorld	//실행
```

- `Class`파일 생성됨
  - 자바 소스 파일 (.java) → **Compile** → Byte Code 파일(.Class)
    - 기계어, binary code
    - binary code가 실행될 수 있는 환경 : Java Virtual Machine
- 컴파일 : `javac`
- 실행 : `java`
  - 조건 :  실행하기 위해서는 void main()이 있어야한다

<br>

<br>

### 프로그램 소스 분석

1. **패키지** 선언
   - 자바 파일을 용도별로 구분
2. **클래스** 선언
3. **메소드** 선언
   - 함수
4. **주석**
   - //
   - /* */
   - /** */ : Java document 주석

<br>

<br>

### 규칙

- 모든 클래스명은 파일명과 동일
- `public static void main (String[] args){}` :  Java 프로그램의 시작점
- **Refactoring** : 코드를 다시 만드는 과정

<br>

<br>

## 2. Data Type

- 캐스팅 : 큰쪽의 데이터를 작은 쪽의 데이터에 넣고 싶을 때

<br>

```java
package com.example;

public class HelloWorld {
    public static void main(String[] args) {
        int a = 10;
        double pi = 3.14;
        float pi2 = 3.14f;
        float pi3 = (float)3.14;

        System.out.println(a*a*pi);
    }
}
```

- 실수 리터럴
  - `e` : 지수
  - 5e2 = 5 * 10^2
- 실수 타입
  - double : 자바는 실수 리터럴을 기본적으로 double로 지정
  - float
- boolean

<br>

<br>

## 3. 타입 변환

>- 자동 타입 변환
>- 강제 타입 변환
>- 문자열 결합 연산
>- Integer.parseInt()

<br>

### 자동 타입 변환 (promotion)

- *값의 허용 범위*가 작은 타입이 큰 타입으로 저장될 경우
  - byte < short < int < long < float < double
- 산술연산시 피연산자의 Data Type로 변환됨
  - 덧셈 연산 중  피연산자 중 하나가 문자열일 경우 전체가 문자열로 변환됨

<br>

### 강제 타입 변환 (casting)

- 큰 허용 범위 타입을 작은 허용 범위 타입으로 강제로 나누어 저장
- 괄호 `()` 사용 : 괄호 안이 나누는 단위

```java
double doubleValue = 3.14;
int intValue = (int) doubleValue;
```

<br>

### parse 메소드

Primitive Type를 Class형태로 사용하기 위해 Wrapper Class를 사용

Wrapper Class에는 parse메소드가 제공됨

- **Integer.parseInt**("123")
  - String → Int
- Double.parseDouble()

````java
String korJumsu = "100";
int total = 0;
total += Integer.parseInt(korJumsu);

System.out.println("kor = " + total);
````

<br>

### String.valueOf()

- 숫자를 문자로 변경
- String str = "" + 3;

<br>

## 4. Scanner

키보드로부터 입력된 내용을 쉽게 읽기 위해 Scanner 사용

<br>

````java
import java.util.Scanner;	//import

public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("HI");

        Scanner s = new Scanner(System.in);
        System.out.print("국어점수=");
        Int kor = s.nextInt();

        System.out.println(kor);
    }
}
````

- 표준 출력 장치 (모니터)
- 표준 입력 장치 (키보드)
- **System.in.read** : 입력받을 때 사용
  - 단점 : 한 글자만 입력받음
- String.format : printf같이 사용 가능

```java
String result1 = String.format("총점=%s 평균=%s", total, avg);
```

<br>

### equals() 메소드

문자열의 데이터가 같은지 비교

```java
String str = "java";

boolean r1 = str.equals("java");
```

<br>

<br>

## 5. 연산자

- **instanceof** : 해당하는 개체가 특정한 데이터타입인지 true/false로 return
- **삼항** : (조건식) ? A : B
  - true면 A false면 B
- **a++** : a를 사용 후 +1
- **++a** : +1 후 a 사용

<br>

### Switch문

````java
switch(b) {
    case 10:
        System.out.println("10을 선택하셨습니다.");
        break;
    case 9:
        System.out.println("9을 선택하셨습니다.");
        break;
    case 8:
        System.out.println("8을 선택하셨습니다.");
        break;
    default:
        System.out.println("디폴트라능ㅎ_ㅎ");
        break;
}
System.out.println("END");
````

- int , char, String만 사용 가능

<br>

<br>

## 6.

````java
public class Example3 {
    public static void main(String[] args) {
        String name="이도원";
        String hobby="JAVA";	

        System.out.println(name == hobby);	//false
        String name1 ="이도원";
        String name2 = "이도원";

        System.out.println(name1 == name2);	//true

        String newName1 = new String("이도원");
        String newName2 = new String("이도원");
        System.out.println(newName1 == newName2);	//false
    }
}
````

- 문자열비교
  - `==` : 번지 비교
  - `equals` : 문자열비교

<br>

| Stack |  Heap  |
| :---: | :----: |
| name  | 이도원 |
| hobby |  JAVA  |

- Stack과 Heap에 들어가는 Data type이 다르다