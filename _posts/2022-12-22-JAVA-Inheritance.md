---
title : 07. Inheritance
date : 2022-12-22 +09:00
categories : [KH 학원 공부, JAVA]
tags : [
  JAVA,
]
---
<!-- ![](/assets/img/java/array2.png) -->

### 상속

1) 정의
- 다른 클래스(부모)가 가지고 있는 멤버(필드, 메소드)들을 새로 작성할 클래스(자손)에 직접 만들지 않고 상속을 받음으로써 자신의 멤버처럼 사용하는 기능

2) 목적
- 클래스의 재사용
- 연관된 모든 클래스들의 공통적인 규약 정의

3) 장점
- 적은 양의 코드로 새로운 클래스 작성 가능
- 코드를 공통적으로 관리하므로 코드의 추가 및 변경이 용이
- 코드의 중복을 제거함으로 프로그램의 생산성과 유지보수가 용이

4) 방법 및 표현식
- 클래스 간의 상속시에는 extends 키워드 사용.

```java
<표현식>
[접근제한자] class 클래스명 extends 클래스명{}

public class Academy extends Company{}
=> Academy (자손), Company (부모)
```

5) 상속의 특징
- 모든 클래스는 Object 클래스의 후손: Object 클래스가 제공하는 메소드를 오버라이딩하여 사용따로 부모 클래스를 설정하지 않을 시에 컴파일러가 컴파일시 자동으로 Object를 부모 클래스로 지정.

```java
class Tv {...}

class SmartTv extends Tv{...}

컴파일 진행시 class Tv -> class Tv extends Object{...}로 됨
```

- 부모클래스의 생성자, 초기화블록은 상속 되지 않는다: 생성자의 {}안의 맨위에는 생성자를 호출하는 생성자(this, super)가 있다(없을시 컴파일러가 자동으로 추가) 따라서 자식클래스 생성시, 생성자가 실행되면 부모클래스의 생성자가 먼저 실행된다. 명시하고 싶을 때는 super()를 쓴다.

```java
class Point{
  int x, y;
  
  Point(int x, int y){
    this.x = x;
    this.y = y;
  }

  String getLocation(){
    return "x : " + x + ", y : " + y;
  }
}

class Point3D extends Point{
  int z;

  Point3D(int x, int y, int z){
    this.x = x;
    this.y = y;
    this.z = z;
  }

  String getLocation(){
    return "x : " + x + ", y : " + y + ", z : " + z;
  }
}

=> 자식클래스의 객체를 생성하게 되면 에러가 발생할 것이다.
그 이유는

Point3D(int x, int y, int z){
  super(); // 컴파일러가 생성자의 첫줄에 생성자호출을 하는 문장을 넣어준다
  this.x = x;	// 그리고 이렇게 부모의 멤버는 자식이 수정하는 것보다는
  this.y = y; // super(x, y)로 부모의 매개변수 생성자를 호출하는게 좋다
  this.z = z;
} 
==> super(x, y)
  this.z = z;

그렇게 되면 super()는 조상클래스인 Point의 기본생성자 Point(){ }를 호출하는 코드인데 
Point에는 기본생성자가 존재하지 않기 때문에 에러가 발생한 것이다
```

- 부모의 private멤버는 상속은 되지만 직접 접근은 불가능하다 ==> super()를 이용하여 부모의 생성자쪽으로 값을 넘겨 생성하거나 getter/setter 메소드를 이용하여 접근하여야 한다

6) super()와 super.
- super()
  - 자식 내 부모객체 생성 및 부모객체의 초기화
  - 부모객체 생성자를 호출하는 메소드
  - 매개변수 생성자의 호출은 super( 매개변수, 매개변수 )

- super.
  - 부모객체를 가리키는 참조변수
  - 부모객체의 필드나 메소드 호출에 사용
  - 부모의 멤버변수와 자식의 멤버변수의 이름이 같다면 this.은 자식의 변수를 super.은 부모의 변수를 가리킴

7) 오버라이딩
- 정의
  - 자식클래스가 부모클래스의 메소드를 재작성하는 것. 수정의 의미
- 특징
  - 메소드의 선언부 위에 @Override 표시> 해당 메소드가 오버라이딩 되었다는 것을 컴파일러에게 알려주는 역할
  - 접근제어자는 부모의 것보다 같거나 넓은 범위로 변경 가능
  - 예외처리는 부모보다 좁은 범위로 수정 가능
- 성립조건
  - 선언부가 동일 : 메소드명, 반환타입, 매개변수(개수, 타입, 순서)
  - private 메소드는 오버라이딩 불가

8) 오버로딩
- 정의 : 한 클래스 내에서 같은 이름의 메소드를 여러개 만드는 것
- 성립조건
  - 같은 메소드명
  - 다른 매개변수 선언부(개수, 타입, 순서)
  - 반환타입, 접근제한자는 오버로딩 조건과 관련이 없다
![](/assets/img/java/Inheritance.png)

9) final 예약어
- final 클래스는 상속이 불가능
- final 메소드는 오버라이딩이 불가능
