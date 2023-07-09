---
layout: post
title: 캡슐화
subtitle: 마틴 파울러의 리팩토링 2판 정리
categories: Refactoring
tags: [Refactoring]
---

시스템에서 각 모듈이 자신을 제외한 다른 부분에 드러내지 않아야 할 비밀을 얼마나 잘 숨기느냐는 모듈을 분리하는 가장 중요한 기준이다.

## 1. 레코드 캡슐화하기  
가변 데이터를 저장하는 용도로는 레코드보다 객체화 하는 것이 좋다. 객체를 사용하면 어떻게 저장했는지를 숨기고 값을 각각의 메서드로 제공할 수 있다.
* 간단한 레코드를 캡슐화하기

```java
class BookData()
{
    private String bookTitle;
    String getBookTitle()
    {
        return bookTitle;
    }
}
```  

* 중첩된 레코드 캡슐화하기

_확인필요_
```java
class WriterData()
{
    private String writerName;
    String getWriterName()
    {
        return this.writerName;
    }
}
class BookData()
{
    private WriterData writerData;
    String getWriterName()
    {
        return this.writerData.writerName;
    }
}
```  

## 2. 컬렉션 캡슐화하기

책에서는 컬렉션에 대해서는 어느 정도 강박증을 갖고 불필요한 복제본을 만드는 편이, 오류를 디버깅하는 것보다 낫다고 말한다. 컬렉션을 캡슐화하는 방법으로 게터로는 컬렉션의 복사본을 전달해서 값을 바꿀 수 없도록 하고 컬렉션에 원소를 추가 및 삭제하는 함수를 추가한다. 

## 3. 기본형을 객체로 바꾸기


## 4. 임시 변수를 질의 함수로 바꾸기  