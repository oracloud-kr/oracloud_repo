+++
date = "2018-09-21T16:00:00+09:00"
description = "Application Container Cloud Service를 활용하여 손쉽게 Node.JS를 배포하는 방법에 대한 소개입니다."
title = "Application Container로 10분 안에 Node.js 배포하기"
thumbnailInList = "https://d1ro8r1rbfn3jf.cloudfront.net/ms_202447/YqgG9G8BOy5QS7EPvTsQFl5OsWrpwr/accs_node.png?Expires=1537598563&Signature=RFS2C2xHjpTMkIz-7oq8TgPx3Eznk9LMk1adxS83ey3Y0lXwPriuB25GJ5hcHtAmO7D7B3yKAJq2lGBxtTSc5R3UsZ6U53hPqhAwdJxG1tKhzdtAdxR~QaVpbyPMJ85D~lCPGV6LavIFPjJcykMsEfYgDCeTQiKgTCSoIhrE2nx8jydqIhh6LtuFADzgWC~228GDqQiRMfEOAqT2kygeXiBoOPAXinE3YxZ~0CqFtn1rYdYrI~74uJp8XqerH4XQtkY3BY~9ybUYcQninx6E8pxHYQPy~I2nFy0Kc5V28L9QuWccVmbLHRJsa7ZSoDDOsG8F~N4tpYqDh2jDCEOR-g__&Key-Pair-Id=APKAJHEJJBIZWFB73RSA"
thumbnailInPost = "https://d1ro8r1rbfn3jf.cloudfront.net/ms_198636/rLayDCqyUcW7gNitRajNC3Rg48PhjL/Application%2BContainer%2BCloud%2BService%2BProficiency%2BWebcast%2Bfor%2BJAPAC%2B-%2BFeb%2B5%2B2016%2B-%2BPPT%2B2018-09-11%2B16-41-43.png?Expires=1537598491&Signature=IclWFCDeUeW74v9auV1wdkXwTtafHm20dRI5XSdTEjfY5psEFLg4GwEzZ9woTfuyLn~ASQMcv1ivK2LjNoIL~I7V56hS6e3mMB84Xk1av42QvMYjXcd9NLdFbT6MnahhWZAVeKEcVbudgMZ1xBXNULGdAVlE043ynYB5LOgt2zLPKuVOvlbp1BLdKALsnp2f2uCCI-1sRVgoGKmWO3UW4H0PoDdilVwHIY941HFmOAYuH18yDVOLXz1ByfXquzlDr23ZO~ccWdlp0jWZq4NnnRcoaMjL0CCotQAotHbpYcXimQ7O68VoNLQV2BHS62iG27Av4usdTwS-qo6Pry-o1Q__&Key-Pair-Id=APKAJHEJJBIZWFB73RSA"
tags = ["ORACLE", "Oracle Cloud", "Application Container Cloud", "accs", "Cloud Native", "Polyglot", "오라클 클라우드", "Node.js"]
categories = ["Oracle-Cloud"]
author = "wonjo.yoo"
language = ""  
+++

Oracle Application Container Cloud를 활용해서 10분 안에 Node.js를 바로 서비스하는 과정을 설명합니다.

***
Oracle Application Container Cloud는 Java, Node.js, PHP, Python, Ruby, Dotnet, Go등의 다양한 언어를 지원합니다.

