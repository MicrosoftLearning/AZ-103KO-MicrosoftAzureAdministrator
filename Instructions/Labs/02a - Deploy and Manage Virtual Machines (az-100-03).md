---
lab:
    title: '가상 머신 배포 및 관리'
    module: '가상 머신 배포 및 관리'
---

# 랩: 가상 머신 배포 및 관리

이 랩의 모든 작업은 원격 데스크톱 세션에서 Azure VM에 수행된 단계를 포함하는 Excerce 2 태스크 2 및 작업 2 작업 3을 제외한 Azure 포털(PowerShell Cloud Shell 세션 포함)에서 수행됩니다.

   > **참고**: Cloud Shell을 사용하지 않는 경우 랩 가상 머신에 Azure PowerShell 모듈이 설치되어 있어야 합니다. [**https://docs.microsoft.com/ko-kr/powershell/azure/azurerm/install-azurerm-ps**](https://docs.microsoft.com/ko-kr/powershell/azure/azurerm/install-azurerm-ps)

랩 파일: 

-  **Labfiles\\AZ100\\Mod03\\az-100-03_deploy_azure_vm.ps1**

-  **Labfiles\\AZ100\\Mod03\\az-100-03_azuredeploy.json**

-  **Labfiles\\AZ100\\Mod03\\az-100-03_azuredeploy.parameters.json**

-  **Labfiles\\AZ100\\Mod03\\az-100-03_install_iis_vmss.zip**

### 시나리오
  
Adatum Corporation은 Azure 가상 시스템(VM) 및 Azure VM 스케일 집합을 사용하여 워크로드를 구현하려고 합니다.


### 목표
  
이 과정을 완료하면 다음과 같은 역량을 갖추게 됩니다:

-  Azure 포털, Azure PowerShell 및 Azure 리소스 관리자 템플릿을 사용하여 Azure VM 배포

-  Windows 및 Linux 운영 체제를 실행하는 Azure VM의 네트워킹 설정 구성

-  Azure VM 스케일 세트 배포 및 구성


### 연습 1: Azure 포털, Azure PowerShell 및 Azure 리소스 관리자 템플릿을 사용하여 Azure VM 배포
  
이 연습의 주요 작업은 다음과 같습니다:

1. Windows Server 2016 데이터 센터를 실행하는 Azure VM을 Azure 포털을 사용하여 가용성 집합에 배포합니다

1. Azure PowerShell을 사용하여 Windows Server 2016 데이터 센터를 실행하는 Azure VM을 기존 가용성 집합에 배포합니다

1. Azure 리소스 관리자 템플릿을 사용하여 Linux를 실행하는 두 개의 Azure VM을 가용성 집합에 배포합니다


#### 작업 1: Windows Server 2016 데이터 센터를 실행하는 Azure VM을 Azure 포털을 사용하여 가용성 집합에 배포합니다

1. 랩 가상 머신에서 Microsoft Edge를 시작하고 [**http://portal.azure.com**](http://portal.azure.com) 에서 Azure 포털을 찾아보고 이 랩에서 사용하려는 Azure 구독에서 소유자 역할이 있는 Microsoft 계정을 사용하여 로그인합니다.

1. Azure 포털에서 **리소스 블레이드 만들기로** 이동합니다.

1. **리소스 블레이드 만들기에서** Windows Server **2016 데이터 센터에** 대한 Azure 마켓플레이스를 검색합니다.

1. 검색 결과 목록을 사용하여 Windows Server 2016 데이터 센터 Azure Marketplace 이미지를 배포하기 위한 **가상 머신 블레이드 만들기** 로 이동합니다.

1. **가상 머신 블레이드 만들기** 를 사용하여 다음 설정을 사용하여 가상 머신를 배포합니다.

    - 구독: 이 랩에서 사용 중인 구독의 이름

    - 리소스 그룹: 새 리소스 그룹 **az1000301-RG** 의 이름

    - 가상 머신 이름: **az1000301-vm0**

    - 지역: 랩 위치에 가장 가깝고 Azure VM을 프로비전할 수 있는 Azure 지역의 이름입니다.

    - 가용성 옵션: **가용성 집합**

    - 가용성 집합: **2** 개의 오류 도메인과 5개의 업데이트 도메인이 있는 새 가용성 집합 **az1000301-avset0** **5** 이름입니다.

    - 이미지: **Windows Server 2016 Datacenter**

    - 크기: **스탠다드 DS1 V2**

    - 사용자 이름: **학생**

    - 암호: **Pa55w.rd1234**

    - 공용 인바운드 포트: **없음**

    - 이미 윈도우 라이센스가 있습니까?: **아니요**

    - OS 디스크 유형: **표준 HDD**

    - 가상 네트워크: 다음 설정이 있는 새 가상 네트워크 **az1000301-vnet0의** 이름:

        - 주소 공간: **10.103.0.0/16**

        - 서브넷 이름: **서브넷0**

        - 서브넷 주소 범위: **10.103.0.0/24**

    - 공용 IP: 새 공용 IP 주소 **az1000301-vm0-ip** 의 이름

    - 네트워크 보안 그룹: **기본**

    - 공용 인바운드 포트: **없음**

    - 빠른 네트워킹: **꺼짐**

    - 부팅 진단: **꺼짐**

    - OS 게스트 진단: **꺼짐**

    - 시스템 할당 관리 ID: **꺼짐**

    - 자동 종료 사용 설정: **꺼짐**

    - 백업 사용: **꺼짐**

   > **참고**: Azure VM을 프로비전할 수 있는 Azure 지역을 식별하려면 [**https://azure.microsoft.com/ko-kr/regions/offers/**](https://azure.microsoft.com/ko-kr/regions/offers/)참고하십시오.

   > **참고**: 이 랩의 두 번째 연습에서는 이 작업에서 만든 네트워크 보안 그룹을 구성합니다.

   > **참고**: 다음 작업을 진행하기 전에 배포가 완료될 때까지 기다립니다. 약 5 분이 소요됩니다.


#### 작업 2: Azure PowerShell을 사용하여 Windows Server 2016 데이터 센터를 실행하는 Azure VM을 기존 가용성 집합에 배포합니다

1. Azure 포털에서 Cloud Shell 창에서 PowerShell 세션을 시작하십시오. 

   > **참고**: 현재 Azure 구독에서 클라우드 셸을 처음 시작하는 경우 클라우드 셸 파일을 유지하도록 Azure 파일 공유를 만들라는 메시지가 표시됩니다. 이 경우 기본값을 허용하면 자동으로 생성된 리소스 그룹에서 저장소 계정이 생성됩니다.

1. 클라우드 셸 창에서 다음 명령을 실행합니다:

   ```
   $vmName = 'az1000301-vm1'
   $vmSize = 'Standard_DS1_v2'
   ```

   > **참고**: 이렇게 하면 Azure VM 이름과 크기를 지정하는 변수의 값이 설정됩니다.

1. 클라우드 셸 창에서 다음 명령을 실행합니다:

   ```
   $resourceGroup = Get-AzResourceGroup -Name 'az1000301-RG'
   $location = $resourceGroup.Location
   ```

   > **참고**: 이러한 명령은 대상 리소스 그룹과 해당 위치를 지정하는 변수의 값을 설정합니다.

1. 클라우드 셸 창에서 다음 명령을 실행합니다:

   ```
   $availabilitySet = Get-AzAvailabilitySet -ResourceGroupName $resourceGroup.ResourceGroupName -Name 'az1000301-avset0'
   $vnet = Get-AzVirtualNetwork -Name 'az1000301-vnet0' -ResourceGroupName $resourceGroup.ResourceGroupName
   $subnetid = (Get-AzVirtualNetworkSubnetConfig -Name 'subnet0' -VirtualNetwork $vnet).Id
   ```

   > **참고**: 이러한 명령은 새 Azure VM을 배포할 가용성 집합, 가상 네트워크 및 서브넷을 지정하는 변수값을 설정합니다.

1. 클라우드 셸 창에서 다음 명령을 실행합니다:

   ```
   $nsg = New-AzNetworkSecurityGroup -ResourceGroupName $resourceGroup.ResourceGroupName -Location $location -Name "$vmName-nsg"
   $pip = New-AzPublicIpAddress -Name "$vmName-ip" -ResourceGroupName $resourceGroup.ResourceGroupName -Location $location -AllocationMethod Dynamic 
   $nic = New-AzNetworkInterface -Name "$($vmName)$(Get-Random)" -ResourceGroupName $resourceGroup.ResourceGroupName -Location $location -SubnetId $subnetid -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
   ```

   > **참고**: 이러한 명령은 새 Azure VM에서 사용할 새 네트워크 보안 그룹, 공용 IP 주소 및 네트워크 인터페이스를 만듭니다

   > **참고**: 이 랩의 두 번째 연습에서는 이 작업에서 만든 네트워크 보안 그룹을 구성합니다.

1. 클라우드 셸 창에서 다음 명령을 실행합니다:

   ```
   $adminUsername = 'Student'
   $adminPassword = 'Pa55w.rd1234'
   $adminCreds = New-Object PSCredential $adminUsername, ($adminPassword | ConvertTo-SecureString -AsPlainText -Force)
   ```

   > **참고**: 이러한 명령은 새 Azure VM의 로컬 관리자 계정의 자격 증명을 지정하는 변수의 값을 설정합니다.

1. 클라우드 셸 창에서 다음 명령을 실행합니다:

   ```
   $publisherName = 'MicrosoftWindowsServer'
   $offerName = 'WindowsServer'
   $skuName = '2016-Datacenter'
   ```

   > **참고**: 이러한 명령은 새 Azure VM을 프로비전하는 데 사용할 Azure Marketplace 이미지의 속성을 지정하는 변수값을 설정합니다.

1. 클라우드 셸 창에서 다음 명령을 실행합니다:

   ```
   $osDiskType = (Get-AzResource -ResourceGroupName $resourceGroup.ResourceGroupName -ResourceType Microsoft.Compute/disks)[0].Sku.name
   ```

   > **참고**: 이 명령은 새 Azure VM의 운영 체제 디스크 유형을 지정하는 변수의 값을 설정합니다.

1. 클라우드 셸 창에서 다음 명령을 실행합니다:

   ```
   vmConfig = New-AzVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $availabilitySet.Id
   Add-AzVMNetworkInterface -VM $vmConfig -Id $nic.Id
   Set-AzVMOperatingSystem -VM $vmConfig -Windows -ComputerName $vmName -Credential $adminCreds 
   Set-AzVMSourceImage -VM $vmConfig -PublisherName $publisherName -Offer $offerName -Skus $skuName -Version 'latest'
   Set-AzVMOSDisk -VM $vmConfig -Name "$($vmName)_OsDisk_1_$(Get-Random)" -StorageAccountType $osDiskType -CreateOption fromImage
   Set-AzVMBootDiagnostic -VM $vmConfig -Disable
   ```

   > **참고**: 이들 명령은 VM 크기, VM의 가용성 집합, 네트워크 인터페이스, 컴퓨터 이름, 로컬 관리자 자격 증명, 원본 이미지, 운영 체제 디스크, 부팅 진단 설정 등, 새 Azure VM을 프로비전하는 데 사용되는 Azure VM 구성 개체의 속성을 설정합니다.

1. 클라우드 셸 창에서 다음 명령을 실행합니다:

   ```
   New-AzVM -ResourceGroupName $resourceGroup.ResourceGroupName -Location $location -VM $vmConfig
   ```

   > **참고**: 이 명령은 새 Azure VM의 배포를 시작합니다.

   > **참고**: 배포가 완료될 때까지 기다리지 말고 다음 작업으로 진행합니다.


#### 작업 3: Azure 리소스 관리자 템플릿을 사용하여 Linux를 실행하는 두 개의 Azure VM을 가용성 집합에 배포합니다

1. Azure 포털에서 **리소스 블레이드 만들기로** 이동합니다.

1. **리소스 만들기** 블레이드에서 Azure 마켓플레이스에서 **템플릿 배포** 를 검색합니다.

1. 검색 결과 목록을 사용하여 **사용자 지정 템플릿 배포** 블레이드로 이동합니다.
 
1. **사용자 지정 배포** 블레이드에서 **편집기에서 사용자 고유의 템플릿 빌드** 를 선택합니다.

1. **편집 템플릿** 블레이드에서 템플릿 파일 **Labfiles\\AZ100\\Mod03\\az-100-03_azuredeploy.json** 을 로드합니다. 

   > **참고**: 템플릿의 내용을 검토하고 Linux Ubuntu를 호스팅하는 두 개의 Azure VM을 가용성 집합으로 기존 가상 네트워크 **az1000301-vnet0** 에 배포하도록 정의합니다.

1. 템플릿을 저장하고 **사용자 지정 배포** 블레이드로 돌아갑니다. 

1. **사용자 지정 배포** 블레이드에서 **매개 변수 편집** 블레이드로 이동합니다.

1. **편집 매개 변수** 블레이드에서 매개 변수 파일 **Labfiles\\AZ100\\Mod03\\az-100-03_azuredeploy.parameters.json을 로드합니다.** 

1. 매개 변수를 저장하고 **사용자** 지정 배포 블레이드로 돌아갑니다. 

1. **사용자 지정 배포** 블레이드에서 다음 설정을 사용하고 템플릿 배포를 시작합니다.

    - 구독: 이 랩에서 사용 중인 구독의 이름

    - 리소스 그룹: 새 리소스 그룹 **az1000302-RG** 의 이름

    - 위치: 이 연습의 앞에서 선택한 동일한 Azure 지역

    - Vm 이름 접두사: **az1000302-vm**

    - Nic 이름 접두사: **az1000302-nic**

    - Pip 이름 접두사: **az1000302-ip**

    - 관리자 사용자 이름: **학생**

    - 관리자 암호: **Pa55w.rd1234**

    - 가상 네트워크 이름: **az1000301-vnet0**

    - 이미지 게시자: **정식**

    - 이미지 제공: **우분투 서버**

    - 이미지 SKU: **16.04.0-LTS**

    - Vm 크기: **Standard_DS1_v2**

   > **참고**: 다음 작업을 진행하기 전에 배포가 완료될 때까지 기다립니다. 약 5 분이 소요됩니다.

> **결과**: 이 연습을 완료한 후 Windows Server 2016 데이터 센터를 실행하는 Azure VM을 Azure 포털을 사용하여 가용성 집합에 배포하고, Windows Server 2016 데이터 센터를 실행하는 다른 Azure VM을 Azure를 사용하여 동일한 가용성 집합에 배포했습니다. PowerShell, 및 Azure 리소스 관리자 템플릿을 사용 하 여 가용성 집합에 리눅스 우분투를 실행 하는 두 개의 Azure VM을 배포 했습니다.

   > **참고**: 템플릿을 사용하여 Windows Server 2016 데이터 센터를 호스팅하는 두 개의 Azure VM을 단일 작업에 배포할 수 있습니다(Linux Ubuntu 서버를 호스팅하는 두 개의 Azure VM에서 수행한 것처럼). 이러한 Azure VM을 두 개의 별도 작업에 배포하는 이유는 Azure Portal 및 Azure PowerShell 기반 배포에 익숙해질 수 있는 기회를 제공하기 위해서입니다.


### 연습 2: Windows 및 Linux 운영 체제를 실행하는 Azure VM의 네트워킹 설정 구성
  
이 연습의 주요 작업은 다음과 같습니다:

1. Azure VM의 정적 개인 및 공용 IP 주소 구성

1. 공용 IP 주소를 통해 Windows Server 2016 데이터 센터를 실행하는 Azure VM에 연결

1. 개인 IP 주소를 통해 리눅스 우분투 서버를 실행 하는 Azure VM에 연결


#### 작업 1: Azure VM의 정적 개인 및 공용 IP 주소 구성

1. Azure 포털에서 **az1000301-vm0** 블레이드로이동합니다.

1. **az1000301-vm0** 블레이드에서 **az1000301-vm0-ip - 구성** 블레이드로 이동하여 네트워크 인터페이스에 할당된 공용 IP 주소 **az1000301-vm0-ip의** 구성을 표시합니다.

1. **az1000301-vm0-ip - 구성 블레이드에서** 공용 IP 주소 할당을 정적** 으로 변경합니다.**

   > **참고**: **az1000301-vm0** 의 네트워크 인터페이스에 할당된 공용 IP 주소를 기록해 둡니다. 이 연습의 뒷부분에서 필요할 것입니다.

1. Azure 포털에서 **az1000302-vm0** 블레이드로이동합니다.

1. **az1000302-vm0** 블레이드에서 **az1000302-vm0 - 네트워킹** 블레이드를 표시합니다.

1. **az1000302-vm0 - 네트워킹** 블레이드에서 네트워크 인터페이스의 속성을 표시하는 블레이드로 이동합니다.

1. **az1000302-vm0** 의 네트워크 인터페이스의 속성을 표시하는 블레이드에서 **ipconfig1** 블레이드로 이동합니다.

1. **ipconfig1 블레이드에서** 개인 IP 주소를 정적으로 구성하고 **10.103.0.100** 으로 설정합니다.

   > **참고**: 개인 IP 주소 할당을 변경하려면 Azure VM을 다시 시작해야 합니다.

   > **참고**: 정적 또는 동적으로 할당된 공용 및 개인 IP 주소를 통해 Azure VM에 연결할 수 있습니다. 정적 IP 할당 선택은 일반적으로 이러한 IP 주소가 IP 필터링, 라우팅과 함께 사용되거나 DNS 서버로 작동하는 Azure VM의 네트워크 인터페이스에 할당되는 시나리오에서 수행됩니다.


#### 작업 2: 공용 IP 주소를 통해 Windows Server 2016 데이터 센터를 실행하는 Azure VM에 연결

1. Azure 포털에서 **az1000301-vm0** 블레이드로이동합니다.

1. **az1000301-vm0** 블레이드에서 **az1000301-vm0 - 네트워킹** 블레이드로이동합니다.

1. **az1000301-vm0 - 네트워킹** 블레이드에서 **az1000301-vm0** 의 네트워크 인터페이스에 할당된 네트워크 보안 그룹의 인바운드 포트 규칙을 검토합니다.

   > **참고**: 기본 제공 규칙으로 구성된 기본 구성은 인터넷에서 인바운드 연결을 차단합니다(RDP 포트 TCP 3389를 통한 연결 포함).

1. 다음 설정을 사용하여 기존 네트워크 보안 그룹에 인바운드 보안 규칙을 추가합니다.

    - 출처: **아무것**

    - 소스 포트 범위: **\***

    - 대상: **아무것**

    - 대상 포트 범위: **3389**

    - 프로토콜: **TCP**

    - 액션: **허용**

    - 우선 순위: **100**

    - 이름: **허용인터넷RDP인바운드**

1. Azure 포털에서 **az1000301-vm0** 블레이드의 **개요 창** 을 표시합니다. 

1. **az1000301-vm0** 블레이드의 **개요 창** 에서 RDP 파일을 생성하고 **az1000301-vm0에** 연결하는 데 사용합니다.

1. 메시지가 표시되면 다음 자격 증명을 지정하여 인증합니다:

    - 사용자 이름: **학생**

    - 암호: **Pa55w.rd1234**


#### 작업 3: 개인 IP 주소를 통해 리눅스 우분투 서버를 실행 하는 Azure VM에 연결
 
1. RDP 세션 내에서 **az1000301-vm0,** **명령 프롬프트** 를 시작합니다.

1. 명령 프롬프트에서 다음을 실행합니다.

   ```
   nslookup az1000302-vm0
   ```

1. 출력을 검사하고 이 연습의 첫 번째 작업에서 할당한 IP 주소로 이름이 확인됩니다(**10.103.0.100**).

   > **참고**: 예상되는 상황입니다. Azure는 가상 네트워크 내에서 기본 제공 DNS 이름 확인을 제공합니다. 

1. RDP 세션 내에서 **az1000301-vm0**, 서버 관리자에서 일시적으로 **IE 강화 보안 구성** 을 사용하지 않도록 설정합니다.

1. **az1000301-vm0** 에 RDP 세션 내에서,인터넷 익스플로러를 시작하고 [**https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html**](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) 에서 **putty.exe**를 다운로드 

1. **putty.exe** 를 사용하여 **SSH** 프로토콜(TCP 22)을 통해 개인 IP 주소에서 **az1000302-vm0** 에 성공적으로 연결할 수 있는지 확인합니다.

1. 메시지가 표시되면 다음 값을 지정하여 인증합니다:

    - 사용자 이름: **학생**

    - 암호: **Pa55w.rd1234**

1. 성공적으로 인증되면 RDP 세션을 **az1000301-vm0** 로 종료합니다.

1. 랩 가상 머신에서 Azure 포털에서 **az1000302-vm0** 블레이드로이동합니다.

1. **az1000302-vm0** 블레이드에서 **az1000302-vm0 - 네트워킹** 블레이드로이동합니다.

1. **az1000302-vm0 - 네트워킹** 블레이드에서 **az1000301-vm0** 의 네트워크 인터페이스에 할당된 네트워크 보안 그룹의 인바운드 포트 규칙을 검토하여 개인 IP 주소를 통한SSH 연결이 성공한 이유를 확인합니다.

   > **참고**: 기본 제공 규칙으로 구성된 기본 구성을 사용하면 Azure 가상 네트워크 환경 내의 인바운드 연결을 허용합니다(SSH 포트 TCP 22를 통한 연결 포함).

> **결과**: 이 연습을 완료한 후 Azure VM의 정적 개인 및 공용 IP 주소를 구성하고 공용 IP 주소를 통해 Windows Server 2016 데이터 센터를 실행하는 Azure VM에 연결하고 개인 IP를 통해 Linux Ubuntu 서버를 실행하는 Azure VM에 연결했습니다. 주소


### 연습 3: Azure VM 스케일 세트 배포 및 구성

이 연습의 주요 작업은 다음과 같습니다:

1. Azure VM 스케일 집합 배포에 사용할 수 있는 DNS 이름 식별

1. Azure VM 스케일 세트 배포

1. DSC 확장을 사용하여 스케일 세트 VM에 IIS 설치


#### 작업 1: Azure VM 스케일 집합 배포에 사용할 수 있는 DNS 이름 식별

1. Azure 포털에서 Cloud Shell 창에서 PowerShell 세션을 시작하십시오. 

1. 클라우드 셸 창에서 다음 명령을 실행하여 자리 표시자 및 lt;사용자 지정 레이블> 고유 할 가능성이있는 문자열및 자리 표시자 및 lt;위치 -az1000301-RG> **az1000301-RG** 리소스 그룹을 만든 Azure 지역의 이름으로 표시됩니다.

   ```
   Test-AzDnsAvailability -DomainNameLabel <custom-label> -Location '<location-of-az1000301-RG>'
   ```

1. 명령이 **True**. 그렇지 않은 경우 &lt;custom 레이블>gt의 다른 값으로 동일한 명령을 다시 실행합니다. 명령이 **True** 를 반환할 때까지. 

1. 성공적인 결과를 가져온 &lt;custom-label&gt; 가치에 주목하십시오. 다음 작업에 필요합니다


#### 작업 2: Azure VM 스케일 세트 배포

1. Azure 포털에서 **리소스 블레이드 만들기로** 이동합니다.

1. 리소스 **블레이드 만들기에서** Azure 마켓플레이스에서 **가상 시스템 크기 집합** 을 검색합니다.

1. 검색 결과 목록을 사용하여 **가상 머신 크기 집합** 블레이드 만들기로 이동합니다.

1. **가상 머신 크기 집합 블레이드 만들기** 를 사용하여다음 설정으로 가상 시스템 스케일 집합을 배포합니다.

    - 가상 머신 스케일 세트 이름: **az1000303vmss0**

    - 운영 체제 디스크 이미지: **Windows Server 2016 Datacenter**

    - 구독: 이 랩에서 사용 중인 구독의 이름

    - 리소스 그룹: 새 리소스 그룹 **az1000303-RG** 의 이름

    - 위치: 이 랩의 이전 연습에서 선택한 것과 동일한 Azure 지역

    - 가용성 영역: **없음**

    - 사용자 이름: **학생**

    - 암호: **Pa55w.rd1234**

    - 인스턴스 수: **1**

    - 인스턴스 크기: **DS1 v2**

    - 낮은 우선 순위로 배포: **아니요**

    - 관리 디스크 사용: **예**

    - 자동 크기 조정: **비활성화**

    - 부하 균형 옵션을 선택합니다. **로드 밸런서**

    - 공용 IP 주소 이름: **az1000303vms0-ip**

    - 도메인 이름 레이블: &lt;사용자 지정 레이블&gt; 이전 작업에서 식별된 값에 입력합니다.

    - 가상 네트워크: 다음 설정이 있는 새 가상 네트워크 **az1000303-vnet0의** 이름:

        - 주소 공간: **10.203.0.0/16**

        - 서브넷 이름: **서브넷0**

        - 서브넷 주소 범위: **10.203.0.0/24**

    - 인스턴스당 공용 IP 주소: **꺼짐**

   > **참고**: 다음 작업을 진행하기 전에 배포가 완료될 때까지 기다립니다. 약 5 분이 소요됩니다.


#### 작업 3: DSC 확장을 사용하여 스케일 세트 VM에 IIS 설치

1. Azure 포털에서 **az1000303vms0** 블레이드로이동합니다.

1. **az1000303vms0** 블레이드에서확장 블레이드를 표시합니다.

1. **az1000303vms0 - 확장** 블레이드에서 다음설정으로** PowerShell 원하는 상태 구성 확장** 을 추가합니다.

   > **참고**: DSC 구성 모듈은 **Labfiles\\AZ100\\Mod03\az-100-03_install_iis_vmss.zip에서**업로드 할 수 있습니다. 이 모듈에는 IIS(웹 서버) 역할을 설치하는 DSC 구성 스크립트가 포함되어 있습니다.

    - 구성 모듈 또는 스크립트: **"az-100-03_install_iis_vmss.zip"**

    - 모듈 인증 이름: **az-100-03_install_iis_vmss.ps1\IISInstall**

    - 구성 인수: 비워 둡니다

    - 구성 데이터 PSD1 파일: 비워 둡니다

    - WMF 버전: **최신** 버전

    - 데이터 수집: **비활성화**

    - 버전: **2.76**

    - 자동 업그레이드 마이너 버전: **예**

1. **az1000303vms0 - 인스턴스** 블레이드로 이동하여 **az1000303vmss0_0** 인스턴스의 업그레이드를 시작합니다.

   > **참고**: 이 업데이트는 DSC 구성 스크립트의 응용 프로그램을 트리거합니다. 업그레이드가 완료될 때까지 기다립니다. 약 5 분이 소요됩니다. **az1000303vms0 - 인스턴스** 블레이드에서 진행 상황을 모니터링할 수 있습니다. 

1. 업그레이드가 완료되면 **az1000303vms0-ip** 블레이드로 이동합니다. 

1. **az1000303vms0-IP** 블레이드에서 **az1000303vmss0** 에 할당된 공용 IP 주소를 기록하십시오.

1. Microsoft Edge를 시작하고 이전 단계에서 식별한 공용 IP 주소로 이동합니다.

1. 브라우저에 기본 IIS 홈 페이지가 표시되는지 확인합니다. 

> **결과**: 이 연습을 완료한 후 Azure VM 규모 집합 배포에 사용할 수 있는 DNS 이름을 식별하고, Azure VM 규모 집합을 배포하고, DSC 확장을 사용하여 스케일 세트 VM에 IIS를 설치했습니다.
