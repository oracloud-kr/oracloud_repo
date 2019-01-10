+++
date = "2017-12-25T21:28:14+09:00"
description = "blogs.oracle.com 블러그의 2017년 Kubernetes와 Serverless 마지막 포스팅 번역입니다. 오라클의 2018년 방향을 감지할 수 있습니다."
title = "[번역]2017 KubeCon 오라클 발표: Kubernetes와 Serverless"
thumbnailInList = "https://taewanmerepo.github.io/2017/12/transplation_kubecon/list.jpg"
thumbnailInPost = "https://taewanmerepo.github.io/2017/12/transplation_kubecon/post.jpg"
tags = ["ORACLE", "Oracle Cloud", "OCI", "kubernetes", "serverless", "오라클 쿠버네티스", "오라클 클라우드"]
categories = ["Oracle-Cloud"]
author = "taewan.kim"
language = ""  
jupyter = "false"
+++

본문은 blogs.oracle.com에서 운영중인 Oracle Developer 블로그의 12월 7일자 포스트를 번역한 문서입니다. 오라클은 최근 KubeCon에서 Serverless 배포와 멀티 클라우드를 관리하는 Kubernetes 툴 2 가지를 공개하였습니다. 이와 관련한 오라클 공식 블로그의 포스트를 번역합니다. 원문 정보는 다음과 같습니다.

> - 출처: 오라클 공식 블로그, blogs.oracle.com
- 원문: https://blogs.oracle.com/developers/kubernetes-serverless-and-federation-oracle-at-kubecon-2017
- 제목: Kubernetes and Serverless: Oracle at KubeCon 2017
- 문서작성 일시: 2017. 12. 7
- 작성자: Bob Quillin, Developer Relations 부사장

----

# Kubernetes and Serverless: Oracle at KubeCon 2017

오늘 텍사스주 오스틴에서 개최된 __KubeCon + CloudNativeCon 2017__ 컨퍼런스에서, 오라클 컨테이너 네이티브 애플리케이션 개발팀은 새로운 Kubernetes 관련 프로젝트 2개를 오픈소스로 공개했고, 현장에서 데모를 진행하였습니다. 이번에 공개한 첫 번째 오픈소스 프로젝트는 Kubernetes 용 "__Fn Installer__"입니다. Fn은 2017년 10월 오라클 오픈 월드에서 공개한 오픈소스 서버리스 프로젝트입니다. 이번에 공개한 Fn용 헬름차트(Helm Charts)[^1]를 사용하면, 오라클 관리형 Kubernetes 서비스인 Oracle Container Engine(OCE)[^2]을 포함한 모든 Kubernetes 배포에서 Fn을 쉽게 설치하고 실행할 수 있습니다.

[^1]: [역자주]: Helm은 Kubernetes 용 패키지 관리 툴입니다. Kubernetes에 배포될 패지키를 설치하고 제거하는 역할을 담당합니다. Ubuntu의 apt-get, CentOS의 yum과 같은 역할을 담당합니다. Helm이 대상 패키지를 관리하는 단위가 Helm Charts 입니다.

[^2]: [역자주]: Oracle Container Engine는 Oracle Cloud infrastructure(OCI) 위에서 동작하는 Docker Container 서비스입니다. 서비스 URL: https://cloud.oracle.com/ko_KR/containers

