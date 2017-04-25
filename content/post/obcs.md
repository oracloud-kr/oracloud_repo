+++
author = "moonsun.lee"
categories = []
date = "2017-03-31T17:51:51+09:00"
description = "Oracle Database Backup Cloud Service(이하 ODBCS)는 오라클 데이터베이스 백업을 위한 클라우드 스토리지 서비스(PaaS)입니다."
language = "bsh"
tags = ["oracle", "dbcs", "database backup", "cloud"]
thumbnailInList = "https://docs.oracle.com/cloud/latest/dbbackup_gs/dcommon/img/cloudgs_dbbackup.png"
thumbnailInPost = "https://i.ytimg.com/vi/SZQ3e6KrU8Y/maxresdefault.jpg"
title = "Oracle Database Backup Cloud Service(ODBCS) 소개"

+++

## Overview

전통적으로 데이터베이스의 오프사이트 백업은 테이프로 백업 후 리모트 위치로 복제하여 보관해왔습니다. 그러나 이와 같은 방법은 가격적으로나 운영적인 측면에서 많은 노력을 기울어야 합니다. ODBCS는 이와 같은 니즈에 대한 매우 훌륭한 선택적인 방안을 제공합니다.

클라우드 스토리지에 데이터베이스 백업본을 저장하는 ODBCS는 On premise의 데이터베이스나 Cloud 상에서 운영되는 데이터베이스 모두 백업 가능합니다. 기존 클라우드 환경에서 제공되던 스토리지 스냅샷 기반에 의한 데이터베이스 백업이 아닌 RMAN을 통한 논리적 백업을 지원하기 때문에 기존 On premise의 환경과 100% 동일한 백업환경을 제공합니다.

또한, 백업된 데이터를 인터넷을 통해서 언제나 접근할 수 있게 하며, 언제든 즉시 유연하게 사용 가능합니다. 복제된 데이터는 동일한 지리적 지역의 다수의 스토리지 노드에 복사되므로 하드웨어 장애나 데이터 손상에 대해서도 보호되며, 복제된 백업본을 통해 On premise에 복구를 할 수도, Oracle Public Cloud 상에 복구를 할 수도 있습니다.

![](https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/266ac4ff-b1dc-471a-a57a-b6502403f164/File/6e5875fc1ba4e2be45176f80a72b2ff5/6e5875fc1ba4e2be45176f80a72b2ff5.png)

ODBCS는 설치가 간단하고 사용하기 매우 쉽습니다. Oracle Technology Netwirk(OTN)을 통해서 Oracle Database Cloud Backup Module을 다운로드 받고 설치합니다. 백업 모듈을 설치한 후 몇 가지 RMAN 설정을 하면, 익숙한 RMAN명령어를 사용하여 Cloud 환경으로 백업할 준비가 됩니다. 클라우드 상에서 제공되는 온라인 대쉬보드를 통해서 백업을 위한 스토리지 사용량과 서비스 현황을 모니터링 할 수 있으며, 필요에 따라 쉽고 빠르게 용량을 추가할 수 있습니다. 아래 데모를 통해 자세히 알아 보도록 하겠습니다.

<iframe width="560" height="315" src="https://www.youtube.com/embed/gbQ0i_uRGak?ecver=1" frameborder="0" allowfullscreen></iframe>


## Demo
### Oracle Database Cloud Backup Module 설치하기
--------------------------------
오라클 데이터베이스 백업 서비스를 이용하려면, 먼저 Oracle Database Cloud Backup Module을 설치해야 합니다. 설치 전에 다음과 같은 사항들을 확인하시기 바랍니다.

| 분류 | 내용  |
| :------------ | :-----------: |
| Oracle Database| - Standard Edition, Enterprise Edition 모두 지원 <br />10g Release 2(10.2.0.5)와 그 이상 버전<br /> 11g Release 1(11.1.0.7), 11g Release 2(11.2.0.3 and 11.2.0.4)와 그 이상 버전|
| Operating System (64 bits) |Linux, Solaris x86-64, SPARC, Windows, AIX, HP-UX, zLinux|
|Java version|JDK 1.5 또는 그 이상 버전 (java –version 으로 현재 PC에 설치된 JDK versiob을 확인)|

이제 모듈을 다운로드 받고 설치합니다.


1. Oracle Technology Network(OTN) 사이트에서 Oracle Database Cloud Backup Module을 다운로드 합니다.<br />
http://www.oracle.com/technetwork/database/availability/oracle-cloud-backup-2162729.html

2. Installer(opc_installer.jar)파일을 포함한 ZIP file을 다운로드 합니다.

3. 다운로드 받은 ZIP file 압축을 풉니다.

4. Terminal Window를 열어 opc_installer.jar 파일로 backup module을 설치합니다.

5. 다운로드 받은 디렉토리로 이동하여 압축을 풀고 아래의 설치 명령어를 통하여 모듈을 설치합니다. 변수값에 오라클 퍼블릭 클라우드 계정정보를 입력하여 주면 됩니다.<br />
<pre class="prettyprint">
*java -jar opc_install.jar -serviceName <strong>myService</strong>
-identityDomain <strong>myDomain</strong> -opcId <strong>'myAccount@myCompany.com'</strong>
-opcPass <strong>'myPassword'</strong> -walletDir /</strong>walletDirectory
-libDir <strong>/libraryDirectory</strong>*
</pre>


