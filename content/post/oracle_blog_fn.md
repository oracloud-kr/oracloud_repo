+++
date = "2017-10-04T21:28:14+09:00"
description = "blog.oracle.com의 포스트를 번역한 문서입니다. 최근에 오라클이 CNCF에 가입한 영향을 설명하는 글입니다.  Docker와 Kubernetes와 관련된 내용을 포함합니다."
title = "[번역]Fn–An Open Source Serverless Functions Platform"
thumbnailInList = "https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/0dd8491b-d053-4912-b0b8-0ff4ca10b987/File/5b7362d3dfbfe6cc09cd68f830492c9d/3238546967_104858abee_b.jpg"
thumbnailInPost = "https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/a3e7d69b-5a30-44b2-af3c-3f7ef2882cd6/File/42216d4b32031c796d6e64ea0010942e/3336189.jpg"
tags = ["docker", "serverless", "DevOps", "java"]
categories = ["blogs.oracle.com"]
author = "taewan.kim"
language = ""  
+++

Oracle OpenWorld 2017에서 오라클 클라우드를 강화하는 많은 새로운 키워드가 발표되었습니다. 오라클 클라우드는 최근에 Container 기술을 중점적으로 개발하고 있습니다. 이번 Oracle OpenWorld 2017에서 Serverless Platform으로 Fn을 공개했습니다. Fn은 오라클 클라우드 Serverless 플랫폼의 핵심에 위치할 것으로 예상됩니다. 2017.10.02 오라클 공식 블로그에 올라온 Fn관련 짧은 포스트를 소개합니다.

- 출처: blogs.oracle.com
- 원문: https://blogs.oracle.com/developers/announcing-fn
- 제목: Oracle cloud acquisitions bear fruit, with a new serverless platform and container services
- 문서작성 일시: 2017. 10. 02
- 작성자: Shaun Smith, Director of Product Management

----

새로운 오픈 소스 서버리스 플랫폼 Fn을 발표하게 된 것을 대단히 기쁘게 생각합니다.[^1]

[^1]: (역자주) "cloud agnostic(클라우드에 무관심한)"을 번역에서 제외했습니다. 원문에서는 Fn이 클라우드 전용 소프트웨어가 아니라는 것을 강조하기 위해서 "cloud agnostic" 한 특징을 갖는다는 설명을 사용하고 있습니다.

Fn 프로젝트는 컨테이너 네이티브 서비리스 플랫폼으로 클라우드와 온프라미스(on-premise)[^1] 모두에서 작동할 수 있습니다. Fn 프로젝트 라이센스는 Apache 2.0입니다. Fn은 사용하기 쉽고, 모든 프로그래밍 언어를 지원하며, 확장할 수 있고 효율적입니다.

[^2]: (역자주)온프레미스는 인터넷 네트워크에 연결된 서버팜이나 클라우드 등의 원격 환경에서 사용하는 것이 아니라 건물에서 일하는 직원 또는 단체에서 설치, 실행되는 방식을 의미합니다.

Fn은 정말 쉽다는 점을 강조하고 싶습니다. 몇 분 안에 Fn을 실험하고 시작할 수 있습니다. Fn에 익숙해지면 바로 고급 기능을 사용할 수도 있습니다. 우리가 제공하는 [퀵 스타트](https://github.com/fnproject/fn#quickstart)를 이용하여 몇 분 안에 자신만의 함수를 실행하고 배포 할 수 있습니다.

## History

Fn 프로젝트는 IronFunctions를 만든 팀이 개발을 담당하고 있습니다. 이 팀은 서버리스 기술을 개척하고 6년간 호스팅 서버리스 플랫폼을 운영해왔습니다. Docker 등장 전후로 수천 명의 고객을 대상으로 수십억 개의 컨테이너를 운영하면서, 개발팀은 컨테이너 기능을 대규모로, 특히 Functions-as-a-service 스타일의 운영에 대해 몇 가지 교훈을 얻었습니다.

이 개발팀은 현재 오라클에서 그동안 축적한 기술과 경험을 Fn에 적용하고 있습니다.

## 특징

Fn은 개발과 운영을 지원하는 여러 기능을 포함합니다.

- 명령 줄 도구를 사용하여 기능을 개발, 테스트 및 배포하기 쉬움
- 단일 의존성: 더커 (Docker)
- 고성능 애플리케이션을 위한 기능
- 람다 코드 호환성: Lambda code
- Lambda code compatibility - Lambda 코드를 내려받아 Fn에서 수행 가능
- 다수 주류 언어를 위한 FDK(Function Developer Kit) 제공
- JUnit test framework을 포함한 고급 Java FDK
- Kubernetes, Mesosphere 및 Docker Swarm과 같은 인지도 높은 오케스트레이션 도구로 Fn 배포
- Function에 트래픽 분산 용도로 제작된 스마트 로드 밸런스
- 확장성, 모듈화, 사용자 정의 부가 기능 및 통합 기능

이 프로젝트의 홈페이지는 [fnproject.io](http://fnproject.io/) 입니다. 모든 개발 작업은 github(http://github.com/fnproject/fn )에서 운영 중입니다.

우리 개발팀은 Fn이 최고의 서버리스 플랫폼으로의 성장에 도움이 되는 귀하의 의견과 참여를 환영합니다.
