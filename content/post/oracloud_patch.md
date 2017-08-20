+++
date = "2017-08-20T23:20:25+09:00"
description = "Oracle Cloud의 PaaS 서비스 패치 절차를 소개합니다. Oracle Big Data Cloud Service - Compute Edition의 패치 절차를 대상으로 합니다."
title = "Oracle Cloud의 PaaS 서비스 패치: Orace BDCSCE"
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2017/08/patch/patch00.jpg"
thumbnailInPost = "https://oracloud-kr-teamrepo.github.io/2017/08/bdcsce_demo/postinlist.jpg"
tags = ["patch", "Oracle BDCSCE"]
categories = ["Oracle Cloud"]
author = "taewan.kim"
language = ""  #bsh,c,cc,cpp,cs,csh,cyc,cv,htm,html,java,js,m,mxml,perl,pl,pm,py,rb,sh,xhtml,xml,xsl
+++

Oracle Cloud는 매달 새로운 기능을 클라우드 서비스에 추가하고 있습니다.
기능 개선을 위해서는 소프트웨어 변경이 필요합니다.
이미 클라우드 서비스 인스턴스를 생성하고 사용 중인 고객을 위해서 오라클은 패치를 만들고 적용하는 프로세스를 제공합니다.
클라우드 사용자는 현재 사용 중인 서비스에 패치를 적용 여부와 시점을 직접 결정할 수 있습니다. 본 문서에서는 "__Oracle Big Data Cloud Service - Compute Edition__"(이하 Oracle BDCSCE)의 인스턴스에 패치를 적용하는 방법을 살펴보겠습니다. 여기에서 다루는 Oracle BDCSCE 인스턴스에 패치를 적용하는 절차는 다른 Oracle PaaS에도 적용할 수 있습니다.

## Oracle Cloud 서비스 패치

Oracle Cloud PaaS의 특징은 클라우드 사용자가 OS 영역에 접근이 가능하다는 것입니다. 클라우드 사용자는 PaaS 서비스의 운영체제에 SSH 접근과 소프트웨어를 설치할 수 있습니다.[^1] 이러한 특징은 사용 중인 서비스에 대에 전문 관리 기능을 적용할 수 있다는 점에서 강점으로 분류됩니다. DBMS 관련 서비스(Oracle Database CS, MySQL CS) 영역이나 오픈소스 서비스(Big Data CS, Event CS) 같은 서비스 사용자들에게는 매우 매력적인 기능입니다.

[^1]: 클라우드 사용자가 PaaS 서버에 추가 소프트웨어를 설치할 경우에 발생하는 라이센스 문제는 모두 클라우드 사용자에게 있습니다.

운영체제 접근은 사용자 측면에서는 강점이지만 서비스 운영 측면에서는 어려움이 있습니다. 특히 지속적인 기능 개선으로 패치가 적용되는 상황에서는 사용자가 운영체제 수준에서 변경 내역과 신규 패치 사이에 충돌이 발생할 수 있습니다. Oracle Cloud는 이러한 위험 요소를 고려하여 안전한 패치 프로세스를 제공합니다. Oracle Cloud 패치 프로세스는 다음과 같이 구성됩니다.

- 클라우드 서비스별 대상 패티 공지
- 대상 패치 변경사항 리뷰
- 사전 패치 테스트
- 패치 테스트 결과 리뷰
- 패치 적용
- 패치 적용 목록 관리
- 패치 이전 버전으로 원복(Rollback)

다음 절에서 "Oracle Big Data Cloud Service - Compute Edtion"에 패치를 적용하는 절차를 소개하겠습니다.

## Oracle BDCSCE 패치

Oracle BDCSCE는 2017년 03월에 출시된 빅데이터 클라우드 서비스입니다. Oracle BDCSCE는 현재 매월 신규 기능[^2]이 추가되고 있습니다. 현재 Oracle BDCSCE 서비스 버전은 17.3.3입니다. 17.3.3 버전에는 다음과 같은 기능이 추가되었습니다.

[^2]: Oracle BDCSCE 기능 변경 내용은 다음 링크에서 확인 가능합니다. https://goo.gl/ty14Pu

