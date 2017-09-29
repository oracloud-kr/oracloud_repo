+++
author = "sumi.ryu"
categories = ["database cloud","mysql"]
date = "2017-09-20T17:17:35+09:00"
description = "MySQL Cloud Service에서의 백업과 복구하는 방법에 대해서 알아보도록 하겠습니다."
language = "bsh"
tags = ["database", "data management", "MySQL"]
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2017/05/mysqlcs/mysqlcs.jpg"
thumbnailInPost = ""
title = "MySQL 인스턴스에 대한 백업과 복구"
+++

본 문서는 Oracle MySQL APAC 세일즈 컨설팅팀에서 고객 전달용으로 작성한 블로그입니다. 해당 문서는 앞서 포스팅된 ["MySQL Cloud를 시작하고 샘플 스키마 생성하기"](post/mysqlcs/)라는 블로그에 이어서 MySQL Cloud Service에서 생성된 인스턴스에 대하여 어떻게 백업을 하고 복구를 하는지에 대하여 보여주고 있습니다.

## 선행 작업

본 문서는 다음과 같은 절차가 완료되었다는 것을 전제로 합니다.

- 오라클 클라우드 계정을 신청
- MySQL Cloud Service 서브스크립션 구매
- Oracle Cloud Service 인스턴스에 등록할 보안키를 생성
- Oracle Cloud Service 인스턴스 생성 완료

선행 작업이 완료되지 못한 상태라면 다음 문서를 참조하여 먼저 준비하시고 다음으로 넘어가시기 바랍니다.  

- [오라클 클라우드 계정](/post/accont/)
- [윈도우, 리눅스, 맥에서 SSH 보안키 생성](/post/ssh_key/)
- [MySQL Cloud를 시작하고 샘플 스키마 생성하기](/post/mysqlcs/)

## 백업과 복구 설정

MySQL Cloud Service는 __MySQL Enterprise Backup__ 을 이용하여 백업과 복구 작업을 수행합니다. MySQL Cloud Service에서 제공하는 백업기능은 MySQL의 __data__ 디렉토리의 전체 파일을 백업하게 되는데 이 백업 기능을 제공하기 위해서 MySQL Cloud Service는 MySQL 데이터베이스 생성단계에서 __MySQL Enterprise Backup__ 툴을 설치하기 때문에 백업과 복구기능은 인스턴스를 생성하는 단계에서 활용여부를 결정해야 합니다.

백업을 사용할 경우에는 백업설정의 __Backup Destination__ 에 “None”이 외의 값 즉 “__Both Cloud and Disk Storage__”거나 “__Cloud Storage Only__”를 선택해야 합니다.

