+++
date = "2017-07-27T11:59:47+09:00"
description = "오라클 클라우드는 Hadoop PaaS 서비스로 Oracle Big Data Cloud Service Compute-Edition을 지난 2017년 03월에 출시하였습니다. Oracle Big Data Cloud Service Compute-Edition은 Hortonworks의 HDP를 기반으로 합니다. Oracle Big Data Cloud Service Compute-Edition 서비스에 대한 주요 특징을 소개합니다."
title = "Oracle Big Data Cloud Service Compute-Edition"
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2017/07/bdcsce_provisioning/oracloud-big-logo.png"
thumbnailInPost = "https://oracloud-kr-teamrepo.github.io/2017/07/bdcsce_provisioning/oracloud-big-logo.jpg"
tags = ['big data', 'hadoop', 'spark']
categories = ['big data']
author = "taewan.kim"
language = ""
+++

빅데이터 기술을 사용하면서 가장 어려운 부분은 오픈소스로 구성된 빅데이터 에코 기술을 설치하고 관리하는 부분입니다. 빅데이터 에코 기술들은 여러 오픈소스 프로젝트로 구성되어 있고, 각 오픈소스 프로젝트는 특정 오픈소스 프로젝트의 버전에 의존성을 갖고 있습니다. 또한, 각 오픈소스는 개별적인 업그레이드 주기를 갖습니다. 여러 오픈소스 프로젝트가 상호 버전 의존성을 갖고 있고, 개별적인 업그레이드 주기를 갖는 기술들로 빅데이터 환경을 구성하고 관리하기 위해서는 많은 노력이 필요합니다. 이 문제를 해결하기 위해서 Cloudera와 Hotonworks는 빅데이터 오픈소스 프로젝트들의 검증된 버전을 모아 패키지로 관리합니다. 이러한 이유로 업계에서는 Cloudera의 CDH(Cloudera Distribution Including Apache Hadoop)와 Hotonworks의 HDP(Hotonworks Data Platform)를 대부분 사용합니다. CDH와 HDP 같은 빅데이터 패키지로 오픈소스 프로젝트 간의 버전 충돌은 해결할 수는 있지만, 빅데이터 에코 기술의 구성을 최적화하고 안정성을 확보하는 관리 체계를 확보하기 위해서는 여전히 상당한 노력이 필요합니다.

앞에서 설명한 빅데이터 패키지는 클라우드 기술과 결합할 때, 기술적인 어려움를 크게 낮출 수 있습니다. 검증된 빅데이터 기술을 클라우드 인프라에 검증된 형태로 구성하고 소프트웨어 업그레이드 프로세스를 제공한다면 빅데이터의 기술적 난이도를 크게 낮출 수 있을 것입니다. 결과적으로 클라우드 빅데이터 서비스 사용자는 데이터 분석에 역량을 더 집중할 수 있을 것입니다. 오라클 클라우드는 PaaS 형태의 빅데이터 서비스로 __Oracle Big Data Cloud Service Compute-Edition__(이하 Oracle BDCSCE)을 지난 2017년 03월에 출시하였습니다. Oracle BDCSCE는 AWS EMR과 같은 하둡 서비스입니다. AWS EMR과 다른 점은 AWS EMR은 자체 HDFS를 제공하지 않고 데이터 저장소로 S3를 지원하는 반면, Oracle BDCSCE는 자체 HDFS를 제공합니다. 또한, Object Storage로 Oracle Cloud Storage Service를 사용할 수 있습니다. Oracle BDCSCE는 하둡 패키지로 Hortonworks의 HDP를 사용합니다.

앞에서 설명한 것처럼, Oracle BDCSCE는 Apache Spark/Apache Hadoop 클러스터를 필요한 시점에 즉시 생성할 수 있는 클라우드 서비스 입니다. Oracle BDCSCE는 Oracle Compute Cloud Service의 VM을 인프라스트럭처로 사용합니다.

Oracle BDCSCE은 다음과 같은 특징을 갖습니다.

