# :cat2: ​**Java** day 02

## 1. 배열

### 문자열배열

```java
package day2.array;

public class Demo4 {
    public static void main(String[] args) {
        String[] strArray = new String[3];

        strArray[0] = "Java";
        strArray[1] = "Java";
        strArray[2] = new String("Java");

        System.out.println(strArray[0] == strArray[1]);			//True
        System.out.println(strArray[0] == strArray[2]);			//False
        System.out.println(strArray[0].equals(strArray[2]));	//True
    }
}
```

-     stack            heap
      strArray -->  [0][1][2]
                    null null null
      strArray[0] --> "Java"
      strArray[1] --> "Java"
      strArray[2] (new) --> new String("Java")

  - new라는 키워드로 데이터 생성시 reference heap에 새로운 데이터 생성됨

<br>

<br>

### System.arraycopy()

 : 배열 복사

```
System.arraycopy(strArray, 0, newArray, 0, strArray.length);
						처음부터		0번째에
```

````java
package day2.array;

public class Demo5 {
    public static void main(String[] args) {
        String[] strArray = new String[3];
        strArray[0] = "Java 1.8";
        strArray[1] = "Java 1.12";
        strArray[2] = new String("Java 1.13");

        String[] newArray = new String[3];
        System.arraycopy(strArray, 0, newArray, 0, strArray.length);
        for (String str: newArray) {
            System.out.println(str);
        }
    }
}
````

<br>

<br>

### 다차원배열

````java
int[][] scores = new int[2][];
scores[0] = new int[2];
scores[1] = new int[4];

scores[0][0] = 100;
scores[0][1] = 200;

for(int[] row : scores){
    for(int value: row){
    	System.out.print(value + " ");
    }
    System.out.println();
}
````

<br>

<br>

## 2. 클래스

> - 클래스 : 자바의 설계도
> - 인스턴스 : 클래스로부터 만들어진 객체

<br>

### (1) 객체 (Object)

- 현실 세계에서 존재하는 모든 것들을 객체라 볼 수 있다

- 구성

  1. **필드** (**속성** / field)
     - ex) 사람
  2. **메소드** (**동작** / method)
     - ex) 밥먹는다

  ````java
  public class Member {
      // (optional) nested class
      class VipMember { }
  
      // fields
      int age = 10;
      String name = "Java";
      
      // 소유관계
      Project midProject;
      Project finalProject;
  
      // methods
      void displayInfo() {
           System.out.println(String.format("이름은 %s이고, 나이는 %s 입니다", name, age));
      }
      int Method2() {return 1;}
  }
  
  class Project {
      String name;
      String period;
      void budget() {
  
      }
  }
  ````

- 객체 모델링 : 현실속의 객체를 프로그래밍화해 놓는 것

<br>

#### 객체 간의 상호작용

- **메소드** 호출 : 하나의 객체가 다른 객체 이용
  - 클래스의 동작을 호출

<br>

#### 객체간 관계

![객체간의 관계 이미지 검색결과](https://image.slidesharecdn.com/javacafejavaoop2017-190419123905/95/2017-19-638.jpg?cb=1556591191)

1. **집합 관계** (has a) : 부품과 완성품

2. **사용 관계** : 객체 간의 상호작용

3. **상속 관계** : 상위(부모) 객체를 기반으로 하위(자식) 객체 생성

<br><br>

### (2) 인스턴스

인스턴스화 : 정의시켰던 클래스로부터 메모리를 할당받아 사용가능한 객체가되게끔 함

````java
Member mem1 = new Member();	//인스턴스화
Member mem2 = new Member();
	
mem1.age = 10;
mem1.name = "Hello";
mem1.displayInfo();
````

- `mem1` : 인스턴스, 객체
- ` Member()` :생성자 메소드
  - 클래스와 동일한 이름의 메소드
  - 자바에서 클래스를 만들면 명시적으로 선언하지 않아도 생성자 메소드가 **자동적으로 생성**됨

<br>

<br>

### (3) 생성자

자바의 생성자는 데이터값을 **초기화**할 때 사용됨

````java
public class Member {
    String name;
    int age;
    Project finalProject;

    // 생성자 메소드
    Member() {
        name = "Java";
        age = 10;
    }
    
    // 생성자 메소드 with 매개변수
    Member(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    // 생성자 메소드 with one 매개변수
    Member(String name) {
        this.name = name;
    }
}

----------------------------------
Member mem1 = new Member("java", 10);
Member mem2 = new Member("python");
````

- `this` : 자기 자신을 가리키는 인스턴스의 고유값
- 생성자 메소드들이 Overloading된 상태

<br>

### Overloading

- 같은 클래스에서 메소드의 이름은 동일하고, 파라미터의 타입이나 개수가 다름

- 같은 이름의 메소드를 여러개 만들어 둔 것

<br>

<br>

## 3. 성적 출력 예제

### Student.java

````java
package day2.array;

public class Student {
    String name;
    int kor;
    int eng;
    int mat;
    int sum;
    float avg;

    Student (String name, int kor, int eng, int mat) {
        this.name = name;
        this.kor = kor;
        this.eng = eng;
        this.mat = mat;
    }

    void calculate() {
        this.sum = this.kor + this.eng + this.mat;
//        sum = kor + eng + mat;
        this.avg = this.sum/3.0f;
    }

    void display() {
        System.out.println(String.format("%s의 총점: %s, 평균: %.2f"
                                         , this.name, this.sum, this.avg));
    }
}
````

<br>

### SungjukApp.java

````java
package day2.array;

public class SungjukApp {
    public static void main(String[] args) {
        Student stu1 = new Student("AAA", 100, 89, 30);
        Student stu2 = new Student("VVV", 89, 80, 60);
        Student stu3 = new Student("CCC", 10, 54, 68);

        stu1.calculate(); stu1.display();
        stu2.calculate(); stu2.display();
        stu3.calculate(); stu3.display();
    }
}
````

