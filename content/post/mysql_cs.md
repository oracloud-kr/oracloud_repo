+++
language = ""
categories = ["MySQL"]
thumbnailInPost = "https://oracloud-kr-teamrepo.github.io/2017/04/mysqlcs_01/mysqllogo.jpg"
tags = ["mysql", "paas", "database"]
title = "Oracle MySQL Cloud Service"
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2017/04/mysqlcs_01/mysqlcs.jpg"
author = "taewan.kim"
description = "Oracle MySQL Cloud Service를 소개합니다. Oracle MySQL Cloud Service는 오라클이 제공하는 Database PaaS입니다. "
date = "2017-05-10T15:48:24+09:00"
+++

## Oracle MySQL Cloud Service 개요

오라클이 오픈소스 형태로 개발하는 MySQL은 전 세계에서 가장 많이 사용되는 오픈소스 데이터베이스 입니다.
오라클 클라우드는 Oracle MySQL Cloud Service(이하 Oracle MySQL CS)를 2016년 11월에 출시하였습니다.
2017년 5월 현재 오라클 클라우드는 관계형 데이터베이스 서비스[^1]로 Oracle과 MySQL을 제공합니다.

Oracle MySQL CS는 Oracle Cloud(Compute CS와 Storage CS)에서 MySQL Enterprise Edition을 PaaS 형태로 제공하는 클라우드 서비스 입니다.

Oracle MySQL CS가 제공하는 엔터프라이즈 지원 기능은 다음과 같습니다.

- 데이터베이스 관리 체계(백업, 패치, 모니터링)
- MySQL 전문 관리 도구
  - MySQL Query Analyzer
  - MySQL Enterprise Monitor
- 셀프 프로비저닝
- 탄력적인 확장성(Elastic scalability)
- 다중계층 보안(Multi-layer security)

[^1]: 오라클은 PaaS 형태로 제공하는 데이터베이스를 Database as a Service[DBaaS]로 명명하고 있습니다.

## MySQL CS의 특징

Oracle MySQL CS는 라이센스, 보안, 성능, 안전성, 관리 효율성, 백업과 복구 측면에서 다음과 같은 특징을 제공합니다.

### MySQL 버전과 라이센스

Oracle MySQL CS의 차별점[^2]은 MySQL Enterprise Edition을 사용한다는 것입니다.
Enterprise Edition을 사용함으로써 성능, 보안 및 가용성 측면에서 강점을 갖습니다. 기존에 on-premise에서 사용하는 MySQL의 기능을 제약없이 클라우드에서 사용할 수 있습니다.

