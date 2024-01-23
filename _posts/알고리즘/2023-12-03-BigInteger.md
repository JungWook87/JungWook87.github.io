---
title : BigInteger
date : 2023-12-03 +09:00
categories : [알고리즘, 개념]
tags : [
  JAVA,
]
---
<!-- ![](/assets/img/Spring/aaaa.png){:style="border:1px solid #eaeaea; border-radius: 7px; padding: 0px;" } -->
<!-- ![](/assets/img/alg/1-1.png){:style="width:1000px" } -->

- 입력 값이 int형이나 long형의 범위를 넘어갈 때 사용
- int형이나 long형을 넘어가게 되면 0으로 출력

### BigInteger 사용법

```java
[ BigInteger 선언 ]

import java.math.BigInteger;

BigInteger number = new BigInteger("값");
// BigInteger를 선언할 때 보통 문자열을 인자 값으로 넘겨서 선언한다
```

```java
[ BigInteger 계산 ]

import java.math.BigInteger;

BigInteger num1 = new BigInteger("값1");
BigInteger num2 = new BigInteger("값2");

// 더하기
num1.add(num2);

// 빼기
num1.subtract(num2);

// 곱셈
num1.multiply(num2);

// 나누기
num1.divide(num2);

// 나머지
num1.remainder(num2);
```

```java
[ BigInteger의 형변환 ] 

int int_num = BigNum.intValue();
long long_num = BigNum.longValue();
float float_num = BigNum.floatValue();
double double_num = BigNum.doubleValue();
String String_num = BigNum.toString();
```
