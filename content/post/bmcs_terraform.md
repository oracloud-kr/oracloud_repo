+++
author = "jisun.kang"
categories = ["iaas"]
date = "2017-04-21T17:15:48+09:00"
description = "BMCS 환경에서 Terraform을 사용하는 방법에 대한 문서입니다"
language = "bsh"
tags = ["devops", "BMCS", "IaaS"]
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2017/04/terraform/logo.png"
thumbnailInPost = ""
title = "Oracle BMCS에서 Terraform을 이용한 관리 자동화 구축"

+++

다음은 Oracle IaaS BMCS 환경에서 Terraform을 사용하여 자동화(Infrastructure as Code)하는 방법에 대해서 가이드 하는 문서입니다.

Terraform에 대한 문서는 <a href="/post/terraform/">이곳</a>를 참고하십시오.

## 1. BMCS에서 Terraform을 사용하기 위한 환경 구성

BMCS에서 Terraform 을 사용하기 위해서는 먼저 VM을 생성한 후 해당 VM에 Terraform provider를 설치해야 합니다.

### Terraform Provider 설치

BareMetal 용 Terraform Provider 는 <a href="https://github.com/oracle/terraform-provider-baremetal/releases">이곳</a>에서 해당 OS버전에 맞추어서 다운로드가 가능합니다. Linux용은
https://github.com/oracle/terraform-provider-baremetal/releases/download/v1.0.3/linux.tar.gz 에서 다운로드 받으실 수 있습니다.

원하는 위치에 linux.tar.gz을 풀고, Home directory에 다음과 같은 ~/.terraformrc 파일을 생성합니다. path_to_provider_binary 부분에 Terraform Provider를 설치한 디렉토리를 적어주시면 됩니다.

```
providers {
	baremetal = "path_to_provider_binary/terraform-provider-baremetal"
}
```

### BMCS 관련 설정 - API 용 key 생성 및 등록

SDK와 Tool을 사용하기 위해서는 API를 위한 PEM(Privacy Enhanced Mail)형식의 RSA Key pair가 필요합니다. BMCS Instance에 접속하기 위한 Key가 아닌 API 사용을 위한 Key입니다. SDK와 Tool에서 기본적으로 사용하는 디렉토리 위치는 ~/.oraclebmc 입니다.

Private key 생성하는 절차는,

```
$ mkdir ~/.oraclebmc
$ openssl genrsa –out ~/.oraclebmc/bmcs_api_key.pem 2048
$ chmod go-r ~/.oraclebmc/bmcs_api_key.pem
```

Public key를 생성하는 절차는 아래와 같습니다.

```
$ openssl rsa –pubout –in ~/.oraclebmc/bmcs_api_key.pem –out ~/.oraclebmc/bmcs_api_key_public.pem
```

화면에 표시된 Public Key를 복사한 후,

```
$ cat ~/.oraclebmc/bmcs_api_key_public.pem
```

Key의 fingerprint를 확인합니다.

```
$ openssl rsa –pubout –outform DER –in ~/.oraclebmc/bmcs_api_key.pem | openssl md5 -c
```

SDK와 API사용 시 사용할 Public Key는 다음과 같은 화면에서 등록하고,

