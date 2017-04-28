+++
author = "taewan.kim"
tags = ["cloud", "storage"]
date = "2017-04-27T09:25:33+09:00"
thumbnailInPost = "https://oracloud-kr-teamrepo.github.io/2017/04/storage_cs/post_logo.png"
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2017/04/object_storage_init/list.jpg"
title = "Oracle Storage Cloud Service"
description = "오라클 클라우드의 Oracle Storage Cloud Serivce를 소개합니다."
language = "bsh"
categories = ["Storage CS"]

+++

클라우드 서비스에서 기본은 Compute 서비스와 오브젝트 스토리지 서비스 입니다.
그 만큼 스토리지 서비스는 가장 많이 사용하면서 중요한 서비스입니다.
오라클 클라우드에서 오브젝트 스토리지에 해당하는 서비스는 Oracle Storage Cloud Service입니다.

Oracle Storage Cloud Service는 손 쉽게 데이터를 저장하고 사용할 수 있는 신뢰성과 안전성을 겸비한 스토리지 서비스 입니다.
높은 수준의 가용성을 제공하기 위해서 모든 데이터는 데이터 센터 내에서 복수의 복사본(Replica)으로 관리됩니다.
99.9%의 가용성과 Eleven-Nine의 Durability[^1]를 SLA로 보장합니다.
데이터를 안전성을 위하여 TLS/SSL 기반의 통신과 서버사이드 및 클라이언트 측의 데이터 암호화를 지원합니다.
데이터 지속성을 위한 자동 데이터 무결성 점검 및 자제 데이터 복구 기능을 제공합니다.[^2]
스토리지 클라우드에 접근 인터페이스는 다음과 같습니다.

- Swift 호환 REST API
- REST API의 Java 라이브러리
- Command Line Interface(CLI)
- OSCSA(Oracle Storage Cloud Software Appliance)
- 3rd Party 스토리지 솔루션

[^2]: Durability는 내구성으로 번역되기도 합니다. 데이터 관점에서 Durability는 저장이 완료된 데이터가 유실되지 않는 비율을 의미합니다. Eleven-Nine의 Durability는 데이터 유실 가능성이 매우 낮다는 지표입니다.

----

## 스토리지 클라우드 제공 서비스

오라클 클라우드가 제공하는 Stroage CS는 다음과 같이 구성됩니다.

- 오브젝트 스토리지
- 아카이브 스토리지
- Oracle Database Backup Service
- Oracle Storage Cloud Software Appliance
- Oracle Storage Cloud Bulk Data Transfer Service

### 오브젝트 스토리지

안전하고 유연하고 확장 가능한 클라우드의 오브젝트 스토리지입니다. 인터넷이 있는 어디에서나 손쉽게 접근 가능한 엔터프라이즈 데이터 보호 및 공유를 위해 설계되었습니다. 정형과 비정형 모든 데이터 세트에 적합한 스토리지입니다. 클라우드 가상머신을 만들때 블록 스토리지를 붙여 저장 공간을 확보합니다.

블록 스토리지에는 일반적인 하드디스크를 생각하시면 됩니다. 파일시스템으로 마운트되어 사용가능한 스토리지 입니다. 이 저장공간에는 소프트웨어 바이너리, 임시 저장 데이터 등이 저장되고, 영속성을 갖고 저장해야하는 데이터는 오브젝트 스토리지에 저장되어야 합니다[^3].

[^3]: 오브젝트 스토리지에는 영속성을 갖고 저장해야 하는 데이터를 저장합니다. 클라우드에서 가상머신은 언제든지 인스턴스가 사라질 수 있습니다. 가상머신이 소멸된 상태에서도 유지되어야 하는 데이터는 오브젝트 스토리지에 저장됩니다.

오브젝트 스토리지의 과금은 정액제인 Non-Metered방식과 사용량에 따른 Metered 방식으로 두 가지로 제공됩니다.
Non-Metered방식은 네트워크 비용 및 오퍼이션 비용을 모두 포괄하는 과금 방식으로 1TB당 월 사용료는 $30입니다.

Metered 방식은 데이터 사이즈, 네트워크 비용, 오퍼레이션 비용의 합으로 과금 됩니다.

|분류|항목|비용|설명|
|---|---|---|---|
|데이터|데이터 사이즈|$0.023 GB/Month<br/>$24TB/Month||
|네트워크|In-bound| 무료 |
|네트워크|Out-bound|$0.12 GB/Month| 오라클 클라우드 외부로 데이터 전송에만 적용<br/> 데이터센터 내부 네트워크 비용은 무료|
|오퍼레니션|Heavy Request|$0.005<br/>(1,000 Req/Month)| PUT, COPY, POST 타입 요청|
|오퍼레니션|Light Request|$0.004<br/>(10,000 Req/Month)| GET 타입 요청|
|오퍼레니션|Delete Request|무료| 오브젝트 삭제 오퍼레이션|

데이터 사이즈 비용은 시간당 데이터 사이즈 사용 금액의 합으로 계산됩니다.

데이터 사이즈 비용과 네트워크 비용은 사용량에 따라서 약간의 차이가 있습니다.
상세 과금 정보는 "[오라클 클라우드 - 스토리지 과금 페이지](https://cloud.oracle.com/en_US/storage/pricing)"를 참조하세요

### 아카이브 스토리지

아카이브 스토리지는 자주 접근하지는 않지만 장기간 보관해야 하는 데이터를 저장하기에 가장 적합한 저장소 입니다. 법정 기간동안 저장해야 하는 문서, 로그, 메일, 데이터 보관에 적합합니다. 자주 접근하지 않는 데이터를 보관하는 용도로 설계된 스토리지이며 엔터프라이즈급 보안, 복원성 및 확장성을 갖추었습니다. 연간 이용료는 테라바이트당 $12부터입니다.