| Oracle BDCSC - 17.3.3 버전 변경 사항|
|---|
|모든 노드에 pip 설치|
|Storage Cloud Service와 인증 정보가 불일치할 경우 알림 기능|
|Setting 탭에서 Thrift URL 출력 추가|
|Status 탭 추가: 서버별 컴포넌트 상태 출력 (그림 1 참조)|
|오브젝트 저장소 탐색 브라우저 성능 개선|
|Zeppelin 쉘 인터프리터에 Alluxio 명령 추가|
|Big Data File System (BDFS)에 저장된 데이터는 Oracle Storage Cloud Service에 자동 저장|
|Big Data File System (BDFS)의 메모리 할당이 클러스터 생성 시 지정된 크기로 할당 됨(기존에 서버당 1GB로 고정)|

- <그림 1>  Oracle BDCSC - 17.3.3의 UI 기능 개선
![](https://oracloud-kr-teamrepo.github.io/2017/08/patch/satus.jpg)

최근에 17.3.3 버전이 Oracle BDCSCE에 적용되었고, 2017년 7월 초에 생성한 Oracle BDCSCE 인스턴스에 패치를 적용하였습니다. 이 패치 적용과정을 이미지 중심으로 소개하겠습니다. 이 패치 적용 과정에서 Oracle Cloud의 다른 PaaS에도 동일하게 적용될 수 있습니다.

아래에서 사용한 이미지는 패치 환경은 다음과 같습니다.

- 서비스 유형: Oracle BDCSCE
- 인스턴스 이름: bdcsce-demo
- 서비스 버전: 17.3.1-20
- 패치 일시: 2017년 8월 17일

이제부터 Oracle BDCSCE의 17.3.3-20 버전 패치는 적용하는 절차를 소개합니다.

### 패치 목록 관리

Oracle BDCSCE 콘솔은 <그림 2>와 같이 현재 생성된 인스턴스의 목록이 출력됩니다.
<그림 2>에서 bdcsce-demo 클러스터의 버전, 실행 상태, 자원 규모를 확인할 수 있습니다.
Oracle BDCSCE의 인스턴스에 적용할 패치가 만들어지면,
"One or more patch(es) available"이란 메시지가 인스턴스 목록에 출력됩니다.

- <그림 2> Oracle BDCSCE 콘솔에서 하둡 클러스터 목록
![](https://oracloud-kr-teamrepo.github.io/2017/08/patch/patch01.jpg)

"One or more patch(es) available" 링크를 클릭하면 해당 인스턴스(클러스터)의 패치 목록 관리 페이지로 이동합니다.
<그림 3>은 bdcsce-demo 클러스터의 패치 목록 관리 페이지입니다.
<그림 3>에서 BDCS-CE Update 17.3.3-20 패치에 대한 다음과 같은 정보를 확인할 수 있습니다.

1. Release date
1. 업데이트 내용 요약 페이지 링크
1. 적용 대상 서비스 유형
1. 패치 적용 후 서비스 재시작 여부

- <그림 3> BDCSCE 클러스터에 적용할 패치 목록 관리 페이지
![](https://oracloud-kr-teamrepo.github.io/2017/08/patch/patch02.jpg)

### 패치 사전 테스트

현재 서비스되고 있는 클러스터에 패치를 적용할 때 문제점 혹은 기존 바이너리와 충돌이 없는지를 확인하는 기능을 제공합니다.
Oracle Cloud의 PaaS는 운영체제 접근을 허용합니다. 따라서 사용자가 직접 설치한 바이너리 혹은 환경 변수 등이 패치 적용 과정에서 충돌이 발생할 수 있습니다. 이러한 문제를 "__Precheck__" 기능으로 확인 가능합니다. <그림 4>의 오른쪽 위에 위치하는 메뉴에서 "__Precheck__"를 실행할 수 있습니다.

- <그림 4> 적용 대상 패티의 "Precheck" 실행
![](https://oracloud-kr-teamrepo.github.io/2017/08/patch/patch03.jpg)

Oracle DBCSCE의 경우 "__Precheck__"는 약 20분 동안 진행됩니다.
"__Precheck__"는 클러스터가 동작하는 서비스 상태에서 수행되며, 기존 서비스에 영향도는 거의 없습니다.
"__Precheck__"가 완료되면 <그림 5>와 같이 "__Precheck summary__" 링크가 출력됩니다.

- <그림 5> "Precheck" 종료 후 "__Precheck summary__" 링크 출력
![](https://oracloud-kr-teamrepo.github.io/2017/08/patch/patch04.jpg)

"__Precheck summary__" 링크를 출력하면 <그림 6>과 같은 요약 리포트가 출력됩니다.
문제가 없으면 <그림 6>과 같은 메시지가 출력됩니다.
"__Precheck__"에서 문제점이 나타나면, Patch를 수행하기 전에 해결해야 할 작업 목록이 출력됩니다.

- <그림 6> __Precheck summary__
![](https://oracloud-kr-teamrepo.github.io/2017/08/patch/patch05.jpg)

<그림 6>과 같이 패치 적용에 문제가 없다는 메시지를 확인했다면 이제 패치를 적용할 차례입니다.

### 패치 적용

4 node로 구성된 BDCSCE 인스턴스에서 BDCS-CE Update 17.3.3-20 패치 적용 시간은 40분이 소요되었습니다.
적용 시간은 패치와 클러스터 규모에 따라 달라질 수 있습니다.
BDCS-CE Update 17.3.3-20 패치를 적용할 때, 대상 인스턴스는 서비스가 중단됩니다. [^3]

[^3]: 모든 패치 적용 시 서비스가 중단되지는 않습니다. <그림 3>에서 "패치 적용 후 서비스 재시작"으로 분류된 패치는 서비스가 중단된 상태로 패치가 진행됩니다.

<그림 7>과 같이 패치 목록 오른편에 위치하는 메뉴에서 "패치"를 클릭하면 패치가 시작됩니다.

- <그림 7> 패치 적용 메뉴
![](https://oracloud-kr-teamrepo.github.io/2017/08/patch/patch06.jpg)

패치 시작에 앞서 <그림 8>과 같이 패치에 대한 메시지를 입력해야 합니다.
이 메시지는 패치 적용 기록 관리에 이용됩니다.

- <그림 8> 패치 메시지 입력 페이지
![](https://oracloud-kr-teamrepo.github.io/2017/08/patch/patch07.jpg)

패치가 시작되면 대상 클러스터의 상태는 "Service Maintenance"로 출력됩니다.

- <그림 9> 패치 진행 중 클러스터 상태: "Service Maintenance"
![](https://oracloud-kr-teamrepo.github.io/2017/08/patch/patch08.jpg)

클러스터 상세 페이지에서는 패치 진행 상세 정보가 출력됩니다.

- <그림 10> 패치 상태 정보 페이지
![](https://oracloud-kr-teamrepo.github.io/2017/08/patch/patch09.jpg)

### 패치 결과 확인

패치가 완료되면 <그림 11>가 다음과 같이 변경된 것을 확인할 수 있습니다.

1. "진행 중인 프로비저닝 메시지" 박스가 사라짐
1. "status"가 Running으로 변경
1. 버전이 17.3.3-20으로 출력

- <그림 11> 패치 적용 후 클러스터 상세 정보 페이지
![](https://oracloud-kr-teamrepo.github.io/2017/08/patch/patch10.jpg)

### 패치 되돌리기

패치가 완료된 후, 여러 가지 이유로 이전 버전으로 되돌려야 하는 경우가 있습니다.
이런 상황을 대비하여 Oracle Cloud는 패치 "Rollback" 기능을 제공합니다.
Oracle Cloud 패치를 수행하기 전에 기존 환경의 백업을 수행하기 때문에, 필요할 경우 이전 환경으로 되돌릴 수 있습니다.
<그림 12>와 같이 기존 패치 적용 목록의 "Roll Back" 메뉴를 이용하여 이전 버전으로 돌아갈 수 있습니다.

- <그림 12> 패치 "Roll Back" 메뉴
![](https://oracloud-kr-teamrepo.github.io/2017/08/patch/patch11.jpg)

## 요약

Oracle Cloud는 매달 새로운 기능을 서비스에 추가하고 있으며, 기존에 서비스를 만든 사용자를 위하여 패치를 제공합니다.
패치는 사용자가 결정한 시점에 적용 가능합니다.
안전한 패치를 위하여, 패치 사전 테스트(Precheck), 패치 적용, 패치 진행 상태 확인, "패치 Roll Back" 기능을 제공합니다.
Oracle Cloud는 패치 작업 부담과 위험을 최소화하는 안전한 패치 프로세스 제공합니다.  
