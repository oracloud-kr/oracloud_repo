+++
date = "2017-08-06T22:20:25+09:00"
description = "Oracle PaaS Service Manager(이하 PSM)는 오라클 클라우드의 PaaS 서비스 관리에 사용되는 CLI(Command Line Interface)입니다. Oracle PSM을 설치하는 절차를 소개합니다."
title = "Oracle PaaS Service Manager 소개 및 설치"
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2017/08/psm/psm.jpg"
thumbnailInPost = "https://oracloud-kr-teamrepo.github.io/2017/08/psm/psm_dbcs_stop.gif"
tags = ["PSM", "Oracle Cloud"]
categories = ["PaaS"]
author = "taewan.kim"
language = "bash"  
+++

Oracle PaaS Service Manager(이하 PSM)는 오라클 클라우드의 PaaS 서비스 관리에 사용되는 CLI(Command Line Interface)입니다.
Oracle 클라우드는 PaaS 서비스를 관리하는 REST API를 제공합니다.
이 REST API를 이용하여 PaaS 서비스를 인스턴스 생성, 중지 그리고 삭제를 할 수 있습니다.
PSM은 PaaS REST API에 대한 CLI 유형의 클라이언트 구현체입니다.
PSM를 이용하여 클라우드 환경 프로비저닝과 삭제 자동화를 구축할 수 있습니다.

## Oracle PSM 지원 PaaS 서비스

|Oracle PaaS |비고|
|---|---|
|Oracle Application Container Cloud Service|Container 서비스, Polyglot 환경, Java, Python, PHP, Ruby 지원|
|Oracle Analytics Cloud|데이터 분석 클라우드 서비스|
|Oracle Big Data Cloud Service - Compute Edition|빅데이터 클라우드 서비스|
|Oracle Database Cloud Service|Managed Oracle Database  서비스|
|Java Cloud Service.|Managed WAS 서비스|
|MySQL Cloud Service |Managed MySQL 서비스|                             
|Oracle Event Hub Cloud Service| Managend Kafka 클러스터|


## Oracle PSM 설치 요구사항

Oracle PSM의 실체는 앞에서 설명한 것과 같이 Python으로 만든 오라클 클라우드 PaaS REST API의 클라이언트 구현체 입니다.
Oracle PSM을 설치하기 위해서는 파이썬이 설치되어 있어야 합니다. Oracle PSM은 파이썬 3.3 버전 이상을 지원합니다.
파이썬 3.3설치된 모든 OS에 Oracle PSM을 설치할 수 있습니다.

파이썬 버전 확인 방법은 다음과 같습니다.

<pre class=prettyprint>
vagrant@vagrant-ubuntu-trusty-64:~$ python3 --version
Python 3.4.3
vagrant@vagrant-ubuntu-trusty-64:~$
</pre>

추가적으로 pip3가 설치되어 있어야 합니다.

REST API를 호출하는 툴로 본 문서에서는 curl을 사용합니다. curl이 설치 되어 있어야 합니다.
curl가 아직 설치 되어 있지 않다면 다음 문서를 참고하시기 바랍니다.

