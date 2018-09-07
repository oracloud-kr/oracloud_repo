+++
date = "2018-09-01T21:59:47+09:00"
description = "챗봇이 무엇인지 설명 하는 글입니다. "
title = "챗봇이란 무엇인가. 챗봇 용어 및 개념 완벽 정리"
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2018/chatbot_image_repo/logo/chatbot.png"
thumbnailInPost = "https://oracloud-kr-teamrepo.github.io/2018/chatbot_image_repo/logo/intrologo.jpg"
tags = ["chatbot", "챗봇", "채터봇", "인공지능", "인공지능봇", "오라클 챗봇", "오라클 chatbot", "oracle chatbot"]
categories = ["Chatbot"]
author = "sung.hye.jeon"
language = ""  
+++

## 챗봇이란 무엇인가. 챗봇 용어 및 개념 완벽 정리 


### - 챗봇이란? 
챗봇이란 간단히말해, 카톡이나 라인과 같은 메신저로 상대방과 대화할때 상대편에 사람이 아닌 software program이 대화를 하고 있다고 생각하시면 됩니다. 
![Screen Shot 2018-08-22 at 2.47.52 P](https://oracloud-kr-teamrepo.github.io/2018/chatbot_image_repo/15349031844600/Screen%20Shot%202018-08-22%20at%202.47.52%20PM.png)

![Screen Shot 2018-08-22 at 2.47.40 P](https://oracloud-kr-teamrepo.github.io/2018/chatbot_image_repo/15349031844600/Screen%20Shot%202018-08-22%20at%202.47.40%20PM.png)

챗봇을 많은 기업들이 도입해, 고객 상담 창구를 확장한 형태로 사용 하는 예를 흔히 보실 수 있습니다. 젊은 밀레니얼 세대는 새로운 앱을 다운 받기를 싫어하며 전화보다는 카톡 혹은 페이스북 메신저를 선호합니다. 때문에, 어떤 기업에서 물건을 사고 레스토랑에 예약을 하거나 행사에 대한 여러가지 질문들 또한 Instant Messenger에서 이뤄지기를 기대하는 것은 자연스러운 현상입니다. 기업 입장에서도 이렇게 유저가 선호하는 채널인 Instant Messenger와 커뮤니케이션을 통해 "stay connected" 될 수 있으며, 유저가 원하는 것이 무엇인지 정확히 알고 가장 최적화되고 적합한 서비스를 제공할수 있습니다. 또한, 무엇보다 24시간 응대가 가능하기때문에, 52시간 근무제로 금지된 근무시간 이외의 영업시간에 들어오는 질문에도 정확하고 신속한 답변을 함으로써 고객의 만족도를 높일 수 있습니다. 

### - 챗봇 형태  
챗봇은 크게 두가지 형식으로 구성될 수 있습니다. 

1. menu-driven
2. AI-based NLP
![Screen Shot 2018-08-22 at 3.37.16 P](https://oracloud-kr-teamrepo.github.io/2018/chatbot_image_repo/15349031844600/Screen%20Shot%202018-08-22%20at%203.37.16%20PM.png)

일반적인 문장으로 물어봤을때 봇이 "의도"가 무엇인지 파악하는 방식이 AI-based NLP입니다. 하지만, 보통 사람의 언어를 완벽히 알아듣는 자연어처리 엔진 (NLP)는 구성하는 것이 매우 어렵습니다. 때문에, 버튼을 눌러서 원하는 바를 사용자가 직접 알려주는 방식인 menu-driven으로 분류 할 수 있습니다.

### - 용어 정리 
Intent : 의도 입니다. 봇의 행동 단위라고 볼수 있습니다. 여러가지 사람의 언어로 물어봐도 어떠한 "의도"로 말하냐에 따라서 봇이 어떤행동을 해야하는지 정해진다고 볼 수 있습니다. 10개의 의도를 파악하기 위해서는 각각의 의도에 여러가지 문장으로 각각의 의도를 가르쳐야합니다.

Training Data : 각 각의 의도를 가르치기 위한 학습데이터입니다. 각 엔진을 정확히 이해한 후 최적의 학습데이터로 학습 시키는 것이 중요합니다. 다시 말해, 학습데이터의 퀄리티와 엔진의 성능이 봇의 정확도를 좌우합니다. 
 
entity : entity는사용자에게 답변을 하기위해 어떠한 행동을 수행할때 꼭 필요한 중요 정보라고 보시면 됩니다. 예를들어, 제가 오늘 날씨 어때? 라고 물어보면, 날씨를 묻는 "의도"를 파악 한 후에, "오늘"이라는 entity를 extract해올 수 있어야 합니다. 

flow: 대화의 흐름입니다. 대화가 어떻게 흘러갈지는 각 비지니스마다 매우 다를 수 있습니다. 예를들어 2개의 레스토랑에서 챗봇을 도입했습니다. A라는 레스토랑은 체인이 많은 대형 체인이며, B레스토랑은 원테이블 레스토랑입니다. 그렇다면, 사용자가 "예약을 하고 싶습니다" 라는 말에 A의 챗봇은 "어느 지점이요?", "몇명인가요?", "룸 혹은 홀 자리가있습니다. 어떤자리를 원하시는가요?" 등의 질문이 들어가야 하지만 B는 바로 "몇명인가요?"로 이어지게 됩니다. 때문에, 대화의 흐름은 각 비지니스에서 디자인하시게 될 대화의 흐름입니다. 


다음 포스트에서는 Gartner, Forrester, Ovum에서 인정한, Mobile Platform의 리더인 오라클의 챗봇 플랫폼의 기능을 설명드리겠습니다. 

![Screen Shot 2018-07-17 at 6.23.10 P](https://oracloud-kr-teamrepo.github.io/2018/chatbot_image_repo/15349031844600/Screen%20Shot%202018-07-17%20at%206.23.10%20PM.png)


### - 오라클 챗봇 테스트 계정? 
오라클 챗봇을 만들 수 있는 Intelligent Bots를 체험 계정은 아래에서 신청 하실 수 있습니다. 문제가 있으시면 오라클 영업대표님께 문의부탁드립니다. 
https://cloud.oracle.com/tryit?source=EMMK170607P00126:pm:mz:::RC_WWMK171211P00045:OMag

### - 오라클 챗봇에 대해 배워보기? 
개발자 이신가요? 직접 코드를 채워가면서 챗봇에 기능 추가하는 workshop입니다. 
현업에 계신가요? 만들어져 있는 코드를 넣어보며 직접 테스트 해보실 수 있는 workshop입니다. 


