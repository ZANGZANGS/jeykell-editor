---
layout: post
title: '커넥션 풀을 관리하는 방법, DBCP와 JNDI'
subtitle: 
banner:
date: 2021-03-12 10:18:00
categories: []
tags: [java, spring]
author: zangzangs

# sidebar: [] 사이드 바를 없애고 싶으면 주석 해제
---

다음 프로젝트 준비를 하다가 커넥션 풀을 DBCP에서 JNDI로 변경한다는 내용을 들었습니다. 커넥션 풀에 대해서는 알고 있었지만 DBCP와 JNDI에 대해서는 처음 듣는 단어라 생소하여 해당 내용을 찾아봤습니다.

<br>

## 커넥션 풀(connection pool)이란

JAVA에서는 JDBC를 사용해서 커넥션을 생성합니다. 그리고 커넥션 풀은 생성된 커넥션을 관리합니다.

>  **JDBC**( **Java Database Connectivity** )는 자바에서 데이터베이스에 접속할 수 있도록 하는 **자바 표준 API**이다.

데이터베이스와 연결된 Connection을 미리 만들어서 pool 속에 저장해 두고 있다가 필요할 때 Connection을 Pool에서 쓰고 다시 Pool에 반환하는 기법을 말합니다.

Pool 속에 미리 Connection이 생성되어 있기 때문에 Connection을 생성하는 시간이 소비되지 않습니다. Connection을 계속해서 재사용하기 때문에 많은 수의 Connection을 만들지는 않습니다. 
Connection Pool을 사용하면 Connection을 생성하고 닫는 시간이 소모되지 않기 때문에 그만큼 애플리케이션의 실행 속도가 빨라집니다.

한 번에 생성될 수 있는 Connection 수를 제어하기 때문에 동시 접속자 수가 몰려도 애플리케이션이 쉽게 다운되지 않습니다.

Connection Pool에서 생성된 Connection의 개수는 한정적입니다. 동시 접속자가 많아 남아 있는 Connection이 없으면 해당 클라이언트는 대기 상태로 전환이 되고, Connection이 반환되면 대기하고 있는 순서대로 Connection이 제공됩니다.
<br/>
<div style="text-align:center;">
    <img src="/assets/images/java/connection-pool.png" alt="커넥션 풀" style="zoom:50%;" />
</div>

---

<br>

커넥션을 관리하는 것은 애플리케이션을 운영하는데 매우 중요합니다. DBCP와 JNDI는 이러한 커넥션 풀을 효율적으로 관리하기 위한 방법입니다.

<br>

## DBCP(Database Connection Pool)

> 데이터베이스  Connection을 **애플리케이션**에서 제어하면서 하나의 커넥션 풀을 가진다.

데이터베이스 애플리케이션을 효율적으로 연결하는 커넥션 풀 라이브러리는 웹 애플리케이션에서는 필수 요소입니다. 웹 애플리케이션 서버로 상용 제품을 사용한다면 보통 제조사에서 제공하는 커넥션 풀 구현체를 사용합니다. 그 외에 오픈소스 라이브러리로 Apache의 Commons DBCP와 Tomcat-JDBC, BoneCP, HikariCP 등이 있습니다.

웹 애플리케이션의 요청은 대부분 DBMS를 사용되기 때문에 커넥션 풀 라이브러리의 설정은 전체 애플리케이션의 성능과 안정성에 영향을 미치는 **핵심** 입니다.

#### 특징
- 데이터베이스와 애플리케이션의 일부분에서 발생하는 문제가 전체로 전파되지 않는다.
- 일시적인 문제가 긴 시간 이어지지 않게 한다.
- 값을 적절하지 못하게 설정해서 커넥션 풀이 애플리케이션에서 병목 지점이 되기도 한다.

<br>

## JNDI(Java Naming and Directory Interface)

> 데이터베이스  Connection을 **WAS Server**에서 제어하면서 하나의 커넥션 풀을 가진다.

JNDI는 디렉터리 서비스에서 제공하는 데이터 및 객체를 발견(discover)하고 참고(lookup)하기 위한 자바 API입니다.

<br>

#### 특징

- 데이터베이스 설정 정보를 관리가 쉽다.
- 애플리케이션 레벨에서 데이터베이스 Connection에 필요한 설정 정보들을 설정해 놓는다. 
- 애플리케이션은 하나이더라도 여러 종류의 데이터베이스를 사용할 수 있다는 점에서 해당 애플리케이션 개발자가 아니라면 정보를 찾는데 꽤나 시간이 걸린다.
- JNDI를 사용하게 되면 DB Connection 정보를 일반 개발자에게 노출되지 않아 보안상의 이점이 있다.

<br>

## 결론
두가지 방식 모두 장단점이 있습니다. 꼭 어떤 방식이 좋고 나쁘다가 아니라 목적에 적합한 방법을 사용하면 됩니다.

<br>

---
### Refference
- [https://d2.naver.com/helloworld/1321](https://d2.naver.com/helloworld/1321)
- [https://eongeuni.tistory.com/43](https://eongeuni.tistory.com/43)
- [https://zunoxi.github.io/infra/2020/06/20/infra-db-jdbc/](https://zunoxi.github.io/infra/2020/06/20/infra-db-jdbc/)
