+++
date = "2017-08-13T23:20:25+09:00"
description = "Oracle Public Cloud에서 Apache Kafka를 클라우드 관리 서비스(PaaS)로 제공합니다. 서비스 명은 Oracle Event Hub Cloud Service입니다. Oracle Event Hub Cloud Service를 소개하고, Kafka 서비스를 생성하는 방법을 소개합니다."
title = "Apache Kafka PaaS: Oracle Event Hub CS 소개"
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/logoinlist.jpg"
thumbnailInPost = "https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/logoinpost.jpg"
tags = ["Event Hub", "Kafka", "Big Data"]
categories = ["Big Data"]
author = "taewan.kim"
language = ""
+++

Oracle Public Cloud에서 Apache Kafka를 클라우드 관리 서비스(PaaS)로 제공합니다. Oracle Cloud는 2018년 3월에 Apache Kafka 서비스를 Oracle Event Hub Cloud Service(이하 Oracle Event Hub CS)라는 이름으로 출시하였습니다. 본 문서에서는 Oracle Event Hub Cloud Service를 소개하고, 이 서비스를 생성하고 클러스터를 테스트하는 방법을 소개합니다.

## Oracle Event Hub Cloud Service

Oracle Cloud는 Event Hub Cloud Service를 2017년 1월에 출시하였습니다. Event Hub CS는 Apache Kafaka의 클라우드 관리 서비스(managed service)입니다. Apache Kafka와 Zookeeper를 포함한 관련 소프트웨어의 설치, 관리, 업그레이드, 모니터링의 전체 라이프사이클을 관리합니다.

2017년 3월에 출시한 Event Hub CS의 Apache Kafka 버전은 0.9 버전이었습ㄴ다. 2017년 8월 현재 서비스되고 있는 Apache Kafka 버전은 0.10.2 입니다.

Oracle Event Hub CS를 이용하여 비동기 메시지 처리 환경을 효과적으로 구성할 수 있습니다. 특히 Oracle Big Data Cloud Service - Compute Edition과 함께 고성능 스트리밍 데이터 처리 환경 혹은 빅데이터 Ingestion 인프라를 구성할 수 있습니다. Oracle Event Hub CS는 다음과 같은 Oracle Cloud 서비스와 연동될 수 있습니다.

- Oracle Big Data Cloud Service - Compute Edition
- Oracle IoT Analytics Cloud Service
- Oracle Big Data Preparation Clood Serivce
- Oracle Mabile Analytics cloud Service

Event Hub CS는 Oracle Cloud의 Trial 계정으로 테스트 가능한 서비스입니다. 본 문서에서 Event Hub CS 인스턴스 생성에 데모에는 Oracle Cloud의 Trial 계정을 사용하였습니다.  

### Oracle Event Hub CS 특징
Oracle Event Hub CS는 다음과 같은 특징을 갖습니다.

- Oracle Event Hub CS 주요 특징
  - Apache Kafka의 관리형 서비스(Managed Service, PaaS)
  - Apache Kafka의 Native API 접근 허용
  - Apache Kafka의 REST API 접근 허용
  - Scale-out 클러스터 확장 기능 제공. <그림 1 참조>
  - 소프트웨어 패치 관리 지원 <그림 2 참조>
  - Oracle Event Hub CS의 VM에 접근 허용 (ssh)
  - Oracle 빅데이터 클라우드 서비스와 최적화 (Oracle Big Data CS - Comput Edtion)
  - Oracle Event Hub CS 클러스터 라이프사이클 관리 CLI 지원: PaaS Service Manager(PSM)

- <그림 1>. Oracle Event Hub CS 클러스터의 Scale-out
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub20.jpg)

- <그림 2>. Oracle Event Hub CS의 클러스터별 패치 관리
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub19.jpg)

### Oracle Event Hub CS 구성 컴포넌트

Oracle EHCS에는 다음과 같은 소프트웨어가 설치됩니다.

- Apache Kafka
- Apache ZooKeeper
- Confluent 3.0.1 (REST proxy, Avro registry)
- NGINX Proxy

### Oracle Event Hub CS 클러스터 규모

Oracle EHCS 클러스터의 최소 규모는 1개 노드 입니다. 1 ~ 3개 노드로 클러스터를 구성할 경우 Apache Kafka와 Apache ZooKeeper가 같은 노드에 설치됩니다. 이때 REST Proxy 서버 설치는 선택 사항입니다.

