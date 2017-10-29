+++
author = "taewan.kim"
thumbnailInPost = ""
title = "오라클 클라우드에서 window VM 인스턴스 생성"
language = ""
categories = ["Oracle IaaS"]
tags = ["IaaS", "Virtualization","Tutorial", "window"]
date = "2017-03-31T22:42:46+09:00"
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/window.png"
description = "오라클 클라우드의 IaaS(Oracle Compute Cloud Service)를 이용하여 Window 인스턴스를 생성하는 방법을 살펴보겠습니다. Step-By-Step 스타일로 Window VM 인스턴스 절차를 정리합니다."

+++

오라클 클라우드(Oracle Public Cloud, OPC)의 Compute 서비스에서 Window VM 인스턴스를 생성하는 절차를 정리합니다. 오라클 클라우드에서 가상머신을 생성하기 위해서는 Oracle Cloud 계정이 필요합니다. 현재 OPC 계정이 없다면 ["오라클 클라우드 계정 생성하기"](http://taewan.kim/blog/2016/10/06/account_reg_of_oracloud/ )를 먼저 확인하시기 바랍니다.

## Oracle Cloud VM 인스턴스 생성 실습
본 문서는 OS X(Mac) 환경에서 작성되었습니다. 이 문서의 실습에 있어서 윈도우와 "OS X"의 차이는 거의 없습니다. Window VM을 생성한 후, RDP로 Window VM에 접속하는 부분만 차이가 있습니다. 이 부분은 Window와 "OS X"를 구분하여 작성했습니다. 본 문서는 2016년 11월 19일을 기준으로 작성되었습니다.   

오라클 클라우드 VM 생성 실습은 약 10~15분 정도 소요됩니다.

### 실습 절차

오라클 클라우드에 Window VM을 생성하는 실습은 다음과 같은 순서로 진행됩니다.

1. Oracle Cloud 로그인
1. Window VM Instance 생성
1. Security Rule 등록 (for RDP)
1. Mac OS X에서 Window VM 접속하기
1. Window 10에서 Window VM 접속하기
1. Window VM 인스턴스 재시작
1. Window VM 인스턴스 정지 및 제거

----

## Oracle Cloud에서 Window VM 생성

### Step01. Oracle Cloud 로그인

OPC에 인스턴스를 생성하기 위해서 http://cloud.oracle.com 에 로그인해야 합니다.
[그림 1]에서 "sign in"을 클릭하면 리전을 선택하는 페이지가 출력됩니다.

- 그림 1. http://cloud.oracle.com 메인 페이지
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0010.jpg)

로그인에 앞서 인스턴스를 생성할 region을 선택합니다.
**Free trial** 계정의 경우 [그림 2]와 같이 **Public Cloud Services - US** 를 선택해야 합니다.
리전을 선택하고 ```My Services``` 버튼을 클릭하면, 로그인 페이지로 이동하게 됩니다.

- 그림 2. 서비스 리전 선택
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0020.jpg)

OPC 로그인을 위해서 [그림 3]과 [그림 4] 같이 도메인 ID, 계정 ID, 패스워드를 입력합니다.
[그림 3]에서 도메인 ID를 입력하고 ```Go``` 버튼을 클릭하면 계정 ID/패스워드 입력 페이지로 이동합니다.

- 그림 3. 도메인 ID 입력
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0030.jpg)


[그림 4]와 같이 계정 ID/PASSWORD을 입력하고 ```Sign In``` 버튼을 클릭하면 로그인이 완료됩니다.

- 그림 4. 로그인 정보 입력
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0040.jpg)

로그인이 정상적으로 처리되면 [그림 5]와 같이 **Oracle Cloud My Services Dashboard** 가 출력됩니다.

### Step02. Window VM Instance 생성

Dashboard 페이지에서는 [그림 5]와 같이 각 클라우드 서비스로 이동 가능한 블록이 출력됩니다.
VM 인스턴스를 생성하는 Compute Cloud Service Console(이하 compute console)로 이동하기 위해서 ```Create Instance``` 버튼을 클릭합니다.

