---
title : 08. Polymorphism
date : 2022-12-23 +09:00
categories : [KH 학원 공부, JAVA]
tags : [
  JAVA,
]
---
<!-- ![](/assets/img/java/array2.png) -->

### 다형성
- 상속 + 컴퓨터 연산규칙(같은 자료형끼리만 연산 가능) + 얕은 복사

1) 정의
- 상속을 이용한 기술로 부모 클래스 타입의 참조변수 하나로, 상속관계에 있는 여러타입의 자식 객체를 참조할 수 있는 기술
![](/assets/img/java/Polmorphism.png)

2) 업캐스팅
- 상속관계에 있는 클래스간에 부모타입의 참조형 변수가 모든 자식 타입 객체의 주소를 참조할 수 있다
- 자동으로 형변환이 일어난다

```java
부모클래스 변수명 = new 자식클래스();
--------------------------------------------------
Car c = new Tesla();


부모타입의 변수를 통해서 접근할 수 있는 객체의 정보는 부모로 부터 상속받은 멤버만 
참조 가능하다.

public class Car(){
  private String Engine;
  private String fuel;
  private int wheel;
  ...
}

public class Tesla extends Car(){
  private int batteryCapacity;
  ...
  
  public int batteryChangePrice(){
    int result = 0;
    
    if(batteryCapaciy <= 1000) result = 1000;
    else if (batteryCapaciy <= 2000) result = 2000;
    else result = 3000;
    
    return result;       
  }
}

----------------------------------------------------------------------------

import ...Car;
import ...Tesla;

public class Service(){
  Car c = new Tesla();

  c.getEngine();	// 가능
  c.getFuel();	// 가능
  c.getWheel();	// 가능
  c.getBatteryCapacity(); // 불가능
  c.batteryChangePrice();	// 불가능
}
-> batteryCapacity변수와 batteryChangePrice메소드는
Tesla(자식)의 고유멤버이므로 Car(부모) 타입의 참조변수 c는 호출할 수가 없다
```

![](/assets/img/java/Polmorphism2.png)

3) 다운캐스팅(Down Casting)
- 자식 객체의 주소를 받은 부모타입의 참조형 변수를 이용하여 자식객체의 고유 멤버를 참조해야 할 경우 사용
- 부모 클래스 타입의 참조형 변수를 자식 클래스 타입으로 강제형변환 하는 것
- 자동으로 일어나지 않기 때문에 반드시 자식 타입을 명시하여 형변환

```java
Car c = new Tesla();
((Tesla)c).batteryChangePrice();

-> (Tesla)c.batteryChangePrice(); 의 경우 "." 연산자가 "(자료형)" 강제형변환 
연산자보다 연산이 우선순위에 있으므로 형변환이 일어나기전에 메소드 호출이 진행된다
그러므로 ()를 이용하여 형변환이 먼저일어나도록 해줘야 한다
```

![](/assets/img/java/Polmorphism3.png)

4) 객체배열과 다형성
- 다형성을 이용하여 상속 관계에 있는 하나의 부모 클래스 타입의 배열을 만들고, 여러종류의 자식 클래스 객체를 저장한다

```java
Car[] carArr = new Car[3];

carArr[0] = new Tesla();
carArr[1] = new Spark();
carArr[2] = new Sonata();
```

5) 매개변수와 다형성
- 다형성을 이용하여 메소드 호출시에 매개변수를 부모 타입의 매개변수로 지정해서 자식 타입의 객체를 받을 수 있음

```java
public void execute(){
  driveCar(new Tesla()); // Car t = new Tesla(); -> driveCar(t);
  driveCar(new Spart());
  driveCar(new Sonata());
}

public void driveCar(Car temp){}
```

6) 바인딩
- 실제 실행할 메소드와 호출하는 코드를 연결 시키는 것
- 정적바인딩, 동적바인딩이 있다

```java
<정적바인딩>
Car c1 = new Car("경유엔진", "경유", 8);
System.out.println(c1.toString());

=> 프로그램 실행 전 컴파일러는 toString() 메소드가 Car에 있는걸로 인식해서 
c1.toString() 호출코드와 String edu.kh.poly.model.vo.Car.toString()
메소드 코드를 연결한다 -> 정적바인딩
※toString()은 필드 값들을 리턴하는 메소드로 오버라이딩 함

<동적바인딩>
Car c2 = new Tesla("경차엔진", "휘발유", 4, 20000);
-> 업캐스팅 되어 있는 상태, 부모 멤버만 참조 가능한 상태
System.out.println(c2.toString());

=> String edu.kh.poly.model.vo.Car.toString() 연결,
c2가 Car 타입이므로 toString()을 Car toString()을 호출 (정적바인딩)

막상 시행해보면 부모 멤버인 engine, fule, wheel 뿐만 아니라
batteryCapacity의 값도 같이 출력된다. 즉, Tesla의 toString()이 호출됨

컴파일 : 부모(Car)와 바인딩 -> 정적바인딩
실행 : 자식(Tesla)의 오버라이딩된 메소드와 바인딩 ->동적바인딩

장점 : 업캐스팅된 상태에서 다운캐스팅 없이 자식의 오버라이딩된 메소드를 수행
```

7) instanceof 연산자
- 현재 참조형 변수가 어떤 클래스 형의 객체 주소를 참조하고 있는지를 확인 할 때 사용. 클래스의 타입이 맞으면 true, 틀리면 false 반환