1. 약 15분 안에 신규 Apache Hadoop/Spark 클러스터 생성
1. Scale-up/down 지원
1. 하나의 Data Lake에 저장된 데이터 분석 용도로 복수의 하둡 클러스터 구성 가능
1. Oracle Identity Cloud Service와 통합되어 계정 및 접근 관리 단일화
1. HDP(Hortonworks Data Platform)에 근간을 둔 검증된 소프트웨어 패키지 제공
1. 업그레이드 및 패치 자동화
1. 클러스터 관리 비용 최소화

Oracle BDCSCE의 최소 구성은 1개 노드[^1]입니다. 고가용성을 보장하기 위해서는 3개 노드 이상으로 클러스터를 구성해야 합니다. Oracle BDCSCE 클러스터에 HDFS 노드 외에 별도로 Compute 노드[^2]를 추가할 수 있습니다. 앞에서 설명한 Compute 노드와 Oracle Storage Cloud Service는 Oracle BDCSCE 클러스터와 독립적으로 운영됩니다.

[^1]: 본문에서 노드는 가상머신(VM, Virtual Machine)을 의미합니다. 1개 노드란 가상머신 서버 1대를 의미합니다.
[^2]: Compute 노드는 Spark 등 연산 프로세스가 동작하는 VM입니다. Comute 노드는 언제든지 Shape 변경이 가능합니다.

Oracle BDCSCE는 클러스터 생성 시점에 Full Profile과 Basic Profile로 인스턴스 구성을 선택할 수 있습니다. Full Profile은 Hortonworks Data Platform을 근간으로 전체 Hadoop Ecosystem을 설치합니다. Basic Profile을 선택하면 Spark 클러스터 환경만 설치됩니다. Oracle BDCSCE는 인스턴스 생성 시점에 Apache Spark 버전을 선택할 수 있습니다. 2017년 7월 현재 Oracle BDCSCE는 Spark 1.6과 2.1버전을 지원합니다.

Oracle BDCSCE가 사용하는 HDP는 오라클과 Hortonworks가 협력하여 오라클 클라우드에 최적화 구성이 적용된 버전입니다. 2017년 7월 현재 Oracle BDCSCE의 HDP 버전은 2.4.2입니다.

