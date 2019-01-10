+++
date = "2017-10-03T21:28:14+09:00"
description = "Terraform으로 오라클 클라우드에 파이썬 기반 Machine Learning 환경을 설치하는 installer를 만들었습니다."   
title = "Terraform Jupyter Installer: Machine Learning 환경 프로비저닝 "   
thumbnailInList = "https://oracloud-img-repo.github.io/2017/10/terraform_jupyter/list.jpg"
thumbnailInPost = "https://oracloud-img-repo.github.io/2017/10/terraform_jupyter/post.jpg"
tags = ["terraform", "테라폼", "oracle cloud", "오라클 클라우드", "jupyter", "machine learning"]
categories = ["Oracle Cloud"]
author = "taewan.kim"
language = ""  
+++

Terraform은 클라우드 인프라의 생성, 변경 및 형상 버전을 관리하는 툴입니다. Terraform을 이용하면 클라우드 자원을 효과적으로 사용하고 관리할 수 있습니다. Terraform을 활용하여 오라클 클라우드의 쉬운 접근법 제시를 목적으로 "__Terraform Installer On Oracle Cloud__"[^1] 프로젝트를 준비하고 있습니다. Terraform으로 자원 할당, VM 생성, 소프트웨어 설치, 보안룰 적용 등이 완료된 완전한 형태의 인프라를 제공하여, 신규 오라클 클라우드 사용자가 오라클 클라우드에 접근을 쉽게 하고, 활용 폭을 넓히는 것을 목표로 합니다.

[^1]: github에서 커뮤니티 프로젝트로 진행할 예정입니다.

"__Terraform Installer On Oracle Cloud__" 프로젝트의 파일럿 형태로 "__[Terraform Jupyter Installer](https://github.com/taewanme/terraform-jupyter-installer)__"를 만들었습니다. 최근에 Machine Learning과 Deep Learning은 업계의 핵심 키워드로 주목받고 있습니다. Machine Learning과 Deep Learning을 경험하기 위해서는 데이터 분석 컴퓨터 환경이 필요합니다. 파이썬을 기반으로 Machine Learning과 Deep Learning 환경을 구성할 경우 jupyter를 사용하는 것이 일반적입니다. 컴퓨터에 파이썬과 Jupyter를 설치하고 10여 종의 ML 및 DL 파이썬 라이브러리를 설치합니다. 사실 이러한 환경 구성 작업은 반복적이고 번잡한 작업이며, 반드시 컴퓨팅 파워가 필요합니다

오라클 클라우드는 트라이얼 계정 사용자에게 월 300불 Credit을 제공합니다. 트라이얼 계정 사용자는 한 달 동안 300불 Credit을 오라클 클라우드에서 자유롭게 사용할 수 있습니다. Oracle Cloud Infrastructure Classic을 기준으로 1 OCPU[^2]로 오라클 리눅스로 VM을 생성할 경우 비용은 시간당 0.1불입니다. 4 OCPU(메모리: 60GB) 서버의 1달(744시간) 사용 비용은 297불입니다. 트라이얼 사용자의 경우 과금은 300불 Credit에서 분단위로 차감됩니다.

파이썬 기반의 기계학습 및 딥러닝 개발 환경이 필요한 분들을 위해서 오라클 클라우드에 Jupyter 기반의 데이터 분석 환경을 만드는 "__Terraform Jupyter Installer__[^3]" 제공한다면 유용할 것 같습니다. 특히 오라클 클라우드 트라이얼 계정을 사용한다면 추가 비용 없이 완전한 데이터 분석 환경을 확보할 수 있습니다.

[^2]: OCPU는 Oracle Compute Processing Unit의 약자로, 오라클 클라우드에서 CPU의 단위입니다. OCPU는 Intel Xeon 프로세서의 물리적 코어 1개와 동일한 CPU 용량을 제공하며 하이퍼 스레딩을 지원합니다. 각 OCPU는 vCPU라고 하는 하드웨어 실행 스레드 2개에 해당합니다. 각 OCPU 별로 범용 구성은 최대 15GB의 RAM으로 프로비저닝될 수 있습니다.

[^3]: "__Terraform Jupyter Installer__"의 github 저장소 주소는 https://github.com/taewanme/terraform-jupyter-installer 입니다.


"__Terraform Jupyter Installer__"가 오라클 클라우드에 데이터 분석 환경을 만드는 시간은 약 7-8분 정도입니다. 지금부터 "__Terraform Jupyter Installer__"를 이용하여 오라클 클라우드에 서버를 빌드하는 과정을 살펴보겠습니다.


## Terraform Jupyter Installer의 역할

Terraform Jupyter Installer는 Oracle Cloud Infrastructure Classic에 Machine Learning 및 Deep Learning을 위한 파이썬 기반 데이터 분석 환경을 구성하는 Installer입니다.

Terraform Jupyter Installer는 다음과 같은 절차를 자동화합니다.

