+++
date = "2017-08-14T23:20:25+09:00"
description = "Oracle Public Cloud에서 Apache Kafka를 클라우드 관리 서비스(PaaS)로 제공합니다. 서비스 명은 Oracle Event Hub Cloud Service입니다. Oracle Event Hub Cloud Service를 소개하고, Kafka 서비스를 생성하는 방법을 소개합니다."
title = "Apache Kafka PaaS: Oracle Event Hub CS 소개"
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/logoinlist.jpg"
thumbnailInPost = "https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/logoinpost.jpg"
tags = ["Event Hub", "Kafka", "Big Data"]
categories = ["Big Data"]
author = "taewan.kim"
language = ""
+++

Oracle Cloud는 Apache Kafka(이하 Kafka)를 클라우드 관리 서비스(PaaS) 형태로 제공하고 있습니다. Oracle Cloud가 2017년 1월에 출시한 "__Oracle Event Hub Cloud Service__"(이하 Oracle Event Hub CS)가 바로 Kafka 관리 서비스입니다. Oracle Event Hub CS는 오라클이 기술지원하고 관리하는 클라우드 서비스로 Kafka 클러스터의 효율적인 관리 방법과 지속적인 확장성을 제공합니다. 본 문서는 다음과 같은 내용으로 구성됩니다.

- Oracle Event Hub CS 소개
- Oracle Event Hub CS 클러스터 생성
- Kafak 클러스터에 Topic 생성
- Topic에 데이터 저장(Producing) & 가져오기(Consuming) 테스트

## Oracle Event Hub CS

Oracle Event Hub CS는 Kafaka 클라우드 관리형 서비스(managed service)입니다. Kafka와 Apache Zookeeper(이하 ZooKeeper)를 포함한 관련 소프트웨어의 설치, 관리, 업그레이드, 모니터링 등 전체 라이프사이클을 관리합니다.

Oracle Event Hub CS는 2017년 1월 출시 당시 Kafka 버전 v0.9를 지원했습니다. 2017년 8월 현재 Oracle Event Hub CS는 Kafka 버전 v0.10.2를 지원합니다.

Oracle Event Hub CS를 이용하면 비동기 메시지 처리 환경을 필요한 시점에 효과적으로 구축할 수 있습니다. 또한 서비스 이용 중에 언제든지 Scale-out 방식으로 확장할 수 있습니다. 특히 Oracle Big Data Cloud Service - Compute Edition과 연동하여 고성능 스트리밍 데이터 처리 환경 혹은 빅데이터 Ingestion 인프라를 구축할 수 있습니다. Oracle Event Hub CS는 다음과 같은 Oracle Cloud 서비스와 연동될 수 있습니다.

