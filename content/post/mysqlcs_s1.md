+++
language = ""
categories = ["MySQL"]
thumbnailInPost = "https://oracloud-kr-teamrepo.github.io/2017/04/mysqlcs_01/mysqlbanner.jpg"
tags = ["mysql", "paas", "database", "oracle cloud"]
title = "Oracle MyCloud CS 소개"
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2017/04/mysqlcs_01/mysqlcs.jpg"
author = "taewan.kim"
description = "Oracle MySQL Cloud Service를 소개합니다. Oracle MySQL Cloud Service는 오라클이 제공하는 Database PaaS입니다. "
date = "2017-04-26T15:48:24+09:00"
+++

MySQL은 전세계에서 오픈소스 데이터베이스 중에서 가장 높은 글로벌 점유율 갖는 데이터베이스 입니다.
Oracle은 오라클 클라우드에 PaaS형태로 MySQL을 서비스하는 Oracle MySQL Cloud Service(이하 MySQL CS)를 2016년 11월에 출시하였습니다.
MySQL CS는 검증된 MySQL Enterprise Edition을 이용하여 신속하고 안전하며 경제적인 엔터프라이즈급 MySQL 데이터베이스 서비스 제공을 목표로 합니다.

클라우드 환경에서 MySQL 데이터베이스를 사용하고자 한다면 Oracle MySQL CS는 가장 적합한 서비스입니다.

MySQL CS은 2017년 4월 현재 MySQL 엔터프라이즈 에디션 5.7을 사용하고 있습니다. 향후에는 MySQL 5.7과 MySQL 8.0 두 버전을 서비스로 제공할 예정입니다.

## MySQL CS의 특징
MySQL CS는 단순성, 자동화 그리고 엔터프라이즈 지원, 관리 효율성의 4가지 측면의 특성을 갖고 있습니다.

### 단순함

오라클 클라우드 서비스 콘솔에서 몇번의 클릭으로 MySQL 인스턴스를 신속하게 프로비저닝 할 수 있습니다.
관리 작업 자동화 툴(REST API, CLI)로 MySQL를 간단하게 관리할 수 있습니다.

### 자동화

데이터베이스 툴을 제공하며 백업과 같은 관리자 작업들의 자동화를 지원합니다.

### 엔터프라이즈 지원

성능, 보안 및 가용성을 고려하여 MySQL CS에는 MySQL Enterprise Edition 이 설치됩니다.
MySQL CS 서비스 비용은 MySQL 데이터베이스 및 MySQL 클라우드 서비스에 대한 지원을 모두 포함합니다.
특히 MySQL CS에서 MySQL은 Oracle Linux 가상머신에 설치됩니다.
MySQL과 Oracle Linux는 오라클이 개발하는 데이터베이스와 운영체제입니다.
MySQL CS에 오라클은 안정성과 성능을 극대화하는 Oracle Linux와 MySQL 구성을 적용하였습니다.

### 관리 효율성

사용자는 MySQL CS 인스턴스에 SSH 접속을 이용하여 운영체제에 직접 접근할 수 있습니다.[^1]
DBA가 MySQL CS 가상머신에 접근하여 고급 튜닝 및 관리 작업을 수행할 수 있습니다.
MySQL 가상 머신에 추가적인 소프트웨어 설치 가능합니다.
MySQL CS가 제공하는 패티 및 업그레이드 기능 수행 중 설치된 소프트웨어가 영향을 미칠 경우 타사 소프트웨어를 수정/설치 제거를 묻는 메시지가 표시됩니다.

[^1]: 일반적으로 클라우드 PaaS 서비스, 특히 Database 관련 서비스는 운영체제의 직접적인 접근을 제한합니다. 그러나 오라클 클라우드는 Database CS에 대한 운영체제 레벨의 접근을 허용하고 있습니다. 경험의 많은 DBA의 경우 데이터베이스 엔진 레벨의 파라미터 조정을 통해서 튜닝작업을 수행해야 합니다. 오라클은 오랜 기간 데이터베이스 개발하고 지원하는 과정에서 DBA의 운영체제 접근은 클라우드 PaaS에서도 필요한 부분으로 분류하고 있습니다.

## MySQL CS 개요

다음 <그림 1>은 Oracle MySQL CS의 전체적인 기능과 구성을 설명합니다.

그림 1. MySQL CS 개요
![](https://oracloud-kr-teamrepo.github.io/2017/04/mysqlcs_01/mysqlcs_overview.jpg)

## 참고자료
- [Introducing the MySQL Cloud Service](http://ronaldbradford.com/blog/introducing-the-mysql-cloud-service-2016-09-23/)
- [www.datavail.com: Oracle MySQL Cloud Service](https://www.datavail.com/blog/oracle-mysql-cloud-service/)
