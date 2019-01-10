+++
date = "2017-08-23T21:00:00+09:00"
description = "Oracle Public Cloud에서는 Oracle API Catalog Cloud Service를 제공합니다. Oracle Public Cloud에서 제공하는 Public API를 검색하고, 테스트할 수 있는 기능을 제공하는 서비스입니다. Swagger 2.0으로 알려져 있는 OpenAPI 2.0 기반의 API 문서 정의를 지원하고 있습니다."
title = "Oracle API Catalog Cloud Service 소개"
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2017/08/api_catalog/apicatalog_logoinlist.png"
tags = ["API", "API Catalog", "integration"]
categories = ["API Catalog"]
language = ""
author = "donghee.lee"
+++

Oracle Public Cloud상에서 제공하는 클라우드 서비스들은 각 서비스에 대한 REST API를 제공하며, 기본적으로 각 클라우드 서비스의 문서에 하나의 항목으로 제공하고 있습니다. Oracle API Catalog Cloud Service는 Oracle Public Cloud에서 제공하는 Public API를 한 곳에서 통합하여 검색하고, 테스트할 수 있는 기능을 제공하는 서비스입니다. 또한 REST API에서 가장 많이 사용되는 Swagger 2.0으로 알려져 있는 OpenAPI 2.0 기반의 API 문서 형식을 지원하고 있습니다. 그리고 **무료**로 사용할 수 있습니다. 본 문서는 다음과 같은 내용으로 구성됩니다.


  - Oracle API Catalog Cloud Service 소개
  - API 테스트
  - 내 API를 API Catalog에 등록하기


## Oracle API Catalog Cloud Service 소개

