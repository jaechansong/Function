# Function

## 1. Repo에 대한 설명
##### 이 Reop에는 정통 팀에서 제공하는 서비스들에 대해 꼭 필요한 기능들을 개발한 Code들이 있다.
### Code들은 주로 Python으로 개발되었으며, Python 외에도 C, Shell 언어 등이 사용되었다.
### 제공하는 기능으로는 '사용자의 요구에 적합한 Cloud 서버 환경 분석', '분석한 Cloud 서버에 대해 CSP에서 제공하는 서버 정보 실시간 Crawling', 'Zabbix Database 자동화', 'IasS 모니터링을 위한 Agent 자동화 설치', 'IasS 보안 검사 설치 자동화', '바이러스 검사 결과 실시간 송수신(소켓)' 등이 있다.
### 이 Repo에서 개발한 기능들을 Django 기반의 "NBP_back" Repo와 "OIDC_FRONT" Repo와 연동하여 사용한다.



Crawling - 실제 사용할 땐 crawling_return.py 사용

Agent 자동화 설치 - 실제 사용할 땐 agent_install_bat.py 사용
 
ClamAV(보안), Socket 통신 Setting 자동화 설치 - 실제 사용할 떈 security_bat.py 사용 



Batch 파일을 활용한 Zabbix Monitoring Agent 설치 자동화


- agent, ClamAV, Socket Setting 자동화 설치 방법

vm/cloud server에서 

'yum install python3' 하고

'python3 -m pip install boto3' 한 뒤에

S3에 있는 해당 파일 url(ex.https://github.com/OIDC-JT/Function/blob/master/agent_install_test.py)을 서버에 깔고

py 파일을 실행 시키고(ex.'python3 agent_install_test.py')


---위 code는 우리 django 서버에서 실행/아래코드는 고객 서버에서 실행---

#1. 서버에서 curl -O(centos), wget(ubuntu) 's3 url' 을 입력하여 bat 파일 다운로드

#2. chmod 755 'bat file명' -->bat file 권한을 access 할 수 있게 변경

#3. ./'file명.bat'으로 실행(agent 설치&설정)

하면 vm/cloud server에 agent 설치 및 설정이 완료되서 passta로 바로 모니터링 가능

+보안 실시간 점검(매일 00시 00분)이 가능하며 점검 결과를 Django Server로 (매일 00시 10분에)소켓 전송 함

(백그라운드에서 'nohup,  ./file &' 기능 사용-터미널이 꺼져도 언제든 수신할 수 있게)

-----------------------------------------------------------------------------------

방식 설명(백엔드)

1. crawwling_return.py(Server Migration Helper) - 사용자가 요구한 서버 환경에 가장 적합한 서버를 각 CSP 사이트에서 서버 정보를 크롤링함

2. agent_install_bat.py(IaaS Hybrid monitoring Service) - 사용자가 서버 추가 시 입력받은 (ID, OS, Server ID)를 해당 함수에 대입하여 적합한 batch 자동화 파일을 S3에 업로드함

3. security_bat.py(IaaS Security Management Service) - 사용자가 서버 추가시 입력받은 (OS와 Server ID)를 해당 함수에 대입하여 적합한 batch 자동화 파일과 Socket Setting Code C파일을 S3에 업로드함
