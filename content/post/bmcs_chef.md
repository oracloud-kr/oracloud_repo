+++
date = "2017-09-11T21:59:47+09:00"
description = "Oracle Cloud Infrastructure(BMCS)에 CHEF를 구성하는 방법을 알아 봅니다. 이후, CHEF를 활용하여 Oracle Cloud Infrastructure(BMCS)를 활용하는 방법도 알아보겠습니다."
title = "Oracle Cloud Infrastructure(BMCS)에서 CHEF 활용하기"
thumbnailInList = "http://3.bp.blogspot.com/-9xQ9sitBwhI/TwS-a8aqCgI/AAAAAAAAD1s/gcASShTYAJg/s1600/chef-2.png"
thumbnailInPost = "https://cloudmarketplace.oracle.com/marketplace/content?contentId=4957921&ver=20161125212346"
tags = ["BMCS", "IaaS", "Chef"]
categories = ["IaaS", "Chef"]
author = "moonsun.lee"
language = "rb"  #bsh,c,cc,cpp,cs,csh,cyc,cv,htm,html,java,js,m,mxml,perl,pl,pm,py,rb,sh,xhtml,xml,xsl
+++

<br />
다음은 Oracle Cloud Infrastructure(BMCS) 환경에서 CHEF를 구성하고 활용하는 방법에 대한 문서입니다.<br /><br />
CHEF에 대한 자세한 내용은 [CHEF문서(docs.chef.io)](https://docs.chef.io)를 참고하시면 됩니다.
## 1. CHEF란?
CHEF는 Ansible, Puppet과 더불어 가장 많이 사용되고 있는 Configuration Management툴이며, 2009년에 처음 OpsCode에서 만든 오픈소스 프레임워크 입니다. CHEF는 Server나 Application을 쉽게 배포하고, Cloud Infrastructure를 자동화 할 수 있도록 도와줍니다.<br /><br />CHEF는 Ruby기반의 DSL(Domain Specific Language)을 사용하기 때문에 배우기 쉬우면서도, Ruby언어의 장점을 그래도 계승하고 있습니다. 또한, CHEF는 세상에 나온지 8년이 되었고, 레퍼런스가 다양하고 광범위하기 때문에 참조할만한 리소스가 매우 많고, 구축할 때 모든 것을 처음부터 일일이 손수 만들 필요가 없습니다.<br /><br />CHEF의 마켓플레이스인 [슈퍼마켓(supermarket.chef.io)](https://supermarket.chef.io)을 통해서 원하는 Cookbook과 Recipe를 쉽게 검색하고 활용할 수 있습니다. 3000여개의 Cookbook이 업로드되어 있으며, 누구나 Contribution 할 수 있습니다.<br /><br />
그럼 CHEF의 구성에 대해 간략히 살펴보도록 하겠습니다. <br />

![](https://www.linode.com/docs/assets/chef_graph.png "CHEF 아키텍쳐")

> [CHEF Solo](https://docs.chef.io/chef_solo.html)는 이번 문서에서는 설명하지 않고, Enterprise 환경에 적합한 Workstation과 Server가 분리된 환경을 구성하고 활용하는 방법에 대해 소개하도록 하겠습니다.

CHEF는 Workstation, Server 그리고 관리되는 Node로 구성됩니다.<br />  

- **CHEF Server** <br />
CHEF Server는 배포할 Node 목록과 이 Node들에 대한 정책을 적용할 Rule, Cookbook, Recipe 등 각종 설정내용을 저장하는 중앙저장소 역할을 합니다.<br /><br />
- **CHEF Workstation** <br />
CHEF Workstation은 배포할 Application에 대한 설정이나 명령어들을 만드는 서버이며, 개발자들은 Workstation에서 대부분의 작업을 수행합니다.<br /><br />
- **Node** <br />
Node는 Chef에서 관리할 자동화를 시킬 대상(서버, 인스턴스, VM 등)을 이야기 합니다.

## 2. CHEF Server 요구사항
CHEF Server는 x86_64기반의 독립된 리눅스 서버가 별도로 필요하며(윈도우 기반은 지원하지 않습니다), 현재 Red Hat과 Ubuntu계열 리눅스를 지원합니다.<br /><br />또한 CHEF Server는 방화벽에서 **80, 443 포트를 Open**하여야 하며, 1000개의 Node를 관리하기 위해서 요구되는 Server의 Minimum Spec은 Oracle Cloud Infrastructure(BMCS)기준으로 ["VM.Standard1.1 ( 1 OCPU / 7.5GB )"](https://cloud.oracle.com/en_US/infrastructure/compute/pricing)입니다.<br /><br />원활한 Demo 진행을 위하여, **CHEF Server와  CHEF Workstation을 위한 Linux Instance 2개를 Oracle Cloud Infrastructure(BMCS)에 미리 생성**해두었으며 OS는 Red Hat 계열인 Oracle Linux를 선택하였습니다.<br /><br />


## 3. CHEF Server 설치 및 구성
먼저 CHEF Server를 설치하고 구성하는 방법에 대해 살펴보겠습니다.<br /><br /> CHEF Server의 Install 패키지는 아래와 같이 [CHEF Downloads 페이지](https://downloads.chef.io)에서 다운 받을 수 있으며, OS 별 패키지 URL을 제공하므로 "wget" 커맨드로 직접 다운로드 받아 설치하셔도 무방 합니다.

> Demo에서는 Oracle Cloud Infrastructure(BMCS)에 생성한 인스턴스에 맞게 "Red Hat 6" 용 12.15.8 버전을 설치하였습니다. 각 OS에 맞게 패키지를 설치하면 됩니다.

***

![](http://linux.systemv.pe.kr/wp-content/uploads/2015/06/150621-0002.png "CHEF 다운로드페이지")

***
<br />

\* **서버에 CHEF Server Package 다운로드 및 설치**

Download 페이지에서 제공되는 패키지 URL을 통하여, CHEF Server를 다운로드 받아 설치합니다.

<pre><code>
[opc@chef-server ~]$ wget https://packages.chef.io/files/stable/chef-server/12.15.8/el/6/chef-server-core-12.15.8-1.el6.x86_64.rpm<br />
[opc@chef-server ~]$ sudo rpm -Uvh chef-server-core-12.15.8-1.el6.x86_64.rpm
Preparing...                ########################################### [100%]
   1:chef-server-core       ########################################### [100%]
</code></pre>

***
<br />

\* **CHEF Server 구성**

설치가 완료되면, CHEF Server의 설치를 마무리 합니다. **chef-server-ctl reconfigure** CLI를 통하여, 서버에 필요한 패키지들을 자동으로 설치하고 설정을 해줍니다.

<pre><code>
[root@chef-server opc]# chef-server-ctl reconfigure
Chef Client finished, 490/1080 resources updated in 06 minutes 02 seconds
Chef Server Reconfigured!
</code></pre>

***
<br />

\* **CHEF Server User 및 Group 생성 (관리자 계정)**

User와 Group 생성시 만들어지는 **user.pem** 와 **group-validator.pem** 은 CHEF Server 연결을 위한 증명서 파일입니다. CHEF Workstation에서 Server로 접속하여 작업을 수행하는데, 접속 인증을 위해 필요한 증명서명 파일입니다. 따라서 이 파일은 추후 CHEF Workstation으로 복사해주어야 합니다.

* 1. CHEF Server User 생성

> chef-server-ctl user-create **user_name** **first_name** **last_name** **email** **password** --filename **FILE_NAME**

<pre><code>
[root@chef-server opc]# chef-server-ctl user-create oracle gildong hong gildong.hong@oracle.com 'welcome1' --filename /path/to/oracle.pem
</code></pre>

* 2. CHEF Server Group 생성

> chef-server-ctl org-create **short_name** **full_organization_name** --association_user **user_name** --filename **FILE_NAME**

<pre><code>
[root@chef-server opc]# chef-server-ctl org-create cloudteam 'Cloud Infrastructure Team' --association_user oracle --filename /path/to/cloudteam-validator.pem
</code></pre>

***
<br />

\* **CHEF Server의 통신을 위한 방화벽 포트 오픈**

<pre><code>
[root@chef-server opc]# iptables -I INPUT -p tcp --dport 80 -j ACCEPT <br />
[root@chef-server opc]# iptables -I INPUT -p tcp --dport 443 -j ACCEPT
</code></pre>


## 4. CHEF Workstation 설치 및 구성

***
<br />


\* **CHEF DK(Development Kit) 다운로드 및 설치**
<pre><code>
[opc@chef-workstation ~]$ wget https://packages.chef.io/files/stable/chefdk/2.0.28/el/6/chefdk-2.0.28-1.el6.x86_64.rpm
--2017-07-24 08:20:04--  https://packages.chef.io/files/stable/chefdk/2.0.28/el/6/chefdk-2.0.28-1.el6.x86_64.rpm
Resolving packages.chef.io... 151.101.24.65
Connecting to packages.chef.io|151.101.24.65|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 102952780 (98M) [application/x-rpm]
Saving to: “chefdk-2.0.28-1.el6.x86_64.rpm”
100%[============================================================================================>] 102,952,780 5.97M/s   in 16s
2017-07-24 08:20:23 (6.21 MB/s) - “chefdk-2.0.28-1.el6.x86_64.rpm” saved [102952780/102952780]<br />
[opc@chef-workstation ~]$ ls
chefdk-2.0.28-1.el6.x86_64.rpm<br />
[opc@chef-workstation ~]$ sudo rpm -Uvh chefdk-2.0.28-1.el6.x86_64.rpm
warning: chefdk-2.0.28-1.el6.x86_64.rpm: Header V4 DSA/SHA1 Signature, key ID 83ef826a: NOKEY
Preparing...                ########################################### [100%]
   1:chefdk                 ########################################### [100%]
Thank you for installing Chef Development Kit!
</code></pre>

***
<br />

\* **CHEF DK(Development Kit) 설치 확인**
<pre><code>
[opc@chef-workstation ~]$ chef verify
[WARN] This is an internal command used by the ChefDK development team. If you are a ChefDK user, please do not run it.
Running verification for component 'berkshelf'
Running verification for component 'test-kitchen'
Running verification for component 'tk-policyfile-provisioner'
Running verification for component 'chef-client'
Running verification for component 'chef-dk'
Running verification for component 'chef-provisioning'
Running verification for component 'chefspec'
...
Verification of component 'git' succeeded.
Verification of component 'chef-client' succeeded.
Verification of component 'chef-provisioning' succeeded.
Verification of component 'chef-dk' succeeded.
Verification of component 'generated-cookbooks-pass-chefspec' succeeded.
Verification of component 'package installation' succeeded.<br />
[opc@chef-workstation ~]$ chef --version
Chef Development Kit Version: 2.0.28
chef-client version: 13.2.20
delivery version: master (17c1b0fed9be4c70f69091a6d21a4cbf0df60a23)
berks version: 6.2.0
kitchen version: 1.16.0
inspec version: 1.31.1
</code></pre>


***
<br />

\* **작업 Repository 폴더 생성 및 설정**

<pre><code>
[opc@chef-workstation ~]$ mkdir ~/chef-repo
[opc@chef-workstation ~]$ cd ~/chef-repo
[opc@chef-workstation chef-repo]$ mkdir ~/chef-repo/.chef

** Server에서 User와 Group 생성시 만들어진 pem파일을 해당 Repository의 .chef 폴더로 복사해 줍니다. 
** .chef 폴더에서 pem 파일을 참조하게 됩니다.

[opc@chef-workstation chef-repo]$ cd .chef<br />
[opc@chef-workstation .chef]$ ls
infrateam-validator.pem  oracle.pem

</code></pre>

***
<br />


\* **Knife.rb 파일 생성 및 설정**

<pre><code>
**Knifr.rb format**
current_dir = File.dirname(__FILE__)
  user = ENV['OPSCODE_USER'] || ENV['USER']
  node_name                user
  client_key               "#{ENV['HOME']}/chef-repo/.chef/#{user}.pem"
  validation_client_name   "#{ENV['ORGNAME']}-validator"
  validation_key           "#{ENV['HOME']}/chef-repo/.chef/#{ENV['ORGNAME']}-validator.pem"
  chef_server_url          "https://api.opscode.com/organizations/#{ENV['ORGNAME']}"
  syntax_check_cache_path  "#{ENV['HOME']}/chef-repo/.chef/syntax_check_cache"
  cookbook_path            ["#{current_dir}/../cookbooks"]
  cookbook_copyright       "Your Company, Inc."
  cookbook_license         "apachev2"
  cookbook_email           "cookbooks@yourcompany.com"

[opc@chef-workstation .chef]$ cat knife.rb<br />
--------------

node_name                'oracle'
client_key               '/home/opc/chef-repo/.chef/oracle.pem'
validation_client_name   'cloudteam-validator'
validation_key           '/home/opc/chef-repo/.chef/cloudteam-validator.pem'
chef_server_url          'https://chef-server.sub07210735550.chefvcn.oraclevcn.com/organizations/cloudteam'
syntax_check_cache_path  '/home/opc/chef-repo/.chef/syntax_check_cache'
cookbook_path 		     [ '/home/opc/chef-repo/cookbooks' ]

--------------

</code></pre>

***
<br />
\* **CHEF Workstation의 통신을 위한 방화벽 포트 오픈 및 연결 확인**

<pre><code>
[root@chef-workstation chef-repo]# iptables -I INPUT -p tcp --dport 80 -j ACCEPT <br />
[root@chef-workstation chef-repo]# iptables -I INPUT -p tcp --dport 443 -j ACCEPT

[opc@chef-workstation chef-repo]$ knife ssl check
Connecting to host chef-server.sub07210735550.chefvcn.oraclevcn.com:443
Successfully verified certificates from `chef-server.sub07210735550.chefvcn.oraclevcn.com'
<br />
[opc@chef-workstation chef-repo]$ knife client list
cloudteam-validator

</code></pre>



## 5. Oracle Cloud Infrastructure(BMCS)를 위한 CHEF Knife Plugin 설치 및 구성


Oracle Cloud Infrastructure(BMCS)를 위한 CHEF Knife Plugin 설치를 진행하겠습니다.<br /><br />
Oracle에서는 Oracle 서비스에 관련된, CHEF Plugin과 Cookbook을 Github에 배포하고 있습니다. 아래 주소로 접속하시면 Oracle Cloud Infrastructure(BMCS)용 Knife Plugin을 받아 볼 수 있고, 현재(2017.09.13) 2.0.0 버전까지 배포되었습니다.<br /><br />
**https://github.com/oracle/knife-oci/releases** (Release Info.)<br />
**https://github.com/oracle/knife-oci/blob/v2.0.0/docs/rename.md** (Readme.md)

Knife Plugin의 아래 명령어들을 이용하여, 직접 Oracle Cloud Infrastructure에서 실행할 수 있습니다.<br /> 
(클라우드 서버 생성/삭제, Compartmet/VM Image/VCN/Subnet/Shape/AD 조회등의 작업을 할 수 있습니다.)

- Launch a OCI instance and bootstrap it as a Chef node: **knife oci server create**
- List OCI compartments. **knife oci compartment list**
- Delete a OCI instance: **knife oci server delete**
- List BMCS instances in a given compartment. Note: All instances in the compartment are returned, not only those that are Chef nodes: **knife oci server list**
- List the images in a compartment: **knife oci image list**
- List the VCNs in a compartment: **knife oci vcn list**
- List the subnets in a VCN: **knife oci subnet list**
- List the shapes that may be used for a particular image type: **knife oci shape list**
- List the availability domains for your tenancy: **knife oci ad list**
 

***

\* **CHEF Workstation에 Oracle CHEF 플러그인 설치**<br /><br />
그럼 플러그인을 설치해보도록 하겠습니다. 위의 페이지에서 직접 다운받으시거나, 다음 URL [ZIP 파일](https://github.com/oracle/knife-oci/archive/v2.0.0.zip),  [tar.gz 파일](https://github.com/oracle/knife-oci/archive/v2.0.0.tar.gz)을 이용하여 Gem 파일을 다운받아 CHEF Workstation 서버에 압축을 풀고, 해당 ruby gem 패키지를 아래와 같이 설치합니다.


<code><pre>
[opc@chef-workstation knife-oci-2.0.0]$ ls
CHANGELOG.md     Gemfile             lib          Rakefile   spec
CONTRIBUTING.md  knife-bmcs.gemspec  LICENSE.txt  README.md  test_output<br />
[opc@chef-workstation knife-oci-2.0.0]$ sudo chef gem install knife-oci
...
Successfully installed knife-oci-2.0.0
4 gems installed
</code></pre>

***
\* **Oracle Cloud Infrastructure CHEF Knife Plugin 설정**<br /><br />
플러그인 설치를 완료하면, 해당 Plugin 구성파일과 CHEF Knife 설정파일(knife.rb)에 Oracle Cloud Infrastructure(BMCS)의 정보를 입력해주어야 합니다.<br />

> Oracle Cloud Infrastructure에서는 Cloud의 리소스들을 "ocid (oracle-assigned unique ID)"라는 형태의 API로 Access 정보를 제공합니다. Cloud Console에 접속하시면 해당 도메인의 ocid를 쉽게 확인 할 수 있습니다. 아래 페이지를 통해 리소스들의 ocid 확인방법을 참고 하시면 됩니다.<br /> https://docs.us-phoenix-1.oraclecloud.com/Content/General/Concepts/identifiers.htm

![](https://oracloud-kr-teamrepo.github.io/2017/04/bmcs_terraform/bmcsconsole3.jpg "bmcsconsole")

> 예시1. OCI Tenancy ocid

![](http://www.ateam-oracle.com/wp-content/uploads/2017/07/find-compartment-ocid.png "bmcsconsole2")

> 예시2. OCI Compartment ocid

***
<br />

* Plugin 구성파일 생성 & 관리하려는 Cloud 도메인 정보 입력

<code><pre>
[opc@chef-workstation ~]$ cat ~/.oci/config
[DEFAULT]
user=ocid1.user.oc1..aaaaaaaaca6xa6huyxe55pbq3twvvgkosiidpzrjisnsueqkg5qtnnhbaqsq
fingerprint=50:17:74:23:0b:a2:6a:be:fb:4d:75:d6:22:07:bb:45
key_file=/home/opc/.config/bmcs_api_key.pem *(OCI 홈페이지에서 제공)
tenancy=ocid1.tenancy.oc1..aaaaaaaamulgqmi6jtkvgoo5o7lj5rn73if3adukc7okfbvyc3jssamzwegq
region=us-phoenix-1
</code></pre>

* 기존 Knife.rb에 BMCS 설정 추가

<code><pre>
[opc@chef-workstation .chef]$ pwd
/home/opc/chef-repo/.chef
[opc@chef-workstation .chef]$ vi knife.rb<br />
** Oracle Cloud Infrastructure (BMCS) 정보 추가
knife[:compartment_id]='ocid1.compartment.oc1..aaaaaaaacazgpy5nlextlsues2lwk7wlsj2djml3w5d3gkcwpltfegzuxvuq'
</code></pre>

## 6. Knife Plugin 테스트

설치 및 설정이 완료되면 knife툴을 이용하여 아래와 같이 Oracle Cloud Infrastructure의 정보를 조회하거나, 인스턴스를 CHEF 상에서 만들고 삭제 할 수 있는 Provisioning 및 List 기능을 제공합니다.

***

\* **Knife Plugin 사용 예제**



<code><pre>
[opc@chef-workstation chef-repo]$ knife oci image list
Display Name                                              ID                                                                                OS                OS Version
Windows-Server-2012-R2-Standard-Edition-VM-2017.04.03-0   ocid1.image.oc1.phx.aaaaaaaa53cliasgvqmutflwqkafbro2y4ywjebci5szc4eus5byy2e2b7ua  Windows           Server 2012 R2 Standard
Windows-Server-2012-R2-Standard-Edition-BM-2017.04.13-0   ocid1.image.oc1.phx.aaaaaaaa7xgecq2kt7tikqfrmshu6gwukoc3lcnf2iqtwmjyarlprp6j6lna  Windows           Server 2012 R2 Standard
Oracle-Linux-7.3-2017.07.17-0                             ocid1.image.oc1.phx.aaaaaaaa5yu6pw3riqtuhxzov7fdngi4tsteganmao54nq3pyxu3hxcuzmoa  Oracle Linux      7.3
Oracle-Linux-7.3-2017.05.23-0                             ocid1.image.oc1.phx.aaaaaaaa6uwtn7h3hogd5zlwd35eeqbndurkayshzvrfx5usqn6cwxd5vdqq  Oracle Linux      7.3
Oracle-Linux-7.3-2017.04.18-0                             ocid1.image.oc1.phx.aaaaaaaaqutj4qjxihpl4mboabsa27mrpusygv6gurp47kat5z7vljmq3puq  Oracle Linux      7.3
Oracle-Linux-6.9-2017.07.17-0                             ocid1.image.oc1.phx.aaaaaaaa3s4v5eamndtyghbo4bj2mhobkwjwbz3eowyy5cebmrsoxvoo
...
</code></pre>

> OCI에서 제공하는 OS 이미지 조회

<code><pre>
knife oci server create
  --availability-domain 'kIdk:PHX-AD-1'
  --compartment-id 'ocidv1:tenancy:oc1:phx:1460406592660:aaaaaaaab4faofrfkxecohhjuivjq26a13'
  --image-id 'ocid1.image.oc1.phx.aaaaaaaaqutj4qjxihpl4mboabsa27mrpusygv6gurp47katabcvljmq3puq'
  --shape 'VM.Standard1.1'
  --subnet-id 'ocid1.subnet.oc1.phx.aaaaaaaaxlc5cv7ewqr343ms4lvcpxr4lznsf4cbs2565abcm23d3cfebrex'
  --ssh-authorized-keys-file ~/.keys/instance_keys.pub
  --display-name myinstance
  --identity-file ~/.keys/instance_keys
  --run-list 'recipe[my_cookbook::my_recipe]'
  --region us-phoenix-1
</code></pre>

> OCI에 인스턴스 생성하기

***


## 7. 마치며
이런 CHEF와 같은 Configuration Management Tool을 이용하여, 우리는 클라우드 환경에서 조금 더 쉽게 코드레벨로 인프라와 어플리케이션을 관리 할 수 있습니다. 단순하게는 형상관리는 어플리케이션만을 관리 할 수 있다고 생각 할 수 있지만, 클라우드 환경에서 Infrastructure도 코드화 되기 때문에 Infrastructure를 Provision 하는데도 소프트웨어 처럼 Code로 관리를 할 수 있습니다.<br />

$300의 무료 Trial을 신청하고, Oracle Cloud를 CHEF로 한번 관리해 보세요!


***
**Useful Links**<br /><br />
**1. Chef Server Installation Guide on Oracle Cloud Document**<br />
http://www.oracle.com/webfolder/technetwork/tutorials/obe/cloud/compute-iaas/chef_setup/chef_setup.html#section7 <br /><br />
**2. Orchestrations in Oracle IaaS with Chef Document**<br />
https://community.oracle.com/community/oracle-cloud/oracle-cloud-developer-solutions/blog/2016/10/31/orchestration-in-oracle-iaas-with-chef <br />

**3. Knife Oracle Bare Metal Cloud Knife Plugin**<br />
https://github.com/oracle/knife-bmcs <br />
https://docs.us-phoenix-1.oraclecloud.com/Content/API/SDKDocs/knifeplugin.htm
***