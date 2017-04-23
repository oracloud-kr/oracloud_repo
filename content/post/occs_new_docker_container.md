+++
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2017/04/docker_container/docker.jpg"
date = "2017-04-12T19:34:26+09:00"
tags = ["docker", "container", "tutorial", "cloud"]
description = "Oracle Container Cloud Service는 kubernetes(k8s), docker-swarm 그리고 marathon 과 같은 container orchestration 툴입니다. Oracle Container Cloud Service를 사용하여 서비스와 스택을 배포하고 관리하는 과정을 설명합니다."
categories = ["docker", "container"]
thumbnailInPost = "https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step000.jpg"
title = "OCCS에 Docker 이미지 배포하기"
language = ""
author = "taewan.kim"

+++
Oracle Container Cloud Service(이하 OCCS)는 오라클 클라우드가 제공하는 Docker 서비스입니다. OCCS에는 [Kubernetes](https://kubernetes.io/)(k8s), [docker-swarm](https://docs.docker.com/swarm/overview/) 그리고 [marathon](https://mesosphere.github.io/marathon/) 과 유사한 container orchestration 도구가 포함되어 있습니다.

이 문서는 이미 OCCS 인스턴스가 생성되었다는 것을 전제로 합니다. 오라클 클라우드 계정이 확보되지 않았거나 OCCS 인스턴스를 만들지 않은 상태라면, [[오라클 클라우드 계정 생성하기](/post/accont/)] 문서와  [[Oracle Container CS 생성 절차](/post/occs-new-inst/)] 문서를 참조하여 계정과 인스턴스를 생성하시기 바랍니다.

## OCCS 용어 정리

|Container CS 용어|설명|
|---|---|
|Service|OCCS를 사용하여 생성, 배포, 관리 가능한 고수준 설정 객체, Docker 이미지에 OCCS관련 설정을 추가한 것|
|Stack|복수의 Serivce로 구성된 이미지 관리 체계. Compose 설정 포멧을 사용하는 고수준 설정 객체|
|Deployment (배포)|Service와 Stack을 배포하여 하나 이상의 컨테이너를 만드는 것|
|Resource Pool|호스트 서버를 상호 독립적인 그룹으로 구성하는 묶음. 각 Resource Pool은 하나 이상의 호스트(가상머신)로 구성. 하나의 호스트는 하나의 Resource Pool에만 할당됨. 한 호스트가 복수의 Resource Pool에 할당될 수 없음.|
|Container|Serivce혹은 Stack형태로 배포된 Docker 이미지|
|Service Discovery|컨테이너의 네트워크 설정 정보를 관리하는 OCCS의 기능. 컨테이너 사이의 통신에 이 정보를 활용|


## OCCS에 Docker 이미지 배포하기

본 문서에서는 다음과 같은 주제를 다루겠습니다.

- Oracle Cloud 로그인 및 OCCS 서비스 콘솔 접근
- OCCS 컨테이너 콘솔 로그인
- 'Hello World' 컨테이너 배포
- Resource Pool
- 컨테이너 조회 및 중지
- 이미지 조회
- Registries
- Service Discovery
- Stack 배포
- Stack Scale-out
- 컨테이너 로그 조회
- Stack 편집

본 문서의 내용에는 3개의 웹 페이지가 등장합니다. 각 웹 페이지는 다음 이름으로 통일합니다.

- Oracle Cloud 대시보드: Oracle Cloud 계정으로 로그인이 완료되면 접근하는 페이지
- OCCS 서비스 콘솔: OCCS 인스턴스를 생성하는 OCCS 메인 페이지
- Container 콘솔: OCCS 인스턴스의 관리 페이지. OCCS 인스턴스 생성시 설정한 관리자 계정으로 로그인.

### Oracle Cloud 로그인 및 OCCS 서비스 콘솔

OCCS의 컨테이너 콘솔에 접근하기 위해서는 Oracle Cloud에 로그인해야 합니다.
로그인하기 위해서는 <그림 1>과 같이 ```http://cloud.oracle.com```에서 ```Sign In``` 버튼을 클릭합니다.

- 그림 1. cloud.oracle.com에서 로그인 페이지로 이동
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step010.jpg)

