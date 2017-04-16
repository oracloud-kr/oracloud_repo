+++
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2017/04/docker_container/docker.jpg"
date = "2017-04-12T19:34:26+09:00"
tags = ["docker", "container", "tutorial", "cloud"]
description = ""
categories = ["docker", "container"]
thumbnailInPost = "https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step000.jpg"
title = "OCCS에 Docker 이미지 배포하기"
language = ""
author = "taewan.kim"

+++
본 문서는 [Oracle Container CS 생성 절차](/post/occs-new-inst/)에서 컨테이너 서비스 인스턴스를 생성한 다음에, 더커 컨테이너를 실행하는 방법을 설명합니다. 본 문서에서는 다음과 같은 내용을 다룰 것 입니다.

- 도커 컨테이너 배포 및 접속 (Hello World 웹 페이지)
- 도커 컨테이너 이동
- 서비스 배포
- 서비스로 배포한 컨테이너 모니터링

## 관련 문서
본 문서는 로그인 가능한 오라클 클라우드 계정이 확보된 상태이며 OCCS(Oracle Container Cloud Service)에 인스턴스가 생성된 상태를 가정합니다. 오라클 클라우드 계정이 없거나 OCCS 인스턴스가 없는 상태라면 다음 문서를 참조하시기 바랍니다.

- [오라클 클라우드 계정 생성하기](/post/accont/)
- [Oracle Container CS 생성 절차](/post/occs-new-inst/)

## OCCS 용어 정리

<table>
<tr><th>Container CS 용어</th><th>설명</th><tr>
<tr><td>Service</td><td>단일 이미지를 컨테이너에 구동하기 위한 템플릿과 관련 정보 - 포트, 스토리지 볼륨 등</td></tr>
<tr><td>Stack</td><td>복수의 서비스를 컨테이너에 구동하기 위한 템플릿 및 관련 정보 Docker Compose 포멧</td></tr>
<tr><td>Deployment</td><td>Service와 Stack을 배포하여 하나 이상의 컨테이너를 만드는 것</td></tr>
<tr><td>Resource Pool</td><td>호스트 서버를 상호 독립적으로 그룹으로 구성하는 단위. 각 Resource Pool은 하나 이상의 호스트로 구성되면, 더커 환경을 관리하고, 복수의 호스트에 서비스와 스택을 배포하게 됨. 하나의 호스트는 하나의 Resource Pool에 포함되고, 하나의 Resource Pool은 여러 호스트를 호함.</td></tr>
<tr><td>Service</td><td>OCCS를 사용하여 생성, 배포, 관리 가능한 고수준 설정 객체, Docker 이미지를 OCCS에 맞도록 설정한 것</td></tr>
<tr><td>Service</td><td>OCCS를 사용하여 생성, 배포, 관리 가능한 고수준 설정 객체, Docker 이미지를 OCCS에 맞도록 설정한 것</td></tr>
</table>

## OCCS에 Docker 이미지 배포하기

OCCS에 더커 이미지를 배포하는 튜토리얼은 다음 순서로 진행됩니다.

- 튜토리얼 구성
  - Oracle Cloud 로그인 및 OCCS 서비스 콘솔 접근
  - OCCS 컨테이너 콘솔 로그인
  - 'Hello World' 컨테이너 배포
  - Resource Pool
  - 컨테이너 조회 및 중지
  - 이미지 조회
  - Registries
  - Service Discovery
  - Stack 배포
  - Stack Scale-out
  - 컨테이너 로그 조회
  - Stack 편집

### Oracle Cloud 로그인 및 OCCS 서비스 콘솔 접근

OCCS의 컨테이너 콘솔에 접근하기 위해서는 Oracle Cloud에 로그인을 해야 합니다.
로그인을 하기 위해서는 <그림 1>과 같이 ```http://cloud.oracle.com```에서 ```Sign In``` 버튼을 클릭합니다.

- 그림 1. cloud.oracle.com에서 로그인 페이지로 이동
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step010.jpg)

<그림 1>에서 ```Sign In```을 클릭하면 <그림 2>의 로그인 페이지로 이동합니다. 로그인 페이지에서 사용자의 계정이 할당된 계정 유형과 데이터 센터를 선택합니다. 현재 Trail 계정을 사용중이라면 다음과 같이 설정합니다.

- 계정 유형: Traditional Cloud Account
- 데이터 센터: Public Cloud Services - US

설정을 마치면 ```My Services``` 버튼을 클릭합니다.

- 그림 2. 계정 유형 및 데이터 센터 설정
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step020.jpg)

<그림 2>의 입력이 완려되면 <그림 3>의 도메인 입력 페이지로 이동합니다. 여기서 도메인이란 클라우드 서비스를 묶는 단위 입니다. 계정 생성시 할당된 도메인 명을 입력합니다. 도메인 명은 계정 생성 마지막 단계에서 발송된 메일에서 확인 가능합니다. 도메인 명을 입력하고 ```실행``` 버튼을 클릭합니다.

- 그림 3. 도메인 명 입력 페이지
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step022.jpg)

이제 로그인 절차의 마지막 단계입니다. 계정과 패스워드를 입력하고 ```사인인``` 버튼을 클릭합니다. 계정은 email형식입니다.

