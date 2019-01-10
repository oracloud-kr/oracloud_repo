+++
date = "2018-10-26T01:09:25+09:00" 
description = "OCI에서의 Custom startup script 및 Cloud-init 사용하기" 
title = "OCI에서의 Custom Startup Scripts 및 Cloud-init 사용하기" 
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2018/10/win_cloudinit/oci_img.png" 
tags = ["oracle", "oci", "iaas", "cloud", "oracle cloud", "Windows", "오라클 클라우드"]
categories = ["Oracle IaaS"]  
author = "jinho.jang"  
language = ""  
adsense = "true"  
+++

OCI에서 Cloudbase-init을 이용해서 윈도우 서버 인스턴스를 쉽게 구성하고 사용자 지정하는 방법을 소개합니다. Windows Server의 새로운 통합 Cloud-Init 환경을 사용하면 더 많은 응용 프로그램, 호스트 구성 및 사용자 지정 설치로 인스턴스를 쉽게 부트 스트랩 할 수 있습니다. 이 기능은 Linux 또는 Windows Server를 실행하는 Oracle Cloud Infrastructure 컴퓨팅 인스턴스에서 Cloud-Init 사용자 정의 사용자 데이터 시작 스크립트(Custom user data startup script)를 통해 처리됩니다.

## 사용자 데이터란?