<그림 1>에서 ```Sign In```을 클릭하면 <그림 2>의 로그인 페이지로 이동합니다. 로그인 페이지에서 사용자 계정 유형과 데이터 센터를 선택합니다. 현재 Trail 계정을 사용로 작업하고 있다면 다음과 같이 설정합니다[^1].

- 계정 유형: Traditional Cloud Account
- 데이터 센터: Public Cloud Services - US

[^1]: 한국에서 Trial 계정을 신청하면 2017년 04월 현재 ```Public Cloud Services - US```에 계정이 생성됩니다. 이 데이터 센터는 Chicago-US2 데이터 센터와 같은 곳입니다.  

계정 유형과 데이터 센터를 선택하고 ```My Services``` 버튼을 클릭합니다.

- 그림 2. 계정 유형 및 데이터 센터 설정
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step020.jpg)

<그림 2>에서  ```My Services``` 클릭하면, <그림 3> 도메인 입력 페이지로 이동합니다. 여기서 도메인이란 오라클 클라우드 서비스를 묶는 단위입니다. 계정 생성 시 할당된 도메인명을 입력합니다. 도메인명[^2]은 계정 생성 마지막 단계에서 발송된 메일에서 확인 가능합니다. 도메인명을 입력하고 ```실행``` 버튼을 클릭합니다.

[^2]: 도메인명의 기본 형태는 emil의 회사명에 국가 코드를 결합한 형태입니다. 예를 들어서 gildong.hong@xyzweb.com으로 오라클 클라우드 계정을 등록했다면 기본 도메인명은 krxyzweb 입니다. 물론 계정 생성 단계에서 기본 도메인명을 변경할 수 있습니다.

- 그림 3. 도메인명 입력 페이지
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step022.jpg)

이제 로그인 절차의 마지막 단계입니다. 계정과 패스워드를 입력하고 ```사인인``` 버튼을 클릭합니다. 계정은 email 형식입니다.

- 그림 4. 인증 정보 입력 및 로그인
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step024.jpg)

정상적으로 로그인되면 <그림 5>와 같이 Oracle Cloud의 대시보드가 출력됩니다.

- 그림 5. Oracle Cloud 대시보드
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step030.jpg)

<그림 6>과 같이 Oracle Cloud의 대시보드에 출력된 Container 상자 오른편 위에 위치한 메뉴 아이콘을 클릭한 후 ```서비스 콘솔 열기``` 메뉴를 선택하면 OCCS 서비스 콘솔로 이동합니다.

- 그림 6. Oracle Cloud의 대시보드에서 OCCS 서비스 콘솔 열기
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step040.jpg)

### OCCS 컨테이너 콘솔 로그인

<그림 7>의 OCCS 서비스 콘솔에는 현재 생성된 OCCS 인스턴스 목록이 출력됩니다. 현재 설명에서 사용할 인스턴스 명은 ```demo-container```입니다. ```demo-container```의 오른쪽 끝에 위치하는 메뉴 아이콘을 클릭하고 ```Container Console``` 메뉴를 선택합니다.

- 그림 7. OCCS 서비스 콘솔에서 Container 콘솔로 이동
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step060.jpg)

<그림 8>과 같이 Container 콘솔에 접근하기 위해서는 로그인 단계를 거쳐야 합니다. OCCS 인스턴스 생성과정에서 설정한 관리자 계정 명과 패스워드를 입력하고 로그인합니다. 기본 설정 계정명은 admin입니다.

- 그림 8. Contaienr 콘솔의 로그인 페이지
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step070.jpg)

Contaienr 콘솔 로그인이 정상적으로 완료되면, Contaienr 콘솔의 대시보드(<그림 9> 참조)가 출력됩니다. 대시보드에서는 클러스터의 배포 상태 및 호스트, Resource Pool, 컨테이너, 이미지 정보를 확인할 수 있습니다.

- 그림 9. Contaienr 콘솔 대시보드
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step080.jpg)

## 서비스 배포 및 OCCS 기본 기능

### HelloWorld 서비스 배포

이제 Nginx에서 동작하는 Hello World 웹 애플리케이션 더커 이미지를 배포하여 컨테이너를 생성할 것입니다.
Contaienr 콘솔 대시보드의 왼쪽 메뉴에서 <그림 10>와 같이 ```Services``` 메뉴를 클릭하면, OCCS에 빌트인으로 등록되어 있는 23개 서비스 목록이 출력됩니다.
Service는 앞에서 설명한 것처럼 Docker 이미지를 OCCS에서 관리 할 수 있도록 OCCS 설정을 추가한 고수준 관리 객체입니다.
빌트인으로 제공되는 23개의 서비스를 참조하거나 복사하여 새로운 서비스를 만들 수 있습니다.
오른편 상단 검색 창에 "hello"를 입력하여 helloworld 서비스를 찾고, 이 서비스의 ```deploy``` 버튼을 클릭하여 서비스를 배포합니다.  