![](https://oracloud-kr-teamrepo.github.io/2017/04/bmcs_terraform/bmcsconsole1.jpg)

등록한 key는 아래처럼 확인 가능합니다.

![](https://oracloud-kr-teamrepo.github.io/2017/04/bmcs_terraform/bmcsconsole2.jpg)

### BMCS 오브젝트에 대한 ID 확인

BMCS의 모든 Object들은 OCID라는 Unique한 ID를 가지고 있습니다. SDK와 Cli에서 모든 Operation은 OCID를 이용합니다. Tenancy의 OCID는 BMCS의 모든 페이지의 하단에서 확인할 수 있습니다.

![](https://oracloud-kr-teamrepo.github.io/2017/04/bmcs_terraform/bmcsconsole3.jpg)

User OCID는 Identity/Users 화면에서 확인 할 수 있습니다.

![](https://oracloud-kr-teamrepo.github.io/2017/04/bmcs_terraform/bmcsconsole4.jpg)

### Key 관련 정보 및 OCID 정보를 OS에 설정

Baremetal을 사용하기 위한 Credential 정보를 위의  ~/.bash_profile에 환경변수로 설정합니다.

```
export TF_VAR_tenancy_ocid=
export TF_VAR_user_ocid=
export TF_VAR_fingerprint=
export TF_VAR_private_key_path=<fully qualified path>
export TF_VAR_private_key_password=
```

## 2. BMCS에서 Terraform을 사용하여 환경 구성 (VCN 생성 및 인스턴스 생성)

다음은 BMCS의 주요 구성요소인 VCN을 생성하고 생성한 VCN에 인스턴스를 생성하는 예제입니다.

### VCN 생성

VCN 셩성시 사용하는 tf 파일을 살펴보면,

```
variable "tenancy_ocid" {}
variable "user_ocid" {}
variable "fingerprint" {}
variable "private_key_path" {}
variable "compartment_ocid" {}

provider "baremetal" {
  tenancy_ocid = "${var.tenancy_ocid}"
  user_ocid = "${var.user_ocid}"
  fingerprint = "${var.fingerprint}"
  private_key_path = "${var.private_key_path}"
}

resource "baremetal_core_virtual_network" "a_TF_managed_VCN" {
  cidr_block = "10.0.0.0/16"
  compartment_id = "${var.compartment_ocid}"
  display_name = "a_TF_managed_VCN"
}
```

아래의 명령어로 위의 파일을 사용하여 VCN을 생성합니다.

```
$terraform apply
```

아래의 명령어로 사전에 수행과정을 simulation 해 볼 수 있습니다.

```
$terraform plan
```

### Instance 생성

Instance 생성시 사용하는 tf 파일입니다.

```
variable "tenancy_ocid" {}
variable "user_ocid" {}
variable "fingerprint" {}
variable "private_key_path" {}
variable "compartment_ocid" {}


provider "baremetal" {
  tenancy_ocid = "${var.tenancy_ocid}"
  user_ocid = "${var.user_ocid}"
  fingerprint = "${var.fingerprint}"
  private_key_path = "${var.private_key_path}"
}

# Note that the difference between launching a VM and BM instance is just a different shape name.

resource "baremetal_core_instance" "BM_instance1" {
  availability_domain = "ZGmQ:PHX-AD-3"
  compartment_id = "${var.compartment_ocid}"
  display_name = "BM_instance1"
  image = "ocid1.image.oc1.phx.aaaaaaaazt3sfrz2lfbha6okihvh4bwaufikhilhsek43hpvzxitl47nv2bq"
  shape = "BM.DenseIO1.36"
  subnet_id = "ocid1.subnet.oc1.phx.aaaaaaaanvacprjbqaufwhibjkyolg27rmbgvxx372sbx64uxklvpdt4k3dq"
  metadata {
    ssh_authorized_keys = "ssh-rsa AAAAB3NzaC1yc2EAAAABJQAAAQEAqetTkzFEESkXr731zsRMKcUlGBvj6UPfPwjg0sIIGJMpVh1moQM1EBxAyWdr01+x1Ff8xxhUwhhV1uGndGJxoFRAJp4U9vPbr3iVPJlTyAlNCQI0ohlnfCR5XqSswVfqAqyGOzJcdaRgV7qhywUXJFky+yVuhclFoiljrRuposn4RQCKxklFxovysrozRYmyIGWR93VIkh8sfb8tarycpqigACLhLtENAzkVT2yJg72ZXNrrOaTjc89BJ/SGh4NO9jkqYRC6xXKRv7XuJfO8T8mUud6+p27LHdxw+PINOUOQmikb7ScjuYVP1gV94/A7cKMrwvZwfi2dT1FHT0Ic+w== rsa-key-20160624"
  }
}

resource "baremetal_core_instance" "BM_instance2" {
  availability_domain = "ZGmQ:PHX-AD-3"
  compartment_id = "${var.compartment_ocid}"
  display_name = "BM_instance2"
  image = "ocid1.image.oc1.phx.aaaaaaaazt3sfrz2lfbha6okihvh4bwaufikhilhsek43hpvzxitl47nv2bq"
  shape = "BM.DenseIO1.36"
  subnet_id = "ocid1.subnet.oc1.phx.aaaaaaaanvacprjbqaufwhibjkyolg27rmbgvxx372sbx64uxklvpdt4k3dq"
  metadata {
    ssh_authorized_keys = "ssh-rsa AAAAB3NzaC1yc2EAAAABJQAAAQEAqetTkzFEESkXr731zsRMKcUlGBvj6UPfPwjg0sIIGJMpVh1moQM1EBxAyWdr01+x1Ff8xxhUwhhV1uGndGJxoFRAJp4U9vPbr3iVPJlTyAlNCQI0ohlnfCR5XqSswVfqAqyGOzJcdaRgV7qhywUXJFky+yVuhclFoiljrRuposn4RQCKxklFxovysrozRYmyIGWR93VIkh8sfb8tarycpqigACLhLtENAzkVT2yJg72ZXNrrOaTjc89BJ/SGh4NO9jkqYRC6xXKRv7XuJfO8T8mUud6+p27LHdxw+PINOUOQmikb7ScjuYVP1gV94/A7cKMrwvZwfi2dT1FHT0Ic+w== rsa-key-20160624"
  }
}
```
