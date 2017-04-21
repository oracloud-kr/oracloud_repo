+++
tags = ["IaaS", "VM", "Tutorial"]
thumbnailInPost = ""
description = "오라클 클라우드의 IaaS(Oracle Compute Cloud Service)를 이용하여 인스턴스를 생성하는 방법을 살펴보겠습니다. Step-By-Step 스타일로 VM 인스턴스 절차를 정리합니다. "
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/Oracle-IaaS.jpg"
date = "2017-03-05T22:20:25+09:00"
language = ""
categories = ["Oracle Cloud", "IaaS"]
title = "오라클 IaaS를 이용한 VM 인스턴스 생성"
author = "taewan.kim"

+++

오라클 클라우드의 IaaS 서비스에서 Ubuntu 14.04를 VM 인스턴스로 만드는 절차를 정리합니다. 오라클 클라우드에서 가상머신을 생성하기 위해서는 Oracle Cloud의 계정이 필요합니다. 계정을 생성하는 방법에 대해서는 ["오라클 클라우드 계정 생성하기"](/post/accont/ ) 를 참조하시기 바랍니다.

## Oracle Cloud VM 인스턴스 생성 실습
본 문서는 OS X(Mac) 환경에서 작성되었습니다. 이 문서를 윈도우에서 실습하실 경우에는 PUTTY와 같은 Terminal을 사용하는 부분과 SSH-KEYGEN 을 사용하는 절차가 달라질 것입니다. 이 부분에 대해서는 별도 문서로 정리하겠습니다.

오라클 클라우드 VM 생성 실습은 약 10~15분 정도 소요됩니다.

### 실습 절차
오라클 클라우드에 VM을 생성하는 실습은 다음과 같은 순서로 진행되며,
각 단계를 "Step by Step" 형태로 설명하겠습니다.

1. 로컬 컴퓨터 ssh key 생성
1. Oracle Cloud 로그인
1. Compute Cloud Service Console
1. Network 설정
  - Public Key 등록
  - Public IP 생성
1. VM Instance 생성
1. Security Rule 등록 (for SSH)
1. VM 로그인 및 nginx 설치
1. Security 등록 (for 80포트) 및 브라우저 접근
1. VM 인스턴스 정지

## Oracle Cloud VM 생성

### Step01: 로컬 컴퓨터 ssh key 생성

Oracle Public Cloud(이하 OPC)에 가상 머신을 생성하면,
사용자는 자신의 컴퓨터 터미널 프로그램을 사용하여 SSH로 VM 인스턴스(이하 인스턴스)에 접근하게 됩니다.
OPC에 인스턴스를 생성하는 과정에는 접근할 컴퓨터의 SSH 공개키를 등록하는 단계를 포함합니다.
이렇게 OPC에 등록된 ssh 공개키는 접속할 컴퓨터를 인증하는 용도로 사용됩니다.

리눅스와 OS X에서 OPC의 인스턴스에 접속할 때 사용되는 암호화 키는 다음과 같은 명령으로 생성됩니다.

```bash
ssh-keygen -t rsa
```

[그림 1]은 ssh-keygen 명령으로 암호화 키를 생성하는 스크린 샷입니다.
ssh-keygen 명령으로 암호화 키를 생성할 때 기본 파일 생성 디렉터리와 파일명은 다음과 같습니다.

- 기본 디렉터리: <USER_HOME>/.ssh/
- 기본 파일명: id_rsa / id_rsa.pub

```bash
taewan@.ssh $pwd
/Users/taewan/.ssh
taewan@.ssh $ls -al
drwx------   24 taewan  staff   816 10 11 15:15 .
drwxr-xr-x+ 147 taewan  staff  4998 10 11 15:15 ..
-rw-------    1 taewan  staff  1675 10 11 13:58 id_rsa
-rw-------    1 taewan  staff   415 10 11 13:58 id_rsa.pub
taewan@.ssh $
```

- 그림 1. ssh-keygen 명령을 이용한 암호화키 생성
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step010.jpg)

ssh-keygen 명령으로 생성된 파일 중 확장자가 pub인 파일이 공개키입니다.
[그림 1]에서 생성한 id_rsa.pub에 포함된 내용이 OPC에 등록되며, 이 정보는 향후 해당 인스턴스의 운영체제 접속 시점에 사용됩니다.
id_ras.pub에 저장된 내용은 [그림 2]와 같습니다.

- 그림 2. ssh-keygen으로 생성한 공개키
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step020.jpg)

### Step02: Oracle Cloud 로그인

