+++
date = "2017-09-28T16:50:36+09:00"
description = "git서버에서 fork받은 프로젝트에 원본 프로젝트의 추가 변경을 반영하는 방법을 소개합니다. "
title = "Fork된 Git 레파지토리에 원본 레파지토리 업데이트"
thumbnailInList = "https://oracloud-img-repo.github.io/2017/09/demo/logo.jpg"
thumbnailInPost = "https://oracloud-img-repo.github.io/2017/09/demo/list.jpg"
tags = ["git", "fork", "github"]
categories = ["Tech Tip"]
author = "taewan.kim"
+++

Github을 사용할 때 fork 레파지토리를 만들어 코드 변경 작업을 수행하는 것이 일반적입니다.
Fork 된 레파지토리가 너무 오래되거나, 원본 레파지토리 병합 요청("__pull request__")이 반려되는 경우,
Fork 된 레파지토리에 원본 레파지토리 변경 사항을 적용해야 하는 경우가 있습니다.
Fork 된 레파지토리에 원본 레파지토리 변경 사항을 반영하는 방법을 정리합니다.

## github에서 Fork를 왜하는가?

github 등 git 서비스에서 commit 권한이 없는 레파지토리를 테스트하거나 대상 레파지토리을 변경하는 목적으로 fork 기능을 사용합니다.
다른 계정(Account) 혹은 조직(Organization)의 레파지토리를 fork 하면 해당 레파지토리는 사용자의 계정으로 복제됩니다. <그림 1참조>

{{< img src="https://oracloud-img-repo.github.io/2017/09/demo/img010.png"
title="그림 1"
caption="github 레파지토리를 Fork(분기)" >}}


## Fork 레파지토리에 원본 레파지토리 변경 반영

Fork 된 레파지토리는  PC 혹은  노트북에 복제하고, 코드 변경의 버전을 관리합니다.

{{< img src="https://oracloud-img-repo.github.io/2017/09/demo/img020.png"
title="그림 2"
caption="git 로컬 레파지토리 복제(clone)" >}}

Fork 시점이후 fork 된 레파지토리에는 원본 레파지토리 변경이 반영되지 않습니다.
Fork 된 레파지토리에 원본 레파지토리의 변경 사항을 반영하는 절차는 다음과 같습니다.

1. 사용자 계정에 fork 되고 로컬 컴퓨터에 복제된 git 레파지토리에 원본 레파지토리의 remote repository 등록
  - <그림 3 참조>
  - 원격 원본 레파지토리 명: upstream (신규 추가)
  - fork 된 원격 레파지토리명: origin (git clone시 자동 생성됨)
1. 로컬 git 레파지토리에 원본 레파지토리의 변경 사항 반영(git pull)
1. 로컬 git 레파지토리의 변경 사항을  fork된 repository에 반영 (git pull): <그림 4 참조>


{{< img src="https://oracloud-img-repo.github.io/2017/09/demo/img030.png"
title="그림 3"
caption="원본 github 레파지토리 등록: upstream" >}}

<그림 3>과 같이 로컬 git 레파지토리에 원본 레파지토리를 remote repository로 등록합니다.
원본 레파지토리 명으로 "__upstream__"을 사용하는 것이 관례입니다.

{{< img src="https://oracloud-img-repo.github.io/2017/09/demo/img040.png"
title="그림 4"
caption="원본 레파지토리의 변경사항 fork 레파지토리에 반영" >}}

## 데모

다음 github 레파지토리를 데모로 사용합니다.

|레파지토리 유형|레파지토리 URL|{GIT_URL}|
|---|---|---|
|원본 레파지토리|https://github.com/oracloud-kr/oracloud_repo |git@github.com:oracloud-kr/oracloud_repo.git |
|Fork 레파지토리|https://github.com/taewanme/oracloud_repo |git@github.com:taewanme/oracloud_repo.git |

### Fork 된 레파지토리 로컬 복제

<그림 2>와 같이 fork 된 레파지토리를 로컬에 복제하는 명령은 다음과 같습니다.

- git 복제 명령은 다음과 같습니다.

```
git clone {GIT_URL}
```

복제된 로컬 복제 예는 다음과 같습니다.

```
> git clone git@github.com:taewanme/oracloud_repo.git
Cloning into 'oracloud_repo'...
remote: Counting objects: 1524, done.
remote: Total 1524 (delta 0), reused 0 (delta 0), pack-reused 1524
Receiving objects: 100% (1524/1524), 2.69 MiB | 879.00 KiB/s, done.
Resolving deltas: 100% (899/899), done.
> cd oracloud_repo
~/oracloud_repo > ls
LICENSE       archetypes       static
README.md     config.toml      content
~/oracloud_repo >
```

