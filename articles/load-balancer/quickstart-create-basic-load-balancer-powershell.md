---
title: 공용 부하 분산 장치 만들기 - PowerShell | Microsoft Docs
description: PowerShell을 사용하여 Resource Manager에서 공용 부하 분산 장치를 만드는 방법에 대해 알아봅니다.
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
tags: azure-resource-manager
ms.assetid: 8257f548-7019-417f-b15f-d004a1eec826
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 4ce11b0b06e1feaf55d17e25500c43a7eb1bf3d5
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/18/2018
---
# <a name="get-started"></a>PowerShell을 사용하여 Resource Manager에서 공용 부하 분산 장치 만들기

> [!div class="op_single_selector"]
> * [포털](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [템플릿](../load-balancer/load-balancer-get-started-internet-arm-template.md)



[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-the-solution-by-using-azure-powershell"></a>Azure PowerShell을 사용하여 솔루션 배포

다음 절차에서는 PowerShell과 함께 Azure Resource Manager를 사용하여 공용 부하 분산 장치를 만드는 방법을 설명합니다. Azure Resource Manager를 사용하면 각 리소스가 개별적으로 생성되고 구성된 다음, 함께 사용되어 부하 분산 장치를 만듭니다.

부하 분산 장치를 배포하려면 다음 개체를 만들고 구성해야 합니다.

* 프런트 엔드 IP 구성: 들어오는 네트워크 트래픽에 대한 공용 IP(PIP) 주소를 포함합니다.
* 백 엔드 주소 풀: 부하 분산 장치의 네트워크 트래픽을 받는 가상 머신에 대한 NIC(네트워크 인터페이스)를 포함합니다.
* 부하 분산 규칙: 백 엔드 주소 풀에 있는 포트에 부하 분산 장치의 공용 포트를 매핑하는 규칙을 포함합니다.
* 인바운드 NAT 규칙: 백 엔드 주소 풀에 있는 특정 가상 머신에 대한 포트에 부하 분산 장치의 공용 포트를 매핑하는 규칙을 포함합니다.
* 프로브: 백 엔드 주소 풀의 가상 머신 인스턴스의 가용성을 확인하는 데 사용하는 상태 프로브를 포함합니다.

자세한 내용은 [부하 분산 장치에 대한 Azure Resource Manager 지원](load-balancer-arm.md)을 참조하세요.

## <a name="set-up-powershell-to-use-resource-manager"></a>Resource Manager를 사용하도록 PowerShell 설치

PowerShell에 대한 Azure Resource Manager 모듈의 최신 프로덕션 버전이 있는지 확인합니다.

1. Azure에 로그인합니다.

    ```powershell
    Login-AzureRmAccount
    ```

    메시지가 표시되면 자격 증명을 입력합니다.

2. 계정에 대한 구독을 확인합니다.

    ```powershell
    Get-AzureRmSubscription
    ```

3. 사용할 Azure 구독을 선택합니다.

    ```powershell
    Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'
    ```

4. 리소스 그룹을 만듭니다. (기존 리소스 그룹을 사용하는 경우에는 이 단계를 건너뜁니다.)

    ```powershell
    New-AzureRmResourceGroup -Name NRP-RG -location "West US"
    ```

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>프런트 엔드 IP 풀에 대한 공용 IP 주소 및 가상 네트워크 만들기

1. 서브넷 및 가상 네트워크를 만듭니다.

    ```powershell
    $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
    New-AzureRmvirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet
    ```

2. DNS 이름이 **loadbalancernrp.westus.cloudapp.azure.com**인 프런트 엔드 IP 풀에서 사용할 **PublicIP**라는 Azure 공용 IP 주소 리소스를 만듭니다. 다음 명령은 정적 할당 형식을 사용합니다.

    ```powershell
    $publicIP = New-AzureRmPublicIpAddress -Name PublicIp -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Static -DomainNameLabel loadbalancernrp
    ```

   > [!IMPORTANT]
   > 부하 분산 장치는 FQDN에 대한 접두사로 공용 IP의 도메인 레이블을 사용합니다. 이는 부하 분산 장치 FQDN으로 클라우드 서비스를 사용하는 클래식 배포 모델과 다릅니다.
   > 이 예제에서는 FQDN이 **loadbalancernrp.westus.cloudapp.azure.com**입니다.

## <a name="create-a-front-end-ip-pool-and-a-back-end-address-pool"></a>프런트 엔드 IP 풀 및 백 엔드 주소 풀 만들기

1. **PublicIp** 리소스를 사용하는 **LB-Frontend**라는 프런트 엔드 IP 풀을 만듭니다.

    ```powershell
    $frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PublicIpAddress $publicIP
    ```

2. **LB-backend**라는 백 엔드 주소 풀을 만듭니다.

    ```powershell
    $beaddresspool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name LB-backend
    ```

## <a name="create-nat-rules-a-load-balancer-rule-a-probe-and-a-load-balancer"></a>NAT 규칙, 부하 분산 장치 규칙, 프로브 및 부하 분산 장치 만들기

이 예제에서는 다음 항목을 만듭니다.

* 포트 3441~포트 3389에서 들어오는 모든 트래픽을 변환하는 NAT 규칙
* 포트 3442~포트 3389에서 들어오는 모든 트래픽을 변환하는 NAT 규칙
* **HealthProbe.aspx**
* 포트 80~포트 80에서 들어오는 모든 트래픽을 백 엔드 풀에 있는 주소로 분산하는 부하 분산 장치 규칙
* 이러한 개체를 모두 사용하는 부하 분산 장치

다음 단계를 사용:

1. NAT 규칙을 만듭니다.

    ```powershell
    $inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP1 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

    $inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP2 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389
    ```

2. 상태 프로브를 만듭니다. 프로브를 구성하는 방법은 두 가지가 있습니다.

    HTTP 프로브

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
    ```

    TCP 프로브

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -Protocol Tcp -Port 80 -IntervalInSeconds 15 -ProbeCount 2
    ```

3. 부하 분산 장치를 만듭니다.

    ```powershell
    $lbrule = New-AzureRmLoadBalancerRuleConfig -Name HTTP -FrontendIpConfiguration $frontendIP -BackendAddressPool  $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80
    ```

4. 이전에 만든 개체를 사용하여 부하 분산 장치를 만듭니다.

    ```powershell
    $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name NRP-LB -Location 'West US' -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe
    ```

## <a name="create-nics"></a>NIC 만들기

네트워크 인터페이스를 만든(또는 기존 인터페이스 수정) 다음 NAT 규칙, 부하 분산 장치 규칙 및 프로브에 연결:

1. NIC를 만들어야 하는 가상 네트워크 및 가상 네트워크 서브넷을 가져옵니다.

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG
    $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet
    ```

2. **lb-nic1-be**라는 NIC를 만들고 첫 번째 NAT 규칙 및 첫 번째(유일한) 백 엔드 주소 풀과 연결합니다.

    ```powershell
    $backendnic1= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic1-be -Location 'West US' -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]
    ```

3. **lb-nic2-be**라는 NIC를 만들고 두 번째 NAT 규칙 및 첫 번째(유일한) 백 엔드 주소 풀과 연결합니다.

    ```powershell
    $backendnic2= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic2-be -Location 'West US' -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]
    ```

4. NIC를 확인합니다.

        $backendnic1

    예상 출력:

        Name                 : lb-nic1-be
        ResourceGroupName    : NRP-RG
        Location             : westus
        Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
        ResourceGuid         : 896cac4f-152a-40b9-b079-3e2201a5906e
        ProvisioningState    : Succeeded
        Tags                 :
        VirtualMachine       : null
        IpConfigurations     : [
                            {
                            "Name": "ipconfig1",
                            "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                            "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1",
                            "PrivateIpAddress": "10.0.2.6",
                            "PrivateIpAllocationMethod": "Static",
                            "Subnet": {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                            },
                            "ProvisioningState": "Succeeded",
                            "PrivateIpAddressVersion": "IPv4",
                            "PublicIpAddress": {
                                "Id": null
                            },
                            "LoadBalancerBackendAddressPools": [
                                {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/backendAddressPools/LB-backend"
                                }
                            ],
                            "LoadBalancerInboundNatRules": [
                                {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/inboundNatRules/RDP1"
                                }
                            ],
                            "Primary": true,
                            "ApplicationGatewayBackendAddressPools": []
                            }
                        ]
        DnsSettings          : {
                            "DnsServers": [],
                            "AppliedDnsServers": [],
                            "InternalDomainNameSuffix": "prcwibzcuvie5hnxav0yjks2cd.dx.internal.cloudapp.net"
                        }
        EnableIPForwarding   : False
        NetworkSecurityGroup : null
        Primary              :

5. `Add-AzureRmVMNetworkInterface` cmdlet을 사용하여 NIC를 다른 VM에 할당합니다.

## <a name="create-a-virtual-machine"></a>가상 머신 만들기

가상 컴퓨터 만들기 및 NIC 할당에 대한 지침은 [PowerShell을 사용하여Azure VM 만들기](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json)를 참조하세요.

## <a name="add-the-network-interface-to-the-load-balancer"></a>부하 분산 장치에 네트워크 인터페이스 추가

1. Azure에서 부하 분산 장치를 검색합니다.

    (아직 수행하지 않은 경우) 부하 분산 장치 리소스를 변수로 로드합니다. 변수는 **$lb**입니다. 이전에 만든 부하 분산 장치 리소스에서 동일한 이름을 사용합니다.

    ```powershell
    $lb= get-azurermloadbalancer -name NRP-LB -resourcegroupname NRP-RG
    ```

2. 백 엔드 구성을 변수로 로드합니다.

    ```powershell
    $backend=Get-AzureRmLoadBalancerBackendAddressPoolConfig -name LB-backend -LoadBalancer $lb
    ```

3. 이미 만든 네트워크 인터페이스를 변수로 로드합니다. 변수 이름은 **$nic**입니다. 네트워크 인터페이스 이름은 이전 예제와 같습니다.

    ```powershell
    $nic =get-azurermnetworkinterface -name lb-nic1-be -resourcegroupname NRP-RG
    ```

4. 네트워크 인터페이스에서 백 엔드 구성을 변경합니다.

    ```powershell
    $nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend
    ```

5. 네트워크 인터페이스 개체를 저장합니다.

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $nic
    ```

    네트워크 인터페이스가 부하 분산 장치 백 엔드 풀에 추가된 후 해당 부하 분산 장치 리소스에 대한 부하 분산 규칙에 따라 네트워크 트래픽을 받기 시작합니다.

