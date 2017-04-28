+++
author = "sunguk.hong"
categories = []
date = "2017-04-26T15:00:47+09:00"
description = "쉽게 따라해보는 데이터 시각화 제1회 '실습환경 구성'"
language = ""
tags = ["Data Visualization","Desktop", "Analytics"]
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2017/04/dvcs/intro.JPG"
thumbnailInPost = "https://oracloud-kr-teamrepo.github.io/2017/04/dvcs/DV main.JPG"
title = "쉽게 따라해보는 데이터 시각화 제1회 '실습환경 구성'"

+++

데이터 분석은 대시보드와 레포팅으로 분류되는 Business Intelligence(BI)영역과 데이터탐색으로 분류되는 Data Discovery 영역으로 나뉘어 집니다.

최근에 각광받고 있는 분석 영역이 Data Discovery 분야인데, 가트너(2016)도 Data Discovery, Self-Service, Cloud를 New BA(Business Analytics)로 정의한 바 있습니다.
실제로 Tableau, 클릭뷰 등과 같은 New BA쪽에 강점을 갖고 있는 분석툴이 고객으로 부터 많은 관심을 받으며 분석 소프트웨어 시장을 리딩하고 있습니다. 

그리고 이와 같은 New BA를 지향하는 분석툴은 '손쉽데 데이터를 시각화할 수 있다'는 특징을 가지고 있습니다. 
 
 
***	
## 가장 쉬운 시각화 솔루션 'Data Visualization'

오라클은 지속적인 M&A를 바탕으로 BI시장에서 리더의 자리를 유지해 왔고, 지금도 국내외 많은 기업들이 오라클의 분석솔루션을 사용하고 있습니다. 

최근에는 New BA에 집중하여 
Anlaytics Cloud, Big Data Discovery Cloud, Big Data Preparation Cloud, Data Visualization Cloud 등의 새로운 Cloud서비스를 발표하였습니다.
주목할 점은 모든 분석환경은 Hybrid Cloud (Public Cloud, Private Cloud, On-Premise)고 구성이 가능하다는 것입니다.

오라클의 Business Analytics 제품중 최근에 출시된 'Data Visualization Cloud Service(이하 DVCS)'은 매우 쉬운 시각화 제품입니다. 
실제 필자의 경험에 의하면, IT에 지식이 없어도 1~2시간의 교육만으로도 교육생들은 데이터베이스에 직접 접근하여 Business Insight를 위한 데이터 시각화를 무리없이 수행해 낼 수 있었습니다.

본 연재는 다음의 목표를 달성하고자 합니다. 

첫째, 분석 전문가들의 영역으로 취급되었던 '데이터분석'을 쉽게 해보자 !

둘째, 그로인해서 데이터에 친숙해지자 !!

셋째, 업무에 바로 적용해 보자 !!!

넷째, 툴사용법을 익히는데 10%, 분석에 90%의 시간을 사용하자 !!!!


***	
## Oracle Data Visualization 

본 연재에서 사용하게 될 Tool은 Oracle Data Visualization(ODV)입니다. ODV는 Cloud버전, On-Premise버전, Desktop버전이 있으며 기능은 거의 동일하며 상호간의 호환이 됩니다. 
그리고 따라하기 실습은 Oracle Data Visualization Desktop (ODV Desktop)버전을 사용하게 됩니다. 

Oracle Data Visualization의 주요기능은 다음과 같습니다. 

- 데이터 연결 : Connection, Quick Data Explore, Data Flow 작성
- 시각화 : Drag-Drop 방식의 데이터 시각화 및 공유
- 고급 통계분석 : 오픈소스 R과 연계한 고급 통계분석

		
***	
### 1. Oracle Public Cloud에서 ODV 사용해 보기

ODV는 Oracle Public Cloud에서 제공하는 PaaS 서비스이며, 아래와 같이 Web브라우저 또는 Mobile 환경에서 데이터 시각화를 가능케 합니다. 

ODV Cloud Service를 체험해 보고자 하는 분들은 아래 화면의 'Try It'버튼을 클릭하여 '1개월 무료 체험'을 해보실 수 있습니다. 
	
![](https://oracloud-kr-teamrepo.github.io/2017/04/dvcs/cloud main.JPG)	

ODV Cloud Service 체험을 위해서는 계정신청이 필요합니다. 

[클릭! 오라클 클라우드 트라이얼 계정신청 방법](http://www.oracloud.kr/post/accont)


***	
### 2. ODV Desktop 버전 설치하기


ODV Desktop은 Oracle OTN홈페이지에서 'License Agreement'에 동의한 후, 파일을 다운받아서 PC에 설치하시면 됩니다. 
설치 위치는 기본폴더(C:\Program Files\Oracle Data Visualization Desktop)를 사용합니다.

[클릭! ODV Desktop 다운로드](http://www.oracle.com/technetwork/middleware/oracle-data-visualization/downloads/oracle-data-visualization-desktop-2938957.html)

![](https://oracloud-kr-teamrepo.github.io/2017/04/dvcs/desktop download.JPG)	

ODV Desktop설치를 위한 최소 사양

- Operating System: Microsoft Windows x64 (64-bit) 7 SP1+, 8.1, or 10; Windows Server 2012 or 2012 R2)
- CPU: Intel(R) Core(TM)2 Duo CPU E8400 @ 3.00GHz, 2992 Mhz 2 Cores, 2 Logical Processors or faster-
- Memory: 4.00 GB Memory or more
- Minimum free disk space: 2GB; plus space for any uploaded data files
- User privileges - User needs Admin privileges to install


***
### 3. Advanced Analytics(AA) 설치하기

ODV Desktop의 설치가 완료한 후, Forecasting, Clustering, Outlier, Text Mining 등의 고급 통계분석을 위한 '오라클 R배포판'을 설치해야 합니다. 

ODV Desktop이 설치되면 아래와 같이 'Oracle' 프로그팸 폴더가 생성되고, 'Install Advanced Analytics'를 클릭하면 '오라클 R배포판'이 설치됩니다. AA 설치를 위해서는 인터넷에 연결되어 있어하며, 기본 폴더(C:\Program Files\R)에 설치됩니다.

![](https://oracloud-kr-teamrepo.github.io/2017/04/dvcs/AA install.JPG)	

설치가 성공적으로 완료되면 다음의 메시지가 출력됩니다. 

![](https://oracloud-kr-teamrepo.github.io/2017/04/dvcs/AA install result.JPG)	


***
### 4. ODV Desktop 초기화면

모든 설치를 완료한 후, 프로그램을 실행시키면 다음과 같은 초기 화면을 확일 할 수 있습니다. 

![](https://oracloud-kr-teamrepo.github.io/2017/04/dvcs/desktop main.JPG)	

설치 잘 되셨나요?

다음 과정에서는 '데이터 가져오기'에 대해 공부해 보도록 하겠습니다. 

***
## 추가 정보

- [Oracle DV Desktop 커뮤니티](https://community.oracle.com/community/business_intelligence/data-visualization)
- [Oracle DV Desktop 블로그](http://oracledataviz.blogspot.kr/)
- [Oracle DV Desktop 유투브 동영상](https://www.youtube.com/watch?v=WnDXCwa-OMo&list=PLOcpw36tp3yLJ5EK7U6EwXF6Z4jLYGmSF)
- [Oracle DV Desktop 공식문서](http://docs.oracle.com/cloud/latest/data-visualization-cloud/index.html)