---
lab:
    title: '스토리지 구현 및 관리'
    module: '모듈 03 - Azure Storage'
---

# 랩: 스토리지 구현 및 관리

이 랩의 모든 작업은 원격 데스크톱 세션에서 Azure VM에 수행된 단계를 포함하는 연습 2 작업 2를 제외하고는 Azure Portal(PowerShell Cloud Shell 세션 포함)에서 수행됩니다.

   > **참고**: Cloud Shell을 사용하지 않는 경우 랩 가상 머신에 Azure PowerShell 1.2.0 모듈(또는 최신 모듈)이 설치되어 있어야 합니다. [https://docs.microsoft.com/ko-kr/powershell/azure/install-az-ps](https://docs.microsoft.com/ko-kr/powershell/azure/install-az-ps)

랩 파일: 

-  **Labfiles\\Module_03\\Implement_and_Manage_Storage\\az-100-02_azuredeploy.json**

-  **Labfiles\\Module_03\\Implement_and_Manage_Storage\\az-100-02_azuredeploy.parameters.json**

### 시나리오
  
Adatum Corporation은 데이터를 호스팅하기 위해 Azure Storage를 활용하려고 합니다

### 목표
  
이 랩을 완료하면 다음과 같은 역량을 갖추게 됩니다.

-  Azure Resource Manager 템플릿을 사용하여 Azure VM 배포.

-  Azure Blob Storage 구현 및 사용

-  Azure File Storage 구현 및 사용


### 연습 0: 랩 환경 준비.
  
이 연습의 주요 작업은 다음과 같습니다.

1. Azure Resource Manager 템플릿을 사용하여 Azure VM 배포.


#### 작업 1: Azure Resource Manager 템플릿을 사용하여 Azure VM 배포.

1. 랩 가상 머신에서 Microsoft Edge를 시작하고 [**http://portal.azure.com**](http://portal.azure.com)에서 Azure Portal을 찾아보고 이 랩에서 사용하려는 Azure 구독에서 소유자 역할이 있는 Microsoft 계정을 사용하여 로그인합니다.

1. Azure Portal에서 **구독** 블레이드로 이동합니다.

1. **구독** 블레이드에서 Azure 구독의 속성을 표시하는 블레이드로 이동합니다.

1. 구독 속성을 표시하는 블레이드에서 **리소스 공급자** 블레이드로 이동합니다.

1. **리소스 공급자** 블레이드에서 다음 리소스 공급자를 등록합니다(이러한 리소스 공급자가 아직 등록되지 않은 경우).
- Microsoft.Network
- Microsoft.Compute
- Microsoft.Storage

**참고:** 이 단계에서는 Azure Resource Manager Microsoft.Network, Microsoft.Compute 및 Microsoft.Storage 리소스 공급자를 등록합니다. Azure Resource Manager 템플릿을 사용하여 해당 리소스 공급자가 관리하는 리소스를 배포하는 데 필요한 일회성 작업(구독당)입니다(이러한 리소스 공급자가 아직 등록되지 않은 경우).

1. Azure Portal에서 **리소스 만들기** 블레이드로 이동합니다.

1. **리소스 만들기** 블레이드를 통해 Azure Marketplace에서 **템플릿 배포**를 검색하고 **템플릿 배포(사용자 지정 템플릿을 사용하여 배포)**를 선택합니다.

1. **만들기**를 클릭합니다.

1. **사용자 지정 배포** 블레이드에서 **편집기에서 사용자 고유의 템플릿을 빌드합니다.** 링크를 클릭합니다. 이 링크가 표시되지 않으면 대신 **템플릿 편집**을 클릭합니다.

1. **편집 템플릿** 블레이드에서 템플릿 파일 **Labfiles\\Module_03\\Implement_and_Manage_Storage\\az-100-02_azuredeploy.json**을 로드합니다. 

   > **참고**: 템플릿의 내용을 검토하고 Windows Server 2016 데이터 센터를 호스팅하는 Azure VM의 배포를 정의한다는 것을 확인하세요.

1. 템플릿을 저장하고 **사용자 지정 배포** 블레이드로 돌아갑니다. 

1. **사용자 지정 배포** 블레이드에서 **매개 변수 편집** 블레이드로 이동합니다.

1. **편집 매개 변수** 블레이드에서 매개 변수 파일 **Labfiles\\Module_03\\Implement_and_Manage_Storage\\az-100-02_azuredeploy.parameters.json**을 로드합니다. 

1. 매개 변수를 저장하고 **사용자 지정 배포 **블레이드로 돌아갑니다. 

1. **사용자 지정 배포** 블레이드에서 다음 설정을 사용해 템플릿 배포를 시작합니다.

    - 구독: 이 랩에서 사용 중인 구독의 이름.

    - 리소스 그룹: 새 리소스 그룹 **az1000201-RG**의 이름.

    - 위치: 랩 위치에 가장 가까운 Azure 지역의 이름 및 Azure VM을 프로비전할 수 있는 위치

    - VM 크기: 강사의 권장 사항에 따라 **Standard_DS1_v2** 또는 **Standard_DS2_v2**를 사용합니다.

    - V 이름: **az1000201-vm1**

    - 관리자 사용자 이름: **학생**

    - 관리자 암호: **Pa55w.rd1234**

    - 가상 네트워크 이름: **az1000201-vnet1**

   > **참고**: Azure VM을 프로비전할 수 있는 Azure 지역을 확인하려면 [**https://azure.microsoft.com/ko-kr/regions/offers/**](https://azure.microsoft.com/ko-kr/regions/offers/)을 참고하십시오.

   > **참고**: 배포가 완료될 때까지 기다리지 말고 다음 연습을 진행합니다. 이 랩의 두 번째 연습에서 가상 머신 **az1000201-vm1**을 사용합니다.

> **결과**: 이 랩을 마친 후에는 이 랩의 두 번째 연습에서 사용할 Azure VM **az1000201-vm1**의 템플릿 배포를 시작했습니다.


### 연습 1: Azure Blob Storage 구현 및 사용.

이 연습의 주요 작업은 다음과 같습니다.

1. Azure Storage 계정 만들기.

1. Azure Storage 계정의 구성 설정 검토.

1. Azure Storage Blob 서비스 관리.

1. Azure Storage 계정 간에 컨테이너 및 Blob 복사.

1. SAS(공유 액세스 서명) 키를 사용하여 BLOB에 액세스.


#### 작업 1: Azure Storage 계정 만들기.

1. Azure Portal에서 **리소스 만들기** 블레이드로 이동합니다.

1. **리소스 생성** 블레이드에서 Azure Marketplace에서 **스토리지 계정**을 검색하세요.

1. 검색 결과 목록을 사용하여 **스토리지 계정 만들기** 블레이드로 이동합니다.

1. **스토리지 계정 만들기** 블레이드에서 다음 설정으로 새 스토리지 계정을 만듭니다. 

    - 구독: 이전 작업에서 선택한 것과 동일한 구독

    - 리소스 그룹: 새 리소스 그룹 **az1000202-RG**의 이름

    - 스토리지 계정 이름: 소문자와 숫자로 구성된 3~24자 사이의 유효하고 고유한 이름.

    - 위치: 이전 작업에서 선택한 Azure 지역의 이름.

    - 성능: **표준**

    - 계정 종류: **저장소 (다용도 v1)**

    - 복제: **LRS(로컬 중복 스토리지)**

1. **검토 + 만들기**를 클릭한 다음 **만들기**를 클릭합니다.

1. 스토리지 계정이 프로비저닝 될 때까지 기다리지 말고 다음 단계로 진행하세요.

1. Azure Portal에서 **리소스 만들기** 블레이드로 이동합니다.

1. **리소스 생성** 블레이드에서 Azure Marketplace에서 **스토리지 계정**을 검색하세요.

1. 검색 결과 목록을 사용하여 **스토리지 계정 만들기** 블레이드로 이동합니다.

1. **스토리지 계정 만들기** 블레이드에서 다음 설정으로 새 스토리지 계정을 만듭니다. 

    - 구독: 이전 작업에서 선택한 것과 동일한 구독

    - 리소스 그룹: 새 리소스 그룹 **az1000203-RG**의 이름

    - 스토리지 계정 이름: 소문자와 숫자로 구성된 3~24자 사이의 유효하고 고유한 이름.

    - 위치: 첫 번째 스토리지 계정을 만들 때 선택한 것과 다른 Azure 지역의 이름.

    - 성능: **표준**

    - 계정 종류: **StorageV2(범용 v2)**

    - 액세스 계층: **핫**

    - 복제: **GRS(지리적 중복 장치)**

1. **검토 + 만들기**를 클릭한 후 **만들기**를 클릭합니다.

1. 스토리지 계정이 프로비전될 때까지 기다립니다. 이 작업은 1분 미만이 소요됩니다.


#### 작업 2: Azure Storage 계정의 구성 설정 검토.
  
1. Azure Portal에서 만든 첫 번째 스토리지 계정의 블레이드로 이동합니다. 

1. 스토리지 계정 블레이드를 열고 성능, 복제 및 계정 종류 설정을 포함하여 **개요** 섹션의 스토리지 계정 구성을 검토합니다.

1. **액세스 키** 블레이드를 표시합니다. 스토리지 계정 이름 값과 key1 및 key2 값을 복사할 수 있습니다. 각 키를 다시 생성할 수도 있습니다.

1. 스토리지 계정의 **구성** 블레이드를 표시합니다.

1. **구성** 블레이드에서 **범용 v2** 계정으로 업그레이드를 수행할 수 있고, 보안 전송을 적용할 수 있고, 복제 설정을 **GRS(지리적 중복 저장소)** 또는 **RA-GRS(읽기 액세스 지리적 중복 저장소)**로 변경할 수 있습니다. 그러나 성능 설정을 변경할 수 없습니다(이 설정은 스토리지 계정을 만들 때만 할당할 수 있음).

1. 스토리지 계정의 **암호화** 블레이드를 표시합니다. 암호화는 기본적으로 활성화되어 있으며 사용자 고유의 키를 사용할 수 있습니다.

   > **참고**: 스토리지 계정의 구성을 변경하지 마세요. 

1. Azure Portal에서 만든 두 번째 스토리지 계정의 블레이드로 이동합니다. 

1. 스토리지 계정 블레이드를 열면 성능, 복제 및 계정 종류 설정을 포함하여 **개요** 섹션의 스토리지 계정 구성을 검토합니다.

1. 스토리지 계정의 **구성** 블레이드를 표시합니다.

1. **구성** 블레이드에서 보안 전송 요구 사항을 비활성화할 수 있고, 기본 액세스 계층을 **Cool**로 설정할 수 있고, 복제 설정을 **LRS(로컬 중복 저장소)** 또는 **RA-GRS(읽기 액세스 지리적 중복 저장소)**로 변경할 수 있습니다. 이 경우 성능 설정도 변경할 수 없습니다.

1. 스토리지 계정의 **암호화** 블레이드를 표시합니다. 이 경우 암호화도 기본적으로 활성화되어 있으며 사용자 고유의 키를 사용할 수 있습니다.


#### 작업 3: Azure Storage Blob 서비스 관리.

1. Azure Portal에서 첫 번째로 생성한 스토리지 계정의 **컨테이너** 블레이드로 이동합니다. 

1. 첫 번째 스토리지 계정의 **컨테이너** 블레이드에서 **공용 액세스 수준**이 **개인(익명 액세스 없음)**으로 설정된 **az1000202-container**라는 새 컨테이너를 만듭니다. 

1. **az1000202-container** 블레이드에서 **Labfiles\\Module_03\\Implement_and_Manage_Storage\\az-100-02_azuredeploy.json** 및 **Labfiles\\Module_03\\Implement_and_Manage_Storage\\az-100-02_azuredeploy.parameters.json**을 컨테이너에 업로드합니다.


#### 작업 4: Azure Storage 계정 간에 컨테이너 및 Blob 복사.

1. Azure Portal에서 Cloud Shell 창에서 PowerShell 세션을 시작하세요. 

   > **참고**: 현재의 Azure 구독에서 Cloud Shell을 처음 시작하는 경우, Cloud Shell 파일을 유지하도록 Azure 파일 공유를 만들라는 메시지가 표시됩니다. 이 경우 기본값을 허용하면 자동으로 생성된 리소스 그룹에서 스토리지 계정이 생성됩니다.

1. Cloud Shell 창에서 다음 명령을 실행합니다.

```pwsh
   $containerName = 'az1000202-container'
   $storageAccount1Name = (Get-AzStorageAccount -ResourceGroupName 'az1000202-RG')[0].StorageAccountName
   $storageAccount2Name = (Get-AzStorageAccount -ResourceGroupName 'az1000203-RG')[0].StorageAccountName
   $storageAccount1Key1 = (Get-AzStorageAccountKey -ResourceGroupName 'az1000202-RG' -StorageAccountName $storageAccount1Name)[0].Value
   $storageAccount2Key1 = (Get-AzStorageAccountKey -ResourceGroupName 'az1000203-RG' -StorageAccountName $storageAccount2Name)[0].Value
   $context1 = New-AzStorageContext -StorageAccountName $storageAccount1Name -StorageAccountKey $storageAccount1Key1
   $context2 = New-AzStorageContext -StorageAccountName $storageAccount2Name -StorageAccountKey $storageAccount2Key1
```
   > **참고**: 이 명령은 이전 작업에서 업로드한 Blob이 들어 있는 Blob 컨테이너의 이름, 두 개의 스토리지 계정, 해당 키 및 각 키에 대한 해당 보안 컨텍스트를 나타내는 변수 값을 설정합니다. 이러한 값을 사용하여 AZCopy 명령줄 유틸리티를 사용하여 스토리지 계정 간에 Blob을 복사하는 SAS 토큰을 생성합니다.

1. Cloud Shell 창에서 다음 명령을 실행합니다:

```pwsh
   New-AzStorageContainer -Name $containerName -Context $context2 -Permission Off
```
   > **참고**: 이 명령은 두 번째 스토리지 계정에 일치하는 이름으로 새 컨테이너를 만듭니다.
   
1. Cloud Shell 창에서 다음 명령을 실행합니다:

```pwsh
   $containerToken1 = New-AzStorageContainerSASToken -Context $context1 -ExpiryTime(get-date).AddHours(24) -FullUri -Name $containerName -Permission rwdl
   $containerToken2 = New-AzStorageContainerSASToken -Context $context2 -ExpiryTime(get-date).AddHours(24) -FullUri -Name $containerName -Permission rwdl
```
   > **참고**: 이러한 명령은 다음 단계에서 두 컨테이너 간에 Blob을 복사하는 데 사용할 SAS 키를 생성합니다.
   
1. Cloud Shell 창에서 다음 명령을 실행합니다:

```pwsh
   azcopy cp $containerToken1 $containerToken2 --recursive=true
```

   > **참고**: 이 명령은 AzCopy 유틸리티를 사용하여 두 저장소 계정 간에 컨테이너의 내용을 복사합니다. 

1. 명령이 두 파일이 전송되었는지 확인하는 결과를 반환했는지 확인합니다. 

1. 두 번째 스토리지 계정의 **Blobs** 블레이드로 이동하여 새로 만든 **az1000202-container**를 나타내는 항목이 포함되어있고 컨테이너에 복사된 Blob 두 개가 포함되어 있는지 확인합니다.


#### 작업 5: SAS(공유 액세스 서명) 키를 사용하여 BLOB에 액세스.

1. 두 번째 스토리지 계정의 **컨테이너** 블레이드에서 컨테이너 **az1000202-container**로 이동한다음 **az-100-02_azuredeploy.json** 블레이드를 엽니다.

1. **az-100-02_azuredeploy.json** 블레이드에서 **URL** 속성의 값을 복사합니다.

1. 다른 Microsoft Edge 창을 열고 이전 단계에서 복사한 URL로 이동합니다. 

   > **참고**: 브라우저에 **ResourceNotFound**가 표시됩니다. 컨테이너에 **공용 액세스 수준**이 **개인(익명 액세스 없음)**으로 설정되어 있기 때문에 예상된 결과입니다.

1. **az-100-02_azuredeploy.json** 블레이드에서 다음 설정을 통해 SAS(공유 액세스 서명) 및 해당 URL을 생성합니다.

    - 사용 권한: **읽기**

    - 시작 날짜/시간: 현재 표준 시간대의 현재 날짜/시간 지정.

    - 만료 날짜/시간: 현재 시간보다 24시간 앞선 날짜/시간 지정.

    - 허용된 IP 주소: 비워 둡니다

    - 허용된 프로토콜: **HTTP**

    - 서명 키: **키 1**

1. **az-100-02_azuredeploy.json** 블레이드에서 **Blob SAS URL**을 복사합니다.

1. 이전에 열려 있는 Microsoft Edge 창에서 이전 단계에서 복사한 URL로 이동합니다. 

   > **참고**: 이번에는 **az-100-02_azuredeploy.json**을 열거나 저장할지 묻는 메시지가 표시됩니다. 이번에는 컨테이너에 익명으로 더 이상 액세스하지 않고 다음 24시간 동안 유효한 새로 생성된 SAS 키를 사용하기 때문에 예상된 결과입니다.

1. 프롬프트가 표시되는 Microsoft Edge 창을 닫습니다. 

> **결과**: 이 연습을 완료한 후 두 개의 Azure Storage 계정을 만들고, 구성 설정을 검토하고, Blob 컨테이너를 만들고, 컨테이너에 Blob을 업로드하고, 스토리지 계정 간에 컨테이너 및 Blob을 복사하고, SAS 키를 사용하여 Blob 중 하나에 액세스할 수 있습니다. 


### 연습 2: Azure File Storage 구현 및 사용.

이 연습의 주요 작업은 다음과 같습니다.

1. Azure 파일 서비스 공유 만들기.

1. Azure VM에서 드라이브를 Azure 파일 서비스 공유로 매핑.


#### 작업 1: Azure 파일 서비스 공유 만들기.
  
1. Azure Portal에서 이전 연습에서 만든 두 번째 스토리지 계정의 속성을 표시하는 블레이드로 이동합니다.

1. 스토리지 계정 블레이드에서 파일 서비스 아래의 **파일 공유**를 선택합니다.

1. 스토리지 계정 **파일 공유**블레이드에서 다음 설정을 사용하여 새 파일 공유를 만듭니다.

    - 이름: **az10002share1**

    - 할당량: **5GB**


#### 작업 2: Azure VM에서 드라이브를 Azure 파일 서비스 공유로 매핑.

   > **참고**: 이 작업을 시작하기 전에 연습 0에서 시작한 템플릿 배포가 완료되었는지 확인합니다. 

1. **az10002share1** 블레이드로 이동하고 **연결**블레이드를 표시합니다.

1. **연결** 블레이드에서 Windows 컴퓨터의 파일 공유에 연결하는 PowerShell 명령을 클립보드에 복사합니다.

1. Azure Portal에서 **az1000201-vm1** 블레이드로 이동합니다.

1. **az1000201-vm1** 블레이드에서 RDP 프로토콜을 통해 Azure VM에 연결하고 로그인하라는 메시지가 표시되면 다음 자격 증명을 제공합니다.

    - 관리자 사용자 이름: **학생**

    - 관리자 암호: **Pa55w.rd1234**

1. RDP 세션 내에서 Windows PowerShell ISE 세션을 시작합니다. 

1. Windows PowerShell ISE 세션에서 스크립트 창을 열고 로컬 클립보드의 내용을 붙여넣습니다.

1. 스크립트를 PowerShell ISE 세션에 붙여 넣고 스크립트 끝에``-Persist``를 추가하고 스크립트를 실행한 다음 해당 출력이 Z: 드라이브를 Azure Storage File Service 공유에 성공적으로 매핑했는지 확인합니다.

1. 시작 메뉴를 마우스 오른쪽 단추로 클릭하고 **실행**을 클릭한 다음, **열기** 대화 상자에서 **Z:**를 입력하고 **Enter** 키를 누릅니다. 그러면 파일 탐색기 창에 **Z:** 드라이브의 내용이 표시됩니다.
1. File Explorer 창에서 Z: 드라이브에 **Folder1** 라는 이름의 폴더를 만듭니다.

1. 파일 탐색기 창에서 **Folder1**로 이동하여 **File1.txt**라는 텍스트 문서를 만듭니다. 

   > **참고**: **File1.txt.txt**라는 파일을 만들지 않도록 알려진 파일 확장명이 표시되지 않는 파일 탐색기의 기본 구성을 고려해야 합니다.

1. PowerShell 프롬프트에서 컨텍스트를 매핑된 드라이브로 변경하려면 **Z:** 디렉터리 입력합니다. 

1. PowerShell 프롬프트에서 드라이브의 콘텐츠를 나열하려면 **dir**을 입력합니다. 파일 탐색기에서 만든 디렉터리가 표시됩니다.

1. PowerShell 프롬프트에서 **cd Folder1**을 입력하여 디렉터리를 폴더로 변경합니다. **dir** 명령을 다시 실행하여 파일 콘텐츠를 나열합니다.

> **결과**: 이 연습을 완료한 후 Azure 파일 서비스 공유를 만들고, Azure VM에서 파일 공유에 드라이브를 매핑하고, Azure VM의 파일 탐색기를 사용하여 파일 공유에 폴더와 파일을 만들었습니다.

## 연습 3: 랩 리소스 제거

#### 작업 1: Cloud Shell 열기

1. 포털 상단에서 **Cloud Shell** 아이콘을 클릭하여 Cloud Shell 창을 엽니다.

1. Cloud Shell 인터페이스에서 **Bash**를 선택합니다.

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 이 랩에서 만든 모든 리소스 그룹을 나열합니다.

```sh
   az group list --query "[?starts_with(name,'az1000')].name" --output tsv
```

1. 이 랩에서 만든 리소스 그룹만 출력에 포함되어 있는지 확인합니다. 다음 태스크에서 이러한 그룹을 삭제합니다.

#### 작업 2: 리소스 그룹 삭제

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 이 랩에서 만든 리소스 그룹을 삭제합니다.

```sh
   az group list --query "[?starts_with(name,'az1000')].name" --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
```

1. 포털 하단에 있는 **Cloud Shell** 프롬프트를 닫습니다.

> **결과**: 이 연습에서는 이 랩에서 사용한 리소스를 제거했습니다.
