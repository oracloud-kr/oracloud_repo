+++
language = ""
description = "오라클 클라우드 계정을 생성한 후 Oracle Storage Cloud Service의 기본 복제 정책을 설정해야 합닏. "
date = "2017-03-06T08:54:32+09:00"
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2017/04/object_storage_init/list.jpg"
title = "Oracle Storage Cloud Serivce 활성화"
author = "taewan.kim"
categories = ["cloud"]
tags = ["Object Storage", "Storage"]
thumbnailInPost = "https://oracloud-kr-teamrepo.github.io/2017/04/object_storage_init/post.png"

+++

Oracle Storage Cloud Serivce(이하 Storage Cloud)는 오라클 클라우드의 핵심 서비스입니다.
오라클 클라우드의 대부분의 서비스는 데이터 저장 목적으로 Storage Cloud를 사용합니다.
오라클 클라우드에서 Storage Cloud를 사용하는 서비스는 다음과 같습니다.

| 서비스 | 설명 |
| --- | --- |
| Java Cloud Service(이하 JCS)| JCS는 클러스터 백업 및 메타 정보 저장을 위해서 Storage Cloud를 사용합니다. JCS 설정시 Storage Cloud 정보 입력이 필 수 사항입니다.|
| Database CS (이하 DBCS) | DBCS는 데이터베이스 백업에 Storage Cloud를 사용합니다. DBCS 인스턴스 생성 시 Storage Cloud 정보를 입력합니다. 필수 사항은 아니며 선택적 입력필드입니다.|
| Big Data - Compute Edition (이하 BDCE) | BDCE는 Spark에서 데이터를 읽어오는 스토리지로 Storage Cloud를 사용합니다. 인스턴스 생성시 Cloud Storage Container, 인증정보를 입력해야 합니다.|

오라클 클라우드의 많은 부분에서 Storage Cloud를 사용하기 때문에 오라클 클라우드 계정을 생성한 직후에
Storage Cloud를 활성화하는 것이 좋습니다. Storage Cloud 활성화를 위해서는 Storage Cloud 서비스 콘솔에서  
기본 정책(데이터 복제 정책)을 설정해야 합니다.
Storage Cloud 복제 정책을 설정한다는 것은 Storage Cloud 서비스를 활성화한다는 의미가 있습니다.

Oracle Cloud 계정이 없다면 다음 문서를 참조하여 계정을 생성하시기 바랍니다.

