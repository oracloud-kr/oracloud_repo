
+++
date = "2018-09-25T02:20:25+09:00"
description = "OCI(IaaS)의 워크샵 시리즈 입니다."
title = "OCI 따라하기 2-2. Private Subnet에 Instance 생성하기"
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch6/diagram.png"
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
	- **Private Subnet에 생성하기**
3. Load Balancer
	- [Load Balancer 생성하기](../oci_workshop_7)
4. Resource 삭제하기
	- [Resource 삭제하기](../oci_workshop_8)

---

### Private Subnet에 Instance 생성하기
1. Compute 메뉴로 이동합니다.
Menu -> Compute -> Instance -> Create Instance 클릭
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch5/Picture1.png)<br>
<그림1> Compute 메뉴로 이동<br>
적절한 Compartment를 선택하고 Create Instance를 클릭합니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch6/Picture1.png)<br>
<그림2> Create Instsance 클릭<br>
팝업창에서 필요한 정보를 입력합니다.

|항목|내용|
|---|---|
|인스턴스 이름|InstancePrivate-AD1|
|AD선택|AD1|
|Boot volume|Oracle-provided OS image|
|Image OS|OL7.5|
|Shape type|Virtual Machine|
|Shape|VM.Standard1.1(1 OCPU, 7GB RAM)|
|Image version|Latest|
|SSH keys|Browser에서 key파일을 선택하거나, 컴퓨터에서 사용하는 ssh key값을 복사하여 입력|
|Virtual Cloud Network|VCN선택|
|Subnet|PrivateSubnet-AD1|
네트워크 항목에서 VCN의 Private subnet을 선택하고 Create Instance를 클릭합니다.
<br>**SSH key값과 관련해서는 [ssh 보안키 생성](http://www.oracloud.kr/post/ssh_key/) 포스트 참조**
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch6/Picture2.png)
![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch6/Picture2-1.png)<br>
<그림3> Instance 정보 입력하기<br><br>
**생성된 인스턴스에는 Private IP만 존재하고 Public IP가 없습니다.**
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch6/Picture3.png)<br>
<그림4> Instance의 Public IP 확인하기<br>
Private subnet 내에 인스턴스를 생성하였으므로 인터넷에서 해당 인스턴스에 접속할 수 없습니다. <br>
DRG를 사용하면 고객 온프레미스 환경(혹은 사내 네트워크)에서 Private subnet에 접근하거나 Bastion서비스를 만들 수 있습니다. Bastion 서비스는 프록시 서버일 수 있으며 악의적인 네트워크 공격에 견딜 수 있는 방어선을 제공합니다.<br>
아래의 그림과 같이 Bastion서버를 구성한 경우, Apache 웹 서버 인스턴스에서 Private 인스턴스에 접근할 수 있습니다. 이 경우, 웹 서버로 SSH를 수행한 다음  Private IP를 사용하여 Private 인스턴스로 다시 SSH를 수행하여야 합니다. 이렇게 하려면, 개인 인스턴스의 개인 키(privkey)를 Apache 웹 서버 인스턴스에 복사해야 합니다. 거기에서 SSH를 사용하여 접속하는 것은 2단계 인증을 필요로 하므로 더 우수한 보안을 제공합니다.<Br>

### 클라우드 구성도
여기까지 수행하시면, 아래와 같이 Public subnet내에 인스턴스가 하나 존재하고, Private subnet내에도 인스턴스를 생성하였습니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch6/diagram2.png)<br>

### Private Instance에 접속하기

위와 같은 환경에서 Private subnet에 있는 인스턴스(B)에 접속하기 위해서는 Public subnet에 있는 인스턴스(A)에 접속하여, 이중 인증을 받아야 합니다.

최초에 인스턴스를 생성할 때에 SSH키 값을 입력하게 되는데, A와 B 인스턴스 모두 동일한 키 값을 가지고 있습니다. 그러므로 B에 접속하기 위해서는 A서버에 private key값이 필요합니다.

1. Private key를 A에 복사합니다. (혹은 해당 키 파일의 내용을 복사하여, 동일한 내용으로 파일을 생성하여도 됩니다.)
```sh
$ cd .ssh
$scp -i ./id_rsa.pub ./id_rsa opc@129.213.38.175:/
```

2. A에 접속하여, private key파일의 권한을 700으로 변경
```sh
$ssh opc@129.213.38.175
[opc@apce-ad1 ~]$ chmod 700 id_rsa
```
3. A에서 B로 접속
```sh
[opc@apce-ad1 ~]$ ssh -i ServerAliveInterval=60 -i id_rsa opc@10.0.4.2
Warning: Identity file ServerAliveInterval=60 not accessible: No such file or directory.
Enter passphrase for key 'id_rsa': 
[opc@istanceprivate-ad1 ~]$
```

**접속이 되지 않는다면? --> Security List를 확인합니다.**
현재 A의 private ip는 10.0.0.2이고 B의 private ip는 10.0.4.2이므로,  private subnet의 security list에서 Ingress rule이 10.0.0.0/16을 허용하여야 합니다. 
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch6/Picture4.png)<br>
<그림5> Security List 확인하기<br>

### Oracle Cloud 구성도
여기까지 수행하였다면, 현재까지 완성한 부분은 다음과 같습니다.<br>
	- 오라클의 네트워크 ( Public및  Private subnet 생성)<br>
	- Subnet의 트래픽을 제어하기 위해 Security list 구성 (Private인스턴스에 접근하기)<br>
	- Routing Table과 규칙을 구성하여 Internet GW를 통해 통신<br>
	- DRG를 통해 온 프레미스 환경에서 접속할 수 있습니다. 
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch6/diagram2.png)<br>
