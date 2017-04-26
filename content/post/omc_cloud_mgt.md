+++
author = "jc.hong"
categories = ["Oracle Management Cloud","PaaS"]
date = "2017-04-21T10:31:26+09:00"
description = ""
title = "클라우드를 이용한 차세대 인프라 운영관리 방안"
thumbnail = ""
tags = ["CLOUD","OMC","Management"]
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2017/04/omc_cloud_mgt/OMCLogo.png"
thumbnailInPost = "https://oracloud-kr-teamrepo.github.io/2017/04/omc_cloud_mgt/OmcDataFlow.jpg"

+++

## OMC : 클라우드를 이용한 차세대 인프라 운영관리 방안

Oracle Management Cloud Service (이하 OMC)를 통한 기존 On-Premise의 인프라 운영 문제점의 해결 방법과 차세대 인프라 관리 방법에 대한 방안 소개
 

- 클라우드 시대 머신러닝을 포함한 IT Trend에 발맞추어 IT 운영 관리는 어떻게 발전되어야 할까요?
- 고객들은 어떤 어려움을 해결하고 싶어할까요?
- 데모를 통해 비즈니스 어플리케이션 장애를 신속하게 해결하는 방법은? 
- 오라클의 클라우드 관리 서비스를 이용한 차세대 IT 운영 관리방법은?

OMC는 기존 개별화된 IT 조직 업무의 단위 모니터링 솔루션에 대한 비 효율성을 극복하기 위해 다양한 인프라의 많은 분석 대이터를 효율적으로 분석하며, 인프라 전반의 통합 모니터링 및 시스템의 성능 예측, 장애분석, 비정상적 행위를 탐지하는 기능을 제공합니다.   

### OMC의 log 및 IT성능 Data 수집 및 전송 방법

 
![](https://oracloud-kr-teamrepo.github.io/2017/04/omc_cloud_mgt/OmcArchitecture.jpg)
<center>< 그림 1. Oracle Management Cloud의 Data Flow ></center>

### 1. 신속한 문제 해결

![](https://oracloud-kr-teamrepo.github.io/2017/04/omc_cloud_mgt/OMC1.jpg)
<center>< 그림 2. OMC를 이용한 신속한 문제 해결 ></center>

### 2. 병목 현상 예측

- 특정 기간의 Exadata 전체 DB의 스토리지 용량 및 메모리 사용량 모니터링
- Exadata Rack 전체 범위
- DB 노드 메모리 바인딩 현상 => 병목
- 개별 메모리 사용량 분석을 통한 향후 사용량 예측

![](https://oracloud-kr-teamrepo.github.io/2017/04/omc_cloud_mgt/OMC2.jpg)
<center>< 그림 3. Host Resource Analytics ></center>
 
### 3. Database 리소스 통합 분석

![](https://oracloud-kr-teamrepo.github.io/2017/04/omc_cloud_mgt/OMC3.jpg)
<center>< 그림 4. Database Resource Analytics ></center>

### 4. 다양한 기본 데쉬보드 및 사용자 데쉬보드 제공

![](https://oracloud-kr-teamrepo.github.io/2017/04/omc_cloud_mgt/OMC4.jpg)
![](https://oracloud-kr-teamrepo.github.io/2017/04/omc_cloud_mgt/OMC41.jpg)
<center>< 그림 5. OMC의 다양한 기본 데쉬보드 및 사용자별 커스터마이징 데쉬보드 ></center>

### 5. 특정 Log 패턴 및 Alert Rule 생성

특정 로그 패턴 및 사용 트랜드 임계치 위반 알람 설정

![](https://oracloud-kr-teamrepo.github.io/2017/04/omc_cloud_mgt/OMC5.jpg)
<center>< 그림 6. OMC의 Alert Rule 설정화면 ></center>

### 6. OMC 구성 예시

![](https://oracloud-kr-teamrepo.github.io/2017/04/omc_cloud_mgt/OMC6.jpg)
<center>< 그림 7. Exadata를 위한 OMC 구성 예시 ></center>


## 추가정보

- [Oracle Management Cloud(OMC) 소개](http://www.oracloud.kr/post/omc/)
- [Oracle Management Cloud 홈페이지](https://cloud.oracle.com/ko_KR/management)
- [OMC 공식 문서 홈페이지 - docs.oracle.com](http://docs.oracle.com/en/cloud/paas/management-cloud/index.html)





