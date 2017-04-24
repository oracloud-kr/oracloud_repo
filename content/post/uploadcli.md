+++
author = "jaeyeop.lee"
categories = ["IaaS"]
date = "2017-04-19T10:45:24+09:00"
description = ""
language = "bsh"
tags = []
thumbnailInList = "https://oracloud-kr-teamrepo.github.io/2017/04/uploadcli/opcStorageBannerIcon.jpg"
thumbnailInPost = ""
title = "Upload CLI을 이용한 Object Storage 데이터 이관"
+++

데이터베이스의 대용량 export dump file을 DBCS에 Upload하는 기본적인 방법은 sftp를 이용하여 DBCS Instance로 Upload하는 것입니다. 하지만, sftp를 이용할 경우 하나의 세션을 이용하기 때문에 Network Bandwidth를 모두 사용하지 못해 대용량 Dump file의 경우 Upload속도가 많이 느려질수 있습니다.

![](https://oracloud-kr-teamrepo.github.io/2017/04/uploadcli/uploadcli-01.jpg)

데이터베이스의 대용량 export dump file을 효율적으로 DBCS에 Upload하는 방법은 다음과 같습니다.

![](https://oracloud-kr-teamrepo.github.io/2017/04/uploadcli/uploadcli-02.jpg)

Oracle Storage Cloud에 Upload하기 위한 Tool인 uploadcli에 대한 설치 방법은 다음과 같습니다.

- 사전 요구사항 : JRE 7 or later
- OTN을 통해 uploadcli를 [download](http://www.oracle.com/technetwork/topics/cloud/downloads/index.html#cli)합니다.

## uploadcli
### 기본 명령어

- java -jar uploadcli.jar -url ***REST_Endpoint_URL*** -user ***userName*** -container ***containerName file-or-files-or-directory***
- Proxy Server를 사용해야 하는 경우
  - java -Dhttps.proxyHost=*host* -Dhttps.proxyPort=*port* -jar uploadcli.jar .....

### uploadcli에서 제공되는 parameter
<table>
<caption><b>uploadcli parameter</b></caption>
<tr style="backgroud-color: rgb(192,192,192)">
  <th width="110">Parameter</th>
  <th width="100">Required or Optional</th>
  <th>Description</th>
</tr>
<tr>
  <td align="center">-user</td>
  <td align="center">Required</td>
  <td>User name (example: jack.jones@example.com).</td>
</tr>
<tr>
  <td align="center">-url</td>
  <td align="center">Required</td>
  <td>REST endpoint URL of your Oracle Storage Cloud Service account.
Example: https://foo.oraclecloud.com/Storage-myIdentity3</td>
</tr>
<tr>
  <td align="center">-url</td>
  <td align="center">Required</td>
  <td>REST endpoint URL of your Oracle Storage Cloud Service account.<br/>
Example: https://foo.oraclecloud.com/Storage-myIdentity3</td>
</tr>
<tr>
  <td align="center">-password-file</td>
  <td align="center">Optional</td>
  <td>Full path and name of the file containing the password for the user.<br/>If you don’t specify this option, the command prompts you to enter the password.</td>
</tr>
<tr>
  <td align="center">-domain</td>
  <td align="center">Optional</td>
  <td>Identity domain name of your Oracle Storage Cloud Service account. Required if -url does not provide the complete REST Endpoint of your Oracle Storage Cloud Service account.
See Authenticating Access When Using the REST API.</td>
</tr>
<tr>
  <td align="center">-service</td>
  <td align="center">Optional</td>
  <td>Name of your Oracle Storage Cloud Service instance.<br>Default: Storage<p>
Required if -url does not provide the complete REST Endpoint of your Oracle Storage Cloud Service account.<br>See Authenticating Access When Using the REST API.</td>
</tr>
<tr>
  <td align="center"><b style="color: rgb(255,0,0)">-segment-size</b></td>
  <td align="center">Optional</td>
  <td>Size of the segments that the large file must be divided into before it is uploaded to Oracle Storage Cloud Service.<br>Specify the size in MiB (one MiB = 1048576 bytes)<p>
Default: 200<br>Minimum: 1 MiB,<br>Maximum: 5000 MiB<p>Guidelines for selecting the segment size:<p>
The segments are uploaded in parallel using multiple threads. A smaller segment size may result in faster overall upload.<p>
If you notice upload retries (up to five times) or slow uploads, decrease the segment size.<p>
For example, when uploading a 20000-MiB file, specify a segment size that is between 20 and 5000 MiB. A smaller segment size than that would result in more than 1000 segments</td>
</tr>
<tr>
  <td align="center">-container</td>
  <td align="center">Required</td>
  <td>Container name to which the manifest object will be uploaded. If the container doesn't exist, the tool creates it.</td>
</tr>
<tr>
  <td align="center">-segment-container</td>
  <td align="center">Optional</td>
  <td>Container name to which all the segment objects will be uploaded. If the container doesn't exist, the tool creates it.<br>Default: containerName_segments</td>
</tr>
<tr>
  <td align="center">-archive</td>
  <td align="center">Optional</td>
  <td>Must be specified if the container is an archive container.</td>
</tr>
<tr>
  <td align="center">-overwrite</td>
  <td align="center">Optional</td>
  <td>Overwrites any existing object with the same name</td>
</tr>
<tr>
  <td align="center">-debug</td>
  <td align="center">Optional</td>
  <td>Generates a debug level log (upload.trace). To troubleshoot an issue, use this flag to create a log file (uploadcli.log).</td>
</tr>
<tr>
  <td align="center">-version</td>
  <td align="center">Optional</td>
  <td>Displays the version of the tool.</td>
</tr>
<tr>
  <td align="center"><b style="color: rgb(255,0,0)">-max-threads</b></td>
  <td align="center">Optional</td>
  <td>Maximum number of threads to be used at a given time. Depending on the speed and quality of the network, the Upload CLI may perform better with different number of threads. Default number of threads: 15</td>
</tr>
<tr>
  <td align="center">-noverbose</td>
  <td align="center">Optional</td>
  <td>By default, the tool displays verbose messages indicating the progress of the segmentation and upload tasks. To suppress the display of these messages, use this option.</td>
</tr>
<tr>
  <td align="center"><i>file-or-files-or-directory</i></td>
  <td align="center">Required</td>
  <td>Full path and name of the file or directory that you want to upload.<br>
You can upload all the files in a directory by specifying the full path of the directory.<p>
To upload all the files in the current directory, specify "." (a period, without quotes).<br>
To upload all the files in a directory, specify the full path and name of the directory<br>
To upload multiple files: Specify the file names and separate the file names with ',' (a comma).<br>
This MUST be the last parameter that you specify.</td>
</tr>
</table>

## uploadcli 테스트
### Storage Cloud에 파일 Upload
uploadcli를 통해 571MB의 File을 upload한 테스트 결과입니다. 기존 53분이 걸리던 작업을 4분 41초에 처리할 수 있습니다.

- 그림 1. uploadcli에서 segmentation을 변경한 테스트 결과
![](https://oracloud-kr-teamrepo.github.io/2017/04/uploadcli/uploadcli-04.jpg)

### DBCS Instance에서 Storage Cloud의 파일 내려받기
DBCS Instance에서 "curl"을 이용하여 Storage Cloud에 upload된 dump file을 download합니다.

- Storage Cloud Authentication얻어오기

<pre class="prettyprint linenums">
curl -v -X GET -H "X-Storage-User: Storage-a425375:jaeyeop.lee@oracle.com" -H "X-Storage-Pass: H******@" \
     https://a425375.storage.oraclecloud.com/auth/v1.0

HTTP/1.1 200 OK
date: 1481594593964
X-Auth-Token: AUTH_tkf0e7a4ca4a704013a47b70a385373bfa
X-Storage-Token: AUTH_tkf0e7a4ca4a704013a47b70a385373bfa
....

</pre>


- Response 중 6라인의 ***X-Auth-Token*** 키를 인증키로하여 다음 REST API를 호출하여 Storage Cloud에 upload된 dump file 내려받습니다.

<pre class="prettyprint linenums">
curl -v -X GET -H "X-Auth-Token:AUTH_tkf0e7a4ca4a704013a47b70a385373bfa" \
     -o oscsa-OnPrem-16.3.1.0.13.tar.gz \
     https://a425375.storage.oraclecloud.com/v1/Storage-a425375/myContainer/oscsa-OnPrem-16.3.1.0.13.tar.gz
</pre>

위 curl 명령을 보면 1라인에 X-Auth-Token에 위에서 확보한 인증 키를 추가한 것을 확인할 수 있습니다.

- 그림 2. DBCS 인스턴스에서 Object Storage에 파일 내려받기.
![](https://oracloud-kr-teamrepo.github.io/2017/04/uploadcli/uploadcli-03.jpg)
