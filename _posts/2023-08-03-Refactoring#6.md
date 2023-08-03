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

비교하는 조건은 다르지만 그 결과로 수행하는 동작은 똑같은 코드들은 조건 검사도 하나로 통합하는 것이 낫다.


## 3. 증첩 조건문을 보호 구문으로 바꾸기  
## 4. 조건부 로직을 다형성으로 바꾸기  
## 5. 특이 케이스 추가하기  
