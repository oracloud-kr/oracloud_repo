+++
date = "2018-09-08T01:09:25+09:00"
description = "2018 Oracle Impact 행사에서 발표한 'Orcle Cloud AI 서비스' 세션 내용을 정리한 문서입니다. 2018.09 현재 오라클이 준비한 AI 서비스와 향후 공개될 AI 플랫폼에 대하여 소개합니다. 추가로 오라클이 추구하는 Enterprise AI 관점을 소개하였습니다."
title = "'Oracle Cloud AI 서비스' @ Oracle Impact"
thumbnailInList = "https://taewanmerepo.github.io/2018/09/impact/list.jpg"
thumbnailInPost = "https://taewanmerepo.github.io/2018/09/impact/post.jpg"
tags = ["Oracle", "Cloud", "AI"]
categories = ["cloud"]
author = "taewan.kim"
language = ""
adsense = "true"
+++

----

2018월 9월 7일에 르메르디앙 호텔에서 __Oracle IMPACT__ 세미나가 진행되었습니다. 이 행사에서 오라클이 바라보는 AI의 관점과 현재 Oracle Cloud의 AI 지원 서비스 및 향후 비전을 주제로 발표를 했습니다. 발표 내용을 간략하게 정리하겠습니다. 

----

## Oracle Impact: Oracle Cloud AI 

### Enterprise AI

{{< img src="https://taewanmerepo.github.io/2018/09/impact/010.jpg"
title="그림 1" caption="오라클 AI 지향점" >}}

오라클은 오라클 클라우드에서 "엔터프라이즈 AI"관점에서 서비스를 만들고 있으며, 이 서비스를 고도화하고 있습니다. 오라클이 지향하는 AI는 기업이 갖고 있는 데이터 혹은 분석 대상이 되는 정형 &비정형 데이터를 효과적으로 분석하는 환경 체계를 제공하는 것입니다. 기업이 소유한 레거시 시스템, 데이터베이스, ERP 데이터를 중심으로 다양한 비정형 데이터와 소셜 데이터를 대상으로 합니다.

### Enterprise AI의 핵심 요소
{{< img src="https://taewanmerepo.github.io/2018/09/impact/020.jpg"
title="그림 2" caption="Enterprise AI 핵심 구성 요소" >}}

체계적인 분석 환경을 제공하기 위해서는 다음과 같은 4가지 요소를 충족해야 합니다. 오라클은 이 4가지 구성요소를 주제로 데이터 분석 서비스를 제공하고 있으며, 개별적인 분석 서비스를 통합하는 플랫폼 서비스를 준비 중에 있습니다. 

- 핵심 고려 사항
    - 제약 없는 데이터 레이크
    - 데이터 수집 
        - 정형(DBMS)
            - Streaming: Golden Gate
            - Batch: ETL
        - 비정형
            - Streaming: 관리형 카프카 서비스(Event Hub)
            - Batch: ETL, CLI, SDK(Java, Python, Go, Ruby...)
    - ERP & Apps 통합 
        - AIA(Adaptive Intelligent Apps)
    - 데이터 거버넌스 
        - Data Catalog Cloud Service (<font color='red'>*</font>)
            - 데이터 객체 추적성 제공

(<font color='red'>*</font>) 표시는 현재 준비 중인 서비스를 의미합니다.

### Oracle Cloud의 Data Lake와 서비스
{{< img src="https://taewanmerepo.github.io/2018/09/impact/030.jpg"
title="그림 3" caption="오라클 클라우드 Data Lake" >}}

데이터 분석을 위해서는 데이터의 크기 및 포맷에 제약이 없고, 고가용성을 제공하는 데이터 저장소가 필요합니다. 이러한 데이터 저장소를 일반적으로 __Data Lake__라고 합니다. 

오라클 클라우드는 Object Storage를 데이터 레이크로 사용하고 있으며, 모든 서비스는 Data Lake를 중심으로 통합되어 있습니다. 모든 서비스는 데이터 레이크에 데이터를 저장하고 읽어 올 수 있으며, 모든 서비스는 Object Storage를 중심으로 데이터를 교환하고 통합할 수 있습니다.

### Oracle Cloud의 데이터 분석 서비스
{{< img src="https://taewanmerepo.github.io/2018/09/impact/040.jpg"
title="그림 4" caption="오라클 클라우드 데이터 분석 서비스" >}}

현재 오라클은 다음과 같은 4개의 분석 서비스를 제공합니다. 각 서비스는 Big Data, RDBMS, BI 기술로 만들어졌으며, 사용자는 선호하는 기술의 서비스를 선택하여 사용할 수 있습니다.

|서비스명|약어| 기반 구현 기술|
|----|----|----|
|Big Data Cloud|BDC|Hadoop Eco 기반의 대용량 데이터 분석 서비스|
|Autonomous Data Warehouse|ADW|오라클 DW 데이터베이스 기술을 기반으로 하는 데이터 분석 서비스|
|Oracle Analytic Cloud|OAC|BI & 시각화 기술을 근간으로 GUI를 기반으로 데이터 분석 및 탐색 서비스|
|IaaS & GPU|OCI|GPU Shape 제공|


### Oracle Cloud의 분석 서비스 특징

{{< img src="https://taewanmerepo.github.io/2018/09/impact/050.jpg"
title="그림 5" caption="오라클 데이터 분석 서비스 주요 특징" >}}

각 서비스의 특징은 위 사진과 같습니다. 현재 이 모든 서비스는 오라클 클라우드에서 사용 가능합니다. 데이터의 특성 및 사용자가 선호하는 방식에 따라서 빅데이터 기술, SQL, GUI 방식을 선택하여 사용할 수 있고, 이 분석 방식을 조합하여 분석 작업을 진행할 수 있습니다. 