- 그림 10. helloworld 서비스 배포
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step090.jpg)

<그림 10>에서 ```deploy```버튼을 클릭하면 <그림 11>과 같이 배포를 위한 추가 설정 팝업창이 출력됩니다. <그림 11>의 설정 창에서는 다음과 같은 값을 설정할 수 있습니다.

- Display name: 컨테이너 명
- Resource pool: 컨테이너를 배포할 Resource Pool
- Quality: 컨테이너 수량
- Scheduling policy: 컨테이너 배포 시 Resource pool에서 호스트 선정 기준
  - Random: 임의 선정
  - Memory: 가용한 메모리가 가장 많은 호스트 선정
  - CPU: 가용한 CPU가 가장 높은 호스트 선정
- Available: 복수의 컨테이너 배포 시 어떤 호스트에 배포할 것인지 결정
  - Per pool: pool의 Scheduling policy의 기준에 따라 지정된 Resource Pool의 호스트에 배포
  - Per Host: Resource Pool의 모든 호스트에 컨테이너 배포
  - Per Tag: tag가 할당된 호스트에 배포, per tag 출력시 tag 입력 필드 UI에 추가됨.

데모에서는 기본 설정을 그대로 유지한 상태로 ```deploy```버튼을 클릭하여 서비스를 배포합니다.

- 그림 11. 서비스 배포 설정 팝업
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step100.jpg)

서비스 배포가 시작되면, UI에서 더커 이미지를 내려받는 것을 확인할 수 있습니다<그림 12 참조>. 이때 컨테이너의 상태는 'Pending'으로 출력됩니다. 기본 설정 상태에서 Docker 이미지는 index.docker.io에서 내려받습니다. 현재 설정된 Registry는 왼편 ```Registries``` 메뉴에서 확인할 수 있습니다.

- 그림 12. Docker 이미지 다운로드 상태 - Pending  
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step110.jpg)

<그림 12>에서 docker 이미지 내려받기가 완료되면, <그림 13>와 같이 컨테이너가 실행됩니다. 상태는 'RUNNING' 상태로 표시됩니다.

- 그림 13. 서비스 배포 완료 상태 - Running
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step120.jpg)

컨테이너 상세 정보 페이지에서의 ```containers``` 탭에서는 <그림 13>과 같이 컨테이너 상태, 컨테이너 명, 호스트 명, 업타임, 건강상태 정보를 출력합니다. ```stack YAML``` 탭에서는 <그림 14>와 같이 컨테이너의 실행 YAML 설정을 확인할 수 있습니다. 이 탭에서 컨테이너의 이미지 정보, 포트 정보 및 환경 정보를 확인할 수 있습니다. <그림 14>에서 현재 컨테이너가 동작하는 가상머신(호스트)의 3000 포트가 컨테이너의 80 포트에 바인딩 되어있음을 확인할 수 있습니다.

- 그림 14. 배포 완료된 컨테이너의 실행 정보 - YAML
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step130.jpg)

<그림 15>와 같이 'Containers' 탭에서 Hostname 컬럼의 링크를 클릭하여, helloworld 컨테이너가 동작하는 호스트의 상세 정보가 출력됩니다.

- 그림 15. 컨테이너가 구동하는 호스트 서버의 정보 출력 페이지 이동
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step140.jpg)

<그림 16>에서 helloworld 컨테이너가 동작하는 호스트의 Public IP가 129.144.12.26 이라는 것을 확인 할 수 있습니다.

- 그림 16. 호스트 IP 확인
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step150.jpg)

<그림 14>와 <그림 16>에서 확인한 IP와 포트 정보로부터 helloworld 컨테이너 웹서버 URL은 ```http://129.144.12.26:3000```라는 것을 확인할 수 있습니다. 이 URL에 접근하면 <그림 17>과 같은 결과가 출력됩니다. <그림 17>은 브라우저로 호스트 IP(129.144.12.26)와 포트(3000)에 접근한 결고입니다. 즉 호스트(129.144.12.26)에 동작 중인 helloworld 컨테이너 80 포트에 접근한 결과물 입니다.

