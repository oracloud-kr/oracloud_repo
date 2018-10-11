
+++
date = "2018-10-10T02:20:25+09:00"
description = "OCI(IaaS)의 워크샵 시리즈 입니다."
title = "OCI 따라하기 3. Load Balancer 생성하기"
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/diagram.png"
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
	- **Load Balancer 생성하기**
4. Resource 삭제하기
	- [Resource 삭제하기](../oci_workshop_8)

---

### Load Balancer 생성을 위한 Pre-Work
워크로드를 분산하여 처리할 인스턴스를 생성하여야 합니다. <br>
AD1과 AD2에 apache web server를 각각 1개씩 생성합니다.
AD2에 apache web server를 생성하는 것은 다음의 링크를 참조합니다.<br>
>>> [Instance 생성하기](../oci_workshop_5)

### Load Balancer용 Security List 생성하기
1. VCN의 상세화면에서 Security List를 클릭합니다.
Menu - Networking -> VCN -> 해당 VCN의 상세 정보
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture1.png)<br>
<그림1> Security list 확인하기<br>
2. Create Security List를 클릭하여 생성합니다. 
•Ingress rule과 egress rule은 모두 삭제한 뒤에, Load Balancer 생성 후에 다시 업데이트 합니다. 
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture2.png)<br>
<그림2> LB용 Security list 생성하기<br>

### Load Balancer용 Route Table생성하기
1. VCN의 상세정보 화면에서 Route Table로 이동하여,  Create Route Table을 클릭합니다.
![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture3.png)<br>
<그림3> Create Route Table로 이동하기<br><br>
2. Route Table을 생성합니다.

|항목|내용|
|---|---|
|라우팅 테이블 이름|LB-Route-Table|
|CIDR|0.0.0.0/0|
|Target Type|Internet Gateway|
|Target Compartment|RT를 생성할 Compartment선택|
|Target Internet Gateway|사용할 Internet Gateway선택|
![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture4.png)<br>
<그림4> LB용 Route Table 생성하기<br>

### Load Balancer용 subnet 생성하기
1. VCN상세 정보 페이지에서 Subnet으로 이동합니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture5.png)<br>
<그림5> Subnet 메뉴로 이동<br>
2. Subnet을 생성합니다.

|항목|내용|
|---|---|
|서브넷 이름|LB-Subnet-AD1|
|Availability Domain|AD-1|
|CIDR Block|현재 AD안에서 사용되지 않는 CIDR을 할당합니다.(예:0.0.5.0/24)|
|Route Table|LB용으로 생성한 Route Table 선택|
|Subnet Access|Public Subnet선택|
|DNS Resolution|Yes|
|DHCP Options|빈칸으로 남겨둡니다.|
|Security List|LB용으로 생성한 Security List 선택| 
![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture6-1.png)
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture6-2.png)<br>
<그림6> LB용 Subnet 생성하기<br>
3. AD2를 위한 Subnet을 생성합니다.

|항목|내용|
|---|---|
|서브넷 이름|LB-Subnet-AD2|
|Availability Domain|AD-2|
|CIDR Block|현재 AD안에서 사용되지 않는 CIDR을 할당합니다.(예:0.0.6.0/24)|
|Route Table|LB용으로 생성한 Route Table 선택|
|Subnet Access|Public Subnet선택|
|DNS Resolution|Yes|
|DHCP Options|빈칸으로 남겨둡니다.|
|Security List|LB용으로 생성한 Security List 선택| 
<br>4.Load Balancer용 Subnet이 생성되었음을 확인합니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture7.png)<br>
<그림7> Load Balancer 메뉴로 이동<br>

### Load Balancer 생성하기
1. Menu -> Networking -> Load Balancer로 이동합니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture8.png)<br>
<그림8> Load Balancer 메뉴로 이동<br>
2. Load Balancer를 생성합니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture9.png)<br>
<그림9> Create Load Balancer 클릭

|항목|내용|
|---|---|
|로드밸런서 이름|Apache-LB|
|Shape|요구되는 Bandwidth에 맞게 선택(예: 100Mbps/400Mbps/8000Mbps)|
|VCN|LB를 생성할 VCN선택|
|Visibility|Create Public Load Balancer 선택|
|Subnet 1|첫번째 Load Balancer가 사용할 AD선택(예:LB-Subnet-AD1)|
|Subnet 2|Stnadby Load Balancer가 사용할 AD선택|
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture10-1.png)<br>
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture10-2.png)<br>
<그림10> Load Balancer 생성하기<br>
다음과 같이 Load Balancer가 생성되었음을 확인할 수 있습니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture11.png)<br>
<그림11> Load Balancer 생성 완료<br>