![](https://oracloud-kr-teamrepo.github.io/2017/04/odbcs/db1.png)

### RMAN 설정하기
--------------------------------

Backup Module 설치가 완료되면 backup destination에 필요한 Recovery Manager(RMAN)을 설정합니다.

RMAN backup, restore, maintenance에 필요한 설정은 CONFIGURE 명령어를 사용합니다.

SBT PARMS 파라미터는 (ex. SBT_PARMS=(OPC_FILE=/u01…) 기존의 PFILE이나 SPFILE에서 설정할 수 있는 값이 아니라 backup 모듈 설치 도중 생성되는 OPC_PFILE에 저장되는 파라미터입니다. 또한 OPC_FILE은 $ORACLE_HOME/dbs 디렉토리에 저장되어 있습니다. 해당 파라미터의 경로를 모를 경우, find 명령어로 검색합니다. (ex. find / -name opcorcl.ora)

<pre class="prettyprint">
RMAN> CONFIGURE CHANNEL DEVICE TYPE 'SBT_TAPE' PARMS  'SBT_LIBRARY=/home/oracle/OPC/lib/ libopc.so, ENV=(OPC_PFILE=/u01/products/db/12.1/dbs/opcodbs.ora)';
</pre>

![](https://oracloud-kr-teamrepo.github.io/2017/04/odbcs/db2.png)

### Backup 시작하기
--------------------------------

Recovery Manager(RMAN) encrypted backup은 보안 상의 이유로 완벽하게 데이터를 암호화하고 전송하여 cloud상에 저장합니다. 아래의 3가지 mode 중 하나를 선택하여 암호화를 수행합니다.


1. Password encryption
2. Transparent Data Encryption (TDE)
3. Dual-mode encryption (combination of password and TDE)


이번 데모에서는 password encryption으로 암호화를 진행합니다. 백업을 수행하기 전, 아래의 명령어로 password encryption을 설정합니다.

<pre class="prettyprint">
RMAN> set encryption on identified by 'abc123' only;
</pre>

Encryption 설정 후 아래의 명령어로 백업을 진행합니다. 정상적으로 백업이 시작되면 data file 백업 후 Control file과 SPFILE은 가장 마지막 부분에서 백업됩니다.

<pre class="prettyprint">
RMAN> backup device type sbt as compressed backupset database plus archivelog format '%d_%U';
</pre>


![](https://oracloud-kr-teamrepo.github.io/2017/04/odbcs/db3.png)


![](https://oracloud-kr-teamrepo.github.io/2017/04/odbcs/db4.png)


![](https://oracloud-kr-teamrepo.github.io/2017/04/odbcs/db5.png)

### Backup 정보 확인하기
--------------------------------
아래의 명령어로 backup file을 확인할 수 있습니다.

<pre class="prettyprint">
RMAN> list backup;
</pre>



![](https://oracloud-kr-teamrepo.github.io/2017/04/odbcs/db6.png)

또한 백업된 시간을 자세히 알고 싶을 경우 아래의 OS 명령어를 입력하고 다시 RMAN에 접속하여 ‘list backup summary’ 명령어를 통해 확인합니다.

<pre class="prettyprint">
[oracle@localhost~] export NLS_DATE_FORMAT='yyyy-mm-dd hh24:mi:ss'
[oracle@localhost~] rman target /

     RMAN> list backup summary
     ...
</pre>

### 의도적인 오류 발생 및 ODBCS를 통한 데이터베이스 복구
--------------------------------

의도적으로 데이터베이스 파일 모두를 삭제하고 데이터베이스를 부팅해보도록 하겠습니다.<br />
데이터베이스에 저장된 모든 .dbf파일을 삭제합니다.
![](https://oracloud-kr-teamrepo.github.io/2017/04/odbcs/db7.png)

정상적으로 삭제가 되었다면, 당연히 아래와 같이 데이터베이스는 정상적으로 기동되지 않습니다.

![](https://oracloud-kr-teamrepo.github.io/2017/04/odbcs/db8.png)

ODBCS에 저장된 백업본을 통해서 복구하는 방법은 기존 On premise 방법과 동일합니다. 아래와 같이 RMAN에 접속해서, Restore 및 Recover 명령어를 통해 손쉽게 클라우드에 저장된 백업본을 통해 데이터베이스를 복구 할 수 있습니다.

![](https://oracloud-kr-teamrepo.github.io/2017/04/odbcs/db9.png)


![](https://oracloud-kr-teamrepo.github.io/2017/04/odbcs/db10.png)

![](https://oracloud-kr-teamrepo.github.io/2017/04/odbcs/db12-1.png)

정상적으로 복구가 완료되었다면 아래와 같이 데이터베이스 파일이 정상적으로 보이며, 데이터베이스가 재기동 됩니다.

![](https://oracloud-kr-teamrepo.github.io/2017/04/odbcs/db12.png)


## 마치며
Oracle Database Backup Cloud Service는 Oracle Database를 이용하는 사용자들에게 매우 환영받을 만한 솔루션임에 틀림없는 것 같습니다. 기존 Backup 방식인 RMAN을 사용자 Infra와 클라우드 Infra 사이의 이질감 없이 기존 기술, 기존 명령어를 그대로 이용한다는 점은 매우 흥미롭습니다. 따라서 offsite 백업을 고려하는 사용자 입장에서는 비용적인 측면이나, 운영비용에서 효과를 거둘 수 있을 것입니다.

위 데모에서도 살펴봤듯이 사용자는 별도의 추가 장비나 별도의 솔루션 없이 ODBCS를 통해 직접 온프레미즈 또는 클라우드상의 오라클 데이터베이스를 실시간으로 백업시킴으로써 중요한 데이터의 유실이나 다운타임을 효과적으로 예방할 수 있을 것입니다.


더 자세한 정보는 아래의 사이트를 확인바랍니다.


https://cloud.oracle.com/en_US/database-backup <br />
https://docs.oracle.com/cloud/latest/dbbackup_gs



![](https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/266ac4ff-b1dc-471a-a57a-b6502403f164/Image/b135ad842cbfbb99d7b29db15e9f0fdb/dbbackup.jpg)
