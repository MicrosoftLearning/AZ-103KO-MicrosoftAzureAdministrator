---
lab:
    title: '부하 분산 장치 및 Traffic Manager'
    module: '모듈 08 - 네트워크 트래픽 관리'
---

# 랩: 부하 분산 장치 및 Traffic Manager

이 랩의 모든 작업은 Azure Portal(PowerShell Cloud Shell 세션 포함)에서 수행됩니다(연습 1 작업 3 제외). 원격 데스크톱 세션에서 Azure VM에 이르는 여러 단계가 포함됩니다.

랩 파일: 

-  **Labfiles\\Module_08\\Load_Balancer_and_Traffic_Manager\\az-101-03_01_azuredeploy.json**

-  **Labfiles\\Module_08\\Load_Balancer_and_Traffic_Manager\\az-101-03_01_1_azuredeploy.parameters.json**

-  **Labfiles\\Module_08\\Load_Balancer_and_Traffic_Manager\\az-101-03_01_2_azuredeploy.parameters.json**

### 시나리오
  
Adatum Corporation은 Azure Load Balancer 의 부하 분산 및 NAT(Network Address Translation) 기능을 활용하여 Azure VM 호스팅 웹 워크로드를 구현하고 고가용성 방식으로 자회사 Contoso Corporation의 관리를 지원하고자 합니다.


### 목표
  
이 랩을 완료하면 다음과 같은 역량을 갖추게 됩니다.

-  Azure Resource Manager 템플릿을 사용하여 Azure VM 배포

-  Azure 부하 분산 구현

-  Azure Traffic Manager 부하 분산 구현


### 연습 0: Azure Resource Manager 템플릿을 사용하여 Azure VM 배포
  
이 랩의 주요 작업은 다음과 같습니다:

1. Azure Resource Manager 템플릿을 사용하여 첫 번째 Azure 지역의 가용성 집합에 설치된 웹 서버(IIS) 역할로 Windows Server 2016 데이터 센터를 실행하는 관리 Azure VM을 배포합니다.

1. Azure Resource Manager 템플릿을 사용하여 두 번째 Azure 지역의 가용성 집합에 설치된 웹 서버(IIS) 역할로 Windows Server 2016 데이터 센터를 실행하는 관리 Azure VM을 배포합니다.


#### 작업 1: Azure Resource Manager 템플릿을 사용하여 첫 번째 Azure 지역의 가용성 집합에 설치된 웹 서버(IIS) 역할로 Windows Server 2016 데이터 센터를 실행하는 관리 Azure VM을 배포합니다.

