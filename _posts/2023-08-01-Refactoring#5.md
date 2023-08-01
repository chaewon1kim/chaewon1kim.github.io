---
layout: post
title: 데이터 조직화
subtitle: 마틴 파울러의 리팩토링 2판 정리
categories: Refactoring
tags: [Refactoring]
---

## 1. 변수 쪼개기 & 필드 이름 바꾸기

- 수집 변수 외에 값을 여러번 대입하는 변수가 있다면 분리해줘야 한다.
- 데이터 구조는 깔끔하게 유지해야 한다.

```java
int tempValue = bookData.getPrice() * bookData.getSales();

tempValue = bookData.getAvgSales() * YEAR;
```  
```java
int bookProfit = bookData.getPrice() * bookData.getSales();

int bookSalesPerYear = bookData.getAvgSales() * YEAR;
```  
위의 예제 tempValue 에는 두 가지 각각 다른 값이 담겼기 때문에 변수를 분리해줘야 한다.
또 tempValue 라는 변수는 변수의 용도를 정확히 표현하지 못하기 때문에 변수의 이름도 바꿔줘야 한다.

bookProfit 변수를 final 로 정의하고 다른 값이 들어가지 못하게 하는 방법도 있다.  


## 2. 파생 변수를 질의 함수로 바꾸기  