```java
if(레퍼런스 instanceof 클래스타입){
  true일 때 처리할 내용, 해당 클래스 타입으로 down casting
} 

if(c instanceof Tesla){
  int result = ((Tesla)c).getBatteryCapacity();
} else if (c instanceof Spark){
  double result = ((Spark)c).getDiscountOffer();
}
```

8) 추상(Abstract)
```java
추상 클래스 : 몸체 없는 메소드를 포함한 클래스(미완성 설계도), 상속 + 다형성
[접근제한자] abstract class 클래스명{ }

추상 메소드 : 몸체 없는 메소드, 상속시 반드시 구현해야 하는, 오버라이딩이 강제화되는 메소드
[접근제한자] abstract 반환형 메소드명(자료형 변수명);
```

- 특징
  - 미완성 클래스(abstact 키워드 사용) : 자체적으로 객체 생성 불가 -> 반드시 상속하여 객체 생성
  - abstract 메소드가 포함된 클래스는 반드시 abstract 클래스 : abstract 메소드가 없어도 abstract 클래스 선언 가능 (abstract 메소드가 0개 이상)
  - 클래스 내에 일반 변수, 메소드 포함 가능    
  - 객체 생성은 안되지만 참조형 변수 타입으로는 사용 가능
- 장점
  - 상속 받은 자식에게 공통된 멤버를 제공하고, 일부 기능의 구현을 강제화 (자식클래스에서 재정의)

9) 인터페이스(Interface)
- 정의
  - 상수형 필드와 추상 메소드만을 작성할 수 있는 추상 클래스의 변형체, 상속 시 인터페이스 내에 정의된 모든 추상메소드를 구현해야 함

```java
[접근제한자] interface 인터페이스명{

  [public static final] 자료형 변수명 = 초기값; // 변수명은 대문자로..

  [public abstract] 반환자료형 메소드명([자료형 매개변수]);
    // public abstract가 생략 가능하기 때문에 
    // 오버라이딩 시 반드시 public 표기해야
}
```

- 사용
  - 어떤 객체가 이미 상속을 받고 있는데 다른 타입을 참고해야 할 경우 인터페이스를 사용한다.

```java
포유류 : 고양이, 개, 고래
조류 : 닭, 독수리, 펭귄
어류 : 상어

고래, 펭귄, 상어 => 물속생활 클래스로 상속 가능??
=> No, 이미 포유류, 조류, 어류의 자손이고, 단일 상속만 가능하기 때문에 이때 사용하는게
인터페이스
```

- 특징
  - 모든 인터페이스의 메소드는 묵시적으로 public abstract
  - 변수는 묵시적으로 public static final    
  - 객체 생성은 안되나 참조형 변수로는 가능 (다형성)    
  - 다중 상속 가능하기 때문에 여러개의 인터페이스 추가 가능    
    ex) public class Whale extends TypeMammalia implements WaterLife, BigAnimal, GoodAnimal{ } 
- 장점    
  - 다형성을 이용하여 상위 타입 역할 (자식 객체 연결)    
  - 인터페이스 구현 객체에 공통된 기능 구현 강제화 (== 구현 객체간의 일관성 제공)    
  - 공동 작업을 위한 인터페이스 제공    
- 추상 클래스 vs 인터페이스
![](/assets/img/java/Polmorphism4.png)

10) 정리
- 상속(자식클래스의) 공통된 부분을 추출하여 부모클래스를 만드는 것  
  -> 공통된 필드, 메서드를 가진 클래스를 만들고, 작성된 코드를 자식들이 물려받아 사용  
  -> 장점 : 코드 길이 감소, 코드 중복 제거, 재사용성 증가, 자식에 대한 일관된 규칙 제공

- [일반클래스] 상속
  - 부모 클래스도 객체로 만들 수 있어야 하는 경우

- [추상클래스] 상속
  - 연관된 클래스의 공통점으로 묶고, 부모 클래스는 객체로 만들 수 없는 경우
  - +일부 미완성 클래스(abstract메소드 0개 이상 포함)  
    ex) Animal 클래스   
    -> 동물 객체는 어떤 동물인가?? eat(), breath()는 어떻게 수행되는가??  
    -> 알 수 없다. 하지만, 동물의 공통된 기능명은 알고 있음

- [인터페이스] 상속 : 접점
  - 연관성이 낮거나 없는 클래스에게 공통된 기능을 제공할 때 사용  
    ex) 키보드, 마우스, 스캐너, 카메라, 기울기 센서 (공통점 : 입력장치)  
    우연히도 입력이라는 기능을 가지고 있음  
    -> 각각의 용도는 다르지만 입력이라는 공통된 기능명이 있음, 입력이라는 접점   
  - +모든 필드가 묵시적(암묵적) public static final  
    ex) public static final double PI = 3.141592; -> duoble PI = 3.141592;
  - +모든 메서드가 묵시적으로 public abstract(추상메서드)  
    => 같은 이름을 제공할뿐이지, 상세한 기능제공은 하지 않는다.  
    ex) (public abstract) void input()  
    // input이라는 이름을 자식에게 제공할 뿐, 상세한 기능은 자식이 알아서 오버라이딩 해라. 추상메서드 이니깐 오버라이딩의 강제화
