---
lab:
    title: '로드 밸런서 및 Traffic Manager'
    module: '모듈 08 - 네트워크 Traffic Manager'
---

# 랩: 로드 밸런서 및 Traffic Manager

이 실습의 모든 작업은 Azure 포털 (PowerShell 클라우드 셸 세션 포함)에서 수행됩니다 (실습 1 작업 3 제외). 원격 데스크톱 세션에서 Azure VM까지 수행되는 단계가 포함됩니다

랩 파일: 

-  **Labfiles\\Module_08\\Load_Balancer_and_Traffic_Manager\\az-101-03_01_azuredeploy.json**

-  **Labfiles\\Module_08\\Load_Balancer_and_Traffic_Manager\\az-101-03_01_1_azuredeploy.parameters.json**

-  **Labfiles\\Module_08\\Load_Balancer_and_Traffic_Manager\\az-101-03_01_2_azuredeploy.parameters.json**

### 시나리오
  
Adatum Corporation은 Azure Load Balancer 의 부하 분산 및 NAT(네트워크 주소 변환) 기능을 활용하여 자회사 Contoso Corporation의 관리를 가용성 이면에 맞게 구현하고자 합니다. 


### 목표
  
이 과정을 완료하면 다음과 같은 역량을 갖추게 됩니다:

-  Azure Resource Manager 템플릿을 사용하여 Azure VM 배포

-  Azure 부하 분산 구현

-  Azure Traffic Manager 부하 분산 조정 구현


### 연습 0: Azure Resource Manager 템플릿을 사용하여 Azure VM 배포
  
이 연습의 주요 작업은 다음과 같습니다:

1. Azure Resource Manager 템플릿을 사용하여 첫 번째 Azure 리전에서 가용성 집합에 설치된 IIS(웹 서버) 역할을 사용하여 Windows Server 2016 데이터 센터를 실행하는 관리 Azure VM배포

1. Azure Resource Manager 템플릿을 사용하여 두 번째 Azure 지역의 가용성 집합에 설치된 IIS(웹 서버) 역할을 사용하여 Windows Server 2016 데이터 센터를 실행하는 관리 Azure VM을 배포합니다.


#### 작업 1: Azure Resource Manager 템플릿을 사용하여 첫 번째 Azure 리전에서 가용성 집합에 설치된 IIS(웹 서버) 역할을 사용하여 Windows Server 2016 데이터 센터를 실행하는 관리 Azure VM배포

