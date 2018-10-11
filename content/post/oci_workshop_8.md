
+++
date = "2018-10-11T02:20:25+09:00"
description = "OCI(IaaS)의 워크샵 시리즈 입니다."
title = "OCI 따라하기 4. Resource 삭제하기"
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch8/diagram.png"
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
	- [Security List 설정하기](../oci_workshop_3)
	- [Private Subnet 설정하기](../oci_workshop_4)
2. OCI Instance
	- [Instance 생성하기](../oci_workshop_5)
	- [Private Subnet에 생성하기](../oci_workshop_6)
3. Load Balancer
	- [Load Balancer 생성하기](../oci_workshop_7)
4. Resource 삭제하기
	- **Resource 삭제하기**

---

### Load Balancer 종료하기
Menu -> Networking -> Load Balancer<br>
해당 Load Balancer를 Terminate합니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch8/Picture1-1.png)<br>
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch8/Picture1-2.png)<br>
<그림1> Load Balancer 종료하기<br>

### Instnace 종료하기
Menu -> Compute -> Instsance<br>
해당 Instance를 Terminate 합니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch8/Picture2-1.png)<br>
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch8/Picture2-2.png)<br>
<그림2> Instnace 종료하기<br><br>
* Permanently delete the attached Boot Volume에 체크하면, 해당 인스턴스의 Boot Volume이 함께 삭제됩니다. <br>체크하지 않는 경우에는, 향후 다른 인스턴스에서 해당 Boot volume을 사용할 수 있습니다.<br>
Menu -> Compute – Boot Volumes
사용하고자하는 Boot Volume의 메뉴에서 Create Instance 선택
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch8/Picture3.png)<br>
<그림3> 남아있는 Boot Volume으로 Instance 생성하기<br>

### Network 삭제하기
네트워크를 정리하는 두 가지 방법이 있습니다. 

1. 첫번째 방법은 각 네트워크 리소스를 개별적으로 삭제하는 것입니다. 이는 VCN을 삭제하지 않고 특정 리소스를 정리하려는 경우에 적합합니다.<br>
참고 : 서브넷에 컴퓨팅 또는 데이터베이스 리소스가 있으면 VCN 또는 서브넷이 삭제되지 않습니다. 해당 네트워크를 사용하는 리소스를 먼저 제거해야합니다.<br>
여러가지의 리소스가 함께 구성된 경우, Load Balancer -> Private Resource -> Public Reousrce 순으로 삭제합니다.
	- Subnet 삭제<br>
	Menu -> Networking -> VCN -> Subnets<br>
	해당 Subnet를 Terminate합니다. (Public과 Private 모두)
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch8/Picture5-1.png)
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch8/Picture5-2.png)<br>
<그림4> Subnet 삭제하기<br>
	- Route Table과 Route Rule을 삭제<br>
	Menu -> Networking -> VCN -> Route Table<br>
	해당 Route Table를 Terminate합니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch8/Picture6-1.png)
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch8/Picture6-2.png)<br>
<그림5> Route Table 삭제하기 <br>
	- Security list 삭제<br>
	Menu -> Networking -> VCN -> Security List<br>
	해당 Security List를 Terminate합니다.<br>
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch8/Picture7-1.png)
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch8/Picture7-2.png)<br>
<그림6> Security list 삭제하기<br>

2. 두 번째 방법은 모든 보안 목록, 경로 테이블 및 게이트웨이를 삭제하는 VCN을 삭제하여 전체 네트워크를 정리할 수 있습니다.<br>
Menu -> Networking -> VCN<br>
해당 VCN을 Terminate 합니다.<br>
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch8/Picture4-1.png)
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch8/Picture4-2.png)<br>
<그림7> VCN 및 네트워크 리소스 삭제하기<br>

이로써, 구성하였던 VCN안의 컴퓨팅 인스턴스를 비롯한 모든 리소스를 삭제하였습니다.<br>
클라우드 환경을 구축하실때에는 [VCN 생성하기](../oci_workshop_1)부터 차례대로 참조하시면서 구성하시면 됩니다.