- 그림 17. ```http://129.144.12.26:3000``` 접근
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step160.jpg)

### Resource Pool

OCCS 서비스 콘솔에서는 Deployments, Hosts, Resource Pools 등 전반적인 상태 정보를 확인할 수 있습니다.  

'Resource Pool'은 OCCS에서 호스트 서버들의 상호 독립적 그룹 묶음입니다. OCCS 인스턴스를 생성하면 기본적으로 3개의 'Resource Pool'이 정의되어 있습니다. 3개의 Worker 노드로 OCCS 인스턴스를 구성할 때 다음과 같은 'Resource Pool'이 생성되고, 각 'Resource Pool'에는 다음과 같이 호스트가 배분됩니다.

|Resource Pool 명|할당된 Host|
|---|---|
|default|3|
|Developement|0|
|Production|0|

- 그림 18. OCCS 서비스 콘솔의 메인 페이지
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step170.jpg)

왼편 메뉴에서 'Resource Pools'을 클릭하면 각 Resource Pool의 상세 정보를 확인할 수 있습니다. <그림 19>에서 default에 3개의 호스트가 할당된 것을 확인할 수 있습니다.

- 그림 19. default 리소스 풀의 상제 정보
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step180.jpg)

<그림 20>에서 호스트를 선택하고 호스트가 소속된 리소스 풀을 변경할 수 있습니다. <그림 20>에서는 default에 할당된 3개 호스트 중에 1개 호스트를 Development 리소스 풀로 이동하는 것을 설명합니다.

- 그림 20. 1개 호스트를 Development 리소스 풀로 이동
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step190.jpg)

<그림 21>은 default에 할당된 2개의 호스트 중 1개 호스트를 Production 리소스 풀로 이동하는 것을 설명합니다.

- 그림 21. 1개 호스트를 Production 리소스 풀로 이동
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step200.jpg)

결과적으로 <그림 22>에서 3개의 리소스 풀에 각각 1개의 호스트가 할당된 것을 확인할 수 있습니다.

- 그림 22. Resource Pool의 상태 조회
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step220.jpg)

### 컨테이너 조회 및 중지

왼편 메뉴에서 ```Containers``` 메뉴에서 현재 동작 중인 컨테이너의 정보를 조회하고, 작동 상태를 제어할 수 있습니다. <그림 23>에서 현재 동작하는 컨테이너의 ```stop``` 버튼을 클릭하여 컨테이너를 중지합니다.  

- 그림 23. helloworld 컨테이너 중지
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step230.jpg)

컨테이너 상태가 중지되면 <그림 24>와 같이 컨테이너를 다시 실행하거나 중지하는 버튼이 나타납니다. 이 버튼을 이용하여 중지된 컨테이너를 다시 시작하거나 삭제할 수 있습니다.

- 그림 24. helloworld 컨테이너 중지 상태
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step240.jpg)

### 이미지 조회

왼편 메뉴에서 ```Images```로 부터 현재 리서스 풀에 내려받은 더커 이미지를 확인할 수 있습니다. 이 페이지에서 더커 이미지를 실행하거나 내려받은 이미지를 삭제할 수 있습니다. <그림 25 참조>

- 그림 25. 이미지 관리 페이지
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step250.jpg)

### Registries Service

왼편 메뉴에서 ```Registries```에서 더커 이미지를 내려받는 더커 레키스트리를 확인할 수 있습니다. 기본 설정으로 index.docker.io가 설정되어 있습니다. 이 페이지에서 더커 레지스트리를 등록하거나 제거하거나 변경할 수 있습니다. <그림 26 참조>

- 그림 26. Registries 관리 페이지
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step260.jpg)

### Service Discovery

Docker를 운영하는 과정에서 가장 관리하기 어려운 부분은 네트워크 설정입니다.
컨테이너 간의 네트워크 구성이 강 결합 방식으로 구성될 경우[^3], 특정 컨테이너의 호스트 변경, 재시작은 다른 컨테이너의 설정 변경을 유발하는 문제가 발생합니다.
이 문제를 OCCS는 Service Discovery로 해결합니다.
OCCS의 Service를 시작하면 컨테이너 네트워크 구성 정보가 Service Discovery에 등록됩니다.
컨테이너 간의 네트워크 연결은 ```Service Discovery```의 정보를 참조하여 동작합니다.
<그림 27>에서 helloworld 컨테이너가 시작할 때, 해당 컨테이너가 동작하는 host와 컨테이너와 host의 바인딩 포트를 조회할 수 있습니다.   

