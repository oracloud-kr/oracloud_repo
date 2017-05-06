+++
language = ""
categories = ["compute"]
author = "taewan.kim"
tags = ["security", "compute" ]
description = "오라클 클라우드의 보안 설정 방법에 대하여 정리합니다."
thumbnailInPost = "https://oracloud-kr-teamrepo.github.io/2017/05/cloud_security/post_logo.jpg"
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2017/05/cloud_security/list_logo.jpg"
title = "오라클 클라우드 Compute CS 보안 적용"
date = "2017-05-04T13:23:35+09:00"
+++

Oracle Compute CS에서 인스턴스(가상머신)을 만들면, 해당 가상머신에 대한 네트워크 접근은 완전히 막혀 있는 상태입니다.
외부와 어떤 네트워크 연결도 불가능한 상태로 만들어집니다.
새로 만든 가상머신에 네트워크 접근하기 위해서는 오라클 클라우드가 제공하는 보안 설정을 적용해야 합니다.

오라클 클라우드는 가상머신 별로 섬세한 보안 설정을 구성하는 방법을 제공합니다.
본 문서에서는 오라클 클라우드의 보안 구성 요소를 파악하고,
각 구성 요소를 이용하여 보안 설정하는 방법에 대하여 살펴보겠습니다.

## 오라클 클라우드 보안 개념

컴퓨터간의 네트워크를 연결을 보안관리하기 위해서는 최소한 다음과 같은 4가지 정보가 필요합니다.

- 시작 컴퓨터
- 목적지 컴퓨터
- 프로토콜
- 포트

오라클 클라우드는 위 4가지 정보를 다음과 같은 용어로 관리합니다.

|구분|설명|오라클 클라우드 용어|
|---|---|---|
|시작 컴퓨터| 네트워크 접속의 시작 점 |Source|
|목적지 컴퓨터| 네트워크 접속의 목적지 |Destination|
|프로토콜 + 포트 |  |Security Application |

컴퓨터들 사이의 통신을 관리하는 보안 룰은 다음과 같은 정보들의 조합으로 구성됩니다.

> 시작 컴퓨터로부터 목적지 컴퓨터에 특정 포트와 프로토콜 통신을 허용하는 룰

오라클 클라우드에서는 위와 같은 보안 룰을 ```Security Rule```이라는 용어로 정의합니다.

**Security Rule** = Souece + Destination + Security Application

Security Rule을 정의할 때 Source와 Destination을 개별 컴퓨터로 대응하면 매우 비효율적입니다.
이러한 이유로 Source와 Destination을 그룹 단위의 컴퓨터 목록으로 관리합니다.
오라클 클라우드는 컴퓨터 그룹 단위를 Security List와 Security IP List로 정의하고,
이 Security List와 Security IP List를 Source와 Destination으로 사용합니다.

결과적으로 오라클 클라우드는 다음과 같은 4가지 구성 요소로 가상머신의 접근 보안 정보를 관리합니다.

- Security List
- Security IP List
- Security Application
- Security Rule

다음 절에서는 위의 각 보안 구성 요소의 정의를 알아보고,
오라클 클라우드에서 각 구성 요소를 관리하는 방법에 대하여 소개하겠습니다.

## 오라클 클라우드 보안 구성 요소

오라클 클라우드가 보안 정보를 관리하는 4개의 구성 요소의 정의는 다음과 같습니다.

|구성요소|정의|
|---|---|
|Security List| Security list는 Oracle Compute CS의 인스턴스(가상머신)의 목록으로, 하나의 source와 destination으로 묶을 수 있는 인스턴스들을 하나의 Security List로 만들어 관리합니다. 같은 Security List에 포함된 인스턴스들 사이의 통신에는 어떤 제약도 없습니다. |
|Security IP List|Security List가 인스턴스 목록을 관리했다면, Security IP List는 목록 관리 대상이 IP Address입니다. 목록 관리 대상은 IP 주소 혹은 IP 서브넷(CIDR 포멧)으로 등록됩니다. Security IP List에는 오라클 클라우드가 아닌 외부 컴퓨터도 IP 주소로 등록될 수 있습니다. |
|Security Application|Security Application은 통신 프로토콜과 대상 포트 정보의 조합입니다. |
|Security Rule| Security Rule은 방화벽 규칙이라고 생각하시면 편리합니다. 두 개의 컴퓨터 목록(Security List 혹은 Security IP List) 사이에 특정 프로토콜과 포트에 대한 네트워크 접속을 허용하는 규칙입니다. Source, Destination, Security Application 정보로 구성됩니다. Source와 Destination에는 Security List 혹은 Security IP List가 할당됩니다.|

