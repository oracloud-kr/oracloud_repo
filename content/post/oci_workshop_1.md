
+++
date = "2018-09-17T02:20:25+09:00"
description = "OCI(IaaS)의 워크샵 시리즈 1-1 입니다."
title = "OCI 따라하기 1-1. VCN 생성하기"
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch1/VCN.png"
thumbnailInPost = ""
tags = ["OCI", "Oracle Cloud"]
categories = ["Oracle Cloud"]
author = "esther.ryu"
language = "bash"  
+++

<font color=grey>
### OCI IaaS 따라하기 시리즈
1. OCI Network<br></font><font color=red>
	* VCN 생성하기</font><font color=grey>
	* DRG 생성하기
	* Security List 설정하기
	* Private Subnet 설정하기
2. OCI Instance
	- Instance 생성하기
	- Private Instance 생성하기
3. Load Balancer
	- Load Balancer 생성하기
	- Load Balancer를 사용하여 보안강화하기
4. Resource 삭제하기</font><br>

---

### Oracle Cloud에 로그인하기
Oracle Cloud를 사용하기 위해서는, 다른 사용자들과 분리된  해당 계정이 사용할 네트워크를 먼저 구성하여야 합니다. Oracle Cloud에서는 이를 **VCN** - Virtual Cloud Network 라고 합니다. 
VCN을 생성하고 Public Internet망을 통해서 통신할 수 있도록 설정하는 방법을 알아보도록 하겠습니다. 