1. 랩 가상 머신에서 Microsoft Edge를 시작하고 [**http://portal.azure.com**](http://portal.azure.com) 에서 Azure 포털을 탐색하고 대상 Azure 가입에서 소유자 역할을 가진 Microsoft 계정을 사용하여 로그인합니다.

1. Azure 포털에서 **리소스 만들기** 블레이드로 이동합니다.

1. **리소스 만들기** 블레이드가 뜨면, Azure 마켓플레이스에서 **Template deployment**를 검색합니다. 그리고 **Template deployment (deploy using custom templates)**를 선택합니다.

1. **만들기**를 클릭합니다.
 
1. **사용자 지정 배포** 블레이드에서 **편집기에서 사용자 고유의 템플릿을 빌드합니다.** 를 선택합니다.

1. **템플릿 편집** 블레이드에서 템플릿 파일인 **Labfiles\\Module_08\\Load_Balancer_and_Traffic_Manager\\az-101-03_01_azuredeploy.json** 을 로드합니다. 

   > **참고**: 템플릿의 내용을 검토하고 Windows Server 2016 Datacenter Core를 호스팅하는 Azure VM 두 대를 가용성 집합에 배포하는 것을 정의한다는 점에 유의하십시오.

1. 템플릿을 저장하고 **사용자 지정 배포** 블레이드로 돌아갑니다. 

1. **사용자 지정 배포** 블레이드에서 **매개 변수 편집** 블레이드로 이동합니다.

1. **편집 매개 변수** 블레이드에서 매개 변수 파일 **Labfiles\\Module_08\\Load_Balancer_and_Traffic_Manager\\az-101-03_01_1_azuredeploy.parameters.json**을 로드합니다.

1. 매개 변수를 저장하고 **사용자 지정 배포** 블레이드로 돌아갑니다. 

1. **사용자 지정 배포** 블레이드에서 다음을 사용하여 템플릿 배포를 시작합니다.

    - 구독: 이 랩에서 사용하려는 구독의 이름

    - 리소스 그룹: **az1010301-RG** 이름으로 새로 만들기

    - 지역: 랩 위치에 가장 가까운 Azure 지역의 이름 및 Azure VM을 프로비전할 수 있는 지역

    - Admin Username: **Student**

    - Admin Password: **Pa55w.rd1234**

    - Vm Name Prefix: **az1010301w-vm**

    - Nic Name Prefix: **az1010301w-nic**

    - Image Publisher: **MicrosoftWindowsServer**

    - Image Offer: **WindowsServer**

    - Image SKU: **2016-Datacenter**

    - Vm Size: **Standard_D2s_v3**

    - Virtual Network Name: **az1010301-vnet**

    - Address Prefix: **10.101.31.0/24**

    - Virtual Network Resource Group: **az1010301-RG**

    - Subnet0Name: **subnet0**

    - Subnet0Prefix: **10.101.31.0/26**

    - Availability Set Name: **az1010301w-avset**

    - Network Security Group Name: **az1010301w-vm-nsg**

    - Modules Url: **https://github.com/Azure/azure-quickstart-templates/raw/master/dsc-extension-iis-server-windows-vm/ContosoWebsite.ps1.zip**

    - Configuration Function: **ContosoWebsite.ps1\\ContosoWebsite**

   > **참고**: Azure VM을 프로비전할 수 있는 Azure 지역을 식별하려면 [**https://azure.microsoft.com/ko-kr/regions/offers/**](https://azure.microsoft.com/ko-kr/regions/offers/)참고하십시오.

   > **참고**: 배포가 완료될 때까지 기다리지 말고 다음 작업으로 진행합니다. 


#### 작업 2: Azure Resource Manager 템플릿을 사용하여 두 번째 Azure 지역의 가용성 집합에 설치된 IIS(웹 서버) 역할을 사용하여 Windows Server 2016 데이터 센터를 실행하는 관리 Azure VM을 배포합니다.

1. Azure 포털에서 **리소스 만들기** 블레이드로 이동합니다.

1. **리소스 만들기** 블레이드가 뜨면, Azure 마켓플레이스에서 **Template deployment**를 검색합니다. 그리고 **Template deployment (deploy using custom templates)**를 선택합니다.

1. **만들기**를 클릭합니다.
 
1. **사용자 지정 배포** 블레이드에서 **편집기에서 사용자 고유의 템플릿을 빌드합니다.** 를 선택합니다.

1. **템플릿 편집** 블레이드에서 템플릿 파일인 **Labfiles\\Module_08\\Load_Balancer_and_Traffic_Manager\\az-101-03_01_azuredeploy.json** 을 로드합니다. 

   > **참고**: 이전 작업에서 사용한 템플릿과 동일합니다. 이를 사용하여 두 번째 지역에 Azure VM 쌍을 배포합니다. 

1. 템플릿을 저장하고 **사용자 지정 배포** 블레이드로 돌아갑니다. 

1. **사용자 지정 배포** 블레이드에서 **매개 변수 편집** 블레이드로 이동합니다.

1. **편집 매개 변수** 블레이드에서 매개 변수 파일 **Labfiles\\Module_08\\Load_Balancer_and_Traffic_Manager\\az-101-03_01_2_azuredeploy.parameters.json**을 로드합니다.

1. 매개 변수를 저장하고 **사용자 지정 배포** 블레이드로 돌아갑니다. 

1. **사용자 지정 배포** 블레이드에서 다음을 사용하여 템플릿 배포를 시작합니다.

    - 구독: 이 랩에서 사용 중인 구독의 이름

    - 리소스 그룹: 새 리소스 그룹 **az1010302-RG** 의 이름

    - 지역 : 이전 작업에서 선택한 것과 다른 Azure 영역의 이름과 Azure VM을 준비 할 수있는 지역

    - Admin Username: **Student**

    - Admin Password: **Pa55w.rd1234**

    - Vm Name Prefix: **az1010302w-vm**

    - Nic Name Prefix: **az1010302w-nic**

    - Image Publisher: **MicrosoftWindowsServer**

    - Image Offer: **WindowsServer**

    - Image SKU: **2016-Datacenter**

    - Vm Size: **Standard_D2s_v3**

    - Virtual Network Name: **az1010302-vnet**

    - Address Prefix: **10.101.32.0/24**

    - Virtual Network Resource Group: **az1010302-RG**

    - Subnet0Name: **subnet0**

    - Subnet0Prefix: **10.101.32.0/26**

    - Availability Set Name: **az1010302w-avset**

    - Network Security Group Name: **az1010302w-vm-nsg**

    - Modules Url: **https://github.com/Azure/azure-quickstart-templates/raw/master/dsc-extension-iis-server-windows-vm/ContosoWebsite.ps1.zip**

    - Configuration Function: **ContosoWebsite.ps1\\ContosoWebsite**

   > **참고**: 배포가 완료될 때까지 기다리지 말고 다음 연습으로 진행합니다.

> **결과**: 이 연습을 완료한 후 Azure Resource Manager 템플릿을 사용하여 두 Azure 지역의 가용성 집합에 설치된 IIS(웹 서버) 역할을 사용하여 Windows Server 2016 데이터 센터를 실행하는 Azure VM의 배포를 시작했습니다.


### 연습 1: Azure 부하 분산 구현
  
이 연습의 주요 작업은 다음과 같습니다:

1. 첫 번째 리전에서 Azure 부하 분산 규칙을 구현합니다.

1. 두 번째 리전에서 Azure 부하 분산 규칙을 구현합니다.

1. 첫 번째 리전에서 Azure NAT 규칙을 구현합니다.

1. 두 번째 리전에서 Azure NAT 규칙을 구현합니다.

1. Azure 부하 분산 및 NAT 규칙 확인


#### 작업 1: 첫 번째 리전에서 Azure 부하 분산 규칙 구현

   > **참고**: 이 작업을 시작하기 전에 이전 연습의 첫 번째 작업에서 시작한 템플릿 배포가 완료되었는지 확인합니다. 

1. Azure 포털에서 **리소스 만들기** 블레이드로 이동합니다.

1. **리소스 만들기** 블레이드가 뜨면, Azure 마켓플레이스에서 **Load Balancer** 를 검색하십시오. 검색 결과 목록에서 **부하 분산 장치**를 선택합니다.

1. 검색 결과 목록을 사용하여로드 **부하 분산 장치 만들기** 블레이드로 이동하십시오.

1. **부하 분산 장치 만들기** 블레이드에서 다음 설정으로 새 부하 분산 장치를 만듭니다:

    - 이름: **az1010301w-lb**

    - 형식: **공개**

    - SKU: **기본**

    - 공용 IP 주소: **az1010301w-lb-pip**로 새로 만들기

    - 할당: **동적**

    - 구독: 이 랩에서 사용 중인 구독의 이름

    - 리소스 그룹: **az1010301-RG**

    - 지역: 이전 연습의 첫 번째 작업에 Azure VM을 배포한 Azure 영역의 이름입니다

1. Azure 포털에서 새로 배포 된 부하 분산 장치인 **az1010301w-lb** 의 블레이드로 이동합니다.

1. **az1010301w-lb** 블레이드에서 **az1010301w-lb - 백 엔드 풀** 블레이드로 이동합니다.

1. **az1010301w-lb - 백 엔드 풀** 블레이드에서 다음 설정으로 백 엔드 풀을 추가합니다:

    - 이름: **az1010301w-bepool**

    - IP 버전: **IPv4**

    - 다음에 연결됨: **가용성 집합**

    - 가용성 집합: **az1010301w-avset**

    - 가상 머신: **az1010301w-vm0** 

    - 네트워크 IP 구성: **az1010301w-nic0/ipconfig1 (10.101.31.4)**

    - 가상 머신: **az1010301w-vm1** 

    - 네트워크 IP 구성: **az1010301w-nic1/ipconfig1 (10.101.31.5)**

   > **참고**: Azure VM의 IP 주소가 역순으로 할당될 수 있습니다. 

   > **참고**: 작업이 완료될 때까지 기다립니다. 이 작업은 1 분 미만이 소요됩니다.

1. **az1010301w-lb - 백 엔드 풀** 블레이드에서 **az1010301w-lb - 상태 프로브** 블레이드로 이동합니다.

1. **az1010301w-lb - 상태 프로브** 블레이드에서 다음 설정으로 상태 프로브를 추가합니다.

    - 이름: **az1010301w-healthprobe**

    - 프로토콜: **TCP**

    - 포트: **80**

    - 간격: **5** 초

    - 비정상 임계값: **2** 연속 오류

    > **참고**: 작업이 완료될 때까지 기다립니다. 이 작업은 1 분 미만이 소요됩니다.

1. **az1010301w-lb - 상태 프로브** 블레이드에서 **az1010301w-lb - 부하 분산 규칙** 블레이드로 이동합니다.

1. **az1010301w-lb - 부하 분산 규칙** 블레이드에서 다음 설정으로 부하 분산 규칙을 추가합니다.

    - 이름: **az1010301w-lbrule01**

    - IP 버전: **IPv4**

    - 프런트 엔드 IP 주소: **LoadBalancerFrontEnd**

    - 프로토콜: **TCP**

    - 포트: **80**

    - 백 엔드 포트: **80**

    - 백 엔드 풀: **az1010301w-bepool (2대 가상 머신)**

    - 상태 프로브: **az1010301w-healthprobe (TCP:80)**

    - 세션 지속성: **없음**

    - 유휴 제한 시간 (분): **4**

    - 유동 IP (Direct Server Return): **사용 안 함**


#### 작업 2: 두 번째 지역에서 Azure 부하 분산 규칙 구현

   > **참고**: 이 작업을 시작하기 전에 이전 연습의 두 번째 작업에서 시작한 템플릿 배포가 완료되었는지 확인합니다. 

1. Azure 포털에서 **리소스 만들기** 블레이드로 이동합니다.

1. **리소스 만들기** 블레이드가 뜨면, Azure 마켓플레이스에서 **Load Balancer** 를 검색하십시오. 검색 결과 목록에서 **부하 분산 장치**를 선택합니다.

1. 검색 결과 목록을 사용하여로드 **부하 분산 장치 만들기** 블레이드로 이동하십시오.

1. **부하 분산 장치 만들기** 블레이드에서 다음 설정으로 새 부하 분산 장치를 만듭니다:

    - 이름: **az1010302w-lb**

    - 형식: **공개**

    - SKU: **기본**

    - 공용 IP 주소: **az1010302w-lb-pip**로 새로 만들기

    - 할당: **동적**

    - 구독: 이 랩에서 사용 중인 구독의 이름

    - 리소스 그룹: **az1010302-RG**

    - 지역: 이전 연습의 두 번째 작업에 Azure VM을 배포한 Azure 영역의 이름입니다

1. Azure 포털에서 새로 배포 된 Azure Load Balancer 장치인 **az1010302w-lb** 의 블레이드로 이동합니다.

1. **az1010302w-lb** 블레이드에서 **az1010302w-lb - 백 엔드 풀** 블레이드로 이동합니다.

1. **az1010302w-lb - 백 엔드 풀** 블레이드에서 다음 설정으로 백 엔드 풀을 추가합니다:

    - 이름: **az1010302w-bepool**

    - IP 버전: **IPv4**

    - 관련: **가용성 집합**

    - 가용성 집합: **az1010302w-avset**

    - 가상 머신: **az1010302w-vm0** 

    - 네트워크 IP 구성: **az1010302w-nic0/ipconfig1 (10.101.32.4)**

    - 가상 머신: **az1010302w-vm1** 

    - 네트워크 IP 구성: **az1010302w-nic1/ipconfig1 (10.101.32.5)**

   > **참고**: Azure VM의 IP 주소가 역순으로 할당될 수 있습니다. 

   > **참고**: 작업이 완료될 때까지 기다립니다. 이 작업은 1 분 미만이 소요됩니다.

1. **az1010302w-lb - 백 엔드 풀** 블레이드에서 **az1010302w-lb - 상태 프로브** 블레이드로 이동합니다.

1. **az1010302w-lb - 상태 프로브** 블레이드에서 다음 설정으로 상태 프로브를 추가합니다.

    - 이름: **az1010302w-healthprobe**

    - 프로토콜: **TCP**

    - 포트: **80**

    - 간격: **5** 초

    - 비정상 임계값: **2** 연속 오류

    > **참고**: 작업이 완료될 때까지 기다립니다. 이 작업은 1 분 미만이 소요됩니다.

1. **az1010302w-lb - 상태 프로브** 블레이드에서 **az1010302w-lb - 부하 분산 규칙** 블레이드로 이동합니다.

1. **az1010302w-lb - 부하 분산 규칙** 블레이드에서 다음 설정으로 부하 분산 규칙을 추가합니다.

    - 이름: **az1010302w-lbrule01**

    - IP 버전: **IPv4**

    - 프런트 엔드 IP 주소: **LoadBalancerFrontEnd**

    - 프로토콜: **TCP**

    - 포트: **80**

    - 백 엔드 포트: **80**

    - 백 엔드 풀: **az1010302w-bepool (가상 머신 2개)**

    - 상태 프로브: **az1010302w-healthprobe (TCP:80)**

    - 세션 지속성: **없음**

    - 유휴 시간 초과 (분): **4**

    - 유동 IP (Direct Server Return): **사용 안 함**


#### 작업 3: 첫 번째 리전에서 Azure NAT 규칙 구현

1. Azure 포털에서 Azure load balancer인 **az1010301w-lb** 의 블레이드로 이동합니다.

1. **az1010301w-lb** 블레이드에서 **az1010301w-lb - 인바운드 NAT 규칙** 블레이드로 이동합니다.

   > **참고**: NAT 기능은 상태 프로브에 의존하지 않습니다.

1. **az1010301w-lb - 인바운드 NAT 규칙** 블레이드에서 다음 설정으로 첫 번째 인바운드 NAT 규칙을 추가합니다:

    - 이름: **az1010301w-vm0-RDP**

    - 프런트 엔드 IP 주소: **LoadBalancerFrontEnd**

    - IP 버전: **IPv4**

    - 서비스: **Custom**

    - 프로토콜: **TCP**

    - 포트: **33890**

    - 대상 가상 머신: **az1010301w-vm0**

    - 네트워크 IP 구성: **ipconfig1 (10.101.31.4)** 또는 **ipconfig1 (10.101.31.5)**

    - 포트 매핑: **사용자 지정**

    - 유동 IP (Direct Server Return): **사용 안 함**

    - 대상 포트: **3389**

    > **참고**: 작업이 완료될 때까지 기다립니다. 이 작업은 1 분 미만이 소요됩니다.

1. **az1010301w-lb - 인바운드 NAT 규칙** 블레이드에서 다음 설정으로 두 번째 인바운드 NAT 규칙을 추가합니다:

    - 이름: **az1010301w-vm1-RDP**

    - 프런트 엔드 IP 주소: **LoadBalancerFrontEnd**

    - IP 버전: **IPv4**

    - 서비스: **Custom**

    - 프로토콜: **TCP**

    - 포트: **33891**

    - 대상 가상 컴퓨터: **az1010301w-vm1**

    - 네트워크 IP 구성: **ipconfig1 (10.101.31.4)** 또는 **ipconfig1 (10.101.31.5)**

    - 포트 매핑: **사용자 지정**

    - 유동 IP (Direct Server Return): **사용 안 함**

    - 대상 포트: **3389**

    > **참고**: 작업이 완료될 때까지 기다립니다. 이 작업은 1 분 미만이 소요됩니다.


#### 작업 4: 두 번째 리전에서 Azure NAT 규칙 구현

1. Azure 포털에서 Azure Load Balancer인 **az1010302w-lb** 의 블레이드로 이동합니다.

1. **az1010302w-lb** 블레이드에서 **az1010302w-lb - 인 바운드 NAT 규칙** 블레이드로 이동합니다.

1. **az1010302w-lb - 인바운드 NAT 규칙** 블레이드에서 다음 설정으로 첫 번째 인바운드 NAT 규칙을 추가합니다:

    - 이름: **az1010302w-vm0-RDP**

    - 프런트 엔드 IP 주소: **LoadBalancedFrontEnd**

    - IP 버전: **IPv4**

    - 서비스: **Custom**

    - 프로토콜: **TCP**

    - 포트: **33890**

    - 대상 가상 컴퓨터: **az1010302w-vm0**

    - 네트워크 IP 구성: **ipconfig1 (10.101.32.4)** 또는 **ipconfig1 (10.101.32.5)**

    - 포트 매핑: **사용자 지정**

    - 유동 IP (Direct Server Return): **사용 안 함**

    - 대상 포트: **3389**

    > **참고**: 작업이 완료될 때까지 기다립니다. 이 작업은 1 분 미만이 소요됩니다.

1. **az1010302w-lb - 인바운드 NAT 규칙** 블레이드에서 다음 설정으로 두 번째 인바운드 NAT 규칙을 추가합니다:

    - 이름: **az1010302w-vm1-RDP**

    - 프런트 엔드 IP 주소: **LoadBalancedFrontEnd**

    - IP 버전: **IPv4**

    - 서비스: **Custom**

    - 프로토콜: **TCP**

    - 포트: **33891**

    - 대상 가상 컴퓨터: **az1010302w-vm1**

    - 네트워크 IP 구성: **ipconfig1 (10.101.32.4)** 또는 **ipconfig1 (10.101.32.5)**

    - 포트 매핑: **사용자 지정**

    - 유동 IP (Direct Server Return): **사용 안 함**

    - 대상 포트: **3389**

    > **참고**: 작업이 완료될 때까지 기다립니다. 이 작업은 1 분 미만이 소요됩니다.


#### 작업 5: Azure 부하 분산 및 NAT 규칙을 확인합니다.

1. Azure 포털에서 Azure load balancer인 **az1010301w-lb** 의 블레이드로 이동합니다.

1. **az1010301w-lb** 블레이드에서 로드 밸런서 프런트 엔드에 할당된 공용 IP 주소를 확인합니다.

1. Microsoft Edge 창에서 새 탭을 열고 이전 단계에서 확인한 IP 주소를 탐색합니다.

1. 탭에 기본 인터넷 정보 서비스 홈 페이지가 표시되는지 확인합니다.

1. 기본 인터넷 정보 서비스 홈 페이지를 표시하는 브라우저 탭을 닫습니다.

1. Azure 포털에서 Azure load balancer인 **az1010301w-lb** 의 블레이드로 이동합니다.

1. **az1010301w-lb** 블레이드에서 로드 밸런서 프런트 엔드에 할당된 공용 IP 주소를 확인합니다.

1. 랩 가상 컴퓨터에서 &lt;az1010301w-lb_public_IP&amp;자리 표시자를 이전 작업에서 식별한 IP 주소로 대체한 후 다음 명령을 실행합니다:

   ```
   mstsc /v:<az1010301w-lb_public_IP>:33890
   ```

   > **참고**: 이 명령은 이전 작업에서 만든 **az1010301w-vm0-RDP NAT** 규칙을 사용하여 **az1010301w-vm0 Azure VM** 에 대한 원격 데스크톱 세션을 시작합니다.

1. 로그인이 시작되면 아래 사항을 입력하십시오:

    - 관리자 사용자 이름: **Student**

    - 관리자 암호: **Pa55w.rd1234**

1. 로그인하면 명령 프롬프트에서 다음 명령을 실행합니다:

   ```
   hostname
   ```

1. 출력을 검토하고 **az1010301w-vm0** Azure VM에 실제로 연결되어 있는지 확인합니다

   > **참고**: 다음 지역에서도 같은 테스트를 반복하십시오.

> **결과**: 이 연습을 완료한 후 두 리전에서 Azure Load Balancer 의 부하 분산 규칙 및 NAT 규칙을 구현하고 확인했습니다.


### 연습 2: Azure Traffic Manager 부하 분산 조정 구현

이 연습의 주요 작업은 다음과 같습니다:

1. Azure Load Balancer 의 공용 IP 주소에 DNS 이름 할당

1. Azure Traffic Manager 부하 분산 조정 구현

1. Azure Traffic Manager 부하 분산 조정 확인


#### 작업 1: Azure Load Balancer의 공용 IP 주소에 DNS 이름 할당

   > **참고**: 각 Traffic Manager 엔드포인트에는 DNS 이름이 할당되어 있어야 하기 때문에 이 작업이 필요합니다. 

1. Azure 포털에서 **az1010301w-lb-pip** 라는 첫 번째 지역에서 Azure Load Balancer와 연결된 공용 IP 주소 리소스의 블레이드로 이동합니다. 

1. **az1010301w-lb-pip** 블레이드에서 **구성** 블레이드로 이동합니다. 

1. **az1010301w-lb-pip - 구성** 블레이드에서 공용 IP 주소의 **DNS 이름 레이블(옵션)**을 고유한 값으로 설정합니다. 

   > **참고**: **DNS 이름 레이블(선택 사항)** 텍스트 상자의 녹색 확인란은 입력한 이름이 유효하고 고유한지 여부를 나타냅니다. 

1. **az1010302w-lb-pip** 라는 두 번째 지역에서 Azure Load Balancer와 연결된 공용 IP 주소 리소스의 블레이드로 이동합니다. 

1. **az1010302w-lb-pip** 블레이드에서 **구성** 블레이드로 이동합니다. 

1. **az1010302w-lb-pip - 구성** 블레이드에서 공용 IP 주소의 **DNS 이름 레이블(옵션)**을 고유한 값으로 설정합니다. 

   > **참고**: **DNS 이름 레이블(선택 사항)** 텍스트 상자의 녹색 확인란은 입력한 이름이 유효하고 고유한지 여부를 나타냅니다. 


#### 작업 2: Azure Traffic Manager 부하 분산 조정 구현

1. Azure 포털에서 **리소스 만들기** 블레이드로 이동합니다.

1. **리소스 만들기** 블레이드가 뜨면, Azure 마켓플레이스에서 **Traffic Manager profile**을 검색합니다.

1. 검색 결과 목록을 사용하여 **Traffic Manager 프로필 만들기** 블레이드로 이동합니다.

1. **Traffic Manager 프로필 만들기** 블레이드에서 다음 설정을 사용하여 새 Traffic Manager 프로필을 만듭니다:

    - 이름: trafficmanager.net DNS 네임스페이스를 사용하는 전역적으로 고유한 이름

    - 라우팅 방법: **가중**

    - 구독: 이 랩에서 사용 중인 구독의 이름

    - 리소스 그룹: **az1010303-RG** 이름으로 새로 만들기

    - 지역: 이 랩의 앞에서 사용한 Azure 지역 중 하나

1. Azure 포털에서 새로 프로비저닝 된 Traffic Manager 프로필의 블레이드로 이동합니다.

1. Traffic Manager 프로필의 블레이드에서 **구성** 블레이드로 이동하고 구성 설정을 검토합니다.

   > **참고**: Traffic Manager 프로필 DNS 레코드의 기본 TTL은 60초 입니다

1. Traffic Manager 프로필에서 **엔드포인트** 블레이드로 이동합니다.

1. **엔드포인트** 블레이드에서 다음 설정으로 첫 번째 엔드포인트를 추가합니다.

    - 형식: **Azure 엔드포인트**

    - 이름: **az1010301w-lb-pip**

    - 대상 리소스 형식: **공용 IP 주소**

    - 대상 리소스: **az1010301w-lb-pip**

    - 가중치: **100**

    - 사용자 지정 헤더 설정: 비워 둡니다

    - 사용 안 함으로 추가: 비워 둡니다

1. **엔드포인트** 블레이드에서 다음 설정으로 두 번째 엔드포인트를 추가합니다:

    - 형식: **Azure 엔드포인트**

    - 이름: **az1010302w-lb-pip**

    - 대상 리소스 형식: **공용 IP 주소**

    - 대상 리소스: **az1010302w-lb-pip**

    - 가중치: **100**

    - 사용자 지정 헤더 설정: 비워 둡니다

    - 사용 안 함으로 추가: 비워 둡니다

1. **엔드포인트** 블레이드에서 두 엔드포인트에 대한 **모니터링 상태** 열의 항목을 검사합니다. 다음 작업을 진행하기 전에 둘 다 **온라인** 으로 확인될 때까지 기다립니다.


#### 작업 3: Azure Traffic Manager 부하 분산 조정 확인

1. **엔드포인트** 블레이드에서 **개요** 블레이드로 이동합니다.

1. Traffic Manager 프로필에 할당된 DNS 이름 (**http://** 접두사 다음의 문자열)을 기록합니다. 

1. Azure 포털에서 Cloud Shell 창에서 PowerShell 세션을 시작하십시오. 

   > **참고**:  현재 Azure 구독에서 클라우드 셸을 처음 시작하는 경우 클라우드 셸 파일을 유지하도록 Azure 파일 공유를 만들라는 메시지가 표시됩니다. 이 경우 기본값을 허용하면 자동으로 생성된 리소스 그룹에서 저장소 계정이 생성됩니다.

1. Cloud Shell 창에서 다음 명령을 실행하여 &lt;TM_DNS_name&gt; 자리 표시자를 이전 작업에서 식별한 Traffic Manager 프로필에 할당된 DNS 이름 값으로 바꿉니다.

   ```
   nslookup <TM_DNS_name>
   ```

1. 출력을 검토하고 **이름** 항목을 기록합니다. 이전 작업에서 만든 Traffic Manager 프로필 엔드포인트중 하나의 DNS 이름과 일치해야 합니다.

1. 60초 이상 기다렸다가 동일한 명령을 다시 실행합니다:

   ```
   nslookup <TM_DNS_name>
   ```
1. 출력을 검토하고 **이름** 항목을 기록합니다. 이번에는 항목이 이전 작업에서 만든 다른 Traffic Manager 프로필 엔드포인트의 DNS 이름과 일치해야 합니다.

> **결과**: 이 연습을 완료한 후 Azure Traffic Manager 부하 분산을 구현하고 확인했습니다


### 연습 4: 랩 리소스 삭제

#### 작업 1: Cloud Shell 열기

1. Azure 포털 상단에서 **Cloud Shell** 아이콘을 클릭하여 Cloud Shell 창을 엽니다.

1. Cloud Shell 인터페이스에서 **Bash**를 선택합니다.

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter**를 눌러 이 랩에서 생성한 모든 리소스 그룹을 나열합니다.

   ```sh
   az group list --query "[?starts_with(name,'az1010')].name" --output tsv
   ```

1. 출력된 결과가 이 랩에서 생성한 리소스 그룹만 포함되어 있는지 확인합니다. 이 그룹은 다음 작업에서 삭제됩니다.

#### 작업 2: 리소스 그룹 삭제하기

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter**를 눌러 이 랩에서 생성한 모든 리소스 그룹을 삭제합니다.

   ```sh
   az group list --query "[?starts_with(name,'az1010')].name" --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
   ```

1. **Cloud Shell** 명령 프롬프트를 닫습니다.

> **결과**: 이 연습을 완료한 후 이 랩에서 사용된 리소스 그룹을 제거했습니다.