### Backend Set 생성하기
Backend set에는 Load Balancer가 트래픽을 보내게 할 Backend server에 대한 정보가 들어 있습니다. 여기서는 Apache 웹 서버가 됩니다.

1. Load Balancer의 상세정보에서 Create Backend Set을 클릭합니다. 
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture12.png)<br>
<그림12> Create Backend Set 클릭<br>
2. Apache 서버들을 위한 Backend Set을 생성합니다.<br>
Policy에는 세 가지 정책 중 하나를 선택할 수 있습니다.<br>
-	Round robin은 트래픽을 Backend에 순차적으로 배분합니다.
-	Least Connection은 연결 수가 가장 적은 Backend로 트래픽을 전송합니다.
-	IP Hash는 IP address를 해싱 키로 사용하여 트래픽을 Backend에 라우팅합니다.<br>
Use Session Persistence를 사용하면, 클라이언트는 쿠키를 사용하여 동일한 Backend 서버에 연결할 수 있습니다. 
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture13.png)<br>
<그림13> Backend Set 생성하기<br>
생성되었음을 확인할 수 있습니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture14.png)<br>
<그림14> Backend Set 생성하기<br>

### Apache Backend 추가하기
1. Backend Set의 상세정보를 확인합니다. 
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture15.png)<br>
<그림15> Backend Set의 상세정보<br>
2. Edit Backends 를 클릭합니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture16.png)<br>
<그림16> Backends 클릭<br>
3. 필요한 Backends서버를 추가합니다.<br>
• Help me create proper security list rules에 반드시 체크 되어 있어야 합니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture17.png)<br>
<그림17> Backends 추가하기<br>
• 인스턴스의 ocid 정보를 사용하여 입력합니다. ocid정보는 View instance를 클릭하면 인스턴스 정보 화면으로 이동하여 해당 정보를 확인할 수 있습니다. 
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture17-1.png)<br>
4. Backends서버들이 추가되었으며, 추가될 Security List Rule들을 확인할 수 있습니다. 아래와 같은 팝업창이 뜨면, 추가될 Security List Rule를 확인하고 Create rule를 클릭합니다. 
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture18.png)<br>
<그림18> Backends에 Security list 추가하기<br>
"Show the egress/ingress rules that will be created"를 클릭하면 추가될 리스트의 상세정보들을 확인할 수 있습니다. Load Balancer의 Security list에 대해 추가된 rule은 포트 80에 대하여 두 apache서버의 서브넷에 대한 트래픽을 허용하는 것입니다. 
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture18-1.png)<br>

### Load Balancer의 Lisetner 생성하기
Listener를 통해 Load Balancer는 Internet gateway에서 인터넷 트래픽을 감지하여, 트래픽을 전송합니다.

1. Load Balancer의 상세 페이지에서 Listener를 클릭합니다.
Menu -> Networking -> Load Balancers 
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture19.png)<br>
<그림19> Listener 메뉴로 이동<br>
2. Create Listener를 클릭하여 생성합니다. 
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture20.png)<br>
<그림20> Creat Listener 클릭

|항목|내용|
|---|---|
|이름|Apache-LB-Listener|
|Protocol|HTTP|
|Port|80|
|SSL|체크하지 않습니다.|
|Timeout|입력하지 않습니다.|
|Backend set|Apache-Backend-Set|
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture21.png)<br>
<그림21> Listener 생성하기<br>
3. 정상적으로 Listener가 생성됨을 확인합니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture22.png)<br>
<그림22> Listener 생성 완료<br>

### Load Balancer에 Internet연결 허용하기
이제 VCN에 Load Balancer를 추가했으므로, VCN에 대한 Security list를 확인하고 internet 연결을 허용하기 위한 ingress rule를 추가합니다.

1. VCN의 상세 메뉴에서 Security Lists를 들어가서, Load Balancer용 Security List를 클릭합니다. 
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture23.png)<br>
<그림23> Load Balancer용 Security list 확인 <br>
2. 해당 Load Balancer에 생성되어 있는 Rule을 수정합니다.<br>
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture24.png)<br>
<그림24> Load Balancer용 Security list의 rule 변경하기 <br>
Internet으로부터의 접속을 허용하기 위해서 다음과 같은 Rule을 추가합니다.

