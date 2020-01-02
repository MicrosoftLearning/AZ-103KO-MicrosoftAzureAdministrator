---
lab:
    title: 'Azure DNS 구성'
    module: '모듈 04 - 가상 네트워킹'
---

# 랩: Azure DNS 구성.
  
이 랩의 모든 작업은 Azure Portal(PowerShell Cloud Shell 세션 포함)에서 수행됩니다. 

   > **참고**: Cloud Shell을 사용하지 않는 경우 랩 가상 머신에 Azure PowerShell 1.2.0 모듈(또는 최신 모듈)이 설치되어 있어야 합니다. [https://docs.microsoft.com/ko-kr/powershell/azure/install-az-ps](https://docs.microsoft.com/ko-kr/powershell/azure/install-az-ps)

랩 파일: 

-  **Labfiles\\Module_04\\Configure_Azure_DNS\\az-100-04b_01_azuredeploy.json**

-  **Labfiles\\Module_04\\Configure_Azure_DNS\\az-100-04b_02_azuredeploy.json**

-  **Labfiles\\Module_04\\Configure_Azure_DNS\\az-100-04_azuredeploy.parameters.json**


### 시나리오
  
Adatum Corporation은 프라이빗 DNS 서버를 배포하지 않고도 Azure에서 공용 및 개인 DNS 서비스를 구현하고자 합니다. 


### 목표
  
이 랩을 완료하면 다음과 같은 역량을 갖추게 됩니다.

- 공용 도메인에 대한 Azure DNS 구성.

- 개인 도메인에 대한 Azure DNS 구성.


### 연습 1: 공용 도메인에 대한 Azure DNS 구성.

이 연습의 주요 작업은 다음과 같습니다.

1. 공용 DNS 영역 만들기.

1. 공용 DNS 영역에서 DNS 레코드 만들기.

1. 공용 도메인에 대한 Azure DNS 기반 이름 확인의 유효성 검사.


#### 작업 1: 공용 DNS 영역 만들기.
  
1. 랩 가상 머신에서 Microsoft Edge를 시작하고 [**http://portal.azure.com**](http://portal.azure.com)에서 Azure Portal을 찾아보고 이 랩에서 사용하려는 Azure 구독에서 소유자 역할이 있는 Microsoft 계정을 사용하여 로그인합니다.

1. Azure Portal에서 **리소스 만들기** 블레이드로 이동합니다.

1. **리소스 만들기** 블레이드를 통해 Azure Marketplace에서 **DNS 영역**을 검색합니다.

1. **DNS 영역**을 선택한 다음 **만들기**를 클릭합니다.

1. **DNS 영역 만들기** 블레이드에서 다음 설정을 사용하여 새 DNS 영역을 만듭니다. 

    - 구독: 이 랩에서 사용 중인 Azure 구독의 이름.

    - 리소스 그룹: 새 리소스 그룹 **az1000401b-RG**의 이름

    - 이름: **com**네임스페이스의 고유하고 유효한 DNS 도메인 이름.

    - 리소스 그룹 위치: **미국 동부**(또는 지원되는 가까운 지역)


#### 작업 2: 공용 DNS 영역에서 DNS 레코드 만들기.
  
1. 랩 컴퓨터에서 Powershell 세션을 열고 다음 명령을 실행하여 랩 컴퓨터의 공용 IP 주소를 확인합니다.

```pwsh
   Invoke-RestMethod http://ipinfo.io/json | Select-Object -ExpandProperty IP
```

   > **참고**: 이 IP 주소를 기록해 둡니다. 이 작업에서 나중에 사용됩니다.

1. Azure Portal의 Cloud Shell에서 PowerShell 세션을 시작합니다. 

   > **참고**: 현재의 Azure 구독에서 Cloud Shell을 처음 시작하는 경우, Cloud Shell 파일을 유지하도록 Azure 파일 공유를 만들라는 메시지가 표시됩니다. 이 경우 기본값을 허용하면 자동으로 생성된 리소스 그룹에서 스토리지 계정이 생성됩니다.

1. Cloud Shell 창에서 공용 IP 주소 리소스를 만들려면 다음을 실행합니다.

```pwsh
   $rg = Get-AzResourceGroup -Name az1000401b-RG

   New-AzPublicIpAddress -ResourceGroupName $rg.ResourceGroupName -Sku Basic -AllocationMethod Static -Name az1000401b-pip -Location $rg.Location
```

1. Azure Portal에서 **az1000401b-RG** 리소스 그룹 블레이드로 이동합니다.

1. **az1000401b-RG** 리소스 그룹 블레이드에서 새로 생성된 공용 DNS 영역을 표시하는 블레이드로 이동합니다.

1. DNS 영역 블레이드에서 **레코드 집합**을 클릭하여 **레코드 집합 추가** 블레이드로 이동합니다.

1. 다음 설정을 사용하여 DNS 레코드를 만듭니다.

    - 이름: **mylabvmpip**

    - 유형: **A**

    - 별칭 레코드 집합: **아니요**

    - TTL: **1**

    - TTL 단위: **시간**

    - IP 주소: 이 작업의 앞에서 확인한 랩 컴퓨터의 공용 IP 주소.

1. 개요 블레이드에서 **레코드 집합**을 클릭하고 다음 설정을 사용하여 또 다른 레코드를 만듭니다.

    - 이름: **myazurepip**

    - 유형: **A**

    - 별칭 레코드 집합: **예**

    - 별칭 유형: **Azure 리소스**

    - 구독 선택: 이 랩에서 사용 중인 Azure 구독의 이름.

    - Azure 리소스: **az1000401b-pip**

    - TTL: **1**

    - TTL 단위: **시간**


#### 작업 3: 공용 도메인에 대한 Azure DNS 기반 이름 확인의 유효성 검사.

1. DNS 영역 블레이드에서 만든 영역을 호스트하는 이름 서버 목록을 확인합니다. 다음 단계에서 명명된 첫 번째 단계를 사용합니다.

1. 랩 가상 머신에서 명령 프롬프트를 시작하고 다음을 실행하여 새로 만든 두 DNS 레코드의 이름 확인을 확인합니다(여기서 &lt;custom_DNS_domain&gt;은 이 연습의 첫 번째 작업에서 만든 사용자 지정 DNS 도메인을 나타내고 &lt;name_server&gt;는 이전 단계에서 식별한 DNS 이름 서버의 이름을 나타냅니다). 

```
   nslookup mylabvmpip.<custom_DNS_domain> <name_server>
   
   nslookup myazurepip.<custom_DNS_domain> <name_server>
```

1. 반환된 IP 주소가 이 작업의 앞에서 확인한 주소와 일치하는지 확인합니다.

> **결과**: 이 연습을 완료한 후 공용 DNS 영역을 만들고, 공용 DNS 영역에서 DNS 레코드를 만들고, 공용 도메인에 대한 Azure DNS 기반 이름 확인의 유효성을 검사했습니다.


### 연습 2: 개인 도메인에 대한 Azure DNS 구성.
  
이 연습의 주요 작업은 다음과 같습니다.

1. 다중 가상 네트워크 환경 프로비전.

1. 프라이빗 DNS 영역 만들기.

1. Azure VM을 가상 네트워크에 배포

1. 개인 도메인에 대한 Azure DNS 기반 이름 예약 및 해결 방법의 유효성 검사.


#### 작업 1: 다중 가상 네트워크 환경 프로비전.
  
1. Azure Portal에서 Cloud Shell 내 PowerShell 세션을 시작합니다. 

1. Cloud Shell 창에서 리소스 그룹을 만들려면 다음을 실행합니다.

```pwsh
   $rg1 = Get-AzResourceGroup -Name 'az1000401b-RG'

   $rg2 = New-AzResourceGroup -Name 'az1000402b-RG' -Location $rg1.Location
```

1. Cloud Shell 창에서 두 개의 Azure 가상 네트워크를 만들려면 다음을 실행합니다.

```pwsh
   $subnet1 = New-AzVirtualNetworkSubnetConfig -Name subnet1 -AddressPrefix '10.104.0.0/24'

   $vnet1 = New-AzVirtualNetwork -ResourceGroupName $rg2.ResourceGroupName -Location $rg2.Location -Name az1000402b-vnet1 -AddressPrefix 10.104.0.0/16 -Subnet $subnet1

   $subnet2 = New-AzVirtualNetworkSubnetConfig -Name subnet1 -AddressPrefix '10.204.0.0/24'

   $vnet2 = New-AzVirtualNetwork -ResourceGroupName $rg2.ResourceGroupName -Location $rg2.Location -Name az1000402b-vnet2 -AddressPrefix 10.204.0.0/16 -Subnet $subnet2
```

#### 작업 2: 프라이빗 DNS 영역 만들기.

1. Cloud Shell 창에서 다음을 실행하여 첫 번째 가상 네트워크 지원 등록 및 두 번째 가상 네트워크 지원 해결 방법을 사용하여 프라이빗 DNS 영역을 만듭니다.

```pwsh
   
   Install-Module -Name Az.PrivateDns -force
   
   $vnet1 = Get-AzVirtualNetwork -Name az1000402b-vnet1

   $vnet2 = Get-AzVirtualNetwork -name az1000402b-vnet2
   
   $zone = New-AzPrivateDnsZone -Name adatum.corp -ResourceGroupName $rg2.ResourceGroupName

   $vnet1link = New-AzPrivateDnsVirtualNetworkLink -ZoneName $zone.Name -ResourceGroupName $rg2.ResourceGroupName -Name "vnet1Link" -VirtualNetworkId $vnet1.id -EnableRegistration

   $vnet2link = New-AzPrivateDnsVirtualNetworkLink -ZoneName $zone.Name -ResourceGroupName $rg2.ResourceGroupName -Name "vnet2Link" -VirtualNetworkId $vnet2.id
```

1. Cloud Shell 창에서 다음을 실행하여 프라이빗 DNS 영역이 성공적으로 만들어졌는지 확인합니다.

```pwsh
   Get-AzPrivateDnsZone -ResourceGroupName $rg2.ResourceGroupName
```


#### 작업 3: Azure VM을 가상 네트워크에 배포.

1. Cloud Shell 창에서 **az-100-04b_01_azuredeploy.json,** **az-100-04b_02_azuredeploy.json** 및 **az-100-04_azuredeploy.data.json** 파일을 업로드합니다.

1. Cloud Shell 창에서 Azure VM을 첫 번째 가상 네트워크에 배포하려면 다음을 실행합니다.

```pwsh
   cd $home

   New-AzResourceGroupDeployment -ResourceGroupName $rg2.ResourceGroupName -TemplateFile "./az-100-04b_01_azuredeploy.json" -TemplateParameterFile "./az-100-04_azuredeploy.parameters.json" -AsJob
```

1. Cloud Shell 창에서 Azure VM을 두 번째 가상 네트워크에 배포하려면 다음을 실행합니다.

```pwsh
   New-AzResourceGroupDeployment -ResourceGroupName $rg2.ResourceGroupName -TemplateFile "./az-100-04b_02_azuredeploy.json" -TemplateParameterFile "./az-100-04_azuredeploy.parameters.json" -AsJob
```

   > **참고**: 다음 작업을 진행하기 전에 두 배포가 모두 완료될 때까지 기다립니다. Cloud Shell 창에서 'Get-Job' cmdlet을 실행하여 작업 상태를 식별할 수 있습니다.


#### 작업 4: 개인 도메인에 대한 Azure DNS 기반 이름 예약 및 해결 방법의 유효성 검사.

1. Azure Portal에서 **az1000402b-vm2**Azure VM의 블레이드로 이동합니다. 

1. **az1000402b-vm2** 블레이드의 **개요** 창에서 RDP 파일을 생성하고 **az1000402b-vm2**에 연결하는 데 사용합니다.

1. 메시지가 표시되면 다음 자격 증명을 지정하여 인증합니다.

    - 사용자 이름: **학생**

    - 암호: **Pa55w.rd1234**

1. 원격 데스크톱 세션 내에서 **az1000402b-vm2로** 명령 프롬프트 창을 시작하고 다음을 실행합니다. 

```
   nslookup az1000402b-vm1.adatum.corp
```

1. 이름이 성공적으로 확인되었는지 확인합니다.

1. 랩 가상 머신으로 다시 전환하고 Azure Portal 창의 Cloud Shell 창에서 다음을 실행하여 프라이빗 DNS 영역에서 추가 DNS 레코드를 만듭니다.

```pwsh
   New-AzPrivateDnsRecordSet -ResourceGroupName $rg2.ResourceGroupName -Name www -RecordType A -ZoneName adatum.corp -Ttl 3600 -PrivateDnsRecords (New-AzPrivateDnsRecordConfig -IPv4Address "10.104.0.4")
```

1. 원격 데스크톱 세션을 **az1000402b-vm2**로 다시 전환하고 명령 프롬프트 창에서 다음을 실행합니다. 

```
   nslookup www.adatum.corp
```

1. 이름이 성공적으로 확인되었는지 확인합니다.

> **결과**: 이 연습을 완료한 후 다중 가상 네트워크 환경을 프로비전하고, 프라이빗 DNS 영역을 만들고, Azure VM을 가상 네트워크에 배포하고, 개인 도메인에 대한 Azure DNS 기반 이름 예약 및 확인의 유효성을 검사했습니다.

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
