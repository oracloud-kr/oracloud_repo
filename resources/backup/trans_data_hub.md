+++
date = "2017-12-26T21:28:14+09:00"
description = "blogs.oracle.com 블러그의 2017년 Kubernetes와 Serverless 마지막 포스팅 번역입니다. 오라클의 2018년 방향을 감지할 수 있습니다."
title = "[번역]관리형 아파치 카산드라 서비스 소개: Oracle Data Hub Cloud Service"
thumbnailInList = "https://taewanmerepo.github.io/2017/12/datahub/list.jpg"
thumbnailInPost = "https://taewanmerepo.github.io/2017/12/datahub/010.jpg"
tags = ["ORACLE", "Oracle Cloud", "NoSQL", "cassandra", "카산드라", "data hub", "오라클 클라우드"]
categories = ["Oracle-Cloud"]
author = "taewan.kim"
language = ""  
jupyter = "false"
+++

본문은 blogs.oracle.com에서 운영 중인 Oracle Developer 블로그의 11월 22일자 포스트를 번역한 문서입니다.
Oracle Data Hub Cloud Service는 MySQL과 Oracle Database를 제외한 다른 데이터베이스와 NoSQL(MongoDB, Cassandra, Redis)을 일괄 제공하는 것을 목표로 만들어진 클라우드 서비스입니다. 현재 첫 번째 데이터베이스로 카산드라를 제공합니다. 이와 관련한 오라클 공식 블로그의 포스트를 번역합니다. 원문 정보는 다음과 같습니다.

> - 출처: 오라클 공식 블로그, blogs.oracle.com: Oracle Developer Blog
- 원문: https://blogs.oracle.com/developers/introducing-data-hub-cloud-service-to-manage-apache-cassandra
- 제목: Introducing Data Hub Cloud Service to Manage Apache Cassandra and More
- 문서작성 일시: 2017.11.22
- 작성자: Developers Blog 팀

----

# 관리형 아파치 카산드라 서비스 소개: Oracle Data Hub Cloud Service

{{< img src="https://taewanmerepo.github.io/2017/12/datahub/020.jpg"
title="그림 1"
caption="Oracle Data Hub 클라우드 서비스 로고" >}}

