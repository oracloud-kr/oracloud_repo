+++
date = "2018-11-11T21:59:47+09:00"
description = "오라클 챗봇을 이용해서 대화형 챗봇을 구현하는 가이드 입니다. 대화를 통해 사용자에게 필요한 정보를 물어보고 사용자가 원하는 정보를 보다 정확하게 제공할 수 있습니다."
title = "오라클 챗봇을 이용해서 대화형 챗봇 만들기"
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2018/chatbot_image_repo/logo/dialogFlow_icon.jpg"
thumbnailInPost = "https://oracloud-kr-teamrepo.github.io/2018/chatbot_image_repo/logo/dialogFlow.jpg"
tags = ["chatbot", "챗봇", "채터봇", "인공지능", "인공지능봇", "오라클 챗봇", "오라클 chatbot", "oracle chatbot", "대화형봇", "한국어챗봇", "대화형"]
categories = ["Chatbot"]
author = "sung.hye.jeon"
language = ""  
+++

### - 대화 흐름 정의? 

대화 흐름 정의 (dialog flow)는 대화 자체의 모델입니다. 대화의 흐름을 정의함으로써, 봇과 사용자 간의 상호 작용을 관리 할 수 ​​있습니다. 간단한 Markup language인 YAML 문법을 사용하는 OBotML은 간단하고 확장성이 좋기때문에, 대화의 흐름을 편하고 완벽하게 구현할 수 있습니다.