[^3]: 컨테이너 간의 네트워크 구성이 강 결합 방식으로 구성된다는 것은 컨테이너 실행에서 -p 옵션을 사용하여 호스트와 컨테이너 포트를 바인딩 하거나 --link 옵션을 사용하여 컨테이너 간에 네트워크를 연동하는 방식을 의미합니다.

- 그림 27. Service Discovery 컨테이너 등록 정보 조회
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step270.jpg)

## Stack 배포 & Scale-out

### Stack 배포

OCCS에서 Stack은 복수의 Service로 구성된 이미지 관리 체계이며, 각 서비스의 설정은 Compose 포맷을 사용합니다.
OCCS에는 빌트인으로 3개 Stack이 포함되어 있습니다. 다음은 OCCS의 기본 Stack 목록입니다.

- Redis-Cluster
- Wordpress-multihost-Stack
- Wordpress-singlehost-Stack

이 Stack을 참고하여 새로운 Stack을 만들 수 있습니다. <그림 28>에서 OCCS의 기본 Stack 목록을 확인할 수 있습니다.

<그림 28>의 기본 Stack을 참조하여 새로운 스택을 만들고 관리 할 수 있습니다.
Stack 설정은 더커 커뮤니티에서 범용적으로 사용하는 compose 포멧을 사용하기 때문에,
기존 docker에 익숙한 사용자라면 OCCS의 stack 개념을 바로 이해할 수 있습니다.

<그림 27>에서 Stack 신규 등록, 설정 변경, Stack 배포, 제거 작업을 수행할 수 있습니다.

- 그림 28. Stack 관리 페이지
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step280.jpg)

<그림 29>는 Stack을 실행할 때 배포 설정을 위해서 나타나는 팝업입니다.
앞에서 살펴본 Service 실행과 다른점은 팝업창에 레이어가 존재하는 것입니다.
각 레이어별로 배포 수량, 배포 전략, 호스트 선정 기준을 할 수 있습니다. 설정이 완료되면 ```deploy```버튼을 클릭합니다.

- 그림 29. Stack 배포 설정 팝업
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step290.jpg)

<그림 30>에서 배포가 시작되면 DB 레이어 부터 컨테이너 배포가 시작되는 것을 확인할 수 있습니다.

- 그림 30. DB 레이어가 먼저 시작하는 Stack 배포 상태
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step300.jpg)

<그림 31>과 같이 DB 컨테이너 실행이 완료되면 WordPress 레이어 배포가 시작됩니다.
이때 OCCS의 컨테이너 상태체크(Health-check) 기능이 동작하여
DB 레이어는 초록색으로 WordPress 레이어는 주황색으로 출력합니다.
주황색은 컨테이너가 비정상적인 상태라는 것을 의미합니다. <그림 31>은 Wordpress가 아직 시작 중이기 때문에 현재 기준으로 정상 상태가 아니라는 의미입니다.

- 그림 31. DB 레이어 종료 후, Wordpress레이어 배포 시작
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step310.jpg)

두 레이어의 컨테이너 2개가 모두 시작되면, <그림 32>와 같이 초록색으로 상태가 출력되는 것을 확인할 수 있습니다.
<그림 32>의 Deployments 페이지에서는 Stack에 대한 전반적인 실행 정보 확인이 가능합니다.

- 그림 32. Deployments 페이지
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step320.jpg)

Deployments 페이지의 YAML 탭에서 컨테이너들 사이의 설정 및 구성 정보를 확인할 수 있습니다. <그림 33 참조>

- 그림 33. Deployments의 페이지의 컨테이너들 사이의 설정 - compose 포멧
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step330.jpg)

### Stack 확장: scale-out

OCCS는 배포된 컨테이너에 부하가 증가할 경우에 컨테이너를 확장하는 ```scale-out```기능을 제공합니다.
<그림 34>와 같이 Deployments 메뉴의 컨테이너 상세 페이지에서 ```Change Scaling```을 클릭하여 컨테이너의 배포 수량을 변경할 수 있습니다.

