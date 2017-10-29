+++
date = "2017-08-12T22:20:25+09:00"
description = "Oracle Cloud는 2016년 10월에 서브넷을 구성하는 기능을 추가했습니다. Oracle Cloud의 Identity Domain에 여러 서브넷을 구성하고 관리할 수 있습니다. IP Networks를 이용하여 서브넷을 구성하는 방법을 소개하겠습니다."
title = "오라클 클라우드 네트워크 - 서브넷 구성"
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2017/08/ip_networks/loginlist.png"
thumbnailInPost = "https://oracloud-kr-teamrepo.github.io/2017/08/ip_networks/loginpost.jpg"
tags = ["Identity Domain", "IP Networks","Network","subnet","Tutorial","작성중"]
categories = ["IaaS"]
author = "taewan.kim"
language = ""
+++

Oracle Cloud는 2016년 10월에 서브넷을 구성하는 기능을 추가했습니다.[^1] Oracle Cloud의 Identity Domain에 여러 서브넷을 구성하고 관리할 수 있습니다. Oracle Cloud에서는 서브넷을 구성할 때 사설 IP 주소(Prevate IP Address)이 관리 방식이 달라집니다. 사설 IP 주소를 오라클 클라우드의 공통 풀(common pool)에서 관리하는 방식이 Shared Network입니다. Subnet에 할당된 사설 IP 주소를 Identity domain에서 관리하는 방식이 IP Networks입니다. 본 문서에서는 IP Networks를 이용하여 서브넷을 구성하는 방법을 소개하겠습니다.

[^1]: Oracle Compute Cloud의 변경 로그는 다음 링크에서 확인 할 수 있습니다. - [What's New for Oracle Compute Cloud Service](http://docs.oracle.com/en/cloud/iaas/compute-iaas-cloud/stnew/index.html#STNEW-GUID-7678CE8B-EF3B-4B48-A2C1-0ADB39BB05DC)

## Terminology: 용어 정리

본 문서에서는 다음과 같은 오라클 클라우드 용어를 사용하며, 의미는 다음과 같습니다.

| 주요 용어 | 설명 |
| --- | --- |
| IP Network | Identity Domain에 정의된 IP Subnet 입니다. |
| IP Network Exchange | IP Network간에 통신을 가능하게 합니다. <br/> - Oracle Cloud의 Router. |

### IP Network

Oracle Cloud에서 IP Network을 이용하여 Identity Domain에 서브넷을 구성할 수 있습니다.
IP Network이 관리하는 사설 IP 주소(Private IP Address)는 IP Network 생성 시점에 입력된 IP address prefix에 의해 결정됩니다.
IP Network에 생성되는 클라우드 인스턴스(VM, 가상머신)에 할돵되는 사설 IP 주소는 Oracle Cloud의 공통 풀(common pool)에서 관리하지 않습니다.
해당 IP Network에서 관리합니다. IP Network가 아닌 Shared Network에 생성되는 인스턴스에는 Oracle Cloud의 공통 풀(common pool)에서 관리하는  사설 IP 주소가 할당됩니다.

### IP Network exchange

Identity Domain내에 여러 IP Network가 생성되고, IP Network 사이의 통신이 필요하다면, IP Network Exchange가 필요합니다.
IP network exchange은 IP Network 간의 패킷 교환을 중제합니다.

## Oracle Cloud에서 Subnet 구성

Oracle Cloud에서 서브넷을 구성하기 위해서는 IP Networks 기능을 사용해야 합니다IP Network 기능을 통해 사설 네트워크 공간을 구성하고 모든 IP 주소의 사용과 네트워크의 분리 수 있습니다. 또한 IP Exchange 기능은 IP Network 간의 커뮤니케이션이 가능합니다.

![](https://oracloud-kr-teamrepo.github.io/2017/08/ip_networks/subnet01.jpg)



## Shared Network vs. IP Networks


In the shared network, each instance is assigned a private IP address from the common pool of Oracle provided IP addresses.  In an IP network, you can define an IP subnet in your account. The address range of the IP network is defined by the IP address prefix defined when creating the IP network.  The following diagram from Oracle Documentation [1] explains the two network offerings:

the main differences between the two network options are:
The cloud instance IP can change In a shared network after you stop or delete the instance. Therefore, you need to keep your router or any applications connecting to the instance updating the IP after these operations.
IP network allows you to isolate the private network from the public Internet.

CIDR Notation
The slash after an IP Address
