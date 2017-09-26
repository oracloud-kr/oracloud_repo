+++
date = "2017-09-25T21:28:14+09:00"
description = "blog.oracle.com의 포스트를 번역한 문서입니다. 최근에 오라클이 CNCF에 가입한 영향을 설명하는 글입니다.  Docker와 Kubernetes와 관련된 내용을 포함합니다."
title = "[번역]오라클의 CNCF(Cloud Native Computing Foundation) 가입 의미"   
thumbnailInList = "https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/a3e7d69b-5a30-44b2-af3c-3f7ef2882cd6/File/42216d4b32031c796d6e64ea0010942e/3336189.jpg"
thumbnailInPost = "https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/a3e7d69b-5a30-44b2-af3c-3f7ef2882cd6/File/42216d4b32031c796d6e64ea0010942e/3336189.jpg"
tags = ["docker", "cncf", "kubernetes", "더커", "orchestration", "oracle"]
categories = ["Oracle Cloud"]
author = "taewan.kim"
language = ""  
+++

blog.oracle.com의 "Modern Developer" 부문에 올라온 포스트를 번역한 문서입니다. 최근에 오라클은 리눅스 재단의 하부 기구인 CNCF(Cloud Native Computing Foundation)에 프리미엄 멤버로 가입했습니다. 이 기구에 오라클이 가입한 목적은 Kubernetes를 오라클 클라우드에서 지원하기 위함입니다. "[What Does Oracle’s Embrace of CNCF Mean for Developers?](https://blogs.oracle.com/what-does-oracle%E2%80%99s-embrace-of-cncf-mean-for-cloud-developers)"는 CNCF에 오라클이 가입한 것이 어떤 의미가 있는지 설명하는 포스트입니다. 원본 정보는 다음과 같습니다.

- 출처: https://blog.oracle.com, MODERN DEVELOPER
- 원문: https://blogs.oracle.com/what-does-oracle’s-embrace-of-cncf-mean-for-cloud-developers
- 발표일시: 2017. 09. 18
- 작성자: Alexa Morales

----

더커(Docker) 현상에 힘입어, 클라우드에 소프트웨어 애플리케이션을 배포하고 작은 서비스를 패키징하는 가장 적합한 기술로 리눅스 컨테이너가 부각되고 있습니다. 사실 리눅스 컨테이너는 현재 주류 기술로 자리를 잡은 상태입니다. 최근에 개발자들은 컨테이너화 된 애플리케이션을 오케스트레이션하는 방법 및 여러 클라우드에 배포된 컨테이너을 관리하는 방법 같은 문제를 고민하고 있으며, 서버리스 컴퓨팅(Serverless Computing)으로 이런 모든 문제를 해결할 수 있는지에 대한 의문을 갖고 있습니다.

