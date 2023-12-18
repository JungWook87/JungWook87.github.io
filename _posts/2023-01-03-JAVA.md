---
title : 12. Network
date : 2023-01-03 +09:00
categories : [KH 학원 공부, JAVA]
tags : [
  JAVA,
]
---
<!-- ![](/assets/img/java/array2.png){:style="border:1px solid #eaeaea; border-radius: 7px; padding: 0px;" } -->

### 네트워크

1) 정의
- 여러 대의 컴퓨터를 통신 회선으로 연결한 것   
  <br>
  1_ 서버와 클라이언트
    - 네트워크로 연결된 컴퓨터간의 관계를 역할로 구분한 개념
    - 서버
      - 서비스를 제공하는 프로그램 또는 컴퓨터클라이언트의 연결을 수락하고 요청 내용을 처리 후 응답을 보내는 역할
    - 클라이언트
      - 서비스를 받는 프로그램 또는 컴퓨터네트워크 데이터를 필요로 하는 모든 어플리케이션이 해당됨

  2_ IP 주소
    - 네트워크 상에서 컴퓨터를 식별하는 번호로 네트워크 어댑터마다 할당되어 있다

  3_ 포트(port)
    - 같은 컴퓨터 내에서 프로그램을 식별하는 번호로 클라이언트는 서버 연결 요청 시 IP주소와 포트 번호를 알아야 한다

2) 소켓 프로그래밍
- 소켓을 이용한 통신 프로그래밍
  <br><br>
  1_ 소켓(Socket)
    - 프로세스 간의 통신에 사용되는 양쪽 끝 단
  
  2_ 프로토콜(Protocol)
    - 컴퓨터 간의 정보를 주고 받을 때의 통신방법에 대한 규약
    - 접속이나, 전달방식, 데이터의형식, 검증 방법 등을 맞추기 위한 약속

  3_ TCP
    - 데이터 전달의 신뢰성을 최대한 보장하기 위한 방식. 연결지향형 통신
    - 전세계적으로 많이 사용. 일일이 확인하고 전달하기 때문에 신뢰성이 높다.

  4_ UDP
    - 데이터의 빠른 전달을 보장하기 위한 방식. 비연결 지향형 통신
    - 스트리밍 서비스에 주로 사용.

3) TCP 소켓 프로그래밍
- 클라이언트와 서버간의 1:1 소켓 통신
- 서버가 먼저 실행 후 클라이언트의 요청을 기다림
- 서버용 프로그램과 클라이언트용 프로그램을 따로 구현
- 자바에서는 ServerSocket과 Socket 클래스 제공
![](/assets/img/java/Network.png)