오라클은 2017년 11월 22일 [Oracle Data Hub Cloud Service](https://cloud.oracle.com/datahub)를 GA로 발표하였습니다. 이제 Data Hub을 이용할 경우, 개발자는 필요한 시점에 언제든지 [아파치 카산드라](https://cassandra.apache.org/) 클러스터를 만들고 실행할 수 있습니다. 또한 개발자가 카산드라 클러스터의 백업, 패치 및 확장에 대한 관리 부담을 크게 가질 필요도 없습니다. __조만간 Oracle Data Hub는 MongoDB, Postgress 과 같은 데이터베이스를 서비스 지원 대상으로 추가할 예정입니다.[^1]__ 더 자세한 내용은 [OpenWorld 2017의 보도 자료](https://www.oracle.com/corporate/pressrelease/oow17-major-innovations-container-native-100217.html)에서 확인할 수 있습니다.

[^1]: [역자주]향후 다양한 데이터베이스와 NoSQL을 Data Hub 서비스에 추가하여 인스턴스 생성 시점에 선택할 수 있도록 지원할 예정입니다.

"__Data Hub Cloud Service__"는 다음과 같은 주요 이점을 제공합니다.

- 동적 확장성 – 사용자는 필요할 경우에 API 및 웹 콘솔 인터페이스를 사용하여 간단하게 스케일-업/스케일-다운, 스케일-아웃/스케일-인 그리고 클러스터 크기를 조절할 수 있습니다. 이러한 작업은 수 분 안에 완료됩니다.  

- 완전한 제어권(Full Control) – 개발팀이 온-프라미스 환경을 클라우드로 이전(Migration)할 때, 데이터베이스 클러스터가 설치되고 운영될 가상 머신(VM)에 완전한 SSH 접근 권한이 개발팀에 제공됩니다. 따라서 개발팀은 기존에 온-프라미스에서 데이터베이스를 관리해오던 방식 그대로 사용할 수 있습니다. 클러스터를 구성하는 서버에 로그인하고 관리 작업을 수행할 수 있습니다.

개발자는 자신의 애플리케이션에 관계형 데이터베이스보다 더 적합한 데이터베이스를 찾기도 합니다. MySQL과 오라클 데이터베이스는 오라클 클라우드에서 이미 상당 기간 운영돼왔습니다. 최근에 애플리케이션 개발자는 애플리케이션에서 사용하는 데이터 모델에 따라서 데이터베이스 기술을 선택하는 융통성을 갖추고 있습니다. 이러한 유스 케이스 별 접근 방식을 통해 개발자는 Oracle Database Cloud Service 혹은 MySQL, MongoDB, Redis, Apache Cassandra 등과 같은 다른 데이터베이스 기술을 상황에 맞게 선택할 수 있습니다. 이러한 선택의 다양성 이슈는 Oracle Data Hub Cloud Service로 해결할 수 있습니다.[^2]

[^2]:[역자주]Oracle Data Hub Cloud 서비스는 향후 다양한 데이터베이스와 NoSQL 및 Cache를 지원할 예정입니다. 현재는 Cassandra만을 지원합니다. 오라클 클라우드에서는 현재 Oracle Database, MySQL, Cassandra를 PaaS 형태로 제공합니다.   

## Data Hub Cloud Service 사용하기

Data Hub Cloud Service를 사용하여 Apache Cassandra 데이터베이스 클러스터를 생성, 관리 및 모니터링하는 것은 정말 간단하고 쉽습니다. 여러분들은 2개 단계의 간단한 절차로 원하는 규모의 아파치 카산드라 데이터베이스 클러스터를 생성할 수 있습니다.

- Step 1
  - 오라클 클라우드 리전 선택: __Oracle Cloud Infrastructure__, __Cloud Infrastructure Classic__[^3] 의 리전 선택
  - 아파치 카산드라 데이터베이스 버전 선택: 최신 버전(3.11), 안정 버전(3.10)
- Step 2
  - 클러스터 규모(노드 수), 컴퓨터 Shape(CPU, Memory 설정 타입) 그리고 스토리지 크기 선택. 여기에서 설정한 항목은 변경 가능합니다. 컴퓨터 파워 혹은 스토리지가 더 필요할 경우 동적인(카산드라 서비스 다운 없이) 재조정 가능합니다.
  - 데이터베이스 클러스터의 완전한 제어권을 확보하기 위한 SSH 접근 정보 입력

[^3]:[역자주]오라클 클라우드는 2개의 클라우드 인프라를 갖고 있습니다. "__Cloud Infrastructure Classic__"은 오라클 클라우드에서 가장 먼저 서비스해온 클라우드 인프라입니다. "__Oracle Cloud Infrastructure__"은 2017년에 공개한 새로운 버전의 클라우드 인프라입니다. 클라우드 인프라 선택에 따라서 네트워크 구성 및 보안 구성에 부분에 작업 방식에 차이가 발생할 수 있습니다.      

### 데이터베이스 버전 선택 유연성

클러스터를 생성할 때 Apache Cassandra 버전을 유연하게 선택할 수 있습니다. 또한, 선택한 카산드라 버전의 패치가 공개되면, 쉽게 패치를 적용할 수 있기 때문에, 데이터베이스의 최신 버전 유지가 쉽습니다. 패치 적용을 선택하면, 서비스는 다운 타임을 최소화하기 위해 롤링 방식으로 클러스터에 패치를 적용합니다.

{{< img src="https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/0eb8e772-8880-46b2-98ee-257cf9c2fa2c/File/755dd684529978d1bbb241fc794ccd46/755dd684529978d1bbb241fc794ccd46.png"
title="그림 2"
caption="클러스터 생성 중 클러스터 기본 정보 등록" >}}

### 동적 확장(Dynamic Scaling)


클러스터 프로비저닝을 하는 동안 클러스터 크기, 컴퓨터 노드의 Shape(CPU/Memory) 및 클러스터에 포함된 전체 노드의 스토리지 사이즈 등을 선택하는 유연성을 제공합니다. 이러한 유연성 덕분에 작업 부하 및 성능 요구 사항에 적합한 컴퓨팅 및 스토리지를 선택할 수 있습니다. 카산드라 클러스터에 "__노드를 추가__"(일반적으로 스케일 아웃이라고 함)해야 하거나, 스토리지를 추가해야할 경우, Data Hub Cloud Service API 혹은 웹 콘솔을 사용하여 쉽고 간단하게 클러스터 스케일을 관리할 수 있습니다. 따라서 클러스터를 프로비저닝 할 때 작업 부하의 크기를 최적화해야 한다는 부담이나 두려움을 가질 필요가 없습니다.


{{< img src="https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/0eb8e772-8880-46b2-98ee-257cf9c2fa2c/File/a1462f04f3a29f8b8e1cf6e621324f6f/a1462f04f3a29f8b8e1cf6e621324f6f.png"
title="그림 3"
caption="클러스터 생성 중 노드의 VM의 Shape 지정" >}}


### 완전 제어(Full Control)

오라클 클라우드 사용자는 클러스터의 모든 노드에 대한 완전한 SSH 접근 권한을 갖습니다. 따라서 데이터베이스와 해당 저장 영역을 완전하게 제어할 수 있습니다. 또한, 여러분들의 확장과 성능 요구 사항에 맞게 노드에 로그인하여 데이터베이스 인스턴스를 구성하는 완전한 유연성을 갖추고 있습니다.

{{< img src="https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/0eb8e772-8880-46b2-98ee-257cf9c2fa2c/File/ba07a072e27dbbc5103c17d55ada65a6/ba07a072e27dbbc5103c17d55ada65a6.png"
title="그림 4"
caption="클러스터 생성 중 보안 키 등록" >}}

"__Create__"를 선택하면 서비스는 컴퓨팅 인스턴스(노드, VM)를 생성하고 블록 볼륨을 노드에 연결합니다. 그리고 클러스터의 각 노드에 Apache Cassandra 바이너리를 배치합니다. Oracle Cloud Infrastructure Classic 플랫폼에서는 서비스가 자동으로 네트워크 액세스 규칙을 활성화합니다. 따라서 사용자는 CQL(Cassandra Query Language)을 사용하여 Cassandra 데이터베이스를 작성할 수 있습니다. Oracle Cloud Infrastructure 플랫폼에서 네트워크 액세스 규칙을 자동 활성화되지는 않습니다. 그러나 사용자는 가상 클라우드 네트워크(Virtual Cloud Network, VCN)의 특정 서브넷에 클러스터를 생성하는 모든 권한과 유연성을 가지며, 더 안전한 네트워크 및 보안 구성을 적용할 수 있습니다.

## 첫걸음...(Getting Started)

오라클 사용자는 "__Oracle My Services dashboard__"를 통해서 Data Hub Cloud Service에 접근하고 사용할 수 있습니다. Data Hub Cloud Service는 유니버셜 크래딧(Universal Credits)[^3]으로 요금이 부과됩니다. 아직 Oracle Cloud를 사용하고 있지 않은 상태라면, [무료 Cloud 크레딧(트라이얼 계정)](https://cloud.oracle.com/tryit)으로 서비스 살펴볼 수 있습니다. 오라클은 Data Hub Cloud Service에 대한 여려분의 의견 및 피드백 격하게 환영합니다.

[^3]:유니버셜 크래딧(Universal Credits)은 오라클의 클라우드 과금 정책입니다. 사용한 양만큼 과금, 종량제 등 다양한 라이센스 방식으로 구성됩니다. 트라이얼 계정을 사용하실 경우 사용한 만큼 시간 단위로 과금 됩니다.

## 추가 자료

- 서비스 개요: https://cloud.oracle.com/datahub
- FAQ: https://cloud.oracle.com/datahub/faq
- 서비스 관련 문서: https://docs.oracle.com/en/cloud/paas/data-hub-cloud/
- 시작 튜토리얼: https://docs.oracle.com/en/cloud/paas/data-hub-cloud/tutorials.html
