+++
description = "오라클 클라우드의 IaaS에는 Oracle Cloud infrastructure(OCI)와 Oracle Cloud infrastructure classic(OCI-Classic) 두 가지가 있습니다. OCI와 OCI-classic에 대한 차별점을 소개합니다."
title = "Oracle Cloud IaaS: OCI vs OCI Classic"
categories = ["oracle"]
thumbnailInPost = "https://taewanmerepo.github.io/2018/04/ocivsociclassic/head.jpg"
thumbnailInList = "https://taewanmerepo.github.io/2018/04/ocivsociclassic/head2.png"
date = "2018-05-03T15:59:47+09:00"
language = ""
tags = ["oracle", "oci", "oci-classic", "C@C", "iaas","김태완"]
author = "taewan.kim"
adsense = "true"
+++

오라클 클라우드는 IaaS로 OCI(Oracle Cloud Infrastructure)와 OCI Classic(Oracle Cloud Infrastructure Classic) 서비스를 제공합니다. 오라클 클라우드를 처음 접할 때, 왜 두 개의 IaaS가 존재하는지 그리고 다른점이 뭔지 혼란스러운 것이 사실입니다. 이와 관련하여 OCI Classic과 OCI란 무엇이고 어떻게 다른지 간략하게 정리해 보겠습니다.

{{% notice note %}}
이 문서에서 다루는 내용은 작성자의 개인적인 의견이며, 오라클의 공식적인 입장과 다를 수 있습니다.
{{% /notice %}}

## 오라클 클라우드 IaaS 서비스

오라클은 2013년 3월에 Nimbula를 인수했습니다. Nimbula는 Private과 Hybrid 클라우드 인프라 관리 기술로 유명한 회사였습니다. 오라클은 Nimbula 기술을 근간으로 클라우드 IaaS 서비스를 개발하였고, 2014년 OOW(Oracle Open World)에서 OPC(Oracle Public Cloud)라는 브랜드로 클라우드 서비스를 런칭하였습니다.

오라클은 2014년부터 OPC로 IaaS 서비스를 하였습니다. 그와 동시에 Region, Availability Domain, Flat Network, 서버 및 랙 디자인을 클라우드 환경에 맞춘 클라우드 전용 데이터 센터 구축을 진행하였습니다. 이 클라우드 데이터 센터 구축 프로젝트를 2세대란 의미(Generation 2)에서 __Gen2__라고 불렀고, Gen2에 올라간 IaaS 서비스를 BMCS(Bare Metal Cloud Service)라고 명명했습니다. 2017년 OOW에 맞춰서, 오라클은 오라클 클라우드 주요 서비스의 브랜드명을 조정하였습니다. 이 과정에서 BMCS는 OCI로 변경되었고, OPC는 OCI Classic으로 서비스 명이 바뀌었습니다.

오라클 클라우드 서비스 중에서 이름이 __classic__ 으로 끝나는 서비스는 Nimbula를 기반으로 개발된 IaaS 서비스라고 분류할 수 있습니다. 예를 들어서 Storage 서비스는 Gen2 기반의 IaaS 기술이고, Storage Classic은 Nimbula 기반의 IaaS 기술이라고 구분할 수 있습니다.

OCI가 등장하기 전까지 모든 오라클 PaaS는 OPC 현재 이름으로는 OCI Classic에서 서비스되었습니다. 2017년 OCI를 발표한 후, OCI Classic에서 동작하던 모든 PaaS를 OCI로 포팅하고 있습니다. 예를 들어 오라클 클라우드의 Big Data 서비스인 Oracle BDC(Big Data Cloud)는 2018년 4월 현재 OCI와 OCI Classic에 배포 가능합니다.


## OCI와 OCI-Classic 무엇이 다른가?

OCI와 OCI Classic에 대하여 간략히 정리해 보겠습니다.

## OCI Classic

OCI Classic은 Nimbula를 기반으로 개발된 IaaS 서비스입니다. Nimbula Director를 기반으로하며 하이퍼바이저로 Xen을 사용합니다. OCI Classic은 VM만을 서비스하고 Bare Metal[^1]은 제공하지 않습니다. 앞에서 소개한 것처럼, 2014년 OOW에서 OPC라는 이름으로 공개되었고, 2017년 OOW에서 OCI Classic으로 이름이 변경되었습니다. 오라클의 모든 데이터 센터(NAC, EMEA, APAC)에서 제공되는 서비스입니다.