```java
< 서버용 TCP 소켓 프로그래밍 순서 >
1. 서버의 포트번호 정함
2. 서버용 소켓 객체 생성
3. 클라이언트 쪽에서 접속 요청이 오길 기다림
4. 접속 요청이 오면 요청 수락 후 해당 클라이언트에 대한 소켓 객체 생성
5. 연결된 클라이언트와 입출력 스트림 생성
6. 보조 스트림을 통해 성능 개선
7. 스트림을 통해 읽고 쓰기
8. 통신 종료

//1 서버의 포트번호 정함
int port 8500; // port 번호는 0~65535 사이 지정

ServerSocket serverSocket = null;
Socket clientSocket = null;

InputStream is = null;
BufferedReader br = null;

OutputStream os = null;
PrintWriter pw = null;

try {
  // 2 서버용 소켓 객체 생성
  // 3 클라이언트 요청 기다림
  serverSocket = new ServerSocket(port);

  System.out.println("[Server]");
  System.out.println("클라이언트 요청을 기다림");

  // 4 접속 요청이 오면 요청 수락 후 클라이언트 소켓 객체 생성
  clientSocket = serverSocket.accept();

  String clientIP = clientSocket.getInetAddress().getHostAdress();

  System.out.println(clientIP + "가 연결을 요청함");

  // 5 연결된 클라이언트와 입출력 스트림 생성
  is = clientSocket.getInputStream();
  os = clientSocket.getOutputStream();

  // 6 보조 스트림을 통한 성능 개선
  br = new BufferedReader(new InputStreamReader(is));
  pw = new PrintWriter(os);

  // 7 스트림을 통해 읽고 쓰기
  pw.println("[서버 접속 성공]");
  pw.flush();

  // 클라이언트가 서버에게 보낸 메시지 받기
  String message = br.readLine();
  System.out.println(clientIP + "가 보낸 메시지 : " + message);

} catch ( IOException e ) {
  e.printStackTrace();
} finally {
  // 8 통신 종료
  tri {
    if(pw != null) pw.close();
    if(br != null) br.close();
      
    if(serverSocket != null) serverSocket.close();
    if(clientSocket != null) clientSocket.close();
  } catch ( IOException e ) {
    e.printStackTrace();
  }    
}
```
```java
< 클라이언트용 TCP 소켓 프로그래밍 순서 >
1. 서버의 IP주소와 서버가 정한 포트번호를 매개변수로 하여 클라이언트용 소켓 객체 생성
2. 서버와의 입출력 스트림 오픈
3. 보조 스트림을 통해 성능 개선
4. 스트림을 통해 읽고 쓰기
5. 통신 종료

// 1 서버의 IP주소와 서버가 정한 포트번호를 매개변수로
String serverIP = "127. 0. 0. 1"; // loop back ip (내 컴퓨터를 가리키는 ip 주소)
int port = 8500;

Socket clientSocket = null;
BufferedReader br = null;
PrintWriter pw = null;

try {
  // 클라이언트용 소켓 객체 생성
  System.out.println("[Client]");
  clientSocket = new Socket(serverIP, port);

  // 2 서버와 입출력 스트림 오픈
  // 3 보조스트림을 통해 성능 개선
  br = new BufferedReader(new InputStreamReader(clientSocket.getInputStream());
  pw = new PrintWriter(clientSocket.getOutputStream());

  // 4 스트림을 통해 읽고 쓰기
  String message = br.readLine();
  System.out.println("서버로부터 받은 메시지 : " + message);

  Scanner sc = new Scanner(System.in);
  System.out,print("입력 : ");
  String input = sc.nextLine();

  pw.println(input);
  // 작성한 메시지 밀어내기
  pw.flush

} catch ( IOException e ) {
  e.printStackTrace();
} finally {
  // 5 통신 종료
  try {
    if(pw != null) pw.close();
    if(br != null) br.close();
    if(clientSocket != null) clientSocket.close();
  } catch( IOException e) {
    e.printStackTrace();		
  }
}
```

4) UDP 소켓 프로그래밍
- UDP는 연결 지향적이지 않기 때문에 연결 요청을 받아줄 서버 소켓이 필요 없음
- java.net 패키지에서 제공하는 두 개의 DatagramSocket 간에 DatagramPacket으로 변환된 데이터를 주고 받음

![](/assets/img/java/Network2.png)

```java
< 서버용 UDP 소켓 프로그래밍 순서 >
1. 서버의 포트번호 정함
2. DatagramSocket 객체
3. 연결한 클라이언트 IP주소를 가진 InetAddress 객체 생성
4. 전송할 메시지를 byte[]로 바꿈
5. 전송할 메시지를 DatagramPacket 객체에 담음
6. 소켓 레퍼런스를 사용하여 메시지 전송
7. 소켓 닫음


< 클라이언트용 UDP 소켓 프로그래밍 순서 >
1. 서버가 보낸 메시지를 받을 byte[] 준비
2. DatagramSocket 객체 생성
3. 메시지 받을 DatagramPacket객체 준비
4. byte[]로 받은 메시지를 String으로 바꾸어 출력
5. 소켓 닫음
```
