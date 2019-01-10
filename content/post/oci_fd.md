
+++
date = "2018-12-07T02:20:25+09:00"
description = "OCI(IaaS)"
title = "가용성 향상을 위한 Availability Domain과 Fault Domain"
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/FD/Picture1.jpeg"
thumbnailInPost = ""
tags = ["OCI", "Oracle Cloud", "Availability Domain", "AD", "Fault Domain" ]
categories = ["Oracle Cloud"]
author = "esther.ryu"
language = "bash"  
+++

### 가용성 향상을 위한 Availability Domain과 Fault Domain

하드웨어의 한계점 중에 하나는, 주기적인 유지 보수를 필요로 하고 예기치 않은 문제가 발생할 수 있다는 것입니다. 클라우드 리소스는 기존의 온 프레미스와 동일하게 하드웨어관련 유지보수를 필요로 합니다. 2018년 8월, Oracle Cloud Infrastructure(OCI)는 Virtual Machine(VM)과 Bare Metal 인스턴스에 **“Fault Domain”**을 도입하였습니다. OCI의 Fault Domain은 애플리케이션이 Availability Domain(AD)내에서 하드웨어 장애 혹은 계획된 유지보수 작업으로 인한 서비스 다운타임을 피할 수 있도록 설계되었습니다.
<br>
<br>
	- OCI의 [Availability Domain(AD)](https://docs.cloud.oracle.com/iaas/Content/GSG/Concepts/concepts.htm)이란?<br>
하나의 지역(Region)내에 하나 혹은 그 이상의 데이터 센터로써, 각 AD는 서로 물리적으로 분리되어 있으며, 다른 AD의 장애로부터 자유롭고, 동시에 실패할 확률이 거의 없습니다. AD는 전원이나 냉각 또는 데이터센터 내부에 있는 가용성을 위한 도메인용 네트워크 등을 공유하지 않기 때문에, 하나의 AD에서 장애가 발생하더라도 다른 AD의 가용성에는 영향을 미치지 않습니다. 하나의 Region안에 있는 모든 AD끼리는 낮은 레이턴시의 광대역 네트워크로 연결되어 있습니다.
<br><br>
	- OCI의 [Fault Domain](https://docs.cloud.oracle.com/iaas/Content/General/Concepts/regions.htm#fault)이란?<br>
AD내에 구성된 하드웨어 및 인프라 그룹으로써, 각 AD에는 3개의 Fault Domain이 있습니다. Fault Domain을 사용하면 단일 AD내의 동일한 하드웨어에 위치하지 않도록 인스턴스를 분산배치 할 수 있습니다. 하나의 Fault Domain에 영향을 끼칠 수 있는 하드웨어의 장애 혹은 계획된 유지보수 작업들은 다른 Fault Domain의 인스턴스에 영향을 미치지 않습니다.
<br>
<br>
Sanjay Pillai는 OCI의 [Fault Domain의 이론에 대한 훌륭한 개요](https://blogs.oracle.com/cloud-infrastructure/introducing-fault-domains-for-virtual-machine-and-bare-metal-instances)와 컴퓨트 인스턴스의 배치방법에 대해 설명했습니다. 애플리케이션을 클라우드 인프라에 배포하는 경우, 애플리케이션의 고유한 아키텍처나 Affinity 혹은 Anti-Affinity 속성에 대한 요구사항에 따라 인스턴스가 Fault Domain에 배포되는 방식이 결정됩니다. OCI문서의 [Best Practice for Your Compute Instance](https://docs.cloud.oracle.com/iaas/Content/Compute/References/bestpracticescompute.htm)에서는 두 가지 애플리케이션 시나리오를 가지고 Fault Domain을 설명하고 있습니다.
<br>
<br>
<br>
가용성 측면에서는 클라우드 서비스를 여러 AD에 배포하는 것이 좋습니다. 그러나 애플리케이션 아키텍처에서 동일한 인스턴스 혹은 구성요소가 동일한 AD에 존재해야 하는 경우, 애플리케이션 구성요소마다 적절한 Fault Domain을 선택하면 리소스의 장애로부터 자유로워집니다. 아래에서는 JD Edwards EnterpriseOne (JDE) 및 Oracle E-Business Suite (EBS)의 게시된 아키텍처와 AD 및 Fault Domain이 애플리케이션을 지원하는 방법을 분석합니다. 
### Multi AD(Availability Domain)

하나의 애플리케이션 스택을 여러 개의 AD에 걸쳐서 운영하는 경우, 인스턴스는 하나의 Fault domain 내에서 적절한 선호도(Affinity)에 따라서 배치됩니다. 아래의 JD Edwards EnterpriseOne의 예시를 보면, 지리적으로 분리된 AD는 애플리케이션의 기본적인 이중화를 제공합니다. Fault Domain은 이전의 Oracle E-Business Suite 예제와 다르게 할당됩니다.
<br>
<br>
JD Edwards EnterpriseOne로 들어오는 커넥션은 Oracle Cloud Infrastructure 외부의 DNS를 거쳐서 로드 밸런서를 통해 AD로 라우팅됩니다. 
유입되는 트래픽은 로드 밸런서의 구성된 분배 정책에 따라  애플리케이션 인스턴스들로 라우트됩니다. 다음 예에서 분리된 AD가 애플리케이션의 이중화를 제공하는 것을 확인할 수 있습니다. 프리젠테이션 티어와 미들티어의 모든 호스트는 FAULT-DOMAIN-1에 속합니다. Fault Domain은 하나의 AD에만 속하므로, Availability Domain 1의 FAULT-DOMAIN-1은 동일한 이름을 가지고 있음에도 불구하고 Avilability Domain 2의 FAULT-DOMAIN-1과는 다른 하드웨어 집합입니다. 각 AD의 모든 호스트가 동일한 Fault Domain에 있기 때문에, 지리적 영역의 하드웨어 장애 또는 계획된 유지 보수는 각 AD에 최소한의 영향을 미칩니다. Availability Domain 2의 호스트에 영향을 미치는 하드웨어 이벤트는 Availability Domain 1에는 영향을 미치지 않습니다. 이와 마찬가지로 모든 호스트를 동일한 Fault Domain에 배치하면, 필요한 인프라 유지 관리 활동이 애플리케이션 스택에 미치는 영향을 최소화 할 수 있습니다. 

<br>![](https://oracloud-kr-teamrepo.github.io/2018/FD/Picture2.jpeg)<br>
<그림1> [OCI에 JD Edwards EnterpriseOne 배포하기](https://docs.oracle.com/en/solutions/learn-architecture-deploy-jd-edwards/index.html#GUID-70720E0B-0A03-4784-8DF6-4BF58445C15E)<br>

### Single AD(Availability Domain)

단일 AD에 애플리케이션 스택을 배포할 때 인스턴스를 여러 Fault Domain에 분산시키면, 적절한 Anti-affinity를 제공받을 수 있습니다. 다음 Oracle E-Business Suite 예제에서 Fault Domain이 하드웨어 오류 및 예정된 유지보수 중에 사용자에게 가용성을 어떻게 보장할 수 있는지 살펴보겠습니다.
<br>
<br>
Oracle E-Business Suite로 들어오는 커넥션은 로드 밸런서를 통해 애플리케이션 풀 내에 있는 서버들로 라우팅 됩니다. 애플리케이션에서 보면, 호스트1은 FAULT DOMAIN1이고, 호스트2는 FAULT DOMAIN2입니다. 예기치 않은 하드웨어 장애가 호스트1에 영향을 주는 경우, Oracle E-Business Suite사용자는 호스트2를 통해 접속할 수 있습니다. 호스트1과 호스트2가 동일한 Fault Domain에 존재하면, Oracle E-Business Suite사용자는 서비스에 접근할 수 없을 것입니다. OCI는 각 Fault Domain의 하드웨어 유지보수 시간을 겹치지 않게 운영하고 있습니다. 

<br>![](https://oracloud-kr-teamrepo.github.io/2018/oci_workshop/FD/Picture3.jpeg)<br>
<그림2> [OCI에Oracle E-Business Suite 배포하기](https://docs.oracle.com/en/solutions/deploy-ebusiness-suite-oci/index.html#GUID-8BF5EDC7-2686-48D2-A11C-0595A933AE13)<br>

애플리케이션에서 Fault Domain을 사용하는 방법에 대해 자세히 알고 싶으시면, 다음 솔루션 설명서 링크에 추가 시나리오와 세부 정보가 안내되어 있습니다. Oracle Cloud Infrastructure에서 컴퓨트 인스턴스의 적절한 Fault Domain을 알맞게 선택하면, 하드웨어의 장애나 예정된 유지보수 활동으로 인해 최종 사용자에게 영향을 끼치는 것을 예방할 수 있습니다. 또 다른 관점에서 OCI의 Fault Domain이 왜 중요한지에 대해서는 [Luke Feldman의 기사](https://blogs.oracle.com/cloud-infrastructure/using-availibility-domains-and-fault-domains-to-improve-application-resiliency)를 참조하시기 바랍니다.

### Solution Design Resource
EBS, JDE 및 Siebel CRM에 대한 Oracle 솔루션 문서 링크가 있습니다. 
<br>
- [Learn About Deploying Oracle E-Business Suite on Oracle Cloud Infrastructure](https://docs.oracle.com/en/solutions/deploy-ebusiness-suite-oci/index.html#GUID-5477BF04-E138-4726-812A-74852BFF2FE3)<br>
- [Learn About Deploying Siebel CRM on Oracle Cloud Infrastructure](https://docs.oracle.com/en/solutions/learn-architecture-deploy-siebel/index.html#GUID-2BC7A5B9-032D-4F91-93C3-D833DD3182C2)<br>
- [Learn about Deploying JD Edwards EnterpriseOne on Oracle Cloud Infrastructure](https://docs.oracle.com/en/solutions/learn-architecture-deploy-jd-edwards/index.html#GUID-371D284E-4631-4949-BC01-8BCB9F44FB5F)<br>

본 게시물은 [Lawrence Gabriel의 게시물](https://blogs.oracle.com/cloud-infrastructure/using-availibility-domains-and-fault-domains-to-improve-application-resiliency)을 번역한 것입니다. 