- [Oracle Big Data Cloud Service - Compute Edition](https://cloud.oracle.com/en_US/big-data-compute-edition) (이하: Oracle Big Data CS-CE)
- [Oracle IoT Analytics Cloud Service](https://cloud.oracle.com/en_US/iot)
- [Oracle Big Data Preparation Clood Serivce](https://cloud.oracle.com/en_US/big-data-preparation)
- [Oracle Mabile Analytics cloud Service](https://cloud.oracle.com/en_US/log-analytics)

Oracle Event Hub CS는 Oracle Cloud의 Trial 계정으로 이용 가능합니다. 본 문서는 Oracle Cloud Trial 계정을 이용하여 Oracle Event Hub CS의 클러스터 생성 및 "Quick Start" 데모를 진행하겠습니다.

### Oracle Event Hub CS 특징
Oracle Event Hub CS는 다음과 같은 특징을 갖습니다.

- Kafka 관리형 서비스(Managed Service, PaaS)
- Kafka Native API 접근 허용
- Kafka 클러스터 REST API 지원 (Proxy 서버 포함)
- Scale-out 클러스터 확장 기능 <그림 1 참조>
- 소프트웨어 패치 관리 <그림 2 참조>
- Oracle Event Hub CS의 클러스터 VM 접근 허용 (ssh)
- Oracle Big Data CS-CE 클라우드 서비스에 최적화 구성
- Oracle Event Hub CS 클러스터의 라이프사이클 관리 CLI[^1] 제공: PaaS Service Manager(PSM)
- Oracle Cloud의 보안체계에 따른 세밀한 보안 설정[^2]

[^1]: CLI는 Command Line Interface의 약자입니다. 터너멀에서 실행되는 명령어로 구성된 툴을 의미합니다.
[^2]: 오라클 보안 체계는 다음 문서를 참조 하시기 바랍니다. - [오라클 클라우드 Compute CS 보안 적용](../compute_security/)

- <그림 1>. Oracle Event Hub CS 클러스터의 Scale-out
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub20.jpg)

- <그림 2>. Oracle Event Hub CS의 클러스터별 패치 관리
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub19.jpg)

<그림 2>는 현재 설치해야 할 패치가 없으므로 패치 항목 없으므로 표시됩니다.

### Oracle Event Hub CS 구성 컴포넌트

Oracle Event Hub CS에는 다음과 같은 소프트웨어가 설치됩니다.

- Java 1.8
- Scala 2.12.2
- Apache Kafka
- Apache ZooKeeper
- Confluent 3.0.1 (REST proxy, Avro registry)
- NGINX

### Oracle Event Hub CS 클러스터 설치 방식

Oracle Event Hub CS는 다음과 같은 두 가지 설치 방식을 지원합니다.

|설치 방식|설명|비고|
|---|---|---|
|Basic|ZooKeeper와 Kafka가 같은 VM에 설치 되는 방식입니다. <br /> 클러스터 사이즈로 VM 1개와 3개 중 하나를 선택할 수 합니다. |- 고가성 지원 불가[^3]<br />- 테스트 용도에 적합|
|Recommended|Kafka와 Zookeeper가 별도의 VM에 설치됩니다. <br />고가용성 디자인이 적용된 설치 방식입니다. |- 고가용성 지원|

[^3]: Kafka 클러스터가 고가용성을 지원하기 위해서는  Kakfa와 ZooKeeper가 분리되어 개별적인 서버에 설치되어야 합니다. 또한 Kafka와 ZooKeeper의 장애 대응과 복제계수를 고려하여 각각 3개 서버 이상으로 클러스터를 디자인 해야 합니다. Kafka 서버는 일반적으로 3set 이상으로 구성하고 ZooKeeper는 3set 혹은 5set으로 구성합니다.

Oracle Event Hub CS 클러스터가 고가용성을 제공해야 한다면, Recommended 설치 모드에서 다음과 같은 구성을 권장합니다.

|컴포넌트|VM 규모|비고|
|---|---|---|
|Apache Kafka 브로커| 5+ VM|- 최소 5 VM 이상 <br />- Topic 및 Partition 증가에 따라서 확장 가능<br/> - 클러스터 생성 후 VM 추가 가능 < 그림 1 참조><br />- VM 추가에 제약 없음|
|Apache Zookeeper|3 VM|- Zookeeper 운영 정책에 따라 변경 가능<br/>- 1VM, 3VM, 5VM 중 하나를 선택<br/>- 일반적으로 3VM으로 구성|
|REST Proxy 노드| 2 VM |- 설치 시 선택 가능한 컴포넌트<br/>- 1 ~ 4 VM 중 하나를 선택<br/>- 일반적을 2VM으로 구성|

## Oracle Event Hub CS 데모

이 절에서는 오라클 클라우드 Trial 계정을 이용하여 Oracle Event Hub CS 클러스터를 생성하는 절차를 소개합니다.

### Oracle Event Hub CS 클러스커 생성

오라클 클라우드 Trial 계정으로 Oracle Cloud에 로그인하면, <그림 3>과 같이 Oracle Cloud 대시보드가 출력됩니다. 왼쪽 위 메뉴 아이콘을 이용하여 각 클라우드 서비스 콘솔로 이동할 수 있습니다.

- <그림 3>. Oracle Cloud 대시보드
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub01.jpg)

<그림 4>에서 "Event Hub - Dedicated"를 선택하여 Oracle Event Hub CS 서비스 콘솔로 이동합니다.

- <그림 4>. Oracle Cloud 대시보드에서 Oracle Event Hub CS 서비스 콘솔 이동 메뉴
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub02.jpg)

Oracle Cloud 계정으로 처음 Oracle Event Hub CS 서비스 콘솔에 접근했다면, <그림 5>와 같은 환영 페이지가 출력될 것입니다. 환영 페이지가 출력될 경우 "Go to console" 버튼을 클릭하여 Oracle Event Hub CS 서비스 콘솔로 이동합니다.<그림 6 참조>