1. 랩 가상 머신에서 Microsoft Edge를 시작하고 Azure Portal([**http://portal.azure.com**](http://portal.azure.com))로 이동합니다. 그리고 대상 Azure 구독에서 owner 역할을 가진 Microsoft 계정을 사용하여 로그인합니다.

1. Azure Portal에서 **리소스 만들기** 블레이드로 이동합니다.

1. **리소스 만들기** 블레이드에서 Azure Marketplace에서 **템플릿 배포**를 검색합니다.

1. 검색 결과 목록을 사용하여 **사용자 지정 템플릿 배포** 블레이드로 이동합니다.

1. **사용자 지정 배포** 블레이드에서 **편집기에서 사용자 고유의 템플릿을 빌드합니다.** 링크를 클릭합니다. 이 링크가 표시되지 않으면 대신 **템플릿 편집**을 클릭합니다.

1. **편집 템플릿** 블레이드에서 템플릿 파일 **Labfiles\\Module_08\\Load_Balancer_and_Traffic_Manager\\az-101-03_01_azuredeploy.json**을 로드합니다. 

   > **참고**: 템플릿의 내용을 검토하세요. 해당 내용이 가용성 집합에 대한 Azure VM(Windows Server 2016 Datacenter Core 호스팅) 두 대의 배포를 정의한다는 점에 주목하세요.

1. 템플릿을 저장하고 **사용자 지정 배포** 블레이드로 돌아갑니다. 

1. **사용자 지정 배포** 블레이드에서 **매개 변수 편집** 블레이드로 이동합니다.

1. **편집 매개 변수** 블레이드에서 매개 변수 파일 **Labfiles\\Module_08\\Load_Balancer_and_Traffic_Manager\\az-101-03_01_1_azuredeploy.parameters.json**을 로드합니다. 

1. 매개 변수를 저장하고 **사용자 지정 배포 **블레이드로 돌아갑니다. 

1. **사용자 지정 배포** 블레이드에서 다음 설정을 사용해 템플릿 배포를 시작합니다.

    - 구독: 이 랩에서 사용하려는 구독의 이름

    - 리소스 그룹: 새 리소스 그룹 **az1010301-RG**의 이름

    - 위치: 랩 위치에 가장 가까운 Azure 지역의 이름 및 Azure VM을 프로비전할 수 있는 위치

    - 관리자 사용자 이름: **학생**

    - 관리자 암호: **Pa55w.rd1234**

    - VM 이름 접두사: **az1010301w-vm**

    - 애칭 접두사: **az1010301w-nic**

    - 이미지 게시자: **MicrosoftWindowsServer**

    - 이미지 제공: **WindowsServer**

    - 이미지 SKU: **2016-Datacenter**

    - VM 크기: 강사의 권장 사항에 따라 **Standard_DS1_v2** 또는 **Standard_DS2_v2**를 사용합니다.

    - 가상 네트워크 이름: **az1010301-vnet**

    - 주소 접두사: **10.101.31.0/24**

    - 가상 네트워크 리소스 그룹: **az1010301-RG**

    - 서브넷 이름: **subnet0**

    - 서브넷 접두사: **10.101.31.0/26**

    - 가용성 집합 이름: **az1010301w-avset**

    - 네트워크 보안 그룹 이름: **az1010301w-vm-nsg**

    - 모듈 URL: **https://github.com/Azure/azure-quickstart-templates/raw/master/dsc-extension-iis-server-windows-vm/ContosoWebsite.ps1.zip**

    - 구성 함수: **ContosoWebsite.ps1\\ContosoWebsite**

   > **참고**: Azure VM을 프로비전할 수 있는 Azure 지역을 확인하려면 [**https://azure.microsoft.com/ko-kr/regions/offers/**](https://azure.microsoft.com/ko-kr/regions/offers/)을 참고하십시오.

   > **참고**: 배포가 완료될 때까지 기다리지 말고 다음 작업을 진행합니다. 


#### 작업 2: Azure Resource Manager 템플릿을 사용하여 두 번째 Azure 지역의 가용성 집합에 설치된 웹 서버(IIS) 역할로 Windows Server 2016 데이터 센터를 실행하는 관리 Azure VM을 배포합니다.

1. Azure Portal에서 **리소스 만들기** 블레이드로 이동합니다.

1. **리소스 만들기** 블레이드에서 Azure Marketplace에서 **템플릿 배포**를 검색합니다.

1. 검색 결과 목록을 사용하여 **사용자 지정 템플릿 배포** 블레이드로 이동합니다.

1. **사용자 지정 배포** 블레이드에서 **편집기에서 사용자 고유의 템플릿을 빌드합니다.** 링크를 클릭합니다. 이 링크가 표시되지 않으면 대신 **템플릿 편집**을 클릭합니다.

1. **편집 템플릿** 블레이드에서 템플릿 파일 **Labfiles\\Module_08\\Load_Balancer_and_Traffic_Manager\\az-101-03_01_azuredeploy.json**을 로드합니다. 

   > **참고**: 이전 작업에서 사용한 템플릿과 동일합니다. 이를 사용하여 두 번째 지역에 Azure VM 쌍을 배포합니다. 

1. 템플릿을 저장하고 **사용자 지정 배포** 블레이드로 돌아갑니다. 

1. **사용자 지정 배포** 블레이드에서 **매개 변수 편집** 블레이드로 이동합니다.

1. **편집 매개 변수** 블레이드에서 매개 변수 파일 **Labfiles\\Module_08\\Load_Balancer_and_Traffic_Manager\\az-101-03_01_2_azuredeploy.parameters.json**을 로드합니다. 

1. 매개 변수를 저장하고 **사용자 지정 배포 **블레이드로 돌아갑니다. 

1. **사용자 지정 배포** 블레이드에서 다음 설정을 사용해 템플릿 배포를 시작합니다.

    - 구독: 이 랩에서 사용 중인 구독의 이름.

    - 리소스 그룹: 새 리소스 그룹 **az1010302-RG**의 이름.

    - 위치: Azure VM을 프로비전할 수 있으며 이전 작업에서 선택한 것과 다른 Azure 지역의 이름

    - 관리자 사용자 이름: **학생**

    - 관리자 암호: **Pa55w.rd1234**

    - VM 이름 접두사: **az1010302w-vm**

    - 애칭 접두사: **az1010302w-nic**

    - 이미지 게시자: **MicrosoftWindowsServer**

    - 이미지 제공: **WindowsServer**

    - 이미지 SKU: **2016-Datacenter**

    - VM 크기: 강사의 권장 사항에 따라 **Standard_DS1_v2** 또는 **Standard_DS2_v2**를 사용합니다.

    - 가상 네트워크 이름: **az1010302-vnet**

    - 주소 접두사: **10.101.32.0/24**

    - 가상 네트워크 리소스 그룹: **az1010302-RG**

    - 서브넷 이름: **subnet0**

    - 서브넷 접두사: **10.101.32.0/26**

    - 가용성 집합 이름: **az1010302w-avset**

    - 네트워크 보안 그룹 이름: **az1010302w-vm-nsg**

    - 모듈 URL: **https://github.com/Azure/azure-quickstart-templates/raw/master/dsc-extension-iis-server-windows-vm/ContosoWebsite.ps1.zip**

    - 구성 함수: **ContosoWebsite.ps1\\ContosoWebsite**

   > **참고**: 배포가 완료될 때까지 기다리지 말고 다음 연습을 진행합니다.

> **결과**: 이 연습에서는 Azure Resource Manager 템플릿을 사용하여 두 Azure 지역의 가용성 집합에 설치된 웹 서버(IIS) 역할을 사용하여 Windows Server 2016 데이터 센터를 실행하는 Azure VM의 배포를 시작했습니다.


### 연습 1: Azure 부하 분산 구현
  
이 연습의 주요 작업은 다음과 같습니다:

1. 첫 번째 지역에서 Azure 부하 분산 규칙을 구현합니다.

1. 두 번째 지역에서 Azure 부하 분산 규칙을 구현합니다.

1. 첫 번째 지역에서 Azure NAT 규칙을 구현합니다.

1. 두 번째 지역에서 Azure NAT 규칙을 구현합니다.

1. Azure 부하 분산 및 NAT 규칙을 확인합니다.


#### 작업 1: 첫 번째 지역에서 Azure 부하 분산 규칙 구현

   > **참고**: 이 작업을 시작하기 전에 이전 연습의 첫 번째 작업에서 시작한 템플릿 배포가 완료되었는지 확인합니다. 

1. Azure Portal에서 **리소스 만들기** 블레이드로 이동합니다.

1. **리소스 만들기** 블레이드의 Azure Marketplace에서 **부하 분산 장치**를 검색하십시오.

1. 검색 결과 목록을 사용하여 **부하 분산 장치 만들기** 블레이드로 이동하십시오.

1. **부하 분산 장치 만들기** 블레이드에서 다음 설정으로 새 Azure Load Balancer 를 만듭니다:

    - 이름: **az1010301w-lb**

    - 유형: **공용**

    - SKU: **기본**

    - 공용 IP 주소: **az1010301w-lb-pip**라는 이름의 새 공용 IP 주소

    - 할당: **동적**

    - 구독: 이 랩에서 사용 중인 구독의 이름

    - 리소스 그룹: **az1010301-RG**

    - 위치: 이전 연습의 첫 번째 작업에서 Azure VM을 배포한 Azure 지역의 이름

1. Azure Portal에서 새로 배포된 Azure Load Balancer **az1010301w-lb** 블레이드로 이동합니다.

1. **az1010301w-lb** 블레이드에서 **az1010301w-lb - 백엔드 풀** 블레이드를 표시합니다.

1. **az1010301w-lb - 백엔드 풀** 블레이드에서 다음 설정으로 백엔드 풀을 추가합니다:

    - 이름: **az1010301w-bepool**

    - IP 버전: **IPv4**

    - 관련: **가용성 집합**

    - 가용성 집합: **az1010301w-avset**

    - 가상 머신: **az1010301w-vm0** 

    - 네트워크 IP 구성: **az1010301w-nic0/ipconfig1 (10.101.31.4)**

    - 가상 머신: **az1010301w-vm1** 

    - 네트워크 IP 구성: **az1010301w-nic1/ipconfig1 (10.101.31.5)**

   > **참고**: Azure VM의 IP 주소를 역순으로 할당할 수 있습니다. 

   > **참고**: 작업이 완료될 때까지 기다립니다. 이 작업에는 1분 미만이 소요됩니다.

1. **az1010301w-lb - 백엔드 풀** 블레이드에서 **az1010301w-lb - 상태 프로브** 블레이드를 표시합니다.

1. **az1010301w-lb - 상태 프로브** 블레이드에서 다음 설정으로 상태 프로브를 추가합니다.

    - 이름: **az1010301w-healthprobe**

    - 프로토콜: **TCP**

    - 포트: **80**

    - 간격: **5**초

    - 비정상 임계값: **2**회 연속 실패

    > **참고**: 작업이 완료될 때까지 기다립니다. 이 작업에는 1분 미만이 소요됩니다.

1. **az1010301w-lb - 상태 프로브** 블레이드에서 **az1010301w-lb - 부하 분산 규칙** 블레이드를 표시합니다.

1. **az1010301w-lb - 부하 분산 규칙** 블레이드에서 다음 설정으로 부하 분산 규칙을 추가합니다.

    - 이름: **az1010301w-lbrule01**

    - IP 버전: **IPv4**

    - 프런트 엔드 IP 주소: **LoadBalancerFrontEnd**

    - 프로토콜: **TCP**

    - 포트: **80**

    - 백엔드 포트: **80**

    - 백엔드 풀: **az1010301w-bepool (가상 머신 2개)**

    - 상태 프로브: **az1010301w-healthprobe (TCP:80)**

    - 세션 지속성: **없음**

    - 유휴 시간 초과(분): **4**

    - 부동 IP(직접 서버 반환): **사용 중지**


#### 작업 2: 두 번째 지역에서 Azure 부하 분산 규칙 구현

   > **참고**: 이 작업을 시작하기 전에 이전 연습의 두 번째 작업에서 시작한 템플릿 배포가 완료되었는지 확인합니다. 

1. Azure Portal에서 **리소스 만들기** 블레이드로 이동합니다.

1. **리소스 만들기** 블레이드의 Azure Marketplace에서 **부하 분산 장치**를 검색하십시오.

1. 검색 결과 목록을 사용하여 **부하 분산 장치 만들기** 블레이드로 이동하십시오.

1. **부하 분산 장치 만들기** 블레이드에서 다음 설정으로 새 Azure Load Balancer 를 만듭니다:

    - 이름: **az1010302w-lb**

    - 유형: **공용**

    - SKU: **기본**

    - 공용 IP 주소: **az1010302w-lb-pip**라는 이름의 새 공용 IP 주소

    - 할당: **동적**

    - 구독: 이 랩에서 사용 중인 구독의 이름

    - 리소스 그룹: **az1010302-RG**

1. Azure Portal에서 새로 배포된 Azure Load Balancer **az1010302w-lb** 블레이드로 이동합니다.

1. **az1010302w-lb** 블레이드에서 **az1010302w-lb - 백엔드 풀** 블레이드를 표시합니다.

1. **az1010302w-lb - 백엔드 풀** 블레이드에서 다음 설정으로 백엔드 풀을 추가합니다:

    - 이름: **az1010302w-bepool**

    - IP 버전: **IPv4**

    - 관련: **가용성 집합**

    - 가용성 집합: **az1010302w-avset**

    - 가상 머신: **az1010302w-vm0** 

    - 네트워크 IP 구성: **az1010302w-nic0/ipconfig1 (10.101.32.4)**

    - 가상 머신: **az1010302w-vm1** 

    - 네트워크 IP 구성: **az1010302w-nic1/ipconfig1 (10.101.32.5)**

   > **참고**: Azure VM의 IP 주소를 역순으로 할당할 수 있습니다. 

   > **참고**: 작업이 완료될 때까지 기다립니다. 이 작업에는 1분 미만이 소요됩니다.

1. **az1010302w-lb - 백엔드 풀** 블레이드에서 **az1010302w-lb - 상태 프로브** 블레이드를 표시합니다.

1. **az1010302w-lb - 상태 프로브** 블레이드에서 다음 설정으로 상태 프로브를 추가합니다.

    - 이름: **az1010302w-healthprobe**

    - 프로토콜: **TCP**

    - 포트: **80**

    - 간격: **5**초

    - 비정상 임계값: **2**회 연속 실패

    > **참고**: 작업이 완료될 때까지 기다립니다. 이 작업에는 1분 미만이 소요됩니다.

1. **az1010302w-lb - 상태 프로브** 블레이드에서 **az1010302w-lb - 부하 분산 규칙** 블레이드를 표시합니다.

1. **az1010302w-lb - 부하 분산 규칙** 블레이드에서 다음 설정으로 부하 분산 규칙을 추가합니다.

    - 이름: **az1010302w-lbrule01**

    - IP 버전: **IPv4**

    - 프런트 엔드 IP 주소: **LoadBalancerFrontEnd**

    - 프로토콜: **TCP**

    - 포트: **80**

    - 백엔드 포트: **80**

    - 백엔드 풀: **az1010302w-bepool (가상 머신 2개)**

    - 상태 프로브: **az1010302w-healthprobe (TCP:80)**

    - 세션 지속성: **없음**

    - 유휴 시간 초과(분): **4**

    - 부동 IP(직접 서버 반환): **사용 중지**


#### 작업 3: 첫 번째 지역에서 Azure NAT 규칙 구현

1. Azure Portal에서 Azure Load Balancer **az1010301w-lb** 블레이드로 이동합니다.

1. **az1010301w-lb** 블레이드에서 **az1010301w-lb - 인바운드 NAT 규칙** 블레이드를 표시합니다.

   > **참고**: NAT 기능은 상태 프로브에 의존하지 않습니다.

1. **az1010301w-lb - 인바운드 NAT 규칙** 블레이드에서 다음 설정으로 첫 번째 인바운드 NAT 규칙을 추가합니다:

    - 이름: **az1010301w-vm0-RDP**

    - 프런트 엔드 IP 주소: **LoadBalancerFrontEnd**

    - IP 버전: **IPv4**

    - 서비스: **사용자 지정**

    - 프로토콜: **TCP**

    - 포트: **33890**

    - 대상 가상 머신: **az1010301w-vm0**

    - 네트워크 IP 구성: **ipconfig1 (10.101.31.4)** 또는 **ipconfig1 (10.101.31.5)**

    - 포트 매핑: **사용자 지정**

    - 부동 IP(직접 서버 반환): **사용 중지**

    - 대상 포트: **3389**

    > **참고**: 작업이 완료될 때까지 기다립니다. 이 작업에는 1분 미만이 소요됩니다.

1. **az1010301w-lb - 인바운드 NAT 규칙** 블레이드에서 다음 설정으로 두 번째 인바운드 NAT 규칙을 추가합니다:

    - 이름: **az1010301w-vm1-RDP**

    - 프런트 엔드 IP 주소: **LoadBalancerFrontEnd**

    - IP 버전: **IPv4**

    - 서비스: **사용자 지정**

    - 프로토콜: **TCP**

    - 포트: **33891**

    - 대상 가상 머신: **az1010301w-vm1**

    - 네트워크 IP 구성: **ipconfig1 (10.101.31.4)** 또는 **ipconfig1 (10.101.31.5)**

    - 포트 매핑: **사용자 지정**

    - 부동 IP(직접 서버 반환): **사용 중지**

    - 대상 포트: **3389**

    > **참고**: 작업이 완료될 때까지 기다립니다. 이 작업에는 1분 미만이 소요됩니다.


#### 작업 4: 두 번째 지역에서 Azure NAT 규칙 구현

1. Azure Portal에서 Azure Load Balancer **az1010302w-lb** 블레이드로 이동합니다.

1. **az1010302w-lb** 블레이드에서 **az1010302w-lb - 인바운드 NAT 규칙** 블레이드를 표시합니다.

1. **az1010302w-lb - 인바운드 NAT 규칙** 블레이드에서 다음 설정으로 첫 번째 인바운드 NAT 규칙을 추가합니다:

    - 이름: **az1010302w-vm0-RDP**

    - 프런트 엔드 IP 주소: **LoadBalancedFrontEnd**

    - IP 버전: **IPv4**

    - 서비스: **사용자 지정**

    - 프로토콜: **TCP**

    - 포트: **33890**

    - 대상 가상 머신: **az1010302w-vm0**

    - 네트워크 IP 구성: **ipconfig1 (10.101.32.4)** 또는 **ipconfig1 (10.101.32.5)**

    - 포트 매핑: **사용자 지정**

    - 부동 IP(직접 서버 반환): **사용 중지**

    - 대상 포트: **3389**

    > **참고**: 작업이 완료될 때까지 기다립니다. 이 작업에는 1분 미만이 소요됩니다.

1. **az1010302w-lb - 인바운드 NAT 규칙** 블레이드에서 다음 설정으로 두 번째 인바운드 NAT 규칙을 추가합니다:

    - 이름: **az1010302w-vm1-RDP**

    - 프런트 엔드 IP 주소: **LoadBalancedFrontEnd**

    - IP 버전: **IPv4**

    - 서비스: **사용자 지정**

    - 프로토콜: **TCP**

    - 포트: **33891**

    - 대상 가상 머신: **az1010302w-vm1**

    - 네트워크 IP 구성: **ipconfig1 (10.101.32.4)** 또는 **ipconfig1 (10.101.32.5)**

    - 포트 매핑: **사용자 지정**

    - 부동 IP(직접 서버 반환): **사용 중지**

    - 대상 포트: **3389**

    > **참고**: 작업이 완료될 때까지 기다립니다. 이 작업에는 1분 미만이 소요됩니다.


#### 작업 5: Azure 부하 분산 및 NAT 규칙을 확인합니다.

1. Azure Portal에서 Azure Load Balancer **az1010301w-lb** 블레이드로 이동합니다.

1. **az1010301w-lb** 블레이드에서 부하 분산 장치 프런트 엔드에 할당된 공용 IP 주소를 식별합니다.

1. Microsoft Edge 창에서 새 탭을 열고 이전 단계에서 식별한 IP 주소를 찾아보세요.

1. 탭에 기본 인터넷 정보 서비스 홈페이지가 표시되는지 확인합니다.

1. 기본 인터넷 정보 서비스 홈페이지를 표시하는 브라우저 탭을 닫습니다.

1. Azure Portal에서 Azure Load Balancer **az1010301w-lb** 블레이드로 이동합니다.

1. **az1010301w-lb** 블레이드에서 부하 분산 장치 프런트 엔드에 할당된 공용 IP 주소를 식별합니다.

1. 랩 가상 머신에서 &lt;az1010301w-lb_public_IP&lt; 자리 표시자를 이전 작업에서 식별한 IP 주소로 교체하고 다음 명령을 실행합니다:

```
   mstsc /v:<az1010301w-lb_public_IP>:33890
```

   > **참고**: 이 명령은 이전 작업에서 만든 **az1010301w-vm0-RDP** NAT 규칙을 사용하여 **az1010301w-vm0** Azure VM에 대한 원격 데스크톱 세션을 시작합니다.

1. 로그인 화면이 표시되면 아래 자격 증명을 입력하십시오:

    - 관리자 사용자 이름: **학생**

    - 관리자 암호: **Pa55w.rd1234**

1. 로그인한 후 명령 프롬프트에서 다음 명령을 실행합니다:

```
   hostname
```

1. 출력을 검토하고 **az1010301w-vm0** Azure VM에 실제로 연결되어 있는지 확인합니다.

   > **참고**: 다음 지역에서도 같은 테스트를 반복하십시오.

> **결과**: 두 Azure 지역에서 Azure의 NAT 규칙 및 부하 분산 규칙을 구현하고, 첫 번째 지역에서 Azure Load Balancer의 NAT 규칙과 부하 분산 규칙을 확인하여 이 연습을 완료해야 합니다.


### 연습 2: Azure Traffic Manager 부하 분산 구현

이 연습의 주요 작업은 다음과 같습니다:

1. Azure Load Balancer의 공용 IP 주소에 DNS 이름 할당

1. Azure Traffic Manager 부하 분산 구현

1. Azure Traffic Manager 부하 분산 확인


#### 작업 1: Azure Load Balancer의 공용 IP 주소에 DNS 이름 할당

   > **참고**: 이는 꼭 필요한 작업입니다. 각 Traffic Manager 엔드포인트에는 DNS 이름이 할당되어야 하기 때문입니다. 

1. Azure Portal의 첫 번째 지역(이름: **az1010301w-lb-pip**)에서 Azure Load Balancer와 연결된 공용 IP 주소 리소스 블레이드로 이동합니다. 

1. **az1010301w-lb-pip** 블레이드에서 **구성** 블레이드를 표시합니다. 

1. **az1010301w-lb-pip - 구성** 블레이드에서 공용 IP 주소의 **DNS 이름 레이블**을 고유한 값으로 설정합니다. 

   > **참고**: **DNS 이름 레이블(선택 사항)** 텍스트 상자의 녹색 확인 표시는 입력한 이름이 유효하고 고유한지 나타냅니다. 

1. **az1010302w-lb-pip**라는 이름의 두 번째 지역에서 Azure Load Balancer와 연결된 공용 IP 주소 리소스 블레이드로 이동합니다. 

1. **az1010302w-lb-pip** 블레이드에서 **구성** 블레이드를 표시합니다. 

1. **az1010302w-lb-pip - 구성** 블레이드는 공용 IP 주소의 **DNS 이름 레이블**을 고유한 값으로 설정합니다. 

   > **참고**: **DNS 이름 레이블(선택 사항)** 텍스트 상자의 녹색 확인 표시는 입력한 이름이 유효하고 고유한지 나타냅니다. 


#### 작업 2: Azure Traffic Manager 부하 분산 구현

1. Azure Portal에서 **리소스 만들기** 블레이드로 이동합니다.

1. **리소스 만들기** 블레이드의 Azure Marketplace에서 **Traffic Manager 프로필**을 검색하십시오.

1. 검색 결과 목록을 사용하여 **Traffic Manager 프로필 만들기** 블레이드로 이동하세요.

1. **Traffic Manager 프로필 만들기** 블레이드에서 다음 설정으로 새 Azure Traffic Manager 프로필을 만듭니다.

    - 이름: trafficmanager.net DNS 네임스페이스의 전역적으로 고유한 이름

    - 라우팅 방법: **가중치 적용**

    - 구독: 이 랩에서 사용 중인 구독의 이름

    - 리소스 그룹: 새 리소스 그룹 **az1010303-RG**의 이름

    - 위치: 이 랩의 앞에서 사용한 Azure 지역 중 하나

1. Azure Portal에서 새로 프로비저닝된 Traffic Manager 프로필의 블레이드로 이동합니다.

1. Traffic Manager 프로필 블레이드에서 **구성** 블레이드를 표시하고 구성 설정을 검토합니다.

   > **참고**: Traffic Manager 프로필 DNS 레코드의 기본 TTL은 60초입니다.

1. Traffic Manager 프로필 블레이드에서 **끝점** 블레이드가 표시됩니다.

1. **끝점** 블레이드에서 다음 설정으로 첫 번째 끝점을 추가합니다.

    - 유형: **Azure 엔드포인트**

    - 이름: **az1010301w-lb-pip**

    - 대상 리소스 종류: **공용 IP 주소**

    - 대상 리소스: **az1010301w-lb-pip**

    - 가중치: **100**

    - 사용자 지정 헤더 설정: 비워둡니다.

    - 사용 중지로 추가: 비워둡니다.

1. **끝점** 블레이드에서 다음 설정으로 두 번째 끝점을 추가합니다.

    - 유형: **Azure 엔드포인트**

    - 이름: **az1010302w-lb-pip**

    - 대상 리소스 종류: **공용 IP 주소**

    - 대상 리소스: **az1010302w-lb-pip**

    - 가중치: **100**

    - 사용자 지정 헤더 설정: 비워둡니다.

    - 사용 중지로 추가: 비워둡니다.

1. **끝점** 블레이드에서 두 끝점에 대한 **모니터링 상태** 열의 항목을 검사합니다. 둘 모두 **온라인** 목록에 포함될 때까지 기다린 후 다음 작업을 진행합니다.


#### 작업 3: Azure Traffic Manager 부하 분산 확인

1. **끝점** 블레이드에서 Traffic Manager 프로필 블레이드의 **개요** 섹션으로 전환합니다.

1. Traffic Manager 프로필에 할당된 DNS 이름(**http://** 접두사 다음의 문자열)을 기록합니다. 

1. Azure Portal의 Cloud Shell 창에서 PowerShell 세션을 시작하십시오. 

   > **참고**:  현재의 Azure 구독에서 Cloud Shell을 처음 시작하는 경우, Cloud Shell 파일을 유지하도록 Azure 파일 공유를 만들라는 메시지가 표시됩니다. 이 경우 기본값을 허용하면 자동으로 생성된 리소스 그룹에서 스토리지 계정이 생성됩니다.

1. Cloud Shell 창에서 다음 명령을 실행하여, &lt;TM_DNS_name&lt; 자리 표시자를 이전 작업에서 식별한 Traffic Manager 프로필에 할당된 DNS 이름 값으로 바꿉니다.

```
   nslookup <TM_DNS_name>
```

1. 출력을 검토하고 **이름** 항목을 기록합니다. 이전 작업에서 만든 Traffic Manager 프로필 엔드포인트 중 하나의 DNS 이름과 일치해야 합니다.

1. 60초 이상 기다린 후 동일한 명령을 다시 실행합니다:

```
   nslookup <TM_DNS_name>
```
1. 출력을 검토하고 **이름** 항목을 기록합니다. 이번에는 이전 작업에서 만든 다른 Traffic Manager 프로필 엔드포인트의 DNS 이름과 항목이 일치해야 합니다.

> **결과**: 이 연습에서는 Azure Traffic Manager 부하 분산을 구현하고 확인했습니다.

## 연습 3: 랩 리소스 제거

#### 작업 1: Cloud Shell 열기

1. 포털 상단에서 **Cloud Shell** 아이콘을 클릭하여 Cloud Shell 창을 엽니다.

1. Cloud Shell 인터페이스에서 **Bash**를 선택합니다.

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 이 랩에서 만든 모든 리소스 그룹을 나열합니다.

```sh
   az group list --query "[?starts_with(name,'az101030')].name" --output tsv
```

1. 이 랩에서 만든 리소스 그룹만 출력에 포함되어 있는지 확인합니다. 다음 태스크에서 이러한 그룹을 삭제합니다.

#### 작업 2: 리소스 그룹 삭제

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 이 랩에서 만든 리소스 그룹을 삭제합니다.

```sh
   az group list --query "[?starts_with(name,'az101030')].name" --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
```

1. 포털 하단에 있는 **Cloud Shell** 프롬프트를 닫습니다.

> **결과**: 이 연습에서는 이 랩에서 사용한 리소스를 제거했습니다.
