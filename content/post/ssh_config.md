+++
thumbnailInPost = "https://oracloud-kr-teamrepo.github.io/2017/04/sshkey/post.jpg"
description = "클라우드 서비스에 접근하기 위해서 혹은 github에 인증을 받기 위해서 ssh 보안키를 생성하는 절차가 필요합니다. "
language = "bsh"
tags = ["cloud", "ssh"]
author = "taewan.kim"
categories = ["cloud"]
title = "ssh config 설정 방법"
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2017/03/ssh_config/hellodotsshconfig.png"
date = "2017-03-06T12:07:16+09:00"
+++

하나의 컴퓨터에서 복수의 SSH 키를 사용할 경우 SSH 접속에 사용하는 ssh 명령이 복잡해지는 단점이 있습니다.
이러한 복잡성은 SSH config 파일을 이용하여 해결할 수 있습니다.

이 문서는 SSH 키 파일을 이미 갖고있는 것을 전제로 합니다. 아직 SSH 키를 생성하지 않은 상태라면,
다음 문서를 참조하시기 바랍니다.

- [윈도우, 리눅스, 맥에서 ssh 보안키 생성](/post/ssh_key/)

## ssh 명령 기본 사용법
ssh 명령의 기본 사용법은 다음과 같습니다.

<pre class="prettyprint">
> ssh 사용자ID@서버명
</pre>

다음과 같은 명령을 내릴 때 ssh 명령은 "~/.ssh/id_rsa" 키 파일을 사용하여,
52.79.103.139 서버에 ubuntu 계정으로 로그인을 시도합니다.

<pre class="prettyprint">
> ssh ubuntu@52.79.103.139
Welcome to Ubuntu 14.04.3 LTS (GNU/Linux 3.13.0-74-generic x86_64)

Last login: Mon Feb  1 01:44:13 2016 from 211.210.76.6
ubuntu@ip-172-31-11-175:~$ pwd
/home/ubuntu
ubuntu@ip-172-31-11-175:~$
</pre>

기본 SSH 키(```~/.ssh/id_rsa```)가 아닌 다른 SSH 키 파일을 사용해야 한다면,
 다음과 같이 -i 옵션을 사용해야 합니다.

<pre class="prettyprint">
> ssh ubuntu@52.79.103.139 -i ~/.ssh/aws-seoul.pem
Welcome to Ubuntu 14.04.3 LTS (GNU/Linux 3.13.0-74-generic x86_64)

Last login: Mon Feb  1 02:10:40 2016 from 211.210.76.6
ubuntu@ip-172-31-11-175:~$
</pre>

위 명령은 SSH 키 파일을 직접 지정해야 하므로 사용하기 부담스러운 면이 있습니다.
ssh 명령을 단순하게 사용하는 방법의 하나는 다음과 같은 alias를 사용하는 것입니다.

<pre class="prettyprint">
> alias awsssh="ssh ubuntu@52.79.103.139 -i ~/.ssh/aws-seoul.pem"
> awsssh
Welcome to Ubuntu 14.04.3 LTS (GNU/Linux 3.13.0-74-generic x86_64)

Last login: Mon Feb  1 02:23:47 2016 from 211.210.76.6
ubuntu@ip-172-31-11-175:~$
</pre>

alias를 사용하면 명령을 쉽게 입력할 수 는 있지만 별도로 alias를 관리한다는 부담이 증가합니다.  
또한 ssh 명령은 인증서, 서버 포트, 포트 포워딩 등 다양한 설정 옵션이 있으므로,
다양한 옵션 변경에 효과적으로 대응하기에 어렵다는 단점이 있습니다.

## ~/.ssh/config 설정 파일

여러 ssh 키 파일들을 사용할 때 발생하는 문제는 ```~/.ssh/config``` 파일로 해결할 수 있습니다.
다음은 일반적인 형태의 ```~/.ssh/config``` 파일 구성입니다.

<pre class="prettyprint">
### for aws
Host aws-ubuntu1
    HostName 52.79.103.139
    IdentityFile ~/.ssh/aws-seoul.pem

