+++
description = "오라클 클라우드의 Compute 서비스는 최근에 업그레이드가 적용되었고, 가상 머신 인스턴스를 UI가 일정부분 변경되었습니다. 변경된 UI상에서 가상머신을 생성하는 방법에 대하여 알아보겠습니다."
title = "우분투 17.04 가상머신 만들기"
categories = ["Cloud"]
thumbnailInPost = ""
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2017/04/compute_new/thumnail_logo.jpg"
date = "2017-05-02T11:59:47+09:00"
language = "bsh"
tags = ["Cloud", "Compute CS"]
author = "taewan.kim"
+++

2017년 04월 오라클 클라우드의 Compute Service에는 17.1.2 버전이 적용되었습니다.
17.1.2 버전에는 네트워크 구성에 대한 새로운 기능이 추가되었습니다.
기존의 Shared Network와 별개로 IPNetwork를 이용하여 서브넷을 구성할 수 있습니다.
새로운 기능 추가와 함께, 가상머신을 만드는 UI와 가상머신을 생성하는 오케스트레이션에 변화가 발생하였습니다.

매년 4월에는 Ubuntu의 메인 새 버전이 출시되는 시점입니다. 올해에서 Ubuntu 17.04 버전이 출시되었습니다.
새로 출시된 Ubuntu 17.04는 현재 오라클 클라우드 마켓플레이스에 추가된 상태입니다.

새로 변경된 Compute Cloud Service(이하 Compute CS)에서 Ubuntu 17.04를 이용하여 가상머신을 만들어 보고
SSH 접근을 위한 네트워크 보안 룰 설정 및 블록 스토리지 마운트를 진행해 보겠습니다.

## 선행 작업

본 문서는 다음과 같은 절차가 완료되었음을 전제로 합니다.

- 오라클 클라우드 계정 생성 완료
- 가상머신 생성에 필요한 인증서를 생성
- 오라클 클라우드 웹 사이트에 로그인이 완료

오라클 클라우드 계정 생성을 확보하지 못했거나 보안 키 생성을 못 한 상태라면 다음 문서를 참조하여 준비하시기 바랍니다.  

- [오라클 클라우드 계정](/post/accont/)
- [윈도우, 리눅스, 맥에서 SSH 보안키 생성](/post/ssh_key/)

## Compute CS 가상머신 만들기

### Ubuntu 17.04 인스턴스 만들기
Compute CS에 Ubuntu 17.04 이미지로 가상머신을 생성하겠습니다.

오라클 클라우드에 로그인하면 <그림 1>의 오라클 클라우드 대시보드가 출력됩니다.
오라클 클라우드 대시보드 왼쪽 위의 메뉴 아이콘을 클릭하면 현재 로그인한 계정이 접근 가능한 오라클 클라우드 서비스가 출력됩니다.