- <그림 5>. Oracle Event Hub CS 클라우드 서비스 환영 페이지
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub03.jpg)

<그림 6>에서 "인스턴스 생성" 버튼을 클릭하여 Oracle Event Hub CS 클러스터 생성을 시작할 수 있습니다.

- <그림 6>. Oracle Event Hub CS 서비스 콘솔: 클러스터 생성 시작
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub04.jpg)

Oracle Event Hub CS 클러스터 생성 과정은 3단계로 구성됩니다. <그림 7>은 클러스터 생성의 1단계입니다. 클러스터 생성 1단계에서는 클러스터 명 및 관리자 메일 등 기본 정보를 입력합니다. 입력이 완료되면, "다음" 버튼을 클릭하여 클러스터 생성 2단계로 넘어갑니다. <그림 8 참조>

- <그림 7>. Oracle Event Hub CS 클러스터 생성 1단계
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub05.jpg)

클러스터 2단계에서는 설치 방식, Kafka 노드 수 등 클러스터 설치에 필요한 상세 정보를 다음과 같이 입력합니다.

|구분|설정 항목|설명|데모 사용 값|
|---|---|---|---|
|기본정보|배치 유형|Basic과 Recommended 중 선택입니다. Recommended는 고가용성 모드입니다.|Basic 선택|
||SSH Public Key|SSH 공개키를 등록합니다.|SSH 공개키 등록|
|Kafka|Number of Brokers|Kafka 브로커 노드 수|3|
||컴퓨터 구성|Kafka 브로커 VM의 Shape|OC1m 선택|
||Usable Topic Storage|Topic 용 사이즈 지정, GB 단위|50|
|Enable REST Proxy|Enable REST Access|프락시 설치 여부 선택|체크|
||노드수|프락시 서버 노드 수|2|
||컴퓨터 구성|Proxy 서버 VM의 Shape|OC1m 선택|
||사용자 이름|Proxy 서버 사용자 ID|admin|
||Password|Proxy 서버 사용자 ID 인증 패스워드|Welcome1@|
||Confirmed Password|Proxy 서버 사용자 ID 인증 패스워드 확인|Welcome1@|

설정은 <그림 8>을 참조하시기 바랍니다. <그림 8>에서 Recommended 설치 모드를 선택할 경우 ZooKeeper 설치 항목이 추가됩니다.

- <그림 8> Oracle Event Hub CS 클러스터 생성 2단계: 세부 정보 입력
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub06.jpg)

<그림 8>에서 "다음" 메뉴를 선택하면 Oracle Event Hub CS 클러스터 생성 3단계 "클러스터 정보 확인"으로 넘어갑니다.

- <그림 9> 클러스터 생성 정보 확인
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub07.jpg)

<그림 9>에서 "생성" 버튼을 클릭하여 Oracle Event Hub CS 클러스터 생성을 시작합니다. 클러스터 생성에는 약 15분 정도가 걸립니다. <그림 10>은 클러스터 생성이 완료된 결과입니다.

- <그림 10> Oracle Event Hub CS 클러스터 생성 결과, 클러스터 목록 출력
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub08.jpg)

앞에서 생성한 KafkaDemo 클러스터는 5개 VM으로 구성됩니다. 전체 CPU는 5 OCPU[^4]이며 75GB 메모리, 435GB 블록스토리지, 6000개 파티션 용량으로 만들어 졌습니다. <그림 10 참조>

[^4]: OCPU는 Oracle Compute Unit의 줄임말입니다. 1 OCPU는 하이퍼 쓰레딩이 활성화된 Intel Xeon 프로세스 입니다. 따라서 1개의 OCPU는 vCPU 2개에 해당합니다.

<그림 10>의 클러스터 명을 클릭하면 <그림 11>과 같이 클러스터 상세 정보 페이지로 이동합니다. 외부 네트워크에서 클러스터에 접근을 허용하기 위해서, 페이지 상단 메뉴 아이콘을 클릭한 후 "Access Rules" 메뉴를 선택합니다.

- <그림 11> Oracle Event Hub CS 클러스터 상세 페이지에서 "Access Rules" 이동
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub09.jpg)

"Access Rules" 관리페이지로 이동한 후, "Create Rule" 버튼을 클릭하여 클러스터에 적용할 "__Security Rule__"을 생성합니다.