Host aws-ubuntu2
    HostName 52.79.103.139
    User ubuntu
    IdentityFile ~/.ssh/aws-seoul.pem   

## for git
Host github.com
    User git
    IdentityFile ~/.ssh/id_rsa_XXXXXXXXXXXX
Host bitbucket.org
    User git
    IdentityFile ~/.ssh/id_rsa_@@@@@@@@@@@@
</pre>

위 설정 파일은 2개의 아마존 서버와 github 및 bitbucket 서버 설정으로 구성되어 있습니다.
위 예제에서 아마존 AWS 서버는 다음의 두 가지 방법으로 로그인할 수 있습니다.
SSH config 설정을 사용할 경우,
복수의 인증서를 사용하는 상황에서도 다음과 같이 ssh 명령을 단순하게 유지할 수 있습니다.

<pre class="prettyprint">
> ssh ubuntu@aws-ubuntu1
Welcome to Ubuntu 14.04.3 LTS (GNU/Linux 3.13.0-74-generic x86_64)

Last login: Mon Feb  1 02:37:28 2016 from 211.210.76.6
ubuntu@ip-172-31-11-175:~$
</pre>

아래의 경우 aws-ubuntu2에 로그인 계정명을 등록하였기 때문에 ssh 명령을 더욱 단순하게 사용 가능합니다.

<pre class="prettyprint">
> ssh aws-ubuntu2
Welcome to Ubuntu 14.04.3 LTS (GNU/Linux 3.13.0-74-generic x86_64)

Last login: Mon Feb  1 02:56:27 2016 from 211.210.76.6
ubuntu@ip-172-31-11-175:~$
</pre>

## ~/.ssh/config 권한 설정

SSH 설정 파일은 다른 사용자가 사용할 경우 심각한 보안 문제가 발생할 수 있습니다.
따라서 다음과 같이 오로지 파일 소유권자만이 설정 파일을 읽을 수 있도록 권한을 제한해야 합니다.

<pre class="prettyprint">
chmod 440 ~/.ssh/config
</pre>

## ssh 설정 파일 구성

SSH 설정 파일의 기본 형태는 다음과 같습니다.

<pre class="prettyprint">
Host firsthost
    SSH_OPTION_1 custom_value
    SSH_OPTION_2 custom_value
    SSH_OPTION_3 custom_value

Host secondhost
    ANOTHER_OPTION custom_value

Host *host
    ANOTHER_OPTION custom_value

Host *
    CHANGE_DEFAULT custom_value
</pre>

SSH 설정 파일의 옵션 값은 다음과 같이 3가지 방법으로 설정 할 수 있습니다.
다음 3 가지는 모두 유효한 설정입니다.

<pre class="prettyprint">
Port 4567
Port=4567
Port = 4567
</pre>

### 호스트 맵핑
호스트 맵핑은 Host 속성과 Hostname 속성을 이용하여 실제 호스트 URL에 맵핑을 합니다.

- Host: ssh 명령에 사용하는 이름
- Hostname: Host에 지정된 이름이 매핑되는 실제 호스트 명

<pre class="prettyprint">
Host dev
	HostName dev.taewan.kim
	Uset admin
</pre>

위와 같이 설정된 상태에서 "ssh dev"이 실행된다면 다음과 같은 명령으로 변환되어 실행됩니다.

<pre class="prettyprint">
ssh admin@dev.taewan.kim -i ~/.ssh/id_rsa"
</pre>

### 서브 도메인 등 와일드카드 문자 지정

다음과 같이 와일드카드 문자로 호스트 명을 지정 할 수 있습니다. 이 경우 taewan.kim의 서브 도메인에 모두 적용됩니다.

<pre class="prettyprint">
Host *.taewan.kim
    User taewan
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/taewan.dev
</pre>

## ssh config의 주요 속성

### 일반 속성

- Hostname: 연결될 서버 호스트 명으로 사용됨.
	- 선택적 속성
	- 미 설정시 Host 값이 Hostname으로 사용됨