[^1]: Bare Metal이란 물리적인 호스트 컴퓨터를 제공하는 것을 의미합니다. 고객에게 가상화 컴퓨팅 자원이 아닌 물리적인 서버를 직접 제공하는 서비스를 의미합니다.

OCI Classic은 네트워크로는 Shared Network과 IP Networks를 지원합니다. Shard Network은 클라우드 계정 단위로 네트웍을 관리합니다. 방화벽 그룹의 개념인 Security List로 VM 그룹을 만들고 보안 룰을 적용하여 VM들을 격리하는 방식을 사용합니다. 그리고 VM에 할당되는 Private IP는 클라우드 계정 단위로 관리 됩니다.

IP Network를 이용하면 클라우드 계정 단위로 IP Address를 관리할 수 있고 서브넷(Subnet)을 구성하여 여러개의 네트워크 망을 관리할 수 있습니다. IP Network을 사용하면 여러개의 네트워크 망을 구성하여 VM을 격리할 수 있습니다. IP Network을 사용하면 Shared Network 보다 향상된 네트워크 구성 및 안전한 환경을 만들 수 있습니다.

OCI Classic이 오라클 최초의 IaaS 서비스인 만큼 오라클의 모든 PaaS는 OCI Classic을 지원합니다.

## OCI

이름에서 알 수 있는 것처럼, 현재 오라클의 주력 IaaS 서비스는 OCI입니다. 데이터 센터, 서버 및 랙 및 네트워크 디자인을 클라우드 최적화한 데이터센터에서 운영되는 IaaS 서비스입니다. 데이터 센터 디자인에 Region, Availability Domain이 적용되어 있으며 Flat Network 기반으로 구성되어 있어 뛰어난 가용성과 확장성을 제공합니다. VCN(Virtual Cloud Network)[^2]를 지원하여 오라클 클라우드에 완전한 private network를 구성하고 설정할 수 있습니다. VCN은 서브넷, 라우트 테이블, 게이트웨이를 갖는 네트워크의 Software-defined Network 버전입니다.

[^2]: VCN(Virtual Cloud Network)은 클라우드 내에서 사용자가 정의하는 가상 네트워크 망을 의미합니다. Private IP를 관리하는 단위가 됩니다. VCN 아래 Subnet을 구성할 수있습니다. VCN을 아마존에서는 VPC(Virtual Private Cloud)라고 표현합니다.


OCI에서는 VM과 Bare metal 서비스를 모두 제공합니다. 하이퍼바이저로 KVM을 사용합니다. Edge Service로 Email, DNS, Load Balancer를 제공합니다. 현재 HPC, AI 및 Machine Learning은 OCI에서만 이용 가능합니다.

{{< img src="https://taewanmerepo.github.io/2018/04/ocivsociclassic/oci.png"
title="그림 1"
caption="OCI를 활용한 클라우드 시스템 구성" >}}

OCI의 주요 구성 컴포넌트는 다음과 같습니다. OCI의 구성 컴포넌트에 대해서는 별도의 문서로 정리하겠습니다.

- Region
- Availability Domain
- Compartments
- IAM
- Virtual Cloud Network
- Edge Service

{{< img src="https://taewanmerepo.github.io/2018/04/ocivsociclassic/ocioverview.jpg"
title="그림 2"
caption="OCI Overview" >}}

2018년 4월 현재 OCI 지원 Region은 다음과 같습니다.

|리전 위치|리전 명|리전 키|
|----|----|----|
|Phoenix, 미국|us-phoenix-1|PHX|
|Ashburn, 미국|us-ashburn-1|IAD|
|프랑크푸크트, 독일|eu-frankfurt-1|FRA|
|런던, 영국|uk-london-1|LHR|

- 출처: https://docs.us-phoenix-1.oraclecloud.com/Content/General/Concepts/regions.htm

## OCI & OCI Classic 비교

|항목|OCI|OCI Classic|
|----|----|----|
|Type|Virtual Machine, Bare Metal|Virtual Machine|
|Network|Virtual Cloud Network|Shared Network, IP Network|
|Hypervisor|KVM|Xen (Nimbula Director)|
|서비스 시작|2017년|2014년|

앞에서도 말했던 것처럼 오라클 클라우드의 주력 IaaS 서비스는 OCI입니다. 그렇다면 OCI Classic에는 어떤 의미가 있을까요?