- 그림 5. Oracle Cloud My Services Dashboard
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0050.jpg)

현재 Cloud 서비스에서 생성 가능한 유형이 출력됩니다. 우리는 Compute Cloud에서 Window VM 을 생성하기 위해 [그림 6]과 같이 ```Compute```를 클릭합니다.

- 그림 6. Compute 서비스 선택
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0060.jpg)

Compute 클라우드 생성에 앞서 사용할 운영체제 이미지를 선택해야 합니다. OPC Compute Cloud에서는 Oracle Image, Private Image 그리고 Marketplaces로 OS 이미지를 관리합니다.

각 OS Images 관리 영역은 다음과 같이 관리 됩니다.

- Oracle Images: 오라클 공식 VM 이미지 (Oracle Enterprise Linux)
- Private Images: 사용자 등록 이미지
- Marketplaces: 마켓 등록 이미지

Marketplaces에는 2016/10/13 현재 335개의 OS 이미지가 등록되어 있습니다. Marketplaces는 [그림 7]과 같이 이동할 수 있습니다.

- 그림 7. Marketplaces 이동
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0070.jpg)

Marketplaces에서 [그림 8]과 같이 "window"를 검색하고 __Window8__ 을 클립 합니다.

- 그림 8. Window 검색 및 Window8 이미지 선택
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0080.jpg)

Marketplaces에서 설치할 이미지를 선택하면 라이센스 관련 동의 팝업 페이지가 출력됩니다. [그림 9]와 같이 두 개 항목을 표시하고 ```install``` 버튼을 클릭합니다.

- 그림 9. Marketplaces 이미지 설치를 위한 라이센스 동의
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0090.jpg)

Window8 이미지를 선택했으면 [그림 10]과 같이 VM 생성을 위해서 ```>``` 버튼을 클릭합니다.

- 그림 10. Window 8 설치 설정 시작
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0100.jpg)

Window VM 설치 첫 번째 단계로 대상 VM의 Shape을 선택합니다. 오라클은 [그림 11]같이 여러가지 Shape를 제공합니다.
[그림 11]과 같이 대상 Shpae를 선택하고, 다음 단계로 넘어가기 위해서 ```>``` 버튼을 클릭합니다.

- 그림 11. Shape 유형 선택
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0110.jpg)

[그림 12]와 같이 설정을 입력합니다.
Public IP Address의 경우 이번에는 자동 생성을 사용할 것입니다. 사전에 고정된 Public IP Address를 사용해야 한다면
사전에 생성된 Public IP Address를 선택할 수 있습니다.
리눅스 VM과 달리 Window VM은 SSH를 사용하지 않습니다. 따라서 ```SSH keys```를 설정할 필요가 없습니다.
윈도우 접속을 위해서 ```RDP```를 Enabled로 선택한 후 Administrator 패스워드를 설정해야 합니다.
설정이 완료되었으면, 다음 단계로 넘어가기 위해서 ```>``` 버튼을 클릭합니다.

- 그림 12. VM Instance 설정 항목
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0120.jpg)

[그림 13]에서는 스토리지 관련 페이지로 Block Storage를 추가할 수 있습니다.
현재 실습에서는 기본 스토리지만을 사용하므로 추가 설정 없이 넘어가겠습니다.
다음 단계로 넘어가기 위해서 ```>``` 버튼을 클릭합니다.

- 그림 13. 스토리지 추가
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0130.jpg)

[그림 14]에서는 Window VM 생성을 위한 전체 설정을 확인하는 페이지 입니다. 설정에 이상이 없다면 ```create```버튼을 클릭하여 Window VM 생성을
시작합니다. VM 생성은 5분 정도 소요됩니다.

- 그림 14. 설정 요약
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0140.jpg)

VM 생성은 Orchestration 탭에서 확인할 수 있습니다.

- 그림 15. Orchestration 탭으로 이동
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0150.jpg)

Window VM이 생성되는 시점에서는 [그림 16]과 같이 3개의 Orchestration이 starting으로 출력됩니다.
VM이 생성되는 시점에서 각 Orchestration의 상태는 __stopped__이거나 __starting__입니다.
Orchestration의 상태는 ```update Orchestration``` 버튼을 클릭하여 업데이트할 수 있습니다.