|항목|내용|
|---|---|
|Source CIDR|0.0.0.0/0|
|IP Protocol|TCP|
|Source Port Range|ALL|
|Destination Port Range|80|
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture24.png)<br>
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture24-1.png)<br>
<그림24> Load Balancer용 Security list의 rule 변경하기 <br>

### Apache Instance의  Internet 연결 삭제 하기
이제는 Apache 인스턴스 들에 대해서 Loac Balancer가 트래픽을 라우팅 할 수 있으므로, 개별 인스턴스에 대한 Internet 연결을 해제하고 확인해봅니다. 

1. Apache인스턴스의 Public IP address를 통해 외부에서 접속이 되는지 확인합니다.
2. Apache 인스턴스가 사용하는 default security list에서 ingress rule을 삭제합니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture25.png)<br>
<그림25> VPN의 Security list 확인 <br>
0.0.0.0/0대역의 80포트에 대한 rule을 삭제합니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture25-1.png)<br>
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture26.png)<br>
<그림26> VPN의 Security list의 rule 변경하기 <br>
3. 첫번째 Apache서버에 접속하여 아래와 같이 수행합니다.

```sh
[opc@apache-ad1 ~]$ sudo su
[root@apache-ad1 opc]# echo 'Apache-AD1' >> /var/www/html/index.html
[root@apache-ad1 opc]# exit
```
  4.두번째 Apache 서버에 접속하여 아래와 같이 수행합니다.<br>
```sh
[opc@apache-ad2 ~]$ sudo su
[root@apache-ad2 opc]# echo 'Apache-AD2' >> /var/www/html/index.html
[root@apache-ad2 opc]# exit
```
<br>
5.Load Balancer의 IP address를 이용하여 Apache 웹 서버에 접속할 수 있는지 확인합니다.<br>
Menu -> Networking -> Load Balancer
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture27.png)<br>
<그림27> Load Balancer의 IP 확인하기 <br>
• 웹 브라우저를 열고 Load Balancer의 Public IP address를 입력합니다.<br>
화면에서 Apache-AD1이라는 메세지를 확인할 수 있습니다. 해당 웹 페이지를 새로고침 하면, Load Balancer가 Round robin으로 설정되어 있으므로 Apache-AD2라는 메세지를 확인할 수 있습니다.

### Backend 서버 보안 강화하기
이제 Load Balancer가 인터넷의 접근 포인트이므로 Backend 서버를 보호하기 위한 규칙을 업데이트 할 수 있습니다. Public subnet의 security list와  Route table를 업데이트하여  Backend 서버로 접근하는 트래픽을 제한합니다.

1. VCN의 상세 정보에서 Route Table을 클릭합니다.
Menu -> Networking -> VCN -> 해당 VCN
좌측의 메뉴에서  Security List를 클릭하고  VCN에서 사용하는 default security list를 선택합니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture28.png)<br>
<그림28> Default security list 정보 확인 <br>
2. Edit All Rules를 클릭하여, 아래의 Ingress Rule을 삭제합니다.

|Source CIDR|IP Protocol|Destination Port Range|
|---|---|---|
|0.0.0.0/0|TCP|22|
|0.0.0.0/0|ICMP|3, 4|
|10.0.0.0/16|ICMP|3| 
모든  Egress Rule도 삭제하고, Save Security List Rule을 클릭합니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture29-1.png)
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture29-2.png)
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/Picture29-3.png)<br>
<그림29> Security rule 수정하기 <br>
이후, Load Balancer를 통해 정상적으로 Apache 웹 서버에 접속이 가능한지 확인합니다.

### Oracle Cloud 구성도
여기까지 수행하였다면, 클라우드 환경은 아래의 그림과 같이 구성되었습니다.<br>
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch7/diagram.png)<br>
OCI VCN과 Network 및 Security resource를 사용하면 가용성이 높고 안전한 솔루션을 쉽게 구현할 수 있습니다. <br>
여러 개의 데이터 센터에 걸쳐 HA를 제공하는 AD 2 개, AD간에 트래픽을 분산 및 라우트하는 Load Balancer, 서브넷에 안전하게 액세스 할 수있는 Security List 및 Route Rule, 인터넷에 연결하는 Internet Gateway 및 온 - 프레미스 네트워크에 연결하는 Dynamic Routing Gateway를 포함하는 클라우드 환경을 구성하였습니다.  

