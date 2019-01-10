+++
description = "오라클 클라우드의 OCI Classic을 이용하여 VM을 생성하는 과정을 소개합니다."
title = "OCI Classic: Compute 서비스, VM 생성"
categories = ["cloud"]
thumbnailInPost = "https://taewanmerepo.github.io/2018/04/ociclass/200.jpg"
thumbnailInList = "https://taewanmerepo.github.io/2018/04/ociclass/list.jpg"
date = "2018-04-27T15:59:47+09:00"
language = ""
tags = ["oracle", "oci-c", "vm", "hostname", "김태완"]
author = "taewan.kim"
adsense = "true"
+++

오라클은 클라우드 IaaS로 Oracle Cloud Infrastructure(OCI)와 Oracle Cloud Infrastructure Classic(OCI Classic) 두 가지를 제공하고 있습니다. 오라클은 Public Cloud IaaS로 OPC(Oracle Public Cloud)를 2014년 OOW(Oracle Open World)에서 공개하였고, OOW 2017에서 OCI Classic으로 서비스 브랜드 명을 변경하였습니다.

OCI Classic은 Nimbula Director(Xen 기반 하이퍼바이저)을 기반으로 만들어졌으며, IaaS 서비스로 가상머신 컴퓨팅과 네트워크(Shared & IP Networks)를 제공합니다.

본 문서에서는 Oracle Cloud에서 OCI Classic에서 VM을 생성하는 과정을 소개합니다.

## VM 생성 시나리오

다음과 같은 시나리오를 바탕으로 VM을 구성해 보겠습니다.

- OCI Classic에서 VM 생성
- Oracle Linix 7.3 지정
- Hostname 지정
- SSH 키 지정
- 네트워크 타입 지정
- VM 이미지 생성 후 ssh 접속 보안 설정
- SSH 접속

## VM 이미지 생성 절차

OCI Classic의 컴퓨팅 서비스는 다음과 같은 방법으로 생성할 수 있습니다.

- 웹 기반 OCI Classic Computing 서비스 콘솔의 웹UI에서 생성
- Terraform을 이용한 생성
- REST API를 이용한 생성
- OPC CLI 툴을 이용한 생성

본문에서는 "__웹 기반 OCI Classic Computing 서비스 콘솔의 웹 UI__"를 이용하여 VM을 생성하는 절차를 소개합니다. 나머지 방법은 별도 문서로 다루겠습니다.  

### 설치 전 클라우드 콘설 접근

OCI Classic의 컴퓨팅 서비스로 VM을 만들기 위해서는 "__OCI Classic Computing 서비스 콘솔__"에 접근해야 합니다. 이 서비스 콘솔은 오라클 클라우드 대쉬보드로 부터 접근 가능합니다. 오라클 클라우드에 로그인하면 <그림 1>과 같은 대쉬로드로 이동합니다. 대쉬보드에 "Compute Classic" 위젯이 보이지 않는다면 "__Customizing Dashboard__"를 선택하여 "__OCI Classic Computing 서비스 콘솔__"을 오픈할 수 있습니다.

{{< img src="https://taewanmerepo.github.io/2018/04/ociclass/img010.jpg"
title="그림 1"
caption="대쉬보드 커스터마이징" >}}

"__Customizing Dashboard__"를 선택하면 <그림 2>와 같은 화면이 출력됩니다. 이 화면에서 대쉬 보드에 공개될 위젯을 선택할 수 있습니다. <그림 2>에서 "Compute Classic"의 상태를 "show"로 변경하고, 오른쪽 위의 "X" 마크를 클릭합니다.

{{< img src="https://taewanmerepo.github.io/2018/04/ociclass/img020.jpg"
title="그림 2"
caption="OCI-Classic Computing 서비스 콘솔 시작" >}}

<그림 2>를 완료하면 <그림 3>과 같이 "Compute Classic" 위젯이 나타납니다. 이 위젯의 오른쪽 아래 메뉴 아이콘으로부터 __Open Service Console__을 클릭하면, <그림 4>와 Compute Classic 서비스 콘솔로 이동합니다.

{{< img src="https://taewanmerepo.github.io/2018/04/ociclass/img030.jpg"
title="그림 3"
caption="OCI-Classic Computing 서비스 콘솔:VM 생성" >}}

### Compute Classic에 VM 생성

Compute Classic 서비스 콘솔에서 __Create Instance__ 버튼을 클릭하여 VM 생성 절차가 시작됩니다.

{{< img src="https://taewanmerepo.github.io/2018/04/ociclass/img040.jpg"
title="그림 4"
caption="VM 생성 시작" >}}

<그림 5>에서는 VM 생성 방식을 선택할 수 있습니다. 사전에 정의된 설정값으로 Oracle Linux, Unbuntu, Window 서버 윈클릭으로 생성할 수 있습니다. 사전 정의된 이미지를 생성할 경우 3가지 정보를 입력하고 "Create" 버튼을 클릭하면 완료됩니다. 절차를 요약하면 다음과 같습니다.