- 그림 1. 오라클 클라우드 보안 구성 요소 요약
![](https://oracloud-kr-teamrepo.github.io/2017/05/cloud_security/components.png)

### Security List

Security list는 하나의 source와 destination으로 묶을 수 있는 인스턴스들의 묶음입니다. Security List는 Compute CS 서비스 콘솔의 네트워크 탭에서 만들 수 있습니다.

<그림 2>와 같이 Compute CS 서비스 콘솔의 네트워크 탭 아래에는 Shared Network와 IP Network가 있습니다.
그중에서 Shared Network 서브 메뉴에서 Security List를 찾을 수 있습니다.
<그림 2>와 같이 Security List 페이지의 오른쪽 위에 위치하는 ```Create Security List``` 버튼을 클릭하여 Security List 생성을 요청합니다.

- 그림 2. Security List 생성요청
![](https://oracloud-kr-teamrepo.github.io/2017/05/cloud_security/img010.jpg)

<그림 2>에서 Security List 생성을 요청하면, <그림 3>과 같이 Security List 생성에 필요한 정보 입력 폼이 출력됩니다.
<그림 3>과 같이 입력하고 ```Create``` 버튼을 클릭합니다.

- 그림 3. Security List 정보 입력
![](https://oracloud-kr-teamrepo.github.io/2017/05/cloud_security/img020.jpg)

정상적으로 Security List가 생성되면 <그림 4>와 같은 결과를 확인할 수 있습니다.

- 그림 4. Security List 생성 결과 확인
![](https://oracloud-kr-teamrepo.github.io/2017/05/cloud_security/img030.jpg)

지금까지 ```seq_web_list``` 보안 리스트를 만들었습니다. 현재 ```seq_web_list```에는 어떠한 가상머신도 할당되지 않았습니다. 보안 리스트에 가상머신 추가는 가상머신 생성단계에 <그림 5>와 같이 지정할 수 있습니다. 가상머신은 동시여 여러 보안 리스트에 포함될 수 있습니다.   

- 그림 5. 가상머신에 보안 리스트 지정
![](https://oracloud-kr-teamrepo.github.io/2017/05/cloud_security/img040.jpg)

가상머신을 만든 이후에도 가상머신에 보안 리스트를 추가하거나 할당한 보안 목록을 제거할 수 있습니다.
<그림 6>은 가상머신 ```Overview``` 페이지에서 보안 리스트 ```default```를 새로 추가하는 모습입니다.
가상머신 ```Overview``` 페이지에서 보안 리스트를 새로 추가하거나 제거할 수 있습니다.

- 그림 6. 가상머신에 보안 리스트 추가 지정
![](https://oracloud-kr-teamrepo.github.io/2017/05/cloud_security/img050.jpg)

### Security IP List

Security List가 인스턴스 목록을 관리했다면, Security IP List는 IP 주소를 기준으로 컴퓨터 목록을 관리합니다. 목록 관리 대상은 IP 주소 혹은 IP 서브넷(CIDR 포멧)으로 등록됩니다. Security IP List에는 오라클 클라우드가 아닌 외부 컴퓨터도 IP 주소로 등록될 수 있습니다.

<그림 7>과 같이 Compute CS 서비스 콘솔의 네트워크 탭 아래에는 Shared Network와 IP Network가 있습니다.
그중에서 Shared Network 서브 메뉴에서 Security IP List를 찾을 수 있습니다.

Security IP List 페이지에는 <그림 6>과 같이 사전에 만들어진 6개의 목록을 확인할 수 있습니다.
이 중에서 ```Public_Internet```을 주목하시기 바랍니다. 이 목록은 불특정 외부 컴퓨터에서 오라클 클라우드의 특정 목록에 접근하는 것을 허용할 때 Source로 자주 사용되는 Security IP List입니다.
오라클 클라우드에 전근해야 하는 관리자 혹은 개발자가 고정 IP를 사용한다면 Security IP List를 만들어서 사용할 수 있지만, 개발자와 관리자가 유동 아이피를 사용하기 때문에 고장 IP로 Security IP List 목록을 만들 수 없다면 ```Public_Internet```를 사용해야 합니다.

- 그림 7. Security IP List 페이지
![](https://oracloud-kr-teamrepo.github.io/2017/05/cloud_security/img060.jpg)

Security IP List를 생성할 경우에는 <그림 8>의 입력 폼과 같이 목록 이름과 IP 목록을 입력합니다. IP 목록은 쉼표로 구분하면 IP 주소 혹은 CIDR 포멧으로 입력합니다.

- 그림 8. Security IP List 페이지
![](https://oracloud-kr-teamrepo.github.io/2017/05/cloud_security/img070.jpg)

### Security Application

Security Application은 통신 프로토콜과 대상 포트 정보를 나타내는 객체입니다.

<그림 9>과같이 Compute CS 서비스 콘솔의 네트워크 탭 아래에는 Shared Network와 IP Network가 있습니다.
그중에서 Shared Network 서브 메뉴에서 Security Application을 찾을 수 있습니다.

Oracle Cloud는 기본적으로 30가지의 Security Application을 제공합니다.
<그림 9>와 같이 Security Application 페이지에서는, 빌트인으로 제공되는 Security Application 목록을 찾을 수 있습니다.

- 그림 9. Security Application을 페이지
![](https://oracloud-kr-teamrepo.github.io/2017/05/cloud_security/img080.jpg)

추가적인 프로토콜 및 포트 조합의 Security Application이 필요하다면 <그림 10>과 같은 입력폼을 이용하여
Security Application을 생성할 수 있습니다.

- 그림 10. Security Application을 생성 폼
![](https://oracloud-kr-teamrepo.github.io/2017/05/cloud_security/img090.jpg)

Port Type은 프로토콜을 지정하는 컬럼으로 다음과 같은 값 중에 하나를 선택할 수 있습니다.

- TCP
- UDP
- ICMP
- GRE
- ESP
- Ohter (직접 입력)

### Security Rule

Security Rule은 어디에서 부터 어디로, 어떤 프로토콜이용하여 어떤 포토에 접속하는 네트워크 접속을 허용하는 규칙입니다.
이 설명에 사용되는 단어는 다음과 같이 오라클 클라우드 용어로 연결됩니다.

| 단어 | 설명 | 비고 |
|---|---|---|
| 어디에서 부터 | 보통 Source라고 표현<br/>네트워크 접속이 시작되는 컴퓨터 그룹| Source 지정시 Security List 혹은 Securty IP List 중 유형을 선택하여 지정 |
| 어디로 | 보통 Destination 이라고 표현<br/>네트워크 접속의 목적지 컴퓨터 그룹| Source 지정시 Security List 혹은 Securty IP List 중 유형을 선택하여 지정|
| 프로토콜, 포트 | 네트워크 접속 기본 정보 | 프로토콜과 포트 정보를 Security Application으로 표현|

Security Rule은 <그림 11>과 같이 ```네트워크``` 탭 아래 Security Rules 페이지에서 만들 수 있습니다.

- 그림 11. Security Rule 생성 요청
![](https://oracloud-kr-teamrepo.github.io/2017/05/cloud_security/img100.jpg)

<그림 12>와 아래 테이블은 ssh 네트워크 통신을 위한 Sequrity Rule을 만드는 입력 폼의 예입니다.

|번호|컬럼명|설명|
|---|---|---|
|1|Name|이름은 알파벳, "_"(underscores) 그리고 "-"(hyphen)으로 구성, 첫글자와 마지막 글자는 반드시 알파벳이어야 함|
|2|Status|```Enabled```과 ```Disabled``` 중 선택|
|3|Security Application|대상 Security Application 지정|
|4|Source| Security List 혹은 Securty IP List 유형 선택 후 사전에 만든 그룹을 지정 |
|5|Destination| Security List 혹은 Securty IP List 유형 선택 후 사전에 만든 그룹을 지정 |

- 그림 12. Security Rule 생성을 위한 기초 정보 입력 폼
![](https://oracloud-kr-teamrepo.github.io/2017/05/cloud_security/img110.jpg)

<그림 12>에서 모든 항목을 입력하고 ```create``` 버튼을 클릭하면,
<그림 13>과 같은 결과를 출력하게 됩니다.

- 그림 13. Security Rule 생성 결과
![](https://oracloud-kr-teamrepo.github.io/2017/05/cloud_security/img120.jpg)

## 보안 적용 예

지금까지 오라클 클라우드에서 보안에 사용되는 4가지 구성 요소를 알아보았습니다.
두 가지 시나리오로 예로 보안 설정을 실습해 보겠습니다.

### 새 가상머신에 ssh와 ping 설정

- 사전 준비 사항
  - 새로 오라클 리눅스를 이미지로 가상머신을 생성
  - 가상머신은 Security List인 web_list에 포함
- 목표
  - 유동 아이피를 사용하는 시스템 관리자가 가상머신에 ssh 접근과 ping이 가능하도록 함
- 가정
  - 관리자가 사용하는 노트북에는 가상머신에 할당된 ssh key의 private key가 저장되어 있음

<그림 14>와 같은 절차로 새로 만든 가상머신의 상세 정보를 확인할 수 있습니다.

- 그림 14. 가상머신 상세 정보 이동 메뉴
![](https://oracloud-kr-teamrepo.github.io/2017/05/cloud_security/img130.jpg)

<그림 15>에서 가상머신이 포함되는 Security List의 목록을 확인할 수 있습니다.

- 그림 15. 가상머신 상세 정보
![](https://oracloud-kr-teamrepo.github.io/2017/05/cloud_security/img140.jpg)

위에서 설정한 보안 설정을 완료하기 위해서는 외부 네트워크에서 web_list에 ssh와 ping을 허용하는 2개의
Security Rule을 만들어야 합니다. 생성할 Security rule 생성 정보는 다음과 같습니다.

|name|status|Security Application|Source|Destination|
|---|---|---|---|---|
|seq_ssh_from_pi_to_web_list|Enabled|ssh|Security IP List <br/> Public_Internet|Security List <br/> web_list|
|seq_ping_from_pi_to_web_list|Enabled|ping|Security IP List <br/> Public_Internet|Security List <br/> web_list|

위 표로 생성한 Security rule의 결과는 <그림 16>과 같습니다.

- 그림 16. 첫 번째 시나리오의 Security rule 생성 결과
![](https://oracloud-kr-teamrepo.github.io/2017/05/cloud_security/img150.jpg)

Security rule 생성이 완료되면 <그림 17>과 같이 가상머신에 ping과 ssh 접근이 가능합니다.

- 그림 17. 가상머신에 ping과 ssh 접속
![](https://oracloud-kr-teamrepo.github.io/2017/05/cloud_security/img160.jpg)

### web, was, mysql 서버의 보안 처리

두 번째 시나리오는 웹 서버, WAS 서버, DB 서버 총 4개의 가상머신으로 구성된 서버의 보안설정입니다.
각 가상머신의 요약 정보는 다음과 같습니다.

|서버명|설치 소프트웨어|역할|외부 접속|
|---|---|---|---|
|web01|Nginx|Load Balancer|관리자가 ssh 접근<br/>외부 인터넷으로 부터 80포트(tcp) 접근|
|was01|tomcat| WAS 서버<br/>mysql 연결|관리자가 ssh 접근<br/>nginx가 8080포트(tcp) 접근|
|was02|tomcat| WAS 서버<br/>mysql 연결|관리자가 ssh 접근<br/>nginx가 8080포트(tcp) 접근|
|db01|mysql| db 서버 | 관리자가 ssh 접근<br/>was01과 was02에서 3306포트(tcp) 접근|

관리자는 유동 IP를 사용함을 가정합니다. 위 가상머신 목록은 <그림 18>과 같이 정리할 수 있습니다.

- 그림 18. 가상머신 레이아웃
![](https://oracloud-kr-teamrepo.github.io/2017/05/cloud_security/img170.jpg)

<그림 18>의 네트워크 통신이 가능해지기 위해서는 가상머신을 다음과 같은 Security List에 할당하고,
4개의 Security Rule을 만들어야 합니다.

- Security List 구성

|서버명|Security List|
|---|---|
| web01 | web_list, default |
| was01 | was_list, default |
| was02 | was_list, default |
| db01  | was_list, default |

Security Rule을 보면 관리자는 모든 서버에 ssh접근을 합니다.
ssh 접근 Security Rule을 모든 서버에 지정하기 위해서, 모든 서버를 ```default``` security list에 할당합니다.

두 번째 시나리오를 만족하는 Security Rule은 <그림 19>로 요약할 수 있습니다.

- 그림 19. Security Rule 요약
![](https://oracloud-kr-teamrepo.github.io/2017/05/cloud_security/img180.jpg)

## 요약
지금까지 오라클 클라우드에서 보안 설정에 필요한 4가지 구성 요소를 살펴보고,
보안 설정을 적용하는 방법에 대하여 알아보았습니다.

오라클 클라우드는 기본에 방화법에 대한 이해와 네트워크에 대한 기본적인 이해가 있다면,
보안 적용을 쉽게 구성할 수 있도록 보안 설정을 구성하였습니다.

4가지 보안 구성 요소는 다음과 같습니다.
- Security List
- Security IP List
- Security Application
- Security Rule

Security List는 오라클 클라우드의 가상머신 중 공통의 역할을 수행하는 서버의 그룹입니다.
Security IP List는 IP 주소로 공통의 특징을 갖는 컴퓨터 목록의 그룹입니다.
Security IP List는 기본적으로 6개의 목록이 만들어져 있습니다.
이 중에서 일반적인 외부 접속을 정의하는 ```Public_Internet```은 자주 사용되는 Security IP List입니다.

추가적으로 두 가지 시나리오를 이용하여 Security Rule를 정의하는 방법을 소개하였습니다.
