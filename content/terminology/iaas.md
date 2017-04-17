+++
description = ""
language = ""
categories = []
title = "Oracle Compute CS 용어사전"
thumbnailInPost = ""
thumbnailInList = ""
date = "2017-04-17T15:36:33+09:00"
author = ""
tags = []

+++
Oracle Compute CS의 주요 용어를 정리합니다. Oracle Compute Cloud Service는 Compute CS로 표기합니다.

### Image list
> 정의:
> Image List는 Compute CS의 머신 이미지 목록이다.
> Image List의 각 머신 이미지는 고유 항목 번호로 식별된다.

### Instance
> 정의:
> Instance는 Compute CS에 머신 이미지와 Shape를 지정하여 생성한 가상머신이다.
> Instance에 지정한 Shape은 CPU와 메모리 자원에 대한 정의이며, Shape에 정의된 자원이 Instance에 할당된다.

### Instance Shapshot
> 정의:
> Instance Snapshot은 인스턴스의 비 지속성 부트 디스크의 상태 정보를 챕처한 후, 이 정보를 이용하여 만든 머신 이미지이다.
> 이 머신 이미지는 Oracle Compute CS 계정에 등록할 수 있으며, 이 머신 이미지를 이용하여 새로운 인스턴스를 생성할 수 있다.

- 지존 이미지를 템플릿으로 머신 이미지를 커스터마이징하기 가장 용이한 방법
- Instance Snapshot으로 부터 동일한 구성을 갖는 여러 인스턴스 생성 가능
- 사용자 추가, 애플리케이션 설치 및 설정 등의 이미지 변경 사항을 템플릿으로 저장하고 재사용하는 방법을 제공
- Instance Snapshot은 Instance 동작중에 생성할 수 있음
- Instance Snapshot을 생성할 Instance는 Nonpersistent boot disk로 생성해야 함
- 스토리지 유형
  - Persistent Boot Disk는 블록 스토리지에서 영역을 할당 받는 스토리지
  - Nonpersistent Boot Disk는 Instance가 생성되는 호스트의 스토리지를 사용

### IP Networks
> 정의:
> IP Network을 사용하여 Oracle Compute CS 계정에 IP 서브넷을 정의할 수 있다.
> IP Network의 주소 범위는 IP Network 생성 시점에 설정하는 ```address preifx```에 따라서 결정된다.
> IP Network에 할당되는 주소는 공유 네트워크(Shared Network)에 포함되지 않는다. 가상 머신을 IP Network에 추가하면
> 인스턴스에 해당 IP Network(subnet)의 IP 주소가 할당된다.
> 인스턴스에 할당되는 IP 주소를 완전하게 제어할 수 있다.

### IP Network Exchange
> 정의: IP Network Exchange는 복수의 IP Network(subnet) 사이의 통신을 가능하게 한다.
> Network Exchage에 연결된 IP Network에 위치하는 Instance들은 상호간의 NAT없이도 패킷을 교환할 수 있다.

### IP Reservation
### Launch Plan
### Machine Image
### Orchestration
### Route
### Security Application
### Security IP list
### Security list
### Security Rule
### Shape
### Site
### Storage Volume
### Storage Volume Snapshot
### Virtual NIC
### VIrtual NIC Set
### VPN Endpoint