- 그림 34. 컨테이너 배포 수량 변경 (```Change Scaling```)
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step340.jpg)

<그림 35>는 Wordpress 레이어의 수량을 1개에서 4개로 변경합니다. 3개의 컨테이너가 추가로 배포됩니다.

- 그림 35. Wordpress 레이어의 컨테이너를 4개로 변경
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step350.jpg)

<그림 36>의 'Containers' 메뉴에서 현재 6개의 컨테이너가 동작 중인 것을 확인할 수 있습니다.
<그림 34>까지 동작 중인 컨테이너는 3개(helloworld 컨테이너, db 컨테이너, wordpress 컨테이너)였습니다.  
<그림 35>에서 wordpress 컨테이너 수량을 변경하여 3개의 컨테이너가 추가된 상태입니다.

- 그림 36. 3개의 컨테이너가 추가되어 총 6개의 컨테이너가 동작
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step360.jpg)

### 컨테이너 로그 조회

컨테이너 상세 페이지에서 해당 컨테이너의 표준 출력(Standard Output)을 확인할 수 있습니다.
<그림 37>은 컨테이너 상세 페이지 이동 경로를 설명합니다.

- 그림 37. 컨테이너 상세 페이지 이동 경로
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step370.jpg)

해당 컨테이너의 로그를 출력하기 위해서 컨테이너 상세 페이지에서 ```View Logs``` 버튼을 클릭합니다. <그림 38 참조>

- 그림 38. 컨테이너 로그 출력 요청
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step380.jpg)

<그림 39>와 같이 해당 컨테이너 표준 출력을 실시간 확인할 수 있습니다.

- 그림 39. 컨테이너 표준 출력(Standrad Output) 로그 조회
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step390.jpg)

### Stack 편집

OCCS는 Stack을 편집하는 전용 UI를 제공합니다. <그림 40>에서 해당 Stack의 ```Edit```버튼을 클릭하면 Stack GUI 편집기가 출력됩니다.

- 그림 40. OCCS의 GUI 기반 Stack 편집기
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step400.jpg)

Stack GUI 편집기는 Stack의 서비스 상태를 그래픽 형태로 표현하며, Drag&drop 방식과 위저드 방식으로 Stack 설정을 편집할 수 있습니다.
<그림 41>은 Wordpress-singlehost-Stack에 HAProxy Service를 추가하는 동작입니다.

- 그림 41. Wordpress-singlehost-Stack에 HAProxy Service 추가
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step410.jpg)


HAProxy Service를 Stack UI 편집기에 추가하면 <그림 42>와 같이 추가된 Service를 설정하는 위저드가 나타납니다.
이 위저드를 이용하여 Service 설정 복잡도를 낮출 수 있습니다.

- 그림 42. Stack 편집기의 위저드
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step420.jpg)

## 요약

OCCS의 주요 개념과 용어를 살펴보고, OCCS를 기본 작동법
(서비스 조회와 배포, 스택 조회 및 배포, 스테일 아웃, 컨테이너 로그 조회 등)을 살펴보았습니다.
OCCS는 Docker를 Production으로 사용할 때 발생하는 문제에 대한 해결 방안을 제공합니다.
OCCS를 사용하면 Docker의 기술적인 문제를 최소화하고 운영 안정성을 높일 수 있습니다.
추가로 OCCS에 Oracle Developer CS 혹은 CI/CD 기법을 결합할 경우 DevOps의 유기적인 환경을 구성할 수 있습니다.

## 참고자료
- [Using Oracle Container Cloud Service](http://docs.oracle.com/en/cloud/iaas/container-cloud/contu/index.html) - 오라클 공식 메뉴얼
- [Running a Docker Container with Oracle Container Cloud Service in Three Easy Steps](https://apexapps.oracle.com/pls/apex/f?p=44785:112:0::::P112_CONTENT_ID:19220) - Oracle Learning Library

## 관련문서
- [오라클 클라우드 계정 생성하기](/post/accont/)
- [Oracle Container CS 생성 절차](/post/occs-new-inst/)
- [오라클 클라우드의 Docker 컨테이너 지원 유형](/post/docker_in_oc/)
- [Oracle Container Cloud Service 소개와 Docker 개요](/post/occs/)
- [Oracle Container CS 생성 절차](/post/occs-new-inst/)