- (1): VM 이름 설절
- OS 이미지 선택
  - Oracle Linux 7.2 UEKR4
  - Ubuntu 16.04
  - Window Service 2012
- (2): SSH 키 등록
  - 계정에 등록된 SSH 키 선택
- (3): SSH의 공개키 Download

위와 같은 절차로 선택 및 기본 정보를 입력하고 "Create" 버튼을 클릭하면 VM이 생성됩니다.

OS 종류, 네트워크 설정, 스토리지 등을 변경해야 한다면 오른쪽 위에 위치하는 "Customize" 버튼을 클릭하여 사용자 정의로 VM이미지를 생성할 수 있습니다. 본 문서에서는 사용자 정의 생성 방식을 소개하겠습니다. "Customize" 버튼을 클릭하고 다음 단계로 넘어갑니다.

{{< img src="https://taewanmerepo.github.io/2018/04/ociclass/img050.jpg"
title="그림 5"
caption="VM 생성 방식 선택" >}}

<그림 6>에서 사용할 OS 이미지를 선택하고 __>__ 버튼을 클릭하여 다음 단계로 넘어갑니다. OS 이미지는 오라클 리눅스가 기본 제공되고, Marketplace에서는 425개의 이미지를 추가로 선택할 수 있습니다. 425개 이미지에는 Ubuntu, Suse, Window가 포함되어 있습니다. Marketplace에서 제공하는 이미지는 오라클과 Bitnami에서 관리합니다.

<그림 6>에서는 OL 7.2 UEKR4_x86_64를 선택하고 __>__ 버튼을 클릭하여 다음 단계로 넘어갑니다.

{{< img src="https://taewanmerepo.github.io/2018/04/ociclass/img060.jpg"
title="그림 6"
caption="OS 선택" >}}

다음 단계는 VM의 자원을 결정하는 단계입니다. 사전 정의된 Shape을 선택합니다. 기본 Trial에서는 ocpu와 메모리 사이즈를 기준으로 "General Purpose"와 "High Memory" 두 가지 유형을 제공합니다. 사용할 shape을 클릭한 후, __>__ 버튼을 클릭하여 다음 단계로 넘어갑니다.

{{< img src="https://taewanmerepo.github.io/2018/04/ociclass/img070.jpg"
title="그림 7"
caption="자원(Shape) 선택" >}}

<그림 8>에서는 VM의 기본 정보를 설정합니다. VM의 이름과 라벨, SSH 키를 등록하는 단계입니다. 사용할 이름과 라벨을 입력하고, 사전에 만든 인증 공개키를 "Add SSH Public key" 버튼을 클릭하여 등록합니다.

SSH 키를 생성하는 방법에 대해서는 아래에 링크된 문서를 참조하시기 바랍니다.

- <a href="/post/ssh_key/" target="_blank"> 윈도우, 리눅스, 맥에서 ssh 보안키 생성</a>

VM 이름, 라벨 및 인증서 공개키를 등록한 후, __>__ 버튼을 클릭하여 다음 네트워크 설정 단계로 넘어갑니다.

{{< img src="https://taewanmerepo.github.io/2018/04/ociclass/img080.jpg"
title="그림 8"
caption="인스턴스 정보 설정" >}}

다음 단계는 네트워크 설정 단계입니다. OCI Classic의 Compute 서비스는 IPNetwork과 Shared Network 두 가지 유형의 네트워크 방식을 제공합니다. Shared Network은 Private IP를 서비스 단위로 발급하고 관리하며, 방화벽과 같은 개념을 적용하여 VM를 묶고 보안 관리합니다. IPNetwork은 서브넷을 구성하여 Private IP를 도메인 단위에서 관리하는 방식입니다.   

네트워크 설정에서는 세 가지를 설정해야 합니다.

- DNS Hostname prefix: 사용할 hostname 지정
- Shared Network: 선택
- IP Network: 선택 해제

<그림 9>와 같이 설정하고, __>__ 버튼을 클릭하여 다음 스토리지 설정 단계로 넘어갑니다.

{{< img src="https://taewanmerepo.github.io/2018/04/ociclass/img090.jpg"
title="그림 9"
caption="네트워크 설정" >}}

<그림 10>에서는 블록 스토리지를 추가하는 단계입니다. 기본 스토리지로 120GB 블록 스토리지가 추가되어 있습니다. 현재는 추가적인 스토리지 추가 없이, __>__ 버튼을 클릭하여 다음 스토리지 설정 단계로 넘어갑니다. 블록 스토리지 추가 및 마운트 방법은 별도 문서로 작성하겠습니다.

{{< img src="https://taewanmerepo.github.io/2018/04/ociclass/img100.jpg"
title="그림 10"
caption="스토리지 설정" >}}

