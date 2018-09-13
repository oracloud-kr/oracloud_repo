+++
date = "2018-09-11T17:00:00+09:00"
description = "Application Container Cloud Service를 활용하여 손쉽게 웹 서비스를 구축하는 방법에 대한 소개입니다."
title = "Application Container로 10분 안에 웹서비스 구축"
thumbnailInList = "https://monosnap.com/image/C1zbSeu5fc6l78BLY3qFwSZqmPe8EQ.png"
thumbnailInPost = "https://monosnap.com/image/gYEfJYExUmyNuGFvqiX1pDdoBzdd1G.png"
tags = ["ORACLE", "Oracle Cloud", "Application Container Cloud", "accs", "Cloud Native", "Polyglot", "오라클 클라우드", "Java EE"]
categories = ["Oracle-Cloud"]
author = "jongmin.lim"
language = ""  
+++

Oracle Application Container Cloud를 활용해서 10분 안에 jsp를 바로 서비스하는 과정을 설명합니다.

***
# Oracle Autonomous Blockchain Cloud Service 소개

2018년 7월 16일 오라클에선 HyperLedger Fabric 기반의 블록체인 서비스가 출시 되었는데 이름은 Autonomous Blockchain Cloud Service라 부릅니다. Oracle의 블록체인에 대한 목표중 하나는 고객이 비즈니스 구현을 쉽고 빠르게 만드는 것으로 실제 HyperLedger Fabric을 사용하는 개발자나 운영자분들은 오픈소스 측면에서의 생소한 블록체인 플랫폼을 운영하거나 개발하는것이 어려워 중도에 다른 플랫폼을 찾아보거나 하는 여러 케이스를 확인하였습니다.
이러한 시장 요구에 발 맞춰 오라클은 Autonomous 블록체인 플랫폼을 탄생 시켰습니다.

## HyperLedger란?
리눅스 재단에서 주관하는 블록체인 오픈소스 프로젝트로서 기업에서 선호하는 이유는 프라이빗 블록체인 플랫폼으로서 기업 비즈니스를 구현하기에 적합하고, 금융에 특화된 타 블록체인과는 달리 여러 산업에 범용적으로 도입이 가능한 표준 기술을 사용한다는 특징이 있습니다.

![OABCS_strength](https://monosnap.com/image/rcCfPSor1LiEteqSB1gpkPrvUsaDA4)

하이퍼레저 프로젝트는 크게 두가지로 나뉘는데 하나는 HyperLedger Framework이고, 다른 하나는 HyperLedger Tool입니다. 이중 HyperLedger Fabric이 분산원장 프레임워크에 해당 됩니다.
하이퍼레저의 그린하우스 전략은 공통 기반 요소에 대한 재활용을 통해서 커뮤니티를 강화함과 동시에 블록체인 기술 요소의 빠른 발전을 유도하고 있는 장점등이 있어 오라클 Autonomous 블록체인 플랫폼은 이 HyperLedger Fabric을 기반으로하여 탄생하게 됩니다.

## 다양한 HyperLedger Fabric의 글로벌 사례
![OABCS_strength](https://monosnap.com/image/cgZj6wPZSoeqgZrqcTEQNPuqrcxc2Y)
HyperLedger Fabric은 여러 산업군에 적용하기 좋은 기업형 솔루션으로 금융, 유통, 제조, 헬스케어, 에너지 분야등의 다양한 산업 사례들이 지속적으로 만들어지고 있습니다.
## Oracle 블록체인의 주요 특징

![OABCS_strength](https://monosnap.com/image/bu2WGRc610sRG6U4crohzwmIeFK688)

### 1. Pre-assembled
ID 관리, 오픈 소스 구성 요소 및 인프라등이 이미 빌트인 되어 있습니다. 이러한 특징으로 인해 구축시간 및 비용을 줄일 수 있습니다.
### 2. Open
리눅스 재단의 HyperLedger 패브릭은 여러 산업군 및 벤더에서 사용하므로 오픈 스탠다드로 사용되는 플랫폼입니다.
### 3. Plug and Play Integrations
Oracle SaaS 및 3rd Party 애플리케이션과의 플러그앤 플레이 통합기능을 지원합니다. 
### 4. Enterprise grade
높은 가용성, 보안, 지속적 백업 기능을 제공하여 엔터프라이즈급의 수준높은 서비스를 보장합니다. 
### 5. Autonomous
Autonomous가 붙는 업계 유일의 블록체인 서비스입니다.

## Autonomous 의미
오라클 주요 클라우드 서비스에 Autonomous라는 개념을 도입하였다. Autonomous는 아래와 같은 특징을 가지고 있습니다.
* Self-Driving : 모든 구성요소 자동 프로비저닝, 복구, 모니터링, 다운타임없는 스케일아웃, 자동 성능설정 조정이 가능합니다.
* Self-Securing : 자동 데이터 암호화, IDM Cloud에서의 지능적 사이버 위협 탐지 및 해결, 다운타임 없는 자동 보안패치등이 가능합니다.
* Self-Reparing : 다운타임 없는 자동 보안패치/업그레이드가 가능합니다.


## Oracle 블록체인 클라우드 아키텍쳐

![OABCS_strength](https://monosnap.com/image/F6lAGosPilmrkS5L1Xns4jb50bN7IM)

Oracle이 오퍼링한 블록체인 클라우드 서비스의 아키텍처를 살펴보도록 하겠습니다. 엔터프라이즈를 위한 블록체인 플랫폼을 구성하기 위해 기본적으로 다음과 같은 중요 4가지 요소(SMART CONTRACTS, CONFIDENTIALITY, DISTRIBUTED LEDGER, CONSENSUS)가 지원되어야 하며, 이러한 블록체인 핵심 요소들이 잘 운영될 수 있도록 오라클 클라우드의 안전한 인프라 서비스 및 이들 요소를 확장하여 사용할 수 있도록 하였습니다.

그리고, 오라클 블록체인이 아닌 외부 HyperLedger 플랫폼(3rd Party 클라우드 사용자와 고객 데이터 센터 안에 있는 HLF)과 연동이 될 수 있는 기능을 지원하고 있습니다.

기타, 레거시 및 Oracle SaaS, 3rd party SaaS등 연계를 위해, REST API 및 Adaptor등에 대한 제공으로 손쉽게 통합 연계 개발이 가능하도록 지원되는 특징을 가지고 있습니다.  


### 오라클 블록체인 서비스를 체험하려면?

오라클 블록체인 클라우드를 체험해 보세요 : https://cloud.oracle.com/trial