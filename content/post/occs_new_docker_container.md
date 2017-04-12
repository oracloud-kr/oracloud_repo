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
