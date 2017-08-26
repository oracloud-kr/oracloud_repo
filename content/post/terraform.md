+++
author = "jisun.kang"
categories = ["iaas"]
date = "2017-04-01T17:15:48+09:00"
description = "자동화 구축시 사용하는 devops 툴인 terraform에 대해서 소개하는 문서입니다"
language = "bsh"
tags = ["devops", "iaas", "automation", "IAC"]
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2017/04/terraform/logo.png"
thumbnailInPost = ""
title = "IaaS Provisioning 툴: Terraform"
+++

## Terraform이란?

Terraform은 인프라를 만들고 바꾸고 버전 관리하는 도구입니다. Terraform은 Infrastructure as Code, Execution Plans, Resource Graph, Change Automation 와 같은 기능을 제공합니다.

이 기능을 이용해서 Infrastructure를 관리할 수 있는데 사용할 수 있는 Infrastructure를 Terraform에서는 Provider라고 부릅니다. Terraform이 지원하는 Provider는 Oracle Baremetal, Oracle Public Cloud, AWS, BitBucket, Chef, CloudFlare, Consul, DigitalOcean, Docker, GitHub, Google Cloud, Grafana, InfluxDB, Heroku, Microsoft Azure, MySQL, PostgreSQL 등 개발에서 보통 사용하는 솔루션은 거의 모두 포함되어 있습니다.

각 Provider에 맞게 설정을 해주어야 하고 추상화가 되어 있어서 하나를 설정하면 Provider를 바꾸어서 사용할 수 있는 개념은 아닙니다. 이러한 Terraform을 사용하면 서로 다른 Cloud를 하나의 툴을 사용하여 관리가능하여, Oracle OPC와 Bare Metal 모두를 하나의 툴로 관리가능합니다.

## Terraform 설정시 사용하는 언어

- HashiCorp configuration language(HCL)

Terraform의 설정 파일은 HashiCorp가 만든 설정 언어인 HCL을 사용하고 있는데 이는 Teffrform 형식인 .tf와 JSON 형식인 .tf.json을 모두 사용할 수 있습니다. 보통 작성할 때는 .tf를 사용하고 자동 생성되는 설정에서 JSON을 사용합니다. HCL은 사람이 읽기 좀 더 쉬운 구조로 되어 있고 주석을 사용할 수 있는 등의 장점이 있습니다.

지정한 폴더에 .tf와 .tf.json를 넣어두면 Terraform이 알파벳 순서로 로드하는데 변수나 리소스 정의의 순서는 상관이 없습니다. 즉, 로드 순서를 신경 써서 알파벳 순서에 맞게 파일을 만들 필요는 없습니다. 그 외에 다른 설정 파일을 덮어쓸 수 있는 오버라이드 파일이 있는데 오버라이드 파일은 파일명이 override이거나 _override로 끝나야 하는데 이는 다른 파일을 다 로드하고 오버라이드 파일을 알파벳순으로 로드합니다.

### 시작하면서

![](https://oracloud-kr-teamrepo.github.io/2017/04/terraform/HCL1.jpg)

주석은 #, // 또는 /* */를 사용합니다. 3번 라인은 opc라는 변수를 선언한 것으로 값 할당은 key = value형식을 사용하는데 여기서 value는 문자열, 숫자, Boolean, 리스트, 맵의 형식을 사용할 수 있습니다. 문자열은 쌍따옴표를 사용하고 String Interpolation에는 ${}문법을 사용합니다. 멀티라인 문자열은 heredoc 스타일로 <<EOF, EOF를 사용하여 코드 안에 파일안에 바로 문자열의 내용을 집어넣습니다.

###  리소스 설정시

```
resource TYPE NAME {
	CONFIG ...
	[count = COUNT]
	[depends_on = [RESOURCE NAME, ...]]
	[provider = PROVIDER]
	[LIFECYCLE]

	[CONNECTION]
	[PROVISIONER ...]
}
```

resource가 키워드이고 TYPE는 프로바이더에 맞게 Terraform에서 정의한 리소스의 타입 이름입니다. NAME은 개발자가 임의로 주면 되는 이름입니다. 2번 라인의 CONFIG는 KEY = VALUE형식이나 KEY { CONFIG }형식이 됩니다. 그래서 OPC 인스턴스를 정의하는 리소스 설정의 예시를 보면 다음과 같습니다.

아래의 예제는 Microsoft_Windows_Server_2012_R2 이미지를 이용하여 OC3 Shape로 OPC Instance를 정의한 것입니다.


![](https://oracloud-kr-teamrepo.github.io/2017/04/terraform/HCL2.jpg)

### Provider 설정시

```
provider NAME {
	CONFIG ...
	[alias = ALIAS]
}
```

Provider의 정의형식은 다음과 같고 OPC Provider를 설정하면 다음과 같이 설정합니다.

![](https://oracloud-kr-teamrepo.github.io/2017/04/terraform/HCL3.jpg)

### 데이터 소스 설정시

데이터 소스를 사용하면 Terraform 구성의 다른 곳에서 사용하기 위해 데이터를 가져 오거나 계산할 수 있습니다. 데이터 소스를 사용하면 Terraform 구성을 Terraform 외부에서 정의 된 정보를 토대로 만들거나 다른 별도의 Terraform 구성으로 정의 할 수 있습니다.

![](https://oracloud-kr-teamrepo.github.io/2017/04/terraform/HCL4.jpg)

### Terraform 변수 선언시

```
variable NAME {
	[type = TYPE]
	[default = DEFAULT]
	[description = DESCRIPTION]
}
```

변수 선언 문법은 위와 같습니다. 이렇게 설정한 변수를 다른 설정에서 사용할 수 있고 CLI에서 변수를 덮어 쓸 수도 있습니다. Type은 변수의 타입인데 type = “string”과 같이 정의해줘도 되지만 정의하지 않으면 Terraform에서 알아서 추론합니다. default가 변수 생성시의 기본으로 주어지는 값입니다.

.tf 파일에 공통변수(access key, secret key, region..)를 설정하는 예제입니다.

```
variable NAME {
	[type = TYPE]
	[default = DEFAULT]
	[description = DESCRIPTION]
}
```

CLI를 통하여 변수값을 할당하고자 할때는

```
$ terraform plan \
	-var 'access_key=foo' \
	- var 'secre_key=bar'

...
```

.tf 파일을 이용하여 변수값을 할당하고자 할때는 파일 안에 다음과 같이 입력하면 됩니다.

```
user = "john.dow@oracle.com"
password = "xxx"
domain = "a332567"
endpoint - "..."
administrator_password = "xxx"
```

## Terraform 설치

Terraform 다운로드 페이지( https://www.terraform.io/downloads.html )에서 OS에 맞게 다운로드 받아서 PATH에 넣으면 됩니다. 해당 문서를 작성시의 최신 버전은 v0.8.8 입니다.

![](https://oracloud-kr-teamrepo.github.io/2017/04/terraform/HCL5.jpg)

정상적으로 설치했으면 버전을 확인할 수 있고 사용할 수 있는 명령어를 볼 수 있습니다.

Terrmform을 설치하신 후 Oracle Cloud에서 Terraform을 설정하고자 하신다면 <a href="/post/bmcs_terraform/">여기</a>에서 관련 정보를 확인하실 수 있습니다. 해당 문서는 BMCS에서 Terraform을 설정하는 방법에 대해서 설명하고 있는 문서입니다.
