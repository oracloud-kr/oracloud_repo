+++
author = "taewan.kim"
categories = ["container"]
thumbnailInPost = "https://oracloud-kr-teamrepo.github.io/2017/03/docker_in_oc/image_in_post.jpg"
title = "오라클 클라우드의 Docker 컨테이너 지원 유형"
date = "2017-03-20T19:01:16+09:00"
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2017/03/docker_in_oc/oracledocker.jpg"
description = "오라클 클라우드에서 docker 컨테이너를 지원하는 3가지 방식을 설명합니다. 오라클 클라우드는 DIY(형태로 오라클 클라우드 VM에 더커를 직접 설치하고 관리하는 방식), 관리 서비스 형태(Oracle Container Cloud Service, Docker 엔진과 orchestration Tool을 제공하는 방식), PaaS 타입(Oracle Application Container Cloud Service, 코드를 직접 배포하는 방식) 3가지를 제공합니다."
language = ""
tags = ["docker", "occs", "acc", "container"]
+++

오라클 클라우드에서 docker를 사용하는 3가지 방법이 있습니다. 첫 번째는 Oracle Compute에 직접 Docker와 Orchestration tool([Kubernetes](https://kubernetes.io/), [docker-swarm](https://docs.docker.com/swarm/overview/), [marathon](https://mesosphere.github.io/marathon/))을 직접 Oracle Cloud의 VM에 설치해서 사용하는 것입니다. 두 번째는 Oracle Cloud에서 IaaS 형태로 제공하는 Oracle Container Cloud Serivce (이하 OCCS)를 사용하는 것입니다. 세 번째는 Oracle Cloud에서 제공하는 PaaS 서비스 ```Oracle Application Container  Cloud(이하 Oracle ACC)```를 사용하는 것입니다.

Oracle Cloud에서 Docker를 사용하는 3가지 방식에 대하여 다음과 같이 분류하고, 유형별로 살펴보겠습니다.

- DIY 방식: Oracle Compute CS에 직접 설치
- OCCS: 오라클이 IaaS 서비스 형태로 제공하는 Container Service
- Oracle ACC: 오라클이 PaaS 형태로 제공하는 Container Service  

## Type 1. DIY (Do It Yourself) 방식

Oracle Compute CS에 VM을 생성한 후, 그 위에 Docker를 직접 설치하고 사용하는 방식입니다. 하나의 VM을 호스트로 Docker를 설치하고 사용하는 방식입니다. 테스트 및 개발 환경이라면 추가적인 고려사항 없이 Docker만 설치해도 충분합니다.

Production 환경을 DIY 방식으로 구성하려면 다음과 같은 사항을 고려해야 합니다.

- 고가용성을 위하여 복수의 Host를 클러스터로 구성
- 고가용성 확보를 위한 주기적 컨테이너 Health-check 및 장애가 발생하면 컨테이너 이동
- 컨테이너 이동에 대한 네트워크 구성 관리
- 클러스터 자원을 고려한 배포 전략
- 스케일 아웃 지원
- 모니터링
- 효율적인 오케스트레이션
- 애자일 툴과 효과적인 통합을 위한 Webhook 제공

Container orchestration 도구를 Production 환경에 적용하면 위와 같은 이슈들을 해결할 수 있습니다. 대표적인 Container orchestration 도구는 다음과 같습니다.

- [Kubernetes](https://kubernetes.io/)
  - 단축 표기: K8S
  - 컨테이너의 배포, 스케일 아웃 관리를 담당
  - 오픈소스 프로젝트
  - Google이 개발한 프로젝트로 현재는 Cloud Native Computing Foundation에 기증된 상태
  - 복수의 클러스터 위에서 클러스터 구성
  - 가장 대중적으로 많이 사용되는 Container orchestration 도구
- [docker-swarm](https://docs.docker.com/swarm/overview/)
  - Swarm은 Docker와 별도로 개발되었지만 도커 1.12 버전부터 Swarm이 Docker에 포함됨
  - 별도의 소프트웨어 설치 필요 없이 사용 가능
  - [1,000개 노드에 50,000개 컨테이너 테스트: 안정성 확보](https://blog.docker.com/2015/11/scale-testing-docker-swarm-30000-containers/)
- [marathon](https://mesosphere.github.io/marathon/)
  - 아파치 Mesos와 DC/OS(Data Center Operating System)을 위한 컨테이너 오케스트레이션 플랫폼

Oracle Compute Cloud Serivce의 VM은 Docker와 Container orchestration 도구 설치에 대한 제약이 없습니다.

***

## Type 2. Oracle Container Cloud Serivce (OCCS)

Oracle Cloud는 Docker 기반의 컨테이너 서비스를 2016년 11월에 출시하였습니다. 오라클은 2015년 11월에 StackEngine을 인수하였습니다. StackEngine사는 2014년 텍사스 오스틴 주에서 설립된 업체로, 엔터프라이즈급 컨테이너 관리 및 자동화 솔루션 개발 업체입니다. StackEngine은 Kubenetes와 Docker-storm과 유사한 Docker 클러스터 운영 및 관리 소프트웨어를 개발해 왔습니다.

오라클은 2015년 11월에 StackEngine을 인수한 후, 1년 동안 Oracle Public Cloud에 StackEngine의 소프트웨어를 통합하였습니다. 이 결과물이 Oracle Container Cloud Service입니다.

OCCS를 이용하여 Docker 운영 환경 관리 효율성을 극대화하고 Automation을 강화할 수 있습니다. 추가로 오라클 클라우드가 제공하는 Developer CS를 OCCS과 함께 사용할 경우 효율적인 DevOps 환경을 구성할 수 있습니다. Developer CS는 오라클 클라우드가 제공하는 개발 환경 서비스입니다. OCCS 사용 시 Oracle Developer CS는 무상으로 제공됩니다. Developer CS는 Git, 이슈트래커, Jenkins, Wiki가 결합된 애자일 개발 환경을 클라우드로 제공하는 서비스입니다. Developer CS는 별도의 블로그 포스트 시리즈로 다루겠습니다.

OCCS의 주요 기능은 다음과 같습니다.

- 호스트와 클러스터링 관리
- 복수의 호스트에 걸쳐 컨테이너 스케일링 관리 기능
- Private Docker Registry 등록 및 연결
- Service Registry
- 컨테이너 환경을 관리하는 대시보드와 모니터링 기능

OCCS의 아키텍처는 다음과 같습니다.

![](https://oracloud-kr-teamrepo.github.io/2017/03/occs_new_inst/occs_arhi.jpg)

OCCS 인스턴스는 1개의 Manager 노드와 1개 이상의 Worker 노드로 구성됩니다. 최소 구성은 노드 2개(Manager 1개, Worker 1개)입니다. Manager 노드는 OCCS 콘솔 UI(웹 애플리케이션)를 제공합니다. 관리자는 브라우저로 OCCS 관리 콘솔에 접근할 수 있습니다. 또한 SSH로 Manager Node에 접근 가능합니다. OCCS 콘솔 UI에 접근 시 로그인 절차가 필요합니다. 로그인 ID/Password는 OCCS 인스턴스를 생성할 때 설정됩니다. OCCS 콘솔 UI는 다음과 같은 형태입니다.  

![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step080.jpg)

### Type 2-1. OCCS: Services

OCCS는 Docker 이미지를 Service를 형태로 관리 합니다. Service는 Docker 이미지와 OCCS의 관리 설정( YAML)으로 구성된 고수준 관리 객체입니다. OCCS는 기본적으로 apache, nginx, jenkins, logstash, mariadb, haproxy를 23개의 서비스를 제공합니다. 이 서비스를 배포하여 컨테이너를 만들 수 있고, 기본 서비스를 참조하여 새로운 서비스를 만들고 등록할 수 있습니다.

![](https://oracloud-kr-teamrepo.github.io/2017/03/docker_in_oc/service.jpg)

Service의 설정을 위해서 아래와 같이 전용 UI 빌더, ```Docker run``` 설정, ```YAML``` 설정을 제공합니다. 이 3가지 설정 방법은 통합되어 있으므로, 빌더에서 수정하면 ```Doker run```이나 YAML에도 동일하게 적용됩니다. 실행 옵션뿐만 아니라 아래 설정 창에서는 배포 정책과 배포 서버 선정 기준도 설정 가능합니다.

![](https://oracloud-kr-teamrepo.github.io/2017/03/docker_in_oc/editor.jpg)

### Type 2-2. OCCS: Stack

Stack은 위에서 설명한 Service의 조합입니다. OCCS는 스택을 정의할 때 Docker compose 설정 포맷을 사용합니다. OCCS 콘솔 UI는 다음 그림과 같이 스택 설정을 위한 GUI를 제공합니다.  

![](https://oracloud-kr-teamrepo.github.io/2017/03/docker_in_oc/stack_editor.jpg)

### Type 2-3. OCCS: Deployment

OCCS에서는 Service 혹은 Stack을 실행하여 컨테이너로 만드는 것을 배포하고 합니다. Service와 Stack을 배포할 때 아래와 같이 컨테이너 수량, 배포 정책, 배포 호스트 선정 기준을 설정할 수 있습니다.  

![](https://oracloud-kr-teamrepo.github.io/2017/03/docker_in_oc/deployment.jpg)

OCCS에서는 배포 과정을 UI로 확인할 수 있습니다.

![](https://oracloud-kr-teamrepo.github.io/2017/03/docker_in_oc/deployment2.jpg)

배포가 완료되면 구동 중인 컨테이너의 상제 정보도 다음 UI를 통해서 확인할 수 있습니다.
추가로 YAML 형태로 컨테이너의 실행 옵션도 확인할 수 있으며, Scale-out이 필요할 경우 컨테이너 수량을 확장하는 메뉴(Change scaling)를 수행할 수 있습니다.

![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step140.jpg)

### Type 2-4. OCCS: Resource Pool

호스트 서버를 상호 독립적 그룹으로 구성하는 단위입니다. 각 Resource Pool은 하나 이상의 호스트로 구성되면, 더커 환경을 관리하고, 복수의 호스트에 서비스와 스택을 배포하게 됩니다. 하나의 호스트는 하나의 Resource Pool에 포함되고, 하나의 Resource Pool은 여러 호스트를 포함합니다.

호스트의 구성은 다음 UI를 통해서 관리 할 수 있습니다.

![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step220.jpg)

### Type 2-5. OCCS: Monitoring

OCCS에 배포된 모든 컨테이너는 웹 UI를 통해서 로그와 상태를 확인할 수 있습니다. 또한, 주기적으로 상태가 검사되어 컨테이너 상태에 이상 증상이 대시보드에 출력됩니다.

![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step390.jpg)

### Type 2-6. OCCS 요약

OCCS는 오라클 클라우드의 더커 컨테이너 IaaS 서비스입니다. Docker 컨테이너 환경의 모든 것 (이미지 설정부터 컨테이너 구성 및 모니터링, 자동화)을 지원하는 서비스입니다. OCCS과 Oracle Developer CS를 결합할 경우 완성도 높은 Docker Orchestration 환경과 DepOps의 환경을 구성할 수 있습니다.

***

## Type 3. Oracle Application Container Cloud(Oralce ACC)

최근에는 Cloud Native, MSA(Micro Service Architecture)와 같은 개념이 많은 관심을 받고 있습니다. 경량형 서버에 다양한 언어(Polyglot)로 개발된 애플리케이션을 신속히 배포하고 활용할 수 있는 클라우드 인프라에 대한 요구가 커지고 있습니다.

Oracle ACC는 다양한 개발언어(Java SE, node.js, PHP, Python)로 개발된 애플리케이션을 빠르고 편리하게 사용할 수 있는 개발/테스트/운영을 제공하는 클라우드 서비스가 있습니다. 소스코드(zip 파일)를 올리면 바로 public 하게 웹 서비스를 할 수 있습니다. 애플리케이션 요청이 증가하면 필요한 서버 용량에 맞게 손쉽게 용량을 증가시킬 수 있습니다.

![](https://oracloud-kr-teamrepo.github.io/2017/03/docker_in_oc/accs_architecture.jpg)

Oracle Application Container Cloud는 Oracle Java SE, Node.js, PHP, Python 애플리케이션을 안전하고 신뢰할 수 있는 클라우드 서비스에 배포할 수 있게 해줍니다. 유연한 확장, 스토리지, 통합 로드 밸런싱 등의 기능으로 Oracle Application Container Cloud에서 Java, Node, PHP, Python 워크로드를 실행하고, 애플리케이션의 확장이 필요할 때 Oracle Compute Cloud Service에서 제공하는 동일한 핵심 능력을 활용하여 환경을 수직/수평으로 확장할 수 있습니다.

Oracle ACC 주요 특징

- 사전에 설정된 Java, Node.js, PHP, Python을 위한 환경 제공 (Docker)
- Java SE는 Java Flight Recorder[^1], Java Mission Control[^2], 고급 메모리 관리 기능, 주지적 패치 제공
- 주요 프레임워크(Spring, play, jersey)과 컨테이너(Tomcat) 지원  
- JVM 기반 언어 지원:  JRuby
- 엔터프라이즈급 지원
- 웹 기반 UI와 REST API 제공
- 다른 Oracle Cloud와 통합성
  - Oracle Storage Cloud Service
  - Oracle Database Cloud Service
  - MySQL Cloud Service
  - 등등등...
- Oracle Developer Cloud와 유기적 개발 환경 구성
- 지원 언어
  - Java SE
  - Node.js
  - PHP
  - Python
  - 자바 기반 언어: JRuby

[^1]:Java Flight Recorder(JFR): 동작하는 JVM과 애플리케이션을 상세 기록함. 이 기록 데이터는 execution profile, GC 통계, optimization decisions, 객체 할당, heap 통계, Lock과 I/O 지연 이벤트를 포함합니다. 이 정보는 Java Mission Control에서 분석 가능합니다. (상제 정보는 다음 링크를 참조: [JFR 사용법](https://blogs.oracle.com/javakr/entry/java_flight_recorder_jfr_%EC%82%AC%EC%9A%A9))

[^2]:Java Mission Control: Java Mission Control 이란 오라클에서 제공되는 Java Advanced 제품의 일부로, 엔터프라이즈의 자바 애플리케이션의 상태를 실시간으로 모니터링 할 수 있는 도구다. (상제 정보는 다음 링크를 참조: [Java Mission Control 사용법](https://blogs.oracle.com/javakr/entry/java_mission_control_%EC%82%AC%EC%9A%A9%EB%B2%95))

Oracle ACC에 애플리케이션을 클러스터로 배포할 수 있습니다. Oracle ACC의 클러스터 고려 사항은 다음과 같습니다.

- 성능: 하나의 애플리케이션을 하나 이상의 인스턴스로 구성하여 응답 시간을 단축
- 스케일: 클러스터 애플리케이션에 인스턴스 추가 가능
- 부하분산(Load Balancing) - 부하를 로드밸런서가 여러 인스턴스에 분산. 내부적으로 Oracle Traffic Director 사용
- 고가용성(High Availability) - 애플리케이션 클러스터링과 부하 분산을 통해서 단일 인스턴스 장애를 클라이언트가 인식하지 못하게 함

### Type 3-1. Oracle ACC 요약

Oracle ACC는 Docker의 운영보다는 Cloud Native, Polyglot, MSA(Micro Service Architecture)과 같이 애플리케이션 배포와 부하 분산 등 애플리케이션 개발에 집중할 수 있는 환경을 제공합니다. 소스코드로부터 직접 인스턴스를 생성하고 클러스터를 구성하는 방식으로 운영되기 때문에 효율적인 자원 활용과 저렴한 인프라 운영환경, 관리비용 최적화, 애자일 개발환경에 적합한 서비스입니다.

***

## Summary: 오라클 클라우드의 Docker 지원

오라클 클라우드에서 2017년 04월 현재 3가지 방식으로 Docker 컨테이너 기술을 사용할 수 있습니다.

첫 번째는 DIY 방식으로 Oracle Computing Service에 직접 호스트 인스턴스를 만들고 Docker를 설치하여 사용하는 방식입니다. 사용자가 선호하는 Docker 버전 및 Docker Orchestration 도구를 기호에 맞게 취사선택을 할 수 있다는 장점과 함께 관리 포인트가 많아진다는 단점이 있습니다. 기존에 Docker 환경을 운영하고 친숙한 기술 셋을 가진 경우에 적합한 적용 방식입니다.  

두 번째는 Docker Container의 전반적인 관리 체계를 포함하는 Oracle Container Cloud Service를 사용하는 것입니다. 이 방식은 Docker를 신규 도입하거나 기존에 Docker를 사용했지만, 더 체계적인 관리 환경을 고민하시는 분들에게 적합한 방식입니다. OCCS는 Docker를 운영환경으로 사용할 경우 발생하는 이미지 관리 체계, 애플리케이션 구성 및 디자인 방식, 고가용성을 제공하기 위한 체계, 컨테이너 모니터링의 전체적인 기능을 체계적으로 제공합니다. 체계적인 Docker 컨테이너 환경을 고민하시는 분들에게 적합한 방식입니다.

세 번째 Oracle Application Container Cloud는 Docker보다는 애플리케이션 개발과 배포에 중점을 두시는 분들에게 적합한 방식입니다. 소스코드로부터 오라클이 제공하는 컨테이너는 생성하고 관리하는 방식입니다. 오라클이 설계한 애플리케이션 클러스터와 스케일 관리 방식을 사용하여 효율적인 애플리케이션 운영 환경을 확보할 수 있습니다. Cloud Native로 전환 혹은 적용을 고려하시는 분들에게 적합한 방식입니다.

## 참고 자료
- http://docs.oracle.com/cloud/latest/stcomputecs/STCSG/toc.htm
- http://docs.oracle.com/en/cloud/iaas/container-cloud/contu/index.html
- http://docs.oracle.com/en/cloud/paas/app-container-cloud/dvcjv/index.html
- [Oracle Container Cloud Service — Overview and First Impressions](https://medium.com/enpit-developer-blog/oracle-container-cloud-service-overview-and-first-impressions-b42bd2ac308)
- [munz&more: Oracle Container Cloud Service (OCCS)](http://www.munzandmore.com/2016/ora/oracle-container-cloud-service-occs)
- [다양한(Polyglot) 개발 언어를 위한 오라클 애플리케이션 컨테이너 클라우드 서비스](https://mmjazz.wordpress.com/2017/03/20/%EB%8B%A4%EC%96%91%ED%95%9C-%EA%B0%9C%EB%B0%9C-%EC%96%B8%EC%96%B4polyglot-programming-language%EB%A5%BC-%EC%9C%84%ED%95%9C-oracle-application-container-cloud-service/amp/)