- Oracle Cloud Infrastructure Classic에 VM 생성
- Security Application 등록
  - jupyter: 8888
  - tensorboard: 8008
- Security List 생성: jupyter_list
- Security Rule 생성
  - Allow-Jupyter-http-access
  - Allow-Tensorboard-http-access
  - Allow-Jupyter-ssh-access
- VM에 소프트웨어 설치
  - Python3 설치
  - Utility 설치
  - Python Libaray 설치: Jupyter, numpy, pillow, matplotlib, scikit-learn, Pandas, scrapy, NLTK, bokeh, NetworkX, scipy, Seaborn, beautifulsoup4, keras, tensorflow
- Jupyter 환경 설정
- Jupyter 서비스 등록
- Jupyter 서비스 시작

## 사전 준비 사항
Terraform Jupyter Installer를 사용하기 위해서는 다음과 같은 4가지가 선행되어야 합니다.

- 오라클 클라우드 계정 생성
- 작업 컴퓨터에 git 설치
- 작업 컴퓨터에 SSH 키 생성 (private/public key)
- 작업 컴퓨터에 Terraform 설치

### 오라클 클라우드 계정

오라클 계정이 없다면 "[Oracle Cloud Trial 신청: $300 Credit](http://www.oracloud.kr/post/oracle_cloud_reg/)" 문서를 참조하여 트라이얼 계정을 생성하시기 바랍니다.

### 작업 컴퓨터에 git 설치

Terraform Jupyter Installer를 github 저장소에서 내려받기 위한 용도입니다. git이 작업 컴퓨터에 없다면 다음 문서를 참조하여 설치하시기 바랍니다.

