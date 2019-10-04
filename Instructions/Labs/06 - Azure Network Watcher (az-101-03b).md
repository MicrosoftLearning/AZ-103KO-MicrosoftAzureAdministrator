---
lab:
    title: 'Azure Network Watcher'
    module: '모듈 06 - 모니터링'
---

# 랩: 네트워크 연결 모니터링 및 문제 해결을 위해 Azure Network Watcher 사용

이 랩의 모든 작업은 Azure Portal(PowerShell Cloud Shell 세션 포함)에서 수행됩니다  

   > **참고**: Cloud Shell을 사용하지 않는 경우 랩 가상 머신에 Azure PowerShell 1.2.0 모듈(또는 최신 모듈)이 설치되어 있어야 합니다. [https://docs.microsoft.com/ko-kr/powershell/azure/install-az-ps](https://docs.microsoft.com/ko-kr/powershell/azure/install-az-ps)

랩 파일: 

-  **Labfiles\\Module_06\\Network_Watcher\\az-101-03b_01_azuredeploy.json**

-  **Labfiles\\Module_06\\Network_Watcher\\az-101-03b_02_azuredeploy.json**

-  **Labfiles\\Module_06\\Network_Watcher\\az-101-03b_01_azuredeploy.parameters.json**

-  **Labfiles\\Module_06\\Network_Watcher\\az-101-03b_02_azuredeploy.parameters.json**


### 시나리오
  
Adatum Corporation은 Azure Network Watcher를 사용하여 Azure 가상 네트워크 연결을 모니터링하려고 합니다.


### 목표
  
이 과정을 완료하면 다음과 같은 역량을 갖추게 됩니다.

-  Azure Resource Manager 템플릿을 사용하여 Azure VM, Azure Storage 계정 및 Azure SQL Database 인스턴스 배포

-  Azure Network Watcher를 사용하여 네트워크 연결 모니터링


### 연습 1: Azure Network Watcher 기반 모니터링을 위한 인프라 준비
  
이 연습의 주요 작업은 다음과 같습니다.

1. Azure Resource Manager 템플릿을 사용하여 Azure VM, Azure Storage 계정 및 Azure SQL Database 인스턴스 배포

1. Azure Network Watcher 서비스 사용

1. Azure 가상 네트워크 간의 피어링 설정

1. Azure Storage 계정 및 Azure SQL Database 인스턴스에 서비스 엔드포인트를 설정합니다.


#### 작업 1: Azure Resource Manager 템플릿을 사용하여 Azure VM, Azure Storage 계정 및 Azure SQL Database 인스턴스 배포

1. 랩 가상 머신에서 Microsoft Edge를 시작하고 [**http://portal.azure.com**](http://portal.azure.com)에서 Azure Portal을 탐색하고 대상 Azure 가입에서 소유자 역할을 가진 Microsoft 계정을 사용하여 로그인합니다.

1. Azure Portal에서 **리소스 만들기** 블레이드로 이동합니다.

1. **리소스 만들기** 블레이드를 통해 Azure Marketplace에서 **템플릿 배포**를 검색합니다.

1. 결과 목록에서 **템플릿 배포(사용자 지정 템플릿을 사용하여 배포)**로 이동하고 **만들기**를 클릭합니다.

1. **사용자 지정 배포** 블레이드에서 **편집기에서 사용자 고유의 템플릿을 빌드합니다.** 링크를 클릭합니다. 이 링크가 표시되지 않으면 대신 **템플릿 편집**을 클릭합니다.

1. **템플릿 편집** 블레이드에서 템플릿 파일 **az-101-03b_01_azuredeploy.json을**로드합니다. 

   > **참고**: 템플릿의 내용을 검토하고 Azure VM, Azure SQL 데이터베이스 및 Azure Storage 계정의 배포를 정의합니다.

1. 템플릿을 저장하고 **사용자 지정 배포** 블레이드로 돌아갑니다. 

1. **사용자 지정 배포** 블레이드에서 **매개 변수 편집** 블레이드로 이동합니다.

1. **편집 매개 변수** 블레이드에서 매개 변수 파일 **az-101-03b_01_azuredeploy.parameters.json**을 로드합니다. 

1. 매개 변수를 저장하고 **사용자 지정 배포** 블레이드로 돌아갑니다. 

1. **사용자 지정 배포** 블레이드에서 다음 설정을 사용하고 템플릿 배포를 시작합니다.

    - 구독: 이 실험실에서 사용하려는 구독의 이름

    - 리소스 그룹: 새 리소스 그룹 **az1010301b-RG**의 이름

    - 위치: 랩 위치에 가장 가까운 Azure 지역의 이름및 Azure VM 및 Azure SQL Database를 프로비전할 수 있는 위치

    - Vm 크기: **Standard_DS2_v2**

    - Vm 이름: **az1010301b-vm1**

    - 관리자 사용자 이름: **학생**

    - 관리자 암호: **Pa55w.rd1234**

    - 가상 네트워크 이름: **az1010301b-vnet1**

    - SQL 로그인 이름: **학생**

    - SQL 로그인 암호: **Pa55w.rd1234**

    - 데이터베이스 이름: **az1010301b-db1**

    - 스쿠 이름: **기본**

    - 스쿠 티어: **기본**

   > **참고**: 지정된 지역에서 구독에서 사용할 수 있는 VM 크기를 식별하려면 Cloud Shell에서 다음을 실행하고 **제한** 열의 값을 검토합니다(여기서 &lt;location&gt;는 대상 Azure 지역을 나타냅니다).
   
   ```pwsh
   Get-AzComputeResourceSku | where {$_.Locations -icontains "<location>"} | Where-Object {($_.ResourceType -ilike "virtualMachines")}
   ```
   
   > **참고**: 지정된 지역에서 Azure SQL Database를 프로비전할 수 있는지 여부를 확인하려면 Cloud Shell에서 다음을 실행하고 결과 **상태**가 **사용 가능**으로 설정되어 있는지 확인합니다(여기서 &lt;location&gt;는 대상 Azure 지역을 나타냅니다).

   ```pwsh
   Get-AzSqlCapability -LocationName <regionname>
   ```
   
   > **참고**: 배포가 완료될 때까지 기다리지 말고 다음 단계로 진행합니다. 

1. Azure Portal에서 **리소스 만들기** 블레이드로 이동합니다.

1. **리소스 만들기** 블레이드를 통해 Azure Marketplace에서 **템플릿 배포**를 검색합니다.

1. 결과에서 **템플릿 배포(사용자 지정 템플릿을 사용하여 배포)**로 이동하고 **만들기**를 클릭합니다.

1. **사용자 지정 배포** 블레이드에서 **편집기에서 사용자 고유의 템플릿을 빌드합니다.** 링크를 클릭합니다. 이 링크가 표시되지 않으면 대신 **템플릿 편집**을 클릭합니다.

1. **템플릿 편집** 블레이드에서 템플릿 파일 **az-101-03b_02_azuredeploy.json**을 로드합니다. 

   > **참고**: 템플릿의 내용을 검토하고 Azure VM의 배포를 정의합니다.

1. 템플릿을 저장하고 **사용자 지정 배포** 블레이드로 돌아갑니다. 

1. **사용자 지정 배포** 블레이드에서 **매개 변수 편집** 블레이드로 이동합니다.

1. 편집 **파라미터 변수 ** 블레이드에서 매개 변수 파일 **az-101-03b_02_azuredeploy.parameters.json**을 로드합니다. 

1. 매개 변수를 저장하고 **사용자 지정 배포** 블레이드로 돌아갑니다. 

1. **사용자 지정 배포** 블레이드에서 다음 설정을 사용하고 템플릿 배포를 시작합니다.

    - 구독: 이 랩에서 사용 중인 구독의 이름

    - 리소스 그룹: 새 리소스 그룹 **az1010302b-RG**의 이름

    - 위치: Azure VM을 프로비전할 수 있지만 이전 배포 중에 선택한 것과 **다른** Azure 지역의 이름입니다. 

    - Vm 크기: **Standard_DS2_v2**

    - Vm 이름: **az1010302b-vm2**

    - 관리자 사용자 이름: **학생**

    - 관리자 암호: **Pa55w.rd1234**

    - 가상 네트워크 이름: **az1010302b-vnet2**

   > **참고**: 이 배포에 대해 다른 Azure 지역을 선택해야 합니다.

   > **참고**: 배포가 완료될 때까지 기다리지 말고 다음 단계로 진행합니다. 


#### 작업 2: Azure Network Watcher 서비스 사용

1. Azure Portal에서 **모든 서비스** 블레이드의 검색 텍스트 상자를 사용하여 **Network Watcher** 블레이드로 이동합니다.

2. **Network Watcher**블레이드에서 이전 작업에 리소스를 배포한 두 Azure 지역에서 Network Watcher가 활성화되어 있는지 확인하고 그렇지 않은 경우 사용하도록 설정합니다.


#### 작업 3: Azure 가상 네트워크 간의 피어링 설정

   > **참고**: 이 작업을 시작하기 전에 이 연습의 첫 번째 작업에서 시작한 템플릿 배포가 완료되었는지 확인합니다. 

1. Azure Portal에서 **az1010301b-vnet1 **가상 네트워크 블레이드로 이동합니다.

1. **az1010301b-vnet1** 가상 네트워크 블레이드에서 **az1010301b-vnet1 - 피어링** 블레이드를 표시합니다.

1. **az1010301b-vnet1 - 피어링**블레이드에서 다음 설정으로 VNet 피어링을 만듭니다.

    - 이름: **az1010301b-vnet1-to-az1010302b-vnet2**

    - 가상 네트워크 배포 모델: **Resource Manager**

    - 구독: 이 랩에서 사용 중인 Azure 구독의 이름

    - 가상 네트워크: **az1010302b-vnet2**

    - 피어링 이름을 az1010302b-vnet2에서 az1010301b-vnet1로 지정합니다. **az1010302b-vnet2-to-az1010301b-vnet1**

    - 가상 네트워크 액세스 허용: **사용**

    - 전달된 트래픽 허용: **사용 안 함**

    - 게이트웨이 전송 허용: 사용 안 함

    > **참고**: Azure Portal을 사용하면 피어링의 양방향을 동시에 구성할 수 있습니다. 다른 관리 도구를 사용하는 경우 각 방향을 독립적으로 구성해야 합니다. 

#### 작업 4: Azure Storage 계정 및 Azure SQL Database 인스턴스에 서비스 엔드포인트를 설정합니다.

1. Azure Portal에서 **az1010301b-vnet1 **가상 네트워크 블레이드로 이동합니다.

1. **az1010301b-vnet1** 가상 네트워크 블레이드에서 **서비스 엔드포인트** 블레이드를 표시합니다.

1. **az1010301b-vnet1 - 서비스 엔드포인트** 블레이드에서 다음 설정으로 서비스 엔드포인트를 추가합니다.

    - 서비스: **Microsoft.Storage**

    - 서브넷: **서브넷0**

1. 이 단계를 반복하여 두 번째 서비스 엔드포인트를 만듭니다.

    - 서비스: **Microsoft.Sql**

    - 서브넷: **서브넷0**

1. Azure Portal에서 **az1010301b-RG **리소스그룹 블레이드로 이동합니다.

1. **az1010301b-RG** 리소스 그룹 블레이드에서 리소스 그룹에 포함된 스토리지 계정의 블레이드로 이동합니다. 

1. 스토리지 계정 블레이드에서 **방화벽 및 가상 네트워크** 블레이드로 이동합니다.

1. 스토리지 계정의 **방화벽 및 가상 네트워크** 블레이드에서 다음 설정을 구성합니다.

    - 다음에서 액세스 허용: **선택한 네트워크**

    - 가상 네트워크: 

        - 가상 네트워크: **az1010301b-vnet1**

            - 서브넷: **subnet0**

    - 방화벽: 

        - 주소 범위: 없음

    - 예외: 

        - 신뢰할 수 있는 Microsoft 서비스가 이 스토리지 계정에 액세스할 수 있도록 허용합니다. **사용**

        - 모든 네트워크에서 스토리지 로깅에 대한 읽기 액세스 허용: **비활성화**

        - 모든 네트워크에서 스토리지 메트릭에 대한 읽기 액세스 허용: **비활성화**

1. Azure Portal에서 **az1010301b-RG **리소스그룹 블레이드로 이동합니다.

1. **az1010301b-RG** 리소스 그룹 블레이드에서 **az1010301b** Azure SQL Server 블레이드로 이동합니다. 

1. Azure SQL Server 블레이드에서 서버의 **방화벽 및 가상 네트워크** 블레이드로 이동합니다.

1. Azure SQL Database 서버의 **방화벽 및 가상 네트워크** 블레이드에서 다음 설정을 구성합니다.

    - Azure 서비스에 대한 액세스 허용: **켜기**

    - 구성된 방화벽 규칙이 없습니다. 

    - 가상 네트워크:

        - 이름: **az1010301b-vnet1**

        - 구독: 이 랩에서 사용 중인 구독의 이름

        - 가상 네트워크: **az1010301b-vnet1**

        - 서브넷 이름: **서브넷0/ 10.203.0.0/24**

> **결과**: 이 연습을 완료한 후 Azure Resource Manager 템플릿을 사용하여 Azure VM, Azure Storage 계정 및 Azure SQL Database 인스턴스를 배포하고 Azure Network Watcher 서비스를 사용하도록 설정하고 Azure 가상 네트워크 간에 설정된 글로벌 피어링을 설정했습니다. Azure Storage 계정 및 Azure SQL Database 인스턴스에 대해 설정된 서비스 엔드포인트를 제공합니다.


### 연습 2: Azure Network Watcher를 사용하여 네트워크 연결 모니터링
  
이 연습의 주요 작업은 다음과 같습니다.

1. Network Watcher를 사용하여 가상 네트워크 피어링을 통해 Azure VM에 대한 네트워크 연결 테스트

1. Network Watcher를 사용하여 Azure Storage 계정에 대한 네트워크 연결 테스트

1. Network Watcher를 사용하여 Azure SQL 데이터베이스에 대한 네트워크 연결 테스트


#### 작업 1: Network Watcher를 사용하여 가상 네트워크 피어링을 통해 Azure VM에 대한 네트워크 연결 테스트

1. Azure Portal에서 **Network Watcher**블레이드로 이동합니다.

1. **Network Watcher** 블레이드에서 **연결 문제 해결**로 이동합니다.

1. **Network Watcher - 연결 문제 해결** 블레이드에서 다음 설정으로 검사를 시작합니다.

    - 출처: 

        - 구독: 이 랩에서 사용 중인 Azure 구독의 이름

        - 리소스 그룹: **az1010301b-RG**

        - 소스 유형: **가상 머신**

        - 가상 머신: **az1010301b-vm1**

    - 대상: **수동으로 지정**

        - URI, FQDN 또는 IPv4: **10.203.16.4**

      > **참고**: **10.203.16.4**는 다른 Azure 지역에 배포한 두 번째 Azure VM az1010302b-vm1의 개인 IP 주소입니다.

    - 프로브 설정:

        - 프로토콜: **TCP**

        - 대상 포트: **3389**

    - 고급 설정:

        - 원본 포트: 비어 있음

1. 연결 검사 결과가 반환될 때까지 기다렸다가 상태가 **연결 가능** 상태인지 확인합니다. 네트워크 경로를 검토하고 VM 사이에 중간 홉이 없는 연결이 직접적이라는 점에 유의하십시오.

    > **참고**: Network Watcher를 처음 사용하는 경우 검사는 최대 5분 정도 걸릴 수 있습니다.


#### 작업 2: Network Watcher를 사용하여 Azure Storage 계정에 대한 네트워크 연결 테스트

1. Azure Portal에서 Cloud Shell에서 PowerShell 세션을 시작합니다. 

   > **참고**: 현재 Azure 구독에서 Cloud Shell을 처음 시작하는 경우 Cloud Shell 파일을 유지하도록 Azure 파일 공유를 만들라는 메시지가 표시됩니다. 이 경우 기본값을 허용하면 자동으로 생성된 리소스 그룹에서 스토리지 계정이 생성됩니다.

1. Cloud Shell 창에서 다음 명령을 실행하여 이전 연습에서 프로비전한 Azure Storage 계정의 Blob 서비스 엔드포인트의 IP 주소를 식별합니다.

   ```pwsh
   [System.Net.Dns]::GetHostAddresses($(Get-AzStorageAccount -ResourceGroupName 'az1010301b-RG')[0].StorageAccountName + '.blob.core.windows.net').IPAddressToString
   ```

1. 결과 문자열을 기록하고 **Network Watcher - 연결 문제 해결** 블레이드에서 다음 설정으로 검사를 시작합니다.

    - 출처: 

        - 구독: 이 랩에서 사용 중인 Azure 구독의 이름

        - 리소스 그룹: **az1010301b-RG**

        - 소스 유형: **가상 머신**

        - 가상 머신: **az1010301b-vm1**

    - 대상: **수동으로 지정**

        - URI, FQDN 또는 IPv4: 이 작업의 이전 단계에서 식별한 스토리지 계정의 Blob 서비스 엔드포인트의 IP 주소

    - 프로브 설정:

        - 프로토콜: **TCP**

        - 대상 포트: **443**

    - 고급 설정:

        - 원본 포트: 비어 있음

1. 연결 검사 결과가 반환될 때까지 기다렸다가 상태가 **연결 가능** 상태인지 확인합니다. 네트워크 경로를 검토하고 VM 사이에 중간 홉이 없고 대기 시간이 최소화된 연결이 직접적인 연결임을 유의하십시오. 

    > **참고**: 연결은 이전 연습에서 만든 서비스 엔드포인트를 통해 이루어집니다. 이를 확인하려면 Network Watcher의 **다음 홉** 도구를 사용합니다.

1. **Network Watcher - 연결 문제 해결** 블레이드에서 **Network Watcher로 이동 - 다음 홉** 블레이드및 다음 홉을 다음 설정으로 테스트합니다.

    - 구독: 이 랩에서 사용 중인 Azure 구독의 이름

    - 리소스 그룹: **az1010301b-RG**

    - 가상 머신: **az1010301b-vm1**

    - 네트워크 인터페이스: **az1010301b-nic1**

    - 소스 IP 주소: **10.203.0.4**

    - 대상 IP 주소: 이 작업의 앞에서 확인한 스토리지 계정의 Blob 서비스 엔드포인트의 IP 주소

1. 결과가 다음 홉 유형을 **가상 네트워크 서비스 엔드포인트**식별하는지 확인합니다.

1. **Network Watcher - 연결 문제 해결** 블레이드에서 다음 설정으로 검사를 시작합니다.

    - 출처: 

        - 구독: 이 랩에서 사용 중인 Azure 구독의 이름

        - 리소스 그룹: **az1010302b-RG**

        - 소스 유형: **가상 머신**

        - 가상 머신: **az1010302b-vm2**

    - 대상: **수동으로 지정**

        - URI, FQDN 또는 IPv4: 이 작업의 앞에서 확인한 스토리지 계정의 Blob 서비스 엔드포인트의 IP 주소

    - 프로브 설정:

        - 프로토콜: **TCP**

        - 대상 포트: **443**

    - 고급 설정:

        - 원본 포트: 비어 있음

1. 연결 검사 결과가 반환될 때까지 기다렸다가 상태가 **연결 가능** 상태인지 확인합니다. 

    > **참고**: 연결은 성공하지만 인터넷을 통해 설정됩니다. 이를 확인하려면 Network Watcher의 **다음 홉** 도구를 다시 사용합니다.

1. **Network Watcher - 연결 문제 해결** 블레이드에서 **Network Watcher로 이동 - 다음 홉** 블레이드및 다음 홉을 다음 설정으로 테스트합니다.

    - 구독: 이 랩에서 사용 중인 Azure 구독의 이름

    - 리소스 그룹: **az1010302b-RG**

    - 가상 머신: **az1010302b-vm2**

    - 네트워크 인터페이스: **az1010302b-nic1**

    - 소스 IP 주소: **10.203.16.4**

    - 대상 IP 주소: 이 작업의 앞에서 확인한 스토리지 계정의 Blob 서비스 엔드포인트의 IP 주소

1. 결과가 다음 홉 유형을 **인터넷**으로 식별하는지 확인합니다.


#### 작업 3: Network Watcher를 사용하여 Azure SQL 데이터베이스에 대한 네트워크 연결 테스트

1. Azure Portal에서 Cloud Shell에서 PowerShell 세션을 시작합니다. 

1. Cloud Shell 창에서 다음 명령을 실행하여 이전 연습에서 프로비전한 Azure SQL Database 서버의 IP 주소를 식별합니다.

   ```pwsh
   [System.Net.Dns]::GetHostAddresses($(Get-AzSqlServer -ResourceGroupName 'az1010301b-RG')[0].FullyQualifiedDomainName).IPAddressToString
   ```

1. 결과 문자열을 기록하고 **Network Watcher - 연결 문제 해결** 블레이드에서 다음 설정으로 검사를 시작합니다.

    - 출처: 

        - 구독: 이 랩에서 사용 중인 Azure 구독의 이름

        - 리소스 그룹: **az1010301b-RG**

        - 소스 유형: **가상 머신**

        - 가상 머신: **az1010301b-vm1**

    - 대상: **수동으로 지정**

        - URI, FQDN 또는 IPv4: 이 작업의 이전 단계에서 식별한 Azure SQL Database 서버의 IP 주소

    - 프로브 설정:

        - 프로토콜: **TCP**

        - 대상 포트: **1433**

    - 고급 설정:

        - 원본 포트: 비어 있음

1. 연결 검사 결과가 반환될 때까지 기다렸다가 상태가 **연결 가능** 상태인지 확인합니다. 네트워크 경로를 검토하고 VM 사이에 중간 홉이 없고 대기 시간이 짧은 연결이 직접적이라는 점에 유의하십시오. 

    > **참고**: 연결은 이전 연습에서 만든 서비스 엔드포인트를 통해 이루어집니다. 이를 확인하려면 Network Watcher의 **다음 홉** 도구를 사용합니다.

1. **Network Watcher - 연결 문제 해결** 블레이드에서 **Network Watcher로 이동 - 다음 홉** 블레이드및 다음 홉을 다음 설정으로 테스트합니다.

    - 구독: 이 랩에서 사용 중인 Azure 구독의 이름

    - 리소스 그룹: **az1010301b-RG**

    - 가상 머신: **az1010301b-vm1**

    - 네트워크 인터페이스: **az1010301b-nic1**

    - 소스 IP 주소: **10.203.0.4**

    - 대상 IP 주소: 이 작업의 앞에서 확인한 Azure SQL Database 서버의 IP 주소입니다.

1. 결과가 다음 홉 유형을 **가상 네트워크 서비스 엔드포인트**식별하는지 확인합니다.

1. **Network Watcher - 연결 문제 해결** 블레이드에서 다음 설정으로 검사를 시작합니다.

    - 출처: 

        - 구독: 이 랩에서 사용 중인 Azure 구독의 이름

        - 리소스 그룹: **az1010302b-RG**

        - 소스 유형: **가상 머신**

        - 가상 머신: **az1010302b-vm2**

    - 대상: **수동으로 지정**

        - URI, FQDN 또는 IPv4: 이 작업의 앞에서 확인한 Azure SQL Database 서버의 IP 주소

    - 프로브 설정:

        - 프로토콜: **TCP**

        - 대상 포트: **1433**

    - 고급 설정:

        - 원본 포트: 비어 있음

1. 연결 검사 결과가 반환될 때까지 기다렸다가 상태가 **연결 가능** 상태인지 확인합니다. 

    > **참고**: 연결은 성공하지만 인터넷을 통해 설정됩니다. 이를 확인하려면 Network Watcher의 **다음 홉** 도구를 다시 사용합니다.

1. **Network Watcher - 연결 문제 해결** 블레이드에서 **Network Watcher로 이동 - 다음 홉** 블레이드및 다음 홉을 다음 설정으로 테스트합니다.

    - 구독: 이 랩에서 사용 중인 Azure 구독의 이름

    - 리소스 그룹: **az1010302b-RG**

    - 가상 머신: **az1010302b-vm2**

    - 네트워크 인터페이스: **az1010302b-nic1**

    - 소스 IP 주소: **10.203.16.4**

    - 대상 IP 주소: 이 작업의 앞에서 확인한 Azure SQL Database 서버의 IP 주소입니다.

1. 결과가 다음 홉 유형을 **인터넷**으로 식별하는지 확인합니다.


> **결과**: 이 연습을 완료한 후 Azure Network Watcher를 사용하여 가상 네트워크 피어링, Azure Storage에 대한 네트워크 연결 및 Azure SQL Database에 대한 네트워크 연결을 통해 Azure VM에 대한 네트워크 연결을 테스트했습니다.

## 연습 3: 랩 리소스 제거

#### 작업 1: Cloud Shell 열기

1. 포털 상단에서 **Cloud Shell** 아이콘을 클릭하여 Cloud Shell 창을 엽니다.

1. Cloud Shell 인터페이스에서 **Bash**를 선택합니다.

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 이 랩에서 만든 모든 리소스 그룹을 나열합니다.

   ```sh
   az group list --query "[?starts_with(name,'az1010')].name" --output tsv
   ```

1. 이 랩에서 만든 리소스 그룹만 출력에 포함되어 있는지 확인합니다. 이러한 그룹은 다음 작업에서 삭제됩니다.

#### 작업 2: 리소스 그룹 삭제

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 이 랩에서 만든 리소스 그룹을 삭제합니다.

   ```sh
   az group list --query "[?starts_with(name,'az1010')].name" --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
   ```

1. 포털 하단에 있는 **Cloud Shell** 프롬프트를 닫습니다.

> **결과**: 이 연습에서는 이 랩에서 사용한 리소스를 제거했습니다.