두 번째로 오라클이 공개한 오픈소스 프로젝트는 [Global Multi-Cluster Management](https://github.com/oracle/navarkos) 입니다. 이 프로젝트는 고도로 분산된 행성급 규모(planet-scale)의 애플리케이션을 지능적으로 관리하기 위해서 분산 클러스터 관리 기능을 Kubernetes federation[^3]에 제공하는 프로젝트입니다. Kubernetes federation을 통해서 복수 리전, 하이브리드 심지어 복수 클라우드에 분산된 Kubernetes 클러스터를 연합하여 운영할 수 있습니다. 이렇게 연합된(Federated) 세상에서는 많은 운영상의 문제가 발생할 수 있습니다. 글로벌 애플리케이션을 관리하고 자동 확장하는 방법 그리고 필요에 따라서 임시 클러스터(Spot Cluster)를 생성하고 애플리케이션을 배치하는 방안을 상상해 보시기 바랍니다. 이것을 주제로 2017년 12월 7일 15시 50분에 "[Multi-Cluster Ops in a Hybrid World](https://kccncna17.sched.com/event/CU7i)" 세션이 진행될 예정입니다. 이 세션의 발표는 Kire Filipovski 와 Vitaliy Zinchenko이 담당합니다.


[^3]: [역자주]Kubernetes federation은 다른 지역에 위치한 여러 Kubernetes 클러스터에 애플리케이션을 배포하고 관리하는 Kubernetes의 핵심 기능입니다. 참고 URL: https://kubernetes.io/docs/concepts/cluster-administration/federation/

## 전망: 개방, 통합 그리고 엔터프라이즈-급 경향 유지

고객은 특정 클라우드에 종속됨 없이 기존에 로컬에서 사용하는 기술 스텍을 그대로 클라우드에서 사용하기를 원합니다. 고객은 이러한 요구사항이 반영된 개방적이고, 클라우드 중립적이면서 커뮤니티가 주도하는 컨테이너-네이티브(Container-native) 기술 스텍을 찾고 있습니다. 이것이 바로 2017년 10월 오라클 오픈월드에서 "[Container Native Application Development Platform](https://blogs.oracle.com/developers/meet-the-new-application-development-stack-kubernetes-serverless-registry-cicd-java)"을 출시할 때 밝힌 오라클의 비전이었습니다.

그 후, [2017년 11월에 공개된](https://www.cncf.io/announcement/2017/11/13/cloud-native-computing-foundation-launches-certified-kubernetes-program-32-conformant-distributions-platforms/) Oracle Container Engine(OCE)은 "[__Certified Kubernetes__](https://github.com/cncf/k8s-conformance)"를 획득한 첫 번째 서비스입니다. 개발자와 개발팀은  "__Certified Kubernetes__"을 획득한 OCE를 사용함으로써 프로덕션과 개발 구현체 사이에 일관성과 이식성을 확신할 수 있습니다.

현재 커뮤니티는 서비리스 기술(Serverless Technology) 선정에서도 개방적이면서 그들의 클라우드 네이티브 스텍과 일치하는 일관된 방식으로 구축됨을 보장하는 방법을 찾고 있습니다. 다시 말해서 개방적이면서 Kubernetes 위에서 동작하는 Serverless 기술을 찾고 있습니다. 오픈소스 기반의 솔루션이 클라우드 종속(Cloud lock-in)의 해법일 수 있습니다. 핵심은 DevOps 팀이 클러스터가 여러 클라우드에 걸쳐있거나 혹은 하이브리드 모드로 운영되는 사황에서도 쉽게 관리할 수 있어야 합니다. 이는 고객, 개발팀 및 기업으로부터 들려오는 세 가지 주요 "요구 사항"과 일치합니다. 따라서 컨테이너 네이티브 플랫폼은 개방적이고 통합형이며 엔터프라이즈급이어야 합니다.

### 개방: 개방 위에 개방

Fn 프로젝트와 Global Multi-Cluster Management는 클라우드 중립적인 오픈소스입니다. 프로젝트의 개방성이 강화되었습니다. 이제 Fn Helm Chart를 통해서 선도적인 오픈 컨테이너 오케스트레이션 플렛폼인 Kubernetes 위에서 오픈 서버리스 프로젝트(Fn)를 실행할 수 있게 되었습니다. Helm 패키지 관리자(helm.sh)을 사용하여 Kubernetes 클러스터에 [Fn](https://github.com/fnproject/fn)을 배포할 수 있습니다. 이렇게 배포된 Fn은 기능적인 제약이 없습니다.

### 통합: 일관성과 연결

통합 플랫폼에 대한 보장을 제공하기 위해서, Fn Installer Helm Charts와 Global Multi-Cluster Management를 Kubernetes 위에서 동작하도록 빌드하였습니다. 이 두 프로젝트는 오라클의 컨테이너 네이티브 플랫폼에 완전히 통합되어 있습니다. 홈 데포와 코스트코[^4]에서 모든 것을 할 수는 있지만, 종합적인 애플리케이션 개발자 경험을 만들 수는 없습니다. 특히 수백 클러스터 규모의 경험은 불가능합니다. 이는 수천 명의 개발 조직을 갖춘 상황에서도 어렵습니다. Fn Installer와 Global Multi-Cluster Management는 오라클의 관리형 Kubernetes 서비스인 OCE 위에서 이용 가능합니다.

[^4]:[역자주]대형 쇼핑몰(Home Depo, Costco) 에서 무엇이든 구입할 수 있지만 경험많은 DevOps 인력을 살 수는 없다는 비유적인 표현입니다. Home Depo(홈 데포): 미국의 대표적인 가정용 건축자재 제조 및 판매업체. Costco: 미국의 대표적인 대형 할인 마트.

### 엔터프라이즈급 : HA, 보안 및 운영 인식

Oracle Container Engine 같은 엔터프라이즈급 Kubernetes 서비스에 Fn을 배포할 수 있으므로 고 가용성의 안전한 백엔드 플랫폼에 서버리스(Serverless)를 배포하고 실행할 수 있게 되었습니다. 또한 Global Multi-Cluster Management는 엔터프라이즈 플랫폼을 여러 클러스터 및 클라우드로 확장하고 더 나은 활용 및 용량 관리를 원하는 기업의 요구를 만족시키는 기술입니다.

대형 분산 시스템의 프로덕션 운영은 단일 클라우드와 온-프라미스에서 조차도 상당히 어렵습니다. 더군다나 복수 리전, 하이브리드(클라우드+온-프라미스) 혹은 복수 클라우드에 걸쳐 여러 클러스터를 사용하는 연합 배포(federated deployments)는 더 복잡합니다. 이런 상황에서, DevOps 팀은 필요한 시점에 애플리케이션 혹은 임시 클러스터를 배포하고 자동 확장할 수 있어야 합니다. 그리고 클라우드 이전과 하이브리드 시나리오를 실행할 수 있어야 합니다.

![](https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/66fa09c5-4231-4306-b7e8-071282e381e2/Image/d2f4f40a79477f19e27594fa6eae3826/fn_300x125.png)

## 거대한 파워와 함께 찾아온 거대한 책임감(그리고 복잡성)

Kubernetes federation은 엄청난 기능과 가능성을 제공함과 동시에 커다란 책임과 새로운 복잡성을 발생시켰습니다. 컨테이너 네이티브 배포에 애플리케이션 인지 결정 로직[^5]을 적용하는 과제가 대표적인 예입니다. 험난한 비즈니스 및 운영 문제에는 비용, 지역적 유사성, 성능, 서비스 품질 및 준수가 포함될 수 있습니다. 여러 Kubernetes 클러스터 배포를 관리할 경우, 온-프라미스 및 퍼블릭 클라우드 환경을 혼합하여 배포하는 상황이 발생할 수 있습니다. 이 때 DevOps팀은 여러 클러스터 프로파일 때문에 어려움을 겪을 수 있습니다.

[^5]:[역자주]애플리케이션 인지 결정 로직이란 애플리케이션에 대한 현재 부하와 애플리케이션 특성을 인지하여 어디에 배포할지를 결정하는 로직을 의미합니다. 배포 자동화와 자동 확장을 지능적으로 처리하는 방식입니다.

다음은 기본적이지만 답변하기는 까다로운 DevOps 관련 질문입니다.  

- 얼마나 많은 클러스터를 운영해야 합니까?
  - 각 환경에 대해서 개별적인 클러스터가 필요합니까?
  - 각 클러스터에 얼마나 많은 용량을 할당해야 하는가?
- 누가 클러스터의 생명주기(Lifecycle)을 관리하나요?
- 어떤 클라우드가 내 애플리케이션에 가장 적합합니까?
- 어떻게 클라우드 종속을 피할 수 있나요?
- 애플리케이션을 여러 클러스터에 어떻게 배치합니까?

"__Global Multi-Cluster Management__"는 세 개의 오픈 소스 컴포넌트(Navarkos, Cluster Manager, Federated Ingress Controller)로 구성됩니다. Navarkos는 그리스어로 "제독(Admiral)"을 의미합니다. Kubernetes 연합 배포는 Navarkos를 이용하여 복수 클러스터 인프라스트럭처를 자동으로 관리할 수 있습니다. 그리고 연합된 Kubernetes 애플리케이션 배포의 반응을 기준으로 클러스터를 관리할 수 있습니다. Cluster Manager는 Kubernetes federation 백엔드를 이용하여 Kubernetes 클러스터의 생명주기(Lifecycle)를 관리합니다. 마지막으로 Federated Ingress Controller는 외부 DNS를 사용하는 Ingress를 대체합니다.

Global Multi-Cluster Management는 Kubernetes federation과 함께 다음과 같은 여러 가지 문제를 해결합니다.

- 필요한 시점에 Kubernetes 클러스터 생성 및 클러스터에 앱 배포
  - 클러스터는 퍼블릭 클라우드와 프라이빗 클라우드에서 모두 실행 가능
  - 수요와 공급에 맞춰 애플리케이션 실행
- 클러스터 일관성과 클러스터 생명주기(life-cycle) 관리
  - Ingress, nodes, network
- 다중 클라우드 응용 프로그램 배포 제어
  - 클라우드 제공 업체에 독립적으로 애플리케이션 제어
- 애플리케이션 인지 클러스터(Application-aware clusters)
  - 클러스터는 idle 시점에 오프라인 전환
  - 워크로드에 따른 자동 확장(auto-scaled) 기능 자동화
  - 비용, 지역적 유사성, 성능, 서비스 품질 및 준수를 포함한 여러 요소를 근간으로 앱을 실행하는 위치를 결정하는 기반 제공

"__Global Multi-Cluster Management__"는 애플리케이션 배포를 기준으로 필요한 시점에 Kubernets 클러스터를 생성하고 규모를 조절하고, 제거합니다. 모든 클러스터 관리는 애플리케이션 배포 요청을 기준으로 관리됩니다. 배포된 애플리케이션이 없는 상황에서는 Kubernetes 클러스터는 어디에도 존재하지 않습니다.   


DevOps 팀이 연합 Kubernetes 환경에 여러 유형의 애플리케이션을 배포할 때, Global Multi-Cluster Management는 어떤 클러스터를 만들어야 하는지, 얼마나 많이 만들지, 어디에 배치해야 하는지 지능적으로 결정합니다. 운영 중인 클러스터는 애플리케이션의 현재 시점의 요청을 기준으로 조절됩니다. Kubernetes 인프라스트럭처는 더 많은 애플리케이션을 운영하고 운영 상황을 인식하게 됩니다.   

## G8 부스 방문에 주세요, 오라클 세션에 참여해 주세요(KubeCon + CloudNativeCon 2017)

G8 부스에 오라클 엔지니어와 기여자(Contributor)들 만날 수 있습니다. 오스틴 네이티브[^6]도 격하게 환영합니다.

[^6]:[역자주]오스틴에는 StackEngine 팀이 위치합니다. 오라클은 현재 2 개의 Docker 기술을 지원합니다. 2016년에 오라클은 StackEngine을 인수하여 Oracle Container Cloud Service를 오라클 클라우드에서 서비스 중입니다.

세션 정보는 다음과 같습니다.

- [Panel: Kubernetes, Cloud Native and the Public Cloud [B] - Moderated by Dan Kohn, Cloud Native Computing Foundation](https://kccncna17.sched.com/event/CU7S/panel-kubernetes-cloud-native-and-the-public-cloud-b-moderated-by-dan-kohn-cloud-native-computing-foundation)
  - 발표자: Join Jon Mittelhauser(오라클 컨테이너 네이티브 플랫폼 팀 부사장) 외 5명
  - 일시: 2017년 12월 6일 11:10 – 11:45
  - 장소: Ballroom A, Level 1
- __[Multi-Cluster Ops in a Hybrid World deep dive](https://kccncna17.sched.com/event/CU7i)__ 세션
  - 발표자: Kire Filipovski(오라클 클라우드 아키텍트), Vitaliy Zinchenko(오라클 클라우드 아키텍트),
  - 일시: 2017년 12월 7일 15:50 – 16:25
  - 장소: Room 8ABC, Level 3.
- __[Lightning Talk: Moving Fast with Microservices: Building and Deploying Containerized Applications in a Cloud-Native World ](http://sched.co/CU7g)__
  - 발표자: Micha Hernandez van Leuffen (오라클 소프트웨어 개발 부사장, Wercker CEO)
  - 일시: 2017년 12월 5일 18:00 - 18:55
  - 장소: Ballroom A, Level 1
- __[Running MySQL on Kubernetes](http://sched.co/CU80)__
  - 발표자: Patrick Galbraith(오라클 플랫폼 엔지니어)
  - 일시: 2017년 12월 7일 15:50 – 16:25
  - 장소: Room 9C, Level 3