- [git 설치하기](http://library1008.tistory.com/51)

### 작업 컴퓨터에 SSH 키 생성

Oracle Cloud Infrastructure Classic에 VM을 생성하기 위해서는 SSH Key가 필요합니다. 앞으로 사용할 SSH Key가 없는 상태라면 다음 페이지를 참조하여 SSH Key를 미리 만들어야 합니다.

- [윈도우, 리눅스, 맥에서 ssh 보안키 생성하기](http://www.oracloud.kr/post/ssh_key/)

SSH public key 파일과 SSH private key 파일 위치는 ssh_public_key_file과 ssh_private_key_file에 등록됩니다.

### 작업 컴퓨터에 terraform 설치

Terraform 다운로드 URL은 https://www.terraform.io/downloads.html 입니다. Terraform은 Go-lang으로 개발되어 있습니다. Terraform은 1개의 실행 파일로 구성되어 있습니다. Terraform 홈페이지 다운로드 사이트[그림 1 참조]에서는 Mac OS X, FreeBSD, Linux, OpenBSD, Solaris, Windows용 실행파일을 제공합니다. 데모에 사용할 컴퓨터의 운영체제와 비트에 맞는 파일을 내려받고, Zip 파일 포맷으로 제공된 파일의 압축을 풀면 “terraform” 실행 파일이 생깁니다. 이 실행 파일의 위치를 PATH 환경 변수에 추가하는 것으로 Terraform 설치는 완료됩니다.

{{< img src="https://oracloud-img-repo.github.io/2017/10/terraform_jupyter/img010.jpg"
title="그림 1"
caption="Terraform 다운로드 페이지 " >}}

## Terraform Jupyter Installer를 이용한 Jupyter VM 생성

Terraform Jupyter Installer를 실행하는 절차는 크게 3 부분으로 구분할 수 있습니다.

1. github 저장소 클론
1. {REPOSITORY_HOME}/variable.tf 파일 수정: 변수 설정
1. terraform apply 실행

### Step 1: github으로 부터 저장소 복제

Terraform Jupyter Installer의 github 저장소 위치는 https://github.com/oracloud-kr/terraform-jupyter-installer 입니다.
다음 명령으로 github 저장소를 복제합니다.

```
git clone git@github.com:oracloud-kr/terraform-jupyter-installer.git
```

### Step 2: variable.tf 파일 수정

앞에서 github으로 부터 내려받은 코드는 terraform을 실행하는 스크립트입니다.
terraform을 정상적으로 동작시키기 위해서는 6개의 변수를 설정해야 합니다.
변수는 {REPOSITORY_HOME}/variable.tf 파일에 정의되어 있습니다.

{REPOSITORY_HOME}/variable.tf에 정의된 변수는 다음과 같습니다.

|변수명|설명|예제|
|---|---|---|
|user|오라클 클라우드 계정 명|okcode@naver.com|
|password|오라클 클라우드 로그인 패스워드|Welcome1!|
|domain|identity domain 명|krspider|
|endpoint|REST Endpoint URL|https://api-z52.compute.us6.oraclecloud.com/|
|ssh_public_key_file|작업 컴퓨터에 SSH public key 파일 위치|/Users/taewan/id_rsa.pub|
|ssh_private_key_file|작업 컴퓨터에 SSH private key 파일 위치|"/Users/taewan/id_rsa|

#### user & passowrd 설정

오라클 클라우드의 계정명과 패스워드를 variable.tf 파일의 user와  password를 설정합니다.

#### domain & endpoint 설정

<그림 2>와 같이 오라클 클라우드 대시보드에서 Compute Classic 세부 정보 페이지 이동합니다.
<그림 3>과 Compute Classic 세부 정보 페이지에서 도메인과 endpoint 정보를 확인하고
variable.tf 파일의 domain과  endpoint에 설정합니다.

{{< img src="https://oracloud-img-repo.github.io/2017/10/terraform_jupyter/img020.jpg"
title="그림 2"
caption="오라클 클라우드 대시보드에서 Compute Classic 세부 정보 페이지 이동" >}}

{{< img src="https://oracloud-img-repo.github.io/2017/10/terraform_jupyter/img030.jpg"
title="그림 3"
caption="Compute Classic 세부 정보 페이지에서 도메인명과 REST Endpoint 정보 확인" >}}

#### ssh_public_key_file & ssh_private_key_file

앞에서 작업 컴퓨터에 생성한 SSH 키 파일의 파일명을 포함한 절대 경로를 variable.tf 파일의 ssh_public_key_file과 ssh_private_key_file에 설정합니다.

다음은 variable.tf 파일의 설정 예입니다.

```

variable user {
  type    = "string"
  default = "okcode@daum.net"
}

variable password {
  type    = "string"
  default = "Welcome1!"
}

variable domain {
  type    = "string"
  default = "krspider"
}

variable endpoint {
  type    = "string"
  default = "https://api-z52.compute.us6.oraclecloud.com/"
}

variable ssh_public_key_file {
  description = "ssh public key"
  default     = "/Users/taewan/id_rsa.pub"
}

variable ssh_private_key_file {
  description = "ssh private key"
  default     = "/Users/taewan/id_rsa"
}

```

### Step 3: Terraform 수행

github 저장소에서 복제한 프로젝트의 최상위 디렉터리에서 "__terraform apply__"을 수행하면 오라클 클라우드에 Jupyter와 Machine Learning을 위한 Jupyter가 설치된 VM이 생성됩니다.

```
> pwd
/Users/taewan/demo/jupyter-terraform
> terraform apply
```

명령 수행 결과(로그)는 다음과 같습니다. 실행 시간은 약 7~8분 정도가 소요됩니다.

```
> terraform apply
opc_compute_security_application.tensorboard: Creating...
 dport:    "" => "8008"
 name:     "" => "tensorboard"
 protocol: "" => "tcp"
opc_compute_security_application.jupyter: Creating...
 dport:    "" => "8888"
 name:     "" => "jupyter-8888"
 protocol: "" => "tcp"

 ## 중간 생략

 Apply complete! Resources: 9 added, 0 changed, 0 destroyed.

 The state of your infrastructure has been saved to the path
 below. This state is required to modify and destroy your
 infrastructure, so keep it safe. To inspect the complete state
 use the `terraform show` command.

 State path:

 Outputs:

 jupyter_url = http://129.144.150.109:8888
 ssh = ssh -i /Users/taewan/oracloud_rsa opc@129.144.150.109
 tensorborac_url = http://129.144.150.109:8008
>  
```

실행 결과 마지막 출력에 SSH 접속 방법과 jupyter 접속 정보를 확인할 수 있습니다.

## Jupyter 접속

위 실행 로그의 접속 정보로 jupyter를 접근할 수 있습니다. 접속 패스워드는 __Welcome1__ 입니다.
jupyter에 로그인하여 그림 4~6과 같이 jupyter를 즐길 수 있습니다.

{{< img src="https://oracloud-img-repo.github.io/2017/10/terraform_jupyter/img040.jpg"
title="그림 4"
caption="jupyter 접속: 패스워드 - Welcome1" >}}

{{< img src="https://oracloud-img-repo.github.io/2017/10/terraform_jupyter/img050.jpg"
title="그림 5"
caption="데모 jupyter 노트북: demo.ipython" >}}

{{< img src="https://oracloud-img-repo.github.io/2017/10/terraform_jupyter/img060.jpg"
title="그림 6"
caption="demo notebook" >}}


## Terraform Jupyter Installer 데모

Terraform Jupyter Installer 실행 데모는 다음 동영상을 통해서 확인할 수 있습니다.

{{< youtube vu0D4mOBQlw >}}


## 마치며

"__Terraform Jupyter Installer__"를 이용하여 오라클 클라우드에 ML/DL 서버를 배포하는 과정을 소개하였습니다.
"__Terraform Jupyter Installer__"의 개선 사항이 있으시거나 오류가 있다면 본문의 댓글로 남겨 주시거나 github 저장소에 메세지를 남겨주시기 바랍니다. github 주소는 다음과 같습니다.

- [Terraform Jupyter Installer](https://github.com/taewanme/terraform-jupyter-installer): https://github.com/taewanme/terraform-jupyter-installer