개발자에게 반가운 소식은 이런 문제 해결에 엄청난 에너지가 집중되고 있다는 것입니다. 2017년 09월 13일에 Oracle은 리눅스 재단 산하의 Cloud Native Computing Foundation(CNCF)에 프리미엄 멤버로 합류했습니다. 이 재단은 컨테이너로 패키지된 마이크로서비스 지향 컴퓨팅을 엔터프라이즈 규모에 적용하는 것을 핵심 주제로 운영되는 곳입니다. 컨테이너 오케스트레이션 툴인 Kubernetes가 CNCF의 핵심 기술입니다. 지난주 오라클은 CNCF에 프리미엄 멤버로 가입하면서, Oracle Linux에 Kubernetes를 공개했고, Kubernetes 프로젝트에 전담 엔지니어를 할당했습니다. [Smith](https://blogs.oracle.com/developers/the-microcontainer-manifesto)[^1], [CrachCart](https://blogs.oracle.com/developers/hardcore-container-debugging)[^2] 및 Oracle Cloud Infrastructure에 Kubernetes를 설치하는 [테라폼 인스톨러](https://github.com/oracle/terraform-kubernetes-installer)[^3]와 같은 툴을 오픈소스로 공개했습니다.

[^1]:[역자주]Smith 프로젝트 위치는 https://github.com/oracle/smith 입니다. 마이크로컨테이너 빌더입니다.
[^2]:[역자주]Oracle Cloud Infrastructure에 Kubernetes를 설치하는 Terraform 파일입니다. 프로젝트 위치: https://github.com/oracle/terraform-kubernetes-installer

시스템을 클라우드에 이전하고는 있지만, 특정 벤더에 종속되는 것을 경계하는 클라우드 네이티브 개발자에게 이러한 업계의 움직임은 매우 반가운 소식입니다. Oracle Container Native Group 개발 부문 부사장인 Bob Quillin은 "앞으로 Kubernetes가 여러분이 사용할 클라우드를 선택하는 기능을 제공한다면, 여러분들은 Kubernetes를 통해서 최적의 환경을 선택하는 최상의 유연성을 갖게될 것[^3]"이라고 말했습니다. 또한, Bob Quillin은 "앞으로 점차 더 많은 고객들이 복수 클라우드 전략을 구사할 것"이라고 강조했습니다.

[^3]:[역자주]Kubernetes의 궁극적인 목표는 복수의 클라우드에 걸처 Docker 클러스터를 구성하고 관리하는 것입니다. Kubernetes를 통해서 어떤 클라우드에 컨테이너를 배포할 지 결정하는 미래 기능에 대한 설명입니다.

### 많아도 너무 많은 마이크로서비스

현장에서 마이크로서비스에 대한 핵심 이슈는 관리해야 할 서비스가 너무 많다는 것입니다. 이렇게 많은 서비스는 스케일 확장과 축소 과정에서 나타나고 사라지는 "__일시성__"의 특징을 갖습니다. Quillin은 "자동화, 오케스트레이션 및 관리 기능을 제공하는 Kubernetes 같은 도구 없이는 엔터프라이즈에서 마이크로서비스를 적용할 수 없음"을 강조했습니다.

Quillin은 Kubernetes를 중심으로한 CNCF의 프로젝트들이 클라우드 네이티브 앱, DevOps 스타일의 지속적 배포(Continuous Delivery) 및 하이브리드 워크로드의 유용함을 증명할 것이라고 예견했습니다.

Quillin은 "CNCF는 개방적이고 클라우드 중립적이며 표준 기반 접근 방식의 개념을 중심으로 만들어졌습니다. 이것이 오라클이 CNCF에 가입한 가장 큰 이유"라고 말했습니다. 또한 Quillin은 이미 CNCF는 업계에서 성공한 오픈소스 컴포넌트의 허브로 자리 잡았고, CNCF는 각 프로젝트를 지원하는 동시에 각 프로젝트의 독립적인 운영을 가능하게 한다고 강조했습니다.

Quillin은 빠르게 변화하는 클라우드 업계를 잘 이해하고 있습니다. Quillin은 2014년에 StackEngine을 설립했고, 이 컨테이너 회사는 2015년에 오라클에 합병되었습니다. 오스틴을 근거지로하는 그의 팀은 계속 개발을 진행해 왔고, 2016년에 Oracle Container Cloud Service를 출시하였습니다. 오라클과 CNCF는 복수 클라우드 위에서 엔터프라이즈 규모의 컨테이너화 된 애플리케이션을 관리하는 Kubernetes와 관련 툴을 지원한다는 목표를 공유하고 있습니다.

Quillin은 "오라클의 엔터프라이즈급 네트워크와 보안에 대한 경험과 베어 메탈 클라우드 성능의 핵심 역량을 근간으로, Kubernetes 지원과 이 기술의 엔터프라이즈 클라우드 적용은 오라클의 고객에게 중요하게 받아들여 질 것 입니다."라고 말했습니다.


### CNCF도 오픈스택과 같은 실수?

오픈 스택은 오픈소스 기반 프라이빗 클라우드와 퍼블릭 클라우드에 많은 노력을 했지만, 현재 현장에서 채택률이 저조한 상태입니다. CNCF는 오픈스택 (Open Stack)의 운명을 피할 수 있을까요? CNCF의 방향성은 커뮤니티에 의해 형성되는 반면, 오픈 스택은 공급 업체가 주도적으로 정의한다고 Quillin은 설명면서 "오픈스택은 엔터프라이즈에서 적용하기에 어렵다고 알려진 여러 컴포넌트를 상호 연결하여 사용하는 복잡한 스택을 갖고 있다."라고 말했습니다.

오라클의 여러 엔지니어링 팀이 Kubernetes에 전담 배치되었으며, 특히 보안, 네트워킹 및 Federation에 집중적으로 배치되었습니다. 대표적으로 예전에 Node.js 프로젝트 리더였던 TJ Fontaine은 Kubernetes의 오라클의 전담 컨트리뷰터로 투입되었고, Jon Mittelhauser (컨테이너 네이티브 엔지니어링의 부사장)는 CNCF의 집행 위원회(governing board)에 합류했습니다.

Quillin는 CNCF에는 Kubernetes 말고도 많은 프로젝트가 있으며, 오라클은 CNCF 산하의 다수의 유망한 프로젝트를 사용하고 있다고 강조합니다[^4]. 오라클은 모니터링 용도로 Prometheus, 분산 코드 추적기인 Open Tracing, 원격 프로시저 호출을위한 gRPC 및 Open Container Initiative(OCI)를 포함하여 다수의 프로젝트를 사용하고 있습니다. "Kubernetes가 CNCF의 주인공이지만, 컨테이너를 운용하기 위해서는, 컨테이너 추적, 모니터링, 디버그 및 제어도 필요하다."라고 그는 설명합니다.

[^4]:[역자주]벤더가 오픈소스를 사용한다는 것은 오픈소스에 기여한다는 중의적인 표현입니다. 오라클은 위에서 언급한 프로젝트에 참여하고 있습니다.

### Serverless에 보안, DevOps

서버리스(Serverless) 트랜드는 Kubernetes, 더 나아가 컨테이너에 어떤 의미를 가질까요? Quillin은 서버리스(Serverless)와 컨테이너 기술은 상호 배타적인 관계가 아니라 상호 보완적인 관계라고 설명하고, 앞으로 CNCF는 복수의 클라우드에 걸쳐 서버리스 환경을 구성하는 방법을 다룰 것이라고 예견했습니다.

"Serverless(서버리스)는 여러분들을 위해서 관리되는 인프라스트럭처를 사용합니다. 따라서 여러분은 개발자 역할에 더 많은 역량을 집중하할 수 있습니다.  Amazon의 Lambda는 주목할만한 제품이지만 AWS에서만 실행되는 폐쇄형 솔루션입니다."라고 Quillin은 말합니다. 또한 "여러 클라우드를 함께 사용하거나  또는 On-premise에서 사용할 수 없습니다. 우리는 업계와 협력하여 개방적이고 클라우드 중립적인 서버리스 기술을 개발하게 되어 매우 기쁩니다. CNCF는 이러한 노력의 선두 주자가 될 것"이라고 말했습니다.

클라우드를 근간으로 하는 응용 프로그램을 지원하는 도구가 점점 더 많이 등장함에 따라 개발자는 마이크로서비스 패턴을 수용하고, 점차 기존 모노리스 애플리케이션에 마이크로서비스 패턴을 적용할 것입니다. 이 아키텍처 패턴은 DevOps의 핵심인 지속적인 통합 및 지속적 배포를 전제로 합니다. Quillin은 오늘날 Kubernetes와 Docker와 같은 도구가 이러한 문화를 선도하고 있다고 말했습니다.

"컨테이너 기술은 개발자를 프로덕션에 직접 연결하는 특성을 갖습니다. 결과적으로 컨테이너 기술은 DevOps를 위한 킬러 애플리케이션입니다."라고 Quillin은 설명하고, "컨테이너는 개발자 컴퓨터에서 QA 서버를 거쳐 운영환경으로 이동하는데 적합한 최적의 산출물[^5]입니다. 컨테이너가 진정한 핵심입니다."라고 강조했습니다.

[^5]:[역자주]여기서 컨테이너는 Docker 컨테이너의 실질적인 배포 단위를 의미합니다.
