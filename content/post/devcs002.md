+++
date = "2018-10-01T13:58:54+09:00"
description = "Developer Cloud의 Build Pipeline 사용하여 여러 빌드 Job들이 연결된 일련의 빌드 작업 만들기"
title = "Developer Cloud의 Build Pipeline 사용하기"
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2018/10/devcs002_intro.png"
thumbnailInPost = ""
tags = ["ORACLE", "Oracle Cloud", "Developer Cloud", "CI", "CD", "devops", "오라클 클라우드","pipeline", "파이프라인"]
categories = ["Oracle-Cloud"]
author = "mee-nam.lee"
language = ""  

+++

**Oracle Developer Cloud Service**는 자동화된 **CI/CD**를 위한 빌드 파이프라인 관리를 제공하는 클라우드 서비스입니다. <br/>
이 문서에서는 지난번 신기능에서 소개해 드린 **Pipleline** 사용법에 대해서 좀 더 상세히 알아보려고 합니다. 이전 버전의 Developer Cloud에서는 빌드를 위해서 하나의 단위 Build Job을 구성하고 해당 Job들은 서로 연관 관계를 가지지 않고 독립적으로 돌아가는 형태였습니다. <br/>여기서 소개하는 **Pipeline** 기능은 기존 단위 Job들을 유기적으로 연결하여 Job들간의 연관성을 가지게 구성하는 것입니다.
Pipeline으로 구성된 Job들은 이전 Job의 성공 또는 실패 결과가 이후 Job의 시작 유무에 영향을 미치게 됩니다.
예를 들어 다음과 같은 Pipeline을 구성할 수 있을 것입니다.

![Alt text](https://monosnap.com/image/1mIljkwte2V2RxHKyk6NnyurEMCRbf.png)

이 예시는 이전 Job들의 결과에 따라 후속 Job들의 실행이 결정되는 순차적인 관계를 가집니다. 각 Job들이 모두 정상적으로 수행되고 성공한다면 일련의 Job들로 연결된 Pipeline 빌드가 성공하고 빌드 작업이 종료되는 구조 입니다.

Pipeline은 기존에 생성된 단위 빌드들을 연결하는 과정입니다. 각 단위 빌드 구성에 대해서는 이전 문서를 참고하세요. 이 문서에는 이미 생성되어 있는 Build Job을 기반으로 한 Pipeline 구성에 대해서만 다룰 것 입니다.

## Pipeline 생성하기
Pipeline을 생성하기 위해서 Job 탭 우측의 **Pipelines** 탭을 클릭하여 이동합니다.

![Alt text](https://monosnap.com/image/HdsW0kIxTH4sLaxe9z6ry3tpN7Zqrb.png)

**New Pipeline** 버튼을 클릭하여 위 예시에서 보았던 것과 유사한 순차형 Pipeline을 생성해 보도록 하겠습니다. 

![Alt text](https://monosnap.com/image/P8xtVZiHdfT989ZdDIeYfH3Q9iX2Zh.png)

Pipeline 이름을 다음과 같이 입력하고 **Create**를 클릭합니다. 나머지 설정은 기본값으로 둡니다.

![Alt text](https://monosnap.com/image/N7gjKmrBVZR5MfpD9qsOkaDwBXSccZ.png)

왼편의 **Jobs** 리스트에서 이미 구성되어있는 Job을 선택하여 오른쪽 캔버스에 드래그 합니다. Job들 간의 순서를 화살표를 드래그 앤 드롭 하여 연결합니다.

![Alt text](https://monosnap.com/image/4U0OjFhvVIhZ2qHqfcukLUlQqF0FoO.png)

pipeline이 구성되었고 바로 빌드를 수행하려면 우측의 **Run** 버튼을 클릭합니다.

![Alt text](https://monosnap.com/image/n4lTI2mKxCbVB2MbPbFO38tYthgSGM.png)

Pipeline의 각 빌드 Job들이 순서대로 Queuing 되면서 실행되는 것을 볼 수 있습니다.

![Alt text](https://monosnap.com/image/pTZOikxhnVZu6sXfAEw5iK3390VDZb.png)

pipeline의 빌드가 모두 완료되면 빌드된 instance들의 상태 및 내용을 볼 수 있습니다. 각 Job들의 빌드 #number를 클릭하면 각 빌드별 로그도 확인할 수 있습니다.

![Alt text](https://monosnap.com/image/73g5YVMhP9Ncn5yDPR58jtVTSleDqr.png)

다음의 예시와 같은 병렬 처리 흐름을 가지는 Pipeline도 작성할 수 있습니다.
아래의 Pipeline에서는 Job1이 완료된 후 Job2, Job3, Job4의 빌드가 트리거 되고 Build Queue에 쌓이는 순서에 따라 처리됩니다. Job2, Job3, Job4가 서로 다른 빌드 VM을 사용한다면 동시에 수행될 수도 있습니다. 동일한 빌드 VM을 사용하여 빌드가 수행된다면, Queue에 먼저 쌓이는 순서대로 처리 됩니다.
Job5는 Job4가 완료된 후 트리거 됩니다.

![Alt text](https://monosnap.com/image/4aAYBnHZlmuaWz8qaxhS0hymLqyEwg.png)

이 Pipeline을 실행해 보면 다음과 같이 Job1 종료 후  Job2, Job3, Job4가 Queuing 된 것을 볼 수 있습니다.

![Alt text](https://monosnap.com/image/syp4FDT5YSDCGOcPQUqGZ5RxH16oIK.png)

위에서 설명한 Pipeline은 이전 Job 성공했을 때 후속 Job으로 넘어가는 것 만 예시하였으나, 이전 Job이 실패 하였을 경우에 처리하는 Job을 두고 싶을 경우에는 연결된 화살표를 더블클릭하여 조건을 **Failed**로 변경해 주면 됩니다.

![Alt text](https://monosnap.com/image/sLIv4H9JaHywNvbbSEMXW0FETPHbzZ.png)

여기까지 Developer Cloud 사용하여 여러 빌드들을 조합하여 일련의 빌드 순서를 만드는 Pipeline이라는 기능에 대해 알아보았습니다.
더 자세한 사항은 아래 제품 자료를 참고하세요

## 참고 자료
- [Oracle Developer Cloud - Pipeline](https://docs.oracle.com/en/cloud/paas/developer-cloud/csdcs/managing-project-jobs-and-builds.html#GUID-8A6787EF-2D7E-4322-A7C9-00509920FC1C)