1. 웹 브라우저에서 Oracle Cloud에 접속합니다.
제공받은 Test환경, 혹은 Tiral계정으로 할당받은 환경으로 접속합니다.
 [https://console.oraclecloud.com/](https://console.oraclecloud.com/) 과 유사한 이름으로 시작하는 url로 접속합니다.
<그림 1>과 같이 Oracle Cloud에 접속하면, Compartment 정보를 입력합니다.
<br><br>**Compartment란?**<br>
클라우드 리소스를 좀 더 효율적으로 관리할 수 있도록 나누어 관리하는 것을 의미합니다. 오라클 클라우드에 접속하면 최초에 root compartment가 기본생성됩니다. 관리자는 root compartment를 나누어 추가 생성하고, 접속 권한을 제한할 수 있습니다. (한번 생성한 compartment는 삭제가 불가능하니 신중하게 선택하시기 바랍니다.) Compartment list는 사용자가 접속할 수 있는 compartment를 보여줍니다. 하나의 compartment를 선택하면 해당 부분의 리소스만 볼 수 있고, 다른 compartment의 리소스를 보려면 compartment의 전환이 필요합니다. 
<br>
![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch1/Picture1.png)
<그림1> Compartment 정보를 입력<br><br>
Compartment를 선택한 후, User Name과 Password를 입력하면,
로그인 후, <그림2>와 같이 대시보드를 확인할 수 있습니다.
![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch1/Picture2.png)
<그림2> Oracle Cloud Dash Board

2. 대시보드로 접속하기 위해  <그림2>에서 Create Compute Instance를 클릭합니다. 
![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch1/Picture3.png)
<그림3> Oracle Cloud Dash Board
<br> ** 여러개의 Compartment가 구성되어 있는 환경인 경우, Compartment를 선택하라는 메세지가 나올 수 있습니다. <br> VCN을 생성할 Compartment를 선택하시면 됩니다.

3. Oracle Cloud의 리소스를 사용할 수 있는 OCI용 대시보드에 접속하였습니다. <그림4>에서 보시는바와 같이 리소스가 할당되어 있는 리전(위치)와 오라클 데이터센터의 위치(센터 이름)을 확인 할 수 있습니다. 
![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch1/Picture4.png)
<그림4> Oracle Cloud Resource확인

### VCN생성하기
VCN은 virtual cloud network의 약자로, Oracle Cloud 내에서 고객이 사용하게 되는 가상 네트워크 입니다. 온프레미스 상에서 사용하는 고객의 네트워크와 동일하게, 서브넷, 인터넷 게이트웨이, 로드밸런서와 방화벽을 포함합니다.

<그림5>와 같이 고객의 클라우드 환경이 퍼블릭 네트워크와 통신이 가능하도록 만드는 IGW(Internet Gateway)와 기본 방화벽을 생성하는 방법을 살펴보도록 하겠습니다.
![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch1/Picture5.png)
<그림5> VCN 및 IGW 구성도

1. 메뉴에서 Networking 탭으로 이동합니다.
![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch1/Picture6.png)
<그림6> Networking 환경

2. Create Virtual Cloud Network을 클릭하여 원하는 Compartment내에 VCN을 생성합니다.
![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch1/Picture7.png)
<그림7> VCN 생성하기
<br><br>
CREATE IN COMPARTMENT: VCN을 생성할 Compartment선택<br>
NAME: (옵션) 알기 쉬운 것으로 작성<br>
CREATE VIRTUAL CLOUD NETWORK PLUS RELATED REOUSRCES 선택<br>
** 이 경우, 관련된 리소스가 함께 생성됩니다. 
![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch1/Picture8.png)
![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch1/Picture8-1.png)
<그림8> VCN 생성하기
<br><br>
Create Virtual Cloud Network을 클릭하면, <그림9>와 같이 생성된 VCN과 관련된 리소스의 정보를 확인할 수 있습니다.
![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch1/Picture9.png)
<그림9> VCN정보 확인하기
<br><br>
생성된 VCN의 상세정보를 클릭하면, 각각의 AD에 대해 아래와 같이 subnet이 생성된 것을 확인할 수 있습니다.
![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch1/Picture10.png)
![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch1/Picture10-1.png)
<그림10> VCN의 Subnet정보확인하기

### VCN의 상세 정보 확인하기
1. VCN에서 사용할 Default Route Table을 확인합니다.
Menu -> Networking -> VCN 에서 해당 VCN의 상세 정보로 이동합니다. 왼쪽의 메뉴에서 Route Tables 클릭하여, VCN에 생성된 Route Table의 정보를 확인합니다.
![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch1/Picture11.png)
<그림11> Route Table 정보 확인하기<br><br>
상세정보를 클릭하면, 기본적으로는 모든 패킷이(목적지가 0.0.0.0/0인 패킷) Internet Gateway를 통해 통신을 하고 있음을 확인할 수 있습니다.
![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch1/Picture12.png)
<그림12> Route Rule 정보 확인하기

2. Security List를 확인합니다.
VCN 정보화면에서 좌측에 Security Lists를 클릭하면, <그림14>와 같이 현재 적용된 Ingress와 Egress Rules를 확인할 수 있습니다. 
![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch1/Picture13.png)
<그림13> Security List 정보 확인하기<br><br>
아래에서는 인터넷에서 포트 22에 대한 SSH 접속을 허용하는 부분과 트래픽 관리를 위한 ICMP 프로토콜을 허용하는 부분이 확인됩니다.
![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch1/Picture14.png)
<그림14> Security List Rule 정보 확인하기

3. DHCP 정보를 확인합니다. 
VCN 정보화면에서 좌측에 DHCP option 클릭하여, Default DHCP Option이 생성되었는지 확인할 수 있습니다. 
![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch1/Picture15.png)
<그림15> DHCP 정보 확인하기<br><br>
해당 DHCP 부분의 상세 정보를 확인하여  DNS resolver가 생성되었는지 확인합니다. 이를 통해 hostname lookup이 가능해집니다.
![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch1/Picture16.png)
<그림16> DHCP 정보 확인하기

4. 인터넷 게이트웨이의 정보를 확인합니다. 
VCN 정보화면에서 좌측에 Internet Gateways를 클릭하여 확인합니다. 인터넷 게이트웨이는 VCN의 가상 리소스이며 인터넷 액세스를 제공하도록 구성되었습니다. 
![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch1/Picture17.png)
<그림17> IGW 정보 확인하기

### VCN 구성도

위의 작업을 수행하시면,  VCN 10.0.0.0/16내에 각 AD별로 각각의 CIDR을 가진 세 개의 Subnet이 생성되었습니다.<br>
다이어그램으로 확인하시면 다음과 같습니다.
![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch1/VCN.png)
<VCN구성도>

여기까지 Oracle Cloud를 활용하기 위한 네트워크 구성 - VCN 생성 - 에 대해서 살펴보았습니다. <br>위의 과정을 통해 Public Internet을 통해 Oracle Cloud환경에 접속하실 수 있습니다. <br>다음에는 Public Internet을 사용하지 않고, 온 프레미스 데이터센터와 연결할 수 있는 DRG(Dynamic Routing Gateway)의 설정방법을 알아보도록 하겠습니다.