- User: 네트웍 커넥션에 사용되는 계정 명
- Port: 원격 ssh 데몬이 사용하는 포트, 기본 값=22

### 네트웍 속성

- ServerAliveInterval
	- 서버에 테스트 패킷을 전송하는 주기 설정
	- 초단위 설정
	- 기본 값: 0
- ServerAliveCountMax
	- 서버에 테스트용 패킷을 전송하는 횟수를 설정
	- 서버에 추가적인 데이터 전송 없이 ServerAliveInterval의 최대 임계 값을 초과할 경우 커넥션 종료
	- 기본값: 3
	- 예: ServerAliveInterval 15, ServerAliveCountMax 3
		- 통신없이 45초 결과후 커넥션 종료
- LogLevel
	- 클라이언트 측의 로그 레벨
	- 최소 로그: QUIET
	- 최대 로그: DEBUG3
	- 설정 값: ["QUIET","FATAL","ERROR","INFO","VERBOSE","DEBUG1","DEBUG2","DEBUG3"]
- StrictHostKeyChecking
	- ~/.ssh/known_hosts에 자동으로 호스트를 추가하는 설정
	- 기본 설정은 저장을 질문 함
	- 비활성 설정: "no"
- UserKnownHostsFile
	- 연결된 호스트에 대한 정보를 남기는 파일을 지정하는 설정
	- 기본 값: ~/.ssh/known_hosts
	- 일반적으로 이 설정을 변경하지 않음
	- StrictHostKeyChecking을 "no"로 설정할 경우에 이 설정을 "/dev/null"로 설정
- VisualHostKey
	- 원격지의 호스트 키를 클라이언트 접속시 출력
- Compression
	- 느린 네트웍 상에서 네트웍 패킷을 압축하는 옵션
	- 일반적인 상황에서 사용하지 않음

### ssh 키 지정
- IdentityFile
	- Host 별로 사용할 키의 위치를 지정
	- 기본값: 프로토콜에 따라 결정 됨 (~/.ssh/id_rsa or ~/.ssh/id_dsa)

### Multiplexing

ssh 명령은 하나의 서버에 접속하는 여러 SSH 커넥션을 하나의 TCP 커넥션을 사용하여 연결하는 기능을 제공합니다.
복수의 SSH 커넥션으로 발생하는 부하를 제거하는 용도로 적합

- ControlMaster
	- Multiplexing을 허용하는 옵션
	- 허용 설정 값: auto
- ControlPath
	- 커넥션을 제어하는 용도로 사용하는 socket 파일 지정
	- /path/to/socket/%r@%h:%p
		- r: username
		- h: remote host
		- p: port
- ControlPersist
	- 커넥션이 유지해야 할 시간을 초단위로 지정
	- 초단위 설정
	- 낮은 값을 설정할 경우 불필요한 커넥션 연결 오픈을 방지할 수 있음

## 요약

하나의 컴퓨터에서 여러 개의 SSH 키 파일를 유지하는 것은 매우 번거로운 일입니다.
최근에 여러 클라우드 서비스, github, bitbucket, gitlab 등 SSH 키 인증을 사용하는 서비스가 늘어나면서
ssh 키 파일 관리가 필요한 상황입니다.

ssh config 파일을 이용하면 도메인, ip 혹은 서버의 alias명 별로 접속 계정, SSH 키 파일, ssh 명령 속성
을 설정할 수 있기 때문에 ssh 명령을 간결하게 유지하는 방법을 제공합니다.

ssh config는  ```~/.ssh/config```파일입니다.
이 파일은 소유권자만이 접근할 수 있도록 퍼미션을 ```400```으로 제한해야 합니다.

## 참고자료
- [ssh 사용을 단순화하기 위한 ssh config 설정](http://taewan.kim/blog/2016/01/28/ssh_config/):  ```http://taewan.kim/blog/2016/01/28/ssh_config/```
- [ssh key 효율적인 관리 방법](http://www.popit.kr/ssh-key-효율적인-관리-방법/): ```http://www.popit.kr/ssh-key-효율적인-관리-방법/```
