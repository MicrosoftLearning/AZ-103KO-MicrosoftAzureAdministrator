---
lab:
    title: '디렉터리 동기화 구현'
    module: '모듈 09 - Azure Active Directory'
---

# 랩: 디렉터리 동기화 구현
  
이 실습의 모든 작업은 실습 3 작업 1, 실습 3 작업 2 및 실습 3 작업 3을 제외한 Azure 포털 (PowerShell Cloud Shell 세션 포함)에서 수행되며 원격 데스크톱 세션에서 Azure VM으로 수행되는 단계가 포함됩니다

   > **참고**: Cloud Shell을 사용하지 않는 경우 랩 가상 시스템에 Azure PowerShell 1.2.0 모듈(또는 최신 모듈)이 설치되어 있어야 합니다.[https://docs.microsoft.com/ko-kr/powershell/azure/install-az-ps?view=azps-1.2.0](https://docs.microsoft.com/ko-kr/powershell/azure/install-az-ps?view=azps-1.2.0)

랩 파일: 없음

### 시나리오
  
Adatum Corporation 은 Active Directory 를 Azure Active Directory 와 통합하려고 합니다.


### 목표
  
 이 과정을 완료하면 다음과 같은 역량을 갖추게 됩니다:

- Active Directory 도메인 컨트롤러를 호스팅하는 Azure VM 배포

- Azure Active Directory 테넌트 만들기 및 구성

- Azure Active Directory 테넌트를 사용 하 여 Active Directory 포리스트를 동기화 합니다


### 연습 1: Active Directory 도메인 컨트롤러를 호스팅하는 Azure VM 배포

이 연습의 주요 작업은 다음과 같습니다:

1. Azure VM 배포에 사용할 수 있는 DNS 이름 식별

1. Azure Resource Manager 템플릿을 사용하여 Active Directory 도메인 컨트롤러를 호스트하는 Azure VM 배포


#### 작업 1: Azure VM 배포에 사용할 수 있는 DNS 이름 식별

1. 랩 가상 머신에서 Microsoft Edge를 시작하고 ** [http://portal.azure.com**](http://portal.azure.com) 에서 Azure 포털을 찾아보고 이 랩에서 사용할 Azure 구독에서 소유자 역할이 있고 글로벌 관리자인 Microsoft 계정을 사용하여 로그인합니다. 해당 구독과 연결된 Azure AD 테넌트의

1. Azure 포털에서 Cloud Shell 창에서 PowerShell 세션을 시작하십시오.

   > **참고**: 현재 Azure 구독에서 Cloud Shell을 처음 시작하는 경우 Cloud Shell 파일을 유지하도록 Azure 파일 공유를 만들라는 메시지가 표시됩니다. 이 경우 기본값을 허용하면 자동으로 생성된 리소스 그룹에서 저장소 계정이 생성됩니다.

1. Cloud Shell (Cloud Shell) 창에서 다음 명령을 실행하여 &lt;custom-label&gt; 고유 할 가능성이있는 임의의 문자열 및 자리 표시 자 &lt;location&gt; Active Directory 도메인 컨트롤러를 호스팅 할 Azure VM을 배포 할 Azure 영역의 이름을 지정합니다.

   > **참고**: Azure VM을 프로비전할 수 있는 Azure 지역을 식별하려면 [**https://azure.microsoft.com/ko-kr/regions/offers/**](https://azure.microsoft.com/ko-kr/regions/offers/)참고하십시오.

   ```pwsh
   Test-AzDnsAvailability -DomainNameLabel <custom-label> -Location '<location>'
   ```

1. 결과가 **True**인지 확인합니다. 그렇지 않은 경우 &lt;custom-label&gt;을 다른 값으로 변경하여 동일한 명령을 **True** 를 반환할 때까지 다시 실행합니다.

1. 성공적인 결과가 출력된 &lt;custom-label&gt;를 별도로 메모합니다. 다음 작업에 필요합니다.


#### 작업 2: Azure Resource Manager 템플릿을 사용하여 Active Directory 도메인 컨트롤러를 호스트하는 Azure VM 배포

1. Azure 포털에서 **리소스 만들기** 블레이드로 이동합니다.

1. **리소스 만들기** 블레이드가 뜨면, Azure 마켓플레이스에서 **Template deployment**를 검색합니다. 그리고 **Template deployment (deploy using custom templates)**를 선택합니다.

1. **만들기**를 클릭합니다.

1. **사용자 지정 배포** 블레이드에서 **GitHub 빠른 시작 템플릿 로드** 드롭다운 목록에서 **active-directory-new-domain** 을 선택합니다.

1. **템플릿 선택**을 클릭합니다.

1. **Create an Azure VM with a new AD Forest** 에서 다음 설정을 사용하여 템플릿 배포를 시작합니다:

    - 구독: 이 랩에서 사용 중인 구독의 이름

    - 리소스 그룹: **az1000501-RG** 이름으로 새로 만들기

    - 지역 : 이전 작업에서 사용한 Azure 지역의 이름

    - Admin Username: **Student**

    - Admin Password: **Pa55w.rd1234**

    - Domain Name: **adatum.com**

    - Dns Prefix: 이전 작업에서 식별한 &lt;custom-label&gt;
    
    - VM Size: **Standard_D2s_v3**

    - _artifacts Location: 기본 값

    - _artifacts Location Sas Token: 비워 둡니다.

    - Location: accept 기본 값

   > **참고**: 배포가 완료될 때까지 기다리지 말고 다음 연습으로 진행합니다. 이 작업의 세 번째 실습에서이 작업에 배포 된 가상 컴퓨터를 사용합니다.

> **결과**: 이 연습을 완료한 후 Azure 리소스 관리자 템플릿을 사용하여 Active Directory 도메인 컨트롤러를 호스트하는 Azure VM의 배포를 시작했습니다


### 연습 2: Azure Active Directory 테넌트 만들기 및 구성

이 연습의 주요 작업은 다음과 같습니다:

1. Azure Active Directory (AD) 테넌트 만들기

1. 새 Azure AD 테넌트에 사용자 지정 DNS 이름 추가

1. 글로벌 관리자 역할을 사용하여 Azure AD 사용자 만들기


#### 작업 1: Azure Active Directory (AD) 테넌트 만들기

1. Azure 포털에서 **리소스 만들기** 블레이드로 이동합니다.

1. **리소스 만들기** 블레이드가 뜨면, Azure 마켓플레이스에서 **Azure Active Directory**를 검색하십시오.

1. 검색 결과 목록을 사용하여 **디렉터리 만들기** 블레이드로 이동합니다.

1. **디렉터리 만들기** 블레이드 에서 다음 설정을 사용하여 새 AzureAD 테넌트를 만듭니다: 

  - 조직 이름: **AdatumSync**

  - 초기 도메인 이름: 문자와 숫자의 조합으로 구성된 고유한 이름입니다. 

  - 국가 또는 지역: **미국**

   > **참고**: **초기 도메인 이름** 텍스트 상자의 녹색 확인 표시는 입력 한 도메인 이름이 유효하고 고유한지 여부를 나타냅니다. 


#### 작업 2: 새 Azure AD 테넌트에 사용자 지정 DNS 이름 추가
  
1. Azure 포털에서 **디렉터리 + 구독**에서 필터를 새로 만든 AzureAD 테넌트에 설정합니다.

   > **참고**: **디렉토리 + 구독** 필터가 Azure 포털의 툴바에 있는 알림 아이콘 왼쪽에 있습니다. 

   > **참고**: **AdatumSync** 항목이 **디렉터리 + 구독** 필터 목록에 나타나지 않으면 브라우저 창을 새로 고쳐야 할 수 있습니다.

1. Azure 포털에서 **AdatumSync - 개요** 블레이드로 이동합니다.

1. **AdatumSync - 개요** 블레이드에서 **AdatumSync - 사용자 지정 도메인 이름** 블레이드로 이동합니다.

1. **AdatumSync - 사용자 지정 도메인 이름** 블레이드에서 Azure AD 테넌트와 연결된 기본 기본 DNS 도메인 이름을 확인하고 메모해 둡니다. - 다음 연습에서 사용할 것입니다.

1. **AdatumSync - 사용자 지정 도메인 이름** 블레이드에서 **adatum.com** 사용자 지정 도메인을 추가합니다.

1. **adatum.com** Azure에서 Azure AD 도메인 이름 확인을 수행하는 데 필요한 정보를 검토합니다.

   > **참고**: **adatum.com** DNS 도메인 이름을 소유하지 않으므로 유효성 검사 프로세스를 완료할 수 없습니다. 이렇게 하면 **adatum.com** Active Directory 도메인을 AzureAD 테넌트와 동기화할 수 없습니다. 이 목적을 위해 이 작업의 앞에서 식별한 Azure AD 테넌트의 기본 기본 DNS 이름(**onmicrosoft.com** 접미사로 끝나는 이름)을 사용합니다. 그러나 결과적으로 Active Directory 도메인의 DNS 도메인 이름과 Azure AD 테넌트의 DNS 이름이 다를 수 있습니다. 즉, Adatum 사용자는 Active Directory 도메인에 로그인할 때와 Azure AD 테넌트에 로그인할 때 다른 이름을 사용해야 합니다.


#### 작업 3: 글로벌 관리자 역할을 사용하여 Azure AD 사용자 만들기

1. Azure 포털에서 **AdatumSync** Azure AD 테넌트의 **사용자 - 모든 사용자** 블레이드로 이동합니다.

1. **사용자 - 모든 사용자** 블레이드에서 다음 설정을 사용하여 새 사용자를 만듭니다:

    - 이름: **syncadmin**

    - 사용자 이름: **syncadmin@***<DNS-domain-name>여기서* *<DNS-domain-name>*은 이전 작업에서 식별한 기본 기본 DNS 도메인 이름을 나타냅니다. 이 사용자 이름을 기록해 둡니다. 이 랩에서 나중에 필요할 것입니다.

    - 작업 정보: **구성되지 않음**

    - 속성: **기본 값**

    - 그룹: **0개 그룹 선택됨**

    - 디렉터리 역할: **전역 관리자**

    - 암호: **암호 표시** 확인란을 선택하고 **암호** 텍스트 상자에 나타나는문자열을 기록합니다. 이 작업의 뒷부분에서 필요할 것입니다.

   > **참고**: Azure AD Connect를 구현하려면 전역 관리자 역할을 가진 Azure AD 사용자가 필요합니다.

1. Microsoft Edge의 새 InPrivate 창을 엽니다.

1. 새 브라우저 창에서 Azure 포털로 이동하고 **syncadmin** 사용자 계정을 사용하여 로그인 합니다. 메시지가 표시되면 암호를 새 값으로 변경합니다.

   > **참고**: AzureAD 테넌트 DNS 도메인 이름을 포함하여 **syncadmin** 사용자 계정의 정규화된 이름을 제공해야 합니다. 

1. **syncadmin** 로 로그아웃하고 InPrivate 브라우저 창을 닫습니다.

> **결과**: 이 연습을 완료한 후 Azure AD 테넌트를 만들고, 새 Azure AD 테넌트에 사용자 지정 DNS 이름을 추가하고, 글로벌 관리자 역할을 가진 Azure AD 사용자를 만들었습니다.


### 연습 3: Azure Active Directory 테넌트를 사용하여 Active Directory 포리스트를 동기화 합니다

이 연습의 주요 작업은 다음과 같습니다:

1. 디렉터리 동기화를 준비하기 위해 Active Directory 구성

1. Azure AD Connect 설치

1. 디렉터리 동기화 확인


#### 작업 1: 디렉터리 동기화를 준비하기 위해 Active Directory 구성

   > **참고**: 이 작업을 시작하기 전에 연습 1에서 시작한 템플릿 배포가 완료되었는지 확인합니다.
  
1. Azure 포털에서 **디렉터리 + 구독** 필터를 이 랩의 첫 번째 연습에서 사용한 Azure 구독과 연결된 Azure AD 테넌트로 다시 설정합니다.

   > **참고**: **디렉토리 + 구독** 필터가 Azure 포털의 툴바에 있는 알림 아이콘 왼쪽에 있습니다. 

1. Azure 포털에서 **adVM** 블레이드로 이동하여 이 랩의 첫 번째 연습에서 배포한 Active Directory 도메인 컨트롤러를 호스팅하는 Azure VM의 속성을 표시합니다.

1. **adVM** 블레이드의 **개요** 창에서 RDP 파일을 생성하고 **adVM**에 연결하는 데 사용합니다.

1. 메시지가 표시되면 다음 자격 증명을 지정하여 인증합니다:

    - 사용자 이름: **Student**

    - 암호: **Pa55w.rd1234**

1. **adVM** 에 원격 데스크톱 세션 내에서 활성 **Active Directory 관리 센터**를 엽니다.

1. **Active Directory 관리 센터** 에서 **ToSync** 라는 루트 수준 조직 단위를 만듭니다.

1. **Active Directory 관리 센터** 에서 조직 단위 **ToSync** 에서 다음 설정을 사용하여 새 사용자 계정을만듭니다.

    - 전체 이름: **aduser1**

    - 사용자 UPN 로그온: **aduser1@adatum.com**

    - 사용자 샘계정 이름 로그온: **adatum\aduser1**

    - 암호: **Pa55w.rd1234**

    - 기타 암호 옵션: **암호가 만료되지 않습니다**


#### 작업 2: Azure AD Connect 설치

1. RDP 세션 내에서 서버 관리자에서 **adVM** 일시적으로 **IE 강화 된 보안 구성** 을 사용하지 않도록 설정 합니다.

1. RDP 세션 내에서 **adVM** 인터넷 익스플로러를 시작하고 [**https://www.microsoft.com/en-us/download/details.aspx?id=47594**](https://www.microsoft.com/en-us/download/details.aspx?id=47594)에서 **Azure AD Connect** 를 다운로드 합니다.

1. **Microsoft Azure Active Directory Connect** 마법사를 시작하고 라이선스 조건을 수락한 다음 **익스프레스 설정** 페이지에서 **사용자 지정** 옵션을 선택합니다.

1. **필요한 구성 요소 설치** 페이지에서 모든 선택적 구성 옵션을 선택 취소하고 설치를 시작합니다.

1. **사용자 로그인** 페이지에서 **암호 해시 동기화** 만 사용하도록 설정되어 있는지 확인합니다.

1. Azure AD에 연결하라는 메시지가 표시되면 이전 연습에서 만든 **syncadmin** 계정의 자격 증명을 사용하여 인증합니다.

1. 디렉터리 연결하라는 메시지가 표시되면 **adatum.com** 포리스트를 추가하고 **새 AD 계정 만들기** 옵션을 선택하고 다음 자격 증명을 사용하여 인증합니다:

    - 사용자 이름: **ADATUM\\Student**

    - 암호: **Pa55w.rd1234**

1. Azure **AD 로그인 구성** 페이지에서 **UPN 접미사가 확인된 도메인 이름과 일치하지 않는 경우 사용자가 온-프레미스 자격 증명으로 Azure AD에 로그인할 수 없다**는 경고에 유의하고 **모든 UPN 접미사를 확인 된 도메인과 일치시키지 않고 계속** 체크박스에 체크를 한다.

   > **참고**: 앞서 설명했듯이 사용자 지정 Azure AD DNS 도메인 **adatum.com** 확인할 수 없기 때문이다.

1. **도메인 및 OU 필터링** 페이지에서 **ToSync** OU만 선택되어 있는지 확인합니다.

1. **사용자를 고유하게 식별** 페이지에서 기본 설정을 수락합니다.

1. **사용자 및 장치 필터** 페이지에서 기본 설정을 수락합니다.

1. **선택적 기능** 페이지에서 기본 설정을 수락합니다.

1. **구성 준비** 페이지에서 구성이 완료될 때 **설정 완료 후 동기화 프로세스 시작**이 선택되어 있는지 확인하고 설치합니다. 

   > **참고**: 설치에는 약 2 분이 소요됩니다.

1. 구성이 완료되면 Microsoft Azure Active Directory Connect 창을 닫습니다.


#### 작업 3: 디렉터리 동기화 확인

1. **adVM**의 RDP 세션 내에서 Internet Explorer를 시작하고, Azure Portal을 [**http://portal.azure.com**](http://portal.azure.com) 탐색하여 작업 1 에서 만든 **syncadmin** 계정을 사용하여 로그인합니다. 

1. Azure 포털에서 **AdatumSync - 개요** 블레이드로 이동합니다.

1. **AdatumSync - 개요** 블레이드에서 AdatumSync Azure AD 테넌트의 **사용자 - 모든 사용자** 블레이드입니다.

1. **사용자 - 모든 사용자** 블레이드에서 목록에 **Aduser1** 사용자 객체가 있고 **원본** 열에 **Windows Server AD** 가 있는지 확인합니다.

1. **사용자 - 모든 사용자** 블레이드에서 **aduser1 - 프로필** 블레이드로 이동합니다. **부서** 속성에 값이 없습니다.

1. **adVM**의 RDP 세션 내에서 **Active Directory 관리 센터**를 열고, **aduser1** 사용자 계정의 속성을 표시하는 창을 열고 **부서** 속성의 값을 **Sales** 로 설정합니다.

1. **adVM**의 RDP 세션 내에서 관리자 권한으로 **Windows PowerShell** 을 시작합니다.

1. Windows PowerShell 프롬프트에서 다음을 실행하여 Azure AD Connect 변동분 동기화를 시작합니다:

   ```pwsh
   Import-Module -Name 'C:\Program Files\Microsoft Azure AD Sync\Bin\ADSync\ADSync.psd1'
   
   Start-ADSyncSyncCycle -PolicyType Delta
   ```

1. **adVM**의 RDP 세션 내에서 Azure Portal을 표시하는 Internet Explorer 창으로 전환합니다.

1. Azure 포털에서 **사용자 - 모든 사용자** 블레이드로 돌아가 페이지를 새로 고칩니다. 

1. **사용자 - 모든 사용자** 블레이드에서 **aduser1 - 프로필** 블레이드로 이동합니다. 이제 **부서** 속성이 **Sales** 로 설정됩니다.

   > **참고**: **부서** 특성이 설정되지 않은 상태로 유지되면 다른 분 동안 기다렸다가 페이지를 다시 새로 고쳐야 할 수 있습니다.

> **결과**: 이 연습을 완료한 후 디렉터리 동기화를 준비하기 위해 Active Directory를 구성하고, Azure AD Connect를 설치했으며, 확인된 디렉터리 동기화를 구성했습니다.

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

#### 작업 3: Azure AD 테넨트 삭제하기

1. 랩 가상 머신에서 관리자 권한으로 Windows PowerShell을 실행합니다.

1. 랩 가상 머신의 Windows PowerShell 콘솔에서 다음을 실행하여 MsOnline PowerShell 모듈을 설치합니다 (프롬프트가 표시되면 NuGet 공급자가 필요합니다. 대화 상자에서 **예**를 클릭합니다.)

   ```pwsh
   Install-Module MsOnline -Force
   ```
   
1. 랩 가상 머신의 Windows PowerShell 콘솔에서 다음을 실행하여 AdatumSync Azure AD 테넌트에 연결합니다 (프롬프트가 표시되면 SyncAdmin 자격 증명으로 로그인).

   ```pwsh
   Connect-MsolService
   ```

1. 랩 가상 머신의 Windows PowerShell 콘솔에서 다음을 실행하여 Azure AD Connect 동기화를 사용하지 않도록 설정합니다.

   ```pwsh
   Set-MsolDirSyncEnabled -EnableDirSync $false -Force
   ```

1. 랩 가상 머신의 Windows PowerShell 콘솔에서 다음을 실행하여 작업이 성공했는지 확인하십시오.

   ```pwsh
   (Get-MSOLCompanyInformation).DirectorySynchronizationEnabled 
   ```   

1. 랩 가상 머신에서 Azure Portal에서 로그아웃하고 Microsoft Edge 창을 닫습니다.

1. 랩 가상 머신에서 Microsoft Edge를 시작하고 Azure Portal로 이동 한 후 SyncAdmin 자격 증명을 사용하여 로그인 합니다.

1. Azure portal에서 **사용자 - 모든 사용자** 블레이드로 이동한 후 SyncAdmin 계정을 제외한 모든 사용자를 삭제합니다.

> **참고**: 이 단계를 완료하려면 몇 시간 정도 기다려야 할 수 있습니다.

1. AdatumSync - 개요 블레이드로 이동하여 **속성**을 클릭합니다.

1. **속성** 블레이드에서 **Azure 리소스에 대한 액세스 관리** 항목에서 **예**를 클릭한 후 **저장**을 클릭합니다.

1. Azure Portal에서 로그아웃하고 SyncAdmin 자격 증명을 사용하여 다시 로그인 합니다.

1. **AdatumSync - 개요** 블레이드로 이동하여 **디렉터리 삭제**를 클릭하여 Azure AD 테넨트를 삭제합니다.

1. **'AdatumSync' 디렉터리를 삭제하시겠습니까?** 블레이드에서 **삭제**를 클릭합니다.

> **참고**: 이 작업에 관한 추가 정보는 다음을 참조하십시오. https://docs.microsoft.com/en-us/azure/active-directory/users-groups-roles/directory-delete-howto

> **결과**: 이 연습을 완료한 후 이 랩에서 사용된 리소스 제거했습니다.
