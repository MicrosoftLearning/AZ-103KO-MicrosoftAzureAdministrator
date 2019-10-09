---
lab:
    title: '디렉터리 동기화 구현'
    module: '모듈 09 - Azure Active Directory'
---

# 랩: 디렉터리 동기화 구현
  
이 실습의 모든 작업은 실습 3 작업 1, 실습 3 작업 2 및 실습 3 작업 3을 제외한 Azure Portal (PowerShell Cloud Shell 세션 포함)에서 수행되며 원격 데스크톱 세션에서 Azure VM으로 수행되는 단계가 포함됩니다

   > **참고**: Cloud Shell을 사용하지 않는 경우 랩 가상 시스템에 Azure PowerShell 1.2.0 모듈(또는 최신 모듈)이 설치되어 있어야 합니다. [https://docs.microsoft.com/ko-kr/powershell/azure/install-az-ps](https://docs.microsoft.com/ko-kr/powershell/azure/install-az-ps)

랩 파일: 없음

### 시나리오
  
Adatum Corporation 은 Active Directory 를 Azure Active Directory 와 통합하려고 합니다.


### 목표
  
 이 과정을 완료하면 다음과 같은 역량을 갖추게 됩니다.

- Active Directory 도메인 컨트롤러를 호스팅하는 Azure VM 배포

- Azure Active Directory 테넌트 만들기 및 구성

- Azure Active Directory 테넌트를 사용 하 여 Active Directory 포리스트를 동기화 합니다


### 연습 1: Active Directory 도메인 컨트롤러를 호스팅하는 Azure VM 배포

이 연습의 주요 작업은 다음과 같습니다.

1. Azure VM 배포에 사용할 수 있는 DNS 이름 식별

1. Azure Resource Manager 템플릿을 사용하여 Active Directory 도메인 컨트롤러를 호스트하는 Azure VM 배포


#### 작업 1: Azure VM 배포에 사용할 수 있는 DNS 이름 식별

1. 랩 가상 머신에서 Microsoft Edge를 시작하고 [**http://portal.azure.com**](http://portal.azure.com)에서 Azure Portal을 찾아보고 이 랩에서 사용할 Azure 구독에서 소유자 역할이 있고 글로벌 관리자인 Microsoft 계정을 사용하여 로그인합니다. 해당 구독과 연결된 Azure AD 테넌트의

1. Azure Portal에서 Cloud Shell 창에서 PowerShell 세션을 시작하십시오.

   > **참고**: 현재 Azure 구독에서 Cloud Shell을 처음 시작하는 경우 Cloud Shell 파일을 유지하도록 Azure 파일 공유를 만들라는 메시지가 표시됩니다. 이 경우 기본값을 허용하면 자동으로 생성된 리소스 그룹에서 스토리지 계정이 생성됩니다.

1. Cloud Shell 창에서 다음 명령을 실행하여 &lt;custom-label&gt; 자리 표시자를 고유한 문자열로 바꾸고 &lt;location&gt; 자리 표시자를 Active Directory 도메인 컨트롤러를 호스팅할 Azure VM을 배포하려는 Azure 지역의 이름으로 바꿉니다.

   > **참고**: Azure VM을 프로비전할 수 있는 Azure 지역을 확인하려면 [**https://azure.microsoft.com/ko-kr/regions/offers/**](https://azure.microsoft.com/ko-kr/regions/offers/)를 참고하십시오.

   ```pwsh
   Test-AzDnsAvailability -DomainNameLabel <custom-label> -Location '<location>'
   ```

1. 명령이 **True**. True가 반환되지 않으면 **True**가 반환될 때까지 다른 값을 사용해 같은 명령을 다시 실행합니다. 

1. 성공적인 결과를 가져온 &lt;custom-label&gt; 가치에 주목하십시오. 다음 작업에 필요합니다


#### 작업 2: Azure Resource Manager 템플릿을 사용하여 Active Directory 도메인 컨트롤러를 호스트하는 Azure VM 배포

1. 랩 가상 머신에서 Microsoft Edge의 다른 인스턴스를 시작하고 GitHub Azure 빠른 시작 템플릿 페이지[(**https://github.com/Azure/azure-quickstart-templates**)](https://github.com/Azure/azure-quickstart-templates)를 검색합니다.

1. Azure 빠른 시작 템플릿 페이지에서 **active-directory-new-domain**을 클릭합니다.

1. **새 Windows VM 만들기 및 새 AD 포리스트, 도메인 및 DC 만들기** 페이지에서 **Azure에 배포**를 마우스 오른쪽 단추로 클릭하고 **새 탭에서 열기**를 클릭합니다.

1. **새 AD 포리스트 블레이드를 사용하여 Azure VM 만들기**에서 다음 설정을 사용하여 템플릿 배포를 시작합니다.

    - 구독: 이 랩에서 사용 중인 구독의 이름

    - 리소스 그룹: 새 리소스 그룹 **az1000501-RG**의 이름

    - 위치: 이전 작업에서 사용한 Azure 지역의 이름

    - 관리자 사용자 이름: **학생**

    - 관리자 암호: **Pa55w.rd1234**

    - 도메인 이름: **adatum.com**

    - Dns 접두사: &lt;custom-label&gt; 이전 작업에서 식별한
    
    - VM 크기: **Standard_D2s_v3**

    - _artifacts 위치: 기본값 수락

    - _artifacts 위치 Sas 토큰: 비워 둡니다

    - 위치: 기본값 수락

   > **참고**: 배포가 완료될 때까지 기다리지 말고 다음 연습으로 진행합니다. 이 작업의 세 번째 실습에서이 작업에 배포된 가상 컴퓨터를 사용합니다.

> **결과**: 이 연습을 완료한 후 Azure Resource Manager 템플릿을 사용하여 Active Directory 도메인 컨트롤러를 호스트하는 Azure VM의 배포를 시작했습니다.


### 연습 2: Azure Active Directory 테넌트 만들기 및 구성

이 연습의 주요 작업은 다음과 같습니다.

1. Azure Active Directory (AD) 테넌트 만들기

1. 새 Azure AD 테넌트에 사용자 지정 DNS 이름 추가

1. 글로벌 관리자 역할을 사용하여 Azure AD 사용자 만들기


#### 작업 1: Azure Active Directory (AD) 테넌트 만들기

1. Azure Portal에서 **리소스 만들기** 블레이드로 이동합니다. 

1. **리소스 만들기** 블레이드를 통해 Azure Marketplace에서 **Azure Active Directory**를 검색하십시오.

1. 검색 결과 목록을 사용하여 **디렉터리 만들기** 블레이드로 이동합니다.

1. **디렉터리 만들기** 블레이드에서 다음 설정을 사용하여 새 Azure AD 테넌트를 만듭니다. 

  - 조직 이름: **아다툼싱크**

  - 초기 도메인 이름: 문자와 숫자의 조합으로 구성된 고유한 이름입니다. 

  - 국가/지역: **미국**

   > **참고**: **초기 도메인 이름** 텍스트 상자의 녹색 확인 표시는 입력 한 도메인 이름이 유효하고 고유한지 여부를 나타냅니다. 


#### 작업 2: 새 Azure AD 테넌트에 사용자 지정 DNS 이름 추가
  
1. Azure Portal에서 **디렉터리 + 구독** 필터를 새로 만든 AzureAD 테넌트에 설정합니다.

   > **참고**: **디렉토리 + 구독** 필터가 Azure Portal의 툴바에있는 알림 아이콘 왼쪽에 나타납니다 

   > **참고**: **AdatumSync** 항목이 **디렉터리 + 구독 필터** 목록에 나타나지 않으면 브라우저 창을 새로 고쳐야 할 수 있습니다.

1. Azure Portal에서 **AdatumSync - 개요** 블레이드로 이동합니다.

1. **AdatumSync - 개요** 블레이드에서 **AdatumSync - 사용자지정 도메인 이름** 블레이드를 표시합니다. 

1. **AdatumSync - 사용자 지정 도메인 이름 **블레이드에서Azure AD 테넌트와 연결된 기본 기본 DNS 도메인 이름을 식별합니다. 그 가치에 주목하십시오 - 당신은 다음 과제에서 그것을 필요로 할 것입니다.

1. **AdatumSync에서 사용자 지정 도메인 이름** 블레이드에서** adatum.com** 사용자 지정 도메인을 추가합니다.

1. **adatum.com**Azure에서 Azure AD 도메인 이름 확인을 수행하는 데 필요한 정보를 검토합니다.

   > **참고**: **adatum.com** DNS 도메인 이름을 소유하지 않으므로 유효성 검사 프로세스를 완료할 수 없습니다. 이렇게 하면 **adatum.com** Active Directory 도메인을 AzureAD 테넌트와 동기화할 수 없습니다. 이 목적을 위해 이 작업의 앞에서 식별한 Azure AD 테넌트의 기본적인 기본 DNS 이름(**onmicrosoft.com** 접미사로 끝나는 이름)을 사용합니다. 그러나 결과적으로 Active Directory 도메인의 DNS 도메인 이름과 Azure AD 테넌트의 DNS 이름이 다를 수 있습니다. 즉, Adatum 사용자는 Active Directory 도메인에 로그인할 때와 Azure AD 테넌트에 로그인할 때 다른 이름을 사용해야 합니다.


#### 작업 3: 글로벌 관리자 역할을 사용하여 Azure AD 사용자 만들기

1. Azure Portal에서 **AdatumSync** Azure AD 테넌트의 **사용자 - 모든 사용자** 블레이드로 이동합니다.

1. **사용자 - 모든 사용자** 블레이드에서 다음 설정을 사용하여 새 사용자를 만듭니다.

    - 이름: **동기화**관리자

    - 사용자 이름: **syncadmin@***<DNS-domain-name>* 여기서 *<DNS-domain-name>*은 이전 작업에서 식별한 기본적인 기본 DNS 도메인 이름을 나타냅니다. 이 사용자 이름을 기록해 둡니다. 이 실험실에서 나중에 필요할 것입니다.

    - 프로필: **구성되지 않음**

    - 속성: **기본값**

    - 그룹: **선택한 그룹 0개**

    - 디렉터리 역할: **전역 관리자**

    - 암호: **암호 표시** 확인란을 선택하고 **암호** 텍스트 상자에 나타나는문자열을 기록합니다. 이 작업의 뒷부분에서 필요할 것입니다.

   > **참고**: Azure AD Connect를 구현하려면 전역 관리자 역할을 가진 Azure AD 사용자가 필요합니다.

1. 비공개 Microsoft 가장자리 창을 엽니다.

1. 새 브라우저 창에서 Azure Portal로 이동하고 **syncadmin** 사용자 계정을 사용하여 로그인 합니다. 메시지가 표시되면 암호를 새 값으로 변경합니다.

   > **참고**: AzureAD 테넌트 DNS 도메인 이름을 포함하여 **syncadmin** 사용자 계정의 정규화된 이름을 제공해야 합니다. 

1. **syncadmin**로 로그아웃하고 InPrivate 브라우저 창을 닫습니다.

> **결과**: 이 연습을 완료한 후 Azure AD 테넌트를 만들고, 새 Azure AD 테넌트에 사용자 지정 DNS 이름을 추가하고, 글로벌 관리자 역할을 가진 Azure AD 사용자를 만들었습니다.


### 연습 3: Azure Active Directory 테넌트를 사용 하 여 Active Directory 포리스트를 동기화 합니다

이 연습의 주요 작업은 다음과 같습니다.

1. 디렉터리 동기화를 준비하기 위해 Active Directory 구성

1. Azure AD Connect 설치

1. 디렉터리 동기화 확인


#### 작업 1: 디렉터리 동기화를 준비하기 위해 Active Directory 구성

   > **참고**: 이 작업을 시작하기 전에 연습 1에서 시작한 템플릿 배포가 완료되었는지 확인합니다.
  
1. Azure Portal에서 **디렉터리 + 구독** 필터를 이 랩의 첫 번째 연습에서 사용한 Azure 구독과 연결된 Azure AD 테넌트로 다시 설정합니다.

   > **참고**: **디렉토리 + 구독** 필터가 Azure Portal의 툴바에있는 알림 아이콘 왼쪽에 나타납니다 

1. Azure Portal에서 **adVM** 블레이드로 이동하여 이 랩의 첫 번째 연습에서 배포한 Active Directory 도메인 컨트롤러를 호스팅하는 Azure VM의 속성을 표시합니다.

1. **adVM** 블레이드의 **개요** 창에서 RDP 파일을 생성하고 **adVM**에 연결하는 데 사용합니다.

1. 메시지가 표시되면 다음 자격 증명을 지정하여 인증합니다.

    - 사용자 이름: **학생**

    - 암호: **Pa55w.rd1234**

1. **adVM**에 원격 데스크톱 세션 내에서 활성 **Active Directory 관리 센터**를 엽니다.

1. **Active Directory 관리 센터**에서 **ToSync** 라는 루트 수준 조직 단위를 만듭니다.


1. **Active Directory 관리 센터**에서 조직 단위 **ToSync**에서 다음 설정을 사용하여 새 사용자 계정을 만듭니다.

    - 전체 이름: **aduser1**

    - 사용자 UPN 로그온: **aduser1@adatum.com**

    - 사용자 샘계정 이름 로그온: **adatum\aduser1**

    - 암호: **Pa55w.rd1234**

    - 기타 암호 옵션: **암호가 만료되지 않습니다**


#### 작업 2: Azure AD Connect 설치

1. RDP 세션 내에서 서버 관리자에서 **adVM**일시적으로** IE 강화 보안 구성**을 사용 하지 않도록 설정 합니다.

1. **adVM** 에 대한 RDP 세션 내에서 Internet Explorer를 시작하고 [**https://www.microsoft.com/ko-kr/download/details.aspx?id=47594**](https://www.microsoft.com/ko-kr/download/details.aspx?id=47594)에서 **Azure AD Connect**를 다운로드합니다.

1. **Microsoft Azure Active Directory Connect **마법사를 시작하고 라이선스 조건을 수락한 다음 **익스프레스 설정** 페이지에서 **사용자 지정**옵션을 선택합니다.

1. 필요한 **구성 요소 설치**페이지에서 모든 선택적 구성 옵션을 선택 취소하고 설치를 시작합니다.

1. **사용자 로그인** 페이지에서 **암호 해시 동기화**만 사용하도록 설정되어 있는지 확인합니다.

1. Azure AD에 연결하라는 메시지가 표시되면 이전 연습에서 만든 **syncadmin **계정의 자격 증명을 사용하여 인증합니다.

1. 디렉터리 연결하라는 메시지가 표시되면 **adatum.com **포리스트를 추가하고 **새 AD 계정 만들기** 옵션을 선택하고 다음 자격 증명을 사용하여 인증합니다.

    - 사용자 이름: **ADATUM\\학생**

    - 암호: **Pa55w.rd1234**

1. **Azure AD 로그인 구성** 페이지에서 **UPN 접미사가 확인된 도메인 이름과 일치하지 않는 경우 사용자가 온-프레미스 자격 증명으로 Azure AD에 로그인할 수 없다**는 경고에 유의하고 **모든 UPN 접미사를 확인된 도메인과 일치시키지 않고 계속** 확인란을 사용합니다.

   > **참고**: 앞서 설명했듯이 사용자 지정 Azure AD DNS 도메인 **adatum.com**을 확인할 수 없으므로 이 결과가 나타납니다.

1. **도메인 및 OU 필터링** 페이지에서 **ToSync** OU만 선택되어 있는지 확인합니다.

1. **사용자를 고유하게 식별**페이지에서 기본 설정을 수락합니다.

1. **사용자 및 장치 필터**페이지에서 기본 설정을 수락합니다.

1. **선택적 기능**페이지에서 기본 설정을 수락합니다.

1. **구성 준비** 페이지에서 구성이 완료될 때 **동기화 시작 프로세스가 **선택되어 설치 프로세스를 계속해야 합니다. 

   > **참고**: 설치에는 약 2 분이 소요됩니다.

1. 구성이 완료되면 Microsoft Azure Active Directory Connect 창을 닫습니다.


#### 작업 3: 디렉터리 동기화 확인

1. RDP 세션 내에서 **adVM**을시작하고, Internet Explorer를 시작하고, http://portal.azure.com**에서 Azure Portal을 찾아보고 [**이전](http://portal.azure.com) 연습에서 만든 **syncadmin **계정을 사용하여 로그인합니다. 

1. Azure Portal에서 **AdatumSync - 개요** 블레이드로 이동합니다.

1. **AdatumSync - 개요** 블레이드에서 AdatumSync Azure AD 테넌트의 **사용자- 모든 사용자** 블레이드입니다.

1. **사용자 - 모든 사용자** 블레이드에는 사용자 개체 목록에 는 **Aduser1** 계정이 포함되며 **Windows 서버 AD**가 **소스** 열에 나타납니다.

1. **사용자 - 모든 사용자** 블레이드에서 **aduser1 - 프로필** 블레이드를표시합니다. **부서**특성은 설정되지 않았습니다.

1. RDP세션 내에서 **adVM,** **Active Directory 관리 센터**로 전환하고, **aduser1** 사용자 계정의 속성을 표시하는 창을 열고 **해당 부서** 특성의 값을 **Sales**로 설정합니다.

1. **adVM**에 RDP 세션 내에서 관리자로 **WindowsPowerShell**을 시작합니다.

1. Windows PowerShell 프롬프트에서 다음을 실행하여 Azure AD Connect 델타 동기화를 시작합니다.

   ```pwsh
   Import-Module -Name 'C:\Program Files\Microsoft Azure AD Sync\Bin\ADSync\ADSync.psd1'
   
   Start-ADSyncSyncCycle -PolicyType Delta
   ```

1. rdP 세션 내에서 **adVM**으로전환하여 Azure Portal을 표시하는 인터넷 탐색기 창으로 전환합니다. 

1. Azure Portal에서 **사용자 - 모든 사용자** 블레이드로 돌아가 페이지를 새로 고칩니다. 

1. **사용자 - 모든 사용자** 블레이드에서 **aduser1 - 프로필** 블레이드를표시합니다. 이제 **부서** 특성이 **Sales**로 설정됩니다.

   > **참고**: **부서** 특성이 설정되지 않은 상태로 유지되면 다른 분 동안 기다렸다가 페이지를 다시 새로 고쳐야 할 수 있습니다.

> **결과**: 이 연습을 완료한 후 디렉터리 동기화를 준비하기 위해 Active Directory를 구성하고, Azure AD Connect를 설치했으며, 확인된 디렉터리 동기화를 구성했습니다.

## 연습 4: 랩 리소스 제거

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

#### 작업 3: Azure AD 테넌트를 삭제합니다.

1. 랩 VM에서 관리자로 Windows PowerShell을 시작합니다. 

1. 랩 VM의 Windows PowerShell 콘솔에서 다음을 실행하여 MsOnline PowerShell 모듈을 설치합니다. 메시지가 표시되면 NuGet 공급자에서 대화 상자를 계속 진행하려면 **Yes**를 클릭합니다.

   ```pwsh
   Install-Module MsOnline -Force
   ```
   
1. 랩 VM의 Windows PowerShell 콘솔에서 다음을 실행하여 AdatumSync Azure AD 테넌트에 연결합니다. 메시지가 표시되면 SyncAdmin 자격 증명으로 로그인합니다.

   ```pwsh
   Connect-MsolService
   ```

1. 랩 VM의 Windows PowerShell 콘솔에서 다음을 실행하여 Azure AD Connect 동기화를 사용하지 않도록 설정합니다.

   ```pwsh
   Set-MsolDirSyncEnabled -EnableDirSync $false -Force
   ```

1. 랩 VM의 Windows PowerShell 콘솔에서 다음을 실행하여 작업이 성공했는지 확인합니다.

   ```pwsh
   (Get-MSOLCompanyInformation).DirectorySynchronizationEnabled 
   ```   

1. 랩 VM의 Azure Portal에서 로그아웃하고 Microsoft Edge 창을 닫습니다. 

1. 랩 VM에서 Microsoft Edge를 시작하고 Azure Portal로 이동한 다음 SyncAdmin 자격 증명을 사용하여 로그인합니다. 

1. Azure Portal에서 AdatumSync Azure AD 테넌트의 **사용자-모든 사용자** 블레이드로 이동하고 AdatumSync 계정을 제외한 모든 사용자를 삭제합니다.

> **참고**: 이 단계를 완료하려면 몇 시간을 기다려야 할 수 있습니다.

1. AdatumSync로 이동 - 개요 블레이드로 이동하고 **속성**을 클릭합니다.

1. Azure Active Directory의 **속성** 블레이드에서 **Azure 리소스에 대한 액세스 관리**에서 **예**를 클릭한 다음 **저장**을 클릭합니다.

1. Azure Portal에서 로그아웃하고 SyncAdmin 자격 증명을 사용하여 다시 로그인합니다. 

1. **AdatumSync - 개요** 블레이드로 이동하고 **디렉터리 삭제**를 클릭하여 Azure AD 테넌트를 삭제합니다.

1. **'AdatumSync' 디렉터리를 삭제하시겠습니까?** 블레이드에서 **삭제**를 클릭합니다.

> **참고**: 이 작업에 대한 추가 정보는 다음을 참조하십시오. https://docs.microsoft.com/ko-kr/azure/active-directory/users-groups-roles/directory-delete-howto  

> **결과**: 이 연습에서는 이 랩에서 사용한 리소스를 제거했습니다.