## <a name="update-an-existing-load-balancer"></a>기존 부하 분산 장치 업데이트

1. 이전 예제에서 부하 분산 장치를 사용하여, `Get-AzureLoadBalancer`를 통해 부하 분산 장치 개체를 변수 **$slb**로 할당합니다.

    ```powershell
    $slb = get-AzureRmLoadBalancer -Name NRP-LB -ResourceGroupName NRP-RG
    ```

2. 다음 예제에서는 프런트 엔드에 있는 포트 81과 백 엔드 풀의 포트 8181을 사용하여 인바운드 NAT 규칙을 기존 부하 분산 장치에 추가합니다.

    ```powershell
    $slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol TCP
    ```

3. `Set-AzureLoadBalancer`을 사용하여 새 구성을 저장합니다.

    ```powershell
    $slb | Set-AzureRmLoadBalancer
    ```

## <a name="remove-a-load-balancer"></a>부하 분산 장치 제거하기

`Remove-AzureLoadBalancer` 명령을 사용하여 **NRP-RG**라는 리소스 그룹에서 이전에 생성한 **NRP-LB**라는 부하 분산 장치를 삭제합니다.

```powershell
Remove-AzureRmLoadBalancer -Name NRP-LB -ResourceGroupName NRP-RG
```

> [!NOTE]
> 선택적 스위치 **-Force** 를 사용하여 삭제에 대한 프롬프트를 방지할 수 있습니다.

## <a name="next-steps"></a>다음 단계

[내부 부하 분산 장치 구성 시작](load-balancer-get-started-ilb-arm-ps.md)

[부하 분산 장치 배포 모드 구성](load-balancer-distribution-mode.md)

[부하 분산 장치에 대한 유휴 TCP 시간 제한 설정 구성](load-balancer-tcp-idle-timeout.md)
