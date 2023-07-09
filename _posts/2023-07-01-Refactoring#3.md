---
layout: post
title: 캡슐화
subtitle: 마틴 파울러의 리팩토링 2판 정리
categories: Refactoring
tags: [Refactoring]
---

시스템에서 각 모듈이 자신을 제외한 다른 부분에 드러내지 않아야 할 비밀을 얼마나 잘 숨기느냐는 모듈을 분리하는 가장 중요한 기준이다. 캡슐화가 잘되어있다면 무언가를 변경해야 할 때 고려해야할 부분이 줄어들어 코드 변경이 쉬워진다.

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
    public String getWriterName()
    {
        return this.writerName;
    }
}
class BookData()
{
    private WriterData writerData;
    public String getWriterName()
    {
        return this.writerData.getWriterName();
    }
}
```  

## 2. 컬렉션 캡슐화하기

책에서는 컬렉션에 대해서는 어느 정도 강박증을 갖고 불필요한 복제본을 만드는 편이, 오류를 디버깅하는 것보다 낫다고 말한다. 컬렉션을 캡슐화하는 방법으로 게터로는 컬렉션의 복사본을 전달해서 값을 바꿀 수 없도록 하고 컬렉션에 원소를 추가 및 삭제하는 함수를 추가한다. 

## 3. 기본형을 객체로 바꾸기

단순한 데이터 출력 그 이상의 일을 하게 되면 클래스로 정의하는 것이 추후에 개발이 점차 진행되면서 기능이 추가될 때 용이하다.
```java
class BookData()
{
    private String writerName;
}
```  
아래처럼 Writer에 관한 객체를 하나 생성하면 추후에 관련 기능을 WriterData 안에 추가할 수 있다.
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
}
```  


## 5. 클래스 추출하기  
개발이 진행될수록 클래스가 비대해질 수 있다. 이 때 일부 데이터와 메서드를 따로 묶을 수 있다면 분리해야 한다.
## 6. 클래스 인라인하기  
클래스 추출하기의 반대되는 리팩토링 방법이다. 분리하고 나니 남은 역할이 없을 때, 자주 사용하는 클래스로 흡수시킨다.
## 7. 위임 숨기기  
클라이언트가 위임 객체의 존재를 알 수 없게 위임 메서드를 서버에 만들어서 위임 객체를 숨긴다.

```java
class BookData()
{
    public WriterData writerData;
}

BookData bookData = new BookData();
public String writerName = bookData.writerData.writerName;
```  
아래처럼 변경하면 클라이언트가 WriterData 객체에 대한 사실을 모르고 writerName 에 접근 가능하다.

```java
class BookData()
{
    private WriterData writerData;
    public String getWriterName()
    {
        return this.writerData.getWriterName();
    }
}

BookData bookData = new BookData();
public String writerName = bookData.getWritedName();
```  


## 8. 중개자 제거하기  

위임 숨기기와 반대되는 리팩토링 방법이다. 
## 9. 알고리즘 교체하기  
