+++
date = "2017-08-29T08:54:32+09:00"
description = "2017년 9월 9일 부로 오라클 클라우드 서비스 명이 변경되었습니다. 변경 사항을 공지합니다. "
title = "Oracle 클라우드 서비스 명 변경: Oracle Cloud Infrastructure"
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2017/08/oraclecode_ktw/list.jpg"
thumbnailInPost = ""
tags = ["Terraform", "Vagrant", "Ansible"]
categories = ["Event"]
author = "taewan.kim"
language = ""  #bsh,c,cc,cpp,cs,csh,cyc,cv,htm,html,java,js,m,mxml,perl,pl,pm,py,rb,sh,xhtml,xml,xsl
+++

2017 Oracle Code in Seoul에서 발표한 "__Vagrant, Terraform, Ansible을 활용한 클라우드 인프라 관리법__"의 발표 자료와 진행한 데모를 소개하는 문서입니다. 클라우드 자원을 체계적으로 관리하고, 완전한 자원 통제권을 확보하는 방법을 소개하는 세션입니다. 이 세션은 2017년 8월 30일 14시 30분에 여의도 콘래드 3층 그랜드 볼륨에서 진행됩니다. 클라우스 서비스에  Infrastructure as Code(IaC) 개념을 적용하여, 클라우드 자원을 형상관리하고 필요한 시점에 원하는 형상의 클라우드 자원을 빌드하는 체계입니다. 이 세션의 장표는 Slideshare에 공유하였습니다.

<iframe src="https://www.slideshare.net/TaewanKim/slideshelf" width="760px" height="570px" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:none;" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe>

## 데모 & 사전 준비 사항

이 세션에서는 3개의 데모가 진행되었습니다.

1. Terraform을 이용하여 오라클 클라우드에 VM 1개 생성
1. Haproxy로 부하분산 웹사이트 구성 - Terraform, Ansible, Testinfra
  - 구성: 5개 VM
  - Load Balancer: HAProxy 1set
  - Web: 2set, Apache2
  - DB: 1set, MySQL
  - Monitoring: 1set, Nagios

## 사전 준비사항: Oracle Cloud 계정

