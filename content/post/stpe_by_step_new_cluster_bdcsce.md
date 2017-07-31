+++
date = "2017-07-31T11:59:47+09:00"
description = "Oracle Big Data Cloud Service Compute-Edition은 오라클 클라우드의 Hadoop PaaS 서비스입니다. Oracle BCCSCE의 클러스터 생성 절차를 소개합니다."
title = "Oracle BDCSCE: 클러스터 생성 "
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2017/07/bdcsce_provisioning/bdcsceInList.jpg"
thumbnailInPost = "https://oracloud-kr-teamrepo.github.io/2017/07/bdcsce_provisioning/oracloud-big-logo.jpg"
tags = ['big data', 'hadoop', 'spark']
categories = ['big data']
author = "taewan.kim"
language = ""
+++

Oracle Big Data Cloud Service Compute-Edition(이하 Oracle BDCSCE)은 PaaS 형태로 제공되는 Oracle Pubic Cloud의 빅데이터 서비스입니다. 본 문서에서는 Oracle BDCSCE 서비스를 이용하여 하둡 클러스터를 생성하는 절차를 소개합니다.

본 문서는 오라클 클라우드 트라이얼 계정을 사용하여 진행하겠습니다. Oracle BDCSCE 서비스를 이용하여 하둡 클러스터를 생성하기 위해서는 오라클 클라우드 계정이 필요합니다. 오라클 계정이 없으면 다음 문서를 참조하여 실습하기 전에 오라클 클라우드 계정을 만드시기 바랍니다.

