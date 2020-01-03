# :thinking:JSP VS Servlet

데이터 베이스 프로그래밍 수업에서 다뤘던 개념들을 종강한지 두달도 되지 않아 홀라당 다 까먹었다.

중요한 개념들이니 다시 공부하고자 작성한다.





## 1. Servlet

#### Servlet?

- 자바 기반의 웹 어플리케이션 프로그래밍 기술
- 자바 코드 속에 HTML 소스 코드가 들어가는 구조
- 자바 언어를 기반으로 동적인 contents를 생성

- 서블릿은 HttpServlet을 상속받은 **자바의 클래스**기 때문에 .java라는 확장자를 갖음
- WAS(Tomcat) 위에 동작



#### Servlet의 단점

- Servlet은 프로그램 내에서 HTML을 처리하기 때문에 간단한 표현을 변경할 때에도 컴파일을 다시 해야함

- 복잡한 Servlet 소스 내에 Presentation Logic과, Business Logic이 분리되지 않아 개발과 관리 면에서 여러 문제점 O

  ​		∴ **JSP**가 탄생





## 2. JSP (Java Server Pages)

#### JSP?

- Servlet을 기반으로 한 웹 어플리케이션 프로그래밍 기술
- HTML 소스 코드 속에 Java 코드가 들어가는 형태
- Server Side **스크립트** **언어**
- JSP는 서블릿의 업그레이드 버전으로 WAS에 의하여 서블릿 클래스로 변환하여 사용됨
- JSP가 수정될 경우 재배포할 필요 없이 WAS가 알아서 처리

> **스크립트 언어?**
>
> - 소스 코드를 컴파일(Compile)하지 않고도 실행할 수 있는 프로그래밍 언어
>
> - Server Side Script : 해당 소스의 실행 결과를 확인하기 위해 웹 서버를 거쳐야 하는 스크립트
> - Client Side Script : 내장된 번역기(Internet Explorer 등)에 의해 실행되는 스크립트로  서버를 거치지 않고 사용자 측에서 처리되는 스크립트



## 3. JSP & Servlet

- JSP와 Servlet은 만드는 방법에만 차이가 있고 동일한 역할 수행

- 개발자에게 코딩하기 복잡한 Servlet 대신 스크립트 언어인 JSP로 작성하게 하면
- JSP/Servlet 컨테이너인 Tomcat이 내부에서 JSP 파일을 Servlet으로 변환 및 컴파일하여 클래스 파일을 메모리에 적재한 후 실행하여 응답함
- 보통 MVC 구조에서
  - Servlet → Controller (Business Logic)에 좋음
  - JSP → View (Presentation Logic)에 좋음





## 4. WAS (Web Application Server)

- WAS = **Web Server + Web Container**
- DB 조회나 다양한 로직 처리를 요구하는 **동적인 컨텐츠**를 제공하기 위해 만들어진 서버
- 주로 DB 서버와 같이 수행됨
- ex)Tomcat, tMAx jeus 등



#### Web Container

- JSP와 Servlet을 실행시킬수 있는 소프트웨어를 웹 컨테이너 or 서블릿 컨테이너라고 부름
- 웹 서버에서 JSP를 요청하면 Tomcat에서 JSP 파일을 Servlet으로 변환해 컴파일하고 서블릿 수행 결과를 다시 웹서버에 전달



#### WAS 내 JSP가 동작하는 구조

![jspContainer](http://myblog.opendocs.co.kr/wp-content/uploads/2014/10/jspContainer.png)

​										출처 : http://myblog.opendocs.co.kr/archives/440

1. 클라이언트의 HTTP 프로토콜 **요청**
2. 웹서버는 <u>Container</u>에게 처리 요청
3. 해당 파일을 Servlet로 **변환**. 만약 이미 변환되어있으면 그 파일을 바로 실행(5.)
4. Servlet 파일을 실행가능한 Class 파일로 **Compile**
5. Class 파일을 메모리에 적재 후 **실행**한 결과를 웹서버에 넘겨줌
6. 웹서버는 브라우저가 인식 가능한 정적페이지를 구성해 클라이언트에 **응답**



#### Tomcat VS Apache

- Apache
  - Web Server (HTTP 요청 처리)
    - 정적인 컨텐츠 처리
- Tomcat
  - WAS (Web Application Server)
    - 동적인 + 정적인 컨텐츠 처리