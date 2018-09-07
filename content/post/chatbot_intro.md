+++
date = "2018-09-01T21:59:47+09:00"
description = "오라클 챗봇 기능을 둘러보는 글입니다. "
title = "오라클 챗봇 (Oracle Intelligent Bots) 기능 둘러보기"
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2018/chatbot_image_repo/logo/intro.jpg"
thumbnailInPost = "https://oracloud-kr-teamrepo.github.io/2018/chatbot_image_repo/logo/intrologo.jpg"
tags = ["chatbot", "OMCe", "Oracle Mobile", "챗봇", "채터봇", "인공지능", "인공지능봇", "오라클 챗봇", "오라클 chatbot", "oracle chatbot"]
categories = ["Chatbot"]
author = "sung.hye.jeon"
language = ""  
+++


이전 포스트에서는 챗봇이 무엇인지 살펴 보셨습니다. 이 포스트에서는 이 후의 포스트에서는 오라클 챗봇을 직접 만들고 여러 서비스에 연결해서 실생활에 사용 가능한 챗봇을 만들어 보겠습니다. 

- 챗봇이란?
전 포스트 참고

- 오라클 챗봇에 대해 배워보기? 
개발자 이신가요? 직접 코드를 채워가면서 챗봇에 기능 추가하는 workshop입니다. 
현업에 계신가요? 만들어져 있는 코드를 넣어보며 직접 테스트 해보실 수 있는 workshop입니다. 

- 오라클 챗봇 테스트 계정? 
오라클 챗봇을 만들 수 있는 Intelligent Bots를 체험 계정은 아래에서 신청 하실 수 있습니다. 문제가 있으시면 오라클 영업대표님께 문의부탁드립니다. 
https://cloud.oracle.com/home