### 로컬 git 레파지토리에 원격 레파지토리 등록

원본 레파지토리를 로컬 git 레파지토리의 원격 등록 명령은 다음과 같습니다.

```
git remote add {REPOSITORY_NAME} {GIT_URL}
```

<그림 3>과 같이 로컬 git 레파지토리에 원본 레파지토리를 원격 레파지토리로 등록하는 데모는 다음과 같습니다.

```
~/oracloud_repo > git remote add upstream git@github.com:oracloud-kr/oracloud_repo.git
~/oracloud_repo > git remote -v
origin	git@github.com:taewanme/oracloud_repo.git (fetch)
origin	git@github.com:taewanme/oracloud_repo.git (push)
upstream	git@github.com:oracloud-kr/oracloud_repo.git (fetch)
upstream	git@github.com:oracloud-kr/oracloud_repo.git (push)
~/oracloud_repo >
```

### 원본 git remote로부터 로컬 레파지토리 업데이트

<그림 4>와 같이 git pull 명령을 이용하여 원본 git 레파지토리로 부터 로컬 레파지토리로 변경 사항을 받을 수 있습니다.
명령은 다음과 같습니다.

```
git pull {REMOTE_ORIGIN_REPO_NAME} {BRANCH_NAME_OF_LOCAL_REPO}
```

<그림 4>와 로컬 git 레파지토리에 원본 레파지토리의 변경 사항을 반영하는 데모는 다음과 같습니다.

```
~/oracloud_repo > git pull upstream master
remote: Counting objects: 806, done.
remote: Compressing objects: 100% (74/74), done.
remote: Total 806 (delta 446), reused 473 (delta 426), pack-reused 300
Receiving objects: 100% (806/806), 254.99 KiB | 310.00 KiB/s, done.
Resolving deltas: 100% (568/568), completed with 53 local objects.
From github.com:oracloud-kr/oracloud_repo
 * branch            master     -> FETCH_HEAD
 * [new branch]      master     -> upstream/master
Updating 23da8f3..5b810be
Fast-forward
 config.toml                                              |    3 +-
 config.toml.production                                   |    5 +-
 content/opc-iaas.md                                      |   14 +-
 themes/mainroad/static/css/minsu.css                     |   20 +-
 themes/mainroad/static/css/style.css                     |   88 ++++---
 themes/mainroad/static/css/style.css.org                 | 1120 ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
 76 files changed, 4670 insertions(+), 218 deletions(-)
 create mode 100644 content/post/DVCS001.md
 create mode 100644 content/post/DVCS002.md
 create mode 100644 themes/mainroad/layouts/partials/widgets/map.html
 create mode 100644 themes/mainroad/static/css/style.css.org
~/oracloud_repo >
```

### 원본 레파지토리 변경 사항 Fork 된 레파지토리에 반영

<그림 4>와 같이 git push 명령을 이용하여 로컬 git 레파지토리에 적용한 원본 레파지토리의 변경 사항을 fork 된 레파지토리에 반영합니다.
명령은 다음과 같습니다.

```
git push {REMOTE_FORKED_REPO_NAME} {BRANCH_NAME_OF_LOCAL_REPO}
```

<그림 4>와 로컬 git 레파지토리에 원본 레파지토리의 변경 사항을 반영하는 데모는 다음과 같습니다.

```
~/oracloud_repo > git push origin master
Counting objects: 806, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (245/245), done.
Writing objects: 100% (806/806), 252.73 KiB | 0 bytes/s, done.
Total 806 (delta 568), reused 774 (delta 544)
remote: Resolving deltas: 100% (568/568), completed with 39 local objects.
To github.com:taewanme/oracloud_repo.git
  23da8f3..5b810be  master -> master
~/oracloud_repo >
```

## 요약

Fork 된 레타지토리는 생성 지점을 기준으로 이후에 발생한 원본 레파지토리의 변경이 반영되지는 않습니다.
PC 혹은 노트북의 로컬 git 레파지토리를 경유하여 Fork된 레파지토리에 원본의 변경 사항을 반영할 수 있습니다.
PC 혹은 노트북의 로컬 git 레파지토리에 "upstream"이라는 이름으로 원본 레파지토리를 등록하고 git pull과 git push 명령으로
<그림 4>와 같은 절차로 원본의 변경 사항을 fork 레파지토리에 반영할 수 있습니다.
