---
lab:
    title: '가상 머신 및 확장 집합'
    module: '모듈 02 - Azure 가상 시스템'
---

# 랩: 가상 머신 및 확장 집합

이 랩의 모든 작업은 Azure Portal(PowerShell Cloud Shell 세션 포함)에서 수행됩니다 

   > **참고**: Cloud Shell을 사용하지 않는 경우 랩 가상 머신에 Azure PowerShell 1.2.0 모듈(또는 최신 모듈)이 설치되어 있어야 합니다. [https://docs.microsoft.com/ko-kr/powershell/azure/install-az-ps](https://docs.microsoft.com/ko-kr/powershell/azure/install-az-ps)

랩 파일: 

-  **Labfiles\\Module_02\\Deploy_and_Manage_Virtual_Machines\\az-100-03b_01_azuredeploy.json**

-  **Labfiles\\Module_02\\Deploy_and_Manage_Virtual_Machines\\az-100-03b_01_azuredeploy.parameters.json**

-  **Labfiles\\Module_02\\Deploy_and_Manage_Virtual_Machines\\az-100-03b_02_azuredeploy.json**

-  **Labfiles\\Module_02\\Deploy_and_Manage_Virtual_Machines\\az-100-03b_02_azuredeploy.parameters.json**

### 시나리오
  
Adatum Corporation은 Azure VM 및 Azure VM Scale Sets에서 실행되는 워크로드에 대한 계산 및 스토리지 리소스를 확장하려고 합니다


### 목표
  
이 과정을 완료하면 다음과 같은 역량을 갖추게 됩니다.

-  Azure Resource Manager 템플릿을 사용하여 Azure VM 및 Azure VM 확장 집합 배포

-  Azure VM의 계산 및 스토리지 리소스 구성 

-  Azure VM Scale Sets의 계산 및 스토리지 리소스 구성


### 연습 0: 랩 환경 준비
  
이 연습의 주요 작업은 다음과 같습니다.

1. Azure Resource Manager 템플릿을 사용하여 Azure VM 배포

1. Azure Resource Manager 템플릿을 사용하여 Azure VM 스케일 집합 배포


#### 작업 1: Azure Resource Manager 템플릿을 사용하여 Azure VM 배포

1. 랩 가상 머신에서 Microsoft Edge를 시작하고 [**http://portal.azure.com**](http://portal.azure.com)에서 Azure Portal을 찾아보고 이 랩에서 사용하려는 Azure 구독에서 소유자 역할이 있는 Microsoft 계정을 사용하여 로그인합니다.

1. Azure Portal에서 **리소스 만들기** 블레이드로 이동합니다.

1. **리소스 만들기** 블레이드를 통해 Azure Marketplace에서 **템플릿 배포**를 검색한 후 **템플릿 배포(고객 템플릿을 사용하여 배포)**를 선택합니다.

1. **만들기**를 클릭합니다.

1. **사용자 지정 배포** 블레이드에서 **편집기에서 사용자 고유의 템플릿을 빌드합니다.** 링크를 클릭합니다. 이 링크가 표시되지 않으면 대신 **템플릿 편집**을 클릭합니다.

1. **템플릿 편집** 블레이드에서 템플릿 파일 **az-100-03b_01_azuredeploy.json을**로드합니다. 

   > **참고**: 템플릿의 내용을 검토하고 Windows Server 2016 데이터 센터를 호스팅하는 Azure VM의 배포를 정의합니다.

1. 템플릿을 저장하고 **사용자 지정 배포** 블레이드로 돌아갑니다. 

1. **사용자 지정 배포** 블레이드에서 **매개 변수 편집** 블레이드로 이동합니다.

1. 편집 **매개 변수**블레이드에서 매개 변수 파일 **az-100-03b_01_azuredeploy.parameters.json**을 로드합니다. 

1. 매개 변수를 저장하고 **사용자 지정 배포** 블레이드로 돌아갑니다. 

1. **사용자 지정 배포** 블레이드에서 다음 설정을 사용하고 템플릿 배포를 시작합니다.

    - 구독: 이 랩에서 사용 중인 구독의 이름

    - 리소스 그룹: 새 리소스 그룹 **az1000301b-RG**의 이름

    - 위치: **미국 동부**(또는 지원되는 가까운 지역)

    - Vm 이름: **az1000301b-vm1**

    - Vm 크기: **Standard_DS2_v2**

    - 관리자 사용자 이름: **학생**

    - 관리자 암호: **Pa55w.rd1234**

   > **참고**: Azure VM을 프로비전할 수 있는 Azure 지역을 확인하려면 [https://azure.microsoft.com/ko-kr/regions/offers/](https://azure.microsoft.com/ko-kr/regions/offers/)를 참고하십시오.

   > **참고**: 배포가 완료될 때까지 기다리지 말고 이 연습의 다음 작업을 진행합니다. 이 랩의 다음 연습에서는 이 배포에 포함된 가상 머신를 사용합니다.

#### 작업 2: Azure Resource Manager 템플릿을 사용하여 Azure VM 스케일 집합 배포

1. 랩 가상 머신에서 Azure Portal에서 **리소스 만들기** 블레이드로 이동합니다.

1. **리소스 만들기** 블레이드를 통해 Azure Marketplace에서 **템플릿 배포**를 검색한 후 **템플릿 배포(고객 템플릿을 사용하여 배포)**를 선택합니다.

1. **만들기**를 클릭합니다.

1. **사용자 지정 배포** 블레이드에서 **편집기에서 사용자 고유의 템플릿을 빌드합니다.** 링크를 클릭합니다. 이 링크가 표시되지 않으면 대신 **템플릿 편집**을 클릭합니다.

1. **템플릿 편집** 블레이드에서 템플릿 파일 **az-100-03b_02_azuredeploy.json**을 로드합니다. 

   > **참고**: 템플릿의 내용을 검토하고 Windows Server 2016 데이터 센터를 호스팅하는 Azure VM 스케일 집합의 배포를 정의합니다.

1. 템플릿을 저장하고 **사용자 지정 배포** 블레이드로 돌아갑니다. 

1. **사용자 지정 배포** 블레이드에서 **매개 변수 편집** 블레이드로 이동합니다.

1. 편집 **파라미터 변수 ** 블레이드에서 매개 변수 파일 **az-100-03b_02_azuredeploy.parameters.json**을 로드합니다. 

1. 매개 변수를 저장하고 **사용자 지정 배포** 블레이드로 돌아갑니다. 

1. **사용자 지정 배포** 블레이드에서 다음 설정을 사용하고 템플릿 배포를 시작합니다.

    - 구독: 이 랩에서 사용 중인 구독의 이름

    - 리소스 그룹: 새 리소스 그룹 **az1000302b-RG**의 이름

    - 위치: 랩 위치에 가장 가까운 Azure 지역의 이름 및 Azure VM을 프로비전할 수 있는 위치

    - Vmss 이름: **az1000302bvmss1**

    - Vm 크기: **Standard_DS2_v2**

    - 관리자 사용자 이름: **학생**

    - 관리자 암호: **Pa55w.rd1234**

    - 인스턴스 수: **1**

   > **참고**: 배포가 완료될 때까지 기다리지 말고 다음 연습으로 진행합니다. 이 랩의 마지막 연습에서 이 배포에 포함된 가상 시스템 확장 집합을 사용합니다.

> **결과**: 이 연습을 완료한 후 이 랩의 다음 연습에서 사용할 Azure VM **az1000301b-vm1** 및 Azure VM 규모 집합 **az1000302bvmss1**의템플릿 배포를 시작했습니다.


### 연습 1: Azure VM의 계산 및 스토리지 리소스 구성 
  
이 연습의 주요 작업은 다음과 같습니다.

1. Azure Portal을 사용하여 Azure VM의 리소스를 수직으로 계산합니다

1. ARM 템플릿을 사용하여 Azure VM의 리소스를 수직으로 계산합니다

1. Azure VM에 데이터 디스크 연결

1. Azure VM 내에서 데이터 볼륨 구성


#### 작업 1: Azure Portal을 사용하여 Azure VM의 리소스를 수직으로 계산합니다

1. 랩 가상 머신에서 Azure Portal에서 **az1000301b-RG** 리소스그룹 블레이드로 이동합니다.

1. **az1000301b-RG** 리소스 그룹 블레이드에서 **az1000301b-vm1**가상 머신 블레이드로 이동합니다.

1. **az1000301b-vm1** 가상 머신 블레이드에서 **크기** 가상 머신 블레이드로 이동합니다.

1. **az1000301b-vm1 - 크기** 가상 머신 블레이드에서 가상 머신의 크기를 **DS3_v2** **표준**으로 늘리고 **크기 조정**을 클릭합니다.

   > **참고**: 이 크기를 사용할 수 없는 경우 다른 크기를 선택합니다. 선택할 수 있는 Azure VM 크기를 식별하려면 [https://azure.microsoft.com/ko-kr/global-infrastructure/services/](https://azure.microsoft.com/ko-kr/global-infrastructure/services/)확인하십시오.

   > **참고**: Azure VM 크기를 조정하려면 운영 체제를 다시 시작해야 합니다. 


#### 작업 2: ARM 템플릿을 사용하여 Azure VM의 리소스를 수직으로 계산합니다

1. 랩 가상 머신에서 Azure Portal에서 **az1000301b-RG** 리소스그룹 블레이드로 이동합니다.

1. **az1000301b-RG** 리소스 그룹 블레이드에서 **az1000301b-RG - 배포**블레이드로 이동합니다.

1. **az1000301b-RG - 배포** 블레이드에서 **Microsoft.Template - 개요**블레이드로 이동하여 가장 최근의 성공적인 배포를 보여 줄 수 있습니다.

1. **az1000301b-RG - 배포** 블레이드에서 **재배포** 옵션을 사용하여 **사용자 지정 배포** 블레이드로 이동합니다.

1. **사용자 지정 배포** 블레이드에서 **매개 변수 편집** 블레이드로 이동합니다.

1. **매개 변수 편집** 블레이드에서 가장 최근의 성공적인배포에 사용된 원래 매개 변수의 값을 검토합니다. 

   > **참고**: 해당 매개 변수에** securestring** 데이터 형식이 있기 때문에 **adminPassword** 매개 변수의 값은 null입니다. 

1. 매개 변수를 저장하고 **사용자 지정 배포 **블레이드로 돌아갑니다. 

1. **사용자 지정 배포** 블레이드에서 다음 설정을 사용하고 템플릿 배포를 시작합니다.

    - 구독: 이 랩에서 사용 중인 구독의 이름

    - 리소스 그룹: **az1000301b-RG**

    - 위치: 이전에 선택한 동일한 Azure 지역의 이름

    - Vm 이름: **az1000301b-vm1**

    - Vm 크기: **Standard_DS2_v2**

    - 관리자 사용자 이름: **학생**

    - 관리자 암호: **Pa55w.rd1234**

   > **참고**: 이 연습의 다음 작업을 진행하기 전에 배포가 완료될 때까지 기다립니다. 배포는 Azure VM의 크기를 원래 값으로 다시 변경하여 효과적으로 축소합니다.


#### 작업 3: Azure VM에 데이터 디스크 연결

1. Azure Portal에서 **az1000301b-vm1** 가상컴퓨터 블레이드로 이동합니다.

1. **az1000301b-vm1** 가상 머신 블레이드에서 **디스크** 블레이드로 이동합니다.

1. **az1000301b-vm1 - 디스크** 블레이드에서 **+ 데이터 디스크 추가**를 사용하고 **이름** 아래의 드롭다운 목록에서 **디스크 만들기**를 클릭합니다.

1. **관리 디스크 만들기** 블레이드에서 다음 설정을 사용하여 새 데이터 디스크를 만듭니다.

    - 이름: **az1000301b-vm1-DataDisk0**

    - 리소스 그룹: **az1000301b-RG**

    - 계정 유형: **표준 HDD**

    - 소스 유형: **없음 (빈 디스크)**

    - 크기 (GiB): **1023**

1. **az1000301b-vm1 - 디스크** 블레이드로 돌아가 새로 만든 디스크에 대한 다음 설정을 구성합니다.

    - LUN: **0**

    - 호스트 캐싱: **없음**

1. **az1000301b-vm1 - 디스크** 블레이드에서 **+ 데이터 디스크 추가** 옵션을 사용하고 **이름** 아래의 드롭다운 목록에서 **디스크 만들기**를 클릭합니다.

1. **관리 디스크 만들기** 블레이드에서 다음 설정을 사용하여 새 데이터 디스크를 만듭니다.

    - 이름: **az1000301b-vm1-DataDisk1**

    - 리소스 그룹: **az1000301b-RG**

    - 계정 유형: **표준 HDD**

    - 소스 유형: **없음 (빈 디스크)**

    - 크기 (GiB): **1023**

1. **다시 az1000301b-vm1 - 디스크**블레이드에, 새로 만든 디스크에 대한 다음 설정을 구성 :

    - LUN: **1**

    - 호스트 캐싱: **없음**

1. **저장**을 클릭합니다.

#### 작업 4: Azure VM 내에서 데이터 볼륨 구성

1. Azure Portal에서 **az1000301b-vm1 **블레이드로 이동합니다.

1. **az1000301b-vm1** 블레이드에서 RDP 프로토콜을 통해 Azure VM에 연결하고 로그인하라는 메시지가 표시되면 다음 자격 증명을 제공합니다.

    - 관리자 사용자 이름: **학생**

    - 관리자 암호: **Pa55w.rd1234**

1. RDP 세션 내의 서버 관리자에서 **파일 및 저장소 서비스**로 이동합니다.

1. **스토리지 풀**을 클릭한 다음 **원시** 스토리지를 마우스 오른쪽 단추로 클릭하고 **새 스토리지 풀**을 클릭합니다.

1. 새 스토리지 풀 마법사를 사용하여 이전 작업에 연결한 두 개의 디스크로 구성된 **StoragePool1**이라는 새 스토리지 풀을 만듭니다.

1. **결과** 페이지에서 **이 마법사가 닫히면 가상 디스크 만들기** 옆의 확인 표시를 선택한 다음 **닫기**를 클릭합니다.

1. 새 가상 디스크 마법사를 사용하여 다음 설정을 사용하여 **VirtualDisk1**이라는 새 가상 디스크를 만듭니다.
    - 엔클로저 인식: **비활성화**

    - 스토리지 레이아웃: **단순**

    - 프로비전: **고정**

    - 크기: 최대 크기

1. **결과** 페이지에서 **이 마법사가 닫히면 볼륨 만들기** 옆의 확인 표시를 선택합니다.

1. 

    - 드라이브 레터: **F**

    - 파일 시스템: **NTFS**

    - 할당 단위 크기: **기본값**

    - 볼륨 레이블: **데이터**

> **결과**: 이 연습을 완료한 후 Azure Portal을 사용하고 ARM 템플릿을 사용하고 Azure VM에 데이터 디스크를 연결하고 Azure VM 내에서 데이터 볼륨을 구성하여 Azure VM의 리소스를 수직으로 계산했습니다.


### 연습 2: Azure VM Scale Sets의 계산 및 스토리지 리소스 구성
  
이 연습의 주요 작업은 다음과 같습니다.

1. Azure Resource Manager 템플릿을 사용하여 설정된 Azure VM 규모의 리소스를 수직으로 계산합니다

1. Azure VM 스케일 집합에 데이터 디스크 연결

1. Azure VM 규모 집합의 데이터 볼륨 구성

1. Azure VM Scale Sets의 리소스 수평 계산 확장


#### 작업 1: Azure Resource Manager 템플릿을 사용하여 설정된 Azure VM 규모의 리소스를 수직으로 계산합니다

1. 랩 가상 머신에서 Azure Portal에서 **az1000302b-RG** 리소스그룹 블레이드로 이동합니다.

1. **az1000302b-RG** 리소스 그룹 블레이드에서 **az1000302b-RG - 배포**블레이드로 이동합니다.

1. **az1000302b-RG - 배포** 블레이드에서 **Microsoft.Template - 개요**블레이드로 이동하여 가장 최근의 성공적인 배포를 보여 줄 수 있습니다.

1. **az1000302b-RG - 배포** 블레이드에서 **재배포** 옵션을 사용하여 **사용자 지정 배포** 블레이드로 이동합니다.

1. **사용자 지정 배포** 블레이드에서 다음 설정을 사용하고 템플릿 배포를 시작합니다.

    - 구독: 이 랩에서 사용 중인 구독의 이름

    - 리소스 그룹: **az1000302b-RG**

    - 위치: 이전에 선택한 동일한 Azure 지역의 이름

    - Vm 이름: **az1000302bvmss1**

    - Vm 크기: 이 랩의 이전 연습에서 Azure VM을 확장하는 데 사용한 VM 크기의 이름입니다.

    - 관리자 사용자 이름: **학생**

    - 관리자 암호: **Pa55w.rd1234**

   > **참고**: 이 연습의 다음 작업을 진행하기 전에 배포가 완료될 때까지 기다립니다. 배포는 Azure VM 규모 집합의 Azure VM 인스턴스 크기를 늘립니다. 

1. Azure Portal에서 **az1000302bvms1** 블레이드로이동하여 VM 스케일 집합의 VM 인스턴스 크기가 변경되었는지 확인합니다.


#### 작업 2: Azure VM 스케일 집합에 데이터 디스크 연결

1. Azure Portal에서 Cloud Shell에서 PowerShell 세션을 시작합니다. 

   > **참고**: 현재 Azure 구독에서 Cloud Shell을 처음 시작하는 경우 Cloud Shell 파일을 유지하도록 Azure 파일 공유를 만들라는 메시지가 표시됩니다. 이 경우 기본값을 허용하면 자동으로 생성된 리소스 그룹에서 스토리지 계정이 생성됩니다.

1. Cloud Shell 창에서 다음을 실행하여 VM 축척 집합의 각 VM 인스턴스에 1개의 1GB 디스크를 연결합니다.

   ```pwsh
   $vmss = Get-AzVmss -ResourceGroupName 'az1000302b-RG' -VMScaleSetName 'az1000302bvmss1'

   Add-AzVmssDataDisk -VirtualMachineScaleSet $vmss -CreateOption Empty -Lun 1 -DiskSizeGB 128 -StorageAccountType 'Standard_LRS'

   Update-AzVmss -ResourceGroupName $vmss.ResourceGroupName -VirtualMachineScaleSet $vmss -VMScaleSetName $vmss.Name 
   ```

1. Azure Portal에서 az1000302bvmss1 VM 확장 집합의 **스토리지** 블레이드로 이동하여 데이터 디스크가 추가되었는지 확인합니다. 

#### 작업 3: Azure VM 규모 집합의 데이터 볼륨 구성
 
1. Cloud Shell 창에서 사용자 지정 스크립트 확장을 사용하여 새로 추가된 디스크의 간단한 볼륨을 구성하려면 다음을 실행합니다.

   ```pwsh
   $vmss = Get-AzVmss -ResourceGroupName 'az1000302b-RG' -VMScaleSetName 'az1000302bvmss1'

   $publicSettings = @{"fileUris" = (,"https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/prepare_vm_disks.ps1");"commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File prepare_vm_disks.ps1"}

   Add-AzVmssExtension -VirtualMachineScaleSet $vmss -Name "customScript" -Publisher "Microsoft.Compute" -Type "CustomScriptExtension" -TypeHandlerVersion 1.8 -Setting $publicSettings   
   
   Update-AzVmss -ResourceGroupName $vmss.ResourceGroupName -VirtualMachineScaleSet $vmss -VMScaleSetName $vmss.Name 
   ```

   > **참고**: 볼륨이 구성되었는지 확인하려면 RDP를 통해 설정된 VM 스케일의 VM 인스턴스에 연결합니다.

1. VM 규모 집합 앞에 있는 Azure Load Balancer 공용 IP 주소를 식별하려면 Azure Portal에서 **az1000302bvms1** 블레이드로 이동하여 **공용 IP 주소** 항목의 값을 확인합니다.

1. 연결해야 하는 포트를 식별하려면 **az1000302b-RG** 리소스 그룹 블레이드로 이동합니다.

1. **az1000302b-RG**리소스 그룹 블레이드에서 **az1000302bvms1-lb**부하 분산 장치 블레이드로 이동합니다.

1. **az1000302bvmss1-lb** 부하 분산 장치 블레이드에서 **공용 IP 주소** 설정 값은 이 작업에서 앞에서 확인한 VM 축척 집합에서 사용한 IP 주소 값과 일치합니다.

1. **az1000302bvms1-lb** 로드 밸러버 블레이드에서 **az1000302bvmss1-lb - 인바운드 NAT 규칙** 블레이드로 이동하십시오.

1. **az1000302bvmss1-lb - 인바운드 NAT 규칙** 블레이드에서 **natpool.0** 블레이드로 이동하여 포트 매핑이 **50000**으로시작한다는 점에 유의하십시오. 

1. 랩 컴퓨터에서 **시작 ->실행** 텍스트 상자(식별한 공용 IP 주소를 나타내는 경우)에서 다음을 실행하여 설정된 VM 규모의 첫 번 째 VM 인스턴스에 원격 데스크톱 연결을 시작합니다. 이 작업의 이전을 수행합니다.

   ```
   mstsc /f /v:<public_IP_address>:50000
   ```

   > **참고**: 자리 표시자 및 public_IP_address&gt;를 이 작업의 앞에서 확인한 공용 IP 주소의 값으로 바꿔야 합니다.

1. RDP를 통해 VM Scale Sets의 VM 인스턴스에 로그인하라는 메시지가 표시되면 다음 자격 증명을 제공하십시오:

    - 관리자 사용자 이름: **학생**

    - 관리자 암호: **Pa55w.rd1234**

   > **참고**: 임시 프로필로 로그인과 관련된 모든 메시지를 무시합니다. 

1. RDP 세션 내에서 파일 탐색기를 시작하고 VM에 할당된 드라이브 문자인 F: 드라이브 문자가 할당된 크기가 127GB인지 확인합니다. 

1. RDP 세션에서 로그아웃합니다.


#### 작업 4: Azure VM Scale Sets의 리소스 수평 계산 확장

1. 랩 가상 머신에서 Azure Portal에서 **az1000302bvmss1** VM Scale Sets 블레이드로 이동합니다.

1. **az1000302bvmss1** VM 확장 집합 블레이드에서 **크기 조정** 블레이드로 이동합니다.

1. **az1000302bvmss1 - 크기 조정** 블레이드에서 **재정의 조건 설정**을 사용하여 인스턴스 수를 2로 늘립니다.

1. **저장**을 클릭합니다.

1. **az1000302bvmss1로 이동 - 인스턴스** 블레이드로 이동하고 인스턴스 수가 2로 증가했는지 확인합니다.

   > **참고**: 추가 인스턴스 프로비저닝 프로세스를 보려면 **az1000302bvmss1 - 인스턴스** 블레이드에서 디스플레이를 새로 고쳐야 할 수있습니다.


> **결과**: 이 연습을 완료한 후 Azure Resource Manager 템플릿을 사용하여 설정된 Azure VM 규모의 리소스를 세로로 계산하고, Azure VM 규모 집합에 데이터 디스크를 연결한 경우, Azure VM 규모 집합에서 구성된 데이터 볼륨을 구성하고, 크기를 조정합니다. Azure VM 배율 집합의 리소스를 가로로 계산합니다.

## 연습 3: 랩 리소스 제거

#### 작업 1: Cloud Shell 열기

1. 포털 상단에서 **Cloud Shell** 아이콘을 클릭하여 Cloud Shell 창을 엽니다.

1. Cloud Shell 인터페이스에서 **Bash**를 선택합니다.

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 이 랩에서 만든 모든 리소스 그룹을 나열합니다.

   ```sh
   az group list --query "[?starts_with(name,'az1000')].name" --output tsv
   ```

1. 이 랩에서 만든 리소스 그룹만 출력에 포함되어 있는지 확인합니다. 이러한 그룹은 다음 작업에서 삭제됩니다.

#### 작업 2: 리소스 그룹 삭제

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 이 랩에서 만든 리소스 그룹을 삭제합니다.

   ```sh
   az group list --query "[?starts_with(name,'az1000')].name" --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
   ```

1. 포털 하단에 있는 **Cloud Shell** 프롬프트를 닫습니다.

> **결과**: 이 연습에서는 이 랩에서 사용한 리소스를 제거했습니다.