- 오라클 챗봇 장점? 
![Screen Shot 2018-08-22 at 3.04.49 P](https://oracloud-kr-teamrepo.github.io/2018/chatbot_image_repo/15347473059168/Screen%20Shot%202018-08-22%20at%203.04.49%20PM.png)

시장에 늦게 진입한 만큼 기존 고객들의 챗봇에 대해 걱정하는 부분 또는 불편함을 인지해 성능을 보완하고 타사에는 없는 추가 개선한 기능들을 있습니다.
1. 새로운기능
    - Q&A
    - Instant App 
    - Resource Bundle
2. 기업수준의 안전하고 간편한 연동

- 오라클 챗봇 한국어 처리 되나요?
네 2018년 6월부터 지원되고 있습니다. 

- 오라클 챗봇 테스트 계정? 
오라클 챗봇을 만들 수 있는 Intelligent Bots를 체험 계정은 아래에서 신청 하실 수 있습니다. 
https://cloud.oracle.com/tryit?source=EMMK170607P00126:pm:mz:::RC_WWMK171211P00045:OMag

- 주요 기능 상세보기 
여러가지 기능이 포함되어있지만, 주요기능에 대해 간략하게 보여드리겠습니다.

![Screen Shot 2018-08-21 at 5.21.50 P](https://oracloud-kr-teamrepo.github.io/2018/chatbot_image_repo/15347473059168/Screen%20Shot%202018-08-21%20at%205.21.50%20PM.png)

![Screen Shot 2018-08-21 at 5.23.01 P](https://oracloud-kr-teamrepo.github.io/2018/chatbot_image_repo/15347473059168/Screen%20Shot%202018-08-21%20at%205.23.01%20PM.png)


1. Intent & Entity 

Training Data를 넣어서 Intent를 학습시킨 후에, Test를 해보 실 수 있습니다. 몇퍼센트의 확률로 intent resolving이 되는지 바로바로 확인하실 수 있습니다. 

성능을 확인하기 위해 일부로 띄어쓰기로 하지 않고 테스트 해봤습니다. 잘되는 것을 확인 하실 수 있습니다.

![Screen Shot 2018-08-21 at 5.34.00 P](https://oracloud-kr-teamrepo.github.io/2018/chatbot_image_repo/15347473059168/Screen%20Shot%202018-08-21%20at%205.34.00%20PM.png)

챗봇을 이용하다가 전화번호를 남겨놓고 싶으실때도 있으실텐데요. 미리 제공되고 있는 system entity를 이용해서 전화번호가 잘 파싱되는것을 확인 하실 수 있습니다. 

![Screen Shot 2018-08-21 at 5.39.10 P](https://oracloud-kr-teamrepo.github.io/2018/chatbot_image_repo/15347473059168/Screen%20Shot%202018-08-21%20at%205.39.10%20PM.png)


2. Q&A
수만건이 있는 단답형 FAQ가 있다고 가정합니다. 질문의 양이 많기 때문에 좋은 퀄리티의 학습데이터로 봇을 학습시키는 것이 중요할 텐데, 이 과정에서  많은 공수가 들어갈 일이 될 수 있습니다. 이런 경우에는, FAQ를 봇에게 쏟아 넣으면, 봇이 각각 질문의 중요 말뭉치를 추출해그 중요 말뭉치들을 이용한 유사성 검색으로 빠르고 정확한 엔진을 구성할 수 있습니다. 
![Screen Shot 2018-08-22 at 3.23.11 P](https://oracloud-kr-teamrepo.github.io/2018/chatbot_image_repo/15347473059168/Screen%20Shot%202018-08-22%20at%203.23.11%20PM.png)

3. Channel 
만든 봇을 여러가지 채널과 연결 할 수 있습니다. Public Messenger 뿐만아니라 제공 되는 SDK를 이용해서 간편하게 미리 만들어둔 앱이나 웹에도 추가 할 수 있습니다. 

4. Instant App 
챗봇은 대화형 인터페이스를 이용합니다. 이 대화형 인터페이스는 간단한 질의 응답이나 업무엔 적합하지만, 한번에 많은 정보를 받아오거나 보여주거나 혹은 텍스트 이외의 정보를 주고받을 때에는 적합하지 않습니다. 그래서 원하는 형태의 form을 간단히 생성해서 봇과의 대화중에 보여줄 수 있습니다. 수많은 ui component가 제공되며, 그 ui component에서 원하시는 시점에 원하는 이벤트를 Ui 화면에서 간단히 생성 시킬수 있습니다. 

원하는 형식을 간단히 생성하는것 이상으로, 더 중요한것은 봇과의 대화와 형식에 넣은 정보간의 오가는 것이 seamless하게 이뤄질 수 있습니다. 유저가 form에 원하는 입력을 모두 마치고, bot과의 대화로 돌아왔을때 form에 주입한 결과를 다시 봇과의 대화에서 받아오는 것. 또한, 그반대의 경우에도 자유롭게 data가 seamless 오고 갈수 있습니다. 

![Screen Shot 2018-08-22 at 3.24.22 P](https://oracloud-kr-teamrepo.github.io/2018/chatbot_image_repo/15347473059168/Screen%20Shot%202018-08-22%20at%203.24.22%20PM.png)

5. 상담원 연결 
봇이 이해못하거나, 민감한 질문이 들어왔을때 뒷단에 상담원이 기다리고 있다가 대화에 들어올 수 있습니다. 봇과 상담원 간의 트랜지션이 seamless하게 이뤄지기 때문에, 사용자 입장에서는 했던 질문을 또 할 필요가 없습니다.

6. Monitoring & Analytics 

챗봇을 만드셨습니다. 하지만, 유저는 챗봇만을 통해서 비지니스와 인터랙트하는 것은 아닐것입니다. 유저는 웹사이트, 앱 등으로도 비지니스와 연결 될 수 있습니다. 그렇다면, 유저가 원하는 것이 무엇인지 정확하게 파악하기위해서는 전 채널을 아우르는 Monitoring 및 Analytics가 있어야합니다. 모든 채널에 대해 분석 및 business impact까지 이어질 수 있는 효과적인 툴을 제공 합니다. 

![Screen Shot 2018-08-22 at 3.43.47 P](https://oracloud-kr-teamrepo.github.io/2018/chatbot_image_repo/15347473059168/Screen%20Shot%202018-08-22%20at%203.43.47%20PM.png)



