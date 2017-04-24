+++
description = "오라클의 더커 컨테이너 클라우드 서비스를 생성 메뉴얼 입니다."
tags = ["docker", "oracle", "cloud", "tutorial"]
categories = ["container"]
date = "2017-04-05T19:36:38+09:00"
thumbnailInPost = ""
thumbnailInList = "http://oracloud-kr-teamrepo.github.io/2017/03/occs_new_inst/OracleContainerCloudService.png"
title = "Oracle Container CS 생성 절차"
language = ""
author = "taewan.kim"
+++

Oracle Cloud는 Docker 기반의 컨테이너 서비스를 2016년 11월에 출시하였습니다. 오라클은 2015년 11월에  StackEngine을 인수하였습니다. ([관련 공지](https://www.oracle.com/corporate/acquisitions/stackengine/index.html)). StackEngine사는 2014년 텍사스 오스틴 주에서 설립된 업체로, 엔터프라이즈급 컨테이너 관리 및 자동화 솔루션 개발 업체입니다. StackEngine은 kubernetes(k8s), docker-swarm 그리고 marathon과 같은 container orchestration 도구입니다.

오라클은 2015년 11월에 StackEngine을 인수한 후, 1년 동안 Oracle Public Cloud에 StackEngine을 통합하였습니다. 이 결과물이 [Oracle Container Cloud Service](https://cloud.oracle.com/ko_KR/container) 입니다.

Oracle Container Clod Service를 통해서 Docker 컨테이너의 클러스터 구성, 인스턴스 배포 및 운영, 모니터링을 효과적으로 할 수 있습니다. 이러한 전체 과정에 대하여 웹 기반의 UI와 Rest API를 제공하기 때문에 사용의 편리성과 프로세스 자동화를 효과적으로 구성할 수 있습니다.

![](https://oracloud-kr-teamrepo.github.io/2017/03/occs_new_inst/occs-view.jpg)


본 문서는 Oracle Container Cloud Service를 시작하는 방법을 Step-By-Step 형식으로 소개합니다. 본 문서의 내용을 실습하기 위해서는 Oracle Cloud 계정이 필요합니다. Oracle Cloud 계정이 없다면 다음 문서를 참조하여 계정을 생성하시기 바랍니다.

- [Oracle Cloud 계정 생성하기](http://www.oracloud.kr/post/accont/)

## Oracle Container Cloud Service 생성에 앞서..

본 문서에서는 다음과 같은 내용을 포함합니다.

- Oracle Cloud에 로그인
- Oracle Container Cloud Service(이하 OCCS)의 서비스 콘솔에서 신규 서비스 인스턴스를 생성
- OCCS 인스턴스 대시보드에 로그인하는 과정
- OCCS 인스턴스 삭제

OCCS 신규 서비스 생성에 걸리는 시간은 약 5~7분 정도입니다.

### OCCS 서비스 인스턴스 구성
OCCS 서비스 인스턴스는 1개 Manager Node와 최소 1개 이상의 Worker Node로 구성됩니다. 여기서 Node는 Oracle Cloud의 리눅스 VM입니다. 실제 Docker 컨테이너는 Worker Node에 배포됩니다. 실습에서는 3개의 Worker node와 1개의 Manager Node를 구성할 것입니다. OCCS의 아키텍처는 다음 그림과 같습니다.

![](https://oracloud-kr-teamrepo.github.io/2017/03/occs_new_inst/occs_arhi.jpg)

### Oracle Cloud의 Trial Account 자원 할당
Oracle Cloud의 테스트 계정(Trail 계정)을 신청하면 다음과 같은 자원이 30일 동안 할당됩니다.
(Trial 계정에 할당되는 자원은 변경 가능하며, 다음은 2017년 04월 01일 기준입니다.)

- OCPU: 6 OCPU
  - 1 OCPU는 물리적인 1 core에 해당하며 일반적으로 2 vcpu에 해당
- 메모리
  - 생성 VM의 Shape에 따라 결정
  - 범용 구성: 1 OCPU: 7.5 GB
  - 고용량 메모리 구성: 1 OCPU: 15GB
  - 최소: 45 GB
  - 최대: 90 GB
- 사용량 기준 블록 스토리지 = 500 G
- 사용량 기준 오브젝트 스토리지 = 500 G
- 사용량 기준 아카이브 스토리지 = 500 G
- IP: 5개

Trial 계정을 이용하여 최대 1개의 Manager node와 5개의 Worker node로 구성된 OCCS 인스턴스를 생성할 수 있습니다.

## Oracle Container Cloud Service 생성하기

OCCS 인스턴스를 생성하기에 앞서 cloud.oracle.com에 로그인해야 합니다. <그림 1>은 cloud.oracle.com의 초기화면입니다. 로그인하기 위해 초기 화면의 오른쪽 위에 위치하는 ```Sign In``` 메뉴를 클릭합니다.

- 그림 1. cloud.oracle.com 홈페이지
![](https://oracloud-kr-teamrepo.github.io/2017/03/occs_new_inst/occs-010.jpg)

본 문서는 Trial 계정을 사용하여 진행할 예정입니다. <그림 1>에서 ```Sign In```을 클릭하면 <그림 2>로 이동합니다. <그림 2>에서는 계정 유형과 데이터 센터를 선택하고 "My Services"를 클릭합니다.

<그림 2>에서 선택 가능한 계정 유형은 다음과 같습니다.

- 계정 유형
  - Tradional Cloud Account
  - Cloud Account with Identity Cloud Service(IDCS)
  - Order with Identity Cloud Service(IDCS)

Trial 계정을 사용할 경우에는 "Traditional Cloud Account"를 선택해야 합니다.

한국 사용자가 신청한 Trial 계정은 "Public Cloud Services - US" 데이터 센터에 자원을 할당받습니다. 따라서 "Public Cloud Services - US"를 선택하고 "My Services"를 클릭합니다.

- 그림 2. 로그인을 위한 데이터 센터 선택
![](https://oracloud-kr-teamrepo.github.io/2017/03/occs_new_inst/occs-020.jpg)

<그림 2>에서 "My Services"를 클릭하면, <그림 3>의 도메인을 입력하는 페이지로 이동합니다. 도메인은 앞으로 사용할 클라우드 서비스를 묶는 단위입니다. 시스템 그룹 단위라고 생각할 수 있습니다. 클라우드 계정 생성 시에 지정된 도메인 정보를 입력하고 "실행" 버튼을 클릭합니다.

- 그림 3. 도메인 정보 입력
![](https://oracloud-kr-teamrepo.github.io/2017/03/occs_new_inst/occs-030.jpg)

<그림 3>에서 "실행" 버튼을 클릭하면 <그림 4>와 같이 ID/패스워드를 입력하는 페이지로 이동합니다. ID는 Trial 계정 생성 시에 입력한 Email 주소입니다. ID/패스워드를 입력하고 "사인인" 버튼을 클릭합니다.

- 그림 4. ID(Email) / 패스워드 입력
![](https://oracloud-kr-teamrepo.github.io/2017/03/occs_new_inst/occs-040.png)

<그림 3, 4>에서 도메인, ID 및 패스워드가 정상적으로 입력되면 Oracle Cloud 대시보드 페이지(<그림 5> 참조)로 이동합니다. 이 대시보드에서 현재 사용 가능한 서비스 유형을 확인할 수 있습니다.

- 그림 5. Oracle Cloud 대시보드
![](https://oracloud-kr-teamrepo.github.io/2017/03/occs_new_inst/occs-050.jpg)

신규 서비스 인스턴스를 생성하기 위해서는 대시보드에서 오른쪽 중간에 위치한 "Create Instance" 버튼을 클릭합니다.

- 그림 6. 신규 서비스 생성을 위해  "Create Instance" 버튼 클릭
![](https://oracloud-kr-teamrepo.github.io/2017/03/occs_new_inst/occs-060.jpg)

대시보드에서 "Create Instance"를 클릭하면 <그림 7>과 같이 생성 가능한 서비스 유형이 출력됩니다. 이 목록 중에 "Container"를 선택하고 더블 클릭합니다.

- 그림 7. Oracle Cloud 서비스 목록
![](https://oracloud-kr-teamrepo.github.io/2017/03/occs_new_inst/occs-070.jpg)

계정 생성 후 Container 서비스에 처음 접근하는 경우에는 <그림 8>과 같이 컨테이너 서비스 환영 페이지로 이동합니다. OCCS 콘솔 페이지로 이동하기 위해서 "Go to console" 버튼을 클릭합니다.   

- 그림 8. Container Cloud Service 환영 페이지
![](https://oracloud-kr-teamrepo.github.io/2017/03/occs_new_inst/occs-080.jpg)

<그림 9>는 OCCS 관리 콘솔입니다. 현재 OCCS 인스턴스가 없으므로 별도의 정보를 출력하지 않습니다.

- 그림 9. OCCS 관리 콘솔
![](https://oracloud-kr-teamrepo.github.io/2017/03/occs_new_inst/occs-090.jpg)

새로운 인스턴스를 생성하기 위해서, <그림 10>에서 "인스턴스 생성" 버튼을 클릭합니다.

- 그림 10. OCCS 인스턴스 생성 시작
![](https://oracloud-kr-teamrepo.github.io/2017/03/occs_new_inst/occs-100.jpg)

인스턴스 생성을 위해서 <그림 11>과 같은 정보를 입력합니다. 입력해야 할 정보는 다음과 같이 구분됩니다.

- 서비스 개요
- 관리자 계정 정보
- 인스턴스 구성 정보

서비스 개요에 입력할 정보는 다음과 같습니다.

<table>
<caption>서비스 개요 입력 사항</caption>
<tr>
  <th>항목</th>
  <th>입력 값</th>
  <th>설명</th>
</tr>
<tr>
  <td>Service Name</td>
  <td>demo-container</td>
  <td>인스턴스 이름</td>
</tr>
<tr>
  <td>Service Description</td>
  <td>container for demo</td>
  <td>인스턴스 간단 설명</td>
</tr>
<tr>
  <td>SSH Public Key</td>
  <td>자동생성</td>
  <td>"편집" 버튼 클릭시 팝업 출력 <br> create new key 선택 후 "입력" 클릭</td>
</tr>
</table>

관리자 계정 정보의 입력 정보는 다음과 같습니다.

<table>
<caption>관리자 계정 정보 입력 사항</caption>
<tr>
  <th>항목</th>
  <th>입력 값</th>
  <th>설명</th>
</tr>
<tr>
  <td>Admin Username</td>
  <td>admin</td>
  <td>occs 인스턴스 관리 콘솔 로그인 계정</td>
</tr>
<tr>
  <td>Admin Password</td>
  <td>Welcome1!</td>
  <td>occs 인스턴스 관리 콘솔 로그인 패스워드<br>
      이 항목은 변경 가능합니다.
  </td>
</tr>
<tr>
  <td>Confirm Admin Password</td>
  <td>Welcome1!</td>
  <td>패스워드 확인 정보</td>
</tr>
</table>

인스턴스 구성 정보의 입력 정보는 다음과 같습니다.

<table>
<caption>인스턴스 구성 정보 입력 사항</caption>
<tr>
  <th>항목</th>
  <th>입력 값</th>
  <th>설명</th>
</tr>
<tr>
  <td>Worker node Compute Shape</td>
  <td>"OC3 - 1.0 OCPU, 7.5GB RAM" 선택</td>
  <td>Worker node 배포 VM의 자원 결정</td>
</tr>
<tr>
  <td>Number of Worker nodes</td>
  <td>3</td>
  <td>Worker node를 3개의 VM에 배포
  </td>
</tr>
<tr>
  <td>Woker node data volume size(GB)</td>
  <td>30</td>
  <td>각 Worker node가 설치되는 VM에 추가할 블록 스토리지 용량</td>
</tr>
</table>

모든 정보를 입력한 후 <그림 11>과 같이 ```다음``` 버튼 클릭합니다.

- 그림 11. OCCS 인스턴스 생성 정보 입력
![](https://oracloud-kr-teamrepo.github.io/2017/03/occs_new_inst/occs-110.jpg)

<그림 11>에서 "다음" 버튼을 클릭하면 <그림 12>와 같이 입력 정보 확인 페이지로 이동합니다.
<그림 12>에서 ```생성``` 버튼을 클릭하면 OCCS 인스턴스 생성이 시작됩니다.

- 그림 12. 입력 정보 확인 페이지
![](https://oracloud-kr-teamrepo.github.io/2017/03/occs_new_inst/occs-120.jpg)

<그림 12>에서 ```생성``` 버튼을 클릭하면 <그림 13>과 같이 Oracle Container Cloud Service 콘솔 페이지로 이동합니다. <그림 13>에서 ```Status``` 속성이 ```Creating service ...```인 것을 확인할 수 있습니다.

- 그림 13. OCCS 인스턴스 생성 후 OCCS 콘솔 페이지 이동
![](https://oracloud-kr-teamrepo.github.io/2017/03/occs_new_inst/occs-130.jpg)

인스턴스 생성에는 약 5~7분 정도 걸립니다. 인스턴스 생성이 완료되면 OCCS 콘솔 페이지는 <그림 14>와 같이 변경됩니다.   

- 그림 14. 인스턴스 생성 후 OCCS 콘솔 페이지
![](https://oracloud-kr-teamrepo.github.io/2017/03/occs_new_inst/occs-140.jpg)

<그림 14>에서 인스턴스 명을 클릭하면, <그림 15>와 같이 선택한 OCCS 인스턴스의 상세 정보 출력 페이지로 이동합니다. OCCS 인스턴스 상세 정보 출력 페이지에서 현재 4개의 노드가 만들어졌고, 4OCPU, 30GB 메모리, 140GB 블록 스토리지가 할당된 것을 확인할 수 있습니다.

- 그림 15. OCCS 인스턴스 상세 정보 출력 페이지
![](https://oracloud-kr-teamrepo.github.io/2017/03/occs_new_inst/occs-150.jpg)

<그림 16> 상단의 메뉴 아이콘을 클릭하면, 하위 메뉴가 출력됩니다. 그중에서 "컨테이너 콘솔"을 선택하면, OCCS 인스턴스 모니터링, 관리 및 설정의 모든 기능을 제공하는 관리 콘솔 페이지에 이동할 수 있습니다.

- 그림 16. 컨테이너 인스턴스 관리 페이지 이동 메뉴
![](https://oracloud-kr-teamrepo.github.io/2017/03/occs_new_inst/occs-160.jpg)

컨테이너 인스턴스 관리 페이지에 처음 접근할 경우 <그림 17>과 같이 브라우저의 개인 정보 보호 페이지로 이동합니다. https 접근 시 인증서 불일치로 발생하는 브라우저의 기능입니다. <그림 17>은 크롬 브라우저의 화면입니다. <그림 17>과 같이 상세 링크를 클릭하고 <129.144.152.189("안전하지 않음으로 이동")> 링크를 클릭합니다. IP는 인스턴스에 따라서 변경됩니다.

- 그림 17. 개인 정보 보호 페이지
![](https://oracloud-kr-teamrepo.github.io/2017/03/occs_new_inst/occs-170.jpg)

<그림 17>에서 관련 링크를 클릭하면 OCCS 인스턴스 관리 콘솔의 로그인 페이지로 이동합니다. 이 로그인 페이지에서 <그림 11>에서 입력한 관리자 계정과 패스워드(admin/Welcome1)를 입력하고 "Login" 버튼을 클릭합니다.  

- 그림 18. OCCS 컨테이너 콘솔
![](https://oracloud-kr-teamrepo.github.io/2017/03/occs_new_inst/occs-180.jpg)

<그림 18>로 부터 로그인이 완료되면 <그림 19>과 같이 OCCS 대시보드로 이동합니다. 여기에서 컨테이너 배포, 스택 생성 및 배포 모니터링을 할 수 있습니다. 컨테이너 배포는 다음 문서에서 다루겠습니다.

- 그림 19. OCCS 대시보드
![](https://oracloud-kr-teamrepo.github.io/2017/03/occs_new_inst/occs-190.jpg)

<그림 20>과 같이 Oracle Cloud의 Oracle Container Cloud Service으 콘솔에서  OCCS 인스턴스를 삭제할 수 있습니다. 삭제 대상 인스턴스 명을 확인하고, 오른편의 메뉴 아이콘을 클릭하면 "삭제"가 출력됩니다. 이 메뉴를 이용하여 인스턴스를 삭제할 수 있습니다. 인스턴스 삭제의 소요시간은 약 2-4분 정도 소요됩니다.

- 그림 20. OCCS 인스턴스 삭제
![](https://oracloud-kr-teamrepo.github.io/2017/03/occs_new_inst/occs-200.png)

## 마치며....

지금까지 Oracle Container Cloud Service에 컨테이너 인스턴스를 생성하고, 컨테이너 인스턴스의 관리 콘솔에 접근하는 과정에 대하여 살펴보았습니다. 다음 문서에는 Oracle Container Cloud Service에서 Docker Container를 배포하는 방법에 대하여 알아보겠습니다.

## 참조
-  [Oracle Learning Library - Creating an Oracle Container Cloud Service Instance](http://www.oracle.com/webfolder/technetwork/tutorials/obe/cloud/container_cloud/creating_an_occs_service_instance/creating_occs_instance.html)
- [Using Oracle Container Cloud Service](http://docs.oracle.com/en/cloud/iaas/container-cloud/contu/index.html)
