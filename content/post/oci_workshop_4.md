
+++
date = "2018-09-20T02:20:25+09:00"
description = "OCI(IaaS)의 워크샵 시리즈 입니다."
title = "OCI 따라하기 1-4. Private Subnet 설정하기"
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch4/diagram2.png"
thumbnailInPost = ""
tags = ["OCI", "Oracle Cloud"]
categories = ["Oracle Cloud"]
author = "esther.ryu"
language = "bash"  
+++

<font color=grey>
### OCI IaaS 따라하기 시리즈
1. OCI Network<br>
	* VCN 생성하기
	* DRG 생성하기
	* Security List 설정하기</font><font color=red>
	* Private Subnet 설정하기</font><font color=grey>
2. OCI Instance
	- Instance 생성하기 생성하기
	- Private Subnet에 생성하기
3. Load Balancer
	- Load Balancer 생성하기
	- Load Balancer를 사용하여 보안강화하기
4. Resource 삭제하기</font><br>

---

### Private subnet route table 설정하기
Security list와 route rule을 사용하여 Private subnet을 생성할 수 있습니다. Route rule과 security list를 가지고 있어도, Private subnet은 Public Internet에 직접적으로 접속할 수 없습니다. Private subnet에서 생성된 인스턴스는 Public IP를 갖지 않습니다. 그러므로 Oracle DB와 같이 데이터에 민감한 리소스를 배치하는데 이상적인 환경을 제공합니다. <br>
Private subnet에서 DRG를 통해 고객의 온프레미스 환경에 접속할 수 있습니다. 온 프레미스 네트워크는 일반적으로 IPSec VPN 또는 FastConnect를 통해 Oracle Cloud에 연결합니다. 연결은 DRG 정보에서 확인할 수 있습니다.
<br><br>

1. VCN정보에서 Route Tables를 클릭합니다. 
Menu - Networking -> VCN -> 해당 VCN의 상세 정보 -> 좌측 메뉴에서 Route Tables 클릭
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch4/Picture1.png)<br>
<그림1> Security List 확인<br><br>
2. Create Route Table을 클릭하여, 원하는 Route Table을 추가합니다.<br>

|항목|내용|
|---|---|
|Name|PrivateSubnet-RouteTable-AD1|
|Destination CIDR Block|0.0.0.0/0 (Note: on-premise연결을 사용한다면, on-premise CIDR block 정보를 활용할 수 있습니다.) |
|Target Type l|Dynamic Routing Gateway (DRG) (Note: 이는 가상의 라우터로, on-premise와의 연결통로입니다.) |
|Target Compartment|생성할 Compartment 선택|
|Target DRG Gateway |구성된 DRG선택|</table>
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch4/Picture2.png)<br>
<그림2> Menu -> Network -> Dynamic Routing Gateway 클릭<br>
생성된 Route Table을 확인할 수 있습니다. 
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch4/Picture3.png)<br>
<그림3> Route Table Rule 추가하기<br><br>

### Network 구성도
여기까지 진행하시면, 아래의 그림과 같이 Private subnet에서 DRG를 통해 고객의 온 프레미스 환경과 통신할 수 있는 Route Table을 생성하였습니다. 
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch4/diagram1.png)<br>

### Private subnet용 Security list 생성하기
1. VCN의 상세 정보에서 Security List를 클릭하고 Create Security List를 클릭합니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch4/Picture4.png)<br>
<그림4> Security List Rule 추가하기<br><br>
2. PrivateSubnet-SecurityList를 추가하도록 합니다.

|항목|내용|
|---|---|
|Source CIDR|사용할 Public subnet의 CIDR (예:10.0.0.0/24)|
|Protocol |TCP|
|Source port|all|
|Destination port|all|</table>
Egress rule은 별도로 설정하지 않으므로 x를 눌러 삭제한 뒤, create security list를 클릭합니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch4/Picture5.png)<br>
<그림5> PrivateSubnet-SecurityList 추가하기<br>

### Network 구성도
여기까지 진행하시면, 아래의 그림과 같이 Private subnet을 위한 route table이 생성되었습니다. 
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch4/diagram2.png)<br>

### Private subnet 생성하기
1. VCN의 상세 정보에서 Subnet을 클릭하여,  Create Subnet을 수행합니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch4/Picture6.png)<br>
<그림6> Private subnet 생성하기<br>
다음의 정보들이 필요합니다.<br>

|항목|내용|
|---|---|
|Name|PrivateSubnet-AD1|
|Availability Domain|public subnet과 동일한 AD를 선택합니다.(예:AD1)| 
|CIDR Block|해당 VCN내에서 다른 AD들이 가지고 있는 public subnet과 다른 대역의 CIDR을 할당합니다. (예:10.0.4.0/24)|
|Route Table|private subnet route table을 선택합니다 (예: PrivateSubnet-RouteTable-AD1)|
|Subnet Access|Private Subnet. *여기에 생성되는 instance는 public ip가 할당되지 않습니다.|
|DHCP Option|Default options|
|Security List|private subnet security list선택 (예: PrivateSubnet-SecurityList)|</table>
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch4/Picture7.png)<br>
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch4/Picture7-1.png)<br>
<그림6> Private subnet 정보 입력

여기까지 수행하셨으면, 아래의 그림과 같이 AD1내에 public subnet과 private subnet이 생성되었습니다.
다이어그램으로 보시면 아래의 그림과 같습니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch4/diagram3.png)<br>
**< Network 구성도 >**
