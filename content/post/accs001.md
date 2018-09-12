+++
date = "2018-09-11T17:00:00+09:00"
description = "Application Container Cloud Service를 활용하여 손쉽게 웹 서비스를 구축하는 방법에 대한 소개입니다."
title = "Application Container로 10분 안에 웹서비스 구축"
thumbnailInList = "https://monosnap.com/image/C1zbSeu5fc6l78BLY3qFwSZqmPe8EQ.png"
thumbnailInPost = "https://monosnap.com/image/gYEfJYExUmyNuGFvqiX1pDdoBzdd1G.png"
tags = ["ORACLE", "Oracle Cloud", "Application Container Cloud", "accs", "Cloud Native", "Polyglot", "오라클 클라우드", "Java EE"]
categories = ["Oracle-Cloud"]
author = "wonjo.yoo"
language = ""  
+++

Oracle Application Container Cloud를 활용해서 10분 안에 jsp를 바로 서비스하는 과정을 설명합니다.

***
## Hello App 준비하기 
간단히 Hello를 출력하는 index.jsp를 아래와 같이 작성하겠습니다.
<pre><code><%
	out.println("Hello Oracle Cloud");
%>
</code></pre>
위 파일이 들어 있는 test폴더를 hello.war 파일로 압축합니다.
<pre><code>jar cvf hello.war test
</code></pre>

***
## Oracle Cloud에 접속하기
이제 Oracle Cloud에 접속합니다.

Oracle Cloud 계정이 없는 경우에는 [Oracle Trial 신청절차](http://www.oracloud.kr/post/oracle_cloud_trial/) 문서를 참고하시기 바랍니다.

Cloud Console에 접속을 한 후 Application Container Cloud Service을 사용하기 위해서는 왼쪽 상단의 햄버거 모양을 누른 후 나오는 왼쪽 메뉴에서 Application Container를 선택하시면 됩니다.
메인 Console 화면에 Application Container Cloud가 표시 되게 하기 위해서는 오른쪽 상단의 Customize Dashboard 버튼을 누른 후에 Application Container를 Show가 되도록 선택하십시오.

![Alt text](https://monosnap.com/image/LIGkJ4WygoYBQUMkKvbMOkK0ztHPSM.png)

***
## WAR 파일 업로드 하고, 서비스 생성하기
Application Container Cloud Service에 접속한 후 Create Application 버튼을 클릭합니다.
 
![ACCS Console](https://monosnap.com/image/TpZHnDnQkA994lplcHGpvIhZfNXqXU.png)

Application Container는 Java SE(일반적으로 알려져 있는 Java), Java EE(Web Application), Node, PHP, Python, Ruby, 닷넷, Go의 다양한 언어를 위한 플랫폼을 지원합니다. 

별도의 환경을 설치할 필요 없이 소스만 업로드를 하면 바로 deploy되어 서비스 할 수 있는 환경을 만들어 줍니다.

이번 예제는 JSP Web Application이므로 Java EE를 선택하겠습니다.

<img src="https://monosnap.com/image/yKmANQHhR6x72l77GR1l0Fsym180Ib.png" width=400>

이름을 hello라고 입력하고, 파일 선택을 클릭하여 hello.war 파일을 선택합니다.

생성시에 instances 갯수를 선택하게 되는데, 2개 이상을 만들게 되면 앞단에 자동으로 Load Balancer가 추가가 되어 Balancing을 해주게 됩니다. 

사용할 메모리를 2G로 지정합니다. Application과 서비스의 규모에 맞게 설정하시면 됩니다.

<img src="https://monosnap.com/image/mCIMdSRrScQvzojQSTFHXGc6hPdDVe.png" width=600>

2~3분 정도 지나면 Application deploy가 완료되고 바로 서비스가 가능한 주소가 할당되게 됩니다.

아래에 붉은 색으로 표시한 곳을 클릭하면 서비스 URL로 바로 이동할 수 있습니다.

![Alt text](https://monosnap.com/image/4NDkNuZl6mLoxmPjBlFsUowhwUPFCD.png)

정상적으로 Hello 페이지가 호출되는 것을 확인할 수 있습니다.
![Alt text](https://monosnap.com/image/6YLl1VnObhym5lXphPFyiSbB08ATmg.png)

짧은 시간 안에 OS 뿐만 아니라 JDK나 WAS(Web Application Server)의 설치 없이 바로 운영이 가능한 서비스가 완성되었습니다.

***
## 관리 및 모니터링
Application Container Cloud 콘솔에서 만들어진 서비스인 hello를 클릭하고 들어가면 모니터링 및 배포를 할 수 있는 관리 화면이 나오게 됩니다.
현재 2개의 인스턴스가 만들어져 있는데, 트래픽의 정도에 따라 인스턴스 수를 증가시키고자 할 경우 숫자를 직접 쓰거나 화살표 버튼을 클릭하고 Apply 버튼만 누르면 바로 인스턴스가 증가 됩니다.

![모니터링](https://monosnap.com/image/eUUw3PohUAGyU7MW1Z7Ysg2d73fH4l.png)

빠르게 Web Application을 개발하고 배포해서 서비스 하고자 할 경우 Application Container Cloud 서비스를 이용해 보시기 바랍니다.

***
## 참고 자료
- [Deploy a Java EE Application to Oracle Cloud](http://www.oracle.com/webfolder/technetwork/tutorials/obe/cloud/apaas/javaEE/java-ee-basic-accs/java-ee-basic-accs.html)

- [Oracle Application Container Cloud Service 매뉴얼](https://docs.oracle.com/en/cloud/paas/app-container-cloud/index.html)

- [Deploy your Applications to the Cloud - youtube 링크](https://www.youtube.com/watch?v=NqeuyUuuXrU)
