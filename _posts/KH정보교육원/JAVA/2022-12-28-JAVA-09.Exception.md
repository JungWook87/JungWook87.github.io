---
title : 09. Exception Handling
date : 2022-12-28 +09:00
categories : [KH 학원 공부, JAVA]
tags : [
  JAVA,
]
---
<!-- ![](/assets/img/java/array2.png) -->

### 예외 처리

- 프로그램 오류
  - 프로그램 수행 시 치명적 상황이 발생하여 비정상 종료 상황이 발생한 것. 프로그램 에러라고도 함

- 오류의 종류
  - 컴파일 에러 : 프로그램의 실행을 막는 소스 코드상의 문법 에러. 소스 코드 수정으로 해결. 이클립스 프로그램 사용시 빨간 밑줄 생기는 것
  - 런타임 에러 : 프로그램 실행 중 발생하는 에러. 주로 if문 사용으로 에러 처리. 실행시 결과창에 예외 발생(ex. 배열의 인덱스 범위를 벗어났거나, 계산식의 오류)
  - 시스템 에러 : 컴퓨터 오작동으로 인한 에러, 소스 코드 수정으로 해결 불가. 연산지연, 버그 같은 것

- 오류 해결 방법
  - 소스 코드 수정으로 해결 가능한 에러를 예외(Exception)라고 하는데 이러한 예외 상황(예측 가능한 에러) 구문을 처리하는 방법인 예외 처리를 통해 해결

1) 예외 확인
- Java API Document에서 해당 클래스에 대한 생성자나 메소드를 검색하면 그 메소드가 어떤 Exception을 발생시킬 가능성이 있는지 확인 가능.> 발생하는 예외를 미리 확인하여 상황에 따른 예외 처리 코드를 작성할 수 있음

```java
java.io.BufferedReader의 readLine() 메소드

public String readLine() throws IOException
```

2) 예외 클래스 계층 구조
![](/assets/img/java/Exception Handling.png)
- Unchecked Exception : 확인을 굳이 안해도 되는 예외, 선택적으로 예외 처리
- Checked Exception : 확인을 꼭 해야만하는 예외, 예외처리 필수

3) Unchecked Exception
- RuntimeException 클래스
  - Unchecked Exception으로 주로 프로그래머의 부주의로 인한 오류인 경우가 많기 때문에 예외 처리보다는 코드를 수정해야 하는 경우가 많다 
- RuntimeException 후손 클래스
  - ArithmeticException : 0으로 나누는 경우 발생, if문으로 나누는 수가 0인지 검사
  ![](/assets/img/java/Exception Handling2.png)
  - ArrayIndexOutOfBoundsException : 배열의 index 범위를 넘어서 참조하는 경우 배열명.length를 사용하여 배열의 범위 확인
  ![](/assets/img/java/Exception Handling3.png)
  - NullPointerException : Null인 참조 변수로 객체 맴버 참조 시도 시 발생. 객체 사용 전에 참조 변수가 Null인지 확인
  ![](/assets/img/java/Exception Handling4.png)
  - ClassCastException : Cast 연산자 사용 시 타임 오류. instanceof 연산자로 객체타입 확인 후 cast연산
  - NegativeArraySizeException : 배열 크기를 음수로 지정한 경우 발생. 배열 크기를 0보다 크게 지정해야 함
  ![](/assets/img/java/Exception Handling5.png)
  - InputMismatchException : Scanner를 사용하여 데이터 입력 시 입력 받는 자료형이 불일치할 경우 발생
  ![](/assets/img/java/Exception Handling6.png)

4) 예외 처리 방법
- Exception이 발생한 곳에서 직접 처리

```java
< try ~ catch문을 이용한 예외 처리 >

- try : Exception 발생할 가능성이 있는 코드를 안에 기술
→ 수행 중 예외 발생시, 예외 객체가 던져짐(throw)

- catch : try 구문에서 Exception 발생 시 해당하는 Exception에 대한 처리 기술. 여러개의 
  Exception 처리가 가능하나 Exception간의 상속 관계를 고려해야 한다
→ try에서 던져진 예외를 잡아서 처리, 예외를 잡아 처리했기 때문에 프로그램이 종료되지 않음

- finally : Exception 발생 여부와 관계없이 꼭 처리해야 하는 로직 기술.
중간에 return문을 만나도 finally구문은 실행되지만 System.exit()를 만나면 무조건 프로그램 종료

public void method() {
  BufferedReader br = null;

  try {
    br = new BufferedReader(new InputStreamReader(System.in));

    System.out.print("입력 : ");

    String str = br.readLine();

    System.out.println("입력된 문자열 : " + str);
  } catch (IOException e) {
    e.printStackTrace();
  }
}

public void method() {
  BufferedReader br = null;

  try {
    br = new BufferedReader(new InputStreamReader(System.in));
    System.out.print("입력 : ");
    String str = br.readLine();
    System.out.println("입력된 문자열 : " + str);
  } catch (IOException e) {
    e.printStackTrace();
  } finally {
    try {
      System.out.println("BufferedReader 반환");
      br.close();
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
}
```

- Exception 처리를 호출한 메소드에게 위임
  - 메소드 선언시 throws Exception명을 추가하여 호출한 상위 메소드에게 처리 위임
  - 계속 위임하면 main() 메소드까지 위임하게 되고 main() 메소드에서도 처리되지 않는 경우 프로그램이 비정상 종료

```java
  public static void main(String[] args){
    ThrowsTest tt = new ThrowsTest();

    try{
      tt.methodA();
    } catch (IOException e) {
      e.printStackTrace();
    } finally {
      System.out.println("프로그램 종료");
    }    
  }
            ⬆️
  public void methodA() throws IOException{
    methodB();
  }
            ⬆️
  public void methodB() throws IOException{
    methodC();
  }
            ⬆️
  public void methodC() throws IOException{
    throw ne IOException();	// IOException 강제 발생
  }
```

5) Exception과 오버라이딩
- 오버라이딩 시 throws하는 Exception의 개수와 상관없이 처리 범위가 같거나 후손이어야 함
- Exception 클래스는 상속이 될수록 상위 클래스보다 예외의 내용이 더 상세하게 기술됨
- 오버라이딩 : 상속받은 메서드를 자식이 재정의
  - 메소드명, 매개변수, 반환형 동일
  - 접근제한자는 같거나 더 넓은 범위
  - 예외의 범위는 같거나 더 좁게
![](/assets/img/java/Exception Handling7.png)

6) 사용자 정의 예외
- Java API에서 제공하는 Exception Class 만으로는 처리할 수 없는 예외가 있을 경우 사용자의 필요에 의해 생성하는 Exception Class
- Exception이 발생하는 곳에서 throw new 예외클래스명()으로 발생

```java
public class UserException extends Exception{
  public UserException() {}
  public UserException(String msg) {
    super(msg);
  }
}

public class UserExceptionController {
  public void method() throws UserException {
    throw new UserException("사용자정의 예외발생");
  }
}

public class Run {
  public static void main(String[] args) {
    UserExceptionController uc = new UserExceptionController();
    try {
      uc.method();
    } catch(UserException e) {
      System.out.println(e.getMessage());
    }
  }
}
```

