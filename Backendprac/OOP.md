# OOP(객체 지향 프로그래밍)

OOP는 객체 지향 프로그래밍(영어: Object-Oriented Programming)의 약자 이다.<br>
그럼 객체 지향 프로그래밍이란 즉, C언어같은 절차 지향적인 프로그래밍이 아닌 객체의 관점에서 프로그래밍을 한다는 것이다.<br>
자바의 경우 그 구성 부분 단위가 클래스이다. 자세히 말하자면 클래스는 설계도고 직접일을하는 구현체는 인스턴스다.

## 객체 지향의 특징

- 캡슐화
    - 캡슐화란 하나의 객체에 대해 그 객체가 특정한 목적을 위한 필요한 변수나 메소드를 하나로 묶는 것을 의미한다.
    - 따라서 클래스를 우리가 만들 떄 훗날 이 클래스에서 만들어진 객체가<br>
        특정한 목적을 잘 수행할 수 있도록 사용해야할 변수와 그 변수를 가지고<br>
        특정한 액션 즉 메서드를 관련성 있게 클래스에 구성해야한다.
- 정보은닉
    - 유저 정보를 가지고 있는 User라는 객체에서 유저의 정보가 public으로 선언되어 있다면,<br>
        누구든 접근해서 유저 정보를 변경할 수 있다.<br>
        그렇기 때문에 private로 해서 데이터를 보호해서 접근을 제한해야한다.

- 상속성, 재사용
    이게 진짜 중요한데
    - 상속이란 기존 상위클래스에 근거하여 새롭게 클래스와 행위를 정의할 수 있게 도와주는 개념이다.
    - 기존 클래스에 기능을 가져와 재사용할 수 있으면서도 동시에 새롭게 만든 클래스에 새로운 기능을 추가할 수 있게 만들어 준다.


```java
public class Parents {
    private String name = "chan woo";
    private int age = 17;

    public void print() {
        System.out.printf("my name is %s.\nI'm %d years old.\n", name, age);
    }
}

public class Child extends Parents {
    String Location;
    public Child(String Location) {
        this.Location = Location;
    }

    public void showMessage() {
        print();
        System.out.printf("I live in %s.", this.Location);
    }
}

public class Main {
    public static void main(String[] args) {
        Child C = new Child("강진");
        C.showMessage();
    }
}

```