- 그림 4. 인증 정보 입력 및 로그인
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step024.jpg)

정상적으로 로그인되면 <그림 5>와 같이 할당된 자원을 전체적으로 보여주는 데쉬보드가 출력됩니다.

- 그림 5. Oracle Cloud의 대쉬보드
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step030.jpg)

### OCCS 컨테이너 콘솔 로그인

<그림 6>과 같이 대쉬보드 Conatainer 박스 오른편 상단에 위치한 메뉴 아이콘을 클릭하고 ```서비스 콘솔 열기``` 메뉴를 클릭합니다.

- 그림 6. 대쉬보드에서 서비스 콘솔로 이동
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step060.jpg)

서비스 콘솔에 접근하기 위해서는 로그인 단계를 거쳐야 합니다. OCCS 인스턴스 생성과정에서 설정한  계정 명과 패스워드를 입력하고 로그인 합니다. 기본 계정 명은 admin입니다.

- 그림 7. 서비스 콘솔 로그인
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step070.jpg)

서비스 콘솔의 로그인이 정상적으로 완료되면, OCCS 서비스 콘솔의 대쉬보드(그림 8> 참조)가 출력됩니다. 대쉬보드에서 클러스터의 배포 상태, 호스트 정보, Resource Pool, 컨테이너, 이미지 정도를 확인할 수 있습니다.

- 그림 8. 서비스 콘솔 대쉬보드
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step080.jpg)

### 'Hello World' 컨테이너 배포
Nginx에서 동작하는 Hello World 웹 애플리케이션의 더커 이미지를 배포하여 컨테이너를 만들 것입니다. 서비스 콘솔 대쉬보드의 왼쪽 메뉴에서 <그림 9>와 같이 ```Services``` 메뉴를 클릭하면 OCCS에 기본적으로 23개의 서비스 목록이 출력됩니다. Service는 앞에서 설명한 것처럼 Docker 이미지를 OCCS에서 관리 할 수 있도록 Docker 이미지에 OCCS 설정을 추가한 객체입니다. OCCS는 기본적으로 23개의 서비스를 제공합니다. 기본 제공하는 서비스를 참고하거나 복사하여 새로운 서비스를 만들수 있습니다. 오른편 상단의 검색 창에 "hello"를 입력하여 helloworld 서비스를 찾고, 이 서비스의 ```deploy``` 버튼을 클릭하여 서비스를 배포합니다.  

- 그림 9. helloworld 서비스 배포
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step090.jpg)

<그림 9>에서 ```deploy```버튼을 클릭하면 <그림 10>과 같이 배포를 위한 추가 설정 팝업창이 출력됩니다. <그림 10>의 설정 창에서는 다음과 같은 값을 설정합니다.

- Display name: 컨테이너 명
- Resource pool: 컨테이너를 배포할 Resource Pool
- Quality: 컨테이너 수량
- Scheduling policy: 컨테이너 배포시 Resource pool에서 호스트 선정 기준
  - Random: 임의 선정
  - Memory: 가용한 메모리가 가장 많은 호스트 순서로 선정
  - CPU: 가용한 CPU가 가장 높은 호스트 순서로 선정
- Available: 복수의 컨테이너 배포시 어떤 호스트에 배포할 것인지 결정
  - Per pool: pool의 Scheduling policy의 기준에 따라 지정된 Resource Pool의 호스트에 배포
  - Per Host: Resource Pool의 모든 호스트에 컨테이너 배포
  - Per Tag: tag가 할당된 호스트에 배포, per tag 출력시 tag 입력 필드 UI에 추가됨.

기본 설정을 그대로 유지한 상태로 ```deploy```버튼을 클릭하면, 서비스 배포가 시작됩니다.

- 그림 10. 서비스 배포 설정 팝업 창
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step100.jpg)



- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step110.jpg)

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step120.jpg)

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step130.jpg)

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step140.jpg)

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step150.jpg)

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step160.jpg)

### Resource Pool


- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step170.jpg)

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step180.jpg)

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step190.jpg)

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step200.jpg)

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step210.jpg)

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step220.jpg)

### 컨테이너 조회 및 중지

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step230.jpg)

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step240.jpg)

### 이미지 조회

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step250.jpg)

### Registries Service

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step260.jpg)

### Discovery

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step270.jpg)

### Stack 배포

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step280.jpg)

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step290.jpg)

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step300.jpg)

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step310.jpg)

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step320.jpg)

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step330.jpg)

### Stack scale-out
- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step340.jpg)

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step350.jpg)

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step360.jpg)

### 컨테이너 로그 조회

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step370.jpg)

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step380.jpg)

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step390.jpg)

### Stack 편집

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step400.jpg)

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step410.jpg)

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step420.jpg)

## 참고
- [Using Oracle Container Cloud Service](http://docs.oracle.com/en/cloud/iaas/container-cloud/contu/index.html) - 오라클 공식 메뉴얼
- [Running a Docker Container with Oracle Container Cloud Service in Three Easy Steps](https://apexapps.oracle.com/pls/apex/f?p=44785:112:0::::P112_CONTENT_ID:19220) - Oracle Learning Library
  - "Step By Step" 형식의 가이드 문서