이전 Post [Application Container로 10분 안에 웹서비스 구축하기](http://www.oracloud.kr/post/accs001/)에서는 JavaEE 인 WAR 파일 배포에 대해 소개드렸습니다.

이번에는 Node.js를 배포하는 방법에 대해서 알아보겠습니다.

## Hello Node.js App 준비하기
간단히 Http 요청을 받을 수 있는 node.js용 파일을 아래와 같이 작성하고 myapp.js라고 저장하겠습니다.
페이지를 호출하면 Hello World라고 출력을 하게 됩니다.

```
    var http = require('http');
    // Read Environment Parameters
    var port = Number(process.env.PORT || 8080);
    var greeting = process.env.GREETING || 'Hello World!';
    var server = http.createServer(function (request, response) {
    response.writeHead(200, {"Content-Type": "text/plain"});
    response.end(greeting + "\n");
    });
    server.listen(port);
```
Application Container Cloud Service 에서는 이미 정의되어 있는 PORT 환경 변수를 사용해서 파라미터를 전달하기 때문에 process.env.PORT를 사용해서 server를 listen 하도록 작성하셔야 합니다.

## Meta파일 작성하기

Application Container Cloud Service 에서는 Meta파일을 작성해서 서비스를 하고자 하는 runtime이 무엇인지, 버전은 어떻게 되는지를 미리 설정파일로 작성할 수 있습니다.

```
{
  "runtime":{
    "majorVersion":"6"
  },
  "command": "node myapp.js",
  "release": {},
  "notes": "Hello World application"
}
```
manifest.json 이라는 이름으로 작성하셔야 하며 위와 같은 포맷을 갖고 있습니다.
조금 더 자세한 설명은 다음의 문서를 참고하시면 됩니다.
[manifest.json 상세내용 참고](https://docs.oracle.com/en/cloud/paas/app-container-cloud/dvcjv/creating-manifest-json-file.html)

이제 준비는 끝났으니 위 두 파일을 zip으로 압축합니다.

***
## Oracle Cloud에 Node 파일 배포하고 서비스 생성하기
Oracle Cloud에 접속한 후 Application Container로 이동합니다.
Application Container Cloud Service에 접속한 후 Create Application 버튼을 클릭합니다.

이 과정은 이전 포스트에서 참고하실 수 있습니다.

[Application Container로 10분 안에 웹서비스 구축하기](http://www.oracloud.kr/post/accs001/)

***
 
이번 예제는 Node 를 선택하겠습니다.

<img src="https://monosnap.com/image/IqlJ5ba7cYJT0Cvq4MhZW3oYkiOT9P.png" width=400>

![Alt text](https://monosnap.com/image/ckqEm8koLxPmoRrNJRAPsQb12RvlIy.png)

위에서 준비한 zip 파일을 선택하고, samplenode라고 이름을 입력합니다.
인스턴스는 1개로, 메모리는 2G로 지정하겠습니다.

2~3분 정도 지나면 Application deploy가 완료되고 바로 서비스가 가능한 주소가 할당되게 됩니다.

![Alt text](https://monosnap.com/image/zdVk2RCZouAIy91WheMCz7lXDhqBPQ.png)

아래에 링크를 클릭해서 서비스 URL로 이동해 보겠습니다.

![Alt text](https://monosnap.com/image/fnsbODzVAPdYfzhSzXcHnE6C1rJOrn)
정상적으로 서비스 페이지가 열렸습니다.

빠른 시간안에 OS 뿐만 아니라 node.js의 설치 없이 소스 배포 만으로 바로 운영이 가능한 서비스가 완성되었습니다.

***
## 관리기능 - Upgrade
생성된 서비스인 samplenode를 보면 'One or more update(s) available'이 나옵니다.
node를 생성시에 버전을 6로 했기 때문에 최신버전의 node runtime patch가 있다는 것을 알려주고 있습니다.

![Alt text](https://monosnap.com/image/ela3Rzo78jQyLmaY9VNRkMqw9EPpbt.png)

위 링크를 클릭하거나, Samplenode 서비스를 클릭해서 관리 페이지로 이동한 후 좌측메뉴에서 Administration을 클릭합니다.

![Alt text](https://monosnap.com/image/MbITIz7BBO0XALQ4BzwbER2hkqGWWT.png)
node runtime 8.9.3 버전으로의 업그레이드가 가능하다고 보여주고 있습니다.
우측에 업데이트 버튼을 클릭하면 업데이틀 하겠냐고 물어보게 됩니다.

![Alt text](https://monosnap.com/image/sBav8c91lzSYifgEGvotTyp0x6EXUE)

Restart를 클릭하면 자동으로 runtime을 patch해줍니다.

이때 만약 인스턴스가 2개 이상일 경우에는 아래와 같이 rolling으로 patch를 할 것인지를 선택할 수 있는데, 서비스 다운타임 없이 패치가 적용 되게 됩니다.

![Alt text](https://monosnap.com/image/KmC6c9Q1A0ZPKHnxwlgBsZF54N8zog.png)


Node.js로 서비스를 하고자 한다면 손쉽게 서비스환경을 구축할 수 있는 Oracle Application Container를 활용해 보시기 바랍니다.

***
## 참고 자료
- [Oracle Application Container Cloud Service - Create Node.js Applications](https://docs.oracle.com/en/cloud/paas/app-container-cloud/create-sample-node.js-applications.html)

- [Oracle Application Container Cloud Service 매뉴얼](https://docs.oracle.com/en/cloud/paas/app-container-cloud/index.html)

- [Deploy your Applications to the Cloud - youtube 링크](https://www.youtube.com/watch?v=NqeuyUuuXrU)