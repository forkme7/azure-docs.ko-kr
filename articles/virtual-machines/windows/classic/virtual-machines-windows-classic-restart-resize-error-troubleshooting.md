---
title: "VM 재시작 또는 크기 조정 문제 | Microsoft Docs"
description: "Azure의 기존 Windows Virtual Machine 재시작 또는 크기 조정 관련 클래식 배포 문제 해결"
services: virtual-machines-windows
documentationcenter: 
author: Deland-Han
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: aa854fff-c057-4b8e-ad77-e4dbc39648cc
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: required
ms.date: 11/03/2017
ms.devlang: na
ms.author: delhan
ms.openlocfilehash: bed5da25042d29983bad9a80cd44bdd7df261c2e
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/21/2017
---
# <a name="troubleshoot-classic-deployment-issues-with-restarting-or-resizing-an-existing-windows-virtual-machine-in-azure"></a>Azure의 기존 Windows Virtual Machine 재시작 또는 크기 조정 관련 클래식 배포 문제 해결
> [!div class="op_single_selector"]
> * [클래식](virtual-machines-windows-classic-restart-resize-error-troubleshooting.md)
> * [리소스 관리자](../restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
> 
> 

중지된 Azure Virtual Machine(VM)을 시작하거나, 기존 Azure AM의 크기를 조정하려다 접하는 일반적인 오류는 할당 오류입니다. 이런 오류는 클러스터나 지역에 사용할 수 있는 리소스가 없거나 요청한 VM 크기를 지원할 수 없을 때 발생합니다.

> [!IMPORTANT]
> Azure에는 리소스를 만들고 작업하는 [Resource Manager와 클래식](../../../azure-resource-manager/resource-manager-deployment-model.md)이라는 두 가지 배포 모델이 있습니다.  이 문서에서는 클래식 배포 모델 사용에 대해 설명합니다. 새로운 배포는 대부분 리소스 관리자 모델을 사용하는 것이 좋습니다.
> 
> 

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>감사 로그 수집
문제 해결을 시작하려면 문제와 관련된 오류를 파악하기 위해 감사 로그를 수집합니다.

Azure 포털에서 **찾아보기** > **가상 머신** > *Windows 가상 머신* > **설정** > **감사 로그**를 클릭합니다.

## <a name="issue-error-when-starting-a-stopped-vm"></a>문제: 중지된 VM 시작 시 오류
중지된 VM을 시작하려는데 할당 오류가 발생했습니다.

### <a name="cause"></a>원인
중지된 VM을 시작하기 위한 요청은 클라우드 서비스를 호스트하는 원본 클러스터에서 시도되어야 합니다. 하지만, 클러스터에 요청을 수행하는 데 사용할 여유 공간이 없습니다.

### <a name="resolution"></a>해결 방법
* 새 클라우드 서비스를 만들어서 지역 또는 지역 기반 가상 네트워크와 연결하지만 선호도 그룹과는 연결하지 않습니다.
* 중지된 VM을 삭제합니다.
* 디스크를 사용하여 새 클라우드 서비스에 VM을 다시 만듭니다.
* 다시 만든 VM을 시작합니다.

새 클라우드 서비스를 만들려다 오류가 발생하면, 나중에 다시 시도하거나 클라우드 서비스의 지역을 변경합니다.

> [!IMPORTANT]
> 새 클라우드 서비스는 새로운 이름과 VIP를 갖게 되므로, 기존 클라우드 서비스의 해당 정보를 사용하는 모든 종속성에 대해 해당 정보를 변경해야 합니다.
> 
> 

## <a name="issue-error-when-resizing-an-existing-vm"></a>문제: 기존 VM 재시작 시 오류
기존 VM의 크기를 조정하려는데 할당 오류가 발생했습니다.

### <a name="cause"></a>원인
VM 크기를 조정하기 위한 요청은 클라우드 서비스를 호스트하는 원본 클러스터에서 시도되어야 합니다. 하지만, 클러스터가 요청한 VM 크기를 지원하지 않습니다.

### <a name="resolution"></a>해결 방법
요청한 VM 크기를 줄이고 크기 조정 요청을 다시 시도합니다.

* **모두 찾아보기** > **가상 머신(클래식)** > *사용자의 가상 머신* > **설정** > **크기**를 클릭합니다. 자세한 단계는 [가상 머신 크기 조정](https://msdn.microsoft.com/library/dn168976.aspx)을 참조하세요.

VM 크기를 줄이는 것이 가능하지 않으면, 다음 단계를 수행합니다.

* 새 클라우드 서비스를 만들어서, 선호도 그룹에 연결되지 않도록 하고 선호도 그룹에 연결된 가상 네트워크와 연결되지 않도록 합니다.
* 그 안에 큰 규모의 VM을 새로 만듭니다.

모든 VM을 동일한 클라우드 서비스로 통합할 수 있습니다. 기존 클라우드 서비스가 지역 기반 가상 네트워크와 연결된 경우, 새 클라우드 서비스를 기존 가상 네트워크에 연결할 수 있습니다.

기존 클라우드 서비스가 지역 기반 가상 네트워크에 연결되지 않은 경우에는, 기존 클라우드 서비스의 VM을 삭제하고 디스크에서 새 클라우드 서비스에 VM을 다시 만들어야 합니다. 하지만, 새 클라우드 서비스가 새 이름과 VIP를 갖게 되므로, 기존 클라우드 서비스에 이 정보를 사용하는 모든 종속성에 대해 해당 정보를 업데이트해야 합니다.

## <a name="next-steps"></a>다음 단계
Azure에서 Windows VM을 만들 때 문제가 발생하면 [Azure에서 Windows 가상 컴퓨터 생성 관련 배포 문제 해결](../troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)을 참조하세요.

