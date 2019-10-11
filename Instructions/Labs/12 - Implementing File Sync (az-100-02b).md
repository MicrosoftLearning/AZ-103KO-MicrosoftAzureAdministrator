---
lab:
    title: 'Azure 파일 동기화'
    module: '모듈 12 – 데이터 서비스'
---

# 랩: Azure 파일 동기화 구현

이 실습의 모든 작업은 Azure Portal에서 수행됩니다. 단, 연습 1 및 연습 2의 단계는 Azure VM에 대한 원격 데스크톱 세션에서 수행해야합니다.

랩 파일: 

-  **Labfiles\\Module_12\\Implementing_File_Sync\\az-100-02b_azuredeploy.json**

-  **Labfiles\\Module_12\\Implementing_File_Sync\\az-100-02b_azuredeploy.parameters.json**

### 시나리오
  
Adatum Corporation은 온-프레미스 파일 서버에서 파일 공유를 호스팅합니다. 대부분의 워크로드를 Azure로 마이그레이션하려는 계획을 고려할 때 Adatum은 Azure에서 사용할 수 있는 파일 공유에 데이터를 복제하는 가장 효율적인 방법을 찾고 있습니다. 이를 구현하기 위해 Adatum은 Azure 파일 동기화를 사용합니다.

### 목표
  
이 과정을 완료하면 다음과 같은 역량을 갖추게 됩니다.

-  Azure Resource Manager 템플릿을 사용하여 Azure VM 배포

-  Azure 파일 동기화 인프라 준비

-  Azure 파일 동기화 구현 및 유효성 검사


### 연습 0: 랩 환경 준비
  
이 연습의 주요 작업은 다음과 같습니다.

1. Azure Resource Manager 템플릿을 사용하여 Azure VM 배포


#### 작업 1: Azure Resource Manager 템플릿을 사용하여 Azure VM 배포

