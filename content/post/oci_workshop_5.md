
+++
date = "2018-09-18T02:20:25+09:00"
description = "OCI(IaaS)의 워크샵 시리즈 입니다."
title = "OCI 따라하기 2-1. Instance 생성하기"
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch5/Picture7.png"
thumbnailInPost = ""
tags = ["OCI", "Oracle Cloud"]
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
	- **Instance 생성하기**<font color=grey>
	- Private Subnet에 생성하기
3. Load Balancer
	- Load Balancer 생성하기
	- Load Balancer를 사용하여 보안강화하기
4. Resource 삭제하기</font><br>

---

### Public Subnet에 생성하기
기존에 생성한 VCN에 있는 3개의 AD(Avilability Domain)중, AD1에 Apache Webserver를 생성해보겠습니다.<br><br>

1. Compute 메뉴로 이동합니다.
Menu - Compute -> Instances
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch5/Picture1.png)<br>
<그림1> Compute 메뉴로 이동<br>

2. 인스턴스를 생성할 Compartment를 확인하고, Create Instance를 클릭합니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch5/Picture2.png)<br>
<그림2> Menu -> Network -> Dynamic Routing Gateway 클릭
팝업창에서 필요한 정보를 입력합니다.

|항목|내용|
|---|---|
|인스턴스 이름|Apce-AD1|
|AD선택|AD1|
|Boot volume|Oracle-provided OS image|
|Image OS|OL7.5|
|Shape type|Virtual Machine|
|Shape|VM.Standard1.1(1 OCPU, 7GB RAM)|
|Image version|Latest|
|SSH keys|Browser에서 key파일을 선택하거나, 컴퓨터에서 사용하는 ssh key값을 복사하여 입력|
|Virtual Cloud Network|Group_Test|
|Subnet|Public Subnet AD1|
<br>**SSH key값과 관련해서는 [ssh 보안키 생성](http://www.oracloud.kr/post/ssh_key/) 포스트 참조**
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch5/Picture3.png)
![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch5/Picture4.png)<br>
<그림3> Instance 정보 입력하기<br><br>
Instsance의 Provision이 끝나면, 해당 인스턴스는 사용할 수 있는 상태가 됩니다.

### Instsance에 Apache 설치하기
1. 인스턴스의 정보에서 먼저 public ip address정보를 확인합니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch5/Picture5.png)<br>
<그림4> Instance의 Public IP 확인하기<br>
2. SSH를 통해 해당 인스턴스에 접속하여, 필요한 패키지를 설치합니다.

```
$ssh -i .ssh/id_rsa opc@129.213.38.175
Enter passphrase for key '.ssh/id_rsa': <- ssh 키에 설정된 password가 있다면 입력
-bash: warning: setlocale: LC_CTYPE: cannot change locale (UTF-8): No such file or directory
[opc@apce-ad1 ~]$ <- Instance에 접속 


[opc@apce-ad1 .ssh]$ sudo yum -y install httpd


...생략...


Installed:
  httpd.x86_64 0:2.4.6-80.0.1.el7_5.1                                                                      

Dependency Installed:
  apr.x86_64 0:1.4.8-3.el7_4.1                             apr-util.x86_64 0:1.5.2-6.0.1.el7               
  httpd-tools.x86_64 0:2.4.6-80.0.1.el7_5.1                mailcap.noarch 0:2.1.41-2.el7                   

Complete!
[opc@apce-ad1 .ssh]$ sudo service httpd start
Redirecting to /bin/systemctl start httpd.service
[opc@apce-ad1 .ssh]$ sudo firewall-cmd --add-service=http
Success
```


### Instance 둘러보기
기본 VNIC이 생성되었으며, 해당 인터페이스에 Private 및 Public IP가 설정되어 있음을 확인할 수 있습니다.<br>
Menu -> Compute -> Instance -> 확인하고자 하는 Instance 선택
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch5/Picture7.png)<br>
<그림5> Instsance의 정보 확인 하기 <br>
Apache서버의 설치가 완료되었습니다. <br><br>
설치한 apache 웹서버의 홈 페이지에 접속할 수 있습니다. 인터넷의 트래픽에 대해 http 및 port 80을 사용하는 Public Security List를 사용할 수 있습니다. 웹 브라우저를 열고 http://<ip_address>를 입력하면, 아래와 같은 화면을 볼 수 있습니다.
<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/ch5/Picture6.png)<br>
<그림6> Apache 서비스 확인<br><br>
**주의사항 : 해당 Instance가 사용하는 Network환경에 변경이 있으면 서비스가 정상적으로 동작하지 않을 수 있습니다.** <br>
	* 기존의 security list에서 port 80에 대한 ingress rule을 삭제하거나 포트 번호를 변경하면, apache 웹 페이지에 엑세스 할 수 없습니다.<br>
	* 기존의 Route table에서 Internet gateway에 대한 규칙을 삭제하면, apache 웹 페이지에 엑세스 할 수 없습니다. 