[^2]: 클라우드 업체는 대부분 MySQL 서비스를 제공합니다. [AWS RDS](https://aws.amazon.com/ko/rds/mysql/), [GCP Cloud SQL](https://cloud.google.com/sql/) 그리고 [Azure MySQL App Service](https://docs.microsoft.com/ko-kr/azure/store-php-create-mysql-database)와 같은 MySQL 서비스는 모두 MySQL Community Edition을 사용합니다. 오라클을 제외한 다른 클라우드에서는 MySQL Enterprise Edition에서 제공하는 고급 기능(성능, 보안, 가용성 관련)을 사용할 수 없습니다.

MySQL Enterprise Edition은 다음과 같은 기능을 포함합니다.

|구성 컴포넌트|설명|
|---|---|
| MySQL Enterprise Transparent Data Encryption(TDE) | 데이터베이스 데이터의 물리적 파일을 암호화하기 때문에, 데이터 파일을 복사하는 행위가 무의미하게 됩니다. 데이터는 쓰기 시점에 암호화되고 읽기 시점에 복호화됩니다. |
| MySQL Enterprise Backup | 온라인 hot backup을 제공하여 데이터 유실의 위험을 최소화합니다. <br /> 백업 데이터 압축 및 백업 데이터 검증 기능을 제공합니다. |
| MySQL Enterprise Scalability | MySQL Thread Pool은 효율적인 쓰레드 제어 모델을 제공합니다. 효과적으로 Scale-up/down의 토대가 됩니다.|
| MySQL Enterprise Authentication | 외부 인증 모듈과 통합 지원을 합니다. PAM과 Window Active Directory 같은 외부 보안 인프라스터럭처를 지원합니다. |
| MySQL Enterprise Encryption | 비대칭키 기반의 암호화와 키 생성을 담당합니다. |
| MySQL Enterprise Firewall | 사이버 보안 위협에 대응하는 컴포넌트입니다. SQL Injection에 실시간 대응이 가능합니다. 비인가 된 데이터베이스 작업에 대한 차단을 제공합니다. |
| MySQL Enterprise Audit | 정책 기반 감사 기능을 제공합니다. 사용자 단위의 로깅을 설정할 수 있습니다. |
| MySQL Enterprise Monitor |  MySQL Enterprise Monitor, MySQL Query Analyzer와 Database File IO 를 이용하여 전문적인 데이터베이스 관리가 가능합니다. |
| Oracle Enterprise Manager for MySQL | 오라클이 제공하는 모니터링 도구입니다. 성능, 가용성에 대한 실시간 모니터링 정보를 제공합니다. 또한, 구성 정보를 확인할 수 있습니다. |
| MySQL Router | MySQL을 위한 경량형 미들웨어로, 백엔드 MySQL 서버를 추상화할 수 있습니다. |
| MySQL Workbench | 데이터베이스 아키텍트, 개발자 그리고 DBA를 위한 통합 도구입니다. |

MySQL CS는 Enterprise Edition에 기반을 하므로 위의 모든 기능을 사용할 수 있습니다.

현재 Oracle MySQL CS가 지원하는 MySQL 버전은 5.7 버전입니다. 향후 MySQL 5.7과 MySQL 5.8을 함께 지원할 예정입니다.

### Oracle Cloud 최적화

Oracle MySQL CS 인스턴스에는 Oracle Compute CS(이하 Compute CS)와 Oracle Storage CS(이하 Storage CS)에 최적화된 MySQL 설정 파일(my.conf)과 운영체제 설정이 적용되어 있습니다.  Oracle MySQL CS 인스턴스의 IO Thread의 수, Redo log 사이즈와 버퍼 사이즈, Buffer pool 등의 설정은 Compute CS의 Shape[^3]을 고려하여 최적의 값이 설정됩니다.

[^3]: Shape은 Oracle Compute Cloud Service에서 인스턴스에 할당할 OCPU 수와 메모리 크기를 지정하는 자원 프로파일입니다. Shape은 인스턴스가 사용하는 디스크 드라이브 유형을 결정합니다. 범용(General Purpose) 또는 높은 메모리(High Memory) Shape을 선택하면 하드 디스크 드라이브가 사용됩니다. High I/O Shape을 선택하면 NVM Express SSD 디스크가 인스턴스에 연결됩니다.

### 사용자 친화적인 인터페이스 제공

오라클 클라우드 서비스 콘솔에서 몇 번의 클릭만으로 MySQL 인스턴스를 신속하게 프로비저닝 할 수 있습니다.
웹 콘솔과는 별개로 MySQL CS의 라이프사이클을 관리하는 [REST API](http://docs.oracle.com/cloud/latest/mysql-cloud/CSMCS/index.html)와 PSM(PaaS Service Management) CLI[^4]를 제공합니다.

- 그림 1. MySQL Cloud Service의 REST API 스펙 문서
![](https://oracloud-kr-teamrepo.github.io/2017/04/mysqlcs_01/restapi.jpg)

[^4]: PSM(PaaS Service Management) CLI는 Oracle PaaS 서비스가 제공하는 REST API를 파이썬으로 구현한 명령  도구(CLI, Command Line Interface)입니다. PSM CLI를 사용하기 위해서는 Python 3.3 이상 버전이 필요합니다. PSM CLI에 대한 상세 정보는 [PSM CLI 메뉴얼](http://docs.oracle.com/en/cloud/paas/java-cloud/pscli/)을 참조하시기 바랍니다.

### 데이터베이스 관리 효율성

원클릭으로 데이터베이스 패치 테스트, 패치 적용 및 패치 롤백을 수행할 수 있습니다.
MySQL CS는 MySQL Cloud 인스턴스를 관리 할 수 있는 간단하고 사용하기 쉬운 웹 기반 콘솔을 제공합니다.
전문 관리자는 REST API 및 명령 도구(CLI)를 사용하여 자동화 도구와 연계가 가능합니다.
사용자 취향에 맞는 웹 콘솔, REST API 혹은 CLI를 선택할 수 있습니다.

MySQL CS는 Enterprise Edition을 사용하고 기능적인 제약이 없으므로 MySQL Enterprise Monitor(<그림 2> 참조), Query Analyzer(<그림 3> 참조), Database File IO(<그림 4> 참조)) 및 MySQL Workbench와 같은 기존에 사용하던 익숙한 MySQL 도구를 사용하여 MySQL 인스턴스의 성능 및 가용성, 관리 효율성을 높일 수 있습니다.

-그림 02. MySQL Enterprise Monitor
![](https://oracloud-kr-teamrepo.github.io/2017/04/mysqlcs_01/dashboard.jpg)

-그림 03. MySQL Enterprise Monitor: Query Analyzer
![](https://oracloud-kr-teamrepo.github.io/2017/04/mysqlcs_01/query_analyzer.jpg)

-그림 04. MySQL Enterprise Monitor: Database File IO
![](https://oracloud-kr-teamrepo.github.io/2017/04/mysqlcs_01/fileio.jpg)


Oracle MySQL Cloud Service는 노동 집약적이고 시간 소모적 데이터베이스 관리 업무를 자동화함으로써 시스템 관리 부하를 최소화합니다. 프로비저닝부터 패치, 삭제, 모니터링까지 관리업무와 비용을 줄여줍니다.

### 백업 & 복구

Oracle MySQL CS는 Enterprise Edition에 포함된 MySQL Enterprise Backup을 이용하여 안정적인 백업 서비스를 제공합니다. 주기적인 핫 온라인 백업을 구성할 수 있으며, 필요한 시점에 언제든지 온라인 스냅샷을 만들 수 있습니다.
Oracle MySQL CS의 웹 콘솔에서는 스케줄 백업을 구성하는 간단한 UI를 제공합니다.
모든 백업은 데이터베이스 온라인 상태로 진행됩니다.
백업의 유형은 다음과 같습니다.

|실행 유형|백업 유형|실행 주기|
|스케줄 백업| full backup | 주 1회(Weekly)|
|스케줄 백업| incremental backup| 매일 1회 (Daily) |
|On-demand 백업| full backup | 사용자 요청 시 백업 수행 |

백업 데이터는 Oracle MySQL CS 인스턴스의 가상머신과 Object Storage에 동시에 저장됩니다.
MySQL CS의 가상머신에 저장되는 백업 데이터는 최대 7일간 유지되고 Storage CS에
저장되는 백업 데이터는 30일까지 유지됩니다. Storage CS의 저장 기간은 기본값이 30일이며
백업 설정 UI에 설정 기간을 변경할 수 있습니다. (<그림 5> 참조)

- 그림 5. 백업 스케줄 설정
![](https://oracloud-kr-teamrepo.github.io/2017/04/mysqlcs_01/backup_config.jpg)

MySQL CS 웹 콘솔에서 관리자는 언제든지 백업을 수행할 수 있습니다.
On-Demand로 백업을 요청할 경우 Full Backup이 수행됩니다.

MySQL Enterprise Backup은 압축과 데이터 검증 기능을 제공합니다. 백업 데이터는 압축되어 저장되고,
복사 시 데이터 검증이 수행됩니다. 데이터 검증 기능을 통해서 MySQL CS는 백업 데이터의 안전성을 검증합니다.

증강분 백업(Incremental Backup) 이후 데이터베이스 변경 정보는 binary log 파일[^5]에 저장됩니다.
Oracle MySQL CS 인스턴스는 Binary log가 기본 활성화되어 있으며, 유지 기간은 90일입니다.

[^5]: MySQL 쿼리를 수행하면서 쌓는 로그를 저장하는 파일입니다. 트랜잭션 시점 복구 및 데이터베이스 복제의 근간이 됩니다. 주기적으로 지워주지 않으면 파일의 용량이 엄청나게 늘어날 수 있습니다.
Oracle MySQL CS는 Binary log 파일을 90일간 보관하는 설정이 기본값입니다. 이 파일 저장 기간은 my.conf 파일과 ```Oracle Enterprise Manager for MySQL```에서 변경 가능합니다.

Oracle MySQL CS 서비스 콘솔에서 백업 데이터로부터 데이터베이스를 복구할 수 있습니다. 복구 유형은 두 가지 입니다.
백업 데이터를 지정하여 해당 백업 데이터까지 복구하는 방식(<그림 6>)과 시간을 지정하여 백업데이터와 binary log 파일로부터 복구하는 point-in-time 복구 방식(<그림 7>)을 제공합니다.

- 그림 6. 백업 스케줄 설정
![](https://oracloud-kr-teamrepo.github.io/2017/04/mysqlcs_01/recovery2.jpg)

- 그림 7. 백업 스케줄 설정
![](https://oracloud-kr-teamrepo.github.io/2017/04/mysqlcs_01/pointintime.jpg)

### 고가용성(High Availability)

Oracle MySQL CS는 가상머신에 SSH 접근할 수 있습니다.
DBA는 데이터베이스 가상머신에 SSH 접근하여, MySQL 복제 환경을 구성할 수 있습니다.
복제 대상으로는 Oracle MySQL CS의 다른 인스턴스 및 원격에 위치하는 on-premise MySQL 데이터베이스 모두 가능합니다.
MySQL Replication Monitor로 마스터와 슬레이브의 상태를 모니터링할 수 있습니다.

- 그림 8. Oracle MySQL CS 인스턴스의 데이터베이스 복제
![](https://oracloud-kr-teamrepo.github.io/2017/04/mysqlcs_01/replication.jpeg)


### 다계층 보안 기능

MySQL CS은 Oracle Compute Cloud Service(이하 Compute CS)의 네트워크 접근 제어 기능과 MySQL Enterprise Edition의 보안 기능을 결합하여 다계층 보안 기능을 제공합니다.

|구분|컴포넌트|설명|
|---|---|---|
|Compute CS|Network Access Control|인프라 계층의 네트워크 통제|
|Enterprise Edition|MySQL Enterprise Firewall|SQL Injection 공격 방어 <br/>침입 탐지|
|Enterprise Edition|MySQL Enterprise Authentication|비대칭 암호화, 데이터 검증, 디지털 시그니처|
|Enterprise Edition|MySQL Enterprise Encryption|외부 인증 모듈 제공: PAM, Window Active Directory|
|Enterprise Edition|Transparent Data Encryption |데이터 파일 암호화|
|Enterprise Edition|MySQL Enterprise Audit|사용자 단위로 정책에 기반을 두고 활동내용 감사, Audit log 관리|

Oracle MySQL CS를 생성하면 기본 5개의 Security Rule이 생성됩니다.
이 Security Rule은 <그림 9>와 같이 Oracle MySQL CS 서비스 콘솔에서 접근 가능합니다.
MySQL 관리자는 <그림 10>에서 Security Rule을 추가할 수 있습니다.  

- 그림 9. Oracle MySQL CS 인스턴스의 Security Rule 접근
![](https://oracloud-kr-teamrepo.github.io/2017/04/mysqlcs_01/access_rule01.jpg)

<그림 10>의 5개 Security Rule은 데이터베이스 접속(3306 포트), Enterprise Monitor 접속(18443 포트)과 SSH 접속(22번 포트) 관련 규칙입니다.

- 그림 10. Oracle MySQL CS 인스턴스의 기본 Security Rule
![](https://oracloud-kr-teamrepo.github.io/2017/04/mysqlcs_01/access_rule02.jpeg)


### 효과적인 패치 관리 환경

오라클은 주기적으로 hot fixes & maintennce release를 패치 형태로 제공합니다.
데이터베이스 패치를 적용은 관리자에게 부담스러운 작업입니다.
Oracle MySQL CS는 효율적인이고 안전한 패치 관리 체계를 제공합니다.
신규 패치가 등록되면 Oracle MySQL CS는 사용자에게 신규 패치 등록을 통지합니다.
사용자는 원하는 시점에 패치를 적용할 수 있습니다.
서비스 콘솔의 패치 UI는 <그림 10>과 같이 ```precheck```와 ```patch``` 메뉴를 제공합니다.
첫 번째 메뉴인 ```precheck```는 실제 적용 전에 패치 과정을 점검하고 summary 리포트를 제공합니다.
사전 점검을 완료하면 ```patch``` 메뉴를 이용하여 실제 패치를 수행할 수 있습니다.

- 그림 11. Oracle MySQL CS 서비스 콘솔의 패치 UI
![](https://oracloud-kr-teamrepo.github.io/2017/05/mysqlcs/patch.jpg)

메뉴를 통해서 패치를 수행하면 실제 패치를 적용하기 전에 바이너리 백업이 수행됩니다.
패치 작업이 완료된 이후에는 <그림 11>과 같이 언제든지 패치 롤백을 요청하여 패치 적용 이전으로 복귀할 수 있습니다.

- 그림 12. Oracle MySQL CS 서비스 콘솔의 패치 롤백 기능
![](https://oracloud-kr-teamrepo.github.io/2017/05/mysqlcs/rollback_patch.jpg)

### 탄력적 확장성(Elastic Scalability)

Oracle MySQL CS는 ```Scale up/down```을 지원합니다.
관리자는 MySQL CS 서비스 콘솔에서 Oracle MySQL CS 인스턴스의 Shape을 변경하여 인스턴스가 사용하는 CPU 수와 메모리 크기, 디스크 유형을 변경할 수 있습니다.
MySQL 인스턴스의 Shape 변경은 REST API와 PSM(PaaS Service Manager) CLI를 이용해서도 가능합니다.
REST API와 PSM CLI를 이용하여 Oracle MySQL CS 인스턴스의 Shape이 주기적으로 변경하도록 자동화할 수 있습니다.
탄력적인 확장성을 지원하기 위한 ```Scale up/down```의 소요 시간은 약 5~10분 정도이며, 이 기간에는 MySQL CS 인스턴스 이용이 제한됩니다.

### MySQL 서포트

MySQL CS의 비용에는 오라클의 MySQL 엔터프라이즈 기술 지원이 포함되어 있습니다.
MySQL 기술 지원과 클라우드 인프라스터럭처 기술 이슈에 대하여 원 스톱으로 오라클 프리미엄 기술 지원을 받을 수 있습니다.
오라클은 글로벌 최대의 MySQL 엔지니어와 개발 그룹을 보유하고 있으며,
이 전문 인력이 직접 기술 지원에 참여하고 있습니다.
주기적으로 hot fixes & maintenance release를 제공하며, 29개국 언어로 24X7X365 기술 지원을 제공합니다.

### SSH 접근 지원

사용자는 MySQL CS 인스턴스에 SSH 접속을 이용하여 운영체제에 직접 접근할 수 있습니다.[^6]
DBA가 MySQL CS 가상머신에 접근하여 고급 튜닝 및 관리 작업을 수행하고 필요한 소프트웨어를 설치할 수 있습니다.
MySQL CS가 제공하는 패치 및 업그레이드 기능 수행 중 사용자가 설치한 소프트웨어가 영향을 미치면 ```해당 소프트웨어의 수정/설치제거```를 묻는 메시지가 표시됩니다.

[^6]: 일반적으로 클라우드 PaaS 서비스, 특히 Database 관련 서비스는 운영체제의 직접적인 접근을 제한합니다. 그러나 오라클 클라우드는 Database CS에 대한 운영체제 레벨의 접근을 허용하고 있습니다. 경험의 많은 DBA의 경우 데이터베이스 엔진 레벨의 파라미터 조정을 통해서 튜닝작업을 수행해야 합니다. 오라클은 오랜 기간 데이터베이스 개발하고 지원하는 과정에서 DBA의 운영체제 접근은 클라우드 PaaS에서도 필요한 부분으로 분류하고 있습니다.

MySQL Workbench를 사용하기 위해서 MySQL이 설치된 서버의 3306포트를 오픈해야 합니다. Oracle MySQL CS는 가상머신에 SSH 접근을 허용하기 때문에 MySQL Workbench가 ssh 터널링으로 MySQL에 접근하는 것이 가능합니다. 결과적으로 MySQL Workbench 사용을 위한 불필요한 포트 오픈을 최소화할 수 있습니다.

## MySQL CS 배포
MySQL CS를 인스턴스 절차에 대해서는 [MySQL Cloud를 시작하고 샘플 스키마 생성하기](/post/mysqlcs/) 문서에서 다루겠습니다.

## 요약

Oracle MySQL Cloud Service는 클라우드 업체 중에서 유일하게 MySQL Enterprise Edition을 사용하는 MySQL 클라우드 서비스입니다.
MySQL Enterprise Edition을 사용하기에 기능의 제약이 없고, 성능, 보안 및 가용성 측면에서 엔터프라이즈의 요구사항을 만족하는 서비스를 제공합니다.
백업, 복구, 패치 및 모니터링에 대한 편리한 웹 기반 UI와 REST API 및 CLI 인터페이스를 제공하여 관리자의 편의성을 높이고 있습니다.
Oracle MySQL Cloud Service는 Oracle Cloud(Storage CS와 Compute CS)에 최적화 구성이 적용되어 있습니다.

Oracle MySQL Cloud Service는 가상머신에 대한 SSH 접속을 허용합니다. MySQL DBA는 가상머신에 접근하여 고급 튜닝 및 관리 작업을 수행할 수 있습니다. 마지막으로 Oracle MySQL Cloud Serivce의 비용에는 오라클 프리미엄 서포트 비용이 포함되어 있습니다. Oracle Compute CS와 MySQL에 관한 24X7 기술 지원을 받을 수 있습니다.

## 참고자료
- [Oracle MySQL 클라우드 서비스 홈페이지](https://cloud.oracle.com/ko_KR/mysql)
- [Oracle MySQL 클라우드 서비스 문서 홈페이지](http://docs.oracle.com/cloud/latest/mysql-cloud/index.html)
- [Oracle MySQL 클라우드 서비스의 REST API](http://docs.oracle.com/cloud/latest/mysql-cloud/CSMCS/index.html)
- [Introducing the MySQL Cloud Service](http://ronaldbradford.com/blog/introducing-the-mysql-cloud-service-2016-09-23/)
- [www.datavail.com: Oracle MySQL Cloud Service](https://www.datavail.com/blog/oracle-mysql-cloud-service/)
- [toadworld: Oracle MySQL Cloud Service on Oracle Cloud Platform](https://www.toadworld.com/platforms/mysql/b/weblog/archive/2017/03/01/oracle-mysql-cloud-service-on-oracle-cloud-platform)
