# AZ-103 Microsoft Azure 관리자

- **당신은 MCT입니까?** - [MCT 에대한 GitHub 사용자 가이드](https://microsoftlearning.github.io/MCT-User-Guide-KO/)를 살펴보십시오.
- **랩 지침을 수동으로 빌드해야 합니까?** - 지침은 [MicrosoftLearning/Docker-Build](https://github.com/MicrosoftLearning/Docker-Build) 리포지토리에서 사용할 수 있습니다.
- AZ-103 모듈별 랩 목록 보기 - https://microsoftlearning.github.io/AZ-103-MicrosoftAzureAdministrator/

> AZ-100 및 AZ-101 인증은 새로운 AZ-103 Microsoft Azure 관리자 시험으로 대체되고 있습니다! 이 발표에 대한 자세한 내용은 리버티 먼슨의 블로그 https://www.microsoft.com/ko-kr/learning/community-blog-post.aspx?BlogId=8&Id=375217 

> 과정 컨텐츠에 대한 제안이나 일반 의견에는 [MCT Courseware 포럼](https://www.microsoft.com/en-us/learning/mct-central.aspx)을 사용해야 합니다. 또한 버그 및 과정 오류는 [Courseware Support 포럼](https://trainingsupport.microsoft.com/en-us)에 보고할 수 있습니다.
 
새 시험을 지원하기 위해 2019년 5월 3일부터 새로운 AZ-103 GitHub 리포지토리를 소개합니다. 이 때 해당 리포지토리에 있는 모든 AZ-100 및 AZ-101 랩이 이 리포지토리로 이동됩니다. 이러한 랩은 AZ-103에서 재사용되고 있으며 저장소는 하나만 유지 하려고 합니다. AZ-100 및 AZ-101 실험실 번호 매기기 시스템은 유지되므로 여전히 AZ-100 또는 AZ-101 과정을 가르치고 있다면 실험실을 쉽게 식별할 수 있습니다. 또한 최신 버전의 랩을 얻고 발견한 문제를 제출할 수 있습니다.

이 리포지토리에는 다음 랩이 포함됩니다:

-  Azure 이벤트 그리드 및 Azure 논리 앱(az-101-02b)
-  Azure AD ID 보호(az-101-04b)
-  Azure 네트워크 감시자(az-101-03b)
-  Azure DNS 구성(az-100-04b)
-  가상 머신 배포 및 관리(az-100-03)
-  거버넌스 및 규정 준수(az-100-01b)
-  Azure 웹 앱 구현 및 관리(az-101-02)
-  지역 간 ASR 구현(az-101-01)
-  디렉터리 동기화 구현(az-100-05)
-  파일 동기화 구현(az-100-02b)
-  스토리지 구현 및 관리(az-100-02)
-  로드 밸런서 및 트래픽 관리자(az-101-03)
-  온-프레미스 하이퍼 VM을 Azure로 마이그레이션(az-101-01b)
-  역할 기반 액세스 제어(az-100-01)
-  셀프 서비스 암호 재설정(az-100-05b)
-  가상 머신 및 스케일 세트(az-100-03b)
-  VNet 피어링 및 서비스 체인링(az-100-04)

다음 실습은 AZ-103 과정의 일부가 아닙니다:

-  Azure 이벤트 그리드 및 Azure 논리 앱(az-101-02b)
-  Azure 웹 앱 구현 및 관리(az-101-02)
-  온-프레미스 하이퍼 VM을 Azure로 마이그레이션(az-101-01b)
-  Privileged Identity Management (az-101-04)

**저희가 제공 하는 사항?**

*	GitHub에 랩 지침 및 랩 파일을 게시하여 과정 작성자와 MCT 간의 상호 작용을 허용합니다. Azure 플랫폼이 변경됨에 따라 콘텐츠를 최신 상태로 유지하는 데 도움이 되기를 바랍니다.

*	이것은 AZ-103, 마이크로소프트 Azure 관리자 과정에 대 한 GitHub 리포지토리입니다. 

*	각 리포지토리 에는 지침 폴더의 마크다운 형식의 랩 가이드가 있습니다. 적절한 경우 Allfiles\Labfiles 폴더 내에서 랩을 완료하는 데 필요한 추가 파일도 있습니다. 모든 과정에 해당 랩 파일이 있는 것은 아닙니다. 

*	각 전달에 대해 트레이너는 GitHub에서 최신 파일을 다운로드해야 합니다. 또한 트레이너는 문제 탭을 확인하여 다른 MCT가 오류를 보고했는지 확인해야 합니다.  

*	랩 타이밍 추정치가 제공되지만 트레이너는 청중을 기반으로 정확한지 확인해야 합니다.

*	랩을 수행하려면 인터넷 연결및 Azure 구독이 필요합니다. 클라우드 셸 사용에 대한 자세한 내용은 강사 준비 가이드를 참조하십시오. 

**저희가 제공하는 방식?**

*	이러한 과정을 가르치는 경우 개선 할 영역을 식별하는 경우 문제 탭을 사용하여 피드백을 제공하십시오. 변경 내용을 통합하기 위해 정기적으로 새 파일을 만듭니다. 

**AZ-103 과정에 대한 일반적인 의견**

* 모든 랩의 PowerShell 스크립트는 현재 버전의 Azure PowerShell Az 모듈을 사용합니다.

* 필수는 아니지만 각 랩을 완료할 때 기존 리소스의 프로비저닝을 해제하는 것이 좋습니다. 이렇게 하면 기본 vCPU 할당량 제한을 초과할 위험을 완화하고 사용 요금을 최소화할 수 있습니다.

* 이러한 리전의 Azure 리전 및 리소스의 가용성은 사용 중인 구독 유형에 따라 어느 정도 달라집니다. 구독에서 사용할 수 있는 Azure 지역을 식별하려면 https://azure.microsoft.com/ko-kr/regions/offers/ . 이러한 리전에서 사용할 수 있는 리소스를 식별하려면 https://azure.microsoft.com/ko-kr/global-infrastructure/services/ . 이러한 제한으로 인해 특히 Azure VM을 프로비전할 때 템플릿 유효성 검사 또는 템플릿 배포 중에 오류가 발생할 수 있습니다. 이 경우 오류 메시지를 검토하고 다른 VM 크기 또는 다른 지역으로 배포를 다시 시도합니다.

* Azure Cloud Shell을 처음 시작할 때 클라우드 셸 파일을 유지하도록 Azure 파일 공유를 만들라는 메시지가 표시될 수 있습니다. 이 경우 일반적으로 기본값을 허용할 수 있으며, 이로 인해 자동으로 생성된 리소스 그룹에서 저장소 계정이 생성됩니다. 해당 저장소 계정을 삭제하면 이 같은 일이 다시 발생할 수 있습니다.

* 템플릿 기반 배포를 수행하기 전에 템플릿에서 참조되는 리소스 형식의 프로비저닝을 처리하는 공급자를 등록해야 할 수 있습니다. Azure Resource Manager 템플릿을 사용하여 이러한 리소스 공급자가 관리하는 리소스를 배포하는 데 필요한 일회성 작업(구독당)입니다(이러한 리소스 공급자가 아직 등록되지 않은 경우). Azure 포털에서 구독의 리소스 공급자 블레이드에서 등록을 수행하거나 클라우드 셸을 사용하여 Register-AzResourceProvider PowerShell cmdlet 또는 az 공급자 Azure CLI 명령을 실행할 수 있습니다.

이 GitHub 리포지토리를 사용하면 랩에 협업 감각을 제공하고 랩 환경의 전반적인 품질을 개선할 수 있기를 바랍니다. 

문안 인사,
*Azure 관리자 과정웨어 팀*