|유형|전처리|데이터 탐색|시각화|전문분석|
|----|----|----|----|---|
|Case1|BDC|ADW|OAC|BDC, ADW|
|Case2|BDC|ADW|ADW|ADW|
|Case3|BDC|BDC|BDC|BDC|
|Case4|OAC|OAC|OAC|OAC|

데이터의 상태와 사용자가 친숙한 기술을 선택하여 Data Lake를 중심으로 데이터를 교환하며 분석 기술을 조합할 수 있습니다.

### 다양한 분석 서비스의 지향점
{{< img src="https://taewanmerepo.github.io/2018/09/impact/060.jpg"
title="그림 6" caption="오라클 Enterrpise AI 목표" >}}

오라클은 다양한 전문 분석 기술을 제공하여 사용자가 회사/팀/개인에 맞는 __적정기술__을 선택할 수 있는 서비스를 제공하는 것을 목표로 합니다. 현재 위 4가지 서비스는 오라클 클라우드에서 이용 가능합니다.

### 데이터 분석 플렛폼: Data Governance
{{< img src="https://taewanmerepo.github.io/2018/09/impact/100.jpg"
title="그림 7" caption="AI 플렛폼의 데이터 거버넌스 필요성" >}}

데이터 분석 과정을 보면 데이터를 수집하고, 데이터를 사람이 이해하는 수준에서 데이터를 피처 엔지니어링을 진행합니다. 그리고 알고리즘이 이해하기 좋은 형태로 데이터를 가공하는 결과를 확인하는 반복적인 과정입니다. 이 과정에서 데이터를 전처리하고 보강하고 변환하는 일련의 과정은 중요한 자산입니다. 이 과정의 작업 절차가 유실될 경우, 컴파일 소프트웨어의 소스코드가 유실되어 지속적인 개선이 불가능한 상황과 유사한 문제가 발생합니다. 

이러한 부분을 보강하기 위해서 오라클은 __Data Catalog Cloud Service__(<font color='red'>*</font>)를 준비 중에 있습니다. __Data Catalog Cloud Service__를 통해서 Data Lake의 파일, 관계형 데이터베이스 테이블, NoSQL의 컬렉션 등 데이터 객체 변환 과정을 추적하고 관리할 수 있습니다. 

이 서비스를 통해서 데이터 분석 과정의 절차가 자산화되고 관리되는 효과를 얻을 수 있습니다. 

### 데이터 분석 플렛폼: Data Science Cloud Service
{{< img src="https://taewanmerepo.github.io/2018/09/impact/090.jpg"
title="그림 8" caption="AI 서비스 플랫폼: Data Science Cloud Service" >}}

이런 다양한 서비스를 개별적으로 인스턴스를 생성하고 데이터를 임포트해서 사용하는 방식은 비효율적입니다. 오라클은 통합적인 분석서비스를 제공하는 것을 목표로 __Data Science Cloud Serivce__를 준비 중에 있습니다. 이 서비스를 통해서 데이터 분석에 관련된 모든 서비스를 하나의 UI와 서비스에서 통일된 작업 흐름을 유지할 수 있습니다. 

세부적인 서비스의 생성 및 호출, 관리는 __Data Science Cloud Serivce__가 전담합니다. 따라서 사용자는 데이터 수집부터 애플리케이션 배포까지 하나의 작업 흐름을 유지할 수 있습니다. 

### 데이터 거버닝과 통합 서비스
{{< img src="https://taewanmerepo.github.io/2018/09/impact/070.jpg"
title="그림 9" caption="오라클 클라우드 AI 플랫폼 구성 요소" >}}

요약하면, 오라클은 데이터 수집부터 분석하는 요소 기술과 데이터의 추적성을 제공하는 거버닝 서비스 그리고 모든 서비스를 통합 관리하는 서비스를 통해서 데이터 분석 플랫폼 제공을 목표로 하고 있습니다. 이런 모든 서비스는 __Data Science Cloud Serivce__로 통합될 예정입니다. 현재 모든 세부분석  서비스는 오라클 클라우드에서 이용 가능합니다. 추가로 Data Catalog Cloud Service와 Data Science Cloud Service는 올해 말 서비스 공개를 목표로 하고 있습니다. 

### 오라클 클라우드 AI의 지향점

{{< img src="https://taewanmerepo.github.io/2018/09/impact/080.jpg"
title="그림 8" caption="오라클의 클라우드 기반 AI 서비스 지향점" >}}

오라클 클라우드가 만드는 AI의 지향점은 "End to End Enterprise AI의 적정 기술"이라고 요약할 수 있습니다. 데이터 수집, 전처리, 분석 그리고 데이터 거버넌스와 완성한 모델을 컨테이너에 배포하는 DevOps를 하나의 분석 플랫폼으로 제공하는 것을 목표로 합니다. 이 과정에서 사용자의 편의와 상태에 따라서 서비스와 분석 기술 및 분석 유형을 직접 선택하고 활용할 수 있으며, 이 모든 것을 통합하는 분석 플랫폼 서비스가 __Data Science Cloud Service__입니다. 이 서비스를 통해서 분석팀 혹은 분석 담당자는 상황에 맞는 __적정기술__을 선택하고 활용할 수 있으며, 궁극적으로 분석 과정을 자산화하고 DevOps로 통합하는 효과적인 체계를 구축할 수 있습니다. 

## 참고자료

전체 발표 자료는 아래 Slideshare에서 확인하실 수 있습니다. 

<iframe src="https://www.slideshare.net/TaewanKim/slideshelf" width="760px" height="570px" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:none;" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe>

