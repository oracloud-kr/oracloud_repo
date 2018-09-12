+++
description = "오라클 클라우드 트라이얼은 사용자에게 30일간 300불 크래딧을 무료로 제공하는 프로그램입니다. 오라클 클라우드 트라이얼을 신청하는 절차를 소개합니다."
title = "Oracle Cloud 트라이얼 신청 절차 (2018.05.01 기준)"
categories = ["cloud"]
thumbnailInPost = "https://taewanmerepo.github.io/2018/04/oracloudtrial/post.jpg"
thumbnailInList = "https://taewanmerepo.github.io/2018/04/oracloudtrial/list.jpg"
date = "2018-05-01T11:59:47+09:00"
language = ""
tags = ["oracle", "cloud", "trial", "김태완"]
author = "taewan.kim"
adsense = "true"
+++

## 오라클 클라우드 트라이얼이란?

오라클 클라우드 트라이얼은 사용자에게 30일간 300불 크래딧을 무료로 제공하는 프로그램입니다. 사용자는 이 기간 동안 할당된 크래딧을 제약 없이 사용할 수 있습니다. 이 프로그램에서 자원 사용량은 시간 단위로 계산되고, 300불 크래딧에서 차감됩니다. 300불 크래딧이 모두 소진되면 오라클 클라우드 트라이얼 프로그램은 종료됩니다.

오라클 클라우드 트라이얼 신청 시 신용카드를 입력해야 합니다. 이 부분은 사용자 확인 절차일 뿐이며, 과금을 목적으로 입력하는 항목이 아닙니다. 사용자 확인 목적으로 1불을 결제하고 1분 후에 해당 결제를 취소합니다. 오라클 클라우드 트라이얼은 사용 기간과 크래딧이 한정된 서비스입니다. 30일 동안 제공된 크래딧을 초과하여 사용할 수 없습니다. 따라서 사용량 초과에 대한 신용카드 청구는 발생하지 않습니다.

## 오라클 클라우드 트라이얼 신청시 주의 사항

오라클 클라우드 트라이얼은 다른 클라우드 벤더와 마찬가지로, 사용자에게 1회 제공을 원칙으로 합니다. 따라서 오라클 클라우드 트라이얼 신청시 다음 세 가지 정보 입력에 주의해야 합니다. 다음 가이드는 공식적인 문서를 인용한 것이 아니며, 경험적인 가이드입니다.

|정보|설명|재사용|
|----|----|----|
|E-mail|E-mail 주소는 사용자 명으로 사용됩니다. <br/>이미 Trail 계정 발급에 사용한 E-mail을 신규 등록에 다시 사용할 수 없습니다.|불가|
|핸드폰 번호|핸드폰 번호는 SMS 인증에 사용되는 정보입니다. <br/>핸드폰 번호 역시 이전에 발급에 사용한 핸드폰을 다시 사용할 수 없습니다. <br/>핸드폰 번호당 Trial 계정은 1회만 지원됩니다. |불가|
|신용카드 |사용자 인증 용도로 사용됩니다. 원칙은 신용카드 역시 재사용 불가입니다. <br/> 그러나 간혹 재사용 가능한 경우도 있습니다. 가능한 재사용하지 말것을 권장합니다.|부분허용|

## 오라클 클라우드 트라이얼로 가능한 것?

오라클 클라우드 트라이얼은 시간 단위로 크래딧이 차감됩니다. 동시에 여러 서비스를 함께 사용할 수 있으며, 각 서비스에 사용중인 자원을 기준으로 크래딧을 차감합니다. 300불 크래딧을 기준으로 사용할 수 있는 서비스 종류와 최대 사용 시간(해당 서비스만 사용할 경우)은 다음과 같습니다.[^1]

[^1]:출처: https://cloud.oracle.com/en_US/tryit

> 최대 사용 시간은 해당 서비스 1 ocpu[^2] 비용을 기준으로 최대 사용시간을 계산하였습니다. 여러 ocpu로 서비스를 구성하는 경우에는 시용 시간을 ocpu수로 나눠야 합니다.

[^2]: ocpu는 오라클 클라우드의 cpu 자원을 할당하는 단위입니다. 1 ocpu는 아마존의 2 vcpu와 같은 크기입니다