- <그림 12> Oracle Event Hub CS 클러스터 Security Rule 생성
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub10.jpg)

Security Rule에서 <그림 13>과 같이 Kafka 브로커 접근 규칙과 Zookeeper 접근 규칙을 정의 합니다.

|Security Rule 명|소스|목적지|포트|
|---|---|---|---|
|kafkaserver_publicaccess|PUBLIC-INTERNET|kafka_KAFKA_SERVER|6667|
|Zookeeper_publicaccess|PUBLIC-INTERNET|kafka_KAFKA_ZK_SERVER|2181|

외부 인터넷에서 Oracle Event Hub CS 클러스터에 접근을 허용하는 규칙을 생성합니다.

- <그림 13> Security Rule 생성
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub11.jpg)
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub12.jpg)

두 Security Rule이 생성되면 <그림 14>와 같이 Security rule에서 새로 정의한 Security Rule을 확인할 수 있습니다.

- <그림 14> Security Rule 목록
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub13.jpg)

### Oracle Event Hub CS 토픽 생성

지금까지 Oracle Event Hub CS 클러스터를 생성하는 절차를 확인해 보았습니다. 이제는 데이터를 저장할 Topic을 만들 차례입니다. Oracle Event Hub CS 클러스터를 생성한 후에, Oracle Event Hub CS 서비스 콘솔의 메뉴를 살펴보면, "Oracle Event Hub Cloud Services - Topics" 메뉴가 추가된 것을 확인 할 수 있습니다. <그림 15 참조>

"Oracle Event Hub Cloud Services - Topics" 메뉴를 사용하여 Oracle Event Hub CS 클러스터에  Kafka 토픽을 서비스 형태로 생성할 수 있습니다.      

- <그림 15> Oracle Event Hub CS 서비스 콘솔의 메뉴
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub14.jpg)

<그림 15>에서 "Oracle Event Hub Cloud Services - Topics" 메뉴를 선택하먄 Kafka 토픽 생성 페이지로 이동합니다. <그림 16>과 같이 토픽 생성 페이지에서 토픽 명, 파티션 수, 데이터 유지 기간을 입력하고 "다음" 버튼을 클릭합니다.

- <그림 16> Kafka 토픽 생성 페이지
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub15.jpg)

<그림 16>에서 "다음" 버튼을 클릭하면, <그림 17>과 같은 Kafka 토픽 생성 정보 확인 페이지로 이동합니다.
그리고 "생성" 버튼을 클릭하면 클러스터에 topic이 바로 생성됩니다.

- <그림 17> Kafka 토픽 생성 정보 확인 페이지
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub16.jpg)

topic 생성 시간은 약 5초 입니다. 토픽이 정상적으로 생성되면 <그림 18>과 같이 토픽 목록이 출력됩니다. 지금 만든 것은 Topic 서비스 입니다. Topic 서비스에서 만들어지는 실제 토픽의 이름은 다음과 같은 패턴으로 만들어 집니다.

```
실제 Topic 명: <Identity_doman_name>-<Topic_service_name>
```

- <그림 18>. 토픽 목록 페이지
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub17.jpg)

### 테스트용 Apache Kafka 설치

Kafka 클러스터 접속 테스트를 위하여 Kafka를 현재 사용중인 컴퓨터에 설치해야 합니다. 여기에서 "사용중인 컴퓨터"란 여러분들이 작업하는(오라클 클라우드에 접속 중인) 컴퓨터(노트북)을 의미합니다. Kafka를 설치하기 위해서는 설치 대상 컴퓨터에 Scala 버전을 확인해야 합니다. 다음과 같은 명령으로 Scala 버전을 확인할 수 있습니다. 아직 Scala가 설치되지 않은 상태라면, Kafka 설치에 앞서 Scala를 먼저 설치해야 합니다.

```
~ > scala -version
Scala code runner version 2.12.2 -- Copyright 2002-2017, LAMP/EPFL and Lightbend, Inc.
~ >
```

위 예제에서 Kafka를 설치할 컴퓨터에 설치된 scala 버전은 2.12.2입니다. 데모에서 설치할 Kafka 버전은 0.10.2.1입니다. Kafka 다운로드 페이지는 다음 URL 입니다. 다음 페이지에서 scala 버전과 Kafka 버전이 맞는 파일을 내려받야야 합니다.

