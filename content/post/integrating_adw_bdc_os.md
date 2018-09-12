+++
date = "2018-03-31T08:05:14+09:00"
description = "Oracle ADWC(Autonomous Data Warehouse Cloud Service)와 Big Data를 Object Storage로 통합하는 방법을 소개합니다."
title = "Oracle Autonomous Data Warehouse와 Big Data의 오브젝트 스토리지 연계"
thumbnailInList = "https://taewanmerepo.github.io/2018/03/adwc/list.png"
thumbnailInPost = "https://taewanmerepo.github.io/2018/03/adwc/head.jpg"
tags = ["ORACLE", "ADEC", "Autonomous", "DW", "Big Data", "통합", "번역"]
categories = ["java"]
author = "taewan.kim"
language = ""  
jupyter = "false"
adsense = "true"
+++
본 문서는 오라클 공식 블로그의 포스트의 번역입니다.

>
- 원문: https://blogs.oracle.com/bigdata/integrating-autonomous-data-warehouse-big-data-object-storage
- 제목: Integrating Autonomous Data Warehouse and Big Data Using Object Storage
- 작성자: Peter Jeffcock (Big Data Product Marketing)
- 작성일: 2018.03.27

문서를 시작하기 전에 주요 단어를 정리합니다.

- Oracle Autonomous Data Warehouse
  - 오라클 자율 주행 Data warehouse용 데이터베이스 서비스. 일반적으로 ADWC로 부릅니다. ADWC는 주행의 개념을 도입한 오라클 클라우드 기술의 첫 번째 데이터베이스 클라우드 서비스입니다. Oracle ADWC는 차세대 오라클 자율 데이터베이스 기술로 만들어 졌으며, 인공지능을 활용하여 탁월한 안정성과 성능을 제공합니다. 탄력적으로 CPU을 조절(확대/축소)하는 기능을 제공합니다. 필요에 따라서 서비스를 종료하고 사용하는 on-demand 방식의 사용이 가능합니다.
- Oracle Big Data Cloud
  - 오라클 클라우드의 하둡 서비스입니다. 일반적으로 Oracle BDC라고 부릅니다. 오라클 클라우드는 빅데이터 서비스로 2가지 서비스를 제공합니다. 전용 서버 개념이 강한 Oracle Big Data Cloud Service와 가상 머신(VM)을 기반으로하는 Oracle Big Data Cloud 입니다. 본문에서 언급한 Oracle Big Data Cloud는 VM 기반의 Hadoop 서비스입니다. 호튼웍스 기반의 패키지로 구성되며, Spark 1.6 버전과 Spark 2.2 버전을 선택할 수 있습니다. 또한 Zeppline이 설치되어 있습니다. 관련 URL: https://cloud.oracle.com/en_US/big-data-cloud   

----

