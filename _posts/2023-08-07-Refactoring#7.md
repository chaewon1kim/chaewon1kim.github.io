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
```java
applyBestSellerDiscount(bookData.getSales(),bookData.getPrice()...);
```  
```java
applyBestSellerDiscount(bookData);
``` 
## 5. 매개변수를 질의 함수로 바꾸기  
