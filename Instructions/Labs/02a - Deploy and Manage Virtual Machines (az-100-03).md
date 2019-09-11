---
lab:
    title: '가상 머신 배포 및 관리'
    module: '모듈 02 - Azure 가상 머신'
---

# 랩: 가상 머신 배포 및 관리

이 랩의 모든 작업은 원격 데스크톱 세션에서 Azure VM에 수행된 단계를 포함하는 연습 2 작업 2 와 연습 2 작업 3을 제외한 Azure 포털(PowerShell Cloud Shell 세션 포함)에서 수행됩니다.

   > **참고**: Cloud Shell을 사용하지 않는 경우 랩 가상 머신에 Azure PowerShell 모듈이 설치되어 있어야 합니다. [**https://docs.microsoft.com/ko-kr/powershell/azure/azurerm/install-azurerm-ps**](https://docs.microsoft.com/ko-kr/powershell/azure/azurerm/install-azurerm-ps)

랩 파일: 

-  **Labfiles\\Module_02\\Deploy_and_Manage_Virtual_Machines\\az-100-03_azuredeploy.json**

-  **Labfiles\\Module_02\\Deploy_and_Manage_Virtual_Machines\\az-100-03_azuredeploy.parameters.json**

-  **Labfiles\\Module_02\\Deploy_and_Manage_Virtual_Machines\\az-100-03_install_iis_vmss.zip**

### 시나리오
  
Adatum Corporation은 Azure 가상 머신(VM) 및 Azure VM 확장 집합을 사용하여 워크로드를 구현하려고 합니다.


### 목표
  
이 과정을 완료하면 다음과 같은 역량을 갖추게 됩니다:

-  Azure 포털, Azure PowerShell 및 Azure Resource Manager 템플릿을 사용하여 Azure VM 배포

-  Windows 및 Linux 운영 체제를 실행하는 Azure VM의 네트워킹 설정 구성

-  Azure VM 스케일 세트 배포 및 구성


### 연습 1: Azure 포털, Azure PowerShell 및 Azure Resource Manager 템플릿을 사용하여 Azure VM 배포
  
이 연습의 주요 작업은 다음과 같습니다:

1. Windows Server 2016 데이터 센터를 실행하는 Azure VM을 Azure 포털을 사용하여 가용성 집합에 배포합니다

1. Azure PowerShell을 사용하여 Windows Server 2016 데이터 센터를 실행하는 Azure VM을 기존 가용성 집합에 배포합니다

1. Azure Resource Manager 템플릿을 사용하여 Linux를 실행하는 두 개의 Azure VM을 가용성 집합에 배포합니다


#### 작업 1: Windows Server 2016 데이터 센터를 실행하는 Azure VM을 Azure 포털을 사용하여 가용성 집합에 배포합니다

1. 랩 가상 머신에서 Microsoft Edge를 시작하고 [**http://portal.azure.com**](http://portal.azure.com) 에서 Azure 포털을 탐색하여 이 랩에서 사용하려는 Azure 구독에서 소유자 역할이 있는 Microsoft 계정을 사용하여 로그인합니다.

1. Azure 포털에서 **리소스 만들기** 블레이드로 이동합니다.

1. **리소스 만들기** 블레이드가 뜨면, Azure 마켓플레이스에서 **Windows Server**를 검색합니다. 검색 결과 목록에서 **Windows Server**를 선택합니다.

1. Windows Server 페이지에서, 드롭다운 메뉴에 있는 **[smalldisk] Windows Server 2016 Datacenter**를 선택합니다. 그리고 **만들기**를 클릭합니다.

1. **가상 머신 만들기**블레이드에서 다음 설정을 사용하여 가상 머신를 배포합니다.

    - 구독: 이 랩에서 사용 중인 구독의 이름

    - 리소스 그룹: **az1000301-RG** 이름으로 새로 만들기

    - 가상 머신 이름: **az1000301-vm0**

    - 지역: 랩 위치에 가장 가깝고 Azure VM을 프로비전할 수 있는 Azure 지역의 이름입니다.

    - 가용성 옵션: **가용성 집합**

    - 가용성 집합: **새로 만들기**를 클릭한다. 새로운 가용성 집합의 이름은 **az1000301-avset0**로, 장애 도메인은 **2**로, 업데이트 도메인은 **5**로 설정한다. **확인**을 클릭한다.

    - 이미지: **[smalldisk] Windows Server 2016 Datacenter**

    - 크기: **표준 DS2 V2**

    - 사용자 이름: **Student**

    - 암호: **Pa55w.rd1234**

    - 공용 인바운드 포트: **없음**

    - 이미 Windows Server 라이센스가 있나요?: **아니요**

1. **다음: 디스크 >**를 클릭한다.

    - OS 디스크 유형: **표준 HDD**

1. **다음: 네트워킹 >**을 클릭한다.

1. 네트워킹 탭이 뜨면, 다음을 사용하여 네트워킹 탭을 설정한다.

    - 가상 네트워크: 가상 네트워크 아래에 **새로 만들기**를 클릭한다.
    
        - 이름: **az1000301-vnet0**

        - 주소 범위: **10.103.0.0/16**

        - 서브넷 이름: **subnet0**

        - 서브넷 주소 범위: **10.103.0.0/24**

    - 공용 IP: **새로 만들기**를 클릭한다.

        - 이름: **az1000301-vm0-ip**

    - NIC 네트워크 보안 그룹: **기본**

    - 공용 인바운드 포트: **없음**

    - 가속화된 네트워킹: **꺼짐**

1. **다음: 관리 >**를 클릭한다.

1. 관리 탭이 뜨면, 다음을 사용하여 관리 탭을 설정한다.

    - 부팅 진단: **끄기**

    - OS 게스트 진단: **끄기**

    - 시스템 할당 관리 ID: **끄기**

    - 자동 종료 사용 설정: **끄기**

1. **검토 + 만들기**를 클릭한다.

1. **만들기**를 클릭한다.

   > **참고**: Azure VM을 프로비전할 수 있는 Azure 지역을 식별하려면 [**https://azure.microsoft.com/ko-kr/regions/offers/**](https://azure.microsoft.com/ko-kr/regions/offers/)참고하십시오.

   > **참고**: 이 랩의 두 번째 연습에서는 이 작업에서 만든 네트워크 보안 그룹을 구성합니다.

   > **참고**: 다음 작업을 진행하기 전에 배포가 완료될 때까지 기다립니다. 약 5 분이 소요됩니다.


#### 작업 2: Azure PowerShell을 사용하여 Windows Server 2016 데이터 센터를 실행하는 Azure VM을 기존 가용성 집합에 배포합니다.

1. Azure 포털의 Cloud Shell 창에서 PowerShell 세션을 시작하십시오. 

   > **참고**: 현재 Azure 구독에서 Cloud Shell을 처음 시작하는 경우 Cloud Shell 파일을 유지하도록 Azure 파일 공유를 만들라는 메시지가 표시됩니다. 이 경우 기본값을 허용하면 자동으로 생성된 리소스 그룹에서 저장소 계정이 생성됩니다.

1. Cloud Shell 창에서 다음 명령을 실행합니다:

   ```pwsh
   $vmName = 'az1000301-vm1'
   $vmSize = 'Standard_DS2_v2'
   ```

   > **참고**: 이렇게 하면 Azure VM 이름과 크기를 지정하는 변수의 값이 설정됩니다.

1. Cloud Shell 창에서 다음 명령을 실행합니다:

   ```pwsh
   $resourceGroup = Get-AzResourceGroup -Name 'az1000301-RG'
   $location = $resourceGroup.Location
   ```

   > **참고**: 이러한 명령은 대상 리소스 그룹과 해당 위치를 지정하는 변수의 값을 설정합니다.

1. Cloud Shell 창에서 다음 명령을 실행합니다:

   ```pwsh
   $availabilitySet = Get-AzAvailabilitySet -ResourceGroupName $resourceGroup.ResourceGroupName -Name 'az1000301-avset0'
   $vnet = Get-AzVirtualNetwork -Name 'az1000301-vnet0' -ResourceGroupName $resourceGroup.ResourceGroupName
   $subnetid = (Get-AzVirtualNetworkSubnetConfig -Name 'subnet0' -VirtualNetwork $vnet).Id
   ```

   > **참고**: 이러한 명령은 새 Azure VM을 배포할 가용성 집합, 가상 네트워크 및 서브넷을 지정하는 변수값을 설정합니다.

1. Cloud Shell 창에서 다음 명령을 실행합니다:

   ```pwsh
   $nsg = New-AzNetworkSecurityGroup -ResourceGroupName $resourceGroup.ResourceGroupName -Location $location -Name "$vmName-nsg"
   $pip = New-AzPublicIpAddress -Name "$vmName-ip" -ResourceGroupName $resourceGroup.ResourceGroupName -Location $location -AllocationMethod Dynamic 
   $nic = New-AzNetworkInterface -Name "$($vmName)$(Get-Random)" -ResourceGroupName $resourceGroup.ResourceGroupName -Location $location -SubnetId $subnetid -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
   ```

   > **참고**: 이러한 명령은 새 Azure VM에서 사용할 새 네트워크 보안 그룹, 공용 IP 주소 및 네트워크 인터페이스를 만듭니다

   > **참고**: 이 랩의 두 번째 연습에서는 이 작업에서 만든 네트워크 보안 그룹을 구성합니다.

1. Cloud Shell 창에서 다음 명령을 실행합니다:

   ```pwsh
   $adminUsername = 'Student'
   $adminPassword = 'Pa55w.rd1234'
   $adminCreds = New-Object PSCredential $adminUsername, ($adminPassword | ConvertTo-SecureString -AsPlainText -Force)
   ```

   > **참고**: 이러한 명령은 새 Azure VM의 로컬 관리자 계정의 자격 증명을 지정하는 변수의 값을 설정합니다.

1. Cloud Shell 창에서 다음 명령을 실행합니다:

   ```pwsh
   $publisherName = 'MicrosoftWindowsServer'
   $offerName = 'WindowsServer'
   $skuName = '2016-Datacenter'
   ```

   > **참고**: 이러한 명령은 새 Azure VM을 프로비전하는 데 사용할 Azure Marketplace 이미지의 속성을 지정하는 변수값을 설정합니다.

1. Cloud Shell 창에서 다음 명령을 실행합니다:

   ```pwsh
   $osDiskType = (Get-AzDisk -ResourceGroupName $resourceGroup.ResourceGroupName)[0].Sku.Name
   ```

   > **참고**: 이 명령은 새 Azure VM의 운영 체제 디스크 유형을 지정하는 변수의 값을 설정합니다.

1. Cloud Shell 창에서 다음 명령을 실행합니다:

   ```pwsh
   $vmConfig = New-AzVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $availabilitySet.Id
   Add-AzVMNetworkInterface -VM $vmConfig -Id $nic.Id
   Set-AzVMOperatingSystem -VM $vmConfig -Windows -ComputerName $vmName -Credential $adminCreds 
   Set-AzVMSourceImage -VM $vmConfig -PublisherName $publisherName -Offer $offerName -Skus $skuName -Version 'latest'
   Set-AzVMOSDisk -VM $vmConfig -Name "$($vmName)_OsDisk_1_$(Get-Random)" -StorageAccountType $osDiskType -CreateOption fromImage
   Set-AzVMBootDiagnostic -VM $vmConfig -Disable
   ```

   > **참고**: 이들 명령은 VM 크기, VM의 가용성 집합, 네트워크 인터페이스, 컴퓨터 이름, 로컬 관리자 자격 증명, 원본 이미지, 운영 체제 디스크, 부팅 진단 설정 등, 새 Azure VM을 프로비전하는 데 사용되는 Azure VM 구성 개체의 속성을 설정합니다.

1. Cloud Shell 창에서 다음 명령을 실행합니다:

   ```pwsh
   New-AzVM -ResourceGroupName $resourceGroup.ResourceGroupName -Location $location -VM $vmConfig
   ```

   > **참고**: 이 명령은 새 Azure VM의 배포를 시작합니다.

   > **참고**: 배포가 완료될 때까지 기다리지 말고 다음 작업으로 진행합니다.


#### 작업 3: Azure Resource Manager 템플릿을 사용하여 Linux를 실행하는 두 개의 Azure VM을 가용성 집합에 배포합니다.

1. Azure 포털에서 **리소스 만들기** 블레이드로 이동합니다.

1. **리소스 만들기** 블레이드가 뜨면, Azure 마켓플레이스에서 **Template deployment**를 검색합니다. 그리고 **Template deployment (deploy using custom templates)**를 선택합니다.

1. **만들기**를 클릭합니다.
 
1. **사용자 지정 배포** 블레이드에서 **편집기에서 사용자 고유의 템플릿을 빌드합니다.** 를 선택합니다.

1. **템플릿 편집** 블레이드에서 템플릿 파일인 **Labfiles\\Module_02\\Deploy_and_Manage_Virtual_Machines\\az-100-03_azuredeploy.json** 을 로드합니다.

   > **참고**: 템플릿의 내용을 검토하고 Linux Ubuntu를 호스팅하는 두 개의 Azure VM을 가용성 집합으로 기존 가상 네트워크 **az1000301-vnet0** 에 배포하도록 정의합니다.

1. 템플릿을 저장하고 **사용자 지정 배포** 블레이드로 돌아갑니다. 

1. **사용자 지정 배포** 블레이드에서 **매개 변수 편집** 블레이드로 이동합니다.

1. **편집 매개 변수** 블레이드에서 매개 변수 파일 **Labfiles\\Module_02\\Deploy_and_Manage_Virtual_Machines\\az-100-03_azuredeploy.parameters.json**을 로드합니다.

1. 매개 변수를 저장하고 **사용자 지정 배포** 블레이드로 돌아갑니다. 

1. **사용자 지정 배포** 블레이드에서 다음을 사용하여 템플릿 배포를 시작합니다.

    - 구독: 이 랩에서 사용 중인 구독의 이름

    - 리소스 그룹: **az1000302-RG** 이름으로 새로 만들기

    - 지역: 이 연습의 앞에서 선택한 동일한 Azure 지역

    - Vm Name Prefix: **az1000302-vm**

    - Nic Name Prefix: **az1000302-nic**

    - Pip Name Prefix: **az1000302-ip**

    - Admin Username: **Student**

    - Admin Password: **Pa55w.rd1234**

    - Virtual Network Name: **az1000301-vnet0**

    - Image Publisher: **Canonical**

    - Image Offer: **UbuntuServer**

    - Image SKU: **16.04.0-LTS**

    - Vm Size: **Standard_DS2_v2**

   > **참고**: 다음 작업을 진행하기 전에 배포가 완료될 때까지 기다립니다. 약 5 분이 소요됩니다.

> **결과**: 이 연습을 완료한 후 Windows Server 2016 데이터 센터를 실행하는 Azure VM을 Azure 포털을 사용하여 가용성 집합에 배포하고, Windows Server 2016 데이터 센터를 실행하는 다른 Azure VM을 Azure PowerShell를 사용하여 동일한 가용성 집합에 배포했습니다. 그리고 Azure Resource Manager 템플릿을 사용하여 가용성 집합에 우분투 리눅스를 실행 하는 두 개의 Azure VM을 배포 했습니다.

   > **참고**: 템플릿을 사용하여 Windows Server 2016 데이터 센터를 호스팅하는 두 개의 Azure VM을 단일 작업에 배포할 수 있습니다(Linux Ubuntu 서버를 호스팅하는 두 개의 Azure VM을 배포한 것 처럼). 이러한 Azure VM을 두 개의 별도 작업에 배포하는 이유는 Azure Portal 및 Azure PowerShell 기반 배포에 익숙해질 수 있는 기회를 제공하기 위해서 입니다.


### 연습 2: Windows 및 Linux 운영 체제를 실행하는 Azure VM의 네트워킹 설정 구성
  
이 연습의 주요 작업은 다음과 같습니다:

1. Azure VM의 정적 개인 및 공용 IP 주소 구성

1. 공용 IP 주소를 통해 Windows Server 2016 데이터 센터를 실행하는 Azure VM에 연결

1. 사설 IP 주소를 통해 리눅스 우분투 서버를 실행 하는 Azure VM에 연결


#### 작업 1: Azure VM의 정적 개인 및 공용 IP 주소 구성

1. Azure 포털에서 **az1000301-vm0** 블레이드로 이동합니다.

1. **az1000301-vm0** 블레이드에서 **네트워킹** 블레이드로 이동하여 네트워크 인터페이스에 할당된 공용 IP 주소인 **az1000301-vm0-ip** 구성을 확인합니다.

1. **네트워킹**블레이드에서 NIC 공용 IP 주소를 나타내는 링크를 클릭합니다.

1. **az1000301-vm0-ip** 블레이드에서 **구성**을 클릭합니다.

1. 할당을 **정적** 으로 변경하고 **저장**을 클릭합니다.

   > **참고**: **az1000301-vm0** 의 네트워크 인터페이스에 할당된 공용 IP 주소를 기록해 둡니다. 이 연습의 뒷부분에서 필요할 것입니다.

1. Azure 포털에서 **az1000302-vm0** 블레이드로 이동합니다.

1. **az1000302-vm0** 블레이드에서 **네트워킹**블레이드로 이동합니다.

1. **az1000302-vm0 - 네트워킹** 블레이드에서 네트워크 인터페이스를 나타내는 링크를 클릭합니다.

1. **az1000302-vm0** 의 네트워크 인터페이스의 속성을 표시하는 블레이드에서 **IP 구성** 블레이드로 이동합니다.

1. **az1000302-vm0** 의 네트워크 인터페이스의 속성을 표시하는 블레이드에서 **ipconfig1** 블레이드로 이동합니다.

1. **IP 구성** 블레이드가 뜨면 **ipconfig1**의 사설 IP 주소 할당을 정적으로 구성하고 **10.103.0.100** 으로 설정합니다. 그리고 **저장**을 클릭합니다.

   > **참고**: 사설 IP 주소 할당을 변경하려면 Azure VM을 다시 시작해야 합니다.

   > **참고**: 정적 또는 동적으로 할당된 공용 및 사설 IP 주소를 통해 Azure VM에 연결할 수 있습니다. 정적 IP 할당 선택은 일반적으로 이러한 IP 주소가 IP 필터링, 라우팅과 함께 사용되거나 DNS 서버로 작동하는 Azure VM의 네트워크 인터페이스에 할당되는 시나리오에서 수행됩니다.


#### 작업 2: 공용 IP 주소를 통해 Windows Server 2016 데이터 센터를 실행하는 Azure VM에 연결

1. Azure 포털에서 **az1000301-vm0** 블레이드로 이동합니다.

1. **az1000301-vm0** 블레이드에서 **네트워킹** 블레이드로 이동합니다.

1. **az1000301-vm0 - 네트워킹** 블레이드에서 **az1000301-vm0** 의 네트워크 인터페이스에 할당된 네트워크 보안 그룹의 인바운드 포트 규칙을 검토합니다.

   > **참고**: 기본 제공 규칙으로 구성된 기본 구성은 인터넷에서 인바운드 연결을 차단합니다(RDP 포트 TCP 3389를 통한 연결 포함).

1. 다음 설정을 사용하여 기존 네트워크 보안 그룹에 인바운드 보안 규칙을 추가합니다.

    - 소스: **Any**

    - 원본 포트 범위: **\***

    - 대상 주소: **Any**

    - 대상 포트 범위: **3389**

    - 프로토콜: **TCP**

    - 작업: **허용**

    - 우선 순위: **100**

    - 이름: **AllowInternetRDPInBound**

1. Azure 포털에서 **az1000301-vm0** 블레이드의 **개요** 를 클릭합니다. 

1. **az1000301-vm0** 블레이드의 **개요** 에서 **연결** 을 클릭하여 RDP 파일을 생성하여 **az1000301-vm0** 에 연결하는데 사용합니다.

1. 메시지가 표시되면 다음 자격 증명을 사용하여 인증합니다:

    - 사용자 이름: **Student**

    - 암호: **Pa55w.rd1234**


#### 작업 3: 사설 IP 주소를 통해 리눅스 우분투 서버를 실행하는 Azure VM에 연결
 
1. **az1000301-vm0**의 RDP 세션 내에서 **명령 프롬프트** 를 시작합니다.

1. 명령 프롬프트에서 다음을 실행합니다.

   ```
   nslookup az1000302-vm0
   ```

1. 출력된 값을 확인하면 이 연습의 첫 번째 작업에서 할당한 IP 주소로 이름이 확인됩니다(**10.103.0.100**).

   > **참고**: 예상되는 상황입니다. Azure는 가상 네트워크 내에서 기본 제공 DNS 이름 확인을 제공합니다. 

1. **az1000301-vm0**의 RDP 세션 내의 서버 관리자에서 **로컬 서버**를 클릭하고 **IE 강화 보안 구성** 을 사용하지 않도록 설정합니다.

1. **az1000301-vm0**의 RDP 세션 내에서,인터넷 익스플로러를 시작하고 [**https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html**](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) 에서 **putty.exe**를 다운로드 한다.

1. **putty.exe** 를 사용하여 **SSH** 프로토콜(TCP 22)을 통해 사설 IP 주소에서 **az1000302-vm0** 에 성공적으로 연결할 수 있는지 확인합니다.

1. 메시지가 표시되면 다음 값을 지정하여 인증합니다:

    - 사용자 이름: **Student**

    - 암호: **Pa55w.rd1234**

1. 성공적으로 인증되면 **az1000301-vm0**의 RDP 세션을 종료합니다.

1. Azure 포털의 랩 가상 머신에서 **az1000302-vm0** 블레이드로 이동합니다.

1. **az1000302-vm0** 블레이드에서 **네트워킹** 블레이드로 이동합니다.

1. **az1000302-vm0 - 네트워킹** 블레이드에서 **az1000301-vm0** 의 네트워크 인터페이스에 할당된 네트워크 보안 그룹의 인바운드 포트 규칙을 검토하여 사설 IP 주소를 통한SSH 연결이 성공한 이유를 확인합니다.

   > **참고**: 기본 제공 규칙으로 구성된 기본 구성을 사용하면 Azure 가상 네트워크 환경 내의 인바운드 연결을 허용합니다(SSH 포트 TCP 22를 통한 연결 포함).

> **결과**: 이 연습을 완료한 후 Azure VM의 정적 사설 및 공용 IP 주소를 구성하고 공용 IP 주소를 통해 Windows Server 2016 데이터 센터를 실행하는 Azure VM에 연결하고 사설 IP 주소를 통해 Linux Ubuntu 서버를 실행하는 Azure VM에 연결했습니다.


### 연습 3: Azure VM 스케일 세트 배포 및 구성

이 연습의 주요 작업은 다음과 같습니다:

1. Azure VM 스케일 집합 배포에 사용할 수 있는 DNS 이름 식별

1. Azure VM 스케일 세트 배포

1. DSC 확장을 사용하여 스케일 세트 VM에 IIS 설치


#### 작업 1: Azure VM 스케일 집합 배포에 사용할 수 있는 DNS 이름 식별

1. Azure 포털의 Cloud Shell 창에서 PowerShell 세션을 시작하십시오. 

1. Cloud Shell 창에서 다음 명령을 실행합니다. 자리 표시자로 표시 된 &lt;custom-label&gt;을 고유한 문자열로 대체합니다.

   ```pwsh
   $rg = Get-AzResourceGroup -Name az1000301-RG
   Test-AzDnsAvailability -DomainNameLabel <custom-label> -Location $rg.Location
   ```

1. 결과가 **True**인지 확인합니다. 그렇지 않은 경우 &lt;custom-label&gt;을 다른 값으로 변경하여 동일한 명령을 **True** 를 반환할 때까지 다시 실행합니다.

1. 성공적인 결과가 출력된 &lt;custom-label&gt;를 별도로 메모합니다. 다음 작업에 필요합니다.


#### 작업 2: Azure VM 스케일 세트 배포

1. Azure 포털에서 **리소스 만들기** 블레이드로 이동합니다.

1.  **리소스 만들기** 블레이드가 뜨면, Azure 마켓플레이스에서 **Virtual machine scale set** 을 검색합니다.

1. 검색 결과 목록을 사용하여 **Virtual machine scale set** 만들기 블레이드로 이동합니다.

1. **가상 머신 확장 집합 만들기** 블레이드에서 다음 설정으로 가상 시스템 스케일 집합을 배포합니다.

    - 가상 머신 확장 집합 이름: **az1000303vmss0**

    - 운영 체제 디스크 이미지: **Windows Server 2016 Datacenter**

    - 구독: 이 랩에서 사용 중인 구독의 이름

    - 리소스 그룹: **az1000303-RG** 이름으로 새로 만들기

    - 지역: 이 랩의 이전 연습에서 선택한 것과 동일한 Azure 지역

    - 가용성 영역: **없음**

    - 사용자 이름: **Student**

    - 암호: **Pa55w.rd1234**

    - 인스턴스 수: **1**

    - 인스턴스 크기: **DS2 v2**

    - 낮은 우선 순위로 배포: **아니요**

    - 관리 디스크 사용: **예**

    - 자동 크기 조정: **사용 안 함**

    - 부하 균형 옵션을 선택합니다. **부하 분산 장치**

    - 공용 IP 주소 이름: **az1000303vms0-ip**

    - 도메인 이름 레이블: 이전 작업에서 식별된 &lt;custom-label&gt;을 값에 입력합니다.

    - 가상 네트워크: 가상 네트워크 아래에 **새로 만들기**를 클릭한다.
    
        - 이름: **az1000303-vnet0**

        - 주소 공간: **10.203.0.0/16**

        - 서브넷 이름: **subnet0**

        - 서브넷 주소 범위: **10.203.0.0/24**

    - 인스턴스당 공용 IP 주소: **끄기**

   > **참고**: 다음 작업을 진행하기 전에 배포가 완료될 때까지 기다립니다. 약 5 분이 소요됩니다.


#### 작업 3: DSC 확장을 사용하여 스케일 세트 VM에 IIS 설치

1. Azure 포털에서 **az1000303vms0** 블레이드로 이동합니다.

1. **az1000303vms0** 블레이드에서 **확장** 블레이드로 이동합니다.

1. **az1000303vms0 - 확장** 블레이드에서 다음 설정으로 **PowerShell Desired State Configuration** 을 추가합니다.

   > **참고**: DSC 구성 모듈은 **Labfiles\\Module_02\\Deploy_and_Manage_Virtual_Machines\\az-100-03_install_iis_vmss.zip** 에서 업로드 할 수 있습니다. 이 모듈에는 IIS(웹 서버) 역할을 설치하는 DSC 구성 스크립트가 포함되어 있습니다.

    - Configuration Modules or Script: **"az-100-03_install_iis_vmss.zip"**

    - Module-qualified Name of Configuration: **az-100-03_install_iis_vmss.ps1\IISInstall**

    - Configuration Arguments: 비워 둡니다.

    - Configuration Data PSD1 File: 비워 둡니다.

    - WMF Version: **latest**

    - Data Collection: **Disable**

    - Version: **2.76**

    - Auto Upgrade Minor Version: **Yes**

1. **az1000303vms0 - 인스턴스** 블레이드로 이동하여 **az1000303vmss0_0** 인스턴스의 업그레이드를 시작합니다.

   > **참고**: 이 업데이트는 DSC 구성 스크립트의 응용 프로그램을 트리거합니다. 업그레이드가 완료될 때까지 기다립니다. 약 5 분이 소요됩니다. **az1000303vms0 - 인스턴스** 블레이드에서 진행 상황을 모니터링할 수 있습니다. 

1. 업그레이드가 완료되면 **az1000303vms0-ip** 블레이드로 이동합니다. 

1. **az1000303vms0-IP** 블레이드에서 **az1000303vmss0** 에 할당된 공용 IP 주소를 메모하십시오.

1. Microsoft Edge를 시작하고 이전 단계에서 식별한 공용 IP 주소로 이동합니다.

1. 브라우저에 기본 IIS 홈 페이지가 표시되는지 확인합니다. 

> **결과**: 이 연습을 완료한 후 Azure VM 규모 집합 배포에 사용할 수 있는 DNS 이름을 식별하고, Azure VM 규모 집합을 배포하고, DSC 확장을 사용하여 스케일 세트 VM에 IIS를 설치했습니다.


### 연습 4: 랩 리소스 삭제

#### 작업 1: Cloud Shell 열기

1. Azure 포털 상단에서 **Cloud Shell** 아이콘을 클릭하여 Cloud Shell 창을 엽니다.

1. Cloud Shell 인터페이스에서 **Bash**를 선택합니다.

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter**를 눌러 이 랩에서 생성한 모든 리소스 그룹을 나열합니다.

   ```sh
   az group list --query "[?starts_with(name,'az1000')].name" --output tsv
   ```

1. 출력된 결과가 이 랩에서 생성한 리소스 그룹만 포함되어 있는지 확인합니다. 이 그룹은 다음 작업에서 삭제됩니다.

#### 작업 2: 리소스 그룹 삭제하기

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter**를 눌러 이 랩에서 생성한 모든 리소스 그룹을 삭제합니다.

   ```sh
   az group list --query "[?starts_with(name,'az1000')].name" --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
   ```

1. **Cloud Shell** 명령 프롬프트를 닫습니다.

> **결과**: 이 연습을 완료한 후 이 랩에서 사용된 리소스 그룹을 제거했습니다.