API Catalog 클라우드 서비스(https://apicatalog.oraclecloud.com/ui/)를 접속해 보면 다음 그림 처럼 Infrastructure(IaaS), Platform(PaaS), Application(SaaS), Industry Solutions 등의 REST API가 카테고리 별로 정리된 카탈로그를 제공합니다. 각각의 서비스의 문서 페이지에도 각각 제공하는 API에 대한 정보를 제공하고 있습니다. 그 REST API에 대한 문서화 방식은 서비스에 따라 조금씩 차이가 있습니다. API Catalog에서는 각 REST API를 한 곳에 통합하여 제공하는 것과 동시에 문서화를 최근에 가장 많이 사용되고 있는 OpenAPI 2.0 방식으로 단일화해서 제공하고 있는 것이 특징입니다.

![](https://oracloud-kr-teamrepo.github.io/2017/08/api_catalog/apicatalog01.png)


## API 테스트
API 모음을 선택하면, API 모음에서 제공하는 API 목록 정보를 확인할 수 있습니다. 사용중인 Oracle Cloud 서비스가 있는 경우에는 로그인하여 해당 API를 직접 테스트 해 볼 수도 있습니다. "Try-It Endpoint"에서 서비스의 주소를 입력하고, 테스트할 API의 'Try it out' 버튼을 클릭하여 테스트합니다.

그림은 Integration Cloud Service의 현재 상태를 조회해 보는 API를 테스트 해 본 예제입니다.

![](https://oracloud-kr-teamrepo.github.io/2017/08/api_catalog/apicatalog02.png)

또한 API 우측 상단에 있는 **'OpenAPI Definition'**을 클릭하면 OpenAPI 2.0(구 Swagger 2.0) 정의 파일을 다운 받을 수 있습니다.

![](https://oracloud-kr-teamrepo.github.io/2017/08/api_catalog/apicatalog03.png)


## 내 API를 API Catalog에 등록하기
2017년 5월 버전부터 사용자의 API를 등록할 수 있는 기능을 제공합니다. Oracle Cloud 계정이 있는 경우, Oracle이 제공하는 Public API외에 자신의 API를 API Catalog에 등록할 수 있습니다. 등록하려는 API 정의는 OpenAPI 2.0(구 Swagger) 형식인 경우에만 해당됩니다. API Catalog 우측에 있는 My APIs를 클릭하여 절차에 대한 설명이 있으며, 아래 순서대로 하시면 됩니다.

### Step #1. 관련 역할(Role) 추가하기
API Catalog에 내 API를 등록하기 위해서는 유저에게 APICATALOG_READWRITE_ROLE 역할을 부여해야 합니다.
최초  APICATALOG_READWRITE_ROLE 역할이 없기 때문에 Custom Role을 생성합니다. Custom Role 생성은 도메인 관리자(Identity Domain Administrator) 권한인 있는 사용자가 추가 할 수 있습니다.

  1. My Services 로그인
  2. Users 탭 클릭
  3. Custom Roles 클릭
  4. 다음 정보로 Custome Rule을 추가

    |항목|값|
    |---|---|
    |Role Name|APICATALOG_READWRITE_ROLE|
    |Display Name|API Catalog Read/Write Role|
    |Description|Read/ Write access for the API Catalog Cloud Service|

![](https://oracloud-kr-teamrepo.github.io/2017/08/api_catalog/apicatalog04.png)


### Step #2. 사용자에 관련 역할 할당하기
내 API를 등록해서 사용할 사용자에서 앞서 만든 APICATALOG_READWRITE_ROLE 역할을 부여합니다.

  1. My Services 로그인
  2. Users 탭 클릭
  3. 부여할 사용자를 찾아 우측 햄버거 버튼 클릭
  4. Manage Roles 클릭
    ![](https://oracloud-kr-teamrepo.github.io/2017/08/api_catalog/apicatalog05.png)
  5. APICATALOG_READWRITE_ROLE 역할 부여 - 여기서는 Display Name으로 보입니다. API Catalog Read/Write Role을 추가합니다.
    ![](https://oracloud-kr-teamrepo.github.io/2017/08/api_catalog/apicatalog06.png)


### Step #3. Organization 등록하기
앞서 역할을 부여한 사용자로 다시 API Catalog에 로그인합니다. APIs를 등록하기 앞서 우선 Organization을 등록합니다. 한번만 등록하면 됩니다. 로그인 후 우측 Publish > Organization을 클릭합니다. + 버튼을 클릭하여 Organization을 추가합니다. Organization은 기본값인 domain id를 이름으로 그대로 사용하거나, 또는 {domain id}- 으로 시작하는 이름으로 지정하면 됩니다.

![](https://oracloud-kr-teamrepo.github.io/2017/08/api_catalog/apicatalog07.png)


### Step #4. Category 추가하기
카테고리는 API를 관리하기 위한 논리적 그룹입니다. 원하는 분류 그룹으로 추가합니다. 로그인 후 우측 Publish > Category을 클릭합니다. New Category 버튼을 클릭하여 추가합니다. 그림과 같이 원하는 카테고리를 추가하면됩니다.

현재 버전기준으로 웹 UI 에서 카테고리를 추가할 때 상위 카테고리를 지정이 안됩니다. 필요한 경우 [API Catalog API](https://apicatalog.oraclecloud.com/ui/views/apicollection/oracle-public/apicatalog/1.0/Categories)를 통해 추가하시면 됩니다.

![](https://oracloud-kr-teamrepo.github.io/2017/08/api_catalog/apicatalog08.png)


### Step #5. 내 API 추가하기
로그인 후 우측 Publish > APIs를 클릭합니다. **Choose File**을 클릭하여 API 정의 파일을 업로드 한 후, 카테고리를 지정하고, 이름 및 제목을 수정하시면 됩니다.

아래 정보는 Swagger 샘플인 PetStore API를 다운받아 등록한 예제입니다. 제목, 버전, 태그 등의 정보는 Swagger.json 파일에서 읽어오게 됩니다.

  - http://petstore.swagger.io/
  - http://petstore.swagger.io/v2/swagger.json

![](https://oracloud-kr-teamrepo.github.io/2017/08/api_catalog/apicatalog09.png)


### Step #6. 확인
My APIs 메뉴에서 등록한 내 API를 확인할 수 있습니다. 현재는 등록한 카테고리 중에 실제 API가 등록된 것만 보입니다.

![](https://oracloud-kr-teamrepo.github.io/2017/08/api_catalog/apicatalog10.png)

등록한 API인 Petstore를 클릭하면 Public APIs와 동일한 UI 형태로 제공하며, 동일하게 Endpoint를 등록하여 API를 테스트 할 수 있습니다.

![](https://oracloud-kr-teamrepo.github.io/2017/08/api_catalog/apicatalog11.png)


## 요약

Oracle API Catalog Service는 Oracle Public Cloud에서 제공하는 Public API를 한 곳에 모아 제공하는 서비스로, API 조회, 테스트가 가능하고 모든 API은 OpenAPI 2.0(구 Swagger 2.0) 형식으로 제공합니다. 더불어 Oracle Cloud 계정이 있는 경우, 사용자의 API도 등록할 수 있습니다.


## 참고자료

- [Using Oracle API Catalog Cloud Service](http://docs.oracle.com/en/cloud/paas/apicatalog-cloud/apiug/index.html)