OPC에 인스턴스를 생성하기 위해서 http://cloud.oracle.com 에 로그인해야 합니다.
[그림 3]에서 "sign in"을 클릭하면 리전을 선택하는 페이지가 출력됩니다.

- 그림 3. http://cloud.oracle.com 메인 페이지
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step030.jpg)

로그인에 앞서 인스턴스를 생성할 region을 선택합니다.
**Free trial** 계정의 경우 [그림 4]와 같이 **Public Cloud Services - US** 를 선택해야 합니다.
리전을 선택하고 ```My Services``` 버튼을 클릭하면, 로그인 페이지로 이동하게 됩니다.

- 그림 4. http://cloud.oracle.com 메인 페이지
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step040.jpg)

OPC 로그인을 위해서 [그림 5]와 [그림 6]과 같이 도메인 ID, 계정 ID, 패스워드를 입력합니다.
[그림 5]에서 도메인 ID를 입력하고 ```실행``` 버튼을 클릭하면 계정 ID/패스워드 입력 페이지로 이동합니다.

- 그림 5. 도메인 ID 입력
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step050.jpg)

[그림 6]과 같이 계정 ID/PASSWORD을 입력하고 ```사인인``` 버튼을 클릭하면 로그인이 수행됩니다.

- 그림 6. 로그인 정보 입력
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step060.jpg)

로그인이 정상적으로 처리되면 [그림 7]과 같은 **Oracle Cloud My Services Dashboard** 가 출력됩니다.

### Step03: compute Cloud Service Console 이동
Dashboard 페이지에서는 [그림 7]과 같이 각 클라우드 서비스로 이동 가능한 블록이 출력됩니다.
VM 인스턴스를 생성하는 **Compute Cloud Service Console(이하 compute console)** 로 이동하기 위해서 대시보드의 왼쪽 위의 메뉴 아이콘을 클릭합니다.

- 그림 7. Dashboard 페이지의 메뉴 아이콘 선택
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step070.jpg)

[그림 7]에서 메뉴 아이콘을 클릭하면 왼편에 [그림 8]과 같이 각 서비스 콘솔과
모니터링 콘솔 페이지 이동을 지원하는 메뉴가 출력됩니다.
출력된 메뉴 중에서 **compute** 를 클릭합니다.  

- 그림 8. Compute 서비스 선택
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step080.jpg)

### Step04: 네트워크 설정

Comput Console에서 네트워크 탭을 선택하여 [그림 9]와 같이 네트워크 설정 페이지로 이동합니다.
네트워크 설정 페이지에서는 Security Rule, Security List, Security Application, Security IP List, IP Reservation, SSH Public Key를 설정할 수 있습니다.
다음은 설정 메뉴에 대한 요약입니다.  

- Security Lists: 보안 대상 서버 그룹 설정, 일종의 방화벽 구성
- Security Rules: 보안 대상 서버 그룹에 설정할 보안 룰 정의
- Security Applications: Security Rule에서 사용할 포트 정보 등록 및 관리
- Security IP Lists: OPC 계정이 관리하지 않는 외부 시스템의 IP 목록 관리
- IP Reservations: Public IP 생성 및 관리
- SSH Public Keys: SSH Public Key 등록

네트워크 설정 관련해서는 별도의 문서로 정리하겠습니다.
본 문서에서는 IP Reservation과 SSH Public Key만을 사용할 것입니다.


- 그림 9. Compute Cloud Service Console
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step090.jpg)

#### SSH Public Key 등록

앞에서 생성한 SSH 공개키(그림 2 참조)의 내용을 등록해야 합니다. 여기에서 등록한 public 키는 VM 생성 시 인스턴스에 할당되고,
앞으로 터미널을 통해서 SSH를 이용하여 시스템에 접근할 때 사용됩니다.

네트워크 페이지에서 SSH 공개키[그림 10]와 같이 **SSH Public Keys** 를 선택하여 SSH Public Key 등록 페이지로 이동합니다.

- 그림 10. SSH Public Keys 페이지 이동
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step100.jpg)


[그림 11]과 같이 ```Add SSH Public Key```을 클릭하면 [그림 12]와 같이 SSH Public Key 등록 폼이 출력됩니다.

- 그림 11. ```SSH Public Keys``` 추가 버튼 클릭
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step110.jpg)

[그림 2]에서 생성한 공개키 파일의 내용을 복사하여 [그림 12]의 등록 폼에 입력합니다.
공개키 이름을 입력한 후 ```Add``` 버튼을 클릭하면 공개키 등록 절차가 완료됩니다.  

- 그림 12. SSH Public 등록 폼
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step120.jpg)