- 그림 1. 오라클 클라우드 대시보드
![](https://oracloud-kr-teamrepo.github.io/2017/04/compute_new/img010.jpg)

2017년 04월에 생성한 오라클 클라우드 Trial 계정의 경우 <그림 2>와 같이 12개의 서비스를 사용할 수 있습니다.
12개의 서비스 중에서 현재 Compute CS에 가상머신을 생성할 계획이기 때문에 ```Compute```를 클릭하여
Compute CS의 서비스 콘솔에 접근합니다.

- 그림 2. 오라클 클라우드 대시보드에서 ```Compute``` 메뉴 선택
![](https://oracloud-kr-teamrepo.github.io/2017/04/compute_new/img020.jpg)

<그림 3>과 같이 Compute CS 서비스 콘솔의 오른쪽 위에 위치하는 ```Create Instance```
버튼을 클릭하여 가상머신 생성을 시작합니다.

- 그림 3. Compute CS 서비스 콘솔에서 ```Create Instance``` 클릭하여 가상머신 생성 시작
![](https://oracloud-kr-teamrepo.github.io/2017/04/compute_new/img030.jpg)

가상머신 생성은 6단계로 구성됩니다. 이 중에서 첫 번째 단계는 이미지 선택 단계입니다. Compute CS는
세 가지 유형으로 이미지를 관리합니다. 첫 번째는 ```Oracle Images```입니다.
오라클은 공식 이미지로 21개를 제공하고 있고, 이 이미지는 ```Oracle Images```에 포함됩니다.
오라클 공식 이미지 21개에는 Oracle Enterprise Linux와 Solaris가 포함되어 있습니다.

두번째는 "Private Images"입니다.
이 카테고리에는 사용자가 직접 올린 이미지 혹은 Market Place에서 다운로드 받은 이미지가 포함됩니다.

세번째는 "Market Place"입니다.
오라클 클라우드는 Bitnami와 협력하여 현재 오라클 클라우드에서 이용 가능한 349개 (2017년 5월 2일 기준) 이미지를 제공합니다.

우리는 Ubuntu 17.04를 설치할 예정입니다. 이 리눅스 이미지는 Market Place에 포함되어 있습니다.
따라서 <그림 4>와 같이 Market Place를 선택합니다.

- 그림 4. Market Place 오픈
![](https://oracloud-kr-teamrepo.github.io/2017/04/compute_new/img040.jpg)

<그림 5>와 같이 Market Place를 선택한 후, Ubuntu를 검색합니다.
검색 결과 중에 ```Ubuntu Server 17.04 amd64``` 박스의 ```select``` 버튼을 클릭합니다.

- 그림 5. Market Place에서 Ubuntu 검색 후, 대상 이미지 선택
![](https://oracloud-kr-teamrepo.github.io/2017/04/compute_new/img050.jpg)

Market place에서 이미지를 선택하면, 해당 이미지는 Private Images에 복사하는 작업이 선행됩니다.
<그림 6>은 Private Images에 이미지 복사에 대한 동의를 한 후 ```install```버튼을 클릭합니다.
이렇게 ```install``` 버튼을 클릭하면 이미지 복사가 진행됩니다.

- 그림 6. 가상머신 이미지 다운로드 동의 및 설치  
![](https://oracloud-kr-teamrepo.github.io/2017/04/compute_new/img060.jpg)

<그림 7>은 이미지 설치가 완료된 다음의 모습입니다. 이제 ```>``` 버튼을 클릭하여 다음 단계로 진행합니다.

- 그림 7. 가상머신 이미지 선택 후 다음 단계로 이동
![](https://oracloud-kr-teamrepo.github.io/2017/04/compute_new/img070.jpg)

가상머신에 적용할 ```shape```을 선택합니다. Shape이란 가상머신에 적용할 하드웨어 스펙입니다.
<그림 8>과 같이 oc3를 선택하고, ```>``` 버튼을 클릭하여 다음 단계로 진행합니다.

- 그림 8. 가상머신에 적용할 Shape 선택
![](https://oracloud-kr-teamrepo.github.io/2017/04/compute_new/img080.jpg)

<그림 9>와 같이 인스턴스 기본 정보를 입력합니다.

|구분|설정값|설명|
|---|---|---|
|Name|Ubuntu_17_04_amd64|서버명|
|Label|Ubuntu_17_04_amd64|서버명|
|Description|Instance for demo| 서버에 대한 설명|

기본값을 설정한 후 ```Add SSH Public Key``` 버튼을 클릭하여, 사전에 준비한 SSH 공개 키를 추가합니다.

- 그림 9. 인스턴스 기본 정보 설정
![](https://oracloud-kr-teamrepo.github.io/2017/04/compute_new/img090.jpg)

<그림 10>과 같이 등록할 SSH 공개키에 대한 이름을 등록하고, 공개키를 추가한 후 ```add``` 버튼을 클릭합니다.

- 그림 10. SSH 공개키 등록
![](https://oracloud-kr-teamrepo.github.io/2017/04/compute_new/img100.jpg)

이제 ```SSH Keys```에 앞에서 등록한 공개키 명이 출력되었다면, ```>``` 버튼을 클릭하여 다음 단계로 진행합니다.

- 그림 11. SSH Keys에 공개키 등록 결과 확인 후, 다음 단계 이동
![](https://oracloud-kr-teamrepo.github.io/2017/04/compute_new/img110.jpg)

<그림 12>는 네트워크 설정 단계입니다.

Public IP Address 컬럼은 다음과 같은 3개의 옵션 중에 한개를 선택합니다.

|옵션|설명|
|---|---|
|Auto Generated|가상머신에 공개 IP를 새로 생성하여 붙임.|
|Persistent Public IP Reservation|사전에 만든 공개 IP 중에 선택하여 가상머신에 붙임|
|None|공개 IP를 연결하지 않음|

<그림 12>에서는  Auto Genetated 옵션을 선택하고 Security List로 ```default```를 선택한 후
, ```>``` 버튼을 클릭하여 다음 단계로 진행합니다.


- 그림 12. 네트워크 설정 단계
![](https://oracloud-kr-teamrepo.github.io/2017/04/compute_new/img120.jpg)

<그림 13>은 스토리지 설정 단계입니다.
오른쪽 위의 ```Add New Volumes```를 클릭하여 새로운 블록 스토리지를 등록합니다.

- 그림 13. 스토리지 설정 단계
![](https://oracloud-kr-teamrepo.github.io/2017/04/compute_new/img130.jpg)

<그림 14>에서는 새로운 블록 스토리지를 등록하는

| 구분 | 설정값 | 설명 |
|---|---|---|
| Name | data_vol | 블록 스토리지의 이름 |
| Size | 30 | GB 단위로 설정. 최댓값: 2000 |
| Storage Property | storage/default | 스토리지 타입 |
| Decription | "" | 블록 스토리지 설명 |
| Attach at Disk # | 2 | 디스크 순서 2번은 /dev/xvdc로 표시 <br/> 디스크 순서 3번은 /dev/xvdd로 표시|
| Boot Drives | "" | 체크 박스를 체크하면 디스크 1번으로 설정됨. 디스크 1번은 운영체제가 설치되는 디스크 |

<그림 14>에서 모든 속성에 대한 설정 완료되면, ```add``` 버튼을 클릭합니다.

- 그림 14. 블록 스토리스 추가
![](https://oracloud-kr-teamrepo.github.io/2017/04/compute_new/img140.jpg)

<그림 15>에서 앞에서 추가된 블록 스토리지가 출력되었다면, ```>``` 버튼을 클릭하여 다음 단계로 진행합니다.

- 그림 15. 블록 스토리지 등록 확인
![](https://oracloud-kr-teamrepo.github.io/2017/04/compute_new/img150.jpg)

<그림 16>에서 가상머신 설정 정보를 확인하고, 입력 정보가 정확하다면, ```create```
버튼을 클릭하여 가상머신 생성을 시작합니다.

- 그림 16. 가상머신 등록 정보 리뷰
![](https://oracloud-kr-teamrepo.github.io/2017/04/compute_new/img160.jpg)

가상 머신 생성 절차가 시작한 후, 웹 페이지는 Compute CS 서비스 콘솔 메인 페이지로 이동합니다.
가상머신 생성 시간은 약 5 분 정도가 입니다. 가상머신 배포 상태는 Orchestration 탭에서 확인 가능합니다.
<그림 17>에서는 Orchestration 탭을 클릭합니다.

- 그림 17. Compute CS 서비스 콘솔 메인 페이지에서 Orchestration 탭 선택
![](https://oracloud-kr-teamrepo.github.io/2017/04/compute_new/img170.jpg)

<그림 18>에서 총 6개의 Orchestration을 볼 수 있습니다. 하나의 가상 머신은 3개의 Orchestration으로 구성됩니다.

- Ubuntu_17_04_amd64_instance: 가상머신 생성 orchestration
- Ubuntu_17_04_amd64_master: instance와 storage를 관리하는 orchestration
- Ubuntu_17_04_amd64_storage: 블록 스토리지 생성 orchestation

<그림 18>에서 각 Orchestration의 상태는 stopped, starting, starting입니다.

- 그림 18. Orchestration 상태 확인   
![](https://oracloud-kr-teamrepo.github.io/2017/04/compute_new/img180.jpg)

Orchestration이 완료되면 상태는 ready로 변환됩니다.

- 그림 19. Orchestration 완료 상태
![](https://oracloud-kr-teamrepo.github.io/2017/04/compute_new/img190.jpg)

<그림 19>에서 Orchestration 상태가 완료되면, Instance 탭에 클릭하여 <그림 20>으로 이동합니다.
<그림 20>에서는 Ubuntu_17_04_amd64가 추가된 것을 확인 할 수 있습니다.
Ubuntu_17_04_amd64에 설정된 공개 IP는 129.144.12.116입니다.

- 그림 20. Ubuntu_17_04_amd64 가상머신 생성 결과
![](https://oracloud-kr-teamrepo.github.io/2017/04/compute_new/img200.jpg)

###  가상머신 접근을 위한 SSH 보안 설정

Ubuntu_17_04_amd64 가상머신이 생성된 후에, <그림 21>과 같이 129.144.12.116에 SSH 키로 접근하면
, ```Time out```되어 실패합니다.

가상머신은 처음 생성될 때 모든 네트워크 접근이 막혀 있습니다. 이와 관련하여 네트워크 접근을 허용하는 설정을 추가해야 합니다.

- 그림 21. SSH 접근 실패
![](https://oracloud-kr-teamrepo.github.io/2017/04/compute_new/img215.jpg)

본 문서에서는 SSH 보안 접근을 처리하는 부분만을 설명하겠습니다. 보안 설정에 대한 정의와 의미는 별도 문서로 다루겠습니다.

Compute CS의 서비스 콘솔 메인 페이지에서 ```Network``` 탭을 클릭하고 왼쪽 메뉴 중에 ```Security Rule```을 클릭한 후,
Security Rule 페이지에서 ```Create Security Rule```[^1]을 클릭하여 보안 룰을 생성합니다.  

[^1]: Security Rule은 네트워크 접속에 대한 룰을 설정하는 오라클 클라우드의 개념입니다. Security Rule은  소스, 목적지 그룹 간에 어떤 포트와 프로토콜(TCP/UDP)에 대한 연결 설정입니다. <그림 23>은 일반 인터넷(public internet)에서 default 그룹에 SSH 통신을 수행하는 것을 허용하는 보안 룰입니다.

- 그림 22. Security Rule 생성 시작
![](https://oracloud-kr-teamrepo.github.io/2017/04/compute_new/img217.jpg)

<그림 23>은 Security Rule의 정보 입력 폼입니다. <그림 23>과 같이 입력하고 ```Create```를 클릭하여 보안 룰을 생성합니다.
Ubuntu_17_04_amd64 가상 머신은 ```deault``` Secutiry List에 포함되기 때문에, ```deault``` Secutiry List에
설정된 Security Rule은 Ubuntu_17_04_amd64 가상 머신에 적용됩니다.

- 그림 23. Security Rule 정보입력
![](https://oracloud-kr-teamrepo.github.io/2017/04/compute_new/img210.jpg)

<그림 23>에서 ```Create```를 클릭하면, <그림 24>와 같이 Security Rule 목록에 새로 생성된 룰이 추가된 것을 확인할 수 있습니다.

- 그림 24. Security Rule 생성 결과
![](https://oracloud-kr-teamrepo.github.io/2017/04/compute_new/img220.jpg)

이제 Ubuntu_17_04_amd64에 SSH 접근에 필요한 Security Rule을 추가했기 때문에,
<그림 25>와 같이 가상머신에 SSH 접근 가능합니다.

- 그림 25. Ubuntu_17_04_amd64에 SSH 접근  
![](https://oracloud-kr-teamrepo.github.io/2017/04/compute_new/img230.jpg)

###  블록 스토리지 마운트
<그림 15>에서 data_vol 블록 스토리지를 Ubuntu_17_04_amd64에 추가 했습니다. data_vol은 index가 2입니다.
이 블록 스토리지는 /dev/xvdc로 표현됩니다.

<pre class="prettyprint">
ubuntu@demo:~$ ls -al /dev/xvd*
brw-rw---- 1 root disk 202, 16 Apr 30 09:29 /dev/xvdb
brw-rw---- 1 root disk 202, 17 Apr 30 09:29 /dev/xvdb1
brw-rw---- 1 root disk 202, 30 Apr 30 09:29 /dev/xvdb14
brw-rw---- 1 root disk 202, 31 Apr 30 09:29 /dev/xvdb15
brw-rw---- 1 root disk 202, 32 Apr 30 09:29 /dev/xvdc
</pre>

블록 스토리지 ```/dev/xvdc```를 마운트하는 명령은 다음과 같습니다.

<그림 26>과 같이
주요 명령 요약은 다름과 같습니다.

|명령|설명|
|---|---|
|sudo mkfs -t ext3 /dev/xvdc | 파일 시스템 생성 |
|mkdir data| 마운트 포인트 디렉터리 생성 |
|sudo mount /dev/xvdc data | data 디렉터리에 블록 스토리지 마운트 |
|df -h| 마운트 포인트 확인 |


- 그림 26. 블록 스토리지 마운트
![](https://oracloud-kr-teamrepo.github.io/2017/04/compute_new/img240.jpg)

블록 스토리지 마운트 설정이 가상머신 재시작 이후에도 유지되기 위해서는 ```/etc/fstab``` 파일에 다음을 추가해야 합니다.

<pre class="prettyprint linenums">
LABEL=cloudimg-rootfs	/	 ext4	defaults	0 0
LABEL=UEFI	/boot/efi	vfat	defaults	0 0
/dev/xvdc  /home/ubuntu/data     ext3    defaults  0 0
</pre>

1, 2라인은 기본 설정이고, 블록스토리지 마운트 관련 설정은 3라인입니다.

## 가상머신 인스턴스 재시작

가상머신을 리부팅해야 할 경우에 <그림 27>과 같이 Compute CS의 서비스 콘솔에서 인스턴스를 재시작할 수 있습니다.
인스턴스 재시작에 필요한 시간은 약 2분 정도입니다.

- 그림 27. 가상머신 인스턴스 재시작
![](https://oracloud-kr-teamrepo.github.io/2017/04/compute_new/img270.jpg)

## Orchestration에서 가상머신 제어

Orchestraion 탭에서 가상머신을 제어할 수 있습니다.
Orchestration에서 가상머신을 종료하는 것은 모든 자원을 반환하고 초기화하는 것을 의미합니다.
종료된 가상머신은 Orchestration을 통해서 다시 시작할 수 있습니다. 다시 시작한 가상머신은
새로운 자원이 할당된 완전히 새로운 인스턴스 입니다.

웹 콘솔을 통해서 만들어진 가상머신은 3개의 orchestration을 갖습니다.
이 3개의 가상머신 Orchestration을 <그림 28>과 같이 메뉴 ```stop```을 실행할 수있습니다.

- 그림 28. Orchestration에서 가상머신 종료하기
![](https://oracloud-kr-teamrepo.github.io/2017/04/compute_new/img280.jpg)

종료된 가상머신의 Orchestration은 ```stopped```으로 표혀됩니다.

- 그림 29. Orchestration 종료 상태
![](https://oracloud-kr-teamrepo.github.io/2017/04/compute_new/img290.jpg)

가상머신을 시작할 때 Orchestration 중에서 master orchestration을 <그림 30>과 같이
메뉴 ```start```를 실행하면됩니다.
Master Orchestration만 시작하면 나머지 2개의 Orchestration은 자동으로 실행됩니다.

- 그림 30. Orchestration에서 가상머신 시작
![](https://oracloud-kr-teamrepo.github.io/2017/04/compute_new/img300.jpg)

Orchestration 실행이 완료되면 상태는 ```ready``` 상태로 표현됩니다. (<그림 31> 참조)

- 그림 31. 가상머신 인스턴스 재시작
![](https://oracloud-kr-teamrepo.github.io/2017/04/compute_new/img310.jpg)

## 요약

지금까지 Compute CS에서 Ubuntu 17.04 가상머신을 생성하고,
외부에서 가상머신에 네트워크 접근이 가능하도록 Security Rule 추가하였습니다. 그리고
SSH 원격 접속 후, 사전에 정의한 블록스토리를 마운투하였으며, 마지막으로 인스턴스 재시작 및
Orchestration 종료 방법을 살펴 보았습니다.
