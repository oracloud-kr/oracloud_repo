+++
author = "taewan.kim"
categories = ["Oracle Cloud"]
date = "2017-09-14T17:15:48+09:00"
description = "2017년 9월 9일 오라클 클라우드 서비스 명이 일부 교체되었습니다. 서비스 명 변경과 관련하여 내용을 정리합니다. "
language = ""
tags = ["Oracle Cloud"]
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2017/09/change_name/list.jpg"
thumbnailInPost = "https://oracloud-kr-teamrepo.github.io/2017/09/change_name/post.jpg"
title = "Oracle IaaS 브랜드 명 변경"
+++

2017년 9월 9일을 오라클 클라우드의 IaaS의 브랜드명이 변경되었습니다.
Oracle Bare Metal Cloud Services (BMCS)와 Oracle Public Cloud (OPC)가 각각 "__Oracle Cloud Infrastructure(OCI)__"와 "__Oracle Cloud Infrastructure Classic(OCI Classic)__"으로 변경되었습니다.
이번 서비스명 변경이 기존에 배포되고 관리되던 서비스에는 미치는 영향은 없으며, 사용자 인터페이스에도 변경은 없습니다.

2017년 9월 9일 이후에 배포되는 문서는 새로운 서비스명으로 기술됩니다.
과거 문서와 신규 문서에서 브랜드명 변경에 따른 혼돈에 주의하시기 바랍니다.

이번에 서비스명은 변경되었지만, 오라클 클라우드의 관련 인터페이스에 변경은 없습니다.
다만 Chef Knife과 Terraform에는 변경이 있습니다. 오라클 클라우드 프로비저닝에 Chef knife를 사용하시는 분들은 다음 URL을 참조하여 플러그임을 업데이트하시기 바랍니다. 기존에 knife-bmcs가 knife-oci로 변경됩니다.

- https://github.com/oracle/knife-bmcs/blob/master/docs/rename.md

Terraform의 오라클 클라우드 provider도 변경될 예정입니다. 새로운 오라클 클라우드 provider는 다음 레파지토리에서 개발 중입니다.

- https://github.com/oracle/terraform-provider-oci

chef와 terraform의 변경 사항에 대해서는 별도 문서로 정리하겠습니다.

마지막으로 변경된 서비스명은 다음 목록을 참조하시기 바랍니다.

|변경 전(과거 서비스 명)|변경 후(현재 서비스 명)|
|----|----|
|Oracle Bare Metal Cloud Services|	Oracle Cloud Infrastructure|
|Oracle Bare Metal Cloud Compute Service|Oracle Cloud Infrastructure Compute|
|Oracle Bare Metal Cloud Block Volume Service|Oracle Cloud Infrastructure Block Volumes|
|Oracle Bare Metal Cloud Object Storage Service|Oracle Cloud Infrastructure Object Storage|
|Oracle Bare Metal Cloud Networking Service|Oracle Cloud Infrastructure Networking|
|Oracle Bare Metal Cloud FastConnect Service|Oracle Cloud Infrastructure FastConnect|
|Oracle Bare Metal Cloud Audit Service|Oracle Cloud Infrastructure Audit|
|Oracle Bare Metal Cloud Identity and Access Management Service|Oracle Cloud Infrastructure Identity and Access Management|
|Oracle Bare Metal Cloud Load Balancing Service|Oracle Cloud Infrastructure Load Balancing|
|Oracle Bare Metal Cloud Database Service|Oracle Cloud Infrastructure Database|
|Oracle Storage Cloud Software Appliance|Oracle Cloud Infrastructure Storage Software Appliance|
|Oracle Bare Metal Cloud Compute Service|	Oracle Cloud Infrastructure Compute|
|Oracle Compute Cloud Service|Oracle Cloud Infrastructure Compute Classic|
|Oracle Compute Cloud Service - Dedicated Compute Capacity|Oracle Cloud Infrastructure Dedicated Compute Classic|
|Oracle Compute Cloud Service - Block Storage|Oracle Cloud Infrastructure Block Storage Classic|
|Oracle Storage Cloud Service|Oracle Cloud Infrastructure Object Storage Classic|
|Oracle Storage Cloud Service - Archive Storage|Oracle Cloud Infrastructure Archive Storage Classic|
|Oracle Network Cloud Service|Oracle Cloud Infrastructure Networking Classic|
|Oracle Compute Cloud Service - Load Balancing Capacity|Oracle Cloud Infrastructure Load Balancing Classic|
|Oracle Network Cloud Service - VPN for Engineered Systems|Oracle Cloud Infrastructure VPN for Engineered Systems Classic|
|Oracle Network Cloud Service - FastConnect|Oracle Cloud Infrastructure FastConnect Classic|
|Oracle Network Cloud Service - FastConnect Standard Edition|Oracle Cloud Infrastructure FastConnect Classic - Standard Edition|
|Oracle Network Cloud Service - FastConnect Partner Edition|Oracle Cloud Infrastructure FastConnect Classic - Partner Edition|
|Oracle Container Cloud Service|Oracle Cloud Infrastructure Container Service Classic|
