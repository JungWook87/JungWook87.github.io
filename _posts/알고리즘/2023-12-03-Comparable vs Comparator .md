---
title : Comparable vs Comparator 
date : 2023-12-03 +09:00
categories : [알고리즘, 백준]
tags : [
  백준,
]
---
<!-- ![](/assets/img/Spring/aaaa.png){:style="border:1px solid #eaeaea; border-radius: 7px; padding: 0px;" } -->
<!-- ![](/assets/img/alg/1-1.png){:style="width:1000px" } -->

### 공통점
- 인터페이스이다.
- 두 객체를 비교하기 위한 인터페이스이다

1) Comparable
    
  ```java
  public interfacle Comparable<T>{
    @Override
    public int compareTo(Type o){} // 비교값에 대해 설정
  }
  
  객체 a;
  객체 b;
  int res = a.compareTo(b); // a와 b를 비교
  ```
  
  - 자신과 매개 변수를 비교
  - 매개변수와 비교하여 본인 더 크면 양수, 같으면 0, 작으면 음수 형태로 반환하는게 일반적
    
2) Comparator 
  
  ```java
  public interfacle Comparator<T>{
    @Override
    public int compare(Type o1, Type o2){} // 비교값과 반환값 설정
  }
  
  객체 a;
  객체 b;
  객체 c;
  int res = a.compare(b, c); // a와 상관없이 b와 c를 비교
  
  => a의 필요성이 없는데 사용되고, 일관성이 떨어짐.
  => 익명 클래스를 사용한다
  ```
  
  - 매개 변수끼리 비교하여 반환
  - 두 매개 변수를 비교하여 결과 값 반환
  - 익명 클래스 : 선언과 동시에 @Override하여 사용.
    
    ```java
    일반적인 객체 생성
    public static void main(String[] args){
      Human h = new Human();
      Student std = new Student();
    
      System.out.println(h.sum());
      System.out.println(std.sum());
    }
    
    public class Human {
      int power = 50;
      int sensitivity = 50;
      
      @Override
      public int sum(){
        return power + sensitivity;
      }
    }
    
    public class Student extends Human{
      int sensitivePower = 100;
    
      @Override
      public int sum(){
        return power + sensitivity + sensitivePower;
      }
    }
    ```
    
    ```java
    익명 클래스 사용
    public static void main(String[] args){
      Human h = new Human();
    
      Human student = new Human(){
        int sensitivePower = 100;
        
        @Override
        public int sum(){
          return power + sensitivity + sensitivePower;
        }
      }
      
      System.out.println(h.sum());
      System.out.println(student.sum());
      
    }
    
    public class Human {
      int power = 50;
      int sensitivity = 50;
      
      @Override
      public int sum(){
        return power + sensitivity;
      }
    }
    ```
