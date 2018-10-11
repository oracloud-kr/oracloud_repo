
+++
date = "2018-09-21T02:20:25+09:00"
description = "OCI(IaaS)의 워크샵 시리즈 1-2 입니다."
title = "OCI 따라하기 1-2. DRG 생성하기"
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch2/DRG.png"
thumbnailInPost = ""
tags = ["OCI", "Oracle Cloud", "VCN", "DRG", "Security List", "Load Balancer"]
categories = ["Oracle Cloud"]
author = "esther.ryu"
language = "bash"  
+++

### OCI IaaS 따라하기 시리즈
1. OCI Network<br>
	- [VCN 생성하기](../oci_workshop_1)
	- **DRG 생성하기**
	- [Security List 설정하기](../oci_workshop_3)
	- [Private Subnet 설정하기](../oci_workshop_4)
2. OCI Instance
	- [Instance 생성하기](../oci_workshop_5)
	- [Private Subnet에 생성하기](../oci_workshop_6)
3. Load Balancer
	- [Load Balancer 생성하기](../oci_workshop_7)
4. Resource 삭제하기
	- Resource 삭제하기

---

### DRG 생성하기
VCN 생성하기에서 생성하였던, IGW(Internet Gateway)가 Public Internet과 연결하기 위한 통로라면, **DRG(Dynamic Routing Gateway)**는 **고객의 온 프레미스 환경에서 VCN에 접속하기 위한 통로**입니다. <br>
DRG는 고객의 IPSec VPN 또는 Fast Connect 연결에서 VCN으로 트래픽을 라우팅하는 가상 라우터입니다. 경로 테이블에 경로 규칙을 만들어 VCN 서브넷에서 고객의 구내 네트워크에 액세스하도록 허용 할 수 있습니다. 
<br><br>
* DRG는 VCN을 만들 때 기본적으로 만들어지지 않습니다. <br>
* Trial Account로 DRG를 생성하려는 경우에는 Resource의 제약으로 인해 생성하지 못할 수 있습니다.

<br>
![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch2/diagram.png)<br>
<DRG의 구성도>

1. DRG를 생성하고자 하는 Compartment를 바르게 선택하고, DRG를 생성하기 위한 메뉴로 이동합니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch2/Picture1.png)<br>
<그림1> Compartment 선택하기<br>
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch2/Picture2.png)<br>
<그림2> Menu -> Network -> Dynamic Routing Gateway 클릭

2. Create Dynamic Routing Gateway를 선택하여, 관련된 정보를 입력합니다.
예) 이름: On-Premise-Gateway
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch2/Picture3.png)<br>
<그림3> Create Dynamic Routing Gateway 클릭<br>
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch2/Picture4.png)<br>
<그림4> DRG의 정보 입력

3. VCN과 DRG를 연결하는 작업을 진행하도록 합니다.
메뉴 -> Networking -> VCN -> DRG와 연결할 VCN의 상세정보로 이동<br>
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch2/Picture6.png)<br>
<그림5> VCN 상세정보 <br><br>
좌측 메뉴에서 Dynamic Routing Gateway를 클릭하고, Attach Dynamic Routing Gateway를 클릭합니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch2/Picture7.png)<br>
<그림6> Attach Dynamic Routing Gateway 클릭<br><br>
Dynamic Routing Gateways의 Dropdown list에서 원하는 DRG를 선택하고 Attach Dynamic Routing Gateway를 클릭합니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch2/Picture8.png)<br>
<그림7> Attach Dynamic Routing Gateway 정보 입력  

### 온 프레미스 VPN장비 등록하기 
이 단계에서는 DRG가 연결될 VPN 고객 전제 장비를 식별하고, 연결하는 작업을 수행합니다.

1. Customer Premises Equipment로 이동합니다.
Menu -> Networking -> Customer Premises Equipment
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch2/Picture9.png)<br>
<그림8> Customer Premises Equipment

2. Create Premises Equipment를 클릭합니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch2/Picture10.png)<br>
<그림9> Create Premises Equipment 클릭<br><br>
올바른 Compartment인지 확인하고, 고객의 VPN장비 이름과 사용하는 IP정보를 입력하고, Create를 클릭합니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch2/Picture11.png)<br>
<그림10> 온 프레미스 VPN장비의 정보 입력<br><br>
아래와 같이 Customer-Premises Equipment가 생성된 것을 확인할 수 있습니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch2/Picture12.png)<br>
<그림11> Customer-Premises Equipment 생성 완료

### VCN과 온 프레미스 VPN장비 연결하기 
이제 DRG는 IPSec VPN이나 FastConnect연결을 통해 고객의 온 프레미스 환경과 연결될 준비가 끝났습니다.


1. VCN 정보로 이동합니다.
메뉴 -> Networking -> VCN 
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch2/Picture13.png)<br>
<그림12> VCN 정보

2. 좌측의 메뉴에서 Dynamic Routing Gateways를 클릭하여, DRG의 상세 정보를 확인합니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch2/Picture14.png)<br>
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch2/Picture15.png)<br>
<그림13> DRG 정보 확인

3. IPSec Connection을 생성합니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch2/Picture16.png)<br>
<그림14> Create IPSec Connection 클릭<br>
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch2/Picture17.png)<br>
<그림15> IPSec Connection 정보 입력<br><br>

**IPSec VPN연결이 설정되면, 고객사의 네트워크 담당자에게 터널 정보를 전달하여 해당 연결을 완료할 수 있습니다.**

### 터널 설정하기 
터널을 설정하여 고객사의 VPN장비와 VCN 양 단의 신뢰성을 보장할 수 있는 통신이 가능해집니다.<br>
터널 정보를 확인하기 위해 IPSec Connection 우측의 …을 클릭하여 Tunnel Information을 클릭합니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch2/Picture18.png)<br>
<그림16> Tunnel Information 확인 <br><br>
다음의 정보들을 확인할 수 있습니다.  <br>
고객사의 네트워크 담당자가 아래의 정보를 가지고 네트워크 연결을 완료할 때까지 상태는 **DOWN**으로 표시됩니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch2/Picture19.png)<br>
<그림17> Tunnel Information<br>

이로써, IPSec VPN 연결에 대한 DRG설정이 완료됩니다. <br>이제 VCN을 고객의 온프레미스 환경에 “하이브리드 클라우드”로 연결할 수 있습니다.

### DRG구성도
현재까지 설정한 네트워크 환경을 한번 리뷰하자면, VCN에 다음과 같은 요소들을 구성하였습니다. <br>
- VCN에 고가용성을 보장하기 위한 3개의 AD <br>
- 각 AD별로 3개의 public subnet <br>
- subnet에서 internet으로 트래픽을 라우팅하는 default route rule  <br>
- 22번 port에 대해 SSH연결을 허용하는 default security rule <br>
- internet gateway  <br>
- Dynamic Routing Gateway <br>
다이어그램으로 보시면 아래의 그림과 같습니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch2/DRG.png)<br>
**< Network 구성도 >**
