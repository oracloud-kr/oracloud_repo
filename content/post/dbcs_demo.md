+++
categories = []
title = "Oracle Database Cloud Service: 인스턴스 생성 및 접속 Demo"
language = ""
description = "Oracle Database Cloud Service(이하 Oracle DBCS)는 오라클 클라우드에서 제공하는 데이터베이스 PaaS입니다. Oracle DBCS 인스턴스를 생성하고 접속하는 과정을 설명한 데모입니다."
date = "2017-04-12T13:20:12+09:00"
author = "taewan.kim"
tags = ["oracle", "dbcs", "cloud"]
thumbnailInPost = "https://oracloud-kr-teamrepo.github.io/2017/03/dbcs_demo/dashboard.png"
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2017/03/dbcs_demo/oracle_cloud.png"

+++

## Oracle DBMS 소개

Oracle Database Cloud Service(이하 Oracle DBCS)는 오라클 클라우드에서 PaaS 형태로 제공하는 오라클 데이터베이스 서비스입니다.

Oracle Database Cloud Service에는 다음과 같이 4 가지 유형의 패키지로 라이센스가 구분됩니다.

- Standard Edition
- Enterprise Edition
  - TDE (transparent Data Encryption)
  - Data Guard
- Enterprise Edition High Performance
  - Multitenant
  - Partitioning
  - Advanced Compression
  - Advanced Security
  - Database Vault
- Enterprise Edition Extreme Performance
  - Oracle RAC 지원
  - In-memory
  - Active Data Guard

2017년 04월 현재 Oracle Database CS의 지원 버전은 다음과 같습니다.

- 11g R2
- 12C R1
- 12C R2

## Oracle DBMS 데모 동영상

아래 동영상은 Oracle DBCS 인스턴스를 생성하고 접속하는 과정을 설명하는 데모입니다.
데모 동영상 제작 시가는 2017.01.03입니다. 당시 Oracld DBCS 인스턴스 생성 소요시간은 25분 정도였고, 현재는 2017년 04월 기준으로 생성 시간이 15분으로 단축되었습니다.

동영상의 데모를 실습하기 위해서는 오라클 클라우드 계정이 필요합니다. 오라클 클라우드 계정이 없으시다면 다음 문서를 확인하시기 바랍니다.

- [Oracle Cloud Trial 계정 생성하기](http://www.oracloud.kr/post/accont/)

데모 동영상은 다음과 같은 내용으로 구성되어 있습니다.

- Oracle Database CS 인스턴스 생성
- Security Rule 수정, 1521번 포트 오픈
- SQLDeveloper
  - 접속 정보 등록
  - 오라클 계정 생성
  - 테이블 생성
  - 쿼리 수행

{{< youtube mMLMtBiZ4-Q >}}

***

## 참고

-  Oracle Learning Library: [Oracle Database Cloud Service Quick Start](https://apexapps.oracle.com/pls/apex/f?p=44785:112:0::::P112_CONTENT_ID:11569)
