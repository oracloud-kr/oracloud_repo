+++
date = "2018-09-19T12:20:25+09:00"
description = "OCI(IaaS)의 워크샵 시리즈 입니다."
title = "OCI 따라하기 시리즈"
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/diagram.png"
thumbnailInPost = ""
tags = ["OCI", "Oracle Cloud", "VCN", "DRG", "Security List", "Load Balancer"]
categories = ["Oracle Cloud"]
author = "esther.ryu"
language = "bash"  
+++

Oracle Cloud에는 VM만을 지원하는 OCI-Classic과, VM과 Bare Metal을 함께 지원하는 OCI-Oracle Cloud Infrastructure 두 가지가 있습니다. 
**"OCI 따라하기"** 시리즈에서는 Oracle Cloud를 처음 접하시는 분들이 쉽게 이해하실 수 있도록, OCI에 기본적인 Network환경을 구성하는 것부터 Instance를 생성하고 AD간의 Load Balancer 설정하는 방법 등을 포함하고 있습니다.

Oracle Cloud에서 Trial Account를 생성하시면, $300 상당의 Credit을 무료로 사용하실 수 있습니다.<br>
Oracle Cloud의 Trial Account를 생성하는 방법은 [Oracle Cloud Trial 신청: $300 Credit](http://www.oracloud.kr/post/oracle_cloud_reg/)을 참조하시기 바랍니다.


### OCI IaaS 따라하기 목차 
* 컨텐츠는 순차적으로 업데이트 됩니다. (각 상세 목차를 클릭하시면 해당 페이지로 연결됩니다.)

1. OCI Network 
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
	- Resource 삭제하기

### 구축 다이어그램
위의 단계를 수행하시면 아래와 같은 환경을 구축하실 수 있습니다.
![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/diagram.png)