- https://kafka.apache.org/downloads

현재 예제에서 Scala 버전 2.12.2과 Kafka 버전 0.10.2.1에 맞는 파일은  kafka_2.12-0.10.2.1.tgz 입니다. 다음과 같이 대상 파일을 내려받고 압축을 해제합니다. 아래에서 파일 다운로드 툴로 "__curl__"을 사용하였습니다.

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

현재 Kafka 브로커 서버 및 ZooKeeper가 설치된 서버의 Public IP는 129.157.161.106이고 Zookeeper 포트 번호는 2181, Kafka 브로커 포트 번호는 6667입니다.[^5] Kafka 클러스터에 생성된 topic 목록은 다음 명령을 이용하여 확인 할 수 있습니다.

[^5]: 이 정보는 <그림 11>과 같은 Oracle Event Hub CS 서비스 콘솔의 클러스터 상세 정보 페이지에서 확인 할 수 있습니다. 포트 정보는 <그림 14>와 같이 "Security Rule" 페이지에서 확인 할 수 있습니다.

- 현재 Kafka가 설치된 위치: ~/demo/kafka_2.12-0.10.2.1

```
> bin/kafka-topics.sh --list \
--zookeeper  129.157.161.106:2181

KafkaDemo
__consumer_offsets
krplustvio-twTopic
>
```
위 명령에서 "__\__"는 콘솔에서 명령이 끊어지지 않고 연결된 라인임을 의미합니다. 위 명령은 한 줄로 처리됩니다.

Kafka에 포함된 kafka-console-producer.sh와 kafka-console-consumer.sh 명령으로 topic에 데이터를 비동기 처리하는 테스트를 수행할 수 있습니다.
- <그림 18> kafka-console-producer.sh와 kafka-console-consumer.sh 테스트
![](https://oracloud-kr-teamrepo.github.io/2017/08/eventhub/eventhub18.jpg)

<그림 18>에서 사용한 명령은 다음과 같습니다.

- Producer 명령 (아래 명령은 한 줄로 처리됩니다.)
```
bin/kafka-console-producer.sh \
--broker-list 129.157.161.106:6667 \
--topic krplustvio-twTopic
```

- Consumer 명령 (아래 명령은 한 줄로 처리됩니다.)
```
bin/kafka-console-consumer.sh \
--bootstrap-server 129.157.161.102:6667 \
--topic krplustvio-twTopic --from-beginning
```

<그림 18>에서 왼쪽 콘솔에는 Kafka에서 제공하는 kafka-console-producer.sh가 앞에서 생성한 twTopic에 메시지를 producing하고, 오른쪽 콘솔에는 kafka-console-consumer.sh가 twTopic으로 부터 메시지를 가져옵니다. 왼쪽 콘솔에서 메시지를 입력하면 바로 오른쪽 콘솔에 출력되는 것을 확인할 수 있습니다.

## 요약

Oracle Cloud는 Kafka 관리 서비스로 Oracle Event Hub CS를 제공합니다. 이 서비스를 이용하여 비동기 스트리밍 처리 인프라를 빠르고 쉽게 만들 수 있습니다. Oracle Event Hub CS는 소프트웨어 패치, 설치, 관리 및 확장을 효과적으로 처리하는 방법을 제공합니다. Oracle Event Hub CS 클러스터 생성 시간은 15분입니다.

Oracle Event Hub CS와 Oracle Big Data Cloud Service - Compute Edition을 이용하여 확장성과 대용량 처리가 가능한 스트리밍 및 비동기 메시지 처리 환경을 필요한 시점에 바로 구축할 수 있습니다.

## 참고자료
- [Oracle Public Cloud and Kafka – Events powering the cloud – The Oracle PaaS Cloud Event BusHub](https://technology.amis.nl/2016/09/28/oracle-public-cloud-and-kafka-events-powering-the-cloud-the-oracle-paas-cloud-event-bushub/)
- [Apache Kafka in the Cloud --- Oracle Event Hub Cloud Service](https://www.linkedin.com/pulse/apache-kafka-cloud-oracle-event-hub-service-harris-moin-qureshi)
- [Oracle Official Document: Using Oracle Event Hub Cloud Service](http://docs.oracle.com/en/cloud/paas/event-hub-cloud/ehcug/index.html)
