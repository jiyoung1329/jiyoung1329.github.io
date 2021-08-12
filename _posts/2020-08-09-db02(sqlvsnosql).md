---
layout: post
title: "[DB] SQL vs NoSQL"
date: 2020-08-09 14:54:23 +0900
category: DataBase
---

이번에 Django-React로 간단한 토이 프로젝트를 진행하면서, DB 선택에 어려움을 겪었다. 아래는 DB 선정하는 과정에서 배운 개념을 정리하려고 한다.



# 1. SQL vs NoSQL 

첫 번째 고민은 MySQL과 같은 SQL을 쓸건지, firebase DB와 같은 NoSQL을 쓸건지 였다. 친구로부터 firebase를 사용하면 소셜 로그인 기능을 편하게 사용 가능하며 DB 관리도 할 수 있다는 얘기를 들었다. 

그런데 문제는 firebase의 DB 구조가 이전에 내가 써왔던 Django DB 구조와는 완전히 달랐던 것이다. Django에서는 SQL기반의 sqlite3 를 기본으로 제공하였지만 firebase는 NoSQL 기반이었다. 그래서 나는 여기서 SQL과 NoSQL의 차이점에 대해 궁금해 졌고 아래와 같이 정리해 보았다.



## (0) 정의

### 1. SQL(Structured Query Language)

[위키백과](https://ko.wikipedia.org/wiki/SQL)에서 찾아보면 SQL(Structured Query Language, 구조화 질의어)은 관계형 데이터베이스 관리시스템(RDBMS, Relational DataBase Management System)의 데이터를 관리하기 위해 설계된 특수 목적의 프로그래밍 언어 라고 되어있다.

즉 헷갈리면 안되는게 SQL은 DB, 그 중 RDBMS을 다루는 언어 중 하나인 것이지, DB 그자체가 아니라는 것이다. 

### 2. NoSQL(Not only SQL)

[위키백과](https://ko.wikipedia.org/wiki/NoSQL)를 찾아보면 NoSQL는 전통적인 RDBMS보다 덜 제한적인 일관성 모델을 이용하는 데이터의 저장 및 검색을 위한 매커니즘을 제공한다고 되어있다.

그렇기에 NoSQL은 단순 검색 및 추가 작업을 위한 매우 최적화된 key-value 저장공간으로 빅데이터와 실시간 웹 어플리케이션의 상업적 이용에 널리 쓰인다.



그럼 이제 아래에서 각 언어의 특징에 대해서 비교해보자.



## (1) 구조적 차이

일단 구조적 차이점을 바로 말해보자면 sql은 테이블 형식, nosql은 json 형태라는 것이다.

게시판을 예로 들어보면 sql에서는 아래와 같이 테이블을 작성할  수 있다.

![alt text](/public/img/gitblog/db_11.png)

하지만 nosql에서는 테이블 작성 없이 아래처럼 데이터를 구성할 수 있다.

```
{
	"id" : "1",
	"author" : "user1",
	"title" : "test",
	"content" : "hello. this is test article",
	"comment" : {
		"id" : "11",
		"user" : "testuser",
		"comment" : "hello. this is test comment"
	}
}
```

즉 따로 테이블관계의 정의를 따질 필요가 없다..!!

그렇다면 아래에서 좀 더 자세히 비교를 해보자.



## (2) Schema

### SQL - Strict Schema(엄격한 스키마)

SQL은 데이터들이 정의된 테이블에 레코드(record, 행)형식으로 저장되며 각 테이블마다 명확하게 정의된 구조가 있다. 

![alt text](/public/img/gitblog/db_14.png)

### NoSQL - No Schema

NoSQL이기 때문에 스키마가 따로 존재하지 않는다. Document 기반 DB를 예로 들면 Field, Document, Collections로 구성되어있는데 SQL와 비교하면 아래와 같다.

![alt text](/public/img/gitblog/db_13.png)



## (3) Relation(Join, 관계)

### SQL - Relation(관계)

SQL에서는 여러테이블에 데이터를 저장해 놓고 관계를 통해 데이터들을 파악할 수 있다. 이 관계를 통해서 데이터를 중복없이 저장할 수 있고, 데이터의 정확성을 높일 수 있다.

### NoSQL - No Relation

말그대로 NoSQL은 관계가 따로 존재하지 않는다. NoSQL에서는 JSON 형태로 데이터를 저장하며, 관계가 필요한 데이터는 아래와 같이 저장한다.

![alt text](/public/img/gitblog/db_12.png)

## (4) Scalability(확장성)

여기서 말하는 확장성은 DB서버의 확장성을 의미한다. DB를 저장하다보면 서버 용량과 성능에 한계가 오게 될 것이고, 이때 수직적 확장 또는 수평적 확장을 통해서 이러한 한계를 극복해야한다. 

### 수직적 확장(Vertical Scaling)

보통 SQL가 수직적 확장이 용이하다. 수직적 확장이란 단순히 CPU, RAM 업그레이드와 같이 DB 서버의 하드웨어 성능을 향상시키는 것이다. 그렇기 때문에 어플리케이션을 수정할 필요가 없다는 장점과 확장에 한계가 있다는 단점이 있다.

### 수평적 확장(Horizontal Scaling)

NoSQL은 수평적 확장에 용이하다. 기존의 하드웨어는 그대로 두고 장비를 추가하여 소프트웨어적으로 묶는 것을 의미한다. 즉 하드웨어 자체의 성능을 업그레이드 하지 않는다. 그렇기 때문에 하드웨어 종속성이 덜하며 이론상으로는 무한한 서버 확장이 가능하다. 

하지만 일반적으로 네트워크로 연결되기 때문에 네트워크 환경에 따라 확장을 해도 원하는 성능을 못얻을 수가 있다. 또한 어플리케이션의 수정이 필요한 경우가 많다.

### 

## (5) 결론

SQL과 NoSQL을 공부하면서 어떤 언어가 DB 설계에 적합한지에 대한 조건을 나름대로 정리해보았다. 비록 DB 설계에 있어 정답은 없지만 만약 나보고 어떤 언어로 DB를 설계할 것이냐고 하면 아래 세가지 조건에 따라 결정할 것 같다. 

1. 데이터 수정을 많이 하는가?

   SQL같은 경우에는 데이터가 중복되지 않기때문에 해당 데이터만 수정하면 되지만 NoSQL 같은 경우는 여러 컬렉션을 모두 수정해 주어야한다. (예를 들어 Comment를 수정하면 Comment와 Article 아래의 Comment 모두 수정해야함)

   

2. 데이터 구조 변경이 자주 이루어지는가?

   SQL에서는 스키마가 테이블로 명확하게 정의되기 때문에 필드 추가시 기존 데이터 수정이 어려울 수 있다. 하지만 NoSQL은 스키마가 따로 정의되지 않기 때문에 언제든지 저장된 데이터를 조정하고 새로운 필드 추가가 자유롭다.

   

3. 데이터의 확장을 어떻게 할것인가? 다루는 데이터의 양이 큰가?

   수직적 확장은 한계가 있기 때문에 많은 데이터를 다뤄야 한다면 수평적 확장이 용이한 NoSQL이 더 적합하다. 하지만 나처럼 작은 프로젝트를 수행하며 다루는 데이터의 양이 작다면 SQL도 적합하다고 생각한다.