### - Dialog Flow Structure 
![](https://oracloud-kr-teamrepo.github.io/2018/chatbot_image_repo/15413938977478/15420035293929.jpg)


대화의 흐름을 위와 같은 state machine을 정의한다고 생각하시면 됩니다. Bot 엔진을 사용해서 어떤의도인지 파악 한 후에 어떻게 대답을 어떻게 구성할지, 더 물어볼 것은 없는지 자유롭게 디자인 하실 수 있습니다. 변수를 생성해서 유저의 답변에 따라 변수의 값을 비교하거나, 어떠한 조건문을 확인 함으로써 다른 단계로 유저를 가이드 할 수 있습니다. 
예를 들어, 위의 그림처럼, 의도를 파악한 후에, 임직원 조회를 했습니다. "임직원 조회" 단계에서는 뒷단에 연결된 API를 호출해서, 조회를 진행했습니다. 그리고 다음 단계인 "조회 결과"에서는 유저에게 조회결과를 보여줍니다. 그리고 "조회결과"단계의 유저의 답변에 의해 초기단계로 이동할지, 조회된 임직원에게 이메일을 보내거나, 그임직원과의 회의실을 예약하는등 여러가지 서비스를 구현 할 수 있습니다. 

OBotML 구현에 있어서는 3가지 중요한 부분이 있습니다. Context, Transitions, states 입니다. 

-   context : 필요한 변수를 context안에 설정하고 그 변수를 각각의 user session동안 기억할 수 있습니다. session이 살아있는한, 유저는 언제든지 돌아와서 그전의 대화나 선택들을 불러올 수 있습니다. variable type은 primitive types (int, string, boolean, double, float) 혹은 entity type 으로 설정 할 수 있습니다.
-   state : 각각의 state(단계)는 봇이 해야할 일을 정의 하고 있습니다. 각각의 단계에서 대답을 할수도, 만들어 놓은 서비스를 부를수도 혹은 두개 다 부를 수도 있습니다. 
-   transition : state 간의 transition을 설정할 수 있습니다. state에서 정의 해 놓은 조건에 따라 특정한 state로 혹은 default로 다음 스테이트로 넘어가게끔 설정 할 수 있습니다. 

  ![](https://oracloud-kr-teamrepo.github.io/2018/chatbot_image_repo/15413938977478/15414037722045.png)
  

### - Dialog flow components 
미리 만들어져 있는 component template을 이용함으로써 간단히 컴포넌트들을 사용하실 수 있습니다. 아래 보이는 화면은 오라클 봇 프로덕트에서 Flow작성할때 사용 할 수 있는 컴포넌트 추가 UI화면입니다. 각 컴포넌트들에는 #으로 주석 처리 되어있는 설명을 참고하시면 쉽게 구현 하실 수 있습니다.
![Screen Shot 2018-11-12 at 3.34.42 P](https://oracloud-kr-teamrepo.github.io/2018/chatbot_image_repo/15413938977478/Screen%20Shot%202018-11-12%20at%203.34.42%20PM.png)

- Control : if-else 문 혹은 switch 문을 쓸 수 있는 템플렛 입니다. 정의한 variable이 어떤 값인지 비교하거나 switch 문을 통해 원하는 단계로 보낼 수 있습니다.

![Screen Shot 2018-11-12 at 3.36.32 P](https://oracloud-kr-teamrepo.github.io/2018/chatbot_image_repo/15413938977478/Screen%20Shot%202018-11-12%20at%203.36.32%20PM.png)

- Language : multilingual bot을 만들때 유용하게 사용 하실 수 있는 컴포넌트 입니다. 사용자가 넣은 언어를 자동으로 감지해서 어떤 언어지 파악한 후에 번역을 하거나, 단답형 질문 유사성 검색 모듈인 Q&A의 자세한 설정을 하는등 언어에 관련한 여러가지 설정 및 구현을 편리하게 할 수 있습니다.

![Screen Shot 2018-11-12 at 3.51.24 P](https://oracloud-kr-teamrepo.github.io/2018/chatbot_image_repo/15413938977478/Screen%20Shot%202018-11-12%20at%203.51.24%20PM.png)


- Security : Oauth 방식으로 서드파티와 연결을 손쉽게 구현 할 수 있습니다.

![Screen Shot 2018-11-12 at 3.53.45 P](https://oracloud-kr-teamrepo.github.io/2018/chatbot_image_repo/15413938977478/Screen%20Shot%202018-11-12%20at%203.53.45%20PM.png)

- User Interface : 상담원 시스템으로 연결하거나, text 이외의 다른 UI를 이용해 답변 하는 봇을 만들 고 싶으실때 사용 할 수 있습니다. 이 컴포넌트를 사용해서 구현한 봇을 Facebook와 같은 Public messenger로 연결 하거나, 오라클에서 제공하는 web sdk를 통해 웹사이트와 연결하면 아래와 같은 화면을 볼 수 있습니다. 
 
![Screen Shot 2018-11-12 at 4.03.30 P](https://oracloud-kr-teamrepo.github.io/2018/chatbot_image_repo/15413938977478/Screen%20Shot%202018-11-12%20at%204.03.30%20PM.png)
![Screen Shot 2018-11-12 at 4.03.48 P](https://oracloud-kr-teamrepo.github.io/2018/chatbot_image_repo/15413938977478/Screen%20Shot%202018-11-12%20at%204.03.48%20PM.png)

![Screen Shot 2018-11-12 at 4.04.11 P](https://oracloud-kr-teamrepo.github.io/2018/chatbot_image_repo/15413938977478/Screen%20Shot%202018-11-12%20at%204.04.11%20PM.png)

- Variable : 변수를 관리할 때 사용 하실 수 있습니다. 
![Screen Shot 2018-11-12 at 4.08.20 P](https://oracloud-kr-teamrepo.github.io/2018/chatbot_image_repo/15413938977478/Screen%20Shot%202018-11-12%20at%204.08.20%20PM.png)


## Dialog flow testing 

- 문법 에러 찾기

원하시는 대화의 흐름을 구현하신 후에, "validate"를 눌러주세요. 아래와 같은 이상이없다는 메세지를 보시면, 테스트를 해보시면 됩니다. 하지마, 아래와 같은 메세지가 아닌, 문제가 있다는 메세지를 보셨을 경우엔, 오른쪽 상단의 bug모양의 버튼을 눌러서 문제점에 대한 Description을 확인후 고치시면 됩니다. 

![Screen Shot 2018-11-12 at 4.10.31 P](https://oracloud-kr-teamrepo.github.io/2018/chatbot_image_repo/15413938977478/Screen%20Shot%202018-11-12%20at%204.10.31%20PM.png)

- 논리 에러 찾기

오른쪽 상단의 세모 버튼을 눌러서 유저 입장에서 타이핑 하며, 대화흐름이 원하는대로 흘러가는지 확인 하실 수 있습니다. 

 ![Screen Shot 2018-11-12 at 4.19.14 P](https://oracloud-kr-teamrepo.github.io/2018/chatbot_image_repo/15413938977478/Screen%20Shot%202018-11-12%20at%204.19.14%20PM.png)


### - SDK 
User interface component에 나왔던, web SDK 및 여러가지 유용한 sdk를 다운 받을 수 있는 링크입니다. 간단히 기존 웹사이트에 추가해, 오라클 챗봇과 연결 할 수 있습니다. 
[Oracle SDK 다운로드 페이지 바로가기](https://www.oracle.com/technetwork/topics/cloud/downloads/mobile-cloud-service-3636470.html)

### - 공식가이드 
각 컴포넌트에 대한 예제와 보다 자세한 설명을 포함 하고 있는 가이드 입니다.
[Oracle 챗봇 Dialog Flow 공식 개발 가이드](https://docs.oracle.com/en/cloud/paas/mobile-suite/use-chatbot/dialog-flow-definition.html#GUID-CE86A43E-286A-462C-8B80-0BA2666D80F7)

### - 오라클 챗봇 테스트 계정? 
오라클 챗봇을 만들 수 있는 Intelligent Bots를 체험 계정은 아래에서 신청 하실 수 있습니다. 문제가 있으시면 오라클 영업대표님께 문의부탁드립니다. 
[Oracle 트라이얼 계정 신청 페이지 바로가기](https://cloud.oracle.com/en_US/tryit)


### - 오라클 챗봇에 대해 배워보기? 
개발자 이신가요? 직접 코드를 채워가면서 챗봇에 기능 추가하는 workshop입니다. [워크샵 바로가기] (https://github.com/OracleCloudKr/Chatbot_WorkShop_Enhanced)
현업에 계신가요? 만들어져 있는 코드를 넣어보며 직접 테스트 해보실 수 있는 workshop입니다. [워크샵 바로가기](https://github.com/OracleCloudKr/ChatBot_Workshop)