공개키 등록이 완료되면 [그림 13]에서 공개키가 등록된 결과를 확인할 수 있습니다.
이제 Public IP를 생성하기 위해서 ```IP Reservation``` 메뉴를 클릭합니다.

- 그림 13. 공개 키 등록 확인 및 IP Reservation 페이지로 이동
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step130.jpg)

#### Public IP 생성

VM을 관리함에 있어서 Public IP를 예약하고 재사용하는 것이 편리합니다.
Public IP를 예약하지 않을 경우 VM을 재시작할 때마다 새로운 Public IP가 적용됩니다.
VM 인스턴스를 효과적으로 관리하기 위해서는 Public IP를 사전에 생성하고, 인스턴스에 할당해야 합니다.

[그림 14]와 같이 ```Create IP Reservation``` 버튼을 클릭하면 공개 IP 설정 폼이 출력됩니다.

- 그림 14. Public IP를 생성하기 위한 ```Create IP Reservation``` 클릭.
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step140.jpg)

생성할 공개 IP에 할당될 IP 명을 입력하고 ```Create``` 버튼을 클릭하면 IP 생성은 완료됩니다.

- 그림 15. Public IP 명 설정 및 생성
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step150.jpg)

공개 IP 생성이 완료되면, [그림 16]과 같이 생성된 공개 IP를 목록에서 확인할 수 있습니다.
이제 VM Instance를 생성하기 위해서 상단의 **Instances** 탭을 클릭합니다.

- 그림 16. 공개 IP 목록
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step160.jpg)

### Step04: VM 인스턴스 생성

이제 ```Create Instance``` 버튼을 클릭하여 인스턴스 생성 페이지로 이동합니다.

- 그림 17. ```Create Instance``` 버튼 클릭
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step170.jpg)

Boot Images 선택 페이지로 이동합니다. OPC는 3가지의 유형의 Images를 제공합니다.

- Oracle Images: 오라클 공식 VM 이미지 (Oracle Enterprise Linux)
- Private Images: 사용자 등록 이미지
- Marketplaces: 마켓 등록 이미지

Ubuntu Boot Image를 선택하기 위해서 왼쪽 메뉴에서 "Marketplaces"를 클릭합니다.

- 그림 18. Marketplaces 이동 메뉴
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step180.jpg)

Marketplaces에는 2016/10/13 현재 335개의 boot 이미지가 등록되어 있습니다.
필요한 이미지는 [그림 19]와 같이 검색 가능합니다.

- 그림 19. Marketplaces의 검색 기능
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step190.jpg)

[그림 20]과 같이 검색어로 "ubuntu"를 입력하면 Ubuntu로 tagging 된 모든 이미지가 조회됩니다.

- 그림 20. Marketplaces의 검색 "Ubuntu"
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step200.jpg)

검색된 Ubuntu 이미지 중에 "Ubuntu Server 14.04 LTS amd64"를 선택하고 [그림 21]과 같이 오른쪽 위의 ```>``` 버튼을 클릭합니다.

- 그림 21. 설치 Boot 이미지 선택
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step210.jpg)

Marketplaces에서 이미지를 선택하면 현재 계정의 Compute Cloud에 다운로드 됩니다. 이와 관련된 알림과 승인 창이 출력됩니다.
[그림 22]와 같이 승인 체크 상자에 체크하고 "Install"을 선택합니다.

- 그림 22. 대상 이미지 다운로드 및 승인 확인 알림
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step220.jpg)

다음으로는 인스턴스의 Shape을 선택하는 페이지로 이동합니다. [그림 23]과 같이 현재 3개의 Shape을 제공합니다.
"General Purpose"를 선택하고 오른쪽 위의 ```>``` 버튼을 클릭합니다.
"General Purpose"로 생성된 인스턴스에는 1 ocpu / memory 7.5GB의 자원이 할당됩니다.
1 OCPU는 아마존 2 vCPU와 같은 용량을 제공합니다.

- 그림 23. 설치할 Compute의 Shape 선택
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step240.jpg)

[그림 24]와 같은 기본 폼이 출력됩니다.
- 그림 24. VM Instance 생성 폼
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step250.jpg)

[그림 25]와 같이 인스턴스 명, host 명을 입력합니다. 추가로 Public IP Address에 **Persistent Public IP Reservation** 을 선택하면 앞에서 생성한 Public IP를 선택하는 입력 폼이 추가됩니다.

- 그림 25. "ㅜname" 지정 및 DNS Hostname Prefix 지정
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step260.jpg)

