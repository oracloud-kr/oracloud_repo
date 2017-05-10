+++
author = "jisun.kang"
categories = ["database cloud"]
date = "2017-05-04T17:17:35+09:00"
description = "MySQL을 Oracle Cloud에서 사용하는 방법에 대해서 알아보도록 하겠습니다."
language = "bsh"
tags = ["database", "data management", "MySQL"]
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2017/05/mysqlcs/mysqlcs.jpg"
thumbnailInPost = ""
title = "MySQL Cloud를 시작하고 샘플 스키마 생성하기"
+++

본 문서는 Oracle SCC Center에서 고객 전달용으로 작성한 워드 문서를 블로그 포맷으로 변경하여 작성한 문서입니다. 해당 문서에는 MySQL을 Oracle Cloud에서 어떻게 시작하고 사용할 수 있는지를 보여주는 문서로 인스턴스를 생성하고 생성된 MySQL 인스턴스에 Sample 스키마를 업로드하는 과정까지 보여주고 있습니다. 

## 선행 작업

본 문서는 다음과 같은 절차가 완료되었다는 것을 전제로 합니다.

- 오라클 클라우드 계정 생성 완료
- 가상머신 생성에 필요한 인증서를 생성
- 오라클 클라우드 웹 사이트에 로그인이 완료

오라클 클라우드 계정을 확보하지 못했거나, 가상머신에 등록 할 보안 키를 만들지 못한 상태라면,
다음 문서를 참조하여 먼저 준비하시고 다음으로 넘어가시기 바랍니다.  

- [오라클 클라우드 계정](/post/accont/)
- [윈도우, 리눅스, 맥에서 SSH 보안키 생성](/post/ssh_key/)

## MySQL 클라우드 서비스 인스턴스 생성

- MySQL 서비스 콘솔로 이동하기 위하여 대시보드 상의 __MySQL Cloud Service__ 오른쪽에 있는 리스트 아이콘을 클릭하고, __서비스 콘솔 열기__ 메뉴를 선택합니다. MySQL이 대시보드에서 보이지 않는 경우 __Customize Dashboard__를 클릭하여 MySQL Cloud Service에 대해서 __표시__를 클릭하시면 됩니다. 

![](https://oracloud-kr-teamrepo.github.io/2017/05/mysqlcs/dashboard1.png)

__Customize Dashboard__ 를 클릭했을때의 화면 중 일부입니다.
![](https://oracloud-kr-teamrepo.github.io/2017/05/mysqlcs/CustomizeDashboard.png)

-   __Service__ 링크를 클릭하고, 

![](https://oracloud-kr-teamrepo.github.io/2017/05/mysqlcs/creation1.png)

새로운 데이터베이스 인스턴스 생성을 위해 __인스턴스 생성__ 버튼을 클릭합니다.

![](https://oracloud-kr-teamrepo.github.io/2017/05/mysqlcs/creation2.png)

- __서비스 이름__ 을 입력하고 다음 버튼을 클릭하면 서비스 세부 정보를 입력할 수 있습니다.

![](https://oracloud-kr-teamrepo.github.io/2017/05/mysqlcs/creation3.png)

- __SSH 공용 키__ 부분에 위에서 생성한 보안키를 복사하고, 데이타베이스의 용량, root 암호 및 데이타베이스 스키마 이름과 서버 문자 집합을 입력합니다. __백업 및 복구 구성__ 의 경우 클라우드 영역에 대해서는 아래와 같은 정보를 입력합니다. 

| 항목 | 입력값 |
| ------ | ------ |
| 클라우드 저장 영역 컨테이너 | Storage-"Identity Domain"/"컨테이너 이름" |
| 클라우드 저장 영역 사용자 이름	| Cloud 로그인 계정 |
| 클라우드 저장 영역 비밀번호 | Cloud 로그인계정 패스워드 |
| 클라우드 저장 영역 컨테이너 생성 | 백업시 새로운 컨테이너 사용을 원하는 경우 선택 |

![](https://oracloud-kr-teamrepo.github.io/2017/05/mysqlcs/creation4.PNG)

- 구성 내역을 확인하시고 __생성__ 버튼을 클릭합니다.

![](https://oracloud-kr-teamrepo.github.io/2017/05/mysqlcs/creation5.png)

- 인스턴스가 생성된 후 콘솔에서 확인가능합니다. (생성시 소요 시간은 일반적으로 약 15분 정도입니다.

![](https://oracloud-kr-teamrepo.github.io/2017/05/mysqlcs/creation6.png)


## MySQL 클라우드 서비스에 대한 원격접속 허용

- 생성된 인스턴스를 선택하고 들어간 후 상단 리스트 아이콘을 클릭하고 __Access Rules__를 선택합니다.

![](https://oracloud-kr-teamrepo.github.io/2017/05/mysqlcs/access1.png) 

- __ora_p2admin_mysql__ 항목 오른쪽의 리스트 아이콘을 클릭하고, __Enable__을 선택합니다.

![](https://oracloud-kr-teamrepo.github.io/2017/05/mysqlcs/access2.png) 

- 3306포트가 정상적으로 외부에서 접근될 수 있도록 Open되었음을 확인합니다.

![](https://oracloud-kr-teamrepo.github.io/2017/05/mysqlcs/access3.png) 

## PetStore 스키마를 MySQL 인스턴스에 추가

- 다운로드 받은 Petstore 스키마 생성파일을 MySQL Cloud server로 upload합니다.

<pre class="prettyprint">
[oracle@localhost ~]$ unzip mybatis-jpetstore-6.0.1-bundle.zip
[oracle@localhost ~]$ cd mybatis-jpetstore-6.0.1
[oracle@localhost ~]$ unzip mybatis-jpetstore-6.0.1-sources.zip
[oracle@localhost ~]$ cd src/main/resources/database
[oracle@localhost ~]$ scp jpetstore-hsqldb-schema.sql opc@129.144.150.22:/home/opc
[oracle@localhost ~]$ scp jpetstore-hsqldb-dataload.sql opc@129.144.150.22:/home/opc
</pre>

- SSH 명령어를 통해 MySQL Cloud server에 접속하여, 스크립트의 오너쉽 권한을 변경한 후, sudo 명령어를 사용하여 oracle 계정으로 로그인 합니다. 

<pre class="prettyprint">
[oracle@ localhost ~]$ ssh opc@129.144.150.22
[opc@jcs4mysql-mysql-1 ~]$ sudo chown oracle:oracle *.sql 
[opc@jcs4mysql-mysql-1 ~]$ sudo cp –Rp *.sql ~oracle
[opc@jcs4mysql-mysql-1 ~]$ sudo su – oracle
</pre>

- 다음과 같이 mysql 명령어를 실행하여 petstore 스키마를 생성합니다. 암호 부분에는 MySQL Cloud service 생성시 사용한 암호를 입력하시면 됩니다. 

<pre class="prettyprint">
[oracle@mysql1-mysql-1 ~]$ mysql -u root -p petstore < jpetstore-hsqldb-schema.sql
Enter password: <>
</pre>

















