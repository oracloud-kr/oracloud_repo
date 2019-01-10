+++
thumbnailInPost = "https://oracloud-kr-teamrepo.github.io/2017/04/sshkey/post.jpg"
description = "클라우드 서비스에 접근하기 위해서 혹은 github에 인증을 받기 위해서 ssh 보안키를 생성하는 절차가 필요합니다. "
language = "bsh"
tags = ["cloud", "ssh"]
author = "taewan.kim"  
categories = ["Cloud"]
title = "윈도우, 리눅스, 맥에서 ssh 보안키 생성"
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2017/04/sshkey/ssh.jpg"
date = "2017-03-04T12:07:16+09:00"
+++

클라우드 서비스로 만든 가상 서버에 안전한 로그인을 하기 위해서 SSH(Secure Shell) 프로토콜을 사용하는 것이 일반적입니다.
SSH 프로토콜은 안전하지 못한 네트워크에서 강력한 인증과 안전한 통신을 가능하게 합니다.
SSH 프로토콜에서 패스워드 입력없이 로그인하기 위해서는 인증서가 필요하며, 기본 포트는 22번입니다.

SSH 프로토콜에 대한 자세한 내용은 다음 문서를 참조하시기 바랍니다.

- [오픈 튜토리얼: SSH 원격제어](https://opentutorials.org/module/432/3738)
- [오픈 튜토리얼: SSH 키](https://opentutorials.org/module/432/3742)

이 문서에서는 다음 내용을 다루겠습니다.

- 운영체제별 SSH 키 생성 방법 (window, linux, osx)
- SSH 키 설치 방법
- ssh config를 이용한 ssh 명령 단순하게 사용하기

## 인증서 생성 방법

각 운영체제(Linux, OSX, Window)의 SSH 키 생성 방법에 대하여 정리하겠습니다.
운영체제 별로 SSH 키를 생성하는 방법은 다음과 같습니다.

|운영체제|SSH 키 생성 툴|
|---|---|
|리눅스|ssh-keygen|
|OS X|ssh-keygen, 리눅스와 OS X는 ssh-key 생성 방법 동일|
|window|puttygen - pytty key genetator |
|window|git 2.7 이상이 설치된 경우, ```git bash```에서 ssh-keygen 이용|

### 리눅스와 OS X에서 SSH 키 생성

리눅스와 OS X에서는 ssh-keygen을 이용하여 SSH 키를 생성합니다.
다음 명령은 ssh-keygen을 사용하는 예입니다.

<pre class="prettyprint linenums">
> ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/taewan/.ssh/id_rsa): ./oracloud_rsa
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in ./oracloud_rsa.
Your public key has been saved in ./oracloud_rsa.pub.
The key fingerprint is:
SHA256:9lsbDIYtkRrkXDflaZ4YAOqV5Kq5TOXrYNZcJE8OA/A taewan@taewanui-MacBook-Pro.local
The key's randomart image is:
+---[RSA 2048]----+
|...   +.o o..    |
| . . B o + o .   |
|  E = X o . +    |
|   . @ o + = .   |
|    + = S = o    |
|   B . . + o     |
|  B +     . +    |
| = o .     o o   |
|  o.o     . .    |
+----[SHA256]-----+
> ls -al oracloud_rsa*
-rw-------  1 taewan  staff  1675  4 22 09:31 oracloud_rsa
-rw-r--r--  1 taewan  staff   415  4 22 09:31 oracloud_rsa.pub
>
</pre>

- 라인 1: ssh-keygen 은 -t 옵션으로 암호화 유형을 선택합니다. 기본값은 rsa입니다.
- 라인 3: SSH 키 위치와 파일명을 지정합니다.
  - 개인 키: ```./oracloud_rsa```
  - 공개 키: ```./oracloud_rsa.pub```

나머지는 별도의 입력 없이 넘어갑니다.

### Window에서 SSH 키 생성

윈도우에서 SSH 키를 생성하는 방법은 putty에 포함된 puttygen을 이용할 수 있습니다. 또한
git이 설치되어 있다면 윈도우에서, git과 함께 설치되는 MINGW32이 제공하는 ssh-keygen을 이용하여
SSH 키를 생성할 수 있습니다.

#### Window에서 puttygen을 이용하는 ssh 키 만들기

[putty](http://www.chiark.greenend.org.uk/~sgtatham/putty/)를 내려받아 설치하면 puttygen도 함께 설치됩니다.
puttygen을 실행하면 <그림 1>과 같은 모습으로 실행됩니다.
<그림 1>와 같이 puttygen에서 키 타입으로 RSA와 키 생성의 비트 수를 2048로 설정한 후 "Generate" 버튼을 클릭합니다.

- 그림 1. puttygen(Putty Key Generator) 모습
![](https://oracloud-kr-teamrepo.github.io/2017/03/ssh_key/window010.jpg)

PuttyGen은 키를 생성할 때  마우스 위치 값을 키 생성 초깃값으로 활용합니다.
따라서 키를 생성이 완료할 때까지 <그림 2>의 빨간색 영역에서 마우스를 움직여야 합니다.

- 그림 2. RSA 키 생성 진행 중
![](https://oracloud-kr-teamrepo.github.io/2017/03/ssh_key/window020.jpg)

RSA 키 생성이 완료되면 <그림 3>과 같이 UI가 변경됩니다.
여기에서 ```Save Public Key``` 버튼을 클릭해서 공개 키 저장을 요청합니다.

- 그림 3. RSA 생성 결과, 공개 키 저장 요청
![](https://oracloud-kr-teamrepo.github.io/2017/03/ssh_key/window030.jpg)

<그림 4>와 같이 공개 키는 확장자를 ```pub```로 설정하여 저장합니다.

- 그림 4. 공개 키 저장
![](https://oracloud-kr-teamrepo.github.io/2017/03/ssh_key/window040.jpg)

공개 키 저장이 완료되면 <그림 5>와 같이 ```Save Private Key``` 버튼을 클릭해서 개인 키 저장을 요청합니다..

- 그림 5. 개인 키 저장 요청
![](https://oracloud-kr-teamrepo.github.io/2017/03/ssh_key/window050.jpg)

<그림 5>와 같이 개인 키를 저장합니다.

- 그림 6. 개인 키 저장
![](https://oracloud-kr-teamrepo.github.io/2017/03/ssh_key/window060.jpg)

#### Window에서 Git과 설치후 ssh 키 만들기

윈도우에 git이 설치되어 있다면, git과 함께 설치되는 MINGW32이 제공하는 ssh-keygen을 이용하여
SSH 키를 생성할 수 있습니다.

<그림 6>과 같이 ```git bash```를 윈도우에서 실행하면 리눅스 명령을 사용할 수 있습니다.
<그림 6>에서는 ssh-keygen을 이용하여 RSA 키를 생성한 후, 생성 파일 목록을 조회하고 있습니다.

- 그림 6. ```git bash```에서 ssh-keygen을 이용하여 SSH 키 생성하기
![](https://oracloud-kr-teamrepo.github.io/2017/03/ssh_key/window070.jpg)

지금까지 리눅스, 맥, 윈도우에서 SSH 키를 생성하는 절차를 알아보았습니다.
다음 절에서는 공개키 서버 설정과 SSH 키 관리 설정 방법에 대하여 알아보겠습니다.

## 공개키 서버 설정
Oracle cloud는 클라우드 서비스 인스턴스를 생성할 때, 공개 키를 등록하는 절차를 포함합니다.  
서버에 수작업으로 공개 키를 등록해야 한다면, 공개 키 등록 절차는 다음과 같습니다.

- 공개키 서버에 전송 (클라이언트 컴퓨터에서 수행)

<pre class="prettyprint">
> scp ./oracloud_rsa.pub 사용자ID@서버명:oracloud_rsa.pub
</pre>

- 서버에서 공개키 설정

<pre class="prettyprint">
> mkdir ~/.ssh
> chmod 700 .ssh
> cat ~/oracloud_rsa.pub >> .ssh/authorized_keys
</pre>

ssh 명령은 기본 SSH 키로 ~/.ssh/id_rsa를 사용합니다.
기본 키가 아닌 다른 SSH 키를 사용해야 한다면 ```-i``` 옵션을 사용해서 서버에 접속합니다.

<pre class="prettyprint">
> ssh 사용자ID@서버명 -i ./oracloud_rsa
</pre>

ssh 접속은 관련 파일의 권한 설정 때문에 발생하는 경우가 있습니다.
ssh 접속 클라이언트 컴퓨터와 서버의 ssh 관련 파일의 권한은 다음과 같아야 합니다.

|파일명| 권한|command|
|---|---|---|
|~/.ssh| 700 | chmod 700 ~/.ssh |
|개인키 (oracloud_rsa)| 600 | chmod 600 ~/oracloud_rsa |
|공개키 (oracloud_rsa.pub)| 644 | chmod 644 ~/oracloud_rsa.pub |
|~/.ssh/authorized_keys| 700 | chmod 600 ~/.ssh/authorized_keys |
|~/.ssh/known_hosts|600|chmod 644 ~/.ssh/known_hosts|

## 요약
운영체제별 ssh 키 파일 생성 방법에 대하여 알아보았습니다.
리눅스와 OS X의 경우 ssh-keygen 명령으로 간단하게 인증서를 생성할 수 있습니다.
윈도우의 경우 puttygen을 사용하거나 윈도우 git을 설치할 때 함께 설치되는 MINGW32의 ssh-keygen
을 사용하여 ssh 키 파일을 생성할 수 있습니다.

하나의 컴퓨터에서 어려개의 ssh 키를 사용하게 되면 ssh 명령이 복잡해지는 단점이 있습니다.
이 부분은 ssh config 파일을 이용하여 해결할 수 있습니다.

ssh config을 설정하는 방법에 대해서는
[ssh config를 이용한 ssh 단순화](/post/ssh_config/)에서 다루도록 하겠습니다.

## 참고자료
- [Wikipedia - Secure shell](https://ko.wikipedia.org/wiki/시큐어_셸)