고가용성을 확보하기 위해서는 다음과 같은 구성을 권장합니다.

- Apache Kafka 브로커 노드: 5 VM
- Apache Zookeeper 노드: 3 VM
- REST Proxy 노드: 2 VM

Oracle Event Hub CS 클러스터는 Basic 설치 모드와 Recommended 설치 모드가 있습니다. Recommended 설치 모드는 Apache Kafka와 Apache Zookeeper를 분리하여 설치하는 고가용성 설치 방식입니다.

## Oracle Event Hub CS 데모

오라클 클라우드 Trial 계정을 이용하여 Oracle Event Hub CS 클러스터를 생성하는 절차를 소개합니다.


### Oracle Event Hub CS 클러스커 생성

오라클 클라우드 Trial 계정으로 Oracle Cloud에 로그인하면 <그림 3>과 같이 Oracle Cloud 대시보드가 출력됩니다. 왼쪽 위 메뉴 아이콘을 이용하여 각 클라우드 서비스 콘솔로 이동할 수 있습니다.

- <그림 3>. Oracle Cloud 대시보드
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub01.jpg)

<그림 4>에서 "Event Hub - Dedicated"를 선택하여 Event Hub 서비스 콘솔로 이동합니다.

- <그림 4>. Oracle Cloud 대시보드에서 Event Hub 서비스 콘솔 이동 메뉴
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub02.jpg)

Oracle Cloud 계정으로 처음 Event Hub 서비스 콘솔로 이동한 경우에는 <그림 5>와 같은 환영 페이지가 출력됩니다. 환영 페이지가 출력될 경우 "Go to console" 버튼을 클릭하여 <그림 6>과 같이 Event Hub 서비스 콘솔로 이동합니다.

- <그림 5>. Event Hub 클라우드 서비스 환영 페이지
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub03.jpg)

<그림 6>과 같이 "인스턴스 생성" 버튼을 클릭하여 Event Hub 클러스터 생성을 시작할 수 있습니다.

- <그림 6>. Event Hub 서비스 콘솔
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub04.jpg)

Oracle Event Hub CS의 클러스터 생성 과정은 3단계로 구성됩니다. <그림 7>은 클러스터 생성의 1단계입니다. 클러스터 생성 1단계에서는 클러스터 명 및 관리자 메일 등 기본 정보를 입력합니다. 입력이 완료되면 <그림 8>과 같이 2단계로 넘어갑니다.

- <그림 7>. Event Hub 클러스터 생성 1단계
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub05.jpg)

클러스터 2단계에서는 설치 방식, kafka 노드 수 등 클러스터 설치에 필요한 주요 정보를 입력합니다.

|구분|설정 항목|설명|설정값|
|---|---|---|---|
|기본정보|배치 모드|Basic과 Recommend 중 선택입니다. Recommend는 고가용성 모드입니다.|Basic 선택|
||SSH Public Key|SSH 공개키를 등록합니다.|SSH 공개키 등록|
|Kafka|노드 수|Kafka 브로커 수|3|
||컴퓨터 구성|Kafka 브로커 VM의 Shape|OC1m 선택|
||Usable Topic Storage|Topic 용 사이즈 지정, GB 단위|50|
|Enable REST Proxy|Enable REST Access|프락시 설치 여부 선택|체크|
||노드수|프락시 서버 노드 수|2|
||컴퓨터 구성|Proxy 서버 VM의 Shape|OC1m 선택|
||사용자 이름|Proxy 서버 사용자 ID|admin|
||Password|Proxy 서버 사용자 ID 인증 패스워드|Welcome1|
||Confirmed Password|Proxy 서버 사용자 ID 인증 패스워드 확인|Welcome1|

설정은 <그림 8>을 참조하시기 바랍니다. <그림 8>에서 Recommended 설치 모드를 선택할 경우 ZooKeeper 설치 항목이 추가됩니다.

- <그림 8> Event Hub 클러스터 생성 2단계: 세부 정보 입력
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub06.jpg)

<그림 8>에서 "다음" 메뉴를 선택하면 Event Hub 클러스터 생성 3단계 "클러스터 정보 확인"으로 넘어갑니다.

- <그림 9> 클러스터 생성 정보 확인
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub07.jpg)

<그림 9>에서 "생성" 버튼을 클릭하여 Event Hub 클러스터 생성을 시작합니다. 클러스터 생성 시간은 약 15분 정도 소요됩니다. <그림 10>은 클러스터 생성이 완료된 후 결과입니다.