사용자 데이터는 Oracle Cloud Infrastructure에서 컴퓨팅 인스턴스가 초기화 될 때 스크립트 또는 사용자 정의 메타 데이터를 주입하는 메커니즘입니다. 이 데이터는 프로비저닝 시 인스턴스에 전달되어 필요에 따라 인스턴스를 사용자 지정합니다. 인스턴스 사용자 데이터는 다양한 스크립팅 언어를 사용하여 구현할 수 있습니다. 자세한 내용은 [Windows Cloudbase-Init](http://cloudbase-init.readthedocs.io/)를 참조하십시오.

## Windows 인스턴스 사용자 데이터 시작 스크립트

Windows Cloudbase-Init 환경은 모든 지역(Region)의 베어메탈 및 VM의 Windows Server 컴퓨팅 인스턴스에서 사용할 수 있습니다. 이 기능에는 추가 비용이 들지 않으며 모든 Windows Server OS 이미지에는 기본적으로 Cloudbase-Init가 설치되어 있습니다.
Cloudbase-Init에는 Windows 원격 관리 (WinRM) 구성을 완벽하게 자동화하는 기능도 함께 제공됩니다.

## 시작하기

첫번째 단계는 사용자 데이터 스크립트를 만드는 것입니다. 지원되는 컨텐츠의 형식은 아래와 같습니다.
​    PEM Certificate / Batch / PowerShell / Bash / Python / EC2 Format / Cloud config

보다 자세한 내용은 [Cloudbase-Init user data](http://cloudbase-init.readthedocs.io/en/latest/userdata.html) 를 참고하시기 바랍니다.

호스트 이름을 변경하고 로컬 부팅 볼륨의 사용자 지정 파일에 출력을 기록하는 간단한 PowerShell 스크립트의 다음 예제를 참조하십시오.  

1. Sysnative 매개 변수는 필수이며 첫 번째 줄에 있어야 합니다. PowerShell의 경우 다음을 사용합니다.  
   ```
   #ps1_sysnative
   ```

2. 다음 스크립트를 복사하여 .ps1 파일로 저장합니다. (이 스크립트는 컴퓨터 이름을 'WIN_OCI_INSTANCE_AD1_FE1'로 변경합니다.)  

   ```
   #ps1_sysnative
   
   function Get-TimeStamp {   
   
       return "[{0:MM/dd/yy} {0:HH:mm:ss}]" -f (Get-Date)   
   
   }
   
   $computerName='WIN_OCI_INSTANCE_AD1_FE1'
   
   $path = $env:SystemRoot + "\Temp\"
   
   $logFile = $path + "CloudInit_$(get-date -f yyyy-MM-dd).log"
   
   Write-Host -fore Green "Creating Log File"
   
   New-Item $logFile -ItemType file
   
   Write-Output "$(Get-TimeStamp) Logfile created..." | Out-File -FilePath $logFile -Append
   
   Write-Host -fore yellow "Changing ComputerName"
   
   Rename-Computer -NewName $computerName
   
   Write-Host -fore green "Changed ComputerName"
   
   Write-Output "$(Get-TimeStamp) Changed ComputerName" | Out-File -FilePath 
   $logFile -Append
   ```

   사용자 지정 사용자 데이터 시작 스크립트는 콘솔 또는 CLI(Command Line Interface)를 통해 인스턴스 생성 설정의 일부로 구현됩니다. 

## 콘솔을 통한 절차    

1. OCI 콘솔](https://console.us-ashburn-1.oraclecloud.com/) 에 로그인  

2. **Menu** -> **Compute** -> **Instances** 를 차례로 선택  

3. **Create Instance** 을 클릭하고 필수 입력란을 채우십시오. **Startup Script** 옵션은 **Show Advanced Options** 아래에 있습니다. 
   ![](https://github.com/oracloud-kr-teamrepo/oracloud-kr-teamrepo.github.io/blob/master/2018/10/win_cloudinit/img01.jpg?raw=true)

4. **Browse** 버튼을 눌러서 2번 단계에서 생성한 PS1 스크립트를 찾아 지정합니다.  
5. **Networking** 섹션을 완성하고 **Create instance**를 클릭하십시오.

인스턴스가 프로비저닝되면 Cloudbase-Init가 스크립트를 실행하고 WinRM을 자동으로 구성합니다.

## CLI를 통한 순서

CLI는 콘솔과 동일한 기능을 제공하여 CLI를 설치할 때 다음과 같은 **[설치 옵션](https://docs.cloud.oracle.com/iaas/Content/API/SDKDocs/cliinstall.htm?tocpath=Developer%20Tools%20%7CCommand%20Line%20Interface%20(CLI)%20%7C_____1)**을 따릅니다. 

1. 먼저 표의 CLI 명령을 사용하여 필수 매개 변수의 값을 가져옵니다 (이 명령은 PowerShell 명령 줄에서 실행 됨) 

    | **Parameter**                        | **CLI Command**                                       ----   |
    | ------------------------------------ | :----------------------------------------------------------- |
    | --compartment-id   [CompartmentOCID] | ./oci iam compartment list<br />$C = 'ocid1.compartment.oc1..aaaaaaaa....' |
    | --availability-domain [ADName]       | ./oci iam availability-domain list                           |
    | --shape [ShapeName]                  | ./oci compute shape list   --compartment-id $C               |
    | --image-id                           | ./oci compute image list -c $C \| ConvertFrom-Json \|   <br />ForEach-Object{$_.data} \| where -Property display-name -<br />Match   'Windows-Server-2016' \| fl -Property display-name, <br />id |
    | --subnet-id [SubnetOCID]             | ./oci network vcn list -c $C    **<br />Select Subnet OCID that matches chosen   <br />AD above:**    ./oci network subnet list -c $C --vcn-id ocid1.vcn.oc1.iad.aaaaaaa…. |
    | --user-data-file [filename]          | enter path and filename for user data   startup script       |
    | --display-name [StringinstanceName]  | enter free form Instance display name                        |
    | --assign-public-ip                   | true                                                         |

2. 컴퓨트 인스턴스를 시작하는 구문  

```
   ./oci compute instance launch --availability-domain [ADName] --compartment-id
   [CompartmentOCID] --shape [ShapeName] --subnet-id [SubnetOCID] --user-data-file [filename] --display-name [StringinstanceName] --assign-public-ip 
   
   example:
   
   ./oci compute instance launch --availability-domain mgRc:US-ASHBURN-AD-3 --compartment-id $C --shape VM.Standard2.1 --image-id ocid1.image.oc1.iad.aaaaaaaag.... --subnet-id ocid1.subnet.oc1.iad.aaaaaaaar....
   --user-data-file PScloudbaseinit1.ps1 --display-name MyCloudInitInstanc
```  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3. 인스턴스 상태를 쿼리해서 이전 명령의 성공한 출력에서 인스턴스 ID를 가져옵니다.   

   ```
   ./oci compute instance get --instance-id ocid1.instance.oc1.iad.abuwcljr32gb5....
   ```



### 일반적인 사용자 데이터 사용자 지정 스크립트 사용 사례 :  

-   레지스트리를 포함한 호스트 설정 업데이트
-   GPU 지원 활성화 – 사용자 지정 스크립트를 통해 GPU 드라이버 설치
-   로컬 사용자 계정 추가 및 변경
-   도메인 컨트롤러에 인스턴스 가입
-   인증서 저장소에 인증서 설치
-   IIS와 같은 추가 Windows 기능 활성
-   필요한 애플리케이션 워크로드 파일을 Object Storage에서 로컬 인스턴스로 직접 복사
-   Chef, Puppet 또는 SCOM 에이전트와 같은 클라이언트 에이전트 다운로드 및 설치



### WinRM

Windows 원격 관리 (WinRM)는 Windows 호스트를 원격으로 관리 할 수 있는 기능을 제공하는 관리 인터페이스 입니다. Windows PowerShell 명령 줄에는 통합 된 WinRM cmdlet의 이점이 있으며 모든 Windows 관리 작업에 대해 단일 도구를 통해 모든 기능을 제공합니다.

**Oracle Cloud Infrastructure Windows 인스턴스에서 WinRM을 사용하는 방법**

1. 콘솔 열기

2. 인스턴스가 사용하는 VCN 보안 목록에 수신 규칙을 추가하십시오.
   ![](https://raw.githubusercontent.com/oracloud-kr-teamrepo/oracloud-kr-teamrepo.github.io/master/2018/10/win_cloudinit/img02.jpg)

    a.	콘솔에서, 시작 스크립트를 사용하여 새로 실행된 인스턴스로 이동하여 인스턴스 세부 정보를 봅니다.<br>
    b.	**Subnet Settings**에서 인스턴스가 속한 서브넷의 이름을 클릭 (Subnet: Subnet-name)<br>
    c.	**Resources** 에서 **Security Lists** 로 이동하여 보안목록을 엽니다.<br>
    d.	**Edit All Rules** 를 클릭<br>
    e.	**Allow Rules for Ingress** 에서 **Add Rule**을 클릭<br>
   ​    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;i. Destination Port Range: 5986<br>
   ​    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;​ii. SOURCE PORT RANGE: All<br>
   ​    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;iii. IP Protocol: TCP<br>
   ​    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;iv. Source CDIR: 0.0.0.0/0<br>
   ​    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(Recommend Source is from your authorized CIDR block)<br>
   ​    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;v. Source Type: CIDR<br>
   f.	**Security List Rules**  저장<br> 

3. Instance detail 화면에서 인스턴스의 Public IP를 가져옵니다.<br>

4. Windows 클라이언트에서 PowerShell 명령창을 엽니다.<br>

5. 다음 PowerShell snippet을 사용하여 인스턴스에 연결하십시오.<br>

   ```
   # Get the public IP from your OCI running windows instance
   $ComputerName = "USE PUBLIC IP OF INSTANCE"
   
   # Store your username and password credentials (default username is opc)
   $c = Get-Credential
   
   # Options
   $opt = New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck
   
   # Create new PSSession (Pre-requisite: ensure security list has Ingress Rule for port 5986) 
   $PSSession = New-PSSession -ComputerName $ComputerName -UseSSL -SessionOption $opt -Authentication Basic -Credential $c
   
   # Connect to Instance PSSession
   Enter-PSSession $PSSession
   
   # To close connection use: Exit-PSSession
   ```



이제 로컬 PowerShell 클라이언트에서 Windows Server Compute 인스턴스를 원격으로 관리 할 수 있습니다.

Windows Server 사용자는 이제 사용자 지정 컴퓨팅 인스턴스를 설정하는 두 가지 좋은 옵션을 사용할 수 있습니다.
또한 WinRM을 사용하여 Windows 인스턴스를 원격으로 관리하고 안전하게 액세스 할 수 있다는 이점이 있습니다. 
보다 자세한 내용은 다음 설명서를 참조하십시오.

·       [Custom User Data Startup Script on Windows Images](https://docs.cloud.oracle.com/iaas/Content/Compute/References/images.htm#winDetails)

·       [CLI reference to launch instance with User Data](https://docs.cloud.oracle.com/iaas/api/#/en/iaas/20160918/datatypes/LaunchInstanceDetails)



**문서 원문**

OCI 공식 블로그에 게시된 Andy Corran이 작성한 https://blogs.oracle.com/cloud-infrastructure/windows-custom-startup-scripts-and-cloud-init-on-oracle-cloud-infrastructure 입니다.