|서비스명|설명|Trial 최대 사용 가능 시간|
|----|----|----|
|Database|오라클 데이터베이스의 관리형 클라우드 서비스|4TB, 720시간|
|Autonomous Data Warehouse|엑사데이터 기반 DW, |엑사데이터 스토리지 2TB, 3,338 시간|
|Java|관리형 웹로직 서비스|3,000시간|
|Compute|IaaS 컴퓨트 서비스, VM & Baremetal|4,000시간|
|Storage|오브젝트 스토리지와 블록 스토리지|5TB|
|Big Data - Compute|하둡과 스팍 클러스터|3,000시간|
|Container|더커 컨테이너|2,200시간|
|Application Runtime|컨테이너 기반 클라우드 런타임-Java, Node.js, Python, PHP, Ruby|5,760시간|
|Analytics|클라우드 분석 플랫폼|3,000시간|
|MySQL|관리형 MySQL 서비스|4TB, 720시간|
|Developer Cloud Service|CI/CD 워크플로우|기간 중 시간 제약 없음|
|Database Backup|오라클 데이터베이스 백업|8,000TB|
|Ravello|멀티 클라우드 VM|1,600시간|
|Mobile|모바일 앱과 챗봇을 위한 오라클 백엔드 서비스|150,000 request|
|API Platform|API Gateway|3,000시간|
|Management Cloud|모니터링 및 로그 분석 서비스|시간당 100 엔티티 기준 3,000시간|
|API Platform|API 공개 및 생성, 모니터링 서비스|API Gateway 기준 3,000시간|
|Load Balancing|트래픽 자동 부산, 확장성과 내결함성 제공|3,000 시간(시간당 100개 엔티티 기준)|
|CASB for SaaS|애플리케이션과 워크로드 용 클라우드 접근 브로커|187,500 시간-사용자기준|
|Messaging|분산 시스템을 위한 클라우드 기반 서비|8억개 메시지|
|GoldenGate|실시간 데이터 통합 및 복제|3,000시간|
|Event Hub|관리형 Kafka 서비스|3,000시간|
|Content and Experience Cloud|클라우드 기반 콘텐츠 허브|1TB|
|WebCenter Portal Cloud|클라우드 포탈|3,000시간|
|SOA Suite|SOA 애플리케이션 쾌속 개발|3,000시간|
|Internet of Things|IoT 앱 쾌속 개발|3,000시간|
|Data Integration|데이터 스트리밍, 이관 및 관제|3,000시간|
|Self-Service Integration|클라우드 작업 대행|30,000 레시피 수행|
|Visual Builder|비즈니스 애플리케이션 쾌속 개발|3,000시간|
|Data Hub|No SQL 서비스|3,000시간|

## 오라클 클라우드 트라이얼 절차

### 오라클 클라우드 트라이얼 등록 페이지 이동

오라클 클라우드 트라이얼을 신청하기 위해서는 Email, 핸드폰 번호, 신용 카드가 필요합니다. 앞에서 설명한 것처럼 오라클 클라우드 트라이얼 등록에 사용했던 Email과 핸드폰 번호를 재사용할 수는 없습니다. 오라클 클라우드 트라이얼 신청 페이지는 <그림 1>과 <그림 2>를 거쳐 이동합니다.

{{< img src="https://taewanmerepo.github.io/2018/04/oracloudtrial/img010.jpg"
title="그림 1"
caption="http://cloud.oracle.com" >}}

http://cloud.oracle.com 에서 "__Try for Free__"를 클릭하여, "Free Account" 페이지로 이동합니다. 여기에서 "Create a Free Account"를 클릭하면 오라클 클라우드 트라이얼 등록 페이지가 출력됩니다.

{{< img src="https://taewanmerepo.github.io/2018/04/oracloudtrial/img020.jpg"
title="그림 2"
caption="'Create Fee Account' 요청" >}}

<그림 3>은 오라클 트라이얼 등록 페이지입니다. 다음과 같이 4부분으로 구성되어 있습니다.

1. Account Details: 사용자 정보 입력 - 이름, 메일, 주소
1. Validation Code: 핸드폰 SMS 코드 인증
1. Credit Card Details: 신용카드 정보 입력(비자카드, 마스터카드)
1. Terms&Condition: 라이선스 및 사용권 동의