[Oracle Autonomous Data Warehouse](https://www.oracle.com/database/data-warehouse/index.html)(이하 Oracle ADWC)에 저장된 데이터를 이용하여 비즈니스 환경을 분석하고 관리할 수 있습니다. 이밖에도 잠재적인 가치를 갖는 많은 다른 데이터[^1]도 존재합니다. [Oracle Big Data Cloud](https://cloud.oracle.com/en_US/big-data-cloud)를 이용하여 이런 유형의 데이터를 저장하고 처리할 수 있습니다. 이렇게 처리된 데이터를 ADWC에 적재하고 쿼리로 질의할 수 있습니다. 이 두 서비스의 통합 지점은 [오브젝트 스토리지](https://cloud.oracle.com/storage/object-storage/features)(Object Storage)입니다.      

[^1]:[역자주]전형적인 데이터베이스 관리하는 대상 데이터(정형 데이터 및 기업 비즈니스 데이터) 이외의 데이터를 의미합니다. 전통적으로 데이터베이스에서 관리하지 않고 관리 대상이 아닌 소위 빅데이터에서 관리하는 데이터를 의미합니다.


## Data Lake와 데이터 웨어하우스(DW)의 사용 사례

![](https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/f0499405-1197-4b43-b7c5-40548eeb9f34/Image/e083d22a5783494e403395c8b865758d/autonomous_data_warehouse_big_data.png)

거의 모든 빅데이터 사례에서 데이터는 [Data Lake와 DW(Data Warehouse)](https://blogs.oracle.com/bigdata/data-lake-database-data-warehouse-difference)에 위치합니다. 예를 들어서, [예측 정비(Predictive Maintenance)](https://blogs.oracle.com/bigdata/predictive-maintenance-big-data-use-cases)에서 Data Lake에 저장된 센서 데이터와 DW에 저장된 공식적인 유지보수 구매 기록을 결합하고 싶을 때가 있습니다.

특정 고객의 다음 행동 예측을 예측 분석할 때, 고객의 구매 전표(DW에 저장)와 고객 웹 브라우저 사용 기록 혹은 소셜 미디어 이용(이런 상세 기록은 주로 Data Lake에서 정장 관리)을 함께 분석하려 할 것입니다. 의료기기 제조업의 사례에서 사용 가능한 모든 데이터의 완전한 뷰를 갖고 있다는 것은 DW와 Data Lake 모두에서 데이터 분석 작업을 한다는 것을 의미합니다.     

## 예측 정비를 위한 Data Lake와 Data Warehouse

예측 정비 분야를 예로 살펴보겠습니다. 예측 정비 분야에서 공식 유지보수 기록, 구매 및 보증 정보는 핵심 정보입니다. 이런 정보는 적절한 프로세스가 준수되고 있는지를 규제 기관이 확인하기 위해서 필요합니다. 또한 구매 부서가 예산을 관리하거나 새로운 컴포넌트를 주문하기 위해서 필요할 수도 있습니다.

또한, 기계, 기상 관측소, 온도계, 지진계 및 이런 유형의 장비는 센서는 장비의 동작을 이해하고 예측을 잠재적으로 돕는 데이터를 생산합니다. DW 관리자에게 수 테라바이트의 원시 데이터 형식이고 잘 알려지지 않은 다중 구조 데이터를 DW에서 관리할 것을 요청하면 냉담한 반응을 경험하게 될 것입니다. 이런 종류의 데이터는 Data Lake가 훨씬 더 적합합니다. 이런 데이터는 Data Lake에서 변환되고, 머신 러닝 알고리즘의 입력으로 사용될 수 있습니다. 그러나 궁극적으로 장애와 허용 오차(Tolerance)를 벗어날 컴포넌트를 예축하기 위해서, Dw와 Data Lake의 데이터셋을 결합하여 관리하는 방식을 추구할 것입니다.

## 사례: 오브젝트 스토리지와 DW를 연동하는 방법

[이전 블로그 포스트](https://blogs.oracle.com/bigdata/the-new-data-lake-you-need-more-than-hdfs)에서 [오프젝트 스토리지를 기반으로 데이터 레이크를 구성하는 방법](https://blogs.oracle.com/bigdata/what-is-object-storage)에 대하여 소개했습니다. 본 문서에서 다루는 DW와 오브젝트 스토리지의 연동은 그 이상의 가치를 갖습니다. 오브젝트 스토리지를 DW 백업, 아카이브, DW를 위한 데이터 준비 용도로 사용하거나 더 이상 DW에 저장할 필요가 없는 데이터의 이관(Offload) 용도로 사용할 수 있습니다. 이런 방식을 사용하면 오브젝트 스토리지(Data Lake)와 DW를 효과적으로 연계할 수 있습니다.

예측 정비 사례로 되돌가 보겠습니다. Data Lake(오브젝트 스코리지)에 센서 데이터를 올린 다음에, 이 데이터는 스팍 클러스터에서 처리될 수 있습니다. 오라클 클라우드에서 Oracle BDC로 스팍 클러스터를 만들 수 있습니다. 여기서 말하는 데이터 처리란 단순한 필터 부터 복잡한 머신 러닝 알고리즘으로 미지의 숨은 패턴을 찾아내는 모든것을 의미합니다.

이런 모든 작업이 완료되면, 데이터 처리 결과는 오브젝트 스토리지에 저장됩니다. 그 다음에 오브젝트 소토리지에 저장된 결과는 Oracle ADWC에 적제될 수 있습니다. 이렇게 적제된 데이터는 필요한 시점에 질의 될 수 있습니다. 어떤 방식으로 데이터를 관리하는 것이 가장 최선일까요? 이것은 상황에 따라 결정됩니다. 데이터의 접근 빈도가 높거나 쿼리 성능이 중요한 요소라면, 일반적으로 Oracle ADWC에 대상 데이터를 적제하는 것이 최선입니다. 그리고 오브젝트 스토리지를 저장 계층 구성의 또 다른 레이어로 생각할 수 있습니다. Oracle ADWC는 스토리지 티어로써 RAM, Flash와 디스크를 갖습니다.

또한, ETL 오프로드 사례에서도 유사한 접근법을 볼 수 있습니다. 원천 데이터는 오브젝트 스토리지에 저장됩니다. 데이터 변환 프로세스는 빅데이터 클라우드 Spark 클러스터에서 실행됩니다. 이때 처리된 결과는 오브젝트 스토리지에 저장됩니다. 이렇게 변환된 데이터는 Oracle ADWC에 적제되어 이용가능해 집니다.


## ADWC(Autonomous Data Warehouse)와 Big Data Cloud 연동

Oracle ADEC와 Oracle BDC는 완전히 분리된 다른 서비스가 아닙니다. 이 두 서비스는 상호보완적인 강점을 갖습니다. 오브젝트 스토리지를 매개로 상호운용할 수 있습니다. 이렇게 사용할 때, 모든 데이터를 활용하여 비즈니스 전반에 효율을 얻을 수 있습니다.

더 많은 정보를 필요하다면, Oracle 무료 평가판에 가입하여 직접 Data Lake를 구축하고 체험할 수 있습니다. 오라클 클라우드에서 Data Lake를 구축하는 튜토리얼과 가이드를 제공합니다.[^2]

[^2]:[역자주]Data Lake를 구성하는 가이드는 다음 URL에서 확인할 수 있습니다. - https://go.oracle.com/bigdatajourney
