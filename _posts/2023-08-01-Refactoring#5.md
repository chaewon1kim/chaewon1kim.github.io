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
가변 데이터는 한 쪽 코드에서 수정한 값이 연쇄효과를 일으켜 다른 쪽 코드에 원인을 찾기 어려운 문제를 야기하기도 한다.

## 3. 참조를 값으로 바꾸기
불변 데이터의 경우에는 값 객체로 만들어 활용하기 좋다.

```java
class BookData{
    BookPrice price;
    ...
    void applyDiscount (int percentage)
    {
        this.price = new BookPrice(this.price*percentage);
    }
}
```  
## 4. 값을 참조로 바꾸기  
특정 객체를 여러 객체에서 공유하고자 한다면, 그래서 값이 변경됐을때 이를 관련 객체에 모두 알려줘야 한다면 공유 객체를 참조로 다루어야 한다.   

## 5. 매직 리터럴 바꾸기
일반적으로 쓰이는 상수 값을 적절한 이름으로 바꿔준다.  

```java
public static final int PI = 3.14
```  