- 그림 16. Window VM 생성 중 Orchestration 상태
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0160.jpg)

Window VM 생성이 완료되면 3개의 Orchestration의 상태는 [그림 17]과 같이 __Ready__ 상태로 변경됩니다.

- 그림 17. Window VM 생성 완료 시 Orchestration 상태
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0170.jpg)

### Step04. Security Rule 등록 (for RDP)
Oracle Cloud에 생성된 Window에 접근하기 위해서는 RDP(Remote Desktop Protocol) 을 사용해야 합니다.
외부에서 Window VM에 접근하기 위해서 RDP 포트를 외부에 오픈하는 설정을 추가해야 합니다. 현재 실습에 사용하는 컴퓨터가 유동 IP를 사용한다면
"Public Internet"에 RDP 포트를 오픈해야 합니다.

이러한 설정을 하기 위해서는 "Compute Service Console"에서 [그림 18]과 같이 Network 탭으로 이동합니다.

- 그림 18. 네트워크 설정 페이지 이동
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0180.jpg)

RDP 포트에 대한 정의는 "Security Application"에 RDP 라는 이름으로 빌트인으로 만들어져 있습니다.
또한 "Pulbic Internet" 역시  Seciryt IP List에 빌트인으로 만들어져 있습니다.
[그림 19]와 같이 ```create Security Rule```버튼을 클릭하여 "Security Rules"를 만들 수 있습니다.

- 그림 19. Security Rule 생성
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0190.jpg)

RDP를 위한 Security Rule은 [그림 20]과 같이 설정합니다.
참고로 앞 [그림 12]에서 Window VM 생성 시 Security Lists를 default로 설정했었습니다.

- Security List 설정값
  - Name: RDP_SEC_Rule
  - Status: Enabled
  - Security Application: RDP
  - Source Security IP List: public-internet
  - Destination Security List: default

설정이 완료되면 [그림 20]과 같이 ```create``` 버튼을 클릭합니다.

- 그림 20. RDP Security Rule 설정
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0200.jpg)

Security Rule이 정상적으로 생성되면, [그림 21]과 같이 Security Rules 목록이 출력됩니다.

- 그림 21. RDP Security Rule 생성 결과
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0210.jpg)

### Step05. Mac OS X에서 Window VM 접속하기

Mac OS에서 원격 Window에 접속하기 위해서 ```Microsoft Remote Desktop```을 설치해야 합니다. 이 프로그램은 App Store에서 무료로 제공됩니다.
[그림 22]는 App Store에서 ```Microsoft Remote Desktop```을 검색한 결과입니다. 아래 ```설치``` 버튼을 클릭하여 프로그램을 설치할 수 있습니다.

- 그림 22. ```Microsoft Remote Desktop``` 검색 결과 (@App Store)
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0220.jpg)

```Microsoft Remote Desktop```이 설치되면 spotlight에서 [그림 23]과 같이 RDP를 실행할 수 있습니다.

- 그림 23. ```Microsoft Remote Desktop``` 실행
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0230.jpg)


```Microsoft Remote Desktop```로 Window VM에 접속하기 위해서는 접속할 Window의 Public IP를 알아야 합니다.
앞서 생성한 Window VM의 IP는 [그림 24]에서 확인할 수 있습니다.

- 그림 24. Public IP이 확인
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0240.jpg)

```Microsoft Remote Desktop```에서 [그림 25]와 같이 ```+``` 버튼을 클릭하여 접속 정보를 설정합니다.

- 그림 25. ```Microsoft Remote Desktop```의 접속 정보 추가
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0250.jpg)

접속 정보는 [그림 26]과 같이 설정됩니다. PC name에서는 접속할 Window의 Public IP를 입력합니다.
Password 항목에는 [그림 12]에서 설정한 패스워드를 사용합니다. 설정이 완료되면 상단의 닫기 아이콘(red)을 클릭합니다.