이제 모든 설정 단계를 마친 상태입니다. <그림 11>에서 마지막 단계에 "이미지 생성 정보 요약"을 확인한 후 "Create" 버튼을 클릭하여 VM 생성을 시작합니다.

{{< img src="https://taewanmerepo.github.io/2018/04/ociclass/img110.jpg"
title="그림 11"
caption="이미지 생성 정보 요약" >}}

<그림 11>에서 "Create" 버튼을 클릭하면 <그림 12>와 같이 Compute Classic 서비스 콘솔의 메인 페이지로 이동합니다. VM 이미지 생성에 약 3-5분 정도 걸립니다. 오른쪽의 페이지 갱신 버튼을 클릭을 클릭하면 <그림 12>와 같이 VM 정보가 출력됩니다. <그림 12>에서 VM 이름, 상태, Public IP와 Private IP를 확인할 수 있습니다.

{{< img src="https://taewanmerepo.github.io/2018/04/ociclass/img120.jpg"
title="그림 12"
caption="VM 이미지 생성 확인(oci-c_vm01)" >}}

현재 VM이 생성된 상태입니다. 앞에서 생성된 VM은 오라클 클라우드 외부망에서 접근할 수 없는 상태입니다. 외부 인터넷에서 앞에서 생성한 VM에 접속하기 위해서 Security Rule을 등록해야 합니다. OCI Classic의 Shared Network에는 다음과 같은 보안 관련 용어를 사용합니다.

- Security List: 오라클 클라우드의 도메인에서 생성한 VM에 Security rule을 적용하는 논리적인 그룹. 방화벽의 그룹과 유사한 개념
- Security Application: Security에 적용할 네트워크 프로토콜과 포트 정보
- Security Rule: Security List에 적용할 보안 규칙

외부에서 앞에서 생성한 VM에 SSH 접근을 위해서는 Security Rule을 새로 등록해야 합니다. Security Rule 등록 페이지는 <그림 13>과 같이 이동 가능합니다.

{{< img src="https://taewanmerepo.github.io/2018/04/ociclass/img130.jpg"
title="그림 13"
caption="Security Rule 등록" >}}

<그림 14>는 Security Rule 등록 화면입니다. 외부망에서 ssh 접근을 허용하는 보안 규칙은 다음가 같이 생성됩니다.

|번호|이름|설정|설명|
|----|----|----|----|
|(1)|Name|ssh-rule|Security rule 이름|
|(2)|Security Application|ssh|Security rule에 적용할 네트워크 프로토콜과 포트 정보(Security Application)|
|(3)|Source|Security IP List<br/> public-internet|네트워크의 시작점을 IP List로 지정. <br/> 외부 인터넷을 표현하는 기본 IP List가 public-internet|
|(4)|Destination|Security List<br/>default|Security rule의 목적이 그룹<br/>Security List: 내부 관리 VM의 논리적인 단위|

설정이 완료되면 "create" 버튼을 클릭하여 Security Rule이 바로 생성됩니다.

{{< img src="https://taewanmerepo.github.io/2018/04/ociclass/img140.jpg"
title="그림 14"
caption="Security Rule 생성: SSH Rule" >}}

이제 ssh 접근이 가능한 상태가 되었습니다. <그림 15>와 같이 ssh 접근이 가능합니다. 오라클 리눅스의 기본 계정명은 opc이고 ubuntu의 경우 기본 계정은 ubuntu입니다. ssh 접근 후 호스트명이 <그림 9>의 DNS Hostname Prefix의 설정값으로 hostname이 설정된 것을 확인할 수 있습니다.

{{< img src="https://taewanmerepo.github.io/2018/04/ociclass/img150.jpg"
title="그림 15"
caption="ssh 접속 및 host명 확인" >}}

## 요약

Oracle Cloud Infrastructure Classic의 Compute 서비스 VM을 생성해 보았습니다. OCI와 OCI Classic를 간단히 구분해 보았습니다. 또한 OCI Classic의 IPNetwork와 Share Network의 차이를 살펴보고 VM 생성 전체 절차를 확인해 보았습니다.

추가적인 OCI Classic의 관련 문서를 별도 문서로 추가하겠습니다.

## Oracle Cloud 관련 문서

> - Oracle Cloud 공통
>   - [Oracle Cloud IaaS: OCI vs OCI Classic](/post/oci_and_oci_classic/)
>   - [Oracle Cloud 트라이얼 신청 절차 (2018.04)](/post/oracle_cloud_trial/)
>   - [윈도우, 리눅스, 맥에서 ssh 보안키 생성](/post/oci_classic_computing/)
> - Oracle Cloud Infrastructure Classic
>   - [OCI Classic: Compute 서비스, VM 생성](/post/oci_classic_computing/)
>   - [OCI Classic: Compute 서비스, VM의 고정 Public IP 지정](/post/oci_classic_computing_with_permanent_public_ip/)
