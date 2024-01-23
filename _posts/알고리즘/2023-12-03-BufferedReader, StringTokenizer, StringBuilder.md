---
title : BufferedReader 와 StringTokenizer 그리고 StringBuilder
date : 2023-12-03 +09:00
categories : [알고리즘, 개념]
tags : [
  JAVA,
]
---
<!-- ![](/assets/img/Spring/aaaa.png){:style="border:1px solid #eaeaea; border-radius: 7px; padding: 0px;" } -->
<!-- ![](/assets/img/alg/1-1.png){:style="width:1000px" } -->

- BufferedReader
  
  ```java
  BufferedReader br =  new BufferedReader(new InputStreamReader(System.in));
  // BufferedReader는 입력값을 한 줄씩 읽어온다(개행을 기준)
  
  // 입력값을 불러오기 위 br.readLine()을 사용하게 되는데, br.readLine()의 코드를 쓰는 순간 무조건 다음 입력값으로 넘어간다.
  
  // 예시
  // 입력값이
  // "2"
  // "두 번째 입력"
  // "세 번째 입력"
  
  System.out.println(br.readLine()); -> 2
  // 여기서 첫 번째 입력값인 2를 사용한다.
  // 그러므로 이어서
  
  int a =  Integer.parseInt(br.readLine());
  // 를 하게되면 "두 번째 입력" 이 들어오게 되므로 에러가 발생한다
  ```
    
- StringTokenizer
  
  ```java
  StringTokenizer st = new StringTokenizer("입력값");
  
  // StringTokenizer(String str)
  // " "(공백) , \t(탭문자) , \n(개행문자)를 분리 문자로 사용하여 str을 분리한다.
  
  // StringTokenizer(String str , String delim)
  // 입력된 delim이 분리 문자로 사용하여 str을 분리한다.
  
  // StringTokenizer(String str , String delim , boolean returnDelims)
  // 입력된 delim이 분리 문자로 사용하여 str을 분리하고, 분리 문자열까지 분리해서 출력할지 여부를 설정한다. 기본은 false이기 때문에, 사용이 필요할 때 true를 붙여주면 된다.
  
  st.countTokens(); // 커서를 기준으로 남아있는 Token 갯수 반환
  st.hasMoreTokens(); // 반환할 수 있는 Token이 있는지 boolean 타입으로 반환
  st.nextToken(); // 현재 커서를 다음 구분자로 이동 후, 이전의 문자열을 반환
  ```
    
- StringBuilder
  
  ```java
  StringBuilder sb = new StringBuilder();
  
  // 데이터 추가
  sb.append("hello");
  sb.append("안녕");
  System.out.println("데이터 추가 : " + sb.toString());
  // => 데이터 추가 : hello안녕
  
  // 데이터 삽입
  sb.insert(5, "java"); // 5번째 자리에 데이터 삽입. index 0부터 시작
  System.out.println("데이터 삽입 : " + sb.toString());
  // => 데이터 삽입 : hellojava안녕
  
  // 데이터 삭제
  sb.delete(9, 11); // 9번부터 11번 전까지, 즉 9부터 10까지 삭제
  System.out.println("데이터 삭제 : " + sb.toString());
  // => 데이터 삭제 : hellojava
  
  // 데이터 교체
  sb.replace(5, 9, "자바");
  System.out.println("데이터 교체 : " + sb.toString());
  // => 데이터 교체 : hello자바
  
  // 데이터 역순(거꾸로 출력)
  sb.reverse();
  System.out.println("데이터 역순 : " + sb.toString());
  // => 데이터 역순 : 바자olleh
  
  // 초기화
  sb.setLength(0);
  // 또는 new StringBuilder();를 통해 새로운 객체를 생성
  // 또는 sb.delete() 메소드를 사용하는 방법
  // 하지만 sb.setLength(0); 방법이 가장 빠르다고 한다.
  ```