- [오라클 클라우드 트라이얼 계정 생성 - http://www.oracloud.kr/post/accont/](http://www.oracloud.kr/post/accont/)

Oracle BDCSCE 서비스에서 클러스터를 생성하기 위해서는 Oracle Storage CS 정보를 입력해야 합니다. 클러스터 생성에 앞서 Oracle Storage CS가 아직 활성화되지 않았다면 다음 문서를 참조하여 활성화하시기 바랍니다.

- [Oracle Storage Cloud Service 활성화 - http://www.oracloud.kr/post/objest-storage-replication/](http://www.oracloud.kr/post/objest-storage-replication/)

## 용어 정리(Glossary of terms)

클러스터 생성 과정에서 사용되는 오라클 클라우드의 용어는 다음과 같이 통일하겠습니다.

1. My Services dashboard
  - 오라클 클라우드에 로그인하면 출력되는 메인 페이지
  - 현재 사용 중인 서비스 사용현황 출력
  - 서비스별 서비스 콘솔 접근 메뉴 제공
1. Oracle BDCSCE Service Console
  - My Services dashboard의 메뉴에서 Oracle BDCSCE를 지정하여 이동
  - Oracle BDCSCE에 생성된 클러스터 정보를 출력하는 페이지
  - 클러스터(인스턴스) 생성 메뉴 제공
  - 클러스터별 콘솔 접근 메뉴 제공
1. Big Data Cluster Console
  - 클러스터별로 제공되는 콘솔
  - Job 관리 UI, Zeppline 노트북 UI, 데이터 저장소 UI 연결
1. Oracle Storage Cloud Service
  - 오라클 클라우드의 스토리지 서비스
  - 이하 Oracle Storage CS로 표시

![](https://oracloud-kr-teamrepo.github.io/2017/07/bdcsce_provisioning/bdcsce_00.jpg)

## Oracle BDCSCE 클러스터 생성 절차

Oracle BDCSCE 클러스터 생성 절차는 단계별로 이미지 중심으로 설명하겠습니다. Oracle BDCSCE 서비스로 클러스터를 생성하는데 걸리는 시간은 약 15분입니다. 본 문서의 출발 시점 다음 이후입니다.

- Oracle Cloud 계정 생성
- Oracle Storage CS 활성화
- Oracle Cloud 로그인

Oracle Cloud에 로그인이 완료되면 <그림 1>과 같이 "My Services dashboard"가 출력됩니다. "My Services dashboard"에서 왼쪽 위의 메뉴 아이콘을 클릭합니다.

- 그림 1. My Services dashboard
![](https://oracloud-kr-teamrepo.github.io/2017/07/bdcsce_provisioning/bdcsce_01.jpg)

<그림 2>와 같이 "My Services dashboard"의 메뉴에서 "Big Data Compute-Edition"을 선택하여 "Oracle BDCSCE Service Console"로 이동합니다.

- 그림 2. My Services dashboard 메뉴에서 "Big Data Compute-Edition" 선택
![](https://oracloud-kr-teamrepo.github.io/2017/07/bdcsce_provisioning/bdcsce_02.jpg)

"Oracle BDCSCE Service Console" 페이지에서 클러스터 생성을 시작하기 위해서 <그림 3>과 같이 "인스턴스 생성" 버튼을 클릭합니다.

- 그림 3. "Oracle BDCSCE Service Console"에서 "인스턴스 생성"
![](https://oracloud-kr-teamrepo.github.io/2017/07/bdcsce_provisioning/bdcsce_03.jpg)

클러스터 생성 절차는 3단계로 구성됩니다. 클러스터 생성 1단계에서는 <그림 4>와 같이 클러스터 기본 정보를 입력합니다. 서비스명, 클러스터 설명 그리고 관리자 메일 3가지 정보를 입력합니다. 트라이얼 계정을 사용할 경우 과금 방식은 "HOURLY"로 고정되어 있습니다. 정보를 모두 입력한 후 "다음" 버튼을 클릭합니다.

- 그림 4. 클러스터 생성 1단계: 클러스터 기본 정보 입력
![](https://oracloud-kr-teamrepo.github.io/2017/07/bdcsce_provisioning/bdcsce_04.jpg)

클러스터 생성 2단계에서는 <그림 5>와 같이 5개 그룹에 대한 18개 입력 정보를 입력합니다.  

- 그림 5. 클러스터 생성 2단계: 클러스터 세부 정보입력
![](https://oracloud-kr-teamrepo.github.io/2017/07/bdcsce_provisioning/bdcsce_05.jpg)

클러스터 생성 2단계에서 입력하는 정보는 크게 5개 영역입니다. 각 입력 항목의 세부 정보는 다음과 같습니다.

### Cluster Configuration

|입력항목|입력 데이터|비고|실습 입력 값|
|---|---|---|---|
|Deployment Profile|Full, Basic 중 선택|클러스터 설치 유형을 선택합니다. <br/> __Full__을 선택하면 Spark, MapReduce, Zeppelin, Hive, Spark Thrift, Big Data File System이 설치됩니다.<br/> __Basic__을 선택하면 Spark, MapReduce 그리고 Zeppline만 설치됩니다.|Full|
|Number of Nodes| 클러스터 노드 수 |클러스터 구성 노드 수의 설정입니다. 최소 구성은 1개 노드 입니다. <br />  HA 구성을 위해서는 최소 3개 노드 이상으로 설정해야 합니다. 기본 설정 값은 __3__ 입니다.|3|
|Nodes designated as Compute Only Slaves|컴퓨트 노드 수| 클러스터 노드를 5개 이상 설정할 경우에 출력되는 항목입니다. 이 항목은 클러스터 노드 중 HDFS를 설치하지 않는 순수 연산 컴퓨터 노드 수를 입력하는 항목입니다. 기본 값은 0 입니다.|해당사항 없음|
|Compute Shape|VM의 Shape 선택|클러스터 각 노드의 VM 자원 할당 유형을 지정합니다. 2017년 7월 현재 다음과 같은 shape이 제공됩니다. <br/> - OC2m - 2.0 OCPU, 30GB RAM<br/> - OC3m - 4.0 OCPU, 60GB RAM<br/> - OC4m - 8.0 OCPU, 120GB RAM<br/> - OC5m - 16.0 OCPU, 240GB RAM<br/> 기본 설정 값은 OC2m 입니다.|OC2m|
|Queue Profile|Job 대기열 프로파일 지정|Job을 실행하는 프로파일을 지정합니다. 선택 가능한 값은 Preemption On/Preemption Off 두 가지 입니다. 이 설정은 클러스터 관리 UI에서 변경 가능합니다. 기본 값은 Preemption On입니다.|Preemption On|
|Spark Version|1.6, 2.1 중 선택|클러스터에 설치될 Spark 버전을 지정합니다. | 2.1|

### Credentials

이 그룹에서는 클러스터의 각 노드 OS에 SSH 접근과 클러스터 별로 만들어지는 Big Data Cluster Console에 접근하는 계정과 패스워드를 설정합니다.

<그림 5>에서 SSH Public Key의 편집 버튼을 클릭하면 <그림 6>과 같은 팝업이 출력됩니다. 기존에 SSH 키를 갖고 있다면 Public key를 등록할 수 있습니다. 실습에서는 SSH 신규 생성을 사용할 것입니다. <그림 6>과 같이 "Create a New Key"을 선택하고 "입력" 버튼을 클릭합니다.

- 그림 6.  SSH 키 파일 생성 선택
![](https://oracloud-kr-teamrepo.github.io/2017/07/bdcsce_provisioning/bdcsce_06.jpg)

새로운 SSH 키가 생성되면 <그림 7>과 같은 팝업이 오픈되고 "Down"버튼을 클릭하면 SSH 키가 다운로드 됩니다. 그리고 "Done" 버튼을 클릭하면 팝업이 사라지고 다음 절차로 넘어갑니다.

- 그림 7. SSH 키 다운로드 메뉴
![](https://oracloud-kr-teamrepo.github.io/2017/07/bdcsce_provisioning/bdcsce_07.jpg)

나머지 입력 항목은 "Big Data Cluster Console"의 로그인 계정명과 패스워드 입력 항목입니다. "Big Data Cluster Console"의 administrative User의 기본 값은 "bdcsce_admin"입니다. 이 값을 그대로 사용할 것을 권장합니다.

패스워드는 여러분들이 기억하기 쉬운 패스워드를 입력합니다. 패스워드에는 영문 대문자, 특수문자, 숫가가 각각 1개 이상 포함되어야 합니다. (패스워드 예시: Welcome1!)

### Cloud Storage Credentials
"Oracle Storage CS" 정보를 입력합니다.

|항목|입력값|비고|실습 입력 값|
|---|---|---|---|
|Cloud Storage Container|컨테이너 명 입력|Oracle BDCSCE와 Oracle Storage CS을 연동하기 위한 컨테이너 명을 입력합니다. 기본 형식은 다음과 같습니다. <br /> 형식 - <br />  ```Storage-<identity_domain_name>/<container name>```<br /> 위의 identity domain name은 트라이얼 계정에 개별적으로 생성됩니다.|Storage-krplustvio/bdcscontainer|
|Username|로그인ID|트라이얼 계정 로그인 아이디를 입력합니다. |트라이얼 계정 아이디 입력|
|Password|패스워드|트라이얼 계정 로그인 패스워드를 입력합니다. |트라이얼 계정 패스워드 입력|
|Create Cloud Storage Container|옵션|지정한 컨테어너가 없을 경우 Oracle Storage CS를 생성할 것인가를 지정하는 옵션입니다. |체크|

위 표에서 "Cloud Storage Container"의 실제 입력 값은 제가 실수에서 사용한 값입니다. 여러분이 실습 할 경우 ```Storage-krplustvio/bdcscontainer```에서 krplustvio를 여러분의 도메인 명으로 변경하셔야 합니다.

### Block Storage Settings

여기에서는 HDFS의 용량을 설정하는 부분입니다. 실습에서는 기본 값을 그대로 사용합니다. 기본 값은 50GB입니다.

|항목|입력값|비고|실습 입력 값|
|---|---|---|---|
|Usable HDFS Storage (GB)	|HDFS 용량|GB단위로 입력합니다. 실제 할당되는 사이즈는 복제를 고려하여 설정값의 2배입니다. 추가적으로 HDFS의 로그를 고려하여 5%를 추가로 할당합니다. |50|
|Usable BDFS Cache (GB) |Cache 용량|GB단위로 입력합니다. 캐쉬로 할당되는 용량입니다.  |50|

### Associations
Oracle Databas CS, Oracle MySQL CS 그리고 Oracle Event Hub CS를 연돌할 때 입력하는 항목입니다. 이 부분은 별도 연동 문서에서 다루도록 하겠습니다. 현재는 별도로 선택을 할 필요가 없습니다.

모든 입력이 완료되면 <그림 8>에서 "다음" 버튼을 클릭하여 마지막 절차인 확인 페이지로 이동합니다. (<그림 9> 참조)

- 그림 8. 클러스터 생성 정보 인력 완료
![](https://oracloud-kr-teamrepo.github.io/2017/07/bdcsce_provisioning/bdcsce_08.jpg)

<그림 9>에서 입력된 정보를 확인하고 "생성" 버튼을 클릭하여 클러스터 생성을 시작합니다.
클러스터 생성이 정상적으로 시작되면 <그림 10>과 같이 Oracle BDCSCE Service Console로 이동합니다. Oracle BDCSCE Service Console에서는 bdcsdemo 클러스터가 생성중인 것을 확인 할 수 있습니다.

- 그림 9. 클러스터 생성 정보 확인 페이지
![](https://oracloud-kr-teamrepo.github.io/2017/07/bdcsce_provisioning/bdcsce_09.jpg)

클러스터 생성에는 약 10~15분 정도가 소요됩니다.

- 그림 10. Oracle BDCSCE Service Console의 클러스터 생성 메세지
![](https://oracloud-kr-teamrepo.github.io/2017/07/bdcsce_provisioning/bdcsce_10.jpg)

## Oracle BDCSCE 클러스터 로그인

클러스터가 정상적으로 생성되면 <그림 11>과 같이 클러스터 정보에 status 항목이 사라지고 저장 영역에 용량이 표시되는 것을 확인 할 수 있습니다.

- 그림 11. 클러스터 생성 후 Oracle BDCSCE Service Console
![](https://oracloud-kr-teamrepo.github.io/2017/07/bdcsce_provisioning/bdcsce_11.jpg)

앞에서 생성한 클러스터의 Big Data Cluster Console에 로그인하기 위해서는 Oracle BDCSCE Service Console에서 클러스터의 메뉴 아이콘을 클릭하고 "Big Data Cluster Console" 메뉴를 선택합니다.(<그림 12> 참조)

- 그림 12. Big Data Cluster Console에서 Big Data Cluster Console 이동
![](https://oracloud-kr-teamrepo.github.io/2017/07/bdcsce_provisioning/bdcsce_12.jpg)

"Big Data Cluster Console"로 이동할 때, 크롬 브라우저를 사용할 경우 <그림 13>와 같은 페이지가 출력됩니다. 이 현상은 "Big Data Cluster Console" 서버에 설정된 인증서와 서버 도메인 명 불일치로 발생할 수 있습니다. <그림 13>과 같이 이동 링크를 출력하여 로그인 페이지로 이동합니다.

- 그림 13. 크롬 브라우저의 안전확인 페이지
![](https://oracloud-kr-teamrepo.github.io/2017/07/bdcsce_provisioning/bdcsce_13.jpg)

<그림 14>과 같이 로그인 팝업이 출력됩니다. BDCSCE 클러스터 생성시 설정한 아이디와 패스워드를 입력합니다.

- 그림 14. "Big Data Cluster Console" 로그인 Big Data Cluster Console
![](https://oracloud-kr-teamrepo.github.io/2017/07/bdcsce_provisioning/bdcsce_14.jpg)

<그림 14>의 인증이 완료되면 <그림 15>와 같은 Big Data Cluster Console이 출력됩니다. 이 콘솔에서 클러스터의 전반적인 자원 운영 상황을 확인할 수 있습니다.

- 그림 15. Big Data Cluster Console: 클러스터 운영 정보
![](https://oracloud-kr-teamrepo.github.io/2017/07/bdcsce_provisioning/bdcsce_15.jpg)

## Big Data Cluster Console 구성
Oracle BDCSCE 서비스의 각 클러스터에는 개별적으로 Big Data Cluster Console이 생됩니다. Big Data Cluster Console은 다음과 같은 기능을 제공 합니다.

- Overview
- Jobs
- Notebook
- Data Stores
- Settings

### Overview

Overview에서는 클러스터의 전반적인 자원 운영 상황을 그래프 형태로 출력합니다(<그림 15> 참조). 출력 항목은 다음과 같습니다.

- HDFS 용량
- CPU 사용 상태
- Memroy 사용 현황
- Job 요약
- Job 히스토리

### Jobs

Jobs에서는 기존에 실행된 Job들 정보가 출력되고 기존에 실행된 Job들의 로그를 조회할 수 있습니다.(<그림 16> 참조) 추가적으로 새로운 Job을 등록하고 실행할 수 있습니다. 등록 대상 Job 유형은 다음과 같습니다.  

- Spark Job
- PySpark Job
- MapReduce Job

- 그림 16. Jobs 텝
![](https://oracloud-kr-teamrepo.github.io/2017/07/bdcsce_provisioning/bdcsce_16.jpg)

### Notebook

Oracle BDCSCE에는 데이터 분석의 효율성을 높이는 방식으로 Apache Zeppline을 포함합니다. Apache Zeppline의 문서를 Notebook이라고 합니다. Oracle BDCSCE 클러스터를 생성하면, 기본적으로 4개의 Notebook이 저장되어 있습니다.<그림 17참조>  

- 그림 17. Notebook 텝
![](https://oracloud-kr-teamrepo.github.io/2017/07/bdcsce_provisioning/bdcsce_17.jpg)

<그림 17>에서 "Australian Dataset (SparkSQL example)"을 클릭하면 <그림 18>과 같이 Notebook이 오픈됩니다. "Australian Dataset (SparkSQL example)" Notebook의 항목 별로 오른편의 삼각형 아이콘을 클릭하면 각 항목이 실행되는 것을 확인할 수 있습니다. Zeppline Notebook에 대해서는 별도 문서로 다루겠습니다.

- 그림 18. Notebook 실행
![](https://oracloud-kr-teamrepo.github.io/2017/07/bdcsce_provisioning/bdcsce_18.jpg)

### Data Stores

Data Stores 텝에서는 HDFS 디렉터리와 Oracle Store CS의 데이터를 조회하는 기능을 제공합니다.
<그림 19>는 HDFS 디렉터리를 조회 화면한 결과입니다.

- 그림 19. HDFS 디렉터리 조회
![](https://oracloud-kr-teamrepo.github.io/2017/07/bdcsce_provisioning/bdcsce_19.jpg)

### Settings

Setting 텝에서는  클러스터이 기본 설정을 관리할 수 있습니다. 기본적인 Queue 설정(<그림 20> 참조), 보안 설정(<그림 21> 참조), Zeppline 관련 노트북 설정(<그림 22> 참조)을 할 수 있습니다.

- 그림 20. Queue 설정
![](https://oracloud-kr-teamrepo.github.io/2017/07/bdcsce_provisioning/bdcsce_20.jpg)

- 그림 21. 보안 정보 수정
![](https://oracloud-kr-teamrepo.github.io/2017/07/bdcsce_provisioning/bdcsce_21.jpg)

- 그림 22. Apache Zeppline 관련 설정
![](https://oracloud-kr-teamrepo.github.io/2017/07/bdcsce_provisioning/bdcsce_22.jpg)

## 요약

지금까지 Oracle BDCSCE의 클러스터를 생성하는 과정을 살펴 보았습니다. Oracle BDCSCE의 Oracle BDCSCE Service Console에서 하둡 클러스터를 생성할 수 있습니다. 생성 절차는 3단계로 구성할 수 있습니다. 하둡 클러스터의 노드 수와 Shape 및 계정 정보를 입력하면 15분 안에 클러스터를 생성할 수 있습니다. 클러서터가 생성되면 Big Data Cluster Console을 통해서 하둡 클러스터에 접근할 수 있습니다. Big Data Cluster Console에서는 클러스터 자원 사용 현황 대쉬보드, Job 관리, Notebook 관리, 스토리지 조회 그리고 클러스터 설정 메뉴를 제공합니다.

## 관련 문서

1. [Oracle Big Data Cloud Service Compute-Edition 소개](../bdcsce01)
1. Oracle BDCSCE: 클러스터 생성

## Reference

- [Official Tutorial- Using Oracle Big Data Cloud Service – Compute Edition](http://docs.oracle.com/en/cloud/paas/big-data-compute-cloud/csspc/index.html)
- [Toad World- Introduction to Oracle Big Data Cloud Service – Compute Edition](https://www.toadworld.com/platforms/oracle/b/weblog/archive/2017/06/14/introduction-to-oracle-big-data-cloud-service-compute-edition-part-ii-services)
- [Getting Started with Oracle Big Data Cloud Service - Compute Edition](http://www.oracle.com/webfolder/technetwork/tutorials/obe/cloud/bdcsce/crecluster/getting_started%20with%20BDCE-CE.html)