![](https://oracloud-kr-teamrepo.github.io/2017/09/mysqlcs_backup/config.png)

__Both Cloud Storage and Disk Storage__ : 백업은 자동적으로 Oracle MySQL Cloud Service 인스턴스의 가상머신의 로컬 컴퓨터 스토리지와 Oracle Storage Cloud Service 컨테이너에 저장됩니다. 이 컨테이너는 기존것을 사용할 수 있고 새로운 Oracle Storage 컨테이너를 생성할 수 있습니다.

__Cloud Storage Only__ : 백업은 자동적으로 Oracle Storage Cloud Service 컨테이너에만 저장됩니다. 이 컨테이너는 기존것을 사용하거나 새로운 Oracle Storage 컨테이너를 생성할 수 있습니다.

## 디폴트 백업설정

자동백업은 주단위 전체 백업, 일단위 증분 백업을 실행하게 됩니다.
백업에 대한 보유기간은 MySQL Cloud Service 가상머신에 저장되는 백업 데이터는 최대 7일간 유지되고 클라우드 스토리지에서는 30일간 유지됩니다.

백업설정 화면에서 디폴트 백업설정을 변경할 수 있습니다.

![](https://oracloud-kr-teamrepo.github.io/2017/09/mysqlcs_backup/change1.png)
![](https://oracloud-kr-teamrepo.github.io/2017/09/mysqlcs_backup/change2.png)

백업설정 화면에서 주 1회씩 실행이 되는 전체 백업 시간과 하루에 1회씩 실행이 되는 증분 백업 시간 그리고 클라우드 스토리지상의 백업 데이터의 유지기간을 변경할 수 있습니다.

## 백업의 로컬 스토리지

MySQL 인스턴스의 로컬 스토리지를 확인할려면 Oracle Compute Cloud Service에서 해당 인스턴스의 “__보기__” 버튼을 클릭하여 스토리지 볼륨상세를 확인할 수 있습니다.

![](https://oracloud-kr-teamrepo.github.io/2017/09/mysqlcs_backup/storage1.png)
![](https://oracloud-kr-teamrepo.github.io/2017/09/mysqlcs_backup/storage2.png)

백업용으로 사용된 로컬 스토리지 용량은 데이터베이스 용량의 2배입니다.예를 들면 데이터베이스 스토리지 용량을 25GB로 설정했을 경우 백업용 스토리지 용량은 50GB로 설정됩니다.

![](https://oracloud-kr-teamrepo.github.io/2017/09/mysqlcs_backup/storage3.png)

## On-Demand 백업

On-Demand 백업은 사용자가 원할때 백업을 수행할 수 있는 기능으로 백업 페이지에서 “__Backup Now__”를 클릭하면 됩니다.


![](https://oracloud-kr-teamrepo.github.io/2017/09/mysqlcs_backup/ondemand1.png)
![](https://oracloud-kr-teamrepo.github.io/2017/09/mysqlcs_backup/ondemand2.png)

백업이 완료되면 백업 이력에 해당 기록이 추가되고 우측 메뉴를 통해 이 백업본에 대해서 __삭제__ 및 __복구__ 를 실행할 수 있습니다.

![](https://oracloud-kr-teamrepo.github.io/2017/09/mysqlcs_backup/ondemand3.png)

## 백업으로 부터 복구

위 화면에서 백업이력의 우측 메뉴버튼을 클릭한후  “__Restore__”를 클릭한 다음 확인하면 해당 백업파일로 복구작업을 실행하게 됩니다.

![](https://oracloud-kr-teamrepo.github.io/2017/09/mysqlcs_backup/restore1.png)
![](https://oracloud-kr-teamrepo.github.io/2017/09/mysqlcs_backup/restore2.png)

복구 작업은 다음 스탭을 거치게 됩니다.

- 데이터베이스 Shutdown
- 복구 준비 작업
- 복구 실행
- 복구 실행후 데이터베이스 서버 재시작

복구작업이 완성되면 복구 이력에서 복구 상태를 확인할 수 있습니다.

![](https://oracloud-kr-teamrepo.github.io/2017/09/mysqlcs_backup/restore3.png)

## Point in Time 복구

MySQL Cloud Service는 __binary log__ 를 이용한 Point in Time 복구를 지원합니다.
Point in Time복구는 내부적으로 다음과 같은 스텝을 거치게 됩니다.

-	데이터베이스 shuts down
-	복구 작업 준비
-	MEB에 의한 데이터의 복구
-	mysqlbinlog에 의한 바이너리 로그의 복구
-	데이터베이스 재시작


Point In Time 복구방법은 다음과 같습니다.
MySQL Cloud Service의 백업화면에서 메뉴를 클릭하여 “__Restore Service__”를 선택합니다.

![](https://oracloud-kr-teamrepo.github.io/2017/09/mysqlcs_backup/restore4.png)

복구 서비스의 입력창에서 복구시점과 복구유형을 선택합니다.
![](https://oracloud-kr-teamrepo.github.io/2017/09/mysqlcs_backup/restore5.png)

![](https://oracloud-kr-teamrepo.github.io/2017/09/mysqlcs_backup/restore6.png)

복구 작업이 완료되면 복구 이력에서 복구작업의 상태를 확인할 수 있습니다.
![](https://oracloud-kr-teamrepo.github.io/2017/09/mysqlcs_backup/restore7.png)

Point in Time복구를 실행할려면 복구할 시점이 기존에 실행했던 __전체백업과 증분백업 시점의 사이__ 에 있거나 또는 __두 증분백업사이의 시점__ 이여야 가능합니다.
![](https://oracloud-kr-teamrepo.github.io/2017/09/mysqlcs_backup/PIT.png)

그 이외의 경우는 mysqlbinlog를 이용하여 해결할 수 있습니다.
구체적인 방법에 대해서는 메뉴얼을 참조하시기 바랍니다.

https://dev.mysql.com/doc/mysql-enterprise-backup/4.0/en/advanced.point.html

## 백업 디렉토리

MySQL Cloud Service인스턴스에서 백업 툴이 위치한 디렉토리 : __/u01/bin/meb/bin__

MySQL Cloud Service인스턴스의 __로컬 스토리지__ 에서의 스케줄 백업 저장 위치 :

/u01/backup/domain name/Instance name/onDemandFull/*

/u01/backup/domain name/Instance name/scheduledFull/*

/u01/backup/domain name/Instance name/scheduledIncremental/*

![](https://oracloud-kr-teamrepo.github.io/2017/09/mysqlcs_backup/backupdir.png)

Cloud Storage 에 저장된 백업파일에 대한 정보는 __Oracle Storage Cloud Service__ 의 "__Containers__"화면에서 찾아 볼 수 있고 특정 백업파일에 대한 __다운로드__, __삭제__ 및 __복제__ 기능을 활용할 수 있습니다.

![](https://oracloud-kr-teamrepo.github.io/2017/09/mysqlcs_backup/storage_cloud1.png)

![](https://oracloud-kr-teamrepo.github.io/2017/09/mysqlcs_backup/storage_cloud2.png)

## 수동 백업 방법

__Full 백업__ 을 하는 경우를 예를 들어 설명 드리자면 다음과 같습니다.

[oracle@master-mysql-1 backup]$ /u01/bin/meb/bin/mysqlbackup --user=root -p --backup-dir=/u01/backup/ManualBackup backup-and-apply-log

![](https://oracloud-kr-teamrepo.github.io/2017/09/mysqlcs_backup/manual1.png)

수동 백업이 완료되면 지정된 백업 디렉토리가 생성이 되고 해당위치에서 백업된 파일들을 찾아 볼수 있습니다.

![](https://oracloud-kr-teamrepo.github.io/2017/09/mysqlcs_backup/manual2.png)

__mysqlbackup__ 의 사용방법에 대해서는 아래 메뉴얼을 참조하십시오.

https://dev.mysql.com/doc/mysql-enterprise-backup/4.0/en/meb-command-reference.html

## 마치며

여기까지 MySQL Cloud Service에 대한 백업과 복구하는 방법에 대해서 살펴보았습니다. 다음 문서에서는 MySQL Cloud Service 환경에서 인스턴스 사이의 복제(Replication)기능을 구성하는 방법에 대해서 설명해 드리겠습니다.