![](https://oracloud-kr-teamrepo.github.io/2017/07/bdcsce_intro/bdcsce.jpg)

## Oracle BDCSCE의 컴포넌트 버전

Oracle BDCSCE의 Full Profile에 설치되는 컴포넌트들과 각 버전은 다음과 같습니다.

|컴포넌트 명|버전|
|----|-----|
|HDFS| 2.7.1.2.4 |
|YARN + MapReduce2| 2.7.1.2.4|
|Tez| 0.7.0.2.4 |
|Hive| 1.2.1.2.4|
|Pig| 0.15.0.2.4|
|ZooKeeper| 3.4.6.2.4|
|Spark| 1.6,2.1|
|Zeppelin Notebook| 0.6.0|
|Alluxio| 1.2.0|
|BDCSCE Logstash Agent| 0.0.1|
|Nginx Reverse Proxy| 0.0.1|
|Spark Cloud Service UI| 0.5.0|
|Spocs Fabric Service| 0.1|

## Oracle BDCSCE의 차별성

Oracle BDCSCE는 다른 클라우드 빅데이터 서비스와 다음과 같은 차별성을 갖습니다.

### 차별성>1. Public & Private

Oracle BDCSCE는 Oracle Public Cloud와 Cloud@Customer에서 모두 제공하는 서비스입니다.
여기서 Oracle Cloud@Customer란 고객사의 데이터센터에 Oracle Public Cloud와 같은 클라우드 환경을 구성하고, 오라클이 관리하는 클라우드 서비스입니다. Oracle BDCSCE는 Public Cloud와 Private cloud(Cloud@Customer)에 같은 형태로 서비스됩니다. Oracle BDCSCE 사용자는 Public cloud와 Private Cloud에서 동일한 빅데이터 경험을 유지할 수 있습니다.

### 차별성>2. 스트리밍 데이터 처리 환경

오라클은 스트리밍 데이터를 수집하는 서비스로 Oracle Event Hub CS를 제공합니다. Oracle Event Hub CS의 실체는 Apache Kafka입니다. Oracle BDCSCE는 Oracle Event Hub CS에 빠른 접근이 가능한 인프라 환경(네트워크 구성)을 제공합니다. Oracle Event Hub CS와 Oracle BDCSCE를 사용하여 고성능 스트리밍 데이터 분석환경을 구성할 수 있습니다. Oracle Event Hub CS와 Oracle BDCSCE의 Spark Streaming을 사용하여 실시간 이벤트 처리 시스템으로 CEP(Context Event Processing)를 구성할 수 있습니다.

![](https://oracloud-kr-teamrepo.github.io/2017/07/bdcsce_intro/eventhubbdcsce.jpg)


### 차별성>3. 관계형 데이터베이스 및 클라우드 스토리지 통합

Oracle BDCSCE는 Oracle DBCS(Oracle Database Cloud Service), Oracle MySQL CS, OEHCS(Oracle EventHub Cloud Service) 및 Oracle Storage Cloud Service와 통합되어 있습니다.

![](https://oracloud-kr-teamrepo.github.io/2017/07/bdcsce_intro/dbcsintegration.jpg)

## Oracle BDCSCE 주요 특징

### 1. 탄력적인 서비스

Oracle BDCSCE는 3가지 측면(Cluster, Storage, Computer)의 확장성을 지원합니다.

- Cluster 확장성
  - 워크로드 크기에 따라서 Scale-out/Scale-in을 지원합니다.
  - HDFS 노드와 Compute 노드를 선택적으로 변경 가능합니다.
  - HDFS 노드 변경 시에 HDFS 데이터 재분배 옵션을 제공합니다.
- Storage 확장성
  - Oracle BDCSCE는 클러스터의 HDFS와 Oracle Storage Cloud Service에 분석 데이터를 관리합니다. Oracle Storage Cloud Service는 제약 없는 데이터 확장을 지원합니다. Metered 방식을 사용할 경우 사용한 데이터 용량과 Operation 수 만큼 요금이 부과됩니다.
- Compute 확장성
  - HDFS 노드와 Compute 노드에 대한 다양한 Shape을 지원합니다.
  - 최소 2 OCPU[^3], 최대 16 OCPU

[^3]: 하이퍼스레딩이 활성화된 Intel Xeon 프로세서의 물리적 코어 한 개 또는 Oracle SPARC 프로세서의 물리적 코어 한 개의 CPU 용량으로 정의됩니다. Intel Xeon 프로세서의 경우 각 OCPU는 vCPU 2개와 같습니다.

![](https://oracloud-kr-teamrepo.github.io/2017/07/bdcsce_intro/scaleout.jpg)


### 2. 오픈기술

Oracle BDCSCE는 Hortonworks의 HDP 2.4.2 버전을 근간으로 합니다. HDP를 오라클 클라우드 특성에 맞춰 최적화된 설정과 컴포넌트 구성으로 Oracle BDCSCE 서비스를 디자인하였습니다. Oracle BDCSCE를 사용함으로써 오픈 기술의 안전성을 확보하고, 오픈소스 설치 및 안정화에 들어가는 노력을 최소화할 수 있습니다.

### 3. 관리 서비스 (Managed Service)

Oracle BDCSCE는 클러스터 관리 부하를 줄일 수 있습니다. 하둡 기술을 사용할 때 가장 어려운 부분은 패치와 업그레이드입니다. Oracle BDCSCE는 검증된 방식의 패치와 업그레이드 기능를 지원합니다.

![](https://oracloud-kr-teamrepo.github.io/2017/07/bdcsce_intro/patch.jpg)

Oracle BDCSCE는 클러스터를 관리 및 모니터링 기능을 Web UI, Rest API와 CLI(Command Line Interface) 형태로 제공합니다. 사용자는 하둡 클러스터 관리를 자동화하거나 단순화 시킬 수 있습니다.

### 4. 보안

Oracle BDCSCE는 접근 제어, 데이터 보호, 네트워크 측면에서 대한 보안 안정성이 적용되어 있습니다.

- 접근 제어(Access Security)
  - Oracle BDCSCE는 Oracle Identity Cloud Service와 통합.
  - Oracle Identity Cloud Service에서 사용자 등록과 접근 제어를 관리 지원.
    - Oracle BDCSCE 사용자의 접근 및 활동에 대하여 강력한 감사(Audit)를 지원.
- 데이터 보안
  - 데이터 운용(data-in-motion) 시점과 데이터 저장(data-in-rest) 시점에 암호화
  - 모든 REST API는 HTTPS를 사용
- 네트워크 보안
  - SDN(Software-Defined Network)을 이용하여 세밀한 네트워크 보안 강화
  - VPN 지원
  - White-list IP[^4] 관리 지원

[^4]: Black-list IP의 반대 개념으로, 접근을 허용하는 특별한 IP를 지정하는 기능입니다.


![](https://oracloud-kr-teamrepo.github.io/2017/07/bdcsce_intro/network.jpg)

## 트라이얼 계정을 이용한 Oracle BDCSCE 테스트

오라클 클라우드가 제공하는 Trial Account로 Oracle BDCSCE를 테스트할 수 있습니다. 오라클 클라우드 Trial Account를 신청하면 1개월간 6 OCPU, 500GB를 제약 없이 사용할 수 있습니다. Trial Account로 Oracle BDCSCE 인스턴스를 생성하여 테스트할 수 있습니다.

오라클 클라우드 계성 생성에 대해서는 다음 Post를 첨고하시기 바랍니다.

- [오라클 클라우드 Trial Account 생성](../accont/)

## 요약

오라클 클라우드(OPC)는 Apache Hadoop 기술을 서비스하는 PaaS로 Oracle Big Data Cloud Service Compute-Edition을 출시하였습니다. Oracle BDCSCE는 Hortonworks의 HDP 2.4.2 버전을 오라클 클라우드에 최적화한 설정과 구성으로 디자인 되었습니다. 최소 구성은 1 노드이고 고가용성(HA, High Availability)를 확보하기 위해서는 클러스터를 3 노드 이상으로 구성해야 합니다.

Full Profile로 인스턴스를 생성할 경우에 HDFS, MapReduce, Hive, Pig, Tez, Spark 등을 사용할 수 있습니다. 관리 도구로 Ambari가 설치되고, 개발 및 운영 편리성을 위하여 Apache Zeppelin이 포함됩니다.

Oracle BDCSCE는 성능, 보안 및 관리 편리성이 고려된 PaaS 입니다. 데이터 확장, 실시간 데이터 처리, 컴퓨팅 노드 확장, 오픈소스 패치에 대한 고려사항이 적용되어 있습니다.

## 관련 문서

1. Oracle Big Data Cloud Service Compute-Edition 소개
1. [Oracle BDCSCE: 클러스터 생성](../stpe_by_step_new_cluster_bdcsce/)

## Reference

- [AMIS TECHNOLOGY BLOG - Apache Kafka on the Oracle Cloud](https://technology.amis.nl/2017/03/18/apache-kafka-on-the-oracle-cloud-my-first-experiences-with-oracle-event-hub-cloud-service/)
- [Official Tutorial- Using Oracle Big Data Cloud Service – Compute Edition](http://docs.oracle.com/en/cloud/paas/big-data-compute-cloud/csspc/index.html)
- [Toad World- Introduction to Oracle Big Data Cloud Service – Compute Edition](https://www.toadworld.com/platforms/oracle/b/weblog/archive/2017/06/14/introduction-to-oracle-big-data-cloud-service-compute-edition-part-ii-services)