- 그림 26. RDP 접속 정보 설정
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0260.jpg)

접속 정보가 설정완료되면 [그림 27]과 같이 출력됩니다. "opc-windiw8" 항목을 더블클릭하여 원격 윈도우 접속이 시작됩니다.

- 그림 27. RDP 설정 결과 및 접속 시작
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0270.jpg)

접속 과정에서 인증서 확인 절차가 [그림 28]과 같이 진행됩니다.

- 그림 28. 접속 중 인증서 확인
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0280.jpg)

[그림 29 ~ 32]는 원격 윈도우에 접속하여 Google에 접속하는 일련의 과정입니다.

- 그림 29. 윈도우 접속 중
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0290.jpg)

- 그림 30. 윈도우 접속 완료
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0300.jpg)

- 그림 31. 원격 윈도우에서 브라우저 실행
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0310.jpg)

- 그림 32. 원격 윈도우에서 구글 접속
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0320.jpg)

### Step06. Window10에서 Window VM 접속하기

윈도우에는 ```Microsoft Remote Desktop```이 기본적으로 설치되어 있습니다. [그림 33~37]은 '''원격 데스크톱 연결```을 실행하고
원격 윈도우에 접속하는 일련의 절차입니다.

- 그림 33. '''원격 데스크톱 연결``` 시작
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0330.jpg)

대상 VM의 Public IP는 [그림 24]에서 확인할 수 있습니다. 이 IP를 [그림 34]에 입력합니다.

- 그림 34. 접속 대상 Public IP 입력 및 연결
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0340.jpg)

[그림 12]에서 설정한  RDP용 패스워드를 [그림 35]에 입력합니다.

- 그림 35. 아이디/패스워드 입력
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0350.jpg)

접속 과정에서 인증서 확인 절차가 [그림 36]과 같이 진행됩니다.

- 그림 36. 접속 중 인증서 확인
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0360.jpg)

접속이 완료되면 [그림 37]과 같은 화면이 출력됩니다. [그림 37]에서는 앞에서 오픈한 브라우저가 화면에 출력되어 있는 것을 확인할 수 있습니다.

- 그림 37. 브라우저 오픈된 상태 확인
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0370.jpg)

### Step07: VM 인스턴스 재시작

OPC에 생성한 Window는 ```Compute Service Console```에서 [그림 38]과 같이 재시작할 수 있습니다.

- 그림 38. 윈도우 VM 재시작
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0380.jpg)

### Step08: VM 인스턴스 정지 및 제거

```Compute Service Console```의 Orchestration 탭에서 생성한 VM을 정지 및 제거할 수 있습니다.
Orchestration을 통해서 VM을 제거할 때 약 5분 정도 소요됩니다.

각 VM은 3개의 Orchestration으로 관리 됩니다. 3개 중에서 master가 대표 Orchestration입니다.
[그림 39]과 같이 win-tw_master를 정지하면 모든 자원은 제거됩니다.

- 그림 39. 윈도우 VM 종료
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0390.jpg)

win-tw_master를 중지하면 [그림 40]과 같이 자원 해제에 대한 정보가 출력됩니다.
```yes```버튼을 클릭하여 중지 작업을 계속합니다.

- 그림 40. Orchestrations 시작 확인
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0400.jpg)

win-tw_master의 종료가 완료되면 [그림 41]과 같이 3개의 orchestration의 상태가 stopped 상태로 변경됩니다.

- 그림 41. Orchestrations 종료 확인 및 인스턴스 확인 이동
![](https://oracloud-kr-teamrepo.github.io/2017/03/win-vm/0410.jpg)

Orchestration이 완료되면 윈도우 인스턴스는 [그림 42]와 같이 삭제됩니다.
전체 인스턴스를 종료하고 삭제의 소요시간은 약 5분입니다.

## 마치며

지금까지 OPC에서 Window VM을 생성, 재시작 그리고 삭제하는 방법에 대하여 살펴보았습니다.
앞으로 Ansible 및 Chef를 OPC에 적용하여 배포 자동화하는 방법에 대하여 살펴보겠습니다.
