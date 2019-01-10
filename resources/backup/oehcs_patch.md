+++
date = "2018-05-22T23:20:25+09:00"
description = "Oracle Event Hub CS(OEHCS)의 패치 절차를 소개합니다."
title = "Oracle Cloud의 PaaS 서비스 패치: Event Hub Service"
thumbnailInList = "https://taewanmerepo.github.io/2018/05/oehcspatch/list.png"
thumbnailInPost = ""
tags = ["patch", "Oracle Event Hub", "김태완"]
categories = ["Cloud"]
author = "taewan.kim"
+++

오라클 Event Hub Service의 패치 절차를 소개합니다. Event Hub Service는 오라클 클라우드에서 제공하는 Kafka 관리형 서비스입니다.
Event Hub Service 패치의 특징은 다음과 같습니다.

- 오라클 클라우드 "Event Hub - Dedicated"의 서비스 콘솔에서 수행
- 패치 요청 인터페이스: WebUI, PSM(PaaS Service Manager), REST API
- Precheck 프로세스 제공: 패치 사전 점검
- 패치 Rollback 기능 제공
- 패치는 무중단 서비스로 진행: Rolling Patch

Event Hub Service는 Precheck와 Rollback 패치 기능을 제공하여 서비스 패치의 안전성을 확보하는 동시에 클러스터 노드를 하나씩 패치하는 Rolling Patch 방식으로 패치를 적용합니다. 따라서 패치를 진행하는 Kafka 클러스터는 서비스 상태를 정상적으로 유지할 수 있습니다.

아래는 Oracle PSM의 oehps 서비스의 패치 관련 명령어 도움말입니다.  

```
> sudo -H psm oehpcs help

DESCRIPTION
  Oracle Event Hub Cloud Service - Dedicated

SYNOPSIS
  psm OEHPCS <command> [parameters]

AVAILABLE COMMANDS
  ## 생략
  o available-patches
       List all available patches for Oracle Event Hub Cloud Service - Dedicated...
  o applied-patches
       List all applied patches for Oracle Event Hub Cloud Service - Dedicated...
  o patch
       This operation will apply a patch to the service
  o precheck-patch
       This operation will run a precheck for a patch on the given service
  o rollback
       This operation will rollback a previously applied patch
  o check-health
       Health Check operation.
  ## 생략
>
```

Event Hub Service는 다음과 같은 절차로 진행됩니다.

----

<그림 1>은 Oracle Event Hub Service의 서비스 콘솔 페이지입니다. 이 페이지에서 Kafka 클러스터 목록을 확인할 수 있습니다. 클러스터 중에 패치 대상 클러스터가 존재할 경우 <그림 1>과 같이 패치가 존재한다는 메시지가 출력됩니다. 이 메시지를 클릭하면 클러스터 상세 정보 페이지로 이동합니다.

{{< img src="https://taewanmerepo.github.io/2018/05/oehcspatch/img010.jpg"
title="그림1"
caption="Event Hub Service의 서비스 콘솔에서 패치 대상 클러스터 확인" >}}

<그림 2>의 클러스터 상세 정보 페이지에서 현재 클러스터에 적용할 패치 목록을 확인할 수 있습니다. <그림 2>와 같이 각 패치 목록은 패치 대상 컴포넌트, 재시작 여부 및 패치 내용(Readme 클릭 시 출력) 정보를 확인할 수 있습니다.

{{< img src="https://taewanmerepo.github.io/2018/05/oehcspatch/img020.jpg"
title="그림2"
caption="패치 페이지 이동: 패치 대상 컴포넌트 확인" >}}

오른쪽 메뉴 아이콘을 클릭하고, __Precheck__를 실행할 수 있습니다 Precheck는 패치를 가상으로 적용하며, 현재 패치가 정상적으로 수행될 것인가를 확인합니다. 패치 대상 파일의 권한 및 변경 유무 등을 체크합니다.  

{{< img src="https://taewanmerepo.github.io/2018/05/oehcspatch/img030.jpg"
title="그림3"
caption="Precheck 기능 수행: 패치 안정성 확인" >}}

<그림 4>는 __Precheck__ 시작 확인 단계입니다. __Precheck__ 버튼을 클릭하면 Precheck 프로세스가 시작합니다.  

{{< img src="https://taewanmerepo.github.io/2018/05/oehcspatch/img040.jpg"
title="그림4"
caption="Precheck 수행 확인" >}}

__Precheck__가 시작되면 <그림 5>와 같이 프로세스 시작 메시지가 출력됩니다.

{{< img src="https://taewanmerepo.github.io/2018/05/oehcspatch/img050.jpg"
title="그림5"
caption="Precheck 요청 확인 메시지" >}}

__Precheck__ 프로세스는 약 3분 정도 소요됩니다. __Precheck Summary__를 클릭하여 Precheck 결과를 확인할 수 있습니다.

{{< img src="https://taewanmerepo.github.io/2018/05/oehcspatch/img060.jpg"
title="그림6"
caption="Precheck 결과 확인 요청" >}}

__Precheck__에 이상이 없으면 <그림 7>과 같은 메시지가 출력됩니다.

{{< img src="https://taewanmerepo.github.io/2018/05/oehcspatch/img070.jpg"
title="그림7"
caption="Precheck 결과 요약 설명" >}}

<그림 8>과 같이 오른쪽 메뉴를 거쳐 패치를 요청할 수 있습니다.

{{< img src="https://taewanmerepo.github.io/2018/05/oehcspatch/img080.jpg"
title="그림8"
caption="패치 수행 요청" >}}

패치를 요청하면 현재 패치에 대한 설명을 추가하는 단계를 거칩니다. 소스 코드 버전 관리의 comment와 같은 의미입니다. 패치 목록을 구분하는 설명으로 활용됩니다.
__Patch__ 명령을 클릭하면 패치가 시작됩니다.

{{< img src="https://taewanmerepo.github.io/2018/05/oehcspatch/img090.jpg"
title="그림9"
caption="패치 커멘트 입력" >}}

패치가 요청되면 <그림 10>과 같이 요청 시작 메시지가 출렵됩니다. 오른쪽의 동그라미 화살표 아이콘을 클릭하며 업데이트 된 상태를 확인할 수 있습니다.

{{< img src="https://taewanmerepo.github.io/2018/05/oehcspatch/img100.jpg"
title="그림10"
caption="패치 진행 상태 확인" >}}

모든 패치가 완료되면 그림 11과 같이 ""사용 가능한 패치"에서 패치 목록이 제거 됩니다. 패치를 적용한 이후에 클러스터에 이상이 발생할 경우 <그림 11>과 같이 클러스터 바이너리와 설정을 패치 전 상태로 돌아가는 기능(Rollback)을 제공합니다.

{{< img src="https://taewanmerepo.github.io/2018/05/oehcspatch/img110.jpg"
title="그림11"
caption="패치 수행 결과 확인-Rollback 메뉴 확인" >}}

이렇게 진행되는 패치는 Rolling 패치 형태로 적용됩니다. 1개씩 서버를 점진적으로 패치하는 방식으로, 무중단 상태로 Event Hub 클러스터를 업데이트할 수 있습니다.
서비스 운영 상태에서도 안전하게 클러스터를 패치하는 것이 가능합니다.

## 관련 문서
- [Oracle Cloud의 PaaS 서비스 패치: Orace BDCSCE](http://taewan.kim/cloud/oracloud_patch/)
