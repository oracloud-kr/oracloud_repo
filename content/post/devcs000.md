+++
date = "2018-08-03T13:58:54+09:00"
description = "2018년 Developer Cloud Service 신기능 소개 입니다."
title = "Oracle Developer Cloud - 새로운 CI (Continuous Integration) 신기능 자세히 알아보기"
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2018/05/devcs/dev01.png"
thumbnailInPost = "https://oracloud-kr-teamrepo.github.io/2018/05/devcs/dev02.png"
tags = ["ORACLE", "Oracle Cloud", "Developer Cloud", "CI", "CD", "devops", "오라클 클라우드"]
categories = ["Oracle-Cloud"]
author = "mee-nam.lee"
language = ""  
+++

Oracle Developer Cloud Service는 자동화된 CI/CD를 위한 빌드 파이프라인 관리를 제공하는 클라우드 서비스입니다. 
이 문서에서는 [Oracle Developer Cloud](https://cloud.oracle.com/en_US/developer-service)의 2018년 4월과 5월 릴리즈에서 추가된 새로운 기능들에 대해서 설명하려고 합니다.

## Oracle Developer Cloud - 빌드 파이프라인 정의 및 가시화
 이 버전의 Developer Cloud Service에서는 구성된 빌드들을 조합하여 일련의 파이프라인을 구성하는 기능이 추가되었습니다. 각 빌드 작업들은 작업의 연관성과 흐름을 가지며 연속적으로 수행되도록 구성될 수 있습니다.
 일련의 빌드 작업의 수행에서 선행 빌드 작업의 성공 여부에 따라 후행 작업의 시작 유무를 조건에 다라 결정 지을 수도 있습니다. 

- 그림 1. 빌드 파이프라인 
 ![](https://oracloud-kr-teamrepo.github.io/2018/05/devcs/dev03_pipeline.jpg)

## 신규 Build Executor
기존에 제공되던 빌더 이외에 신규 릴리즈에서 Docker, Kubectl, Terraform 등의 빌더가 추가되었습니다. 이런 기능들을 통해서 단순히 애플리케이션 소스만을 빌드하는 것에 그치지 않고, 빌드된 애플리케이션이 포함된 컨테이너 이미지를 빌드하고 이를 Kubernetes Cluster로 배포하거나 Terraform을 이용해 다른 컴퓨트 환경으로 배포하거나 OCIcli를 통해 오라클 클라우드 환경으로 프로비저닝 하는 과정을 연결하는 빌드 파이프라인 구성이 용이하게 되었습니다.  

- 그림 2. 빌드 구성시 추가할 Executor 선택 메뉴
![](https://oracloud-kr-teamrepo.github.io/2018/05/devcs/build_executor.jpg)

## 독립된 빌드 환경 - Compute VM
새롭게 릴리즈된 Developer Cloud Service는 이전 버전처럼 공유되는 환경을 사용한 서비스가 아닌, 별도의 독립된 클라우드 환경을 가진 인스턴스로 생성됩니다. 따라서 빌드 환경도 독립된 컴퓨트 VM을 사용하는 형식으로 변경되었습니다. 제공되는 소프트웨어 탬플릿들 중 원하는 소프트웨어들만 선택하여 맞춤형 빌드 환경을 구성할 수 있게 되었습니다. 이 빌드 VM은 빌드 구성에 따라 다른 VM을 사용할 수 있도록 여러개를 생성할 수 있으며, 빌드 시에만 구동되고 빌드가 완료되면 자동으로 정지됩니다.

- 그림 3. 빌드 VM 구성에 사용되는 소프트웨어 탬플릿 
 ![](https://oracloud-kr-teamrepo.github.io/2018/05/devcs/Software_template.jpg)

 ![](https://oracloud-kr-teamrepo.github.io/2018/05/devcs/BuildVM.jpg)

## Software Configuration
 ![](https://oracloud-kr-teamrepo.github.io/2018/05/devcs/software_config.jpg)

## 새로운 Agile Report
 ![](https://oracloud-kr-teamrepo.github.io/2018/05/devcs/agile_report.png)

 So the concept of creating build jobs remains the same. We have Pipeline in addition to the build jobs that you can create.

아래 스크린 샷은 [Oracle Developer Cloud](https://cloud.oracle.com/en_US/developer-service)의 새로운 'Build'탭에 대한 사용자 인터페이스를 보여줍니다. 이를 한눈에 보면 'Jobs' 탭과 함께 'Pipelines' 이라는 새로운 탭이 추가된 것을 알 수 있습니다. 따라서 빌드 작업을 만드는 개념은 동일하게 유지가 되고 추가적으로 빌드 작업 생성에 파이프라인을 이용하게 되었습니다.

{{< img src="https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/d5e9c9e1-977b-417a-a349-ce8a7f4052af/Image/be6d174a4819890aaa10243062bca0f8/1.png"
title="그림 2"
caption="Oracle Deveoper Cloud Servcie에서 Build 화면" >}}

빌드 작업의 생성에도 변경이 있습니다. 빌드 탭에서 '+ 새 작업'단추를 클릭하여 빌드 작업을 만들려고하면 새 빌드 작업을 만들 수있는 대화 상자가 나타납니다. 첫 번째 스크린 샷은 작업 이름을 제공하고 프리 스타일 작업을 만들거나 기존 빌드 작업을 복사 할 수있는 이전과 같은 'New Job'대화 상자를 보여줍니다.

두 번째 스크린 샷은 Oracle Developer Cloud에서 제공되는 최신 'New Job'대화 상자를 보여줍니다. 작업 이름, 설명 (이전에 빌드 구성 인터페이스에서 볼 수 있었던 것), 새 작업 만들기(create new) / 기존 작업에서 복사(copy existing job) 옵션, '병합 요청(use for merge request)' 선택 체크 박스와 가장 눈에 띄는 추가 사항인 소프트웨어 템플릿을 선택하는 드롭 다운 리스트가 있습니다.

{{< img src="https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/d5e9c9e1-977b-417a-a349-ce8a7f4052af/Image/4ba170813ec3973963d389a96dff41f1/2.png"
title="그림 3"
caption="Oracle Deveoper Cloud Servcie의 이전 New Job 대화 상자" >}}

{{< img src="https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/d5e9c9e1-977b-417a-a349-ce8a7f4052af/Image/4ba170813ec3973963d389a96dff41f1/2.png"
title="그림 4"
caption="Oracle Deveoper Cloud Servcie의 최신 New Job 대화 상자" >}}


# What these additional fields in the ‘New Job’ dialog mean?

### heading-3

## heading-2

### heading-3

### heading-3