[그림 26]과 같이 Public IP Address에는 [그림 16]에서 생성한 Public IP를 선택합니다. Security List 항목에는 기본적으로 생성된 default를 선택합니다. 그리고 SSH Key에는 [그림 13]에서 등록한 SSH 공개키를 선택합니다. 이상으로 입력이 완료되면 오른쪽 상단의 ```>``` 버튼을 클릭합니다.  

- 그림 26. Public IP, Security List, SSH Key 입력
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step280.jpg)

[그림 27]은 스토리지 설정 페이징 입니다.
스토리지 관련해서는 별도의 문서로 다룰 것입니다.
지금은 기본적으로 만들어진 "demo_ubuntu_vm_storage"를 그대로 사용할 것입니다.
이제 모든 설정은 완료되었습니다. 오른쪽 위의 ```>``` 버튼을 클릭합니다.  

- 그림 27. Storage 설정 페이지
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step290.jpg)

[그림 28]은 지금까지 인스턴스 생성 설정에 대한 요약 정보가 출력됩니다.
모든 정보가 맞는다면 ```Create``` 버튼을 클릭하여 인스턴스를 생성합니다.

- 그림 28. Instance 요약
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step300.jpg)

[그림 28]에서 ```Create``` 버튼을 클릭하면 Instances 페이지로 이동합니다.
Instances에는 현재 인스턴스가 추가되지 않은 상태입니다.
현재 생성 중인 인스턴스 상태는 Orchestrations 탭에서 확인 할 수 있습니다.

- 그림 29. Orchestrations 탭 이동
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step310.jpg)

[그림 30]과 같이 Orchestrations 탭에는 3개의 orchestation이 등록되어 있고 2개가 동작 중인 것을 확인할 수 있습니다.
각각은 master, storage, instance 용 orchestration입니다.
master는 storage와 instance orchestration을 실행하는 역할을 담당합니다.
storage orchestration이 먼저 실행디고 그다음
Instance Orchestration이 실행됩니다.
지금 생성하는 인스턴스의 생성 소요 시간은 약 3~5분 정도 입니다.

- 그림 30. Orchestrations 목록과 상태
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step320.jpg)

인스턴스 생성이 완료되면 [그림 31]과 같이 Orchestrations 목록이 모두 Ready 상태로 변경된 것을 확인할 수 있습니다.
Orchestration이 완료되었다면 Instances 탭으로 이동합니다.

- 그림 31. 인스턴스 생성이 완료된 Orchestrations 목록과 상태
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step330.jpg)

Instances 탭에서 새로 등록된 인스턴스를 확인할 수 있습니다.
또한 인스턴스에 사전에 등록한 Public IP가 설정된 것을 확인할 수 있습니다.
현재 인스턴스에는 외부에서 접근 불가능한 상태입니다.
외부에서 접근하기 위해서는 Network 탭으로 이동하여 Security Rule을 등록해야 합니다.

- 그림 32. 인스턴스 등록 확인
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step340.jpg)

### Step04: Security Rule 등록 (for SSH)

사용자 컴퓨터 터미널에서 SSH로 앞에서 생성한 인스턴스에 접근하기 위해서,
인스턴스에 SSH가 사용하는 22번 포트에 대한 보안 설정을 추가해야 합니다.
이러한 보안 설정은 Security Rule에서 설정됩니다.
Network 탭의 왼쪽 메뉴에서 "Security Rule"로 이동합니다.
"Security Rule" 페이지에서 [그림 33]과 같이 ```Create Security Rule``` 버튼을 클릭하면
Security Rule을 추가할 수 있습니다.

- 그림 33. Security Rule 등록
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step350.jpg)

[그림 34]는 Security Rule 등록 폼입니다. Security Rule을 등록하기 위해서는 다음과 같은 정보를 입력해야 합니다.

1. Security rule 명
1. Security Application: 포트
1. Source
1. Destination

기본적으로 위 4가지 정보를 등록해야 합니다.

- 그림 34. Security Rule 등록 폼
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step360.jpg)

SSH를 위한 Security Rule은 [그림 35]와 같이 입력합니다.
source의 public-internet은 모든 IP를 의미(0.0.0.0)합니다.
Destination에 설정한 default는 VM 인스턴스 생성 시 할당한 값입니다.

- 그림 35. SSH 서비스 오픈을 위한 설정
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step370.jpg)

[그림 35]에서 ```Create``` 버튼을 클릭하면, [그림 36]에서 Security Rule이 생성된 것을 확인할 수 있습니다.

- 그림 36. Security Rule 생성
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step380.jpg)

### Step05: VM 로그인 및 nginx 설치

