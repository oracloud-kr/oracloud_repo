+++
author = "sunguk.hong"
categories = []
date = "2017-05-15T15:00:47+09:00"
description = "쉽게 따라해보는 데이터 시각화 제2회 '데이터 연결'"
language = ""
tags = ["Data Visualization","Desktop", "Analytics"]
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2017/04/dvcs/intro.JPG"
thumbnailInPost = "https://oracloud-kr-teamrepo.github.io/2017/05/dvcs_002/DVCS002_main.JPG"
title = "쉽게 따라해보는 데이터 시각화 제2회 '데이터 연결'"

+++

데이터 시각화를 위해서 가장 먼저 해야 할 작업은 '분석에 사용할 데이터의 연결'입니다. 
 
ODV(Oracle Data Visualization)는 데이터 시각화를 위해서 다양한 File, RDB, Hadoop 등 데이터소스를 지원하고 있으며, 분석가능한 데이터소스를 계속적으로 추가하고 있습니다. 현재까지 지원되고있는 데이터소스는 다음과 같습니다. 

- Amazon Redshift
- Apache Hadoop Hive
- Cloudera Hadoop Hive
- Cloudera Impala
- Dropbox
- Greenplum
- Google Analytics
- Hortonworks Hadoop Hive
- IBM DB2
- Impala
- Informix
- Microsoft Excel
- Microsoft SQL Server
- MongoDB
- MySQL
- Netezza
- Oracle SaaS Application
- Oracle Database
- Oracle Essbase
- Salesforce
- SAP SybaseIQ
- Spark
- Teradata 

 
***	
## 데이터소스 선택

데이터소스의 접속을 위해서, 먼저 DVD(Data Visualization Desktop) 상단의 '데이터소스'메뉴을 클릭합니다.
그러면 데이터와 관련한 메뉴항목이 화면의 왼쪽에 표시되는데, '생성'항목의 '데이터소스'를 클릭하게 되면 '새 데이터 소스 생성'창이 나타납니다. 

![](https://oracloud-kr-teamrepo.github.io/2017/05/dvcs_002/DVCS002_Menu.JPG)	

'새 데이터 소스선택' 창에서 '파일'버튼을 클릭하면 Local PC에 저장되어 있는 Excel 및 txt 파일을 데이터분석을 위한 소스로 사용할 수 있으며, '접속추가'버튼을 클릭하면
DVD에서 제공하는 각 종 Database 등을 직접 연결하여 분설할 수 있습니다. 


![](https://oracloud-kr-teamrepo.github.io/2017/05/dvcs_002/DVCS_new data source.JPG)

DVD는 오라클 Database 뿐만 아니라, DB2, MS SQL Server, Teradata 등 각종 RDB의 직접 연결 할 수 있으며, 또한 Apach Hive, Spark, Hadoop 등 비정형데이터와 Dropbox, Google Analytics 등과의 connection도 제공합니다.

![](https://oracloud-kr-teamrepo.github.io/2017/05/dvcs_002/DVCS002_Data Source.JPG)
***	
## 새 접속 추가

분석을 위한 Connectoin을 선택하면, 접속에 필요한 기본정보의 입력을 위한 창이 열리게 됩니다. 
예를 들어 'Oracle Database'의 접속을 위해서는 '접속이름', '호스트', '포트', '사용자 이름', '비밀번호', '서비스 이름'을 입력하게 됩니다.

![](https://oracloud-kr-teamrepo.github.io/2017/05/dvcs_002/DVCS_data connection.JPG)

성공적으로 접속이 완료되면, 아래와 같이 DB에 포함되어 있는 Database와 Table을 조회할 수 있으며, 분석에 필요한 항목만 선택하여 분석 영역으로 정의하게 됩니다. 

	
![](https://oracloud-kr-teamrepo.github.io/2017/05/dvcs_002/DVCS_data selection for analytics.JPG)

분석항목을 선택한 후 '데이터 새로고침'을 클릭하게 되면 아래와 같이 데이터를 확인 할 수 있습니다. 
![](https://oracloud-kr-teamrepo.github.io/2017/05/dvcs_002/DVCS_data selection for analytics_complete.JPG)	
***	
## 데이터 시각화

접속추가가 완료되면, 사용자는 즉시  시각화 작업을 시작할 수 있습니다. (아래 화면의 왼쪽이 오라클 Database에서 불러오 분석 항목임-NYC TAXI DATA)

	
![](https://oracloud-kr-teamrepo.github.io/2017/05/dvcs_002/DVCS002_Data Visualization.JPG)	


너무 쉽죠?

다음 과정에서는 'Excel'데이터를 upload하여 본격적인 데이터 시각화를 해 보도록 하겠습니다. 

***
## 추가 정보

- [Oracle DV Desktop 커뮤니티](https://community.oracle.com/community/business_intelligence/data-visualization)
- [Oracle DV Desktop 블로그](http://oracledataviz.blogspot.kr/)
- [Oracle DV Desktop 유투브 동영상](https://www.youtube.com/watch?v=WnDXCwa-OMo&list=PLOcpw36tp3yLJ5EK7U6EwXF6Z4jLYGmSF)
- [Oracle DV Desktop 공식문서](http://docs.oracle.com/cloud/latest/data-visualization-cloud/index.html)