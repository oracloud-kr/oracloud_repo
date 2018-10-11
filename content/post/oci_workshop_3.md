
+++
date = "2018-09-22T02:20:25+09:00"
description = "OCI(IaaS)의 워크샵 시리즈 입니다."
title = "OCI 따라하기 1-3. Security List 설정하기"
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch3/SL.png"
thumbnailInPost = ""
tags = ["OCI", "Oracle Cloud", "VCN", "DRG", "Security List", "Load Balancer"]
categories = ["Oracle Cloud"]
author = "esther.ryu"
language = "bash"  
+++


### OCI IaaS 따라하기 시리즈
1. OCI Network<br>
	- [VCN 생성하기](../oci_workshop_1)
	- [DRG 설정하기](../oci_workshop_2)
	- **Security List 설정하기**
	- [Private Subnet 설정하기](../oci_workshop_4)
2. OCI Instance
	- [Instance 생성하기](../oci_workshop_5)
	- [Private Subnet에 생성하기](../oci_workshop_6)
3. Load Balancer
	- [Load Balancer 생성하기](../oci_workshop_7)
4. Resource 삭제하기
	- Resource 삭제하기

---

### Security List 설정하기
**Security list**는 특정 포트 및 프로토콜을 사용하여 subnet및 인스턴스로의 트래픽을 제어하는 가상 방화벽입니다. 기존의 security list에 다른 규칙을 추가하는 방법을 알아봅니다.<br><br>

1. VCN의 상세정보에서 좌측 메뉴의 Security List를 확인합니다.
Menu - Networking -> VCN -> 해당 VCN의 상세 정보
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch3/Picture1.png)<br>
<그림1> Security List 확인<br>

2. 현재 설정되어 있는 security list를 확인합니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch3/Picture2.png)<br>
<그림2> Menu -> Network -> Dynamic Routing Gateway 클릭

3. Edit All Rules를 클릭하여, 아래의 Rule을 추가합니다.<br>

|항목|내용|
|:---:|:---:|
|Source CIDR|0.0.0.0/0 (from IPs on the internet)|
|Protocol|TCP|
|Source port|all|
|Destination port|80 (for HTTP)|
</table>
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch3/Picture3.png)<br>
<그림3> Security List Rule 추가하기<br><br>
추가할 security rule의 정보를 입력하고 Save security rule list를 클릭합니다.<br>
Stateless박스 부분은 체크하지 않습니다. Stateful은 소스에 대한 응답을 다시 허용합니다. stateless는 소스에 응답하기 위해 명시적으로 egress규칙을 가져야 합니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch2/Picture4.png)<br>
<그림4> Security List Rule 추가하기<br><br>
4. 생성된 Rule을 확인합니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch3/Picture5.png)<br>
<그림5> Security List 확인 <br><br>

### Security List 구성도
여기까지 수행하셨으면, 아래의 그림과 같이 기존의 default security list에 추가적인 security list가 생성되었습니다. <br>
이와 같은 작업을 통해 IP address 혹은 port에 따라 클라우드 환경에 대한 접근을 제어할 수 있으며, 이는 가상의 방화벽 역할을 포함합니다.
다이어그램으로 보시면 아래의 그림과 같습니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch3/SL.png)<br>
**< Network 구성도 >**