- <그림 10> Event Hub 클러스터 생성 결과, 클러스터 목록 출력
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub08.jpg)

<그림 10>의 클러스터 명을 클릭하면 <그림 11>과 같이 클러스터 상세 정보 페이지로 이동합니다. 상단의 메뉴 아이콘을 클릭하여 "Access Rules"를 클릭합니다.

- <그림 11> Event Hub 클러스터 상세 페이지에서 "Access Rules" 이동
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub09.jpg)

"Access Rules" 관리페이지로 이동한 후, "Create Rule" 버튼을 클릭하여 클러스터에 접근하기 위한 Security Rule을 생성합니다.

- <그림 12> Event Hub 클러스터 Security Rule 생성
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub10.jpg)

Security Rule에서 <그림 13>과 같이 kafka 브로커 접근 규칙과 Zookeeper 접근 규칙을 정의 합니다.

|Security Rule 명|소스|목적지|포트|
|---|---|---|---|
|kafkaserver_publicaccess|PUBLIC-INTERNET|kafka_KAFKA_SERVER|6667|
|Zookeeper_publicaccess|PUBLIC-INTERNET|kafka_KAFKA_ZK_SERVER|2181|

외부 인터넷에서 Oracle Event Hub 클러스터에 접근을 허용하는 규칙을 생성하고 활성화합니다.

- <그림 13> Security Rule 생성
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub11.jpg)
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub12.jpg)

두 Security Rule이 생성되면 <그림 14>와 같이 Security rule에서 결과 확인 가능합니다.

- <그림 14> Security Rule 목록
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub13.jpg)

### Oracle Event Hub CS 토픽 생성

지금까지 Oracle Event Hub 클러스터를 생성하는 절차를 확인해 보았습니다. 이제는 데이터를 저장할 Topic을 만들 차례입니다. Oracle Event Hub 클러스터를 생성하면, Oracle Event Hub 서비스 콘솔의 이동 메뉴에 <그림 15>와 같이 "Oracle Event Hub Cloud Services - Topics" 메뉴가 추가된 것을 확인 할 수 있습니다. Oracle Event Hub 클러스터에서는 Kafka 토픽을 서비스 형태로 생성합니다.      

- <그림 15> Oracle Event Hub 서비스 콘솔의 메뉴
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub14.jpg)

<그림 15>에서 "Oracle Event Hub Cloud Services - Topics" 메뉴를 선택하여 Kafka 토픽 생성 페이지로 이동합니다. <그림 16>과 같이 토픽 생성 페이지에서 토픽 명, 파티션 수, 데이터 유지 기간을 입력하고 "다음" 버튼을 클릭합니다.

- <그림 16> Kafka 토픽 생성 페이지
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub15.jpg)

<그림 16>에서 "다음" 버튼을 클릭하면, <그림 17>과 같은 Kafka 토픽 생성 정보 확인 페이지로 이동합니다.
그리고 "생성" 버튼을 클릭하면 클러스터에 topic이 바로 생성됩니다.

- <그림 17> Kafka 토픽 생성 정보 확인 페이지
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub16.jpg)

topic 생성 시간은 5초 미만입니다. 토픽이 정상적으로 생성되면 <그림 18>과 같이 토픽 목록이 출력됩니다.

- <그림 18>. 토픽 목록 페이지
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub17.jpg)

### 테스트용 Apache Kafka 설치

Kafka 클러스터 접속 테스트를 위하여 Apache Kafka를 설치해야 합니다. Apache Kafka를 설치하기 위해서는 테스트용 컴퓨터에 설치된 Scala 버전을 알아야 합니다. 다음과 같은 명령으로 Scala 버전을 확인합니다. Scala가 설치되지 않았다면 Scala를 먼저 설치해야 합니다.

```
~ > java -version
java version "1.8.0_101"
Java(TM) SE Runtime Environment (build 1.8.0_101-b13)
Java HotSpot(TM) 64-Bit Server VM (build 25.101-b13, mixed mode)
~ > scala -version
Scala code runner version 2.12.2 -- Copyright 2002-2017, LAMP/EPFL and Lightbend, Inc.
~ >
```

위 예제에서 테스트용 컴퓨터의 scala 버전은 2.12.2입니다. 다운로드 받을 Apache kafka 버전은 0.10.2.1입니다. Apache 다운로드 페이지는 다음 URL 입니다.

