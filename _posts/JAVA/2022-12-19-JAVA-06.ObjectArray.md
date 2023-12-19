---
title : 06. ObjectArray
date : 2022-12-19 +09:00
categories : [KH 학원 공부, JAVA]
tags : [
  JAVA,
]
---
<!-- ![](/assets/img/java/array2.png) -->

### 객체배열

1) 정의
- 객체 참조형 변수를 저장하는 배열, 배열의 자료형을 클래스로 지정하여 활용한다
![](/assets/img/java/objectArray.png)

2) 선언 및 할당
```java
<선언>
클래스명[] 배열명;	// = 클래스명 배열명[];
ex) Academy[] arr; // = Academy arr[];

<할당>
배열명 = new 클래스명[배열크기];
ex) arr = new Academy[5];

<선언과 동시에 할당>
클래스명 배열명[] = new 클래스명[배열크기];
ex) Academy[] arr = new Academy[5];
```

3) 초기화
```java
<인덱스를 이용한 초기화>
배열명[i] = new 클래스명();
ex) arr[0] = new Academy(1, "KH정보교육원");
    arr[1] = new Academy(2, "케이에이치");

<선언과 동시에 초기화>
클래스명 배열명[] = {new 클래스명(), new 클래스명()};
ex) Academy arr[] = {
  new Academy(1, "KH정보교육원"), 
  new Academy(2, "케이에이치") };
```

4) 객체 배열의 메모리 구조
![](/assets/img/java/objectArray2.png)
