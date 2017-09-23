+++
date = "2017-09-24T21:28:14+09:00"
description = "오라클 클라우드의 PaaS 서비스는 인스턴스를 생성하는 과정에 Oracle Storage CS의 컨테이너를 등록하는 설정을 포함합니다. 지난 8월에 오라클 클라우드의 Trial 계정이 Credit 체제로 변경되면서 Storage CS의 컨테이너를 등록하는 방식이 변경되었습니다. Storage CS의 컨테이너 등록 정보를 확인하는 방법을 소개합니다. "
title = "Oracle PaaS의 Storage CS 컨테이너 설정 형식 변경"
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2017/04/object_storage_init/list.jpg"
thumbnailInPost = "https://oracloud-kr-teamrepo.github.io/2017/04/storage_cs/post_logo.png"
tags = ["Storage CS", "Oracl BDCSCE", "DBCS", "MySQL CS", "스토리지 컨테이너"]
categories = ["Oracle Cloud"]
author = "taewan.kim"
language = ""  
#jupyter = "true"
+++

오라클 클라우드에서 Oracle BDCS[^1], Oracle MySQL CS[^2] 그리고 Oracle BDCSCE[^3]와 같은 PaaS 서비스는 인스턴스 생성 과정에 Oracle Storage CS의 컨테이너를 등록 하는 항목을 포함합니다. 오라클 클라우드 PaaS는 백업 및 데이터 저장 용도로 Oracle Storage CS를 사용합니다. 지난 8월에 오라클 클라우드의 트라이얼 계정이 Credit 체제로 변경되는 과정에서 Storage CS 컨테이너 등록 형식이 변경되었습니다. 본 문서에는 Storage CS의 컨테이너 등록 정보를 확인하는 방법과 방법을 소개합니다.

[^1]: Oracle BDCS는 Oracle Database를 오라클 클라우드에서 서비스하는 데이터베이스 PaaS 서비스입니다. 오라클은 Oracle Database를 DaaS(Database as a Service)로 표현합니다.

[^2]: Oracle MySQL CS는 Mysql Enterprise Edition을 오라클 클라우드에서 PaaS 형태로 서비스하는 데이터베이스 PaaS 서비스 입니다. 오라클은 Oracle Database를 DaaS(Database as a Service)로 표현합니다.

[^3]: Oracle BDCSCE는 Oracle Big Data Cloud Service - Compute Edition의 약자입니다. 오라클 클라우드에서 PaaS 형태로 제공하는 빅데이터 하둡 클러스터 클라우드 서비스입니다.

## Oracle Storage CS 이용 Oracle PaaS

아래 <그림 1>, <그림 2>, <그림 3>은 각각 Oracle MySQL CS, Oracle Database CS 그리고 Oracle BDCSCE의 Oracle Storage CS의 설정 화면입니다.

{{< img src="https://oracloud-kr-teamrepo.github.io/2017/09/storage_container/mysqlcs.jpg"
title="그림 1"
caption="Oracle MySQL CS의 Storage CS 설정" >}}

{{< img src="https://oracloud-kr-teamrepo.github.io/2017/09/storage_container/dbcs.jpg"
title="그림 2"
caption="Oracle DBCS의 Storage CS 설정" >}}

{{< img src="https://oracloud-kr-teamrepo.github.io/2017/09/storage_container/bdcsce.jpg"
title="그림 3"
caption="Oracle DBCSCE의 Storage CS 설정" >}}

각 PaaS 서비스에서 Oracle Storage CS를 사용하는 요약은 다음과 같습니다.

|서비스 유형|설정 항목|용도|
|---|---|---|
|Oracle MySQL CS|Cloud Storage Container|백업&복구|
|Oracle DBCS|Cloud Storage Container|백업&복구|
|Oracle BDCSCE|Cloud Storage Container|데이터 저장 및 로딩|

## Oracle Storage CS 설정 형식

기존에 Storage Cloud Service 설정 형식은 다음과 같습니다.

