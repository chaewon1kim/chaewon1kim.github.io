---
layout: post
title: 리팩토링 원칙과 Code Smells
subtitle: 마틴 파울러의 리팩토링 2판 정리
categories: Refactoring
tags: [Refactoring]
---

## **리팩토링이란?**  
소프트웨어 겉보기 동작은 유지한 채, 코드를 이해하고 수정하기 쉽도록 내부 구조를 변경하는 기법

여기서 겉보기 동작을 유지한다는 건 전과 후의 소프트웨어가 똑같이 동작해야한다는 뜻이다.
리팩토링을 통해서 성능은 개선될 수도 악화될 수도 있다. 일단은 리팩토링하고 후에 최적화 과정을 통해 성능을 개선하자.
리팩토링 하기 전에는 테스트 코드를 마련해놓고 시작하자.
좋은 코드는 수정하기 쉬운 코드이다.

## **리팩토링 하는 이유**
리팩토링하면 소프트웨어 설계가 좋아진다.
리팩토링하면 소프트웨어를 이해하기 쉬워진다.
리팩토링하면 버그를 쉽게 찾을 수 있다.
리팩토링하면 프로그래밍 속도를 높일 수 있다.

## **언제 리팩토링 해야하는가?**
돈 로버츠의 가이드:  

>"처음에는 그냥 한다.  
두번째도 그냥 한다.  
비슷한 일을 세번째 하게 되면 리팩토링한다."

삼진 리팩토링으로 기억하자.

## **코드 스멜 Code Smell**  

### 1. Mysterious name :  
함수나 변수 명은 단순하고명료하게 작성해야 한다. 이름만 보고도 무슨 일을 하고 어떻게 사용해야 하는지 명확히 알 수 있도록 해야한다.  

```java
int a;
int b;

for(int i=0;i<5;i++) 
  a+=1;
```
위와 같은 예시로 변수가 무슨 용도인지 알 수 없다면 이름을 바꿔야한다.

### 2.Duplicated Code :  
똑같은 코드 구조가 여러 곳에서 반복된다면 하나로 통합하여 중복을 제거 한다.  
### 3.Long Function :
함수는 짧고 간결하게 유지한다. 원래 코드보다 길어진다고 해도 함수의 목적(의도)가 더 잘 드러난다면 분리해야 한다.  

### 4.Long Parameter List :  
매개변수 목록이 길어질수록 함수는 이해하기 힘들다.  

### 5.Global Data :  
전역 데이터가 가변이라면 추적이 어렵다. 접근 범위를 최소로 하거나 캡슐화하여 사용한다.  

### 6.Mutable Data :  
데이터를 변경했더니 예상치 못한 결과나 골치 아픈 버그로 이어지는 경우 수정이 어렵다.  

### 7.Divergent Change :  
하나의 모듈이 서로 다른 이유들로 인해 여러 가지 방식으로 변경되는 일이 많을 때를 의미한다. SRP (Single Responsibility Principle) 이 지켜지지 않았을때 발생한다. (<-> Shotgun Surgery)  
### 8.Shotgun Surgery :  
코드를 변경할 때마다 여러 클래스를 수정해야 할 때 나는 냄새(<-> Divergent Change)  
### 9.Feature Envy :  
자기가 속한 모듈의 함수나 데이터보다 다른 모듈과 상호작용하는 일이 더 많을때 발생한다. 
기본이 되는 원칙은 '함께 변경할 대상을 한데 모으는 것'이다.  
(예외 : Strategy Pattern, Visitor Pattern 등 상황에 따라 판단한다.)  
### 10.Data Clumps :  
데이터가 항상 뭉쳐다닌다면 클래스로 묶어주자.  

### 11.Primitive Obsession  

### 12.Repeated Switches :  
중복 스위치문이 많이 나타날 때 수정이 힘들다. 스위치 문은 polymorphism 으로 개선 가능함  
### 13.Loops :  
반복문은 파이프라인으로 대체 가능하다.  

```java
public static void printGender()
{
  ArrayList<People> PeopleList = new ArrayList<People>();
  for(People p : PeopleList)
  {
    System.out.println ("GENDER : " + p.gender);
  }
}
```
ArrayList 구조체를 아래처럼 Stream 으로 변환하면 파이프라인 구조로 처리할 수 있다. 위처럼 간단한 예제는 상관없지만 Stream 객체의 map, filter 등을 활용하면 불필요한 loop문들을 제거할 수 있다.  

```java
public static void printGender()
{
  ArrayList<People> PeopleList = new ArrayList<People>();
  Stream<People> PeopleSteam = PeopleList.stream();
  PeopleSteam.forEach(p -> { 
    System.out.println("GENDER : " + p.gender);
  );
}

```

### 14.Lazy Element :  
어떤 이유로 의미 없는 함수로 남거나 메서드가 하나 뿐인 클래스 등이 되었을 때 발생한다.  
### 15.Speculative Generality :  
'나중에 필요할거야' 라는 생각으로 당장 필요 없는 로직 등으로 이해하거나 관리하기 어려워질 때 발생한다. 당장 걸리적거리는 코드는 바로 제거하자.  

### 16. Temporary Field :  
특정 상황에서만 값을 갖는 필드는 이해하기 어렵다. 이러한 임시 필드는 클래스로 추출하여 제거한다.  
### 17.Message Chains :  
꼬리에 꼬리를 물고 임시 변수 등이 줄줄이 나열되는 경우  
```java
managerName = aPerson.department.manager.name;
```
위처럼 다른 객체를 요청하는 작업이 연쇄적으로 이어지는 코드를 말한다. 이럴 경우, 클라이언트가 객체 구조에 종속되므로 객체 수정이 발생하면 클라이언트 코드도 수정해야 한다. 아래처럼 위임을 숨기는 방법을 사용할 수 있다.

```java
managerName = aPerson.managerNmae;
```
### 18.Middle Man :  
클래스가 간단한 위임을 너무 많이 하고 있을 때 나는 냄새. 클래스의 메소드 중 절반이 다른 클래스에 구현을 위임하고 있다면 중개자(Middle Man) 을 제거하고 직접 소통하도록 한다.  
### 19.Insider Trading  
### 20.Large Class :  
필드 수, 코드량이 너무 많아진 클래스로 중복을 제거하고 클래스를 추출해서 분리할 수 있다.  
### 21.Alternative Classes with Different Interfaces  
### 22.Data Class :  
데이터 클래스에 public 필드가 있다면 캡슐화하고 불변 데이터는 setter 를 제거한다.  

### 23.Refused Bequest :  
계층 구조를 잘못 설계해서 일부의 메서드와 데이터만 물려 받을 때, 서브 클래스가 부모의 동작은 필요로 하지만 인터페이스는 따르지 않을 때 발생한다.  
### 24.Comments :  
코드를 잘못 작성해서 주석이 장황하게 달렸을때 나는 냄새  