1. 랩 가상 머신에서 Microsoft Edge를 시작하고 [**http://portal.azure.com**](http://portal.azure.com)에서 Azure Portal을 찾아보고 이 랩에서 사용하려는 Azure 구독에서 소유자 역할이 있는 Microsoft 계정을 사용하여 로그인합니다.

1. Azure Portal에서 **새로 만들기** 블레이드로 이동합니다.

1. **새로 만들기** 블레이드를 통해 Azure Marketplace에서 **템플릿 배포**를 검색합니다.

1. 검색 결과 목록을 사용하여 **사용자 지정 배포** 블레이드로 이동합니다.

1. **사용자 지정 배포** 블레이드에서 **편집기에서 사용자 고유의 템플릿 빌드**를 선택합니다.

1. **편집 템플릿** 블레이드에서 템플릿 파일 **az-100-100-02b_azuredeploy.json**을 로드합니다. 

   > **참고**: 템플릿의 내용을 검토하고 Windows Server 2016 데이터 센터를 호스팅하는 Azure VM의 배포를 정의합니다 단일 데이터 디스크.

1. 템플릿을 저장하고 **사용자 지정 배포** 블레이드로 돌아갑니다. 

1. **사용자 지정 배포** 블레이드에서 **매개 변수 편집** 블레이드로 이동합니다.

1. **편집 매개 변수** 블레이드에서 매개 변수 파일 **az-100-02b_azuredeploy.parameters.json**을 로드합니다. 

1. 매개 변수를 저장하고 **사용자 지정 배포** 블레이드로 돌아갑니다. 

1. **사용자 지정 배포** 블레이드에서 다음 설정을 사용하고 템플릿 배포를 시작합니다.

    - 구독: 이 랩에서 사용 중인 구독의 이름

    - 리소스 그룹: 새 리소스 그룹 **az1000201b-RG**의 이름

    - 위치: 랩 위치에 가장 가까운 Azure 지역의 이름 및 Azure VM을 프로비전할 수 있는 위치

    - Vm 크기: **Standard_DS1_v2**

    - Vm 이름: **az1000201b-vm1**

    - 관리자 사용자 이름: **학생**

    - 관리자 암호: **Pa55w.rd1234**

    - 가상 네트워크 이름: **az1000201b-vnet1**

   > **참고**: Azure VM을 프로비전할 수 있는 Azure 지역을 확인하려면 [**https://azure.microsoft.com/ko-kr/regions/offers/**](https://azure.microsoft.com/ko-kr/regions/offers/)를 참고하십시오.

   > **참고**: 배포가 완료될 때까지 기다리지 말고 다음 연습으로 진행합니다. 이 랩의 다음 연습에서는 이 배포에 포함된 가상 머신를 사용합니다.

   > **참고**: Azure VM **az1000201b-vm1**의 목적은시나리오에서 온-프레미스 파일 서버를 에뮬레이트하는 것입니다.

> **결과**: 이 연습을 완료한 후 이 랩의 다음 연습에서 사용할 Azure VM **az1000201b-vm1**의 템플릿 배포를 시작했습니다.


### 연습 1: Azure 파일 동기화 인프라 준비

이 연습의 주요 작업은 다음과 같습니다.

1. Azure 스토리지 계정 및 파일 공유 만들기

1. Azure 파일 동기화와 함께 사용할 Windows Server2016 준비

1. Azure 파일 동기화 평가 도구 실행


#### 작업 1: Azure 스토리지 계정 및 파일 공유 만들기

1. Azure Portal에서 **새로 만들기** 블레이드로 이동합니다.

1. **새로 만들기** 블레이드를 통해 Azure Marketplace에서 **스토리지 계정**을 검색합니다.

1. 검색 결과 목록을 사용하여 **스토리지 계정 만들기** 블레이드로 이동합니다.

1. **스토리지 계정 만들기** 블레이드에서 다음 설정으로 새 스토리지 계정을 만듭니다. 

    - 구독: 이전 작업에서 선택한 동일한 구독

    - 리소스 그룹: 새 리소스 그룹 **az1000202b-RG**의 이름

    - 스토리지 계정 이름: 소문자와 숫자로 구성된 3~24자 사이의 유효하고 고유한 이름

    - 위치: 이전 작업에서 선택한 Azure 지역의 이름

    - 성능: **표준**

    - 계정 종류: **스토리지(범용 v1)**

    - 복제: **로컬 중복 스토리지(LRS)**

    - 보안 전송 필요: **비활성화**

    - 다음에서 액세스 허용: **모든 네트워크**

    - 계층적 네임스페이스: **비활성화**

   > **참고**: 스토리지 계정이 프로비저닝될 때까지 기다린 후 다음 단계로 진행합니다.

1. Azure Portal에서 새로 프로비전된 스토리지 계정을 나타내는 블레이드로 이동합니다.

1. 스토리지 계정 블레이드에서 파일 서비스의 속성을 표시합니다.

1. 스토리지 계정 **파일** 블레이드에서 다음 설정으로 새 파일 공유를 만듭니다.

    - 이름: **az10002bshare1**

    - 할당량: 없음


#### 작업 2: Azure 파일 동기화와 함께 사용할 Windows Server2016 준비

   > **참고**: 이 작업을 시작하기 전에 연습 0에서 시작한 템플릿 배포가 완료되었는지 확인합니다. 

1. Azure Portal에서 **az1000201b-vm1** 블레이드로 이동합니다.

1. **az1000201b-vm1** 블레이드에서 RDP 프로토콜을 통해 Azure VM에 연결하고 로그인하라는 메시지가 표시되면 다음 자격 증명을 제공합니다.

    - 관리자 사용자 이름: **학생**

    - 관리자 암호: **Pa55w.rd1234**

1. 서버 관리자에서 Azure VM에 대한 RDP 세션 내에서 **파일 및 저장소 서비스** 로 이동하여 Azure VM에 연결된 데이터 디스크를 찾고, **GPT** 디스크로 초기화하고, **새 볼륨 마법사** 를 사용하여 전체 디스크를 차지하는 단일 볼륨을 다음 설정으로 만듭니다.

    - 드라이브 레터: **S**

    - 파일 시스템: **NTFS**

    - 할당 단위 크기: **기본값**

    - 볼륨 레이블: **데이터**

1. RDP 세션 내에서 Windows PowerShell 세션을 관리자로 시작합니다. 

1. Windows PowerShell 콘솔에서 다음을 실행하여 파일 공유를 설정합니다.

   ```powershell
   $directory = New-Item -Type Directory -Path 'S:\az10002bShare'

   New-SmbShare -Name $directory.Name -Path $directory.FullName -FullAccess 'Administrators' -ReadAccess Everyone   

   Copy-Item -Path 'C:\WindowsAzure\*' -Destination $directory.FullName –Recurse
   ```

   > **참고**: 파일 공유를 샘플 데이터로 채우려면 약 100MB 상당의 파일이 포함되어야 하는 *C:\\WindowsAzure* 폴더의 콘텐츠를 사용합니다.

1. Windows PowerShell 콘솔에서 다음을 실행하여 최신 Az PowerShell 모듈을 설치합니다.

   ```powershell
   Install-Module Az -AllowClobber
   ```

   > **참고**: 메시지가 표시되면 PSGallery 리포지토리에서 설치를 진행하려는지 확인합니다.


#### 작업 3: Azure 파일 동기화 평가 도구 실행

1. Windows PowerShell 콘솔에서 Azure VM에 대한 RDP 세션 내에서 다음을 실행하여 최신 버전의 패키지 관리 및 PowerShellGet을 설치합니다.

   ```powershell
   Install-Module -Name PackageManagement -Repository PSGallery -Force

   Install-Module -Name PowerShellGet -Repository PSGallery -Force
   ```

   > **참고**: 메시지가 표시되면 NuGet 공급자의 설치를 진행하려는지 확인합니다.

1. PowerShell 세션을 다시 시작합니다.

1. Windows PowerShell 콘솔에서 다음을 실행하여 Azure 파일 동기화 PowerShell 모듈을 설치합니다.

   ```powershell
   Install-Module -Name Az.StorageSync -AllowClobber -Force
   ```

1. Windows PowerShell 콘솔에서 다음을 실행하여 Azure 파일 동기화 PowerShell 모듈을 설치합니다.

   ```powershell
   Invoke-AzStorageSyncCompatibilityCheck -Path 'S:\az10002bShare'
   ```

1. 결과를 검토하고 호환성 문제가 발견되지 않았는지 확인합니다.

> **결과**: 이 연습을 완료한 후 Azure Storage 계정 및 파일 공유를 만들고, Azure 파일 동기화와 함께 사용할 Windows Server 2016을 준비하고, Azure 파일 동기화 평가 도구를 실행했습니다.


### 연습 2: Azure 파일 동기화 인프라 준비

이 연습의 주요 작업은 다음과 같습니다.

1. 스토리지 동기화 서비스 배포

1. Azure 파일 동기화 에이전트 설치

1. 스토리지 동기화 서비스로 Windows 서버 등록

1. 동기화 그룹 및 클라우드 끝점 만들기

1. 서버 끝점 만들기

1. Azure 파일 동기화 작업의 유효성 검사


#### 작업 1: 스토리지 동기화 서비스 배포

1. Azure VM에 대한 RDP 세션 내에서, 관리자에서 로컬 서버 보기로 이동하여 일시적으로 **IE 고급 보안 구성**을 끕니다.

1. Azure VM에 대한 RDP 세션 내에서, Internet Explorer를 시작하고 [**http://portal.azure.com**](http://portal.azure.com) 에서 Azure Portal을 탐색하고 이 랩에서 이전에 사용한 것과 동일한 Microsoft 계정을 사용하여 로그인합니다.

1. Azure Portal에서 **새로 만들기** 블레이드로 이동합니다.

1. **새로 만들기** 블레이드를 통해 Azure Marketplace에서 **Azure 파일 동기화**를 검색하십시오.

1. **배포 저장 동기화**블레이드로 이동 검색 결과 목록을 사용합니다.

1. **스토리지 동기화 배포** 블레이드에서 다음 설정으로 스토리지 동기화 서비스를 만듭니다.

    - 이름: **az1000202b-ss**

    - 구독: 이전 작업에서 선택한 동일한 구독

    - 리소스 그룹: 새 리소스 그룹 **az1000203b-RG**의 이름

    - 위치: 이 연습의 앞에서 스토리지 계정을 만든 Azure 지역의 이름입니다


#### 작업 2: Azure 파일 동기화 에이전트를 설치합니다.

1. RDP 세션 내에서 Internet Explorer의 다른 인스턴스를 시작하고 [**https://go.microsoft.com/fwlink/?linkid=858257**](https://go.microsoft.com/fwlink/?linkid=858257)에서 Microsoft 다운로드 센터를 찾아보고 Azure 파일 동기화 에이전트 Windows Installer 파일 **StorageSyncAgent_V6_WS2016.msi**를 다운로드합니다.

1. 다운로드가 완료되면 기본 설정으로 스토리지 동기화 에이전트 설치 마법사를 실행하여 Azure File Sync 에이전트를 설치합니다.

1. Azure 파일 동기화 에이전트 설치가 완료되면 **Azure 파일 동기화 - 서버 등록** 마법사가 자동으로 시작됩니다.


#### 작업 3: 스토리지 동기화 서비스로 Windows 서버 등록

1. **Azure 파일 동기화 - 서버 등록** 마법사의 초기 페이지에서 이 랩에서 이전에 사용한 것과 동일한 Microsoft 계정을 사용하여 로그인합니다.

1. **Azure 파일 동기화 - 서버 등록** 마법사의 **스토리지 동기화 서비스 선택** 페이지에서 등록할 다음 설정을 지정합니다.

    - Azure 구독: 이 랩에서 사용 중인 구독의 이름

    - 리소스 그룹: **az1000203b-RG**

    - 스토리지 동기화 서비스: **az1000202b-ss**

1. 메시지가 표시되면 이 랩에서 이전에 사용한 것과 동일한 Microsoft 계정을 사용하여 다시 로그인합니다.


#### 작업 4: 동기화 그룹 및 클라우드 끝점 만들기

1. Azure VM에 대한 RDP 세션에서 Azure Portal에서 **az1000202b-ss** 스토리지 동기화 서비스 블레이드로 이동합니다.

1. **az1000202b-ss** 스토리지 동기화 서비스 블레이드에서 **동기화 그룹** 블레이드로 이동하여 다음 설정으로 새 동기화 그룹을 만듭니다.

    - 동기화 그룹 이름: **az1000202b-syncgroup1**

    - Azure 구독: 이 랩에서 사용 중인 구독의 이름

    - 스토리지 계정: 이전 연습에서 생성한 스토리지 계정의 리소스 ID

    - Azure 파일 공유: **az10002bshare1**


#### 작업 5: 서버 끝점 만들기

1. rdP 세션 내에서 Azure Portal에서 **az1000202b-ss** 스토리지 동기화 서비스 블레이드에서 **az10000202b-syncgroup1** 블레이드로 이동합니다.

1. **az1000202b-syncgroup1** 블레이드에서 **서버 끝점 추가** 블레이드로 이동하여 다음 설정을 사용하여 새 서버끝점을 만듭니다.

    - 등록된 서버: **az1000201b-vm1**

    - 경로: **S:\\az10002bShare**

    - 클라우드 계층화: **사용**

        - 항상 볼륨에서 여유 공간의 지정된 비율을 유지합니다. **15**

        - 지정된 일 수 내에 액세스하거나 수정된 파일만 캐시합니다. **30**

    - 오프라인 데이터 전송: **비활성화**


#### 작업 6: Azure 파일 동기화 작업의 유효성 검사

1. Azure Portal에서 Azure VM에 대한 RDP 세션 내에서 **az1000202b-syncgroup1** 블레이드에서 서버 끝점 **az100021b-vm1**의 상태 상태를 모니터링하여 **프로비저닝**에서 **보류 중**으로 변경하고 결국 녹색 체크 표시.

   > **참고**: 몇 분 후에 다음 단계로 진행할 수 있습니다. 

1. Azure Portal에서, 랩에서 이전에 만든 스토리지 계정의 블레이드로 이동한 다음 **파일** 탭으로 전환한 다음 **az10002bshare1**을 클릭합니다.

1. **az10002bshare1** 블레이드에서 **연결**을 클릭합니다.

1. **연결** 블레이드에서 Windows 컴퓨터의 파일 공유에 연결하는 PowerShell 명령을 클립보드에 복사합니다.

1. RDP 세션 내에서 Windows PowerShell ISE 세션을 시작합니다. 

1. Windows PowerShell ISE 세션에서 스크립트 창을 열고 로컬 클립보드의 내용을 붙여넣습니다.

1. `New-PSDrive` cmdlet이 포함된 줄 끝에 `-Persist` 스위치를 추가합니다.

1. 스크립트를 실행하고 출력이 Z: 드라이브를 Azure Storage File Service 공유로 성공적으로 매핑했는지 확인합니다.

1. RDP 세션 내에서 파일 탐색기를 시작하고 Z: 드라이브로 이동한 다음 S:\\az10002bShare와 동일한 콘텐츠가 포함되어 있는지 확인합니다.

1. Z: 드라이브에 개별 폴더의 속성 창을 표시하고 보안 탭을 검토하고 항목이 S: 드라이브의 해당 폴더에 할당된 NTFS 권한을 나타냅니다.


> **결과**: 이 연습을 완료한 후 스토리지 동기화 서비스를 배포하고, Azure 파일 동기화 에이전트를 설치하고, Windows 서버를 스토리지 동기화 서비스로 등록하고, 동기화 그룹 및 클라우드 끝점을 만들고, 서버 끝점을 만들고, Azure 파일 동기화 작업의 유효성을 검사했습니다.