OCI의 장점은 엔터프라이즈 수준의 요구사항을 충족하는 확장에 제약이 없는 클라우드 서비스라는 것입니다. 단점은 OCI Classic과 비교하여 VCN 구성 등 복잡도가 증가한다는 것입니다. OCI Classic은 극단적인 확장과 엔터프라이즈의 복잡한 네트워크 및 가용성 요구사항을 충족하기에는 부족할 수 있지만, 사용하기 쉽고 개념적으로 복잡하지 않다는 장점이 있습니다.

사용자 측면에서 엔터프라이즈 수준이 가용성 및 보안 요구사항과 고성능 자원이 필요하다면 OCI가 적합합니다. 반대로 복잡하지 않은 중소규모 서비스 구성에는 OCI Classic이 편리할 수 있습니다.

## OCI Classic vs C@C(Cloud at Customer)

오라클은 Public Cloud 이외에도 Private Cloud로 C@C(Cloud at customer)를 제공합니다. 오라클이 클라우드에서 제공하는 서버와 소프트웨어 구성을 고객사에 제공하고 해당 서비스를 오라클이 관리하는 서비스입니다. 위치를 기준으로 볼때  C@C는 고객사의 데이터 센터에 위치하기 때문에 Private Cloud입니다. 그러나 운영 형태로 보면 오라클이 직접 운영하는 Public Cloud 서비스입니다. 이 C@C 서비스는 OCI Classic과 같은 체계를 갖고 있습니다.

OCI Classic의 토대가 되는 Nimbula는 볼래 Private Cloud와 Hybrid Cloud를 지양하는 클라우드 기술이었습니다. 이 기술은 이제 C@C 형태로 오라클의 Public Cloud와 고객사의 Private Cloud(C@C)를 연결하는 Hybrid Cloud 기술로 중요한 의미를 갖습니다. 현재 OCI Classic에 배포가능한 모든 PaaS는 Oracle C@C에 배포될 수 있습니다.

{{< img src="https://taewanmerepo.github.io/2018/04/ocivsociclassic/catc.png"
title="그림 3"
caption="Oracle Cloud at customer, C@C" >}}

## 요약

오라클 클라우드는 IaaS로 OCI Classic과 OCI를 제공합니다. OCI Classic은 Nimbula를 기반으로 개발되었으며, 오라클 클라우드의 첫 번째 IaaS 서비스입니다. 현재 제공되는 모든 Oracle PaaS는 OCI Classic으로 초기 개발되었습니다.

OCI는 클라우드 전용 데이터 센터 아키텍처를 기반으로 디자인된 차세대 오라클 클라우드 인프라입니다. 현재 오라클의 주력 IaaS 서비스는  OCI입니다. OCI는 VM과 Bare Metal 서비스를 모두 지원하며, 고성능 컴퓨팅(AI, ML, HPC) 서비스를 제공합니다. 또한 오라클 PaaS는 현재 대부분 OCI Classic과 OCI를 모두 지원합니다.

Oracle C@C(Cloud at Customer)는 고객사의 데이터센터에서 배포되어 운영되는 클라우드 서비스로 OCI Classic을 기반 기술로 개발되었습니다. 서버의 위치는 고객사 데이터 센터이며, 클라우드 시스템의 운영은 오라클이 담당합니다. 위치를 기준으로 Private Cloud이지만, 기반 기술 및 운영 주체를 기준으로 볼 때 Public Cloud입니다. OCI Classic에 배포 가능한 모든 PaaS는 현재 Oracle C@C에도 배포될 수 있습니다.

## Oracle Cloud 관련 문서

> - Oracle Cloud 공통
>   - [Oracle Cloud IaaS: OCI vs OCI Classic](/post/oci_and_oci_classic/)
>   - [Oracle Cloud 트라이얼 신청 절차 (2018.04)](/post/oracle_cloud_trial/)
>   - [윈도우, 리눅스, 맥에서 ssh 보안키 생성](/post/oci_classic_computing/)
> - Oracle Cloud Infrastructure Classic
>   - [OCI Classic: Compute 서비스, VM 생성](/post/oci_classic_computing/)
>   - [OCI Classic: Compute 서비스, VM의 고정 Public IP 지정](/post/oci_classic_computing_with_permanent_public_ip/)