{{< img src="https://taewanmerepo.github.io/2018/04/oracloudtrial/img000.jpg"
title="그림 3"
caption="Oracle Cloud의 Trial 신청 폼" >}}

### 오라클 클라우드 트라이얼 등록 서식

Oracle Cloud의 Trial 신청 양식의 세부 등록 방법을 살펴보겠습니다.

<그림 1>은 사용자 정보와 검증 코드를 입력하는 부분입니다. 사용자 정보 입력 부분에서 각 항목은 다음과 같이 정리할 수 있습니다.

|항목|설명|일반적인 설정 값|
|---|---|---|
|Account Type|Company 및 Personal 모두 선택 가능합니다. <br/> company 사용 시 회사명을 입력해야 하고, 회사 email을 사용해야 합니다. |Personal|
|Cloud Account Name|앞으로 사용할 계정 명입니다. <br/> 사용자가 만들 가상 데이터 센터 이름으로 사용됩니다. |공백 없는 영문자로 설정|
|Default Data Resion|사용할 기본 데이터 센터 리전을 지정합니다.<br/> 반드시 "North America"를 선택합니다. <br/>다른 리전 사용 시 사용의 제한이 발생할 수 있습니다. |North America|
|First Name|사용자 이름||
|Last Name|사용자 성||
|Country|사용자의 국가를 지정하는 항목입니다. "Korea, Republic of"를 선택합니다.<br/>신용카드와 주소 우편번호 검증에 사용되는 항목입니다. <br/>대한민국에서 발급한 신용카드 사용할 경우 이 부분을 "Korea, Republic of"으로 지정해야 합니다. |Korea, Republic of|
|Address|영어 주소 입력입니다. 우편번호는 5자리 우편번호를 입력해야 합니다. <br/> 네이버 영문 주소 서비스를 이용하시면 편리합니다. <[네이버 영문 주소](https://search.naver.com/search.naver?where=nexearch&query=%EB%84%A4%EC%9D%B4%EB%B2%84+%EC%98%81%EC%96%B4%EC%A3%BC%EC%86%8C&ie=utf8&sm=tab_she&qdt=0) 이동> ||

사용자 정보의 입력 값으로 "Validation Code" 항목의 국가 코드와 핸드폰 번호 국제전화 번호가 설정됩니다. Mobile Number 항목에 SMS를 받을 핸드폰 번호를 입력합니다. 이 부분에서 핸드폰 번호를 입력할 때, 첫 번째 0을 생략하고 입력합니다. 예를 들어서 010-5455-1234는 __1054551234__로 입력해야 합니다.

{{< img src="https://taewanmerepo.github.io/2018/04/oracloudtrial/img100.jpg"
title="그림 1"
caption="사용자 정보와 SMS 인증 핸드폰 번호 입력" >}}

전화번호 입력 후, "Request Cod"를 클릭하면 <그림 2>와 같이 SMS 문자 메시지가 전송됩니다. 이 문자 메시지의 6자리 인증 코드가 인증번호입니다.

{{< img src="https://taewanmerepo.github.io/2018/04/oracloudtrial/sms.jpg"
title="그림 2"
caption="SMS 인증 번호" >}}

<그림 2>의 SMS 인증 번호를 <그림 3>의 "Validation Code" 항목에 입력하고 "Verify" 버튼을 클릭하여 인증을 완료합니다.

{{< img src="https://taewanmerepo.github.io/2018/04/oracloudtrial/img110.jpg"
title="그림 3"
caption="SMS 인증" >}}

SMS 인증이 정상적으로 완료되면 <그림 4>와 같이 'Add Payment Method' 버튼이 활성화됩니다. 이 버튼을 클릭하여 신용카드 등록을 진행해야 합니다. 여기서 등록되는 신용카드는 사용자 인증 목적이며 과금 용도로 사용되지 않습니다. 인증 목적으로 1불을 결제하고 승인 후 취소하는 방식을 사용합니다.

{{< img src="https://taewanmerepo.github.io/2018/04/oracloudtrial/img120.jpg"
title="그림 4"
caption="'Add Payment Method' 활성화" >}}

신용 카드 등록에 앞서 <그림 5>와 같이 주소를 변경할 것인지 여부를 묻습니다. 변경 사항이 없다는 "Use Original"을 선택하고 다음으로 넘어갑니다.

{{< img src="https://taewanmerepo.github.io/2018/04/oracloudtrial/img130.jpg"
title="그림 5"
caption="주소 확인: 'Use Original' 선택" >}}

<그림 6>과 같이 신용카드 정보를 입력합니다. Visa와 Master 카드를 등록할 수 있습니다.

{{< img src="https://taewanmerepo.github.io/2018/04/oracloudtrial/img140.jpg"
title="그림 6"
caption="신용카드 정보 입력" >}}

신용 카드가 정상적으로 등록되면, <그림 7>과 같이 라이선스 및 사용권 동의를 하고 "Complete" 버튼을 클릭하면 Trial 등록 절차는 완료됩니다.

{{< img src="https://taewanmerepo.github.io/2018/04/oracloudtrial/img150.jpg"
title="그림 7"
caption="'라이선스 및 사용권 동의'와 등록 완료" >}}

등록이 완료되면 <그림 8>과 같이 등록 완료 페이지로 이동합니다.

{{< img src="https://taewanmerepo.github.io/2018/04/oracloudtrial/img160.jpg"
title="그림 8"
caption="오라클 클라우드 트라이얼 등록 완료 페이지" >}}

트라이얼 등록이 완료되면, 사용자 정보 등록과정에서 입력한 Email로 다음 <그림 9>와 같은 메일이 발송됩니다.

{{< img src="https://taewanmerepo.github.io/2018/04/oracloudtrial/img170.jpg"
title="그림 9"
caption="오라클 클라우드 트라이얼 신청 접수 완료 메일" >}}

### 오라클 클라우드 트라이얼 최종 승인 메일

오라클 클라우드 트라이얼을 신청하면 15분 ~ 4시간 안에 오라클 클라우드 트라이얼 최종 승인 메일이 발송됩니다. 오라클 클라우드 트라이얼 등록 완료 메일<그림 10>에는 다음과 같은 정보를 포함합니다.

- username
- 임시 패스워드
- Cloud Account
- Credit

<그림 10>에서 사용 가능 크래딧으로 400SGD로 표시된 것을 확인할 수 있습니다. SGD는 싱가폴달러를 의미합니다. 400SGD는 미화로 300불입니다.

{{< img src="https://taewanmerepo.github.io/2018/04/oracloudtrial/img200.jpg"
title="그림 10"
caption="오라클 클라우드 트라이얼 등록 완료 메일" >}}

## 오라클 클라우드 최초 접속

이제 오라클 클라우드에 로그인 준비가 모두 끝났습니다. <그림 11>과 같이 https://cloud.oracle.com 에서 오라클 클라우드에 로그인할 수 있습니다. 오라클 클라우드 홈페이지에서 "Sign In" 메뉴를 클릭하여 로그인 페이지로 이동합니다.

{{< img src="https://taewanmerepo.github.io/2018/04/oracloudtrial/img210.jpg"
title="그림 11"
caption="오라클 클라우드 홈페이지" >}}

<그림 12>와 같이 최종 승인 메일의 __Cloud Account__ 명을 입력하고 "My Service" 버튼을 클릭하면 최종 로그인 페이지로 이동합니다.

{{< img src="https://taewanmerepo.github.io/2018/04/oracloudtrial/img220.jpg"
title="그림 12"
caption="Cloud Account 지정 페이지" >}}

<그림 13>과 같이 최종 승인 메일의 Username과 Temporary Password를 입력하고 "Sing In(사인인)" 버튼을 클릭하면 오라클 클라우드에 로그인이 완료됩니다.  

{{< img src="https://taewanmerepo.github.io/2018/04/oracloudtrial/img230.jpg"
title="그림 13"
caption="오라클 클라우드 로그인" >}}

임시 패스워드로 최초 로그인 시 신규 패스워드를 등록하는 패이지가 출력합니다. 신규 페이스워가 등록되면 오라클 클라우드 대쉬보드(<그림 14> 참조)로 이동하게 됩니다.

{{< img src="https://taewanmerepo.github.io/2018/04/oracloudtrial/img240.jpg"
title="그림 14"
caption="오라클 클라우드 대쉬보드" >}}

## 오라클 클라우드 트라이얼 등록 오류 처리

일반적으로 <그림 9>의 "오라클 클라우드 트라이얼 신청 접수 완료 메일"을 수신한 후 15분 ~ 4시간 안에 <그림 10>과 같은 "오라클 클라우드 트라이얼 등록 완료 메일"을 수신하게 됩니다. 간혹 최종 등록 완료 메일을 수신하지 못하는 경우가 있습니다. 등록 완료 메일을 4시간 이상 수신하지 못할 경우, "오라클 클라우드 트라이얼 신청 접수 완료 메일"의 "__Chat__" 링크를 클릭하여 오라클 서포트에 접속한 후, 이슈를 문의하거나 등록할 수 있습니다.

{{< img src="https://taewanmerepo.github.io/2018/04/oracloudtrial/img250.jpg"
title="그림 15"
caption="오라클 클라우드 트라이얼 등록 완료 페이지" >}}

오라클 서포트는 클라우드 관련 이슈를 채팅과 메일로 일차 지원합니다. 오라클 트라이얼 계정 활성화와 관련된 지연 현상을 챗팅 혹은 메일로 남기면 다음과 같은 메일을 받게 됩니다.

{{< img src="https://taewanmerepo.github.io/2018/04/oracloudtrial/img260.jpg"
title="그림 16"
caption="오라클 클라우드 서포트 접속" >}}

<그림 17> 메일에는 신용카드 처리 내역을 답장으로 보낼 것을 요청하는 문구가 포함됩니다.

{{< img src="https://taewanmerepo.github.io/2018/04/oracloudtrial/img270.jpg"
title="그림 17"
caption="오라클 클라우드 서포트 접속" >}}

이러한 메일을 수신한 경우 <그림 18>과 같이 카드 결제 및 취소 SMS 메세지의 이미지 혹은 카드 결제 내역의 카드사 혹은 은행의 확인 결과를 오라클 서포트에 반송 메일로 전달하면 바로 처리됩니다.  

{{< img src="https://taewanmerepo.github.io/2018/04/oracloudtrial/img280.jpg"
title="그림 17"
caption="오라클 클라우드 서포트 접속" >}}

## 요약

오라클 클라우드 트라이얼은 오라클 클라우드를 전체적으로 테스트 및 검증에 적합한 무료 프로그램입니다. 오라클 클라우드 트라이얼은 사용자 별로 1회 지원을 원칙으로 하기 때문에 트라이얼 신청 시 EMail과 핸드폰 번호를 재사용할 수 없습니다.

오라클 클라우드 트라이얼 신청 시 주의해야 할 사항이 있습니다. 첫 번째는 Email과 핸드폰 번호를 재사용할 수 없다는 것이고 두 번째는 간혹 SMS 인증 문자를 받지 못했다는 사례가 있습니다. 이 사례의 대부분은 핸드폰 번호 입력 시, 첫 번째 "0"를 빼지 않고 모두 입력한 경우가 대부분입니다. SMS 인증용 핸드폰 번호 입력 시, 반드시 첫 번째 "0"을 빼고 입력해야 합니다. 마지막으로 주소로 입력한 국가와 카드 발행 국가가 일치해야 합니다.

오라클 클라우드 트라이얼을 신청하면 15분 ~ 4시간 안에 오라클 클라우드 트라이얼 최종 승인 메일이 발송됩니다. 이 메일에 등록된 정보로 오라클 클라우드에 접속하고 사용할 수 있습니다.
오라클 클라우드 트라이얼 활성화 지연이 발생할 경우, 오라클 서포트에 채팅 및 메일로 이슈를 전달하면 신속하게 처리됩니다.  

> Enjoy and Dive into Oracle Cloud

## Oracle Cloud 관련 문서

> - Oracle Cloud 공통
>   - [Oracle Cloud IaaS: OCI vs OCI Classic](/post/oci_and_oci_classic/)
>   - [Oracle Cloud 트라이얼 신청 절차 (2018.04)](/post/oracle_cloud_trial/)
>   - [윈도우, 리눅스, 맥에서 ssh 보안키 생성](/post/oci_classic_computing/)
> - Oracle Cloud Infrastructure Classic
>   - [OCI Classic: Compute 서비스, VM 생성](/post/oci_classic_computing/)
>   - [OCI Classic: Compute 서비스, VM의 고정 Public IP 지정](/post/oci_classic_computing_with_permanent_public_ip/)