- [Oracle Cloud 계정 생성하기](http://www.oracloud.kr/post/accont/)

## Storage Cloud 서비스 콘솔

오라클 클라우드에 로그인하면 <그림 1>과 같이 오라클 클라우드 대시보드가 출력됩니다.

- 그림 1. 오라클 클라우드 대시보드
![](https://oracloud-kr-teamrepo.github.io/2017/04/object_storage_init/ocdashboard.jpg
)

Storage Cloud 서비스 콘솔에 접근하기 위하여 <그림 2>와 같이 Storage 상자의 오른쪽 위에 메뉴 아이콘을 클릭한 후, 출력되는 메뉴 목록에서 ```서비스 콘솔 열기```를 클릭하면 <그림 3>의 서비스 콘솔로 이동합니다.

- 그림 2. 오라클 클라우드 대시보드
![](https://oracloud-kr-teamrepo.github.io/2017/04/object_storage_init/ocdashboard2.jpg)

## Storage Cloud의 복제 정책

- 용어 정리
  - Primary Data Center(기본 데이터 센터, Primary DC): 데이터가 저장되는 데이터 센터
  - Georeplication Data Center(지역복제 데이터 센터, Georeplication DC): 데이터가 복제되는 원격지 데이터 센터

Storage Cloud의 복제 정책은 다음 2가지로 구분됩니다.

### 지역 간 데이터 복제 없음 (No Georeplication policy)

모든 읽기 쓰기 요청은 Primary DC에서 처리됩니다. Primary DC가 사용 불가능한 상태가 되면 모든 요청은 실패합니다.

### 지역 간 데이터 복제 허용 (Georeplication policy)

데이터는 Primary DC와 Georeplication DC에 모두 데이터가 저장됩니다. 모든 요청은 Global Namespace URL에 요청되고,
데이터 저장 요청은 Primary DC에 전달됩니다. 저장된 데이터는 비동기 적으로 Georeplication DC에 저장됩니다.
Primary DC와 Georeplication DC의 데이터 동기화는 Eventual Consistency[^1]입니다.

Primary DC에 장애가 발생하면 쓰기 요청에 대해서는 403 에러가 발생하지만, 읽기 요청은 Georeplication DC에서 처리되어 정상적으로 처리 됩니다. Primary DC가 복구되면 Global Namespace URL은 쓰기 요청을 다시 Primary DC에 전달하고 정상적으로 처리됩니다.

지역 간 데이터 복제 허용 옵션을 설정하면 Storage Cloud의 비용은 저장 공간의 두 배로 책정됩니다.

[^1]: Eventural Consistency는 일시적으로 일관성이 깨질수 있지만, 지정된 시간간격 이내에서 일관성을 확보한다는 개념입니다. 궁긍적 일관성, 지연 일관성 이라는 표현으로 번역될 수 있습니다.  

## Storage Cloud의 복제 정책 설정

Storage Cloud의 서비스 콘솔에 처음 접근할 경우에 데이터 복제 정책을 설정하는 팝업이 <그림 3>과 같이 출력됩니다.
Storage Cloud의 복제 정책은 두 가지로 구분됩니다.

- 그림 3. Storage Cloud 서비스 콘솔의 복제 정책 설정 팝업
![](https://oracloud-kr-teamrepo.github.io/2017/04/object_storage_init/policy01.jpg)

Storage Cloud는 <그림 4>와 같이 7개의 설정 옵션이 제공됩니다. 현재 데모는 트라이얼 계정을 사용하고 있고
한국에서 요청하는 트라이얼 계정은 US2 데이터센터에 위치합니다. <그림 4>의 옵션에서 데이터센터가 1개인 것은
복제 없이 Primary DC를 지정하는 옵션입니다. "-"로 구분된 2개의 데이터센테를 표현하는 옵션은 Primary DC(앞)와
Georeplication DC(뒤)를 지정하는 복제 옵션입니다.

현재 Trial 계정에서는 별도의 복제가 필요 없다면 ```Chicago(US2)```를 선택하는 것이 권장됩니다.
Trial 계정의 다른 서비스 인스턴스들이 모두 ```Chicago(US2)```에서 생성되기 때문에, Storage Cloud의 위치를
동일하게 맞추는 것이 성능 관점에서 유리합니다.

복제 설정을 할 것이라면 ```Chicago-Ashburn(US2-US6)```를 선택하는 것이 권장됩니다. Primary DC를 US2를 사용하는 복제 옵션이기 때문에
Storage Cloud의 성능과 안정성 모두를 만족하는 옵션입니다.

- 그림 4. Storage Cloud 서비스 복제 옵션 목록
![](https://oracloud-kr-teamrepo.github.io/2017/04/object_storage_init/policy02.jpg)

<그림 5>에서는 ```Chicago(US2)```를 선택하여 복제하지 않고 US2를 Primary DC로 선택하는 방식을 설정하고 있습니다.

- 그림 5. Storage Cloud 서비스의 복제 설정 완료
![](https://oracloud-kr-teamrepo.github.io/2017/04/object_storage_init/policy03.jpg)

<그림 5>에서 옵션을 선택한 후 ```set policy``` 버튼을 클릭하면 <그림 6>과 같은 서비스 콘솔의 화면이 출력됩니다.

- 그림 6. Storage Cloud의 대시보드
![](https://oracloud-kr-teamrepo.github.io/2017/04/object_storage_init/sc_dashboard.jpg)

## 요약

오라클 클라우드 계정을 생성한 후 Storage Cloud 활성화 절차에 대하여 살펴보았습니다.
Storage Cloud 복제 정책을 설정한다는 것은 Storage Cloud 서비스를 활성화한다는 의미입니다.

복제 설정 옵션은 Storage Cloud의 primary DC를 선택하고 복제 여부를 선택하는 방식으로 구성됩니다.
Primary DC는 계정이 사용하는 데이터 센터와 같은 DC를 선택하는 것이 효과적입니다.

복제 선택 시 Primary DC에 저장된 데이터는 복제 DC에 비동기적으로 복제됩니다.

복제하지 않은 정책을 사용할 경우에 Primary DC가 이용 불가능한 상태가 되면 모든 데이터 읽기/쓰기 요청은 실패합니다.
복제 정책을 사용할 경우 사용할 경우 Primary DC 장애 때에도 데이터 읽기 요청은 원격지 복제 데이터 센터를 이용하여 정상적으로 처리됩니다.
비용 관점에서 복제 정책을 사용할 경우 Storage Cloud에 요금이 부과되는 비용은 저장 용량의 2배입니다.