아카이브 스토리지는 사용량에 따른 Metered 방식으로 과금됩니다. 1 테라바이스 데이터의 월간 이용료는 $1 입니다.
아카이브 스토리지 과금은 데이터 사용료, 오퍼레이션 비용 그리고 옵션 비용으로 구성됩니다.  
오퍼레이션 비용은 오브젝트 스토리지와 동일합니다.

|항목|비용|설명|
|---|---|---|
|Heavy Request|$0.0051<br/>(1,000 Req/Month)| PUT, COPY, POST 타입 요청|
|Light Request|$0.004<br/>(10,000 Req/Month)| GET 타입 요청|
|Delete Request|무료| 오브젝트 삭제 오퍼레이션|

옵션 비용은 다음과 같습니다.

|옵션|비용|설명|
|Archive Early Deletion|$0.003/GB|저장후 90일 이전 삭제 시 적용|
|Archive Data Retrieval|$0.005GB|저장 데이터를 읽기 위해서 로딩할 때 적용|
|Archive Small Reads and Writes|$0.05/1000requests|10MB 이하 파일에 적용|

### Oracle Database Backup Service

모든 Oracle Database(Oracle, MySQL) 백업용 스토리지입니다. Oracle Database 백업 데이터를 저장하고 이 데이터에 접근할 수 있는 안정적이고 확장 가능한 오브젝트 스토리지 솔루션입니다. RMAN과 직접 통합을 원하는 Oracle Database 고객의 고유한 요구에 맞게 설계된 데이터 백업 서비스입니다. 클라우드 PaaS 데이터베이스 뿐만 아니라 On-premise 오라클 데이터베이스의 백업 데이터 저장에도 이용 가능합니다.

### Oracle Storage Cloud Software Appliance

일반 파일 시스템과 오브젝트 스토리지는 파일을 다루는 방식에 차이가 있습니다.
따라서 파일 시스템의 데이터를 오브젝트 스토리지에 저장하기 위해서는 별도의 조치가 필요합니다.
Oracle Storage Cloud Software Appliance(이하 OSCSA)는 일반 파일 시스템의 데이터를 오브젝트 스토리지에 이전하는 가장 간단한 방법입니다.
OSCSA는 POSIX 호환의 로컬 NFS 인터페이스를 제공하여 서버의 데이터를 오브젝트 소트리지 혹은 아카이브 스토리지에 손쉽게 이관 가능합니다.
다시말해 OSCAS에 지정된 디렉터리에 파일을 저장하면, 그 데이터는 자동으로 오브젝트 소트리지 혹은 아카이브 스토리지에 전송되는 방식입니다.
OCCSA는 NAS 클라우드 게이트웨이 역할을 수행합니다. 지정한 디렉터리에 위치한 데이터를  데이터를 이관합니다. 암호로 데이터 보안, 체크섬 검증을 통한 데이터 무결성, 파일과 객체 간의 자동 변환, 데이터 캐싱을 통한 로컬 NAS에 가까운 성능 개선 기능을 제공합니다. 손쉬운 배포 위한 Docker 이미지 형태로 배포됩니다. Oracle Storage Cloud Software Appliance는 다음과 그림과 같이 동작합니다.

![](https://oracloud-kr-teamrepo.github.io/2017/04/storage_cs/oscsa.jpg)

1. 고객사 서버에 NFS 클라이언트 연결
1. 네트워크 연결
1. 소프트웨어 어플라이언트에 지정한 디렉터리에 파일 저장
1. 오라클 스토리지 클라우드로 비동기 저장

스토리지 클라우드를 위한 File 기반 NFS 를 통한 서비스 접근을 쉽게 할 뿐만 아니라,
캐쉬 구조에 기반한 고성능 및 고 가용성 구성을 지원합니다. NFS 지원 버전은 V4입니다.

### Public Cloud Data Transfer Service

대용량 데이터를 Storage Cloud로 적재 시 사용 가능한 서비스로 고객사의 데이터를 HDD 디스크 형태로 오라클 데이터 센터로 배송하는 서비스입니다. 1회 최대 48TB의 데이터를 Oracle Storage Cloud Service의 오브젝트 스토리지 또는 아카이브 스토리지로 데이터를 이관할 수 있습니다. 데이터 적재 대상은 Object Storage와 Archive Storage 모두 가능합니다.

## 오브젝트 & 아카이브의 데이터 과금 산정

- 20일간 10TB를 Object Storage에 보관

<pre class="prettyprint">
((10TB / 744 hours) * $23) * 24 hours * 20 days = $148
</pre>

- 20일간 10TB를 Archive Storage에 보관

<pre class="prettyprint">
((10TB / 744 hours) * $1.00) * 24 hours * 20 days = $6.45
</pre>


## 데이터 이동 방법

![](https://oracloud-kr-teamrepo.github.io/2017/04/storage_cs/storage_interface.jpg)

오브젝트 스토리지에 데이터를 이동하는 방법으로 위 그림과 같은 인터페이스가 제공됩니다.
오라클 오브젝트 스토리지는 모든 Swift 호환 REST API를 제공합니다. 이 REST API를 이용하여 데이터를 저장하고 관리하는 모든 오퍼레이션을 수행할 수 있습니다.
오라클은 REST API를 Java로 감싼 자바 라이브러리와 명령어 기반의 CLI(Command Line Interface) 제공합니다.
특정 디렉터리의 데이터를 스토리지 서비스에 동기화하는 Storage Cloud Software Appliance와 오라클 백업 데이터를 저장하는 RMAN 모듈를 이용할 수도 있습니다.
추가로 레거시와 메인프레임웍 데이터를 이관하는 여러 툴들을 제공합니다.

----
