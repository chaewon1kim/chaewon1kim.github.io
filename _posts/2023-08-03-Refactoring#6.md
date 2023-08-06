---
layout: post
title: 조건부 로직 간소화
subtitle: 마틴 파울러의 리팩토링 2판 정리
categories: Refactoring
tags: [Refactoring]
---

## 1. 조건문 분해하기  

코드 덩어리를 보면 부위 별로 해체한 뒤에 해체된 코드 덩어리들을 각 덩어리의 의도를 살려 이름을 지어준다.  

```java
if(bookData.getSales > 10000 && bookData.rate >= 4)
{
    ....
}
else if(bookData.getSales > 500 && bookData.rate >= 4)
{
    ....
}
```  
```java
if(isBestSeller(bookData))
{
    ...
}
else if(isRisingSeller(bookData))
{
    ...
}
```  
## 2. 중복 조건식 통합하기  

비교하는 조건은 다르지만 그 결과로 수행하는 동작은 똑같은 코드들은 조건 검사도 하나로 통합하는 것이 낫다. 이렇게 하면 내가 하려고 하는 일이 더 명확해지고, 함수로 추출하기도 가능해진다.

```java
if(bookData.getSales <10000) return 0;
if(bookData.rate < 4) return 0;
....
```  
```java
public boolean IsBestSeller()
{
    if(bookData.getSales <10000 ||
        bookData.rate < 4 || 
        ....) return false;
    else return true; 
}
```  
## 3. 증첩 조건문을 보호 구문으로 바꾸기  
if-else 절보다 조건이 참이면 함수에서 빠져나오도록(보호구문) 한다. 반환점이 하나일 때 함수의 로직이 더 명확해진다면 이렇게 수정한다. 

```java
public void getBookType(BookData bookData)
{
    
    if(isAAA()){
        return AAA();
    }
    else{
        if(isBBB()){
            return BBB();
        }
        else{
            ....
        }
    }
}
```  

```java
public void getBookType(BookData bookData)
{
    
    if(isAAA()) return AAA();
    if(isBBB()) return BBB();
}
```  

## 4. 조건부 로직을 다형성으로 바꾸기  
switch-case 문의 경우에 클래스로 분리해서 구분한다.

## 5. 특이 케이스 추가하기  
특수한 경우의 공통 동작을 모아서 하나로 사용하는 특이 케이스 패턴을 적용한다. 
아래 예처럼 정의되지 않는 type 의 경우에 unknown class 로 새로 정의해서 사용한다.

```java
public void getBookType(BookData bookData)
{
    
    if(isAAA()) return AAA();
    if(isBBB()) return BBB();
    else return Unknown();
}
```  

### 6. 어서션 추가하기  
어셔션은 항상 참이라고 가정하는 조건부 문장으로, 어서션이 실패했다는 건 프로그래머가 잘못했다는 뜻이다.  


### 7. 제어 플래그를 탈출문으로 바꾸기  
제어 플래그는 어디선가 값을 계산하고 다른 어딘가의 조건문에서 검사하는 형태로 사용된다.
```java
for(Book b: BookList)
{
    if(!found)
    {
        ....
    }
}
```  
```java
for(Book b: BookList)
{
    if(b.isSoldOut)
    {
        ....
        break;
    }
}
```  