```
{SERVICE_NAME}-{IDENTITY_DOMAIN_NAME}/{CONTAINER_NAME}
```

2017년 8월 오라클 클라우드의 트라이얼 계정의 지원이 Credit 방식으로 전환되면서, Storage Cloud Service 설정 형식은 다음과 같이 변경되었습니다.

```
http://<storagedomain>/v1/<schema name>/<container name>
```

위 PaaS 서비스 설정에 새로운 형식으로 Storage CS의 컨테이너 주소를 등록해야 합니다.

## Oracle Storage CS 주소 확인

Identity Domain의 Storage CS 정보는 Oracle Storage CS의 "__서비스 세부 정보__" 페이지에서 확인할 수 있습니다. Oracle Storage CS의 "__서비스 세부 정보__" 페이지는 다음과 같은 경로로 찾아갈 수 있습니다. Oracle Storage CS의 "__서비스 세부 정보__" 페이지는 <그림 4> ~ <그림 7>과 같이 이동할 수 있습니다.

{{< img src="https://oracloud-kr-teamrepo.github.io/2017/09/storage_container/img01.jpg"
title="그림 4"
caption="대시보드에 Storage CS 패널이 없을 경우 사용자 정의 메뉴 클릭" >}}

{{< img src="https://oracloud-kr-teamrepo.github.io/2017/09/storage_container/img02.jpg"
title="그림 5"
caption="대시보드 사용자 정의에서 '__표시__'로 설정'" >}}

{{< img src="https://oracloud-kr-teamrepo.github.io/2017/09/storage_container/img03.jpg"
title="그림 6"
caption="Oracle Storage CS의 세부 정보 실행" >}}

{{< img src="https://oracloud-kr-teamrepo.github.io/2017/09/storage_container/img04.jpg"
title="그림 7"
caption="Oracle Storage CS의 세부 정보에서 REST Endpoint 확인" >}}


"```http://<storagedomain>/v1/<schema name>__```"는 Oracle Storage CS의 세부 정보 페이지의 REST Endpoint의 주소입니다.

위 데모에서 REST Endpoint는 다음과 같습니다.

|Storage CS의 REST Endpoint|
|---|
|https://Storage-a514707.storage.oraclecloud.com/v1/Storage-0961de4eaa364afc937598c31b21ff91|

위 REST Endpoint에서 스토리지 도메인과 스키마 명은 다음과 같이 확인 할 수 있습니다.

- storagedomain: Storage-a514707.storage.oraclecloud.com
- schema name: Storage-0961de4eaa364afc937598c31b21ff91

## Oracle Storage Container 설정

Oracle Storage CS의 세부 정보 페이지의 REST Endpoint에서 확인한 스토리지 도메인과 스키마 명, 컨테이너 명을 결합하여 Oracle Storage Container를 설정할 수 있습니다. 위 <그림 1>, <그림 2>와 <그림 3>에서 실제 설정한 예는 다음과 같습니다.

|그림 #|서비스 유형|Oracle Storage Container |
|---|---|---|
|그림 1|mysql cs|https://Storage-a514707.storage.oraclecloud.com/v1/Storage-0961de4eaa364afc937598c31b21ff91/mysqlbackup|
|그림 2|dbcs|https://Storage-a514707.storage.oraclecloud.com/v1/Storage-0961de4eaa364afc937598c31b21ff91/dbbackup|
|그림 3|bdcsce|https://Storage-a514707.storage.oraclecloud.com/v1/Storage-0961de4eaa364afc937598c31b21ff91/bdcsce|

## 요약

지금까지 Oracle Storage CS의 세부 정보 페이지의 REST Endpoint에서 확인한 스토리지 도메인과 스키마 명 그리고 새로 지정한 컨테이너 명을 조합하여 Oracle Storage CS Container를 설정할 수 있습니다.


## 참고자료
- https://en.wikipedia.org/wiki/Sigmoid_function
- [CHAPTER 5.Why are deep neural networks hard to train?](http://neuralnetworksanddeeplearning.com/chap5.html)
