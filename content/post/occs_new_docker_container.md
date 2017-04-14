+++
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2017/04/docker_container/docker.jpg"
date = "2017-04-12T19:34:26+09:00"
tags = ["docker", "container", "tutorial", "cloud"]
description = ""
categories = ["docker", "container"]
thumbnailInPost = ""
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

## OCCS에 Docker 이미지 배포하기

튜토리얼은 다음과 같은 순서로 진행됩니다.

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

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step010.jpg)

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step020.jpg)

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step022.jpg)

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step024.jpg)

### OCCS 컨테이너 콘솔 로그인

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step030.jpg)

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step040.jpg)

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step050.jpg)

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step060.jpg)

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step070.jpg)

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step080.jpg)

### 'Hello World' 컨테이너 배포

- 그림 1.
![](https://oracloud-kr-teamrepo.github.io/2017/04/docker_deploy/step090.jpg)

- 그림 1.
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