- https://kafka.apache.org/downloads

scala 버전에 맞는 kafka를 내려받습니다. 현재 예제(Scala 버전:2.12.2, Kafka 버전: 0.10.2.1)에서는 kafka_2.12-0.10.2.1.tgz 입니다. 다음과 같이 대상 파일을 내려받고 압축을 해제합니다.

```
~/demo > curl http://www-us.apache.org/dist/kafka/0.10.2.1/kafka_2.12-0.10.2.1.tgz -o kafka_2.12-0.10.2.1.tgz
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 32.4M  100 32.4M    0     0   623k      0  0:00:53  0:00:53 --:--:--  580k
~/demo >
~/demo > tar -xzf kafka_2.12-0.10.2.1.tgz
~/demo > ls -al kafka_2.12-0.10.2.1
total 72
drwxr-xr-x   8 taewan  staff    272  4 22 01:27 .
drwxr-xr-x   4 taewan  staff    136  8 14 16:26 ..
-rw-r--r--   1 taewan  staff  28824  4 22 01:23 LICENSE
-rw-r--r--   1 taewan  staff    336  4 22 01:23 NOTICE
drwxr-xr-x  31 taewan  staff   1054  4 22 01:27 bin
drwxr-xr-x  15 taewan  staff    510  4 22 01:27 config
drwxr-xr-x  73 taewan  staff   2482  4 22 01:27 libs
drwxr-xr-x   3 taewan  staff    102  4 22 01:27 site-docs
~/demo >
```

### Oracle Event Hub CS 클러스터 테스트

현재 Kafka 블로커 서버 및 ZooKeeper가 설치된 서버의 Public IP는 129.157.161.106이고 Zookeeper가 사용하는 포트 번호는 2181입니다. Apache ZooKeeper에 포함된 topic 목록을 확인하는 명령을 이용하여 다음과 같이 토픽 목록을 확인 할 수 있습니다.

- 현재 Kafka가 설치된 위치: ~/demo/kafka_2.12-0.10.2.1

```
> bin/kafka-topics.sh --list --zookeeper  129.157.161.106:2181
KafkaDemo
__consumer_offsets
krplustvio-twTopic
>
```

Apache Kafka에 포함된 kafka-console-producer.sh와 kafka-console-consumer.sh 명령으로 topic에 데이터를 비동기 처리하는 테스트를 수행할 수 있습니다. <그림 18 참조>

- <그림 18> kafka-console-producer.sh와 kafka-console-consumer.sh 테스트
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub18.jpg)

<그림 18>에서 사용한 명령은 다음과 같습니다.

|Type|Command|
|---|---|
|Producer|bin/kafka-console-producer.sh --broker-list 129.157.161.106:6667 --topic krplustvio-twTopic|
|Consumer| bin/kafka-console-consumer.sh --bootstrap-server 129.157.161.102:6667 --topic krplustvio-twTopic --from-beginning|


## 요약

Oracle Cloud는 Apache Kafka 관리 서비스로 Oracle Event Hub Cloud Service를 제공합니다. 이 서비스를 이용하여 비동기 스트리밍 처리 인프라를 빠르고 쉽게 만들 수 있습니다. Oracle Event Hub Cloud Service는 소프트웨어 패치, 설치, 관리 및 확장을 효과적으로 처리하는 방법을 제공합니다. Oracle Event Hub Cloud Service 클러스터는 생성 시간은 15분입니다.

Oracle Event Hub Cloud Service와 Oracle Big Data Cloud Service - Compute Edition을 이용하여 확장성과 대용량 처리가 가능한 스트리밍 처리 환경을 필요한 시점에 바로 구축할 수 있습니다.

## 참고자료
- [Oracle Public Cloud and Kafka – Events powering the cloud – The Oracle PaaS Cloud Event BusHub](https://technology.amis.nl/2016/09/28/oracle-public-cloud-and-kafka-events-powering-the-cloud-the-oracle-paas-cloud-event-bushub/)
- [Apache Kafka in the Cloud --- Oracle Event Hub Cloud Service](https://www.linkedin.com/pulse/apache-kafka-cloud-oracle-event-hub-service-harris-moin-qureshi)
- [Oracle Official Document: Using Oracle Event Hub Cloud Service](http://docs.oracle.com/en/cloud/paas/event-hub-cloud/ehcug/index.html)