데모에서 VM은 모두 Oracle Cloud에 배포됩니다. 따라서 Oracle Cloud의 계정이 필요합니다. 현재 Oracle Cloud 계정이 없다면 [Oracle Cloud Trial 신청: $300 Credit](http://www.oracloud.kr/post/oracle_cloud_reg/) 문서를 참조하여 계정을 생성하시기 바랍니다.

## 사전 준비사항: Software

데모를 진행하기 위해서는 다음과 같은 소프트웨어가 필요합니다.

|Software| 설명 |설치 방법 소개|
|---|---|---|
|git| Provisioning 스크립트는 Github에 공개되어 있습니다. github의 프로젝트를 클론하는 용도로 사용됩니다.|NO|
|python2|ansible은 파이썬으로 개발된 툴입니다. ansible과 Testinfra를 구동하기 위해서는 파이썬 2.6 이상이 필요합니다. |NO|
|terraform|Infrastructure 프로비저닝 툴입니다. |YES|
|ansible|Configuration Managment 툴입니다.|YES|
|Testinfra|Infrastructure의 상태를 체크하는 단위 테스트 툴입니다.|YES|

git과 python을 설치해야 한다면 다음 링크를 참조하시기 바랍니다.

- [git 설치](https://www.slideshare.net/jinkyouson/1-git-66328046)
  - Slideshare 문서
  - window와 Mac에서 git을 설치하는 방법 소개
- [python 설치](https://wikidocs.net/44)
  - python 설치 소개
  - 리눅스, 맥, 윈도우

### Terraform

Terraform은 Go-lang으로 개발되어 있습니다. Terraform은 1개의 실행 파일로 구성되어 있습니다. [테라폼 홈페이지 다운로드 사이트](https://www.terraform.io/downloads.html)에서는 Mac OS X, FreeBSD, Linux, OpenBSD, Solaris, Windows용 실행파일을 제공합니다. 데모에 사용할 컴퓨터의 운영체제와 비트에 맞는 파일을 내려받고, Zip 파일 포멧으로 제공된 파일의 압축을 풀면 "__terraform__" 실행 파일이 생깁니다. 이 실행 파일의 위치는 PATH 환경 변수에 추가하여 어디서든지 실행할 수 있도록 준비합니다. Terraform 설치는 이것으로 완료되었습니다.

### Ansible

Ansible은 설치 방법은 다음과 같습니다.

- 소스 설치
- pip로 설치
- yum 설치 (CentOS, RHEL, Oracle Linux)
- apt-get 설치 (Debian, Ubuntu)
- Brew 설치 (Mac OS X)

----

- 소스 설치
  - 2017년 8월 30일 현재 ansible의 최신 버전은 "__v2.3.2.0-1__"입니다. 소스 설치 절차는 다음과 같습니다.

```
$ pip install paramiko PyYAML jinja2 httplib2
$ git clone git://github.com/ansible/ansible.git
$ cd ./ansible
$ git checkout tags/v2.3.2.0-1  
$ source ${HOME}/ansible/hacking/env-setup
```

- pip 설치
```
$ pip install ansible
```

- yum 설치
```
$ yum install ansible -y
```

- apt-get 설치
```
$ sudo apt-get install ansible
```

- brew 설치
```
$ brew install ansible
```

### Testinfra

Testinfra는 파이썬으로 만든 인프라 테스트 프레임워크입니다. pip로 설치됩니다.

```
$ pip install testinfra
```

## 데모 준비

### 데모 스크립트 내려받기

데모를 위해서는 Github에 공유한 프로젝트를 클론해야 합니다.[^1]

[^1]: Github 주소는 https://github.com/taewanme/oracle_code_provisioining_demo 입니다.

프로젝트 클론 명령은 다음과 같습니다.

```
$ git clone https://github.com/taewanme/oracle_code_provisioining_demo.git demo
Initialized empty Git repository in /home/opc/demo/.git/
remote: Counting objects: 68, done.
remote: Compressing objects: 100% (47/47), done.
remote: Total 68 (delta 4), reused 68 (delta 4), pack-reused 0
Unpacking objects: 100% (68/68), done.
$ cd demo
$ pwd
/home/opc/demo
$
```

이제 부터 "__/home/opc/demo__" 디렉터리를 __{PROEJCT_HOME}__ 으로 표현하겠습니다.

### ssh 키 생성하기

데모를 진행하기 위해서는 ssh 키(public key/private key)가 필요합니다. ssh 키는 __{PROEJCT_HOME}__ 에 생성합니다. ssh 키를 생서은 다음 문서를 참조하시기 바랍니다.

- [윈도우, 리눅스, 맥에서 ssh 보안키 생성](http://www.oracloud.kr/post/ssh_key/)
  - ssh-keygen을 이용하여 ssh 보안키 생성 방법 소개
  - PuTTY Key Generator 사용법 소개

생성한 ssh 키파일은 id_rsa.pub와 id_rsa로 가정합니다.

### Oracle Cloud 접속 정보 확보

데모를 진행하기 위해서는 Oracle Cloud 계정을 확보해야 합니다. 다음과 같은 계정 정보를 확보해야 합니다.

- Oracle Cloud 계정
- Oracle Cloud 로그인 패스워드
- Identity Domain 명
- REST Endpoint URL

<그림 1>과 같이 대시보드에서 서비스 세부정보 페이지로 이동하여 계정, Identity Domain, REST Endpoint URL을 확인할 수있습니다.


- <그림 1> 오라클 계정, Identiy Domain, REST Endpoint URL 정보 확인
![](https://oracloud-kr-teamrepo.github.io/2017/08/oraclecode_ktw/img15.jpg)
![](https://oracloud-kr-teamrepo.github.io/2017/08/oraclecode_ktw/img20.jpg)

## 데모

단순히 VM을 한 개 생성하는 것과 Haproxy를 활용한 LB 사이트를 구성하는 2개를 준비하였습니다.

### Case 1: 1개 VM 생성

- __{PROEJCT_HOME}__/case01/terraform 으로 이동합니다.

__{PROEJCT_HOME}__/case01/terraform/variable.tf를 <그림 1> 정보를 확인하여 다음과 같이 수정합니다.

```
variable user {
  type    = "string"
  default = "taewanme@gmail.com"
}

variable password {
  type    = "string"
  default = "@Test123M@"
}

variable domain {
  type    = "string"
  default = "a514681"
}

variable endpoint {
  type    = "string"
  default = "https://compute.uscom-central-1.oraclecloud.com/"
}

## ssh public key 위치: 파일 기준 상대 경로 지원
variable ssh_public_key_file {
  description = "ssh public key"
  default     = "../../id_rsa.pub"
}
```

variable.tf 파일 수정이 완료되면 __{PROEJCT_HOME}__/case01/terraform 에서 다음과 같은 명령을 수행합니다.

```
$ terraform plan
$ terraform apply
$ terraform show
```

### Case 2: Haproxy를 활용한 LB 사이트

#### Infra Build

__{PROEJCT_HOME}__/case02/terraform 로 이동하고 variable.tf 파일을 case 1와 동일한 방법으로 업데이트 합니다. variable.tf를 수정한 후 다음과 같은 명령을 수행합니다.

```
$ terraform plan
$ terraform apply
$ terraform show
```

수행이 완료되면 <그림 2>와 같이 서버 목록을 확인할 수 있습니다.

- <그림 2> 5개 서버 빌드 결과 (Oracle Cloud 대시보드에서 서버 목록으로 이동)
![](https://oracloud-kr-teamrepo.github.io/2017/08/oraclecode_ktw/img10.jpg)
![](https://oracloud-kr-teamrepo.github.io/2017/08/oraclecode_ktw/img30.jpg)

#### Configuration Management

"__{PROEJCT_HOME}__/case02/ansible" 로 이동합니다. <그림 2>의 공개 IP를 참조하여 host 파일을 업데이트 합니다.

다음은 host 파일을 업데이트한 예제 입니다.

```
[webservers1]
129.150.82.135

[webservers2]
129.150.76.149

[dbservers]
129.150.85.162

[lbservers]
129.150.85.87

[monitoring]
129.150.85.99
```

host 파일 수정이 완료되면 "__{PROEJCT_HOME}__/case02/ansible"에서 다음은 명령을 수행합니다.

```
$ export ANSIBLE_HOST_KEY_CHECKING=False
$ ansible-playbook -i hosts site.yml --key-file=../../id_rsa
```

#### Server Testing

testinfra를 사용하기에 앞서 ssh 설정을 선행해야 합니다. ssh 커넥션이 사용할 인증서를 지정하는 설정입니다.

__~/.ssh/config__ 파일에 다음과 같은 설정을 추가합니다. 테스트할 서버의 IP를 __129.150.76.149__ 로 가정합니다.


```
Host 129.150.76.149
   User opc
   IdentityFile __{PROEJCT_HOME}__/oracloud_rsa
```

"__{PROEJCT_HOME}__/case02/testinfra"에 이동하고 다음과 같은 명령을 수행합니다.
hosts 옵션에 설정에는 앞에서 만들어진 wegbservers1의 공개 IP를 사용합니다.

```
$ testinfra --connection=ssh --hosts=129.150.76.149 --sudo test.py
```

예상 결과는 다음과 같습니다.

![](https://oracloud-kr-teamrepo.github.io/2017/08/oraclecode_ktw/img40.jpg)

## summary

"Vagrant, Terraform, Ansible을 활용한 클라우드 인프라 관리법"의 데모는 여기까지 입니다. 데모는 youtube 동영상으로 공개할 예정입니다. 데모 실습에 질문이 있으실 경우 댓글을 남겨 주시거나 메뉴의 "오라클 클라우드 챗팅룸(Gitter)"에 질문을 남겨 주시기 바랍니다. 감사합니다.
