---
title: "응용 프로그램 프록시 응용 프로그램에 대해 Single Sign-On을 구성하는 방법 | Microsoft Docs"
description: "응용 프로그램 프록시 응용 프로그램에 대해 Single Sign-On을 신속하게 구성하는 방법"
services: active-directory
documentationcenter: 
author: ajamess
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 3fddcb9387017bfaf48199bdacd839bfc357a820
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-configure-single-sign-on-to-an-application-proxy-application"></a>응용 프로그램 프록시 응용 프로그램에 대해 Single Sign-On을 구성하는 방법

SSO(Single Sign-On)를 사용하면 사용자가 여러 번 인증하지 않고도 응용 프로그램에 액세스할 수 있습니다. Azure Active Directory에 대해 클라우드에서 단일 인증을 허용하고 서비스 또는 커넥터가 사용자를 가장하여 응용 프로그램에서 추가 인증 질문을 완료할 수 있습니다.

## <a name="how-to-configure-single-sign-on"></a>Single Sign-On을 구성하는 방법
SSO를 구성하려면 먼저 응용 프로그램이 Azure Active Directory를 통해 사전 인증할 수 있도록 구성되어 있는지 확인합니다. 확인하려면 **Azure Active Directory** -&gt; **엔터프라이즈 응용 프로그램** -&gt; **모든 응용 프로그램** -&gt; 해당 응용 프로그램 **-&gt; 응용 프로그램 프록시**로 이동합니다. 이 페이지에서 "사전 인증" 필드가 표시되고 "Azure Active Directory"로 설정되어 있는지 확인합니다. 

사전 인증 방법에 대한 자세한 내용은 [앱 게시 설명서](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal)의 4단계를 참조하세요.

   ![Azure Portal의 사전 인증 방법](./media/application-proxy-config-sso-how-to/app-proxy.png)

## <a name="configuring-single-sign-on-modes-for-application-proxy-applications"></a>응용 프로그램 프록시 응용 프로그램에 대한 Single Sign-On 모드 구성
그런 다음 특정 유형의 Single Sign-On을 구성합니다. 로그온 방법은 백 엔드 응용 프로그램에서 사용하는 인증 유형에 따라 분류됩니다. 앱 프록시 응용 프로그램은 세 가지 유형의 로그온을 지원합니다.

-   **암호 기반 로그온**: 암호 기반 로그온은 사용자 이름과 암호 필드를 사용하여 로그온하는 모든 응용 프로그램에 사용할 수 있습니다. 구성 단계는 [암호-SSO 구성 설명서](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-whats-new-azure-portal#bring-your-own-password-sso-applications)를 참조하세요.

-   **Windows 통합 인증**: IWA(Windows 통합 인증)를 사용하는 응용 프로그램의 경우 KDC(Kerberos 제한 위임)를 통해 Single Sign-On을 사용할 수 있습니다. 그러면 Active Directory에서 응용 프로그램 프록시 커넥터에게 권한이 부여되어 사용자를 가장하고 대신 토큰을 주고받을 수 있습니다. KCD 구성에 대한 자세한 내용은 [Single Sign-on과 KCD 설명서](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd)를 참조하세요.

-   **헤더 기반 로그온**: 헤더 기반 로그온은 파트너 관계를 통해 활성화되며 몇 가지 추가 구성이 필요합니다. 인증에 헤더를 사용하는 응용 프로그램에 대해 Single Sign-On을 구성하기 위한 파트너 관계 및 단계별 지침은 [Azure AD용 PingAccess 설명서](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access)를 참조하세요.

이러한 각 옵션은 "엔터프라이즈 응용 프로그램"에서 응용 프로그램으로 이동하여 왼쪽 메뉴에서 **Single Sign-On** 페이지를 열면 찾을 수 있습니다. 응용 프로그램이 이전 포털에서 작성된 경우 이러한 옵션이 모두 표시되지 않을 수도 있습니다.

이 페이지에는 추가 로그온 옵션인 연결된 로그온이 있습니다. 응용 프로그램 프록시에서도 지원됩니다. 그러나 이 옵션은 응용 프로그램에 Single Sign-On을 추가하지 않습니다. 즉, 응용 프로그램에 이미 Active Directory Federation Services와 같은 다른 서비스를 사용하여 Single Sign-On이 구현되어 있을 수 있습니다. 

이 옵션을 사용하면 관리자가 응용 프로그램에 액세스할 때 사용자에게 제일 먼저 표시되는 응용 프로그램에 대한 링크를 만들 수 있습니다. 예를 들어 Active Directory Federation Services 2.0을 사용하여 사용자를 인증하도록 구성된 응용 프로그램이 있는 경우, 관리자가 "연결된 로그온" 옵션을 사용하여 액세스 패널에 이에 대한 링크를 만들 수 있습니다.

## <a name="next-steps"></a>다음 단계
[응용 프로그램 프록시를 사용하여 앱에 Single Sign-On 제공](active-directory-application-proxy-sso-using-kcd.md)