- [리눅스에서 curl 설치하기](https://www.lesstif.com/pages/viewpage.action?pageId=6979777)
- [Windowd에서 curl 설치하기](https://m.blog.naver.com/PostView.nhn?blogId=javaking75&logNo=220776461230&proxyReferer=https%3A%2F%2Fwww.google.co.kr%2F)

## Oracle PSM 다운로드

Oracle PSM 설치 파일은 오라클 클라우드 서비스 콘솔에서 웹사이드를 통해서 다운로드 받거나
REST API로 직접 다운로드 받을 수 있습니다.

### 오라클 클라우드 웹 사이트 다운로드

오라클 클라우드의 PaaS 서비스 콘솔에서 Oracle PSM 설치 파일을 다운로드 받을 수 있습니다.

- 그림 1. Oracle Cloud Dashboard: 오라클 클라우드 로그인 결과
![](https://oracloud-kr-teamrepo.github.io/2017/08/psm/psm01.jpg)

Oracle Cloud 웹사이드에 로그인을 하면 "__Oracle Cloud Dashboard__"가 오픈됩니다. <그림 1 참조>
"__Oracle Cloud Dashboard__"의 왼쪽 위의 메뉴 아이콘을 클릭하고 PaaS 서비명을 클릭하여 PaaS 서비스 콘솔로 이동합니다.

여기서 PaaS 서비스 명이란 다음 중 하나를 의미합니다.

- Java
- Database
- Application Container
- Big Data - Compute Edition
- Container
- Event Hub - Dedicated
- MySQL
- Oracle Analytics Cloud

데모에서는 "__Big Data - Compute Edition__"을 선택하였습니다. <그림 2 참조>

- 그림 2. PaaS 서비스 콘솔로 이동: Big Data - Compute Edition
![](https://oracloud-kr-teamrepo.github.io/2017/08/psm/psm02.jpg)

<그림 3>와 같이 "Oracle Big Data Cloud Service - Compute Edition"의
서비스 콘솔의 오른쪽 상단에 출력된 계정명을 클릭하여 "도움말"->"Download Center"를 선택합니다.

- 그림 3. 계정명을 클릭하고 컨텍스트 메뉴에서 "Download Center" 클릭
![](https://oracloud-kr-teamrepo.github.io/2017/08/psm/psm03.jpg)

"Download Center" 메뉴를 선택하면 <그림 4>와 같은 팝업 왼도우가 출력됩니다.

- 그림 4. Download Center 팝업 윈도우에서 PMS 다운로드 아이콘 클릭
![](https://oracloud-kr-teamrepo.github.io/2017/08/psm/psm04.jpg)


<그림 4>와 같이 첫번째 PSM설명의 오른쪽 다운로드 아이콘을 클릭하면 PSM 설치 파일이 다운로드 됩니다.
설치파일 사이즈는 46Kbyte 입니다. <그림 5 참조>

- 그림 5. PMS 다운로드 결과
![](https://oracloud-kr-teamrepo.github.io/2017/08/psm/psm05.jpg)

### Oracle PSM 다운로드 REST API

REST API의 주소 패턴은 다음과 같습니다.

```
https://<URL>/paas/core/api/v1.1/cli/<IDENTITY_DOMAIN>/client
```

위 패턴에서 URL과 IDENTITY_DOMAIN은 다음과 같은 값으로 대치됩니다.

- ```<URL>```
  - 데이터 센터의 위치에 따라서 다음 URL 중 하나를 선택합니다.
    - 북미 데이터 센터: psm.us.oraclecloud.com
    - 유럽 데이터 센터: psm.europe.oraclecloud.com
  - Trial 계정은 북미 데이터 센터를 사용
- ```<IDENTITY_DOMAIN>```
  - 사용중인 계정의 Identity domain을 사용합니다.

curl[^1]로 PSM 다운로드 REST API를 호출할 경우, 다음과 같은 형태로 호출합니다.

[^1]: curl 은 command line 용 data transfer tool입니다. HTTP/HTTPS/FTP/LDAP/SCP/TELNET/SMTP/POP3 등 주요한 프로토콜을 지원하며 Linux/Unix 계열 및 Windows 등 주요한 OS 에서 구동됩니다. REST API 테스트용도로 광범위하게 사용되는 툴입니다.

```
curl -X GET -u <ACCOUNT_ID>:<PASSWORD> \
-H X-ID-TENANT-NAME:<IDENTITY_DOMAIN> \
https://<URL>/paas/core/api/v1.1/cli/<IDENTITY_DOMAIN>/client \
-o psmcli.zip
```

위 curl 호출 형식에서 ```<VARIALE>```로 표시된 부분은 변수입니다.
위에서 사용한 각 변수(```<VARIALE>```)는 다음 값으로 대치됩니다.

|변수|설정값|
|---|---|
|```<ACCOUNT_ID>```|Oracle Cloud 계정 ID (예: tw01@plustv.io)|
|```<PASSWORD>```|Oracle Cloud 계정의 로그인 패스워드|
|```<IDENTITY_DOMAIN>```|Oracle Cloud 계정의 Identity Domain(예:krplustvio)|
|```<URL>```|psm.us.oraclecloud.com|

위 명령에서 각 행 마지막에 ```\```은 콘솔에서 명령이 끝나지 않고 다음 행으로 연속된다는 의미입니다.
위 명령은 한줄로 처리됩니다.

- 그림 6. PSM 다운로드 REST API 호출 및 다운로드 결과 확인
![](https://oracloud-kr-teamrepo.github.io/2017/08/psm/psm06.jpg)

## Oracle PSM 설치

Oracle PSM은 파이썬3가 설치된 모든 운영체제에 설치 가능합니다. 본 문서에서는 window와  리눅스(Mac OS)를 대상으로 설명하겠습니다.

### 환경변수 설정

현재 프록시 서버를 사용하고 있는 경우에는 Oracle PSM 설치에 앞서 환경변수를 설정해야 합니다.
각 운영체제별 환경 설정 방법은 다음과 같습니다.

프록시 서버 주소는 http://myproxy.server.com:80 / https://myproxy.server.com:80 로 가정합니다.

- Window 환경 변수 설정

```
set http_proxy=http://myproxy.server.com:80      
set https_proxy=https://myproxy.server.com:80     
```

- Linux / Mac OS 환경 변수 설정

```
export http_proxy=http://myproxy.server.com:80
export https_proxy=https://myproxy.server.com:80
```

### Oracle PSM 설치

Oracle PSM 설치 명령은 운영체제 별로 다음과 같습니다. 다음 명령은 psmcli.zip파일이 위치한 디렉터리에서 실행해야 합니다.

- Window
  - ```pip3 install -U psmcli.zip```
- Linux, Mac OS
  - ```sudo pip3 -H install -U psmcli.zip```

다음은 Ubuntu에서 위 명령을 수행한 결과 입니다.
```
[opc@e25327 ~]$ ls -al *.zip
-rw-rw-r-- 1 opc opc 69104 Aug  5 13:08 psmcli.zip
[opc@e25327 ~]$ sudo pip3 install -U psmcli.zip
Processing ./psmcli.zip
Collecting requests<=2.8.1,>=2.7.0 (from psmcli==1.1.15)
  Downloading requests-2.8.1-py2.py3-none-any.whl (497kB)
    100% |████████████████████████████████| 499kB 901kB/s
Collecting keyring<=5.6,>=5.4 (from psmcli==1.1.15)
  Downloading keyring-5.6.tar.gz (69kB)
    100% |████████████████████████████████| 69kB 2.3MB/s
Collecting colorama==0.3.3 (from psmcli==1.1.15)
  Downloading colorama-0.3.3.tar.gz
Collecting PyYAML==3.11 (from psmcli==1.1.15)
  Downloading PyYAML-3.11.zip (371kB)
    100% |████████████████████████████████| 372kB 880kB/s
Installing collected packages: requests, keyring, colorama, PyYAML, psmcli
  Running setup.py install for keyring
  Running setup.py install for colorama
  Running setup.py install for PyYAML
  Running setup.py install for psmcli
Successfully installed PyYAML-3.11 colorama-0.3.3 keyring-5.6 psmcli-1.1.15 requests-2.8.1
You are using pip version 7.1.2, however version 9.0.1 is available.
You should consider upgrading via the 'pip install --upgrade pip' command.
[opc@e25327 ~]$
```

### Oracle PSM 설치 확인

다음 명령으로 PSM 설치 상태를 확인할 수 있습니다. 다음과 같은 메세지가 출력된다면 설치가 완료된 상태입니다.

```
[opc@e25327 ~]$ psm
usage: psm <service> <command> [parameters]
psm: error: the following arguments are required: service
[opc@e25327 ~]$
```

### Oracle PSM 설정

Oracle PSM을 사용하기 전에, Oracle PSM이 Oracle Cloud에 접속하기 위해서 필요한 정보를 설정하는 절차를 수행해야 합니다.

다음과 같이 ```psm setup```을 실행하면 사용자명, 패스워드, Identity Domain, 데이터 센터 지역, 출력 형식을 선택 항목을 입력해야 합니다. ```[value]```는 기본값입니다. 기본값이 맞다면 추가입력없이 "__Enter__"를 치시면 됩니다.
출력 형태는 json, html을 입력할 수 있습니다.

```
[opc@e25327 ~]$ psm setup
Username: tw01@plustv.io
Password:
Retype Password:
Identity domain: krplustvio
Region [us]: us
Output format [short]:
```

<그림 7>은 ```psm setup```의 실행 예입니다. 입력이 완료되면
<그림 7>과 같이 지원 클라우드 서비스 목록이 출력됩니다.
2017년 8월 5일 현재 21개 서비스를 지원합니다.
Oracle PSM의 PaaS 지원 목록은 지속적으로 추가되고 있습니다.

- 그림 7. psm setup 실행 예제
![](https://oracloud-kr-teamrepo.github.io/2017/08/psm/psm07.jpg)

### Oracle PSM 업데이트

Oracle PSM의 PaaS 지원 목록이 주기적으로 추가되기 때문에, Oracle PSM은 주기적으로 업데이트 됩니다.
다음 명령으로 Oracle PSM 버전 확인 및 업그레이드를 할 수 있습니다.

```
[opc@e25327 ~]$ psm --version
PSM CLI Client - version 1.1.15
[opc@e25327 ~]$ psm update
INFO: You already have the most up-to-date version of psm client installed on the system
[opc@e25327 ~]$
```

## Oracle PSM 데모
기본적인 명령 구문은 다음과 같습니다.

```
psm [ 클라우드 서비스 이름 ]  [ 명령 ]  ( help )
```

Oracle PSM을 사용하는 간단한 방법을 소개합니다. Oracle BDCSCE 서비스를 Oracle PSM으로 조작하는 데모를 다음과 같은 순서로 진행하겠습니다.

- BDCSCE 서비스 목록 조회: ```psm bdcsce services```
- BDCSCE sparkdemo 인스턴스 상세 정보 조회: ```psm bdcsce service -s sparkdemo```
- BDCSCE 서비스 제거: ```psm bdcsce delete-service -s sparkdemo```
- BDCSCE 서비스 목록 조회: ```psm bdcsce services```

```
[opc@e25327 ~]$ psm bdcsce services
 Service    Status
 ---------- --------
 sparkdemo  Ready
[opc@e25327 ~]$ psm bdcsce service -s sparkdemo
 Service:                    sparkdemo
 Status:                     Ready
 Version:                    17.3.1-20
 Edition:                    Compute Edition
 Compute Site:               US006_Z49
 Cloud Storage Container:    Storage-krplustvio/bdcscontainer
 Created On:                 2017-08-06T02:59:04.153+0000
[opc@e25327 ~]$ psm bdcsce delete-service -s sparkdemo
 Message:    Submitted job to delete service [sparkdemo] in domain [krplustvio].
 Job ID:     14151120
[opc@e25327 ~]$ psm bdcsce services
 Service    Status
 ---------- -------------------------
 sparkdemo  Terminating Service ...
 [opc@e25327 ~]$ psm bdcsce services
 No data found
 [opc@e25327 ~]$
```

## 참고자료
- [Oracle PSM 공식 레퍼런스 문서](https://docs.oracle.com/en/cloud/paas/java-cloud/pscli/toc.htm)
- [Getting started with Oracle PaaS Service Manager Command Line Interface (PSM)](https://technology.amis.nl/2017/03/07/getting-started-with-oracle-paas-service-manager-command-line-interface-psm/)
- [Oracle Cloud의 명령 줄 인터페이스 (CLI)](http://qiita.com/shinyay/items/a3773b37fdbb677e52b1)
