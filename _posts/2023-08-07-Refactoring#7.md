---
layout: post
title: API 리팩토링
subtitle: 마틴 파울러의 리팩토링 2판 정리
categories: Refactoring
tags: [Refactoring]
---

## 1. 질의 함수와 변경 함수 분리하기  
질의 함수에는 모두 부수효과가 없어야 하며 부수효과가 있는 함수를 발견하면 상태를 변경하는 부분과 질의하는 부분을 분리하려고 해야한다.



## 2. 함수 매개변수화 하기  
두 함수의 로직이 비슷하고 단지 리터럴 값만 다르다면 함수를 하나로 합치고 값을 매개변수로 받도록 고쳐 중복을 없앨 수 있다.

```java
public void apply_10PercentDiscount(BookData book)
{
    return book.getPrice() * 0.9;
}
```  

```java
public void applyDiscount(BookData book, float discount_factor)
{
    return book.getPrice() * discount_factor;
}
```  
## 3. 플래그 인수 제거하기

호출하는 함수에 플래그 인수가 있으면 어떻게 호출해야하는지 이해하기 어려워진다.  
(플래그 인수는 호출하는 쪽에서 리터럴 값을 건네야 하고, 호출 되는 함수에서는 이 인자를 제어 흐름을 결정하는데 사용한다.)  
```java
public void applyDiscount(BookData book, boolean isBestSeller)
{
    if(isBestSeller)
        ...
    else 
        ....
}
```  

```java
public void applyBestSellerDiscount(Bookdata book)
{ 
    ....
}

public void applyRegularDiscount(Bookdata book)
{ 
    ....
}
```  
## 4. 객체 통째로 넘기기  
레코드를 통째로 넘기면 변화에 대응하기 쉽다.  
(추가로 데이터가 필요한 상황이 생겼을 때, 매개변수 추가가 아니라 피호출 함수 내부에서 수정이 가능하다.)
```java
applyBestSellerDiscount(bookData.getSales(),bookData.getPrice()...);
```  
```java
applyBestSellerDiscount(bookData);
``` 
## 5. 매개변수를 질의 함수로 바꾸기  
제거하려는 매개변수의 값을 다른 매개변수로부터 얻을 수 있다면 쉽게 제거하고 질의 함수로 변경할 수 있다.  
(*내가 자주하는 실수: 매개변수를 없애는 대신 가변 전역 변수를 이용하는 일을 하면 안된다.*)

```java
applyBestSellerDiscount(bookData.getSales(),bookData);
```  
```java
applyBestSellerDiscount(bookData);
``` 

## 6. 질의 함수를 매개변수로 바꾸기  
이 방법은 아래의 경우에 활용 가능하다. 
- 전역 변수를 참조하고 있거나,
- 제거하길 원하는 원소를 참조하고 있는 경우

5번 방안과 상충되는데 5번 방안에 너무 의존하면 객체들끼리, 함수들끼리 수많은 결합이 생기고 6번 방안에 의존하면 매개변수 목록이 길어질 수 있다. 프로그램을 잘 이해하게 되었을 때 적절히 사용하는 것이 중요하다.

## 7. 세터 제거하기  
객체 생성 후에 값이 변하지 않길 원하는 필드라면 세터를 제거한다.  
생성자 함수에서만 설정되고 변경될 가능성이 없어진다.  

## 8. 생성자를 팩터리 함수로 바꾸기  
생성자에는 일반 함수에는 없는 이상한 제약이 붙기도 하기 떄문에 이런 경우에는 팩터리 함수로 대체한다.  
(생성자에서는 반드시 생성자를 정의한 클래스를 반환해야한다. 서브클래스의 인스턴스를 반환할 수는 없다.)
## 9. 함수를 명령으로 바꾸기  

## 10. 명령을 함수로 바꾸기
## 11. 수정된 값 반환하기
## 12. 오류 코드를 예외로 바꾸기
## 13. 예외를 사전 확인으로 바꾸기  
