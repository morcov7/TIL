# Java Day 3

## 클래스의 다른 생성자 호출

- 중복된 코드 양을 줄이기 위해 → Refactoring
- `this` 키워드 사용

````java
Car(String model) {
	this(model, "은색", 250);
}

Car (String model, String color, int maxSpeed) {
	this.model = model;
	this.color = color;
	this.maxSpeed = maxSpeed;
}
````

<br>

<br>

## Method ex) Calculate

````java
public class Demo1 {
    public static void main(String[] args) {
        Calculate calc = new Calculate();   //stack(calc) heap(new calculate() 객체)
        int result = calc.add(10,5);
        System.out.println(result);
        
        System.out.println(calc.add(1,2,3));
        System.out.println(calc.add(1,2,3, 5, 6));
        
        int[] sum = {100, 200, 300, 400};
        int result4 = calc.add(sum);
    }
}

public class Calculate {
    // default constructor 생략
    // public Calculate() {}

    int add (int x, int y) { return x+y;}
    double add (double x, double y) { return x+y; }		//overloading
    int add (int... arr) {
        int r = 0;
        for (int value : arr) {
            r += value;
        }
        return r;
    }
    
    int subtract (int x, int y) {return x-y;}
    int multiply (int x, int y) {return x*y;}
    int divide (int x, int y) {return x/y;}
}
````

- **Overloading**
  - 같은 클래스 내 이름이 같은 메소드
  - 매개변수의 개수가 다르거나 타입이 달라야함
- `int... arr`
  - 배열 생성하지 않고 여러 값만 넘겨주는 방식
  - 가변데이터 매개변수

<br>

<br>

## Modifier

#### private < X(default) < protected < public

- private
  - 같은 class O / 같은 패키지  X
- X (default)
  - 같은 class O / 같은 패키지 O / 다른쪽의 패키지 X
- protected
  - 같은 class O / 같은 패키지 O / 다른 패키지 O (조건: **상속관계**)
- public
  - 다 O 

<br>

<br>

## 객체 배열

````java
Student[] students = new Student[]{
    new Student("AAA", 100, 89, 30),
    new Student("BBB", 79, 89, 96),
    new Student("CCC", 56, 89, 50)
};

for (Student student: students) {
    student.calculate();
}

for (Student student : students) {
    student.display();
}
````

<br>

<br>

## Overriding

**부모**클래스가 가지고 있었던 메소드를 **재정의**해서 사용

- 조건 : 부모의 메소드 이름과 **동일**해야함

<br>

#### 예시 (toString, equals)

````java
@Override
public String toString() {
    return "나는 재정의된 toString() 임니당 ㅎ_ㅎ";
}

@Override
public boolean equals(Object obj){
    // 캐스팅
    Student temp = (Student)obj;
    return this.kor == temp.kor;
}

------
    				//this				//obj
System.out.println(students[0].equals(students[1]));
````

- Student Class의 부모 클래스인 **Object클래스**의 toString()  메소드를 **Overriding**
- `@Override` annotation
  - 지금 작성하는 메소드가 부모의 메소드를 재정의한다고 명시해두는 것
- Annotation 장점
  - 코드가 짧아짐
  - 정확한 의도 전달

<br>

### Run-time error

실행 후 발생하는 오류

<br>

<br>

## Comparator

````java
public class MyComparator implements Comparator<Student> {
    public int compare (Student s1,  Student s2) {
        if (s1.getSum() > s2.getSum()) {
            return 1;
        } else if (s1.getSum() < s2.getSum()) {
            return -1;
        } else {
            return s1.getName().compareTo(s2.getName());
        }
    }
}
````

<br>

````java
Arrays.sort(students, new MyComparator());	//외부 Comparator
Arrays.sort(students);	//Student implements Comparable

===
public class Student implements Comparable<Student>{
    @Override
    public int compareTo(Student o) {
        if (this.getAvg() > o.getAvg()) {
            return 1;
        } else if (this.getAvg() < o.getAvg()) {
            return -1;
        } else {
            return this.getName().compareTo(o.getName());
        }
    }
}
````

1. MyComparator 따로 구현
   - POJO 상태에서 기능 추가
   - 외부 확장 용이
   - 좀더 선호
2. Student에서 Comparator implement

<br>

<br>

## 객체지향언어

### 인스턴스 (instance) 멤버

객체를 생성한 후 사용할 수 있는 필드와 메소드

- `this` :  객체 내에서 인스턴스 멤버에 접근하기 위해 사용

<br>

### 정적 (static) 멤버

클래스에 고정된 멤버

- `static` 으로 선언된 변수는 **인스턴스 없이** 사용 가능
- 공용으로 **재사용**하고자 하는 데이터는 static으로 선언
- 클래스 이름과 함께 도트 연산자로 접근
  - 클래스.필드;
  - 클래스.메소드( );

<br>

```java
public class Counter {
    private static int count;
    public static void addCount() {
        count++;
    }

    public int getCount() {
        return count;
    }
}
```

<br>

```java
Counter.addCount();

Counter c1 = new Counter();
System.out.println(c1.getCount());
```

- `static`로 선언된 메소드는 바로 사용 가능
- static으로 선언되지 않은 메소드는 인스턴스 생성 후 사용 가능

<br>

<br>

## public static void main?

- 외부에서 인스턴스 없이 호출 가능
- **JVM**이 main 함수를 호출
  - eX) Demo3.main()

<br>

<br>

## 싱글톤 (Singleton)

**단 하나의 객체**를 만들어 사용

- 매번 `new`로 새로운 data를 생성하지 않고 만들어진 data를 **재사용**
- 메모리 확보에 도움됨
- 공용 데이터를 사용하기 위해

<br>

````java
public class Demo4 {
    private static Demo4 obj = new Demo4();
    private Demo4() { }

   public static synchronized Demo4 getInstance() {
        if (obj == null) {
            obj = new Demo4();
        }
        return obj;
    }
}
````

- obj가 만들어 지지 않았으면 새로운 obj를 생성하고 return
- synchronized : 데이터 요청이 순차적으로 처리됨

<br>

````java
public class SingletonDemo {
    public static void main(String[] args) {
        //Demo4 demo4 = new Demo4();
       Demo4 obj1 = Demo4.getInstance();
       Demo4 obj2 = Demo4.getInstance();

       System.out.println(obj1);
       System.out.println(obj2);
       System.out.println(obj1 == obj2);	//true

       Calendar cal = Calendar.getInstance();
    }
}
````

<br>

#### 싱글톤 패턴의 예 : Calendar 

```java
Calendar cal = Calendar.getInstance();
System.out.println(cal.get(Calendar.YEAR));
```

<br>

<br>

## final 필드와 상수

- final 필드 : 초기값이 저장되면 최종값이 되어 수정 불가

```java
final float point = 0.3f;
public static final String COMPANY = "Java";

System.out.println(Math.PI);
System.out.println(Math.max(10, 20));
```

