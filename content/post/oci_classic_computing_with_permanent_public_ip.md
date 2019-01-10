+++
description = "오라클 클라우드의 OCI Classic의 VM에 고정 Public IP를 설정하는 방법을 소개합니다."
title = "OCI Classic: Compute 서비스, VM의 고정 Public IP 지정"
categories = ["cloud"]
thumbnailInPost = "https://taewanmerepo.github.io/2018/04/publicip/post.jpg"
thumbnailInList = "https://taewanmerepo.github.io/2018/04/ociclass/list.jpg"
date = "2018-04-30T11:59:47+09:00"
language = ""
tags = ["oracle", "oci-c", "vm", "pubic ip", "김태완"]
author = "taewan.kim"
adsense = "true"
+++

[OCI Classic: Compute 서비스, VM 생성](/post/oci_classic_computing/) 문서를 참고하여 VM을 생성하면 Public IP가 할당됩니다. 이 Public IP를 이용하여 외부에서 VM에 접근이 가능합니다. 그러나 이렇게 VM을 만들면, VM을 재시작할 때마다 <그림 1>과 같이 Public IP가 변경됩니다. Public IP가 자주 변경되면, 여러 서버를 구성 할 때 관리가 복잡해집니다.

{{< img src="https://taewanmerepo.github.io/2018/04/publicip/img01.jpg"
title="그림 1"
caption="OCI-Classic VM의 Public IP 변경: {129.152.141.152 => 129.152.141.117}" >}}

앞에서 만든 VM을 재시작하면 Public IP가 변경되는 이유는 VM을 생성할 때, Network 설정 단계에서 <그림 2>와 같이 "__Public IP Address__"가 "__Auto Generation__"으로 설정했기 때문입니다. 고정된 Public IP를 사용해야 한다면 이 설정을 변경해야 합니다.

{{< img src="https://taewanmerepo.github.io/2018/04/publicip/img05.jpg"
title="그림 2"
caption="VM 생성시 Public IP 지정" >}}

본 문서에서는 예약 공개 IP(Reserved Public IP)를 설정하는 방법을 소개합니다.

## 예약 공개 IP(Reserved Public IP) 등록

OCI Classic의 Compute Service에서 예약 공개 IP를 설정하는 방법은 2가지 입니다. 두 가지 방법 중에서 "__OCI Classic Compute 서비스 콘솔에서 생성__"하는 방법을 먼저 다루겠습니다. 예약 공개 IP 관리는 OCI Classic Compute 서비스 콘솔의 네트워크 텝에서 제공됩니다. <그림 3>과 같이 네트워크 텝 -> IP Reservations 메뉴로 고정 IP 관리 페이지로 이동 가능합니다. 이 페이지에서 기존에 관리하는 IP 목록이 출력됩니다. 여기에서 "Create IP Reservation" 버튼을 클릭하여 고정 Public IP를 등록할 수 있습니다.

{{< img src="https://taewanmerepo.github.io/2018/04/publicip/img020.jpg"
title="그림 3"
caption="OCI Classic Compute 서비스 콘솔에서 IP 예약 페이지 이동" >}}

<그림 3>에서 "Create IP Reservation" 버튼을 클릭하면 <그림 4>와 같이 "__Create Public IP Reservation__" 등록 폼이 출력됩니다. 여기에서 예약 공개 IP를 구분하는 IP 이름을 설정하고 "Create" 버튼을 클릭하면 고정 IP가 생성됩니다.

{{< img src="https://taewanmerepo.github.io/2018/04/publicip/img030.jpg"
title="그림 4"
caption="예약 공개 IP 등록" >}}

<그림 4>에서 IP 명을 입력하고 "Create" 버튼을 클릭하면 고정 IP가 생성되고, <그림 5>와 같이 관리 목록에 생성된 IP가 출력됩니다.

{{< img src="https://taewanmerepo.github.io/2018/04/publicip/img040.jpg"
title="그림 5"
caption="예약 Public IP 확인" >}}

## VM 생성시 예약 공개 IP 할당

<그림 6>과 같이 VM 생성의 Network 설정 단계에서 예약 고정 IP를 설정할 수 있습니다. Public IP Address 설정을 __Auto Generation__에서 "__Persistent Public IP __"로 변경하면 이미 생성되어 관리되고 있는 예약 공개 IP 목록이 출력됩니다. 이 중에서 사용할 예약 공개 IP를 지정하면 됩니다. __Public IP Address__ 설정 항목 옆에 "__Create IP Reservation__"을 클릭하면 <그림 4>와 같은 예약 공개 IP 생성 폼이 출력됩니다. 여기에서 직접 예약 공개 IP를 생성할 수 있습니다.

{{< img src="https://taewanmerepo.github.io/2018/04/publicip/img050.jpg"
title="그림 6"
caption="VM 생성 Network 단계에서 예약 Public IP 할당 " >}}

## VM에 할당된 예약 공개 IP 테스트

앞에 설정으로 만들어진 VM은 "public_ip_for_test"라는 이름의 예약 공개 IP가 지정된 상태이며, Public IP는 129.152.141.152입니다. 이 VM을 재실행하여 Public IP가 고정되어 있는지를 다음 <그림 7>과 같이 확인할 수 있습니다.

{{< img src="https://taewanmerepo.github.io/2018/04/publicip/img060.jpg"
title="그림 7"
caption="VM 생성 Network 단계에서 예약 Public IP 할당 " >}}

<그림 7>에서 VM의 Public IP가 변경되지 않았음을 확인할 수 있습니다.

## 요약

OCI Classic: Compute 서비스의 VM에 예약 공개 IP를 설정하는 방법을 알아보았습니다. VM을 생성하면 각 VM에는 Private IP가 필수로 할당되고, Public IP는 선택적으로 할당됩니다. Shared Network를 사용할 때, Private IP는 클라우드 사용자의 도메인별로 관리 됩니다. Private IP와 Public IP는 기본 설정이 VM 재시작 시 매번 변경되는 것입니다. 서버간이 내부 구성을 위하여 Private IP와 Public IP를 고정해야 한다면 각각 다음과 같은 방법을 사용해야 합니다.

- Private IP
  - VM 생성 시 DNS Domain Prefix에 hostname을 지정
  - 이 Hostname은 DNS에 관리하여 Private IP변경에 대응
  - 내부 구성 시 Hostname을 사용
- Public IP
  - 예약 공개 IP를 사용하여 VM 재시작 시, 변경 방지
  - VM 생성 시, Network 단계에서 설정

## Oracle Cloud 관련 문서

> - Oracle Cloud 공통
>   - [Oracle Cloud IaaS: OCI vs OCI Classic](/post/oci_and_oci_classic/)
>   - [Oracle Cloud 트라이얼 신청 절차 (2018.04)](/post/oracle_cloud_trial/)
>   - [윈도우, 리눅스, 맥에서 ssh 보안키 생성](/post/oci_classic_computing/)
> - Oracle Cloud Infrastructure Classic
>   - [OCI Classic: Compute 서비스, VM 생성](/post/oci_classic_computing/)
>   - [OCI Classic: Compute 서비스, VM의 고정 Public IP 지정](/post/oci_classic_computing_with_permanent_public_ip/)