사용자 컴퓨터에서 [그림 37]과 [그림 38] 같이 다음 명령으로 앞에서 생성한 VM 인스턴스에 로그인할 수 있습니다.

```bash
ssh ubuntu@129.144.152.137
```

- 그림 37. Ubuntu 로그인
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step390.jpg)

- 그림 38. Ubuntu 로그인 결과
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step400.jpg)


Ubuntu에 nginx를 설치하기 위해서는 [그림 37]과 [그림 38]과 같은 명령을 입력해야 합니다.

```
sudo apt-get updates
sudo apt-get install -y nginx
```

- 그림 39. apt 레파지토리 업데이트
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step440.jpg)

- 그림 40. nginx 설치
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step460.jpg)


nginx가 설치가 완료되면, [그림 41]과 같은 명령으로 nginx 서비스 상태를 확인할 수 있습니다.

- 그림 41. nginx 서비스 상태 확인
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step470.jpg)

### Step06: Security 등록 (for 80포트) 및 브라우저

nginx의 서비스를 브라우저로 접근하기 위해서는 추가적인 80포트 보안 설정이 필요합니다.
80 포트에 유입되는 모든 요청을 처리하기 위해서는 새로운 Security Rule을 [그림 42]와
[그림 43]과 같이 생성해야 합니다.

- 그림 42. 80 포트 오픈을 위한 Security Rule 생성 요청
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step480.jpg)

TCP/80 포트의 경우 Security Application에 이미 만들어져 있습니다.
따라서 새로 만들 필요 없이 [그림 43]과 같이 Security Application에 http를 선택하면 됩니다.
또한 인터넷으로부터 들어오는 모든 요청을 받아들이기 위해서 source에
public-internet을 선택하고 ```Create``` 버튼을 클릭합니다.

- 그림 43. 80 포트 오픈을 위한 Security Rule 생성
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step490.jpg)

Security Rule이 정상적으로 생성될 경우 [그림 44]와 같이 Security Rules 목록을 확인할 수 있습니다.

- 그림 44. Security Rules 목록을 확인
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step500.jpg)

Security Rules가 등록되면 바로 [그림 45]와 같이 브라우저 접근이 가능합니다.

- 그림 45. 브라우저 접근
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step510.jpg)

### Step07: VM 인스턴스 정지

OPC에 생성된 VM 인스턴스는 Orchestrations 탭에서 시작과 종료를 할 수 있습니다.
현재 인스턴스가 동작하는 상태는 [그림 46]과 같습니다.

- 그림 46. 인스턴스 구동 시 Orchestration 상태 조회
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step520.jpg)

인스턴스를 종료하기 위해서는 [그림 48]과 같이 master orchestration의 메뉴 아이콘을 클릭하고 stop을 클릭하면 됩니다.

- 그림 48. 인스턴스 셧다운을 위한 Master 종료 명령
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step530.jpg)

현재 인스턴스는 디폴트 스토리지를 사용하기 때문에 인스턴스 종료 시 데이터가 유실될 수 있습니다.
인스턴스 종료 시 데이터 유실을 방지하기 위해서는 별도의 스토리지를 생성하고 설정해야 합니다.
이 부분은 별도의 문서로 다루겠습니다.

현재 인스턴스를 종료하면 [그림 49]와 같이 데이터 유실에 관한 에러 메시지가 출력됩니다.

- 그림 49. 인스턴스 종료 시 데이터 유실 경고 메세지
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step540.jpg)

[그림 49]에서 "YES"를 클릭하면 Master Orchestration이 시작하며 [그림 50]과 같이 종료된 것을 확인할 수있습니다.

- 그림 50. 인스턴스 종료하는 Orchestration 프로세스 종료
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step550.jpg)

Orchestration이 종료되면 Instances 탭의 인스턴스 정보에서 Public IP가 사라진 것을 확인할 수 있습니다.

- 그림 51. Instance 탭에서 VM 종료 상태 확인
![](https://oracloud-kr-teamrepo.github.io/2017/03/iaas_quick/step560.jpg)

종료된 인스턴스를 다시 시작하기 위해서는 Orchestrations 탭에서 master orchestration의 메뉴 아이콘을 클릭하고 start를 클릭하면 됩니다.

## 마치며
지금까지 OPC에서 Ubuntu 이미지를 이용하여 VM 인스턴스를 생성하고, ssh 보안설정, 소프트웨어 설치, 80포트 보안 설정, 인스턴스 종료와 시작에 대해서 알아보았습니다.

다음 문서에서는 스토리지 및 보안 관련된 사항을 다루도록 하겠습니다.  
