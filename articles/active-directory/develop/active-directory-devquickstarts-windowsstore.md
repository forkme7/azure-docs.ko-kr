---
title: "Azure AD Windows 유니버설 플랫폼(UWP/XAML) 시작 | Microsoft Docs"
description: "로그인을 위해 Azure AD와 통합되고 OAuth를 사용하여 Azure AD로 보호되는 API를 호출하는 Windows 스토어 앱을 빌드합니다."
services: active-directory
documentationcenter: windows
author: jmprieur
manager: mtillman
editor: 
ms.assetid: 3b96a6d1-270b-4ac1-b9b5-58070c896a68
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 11/30/2017
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: b8bfbfca8daf506df083555d3046433b63e6bffe
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/21/2018
---
# <a name="azure-ad-windows-universal-platform-uwpxaml-getting-started"></a>Azure AD Windows 유니버설 플랫폼(UWP/XAML) 시작
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

> [!NOTE]
> Windows Store 8.1 및 이전 버전 프로젝트는 Visual Studio 2017에서 지원되지 않습니다.  자세한 내용은 [Visual Studio 2017 플랫폼 대상 지정 및 호환성](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs)을 참조하세요.

Windows 스토어용 앱을 개발하는 경우 Azure AD(Azure Active Directory)를 사용하면 간단하고 편리하게 Active Directory 계정으로 사용자를 인증할 수 있습니다. Azure AD와 통합하면 앱에서 Office 365 API 또는 Azure API 같은 Azure AD를 통해 보호되는 웹 API를 안전하게 사용할 수 있습니다.

보호된 리소스에 액세스해야 하는 Windows 스토어 데스크톱 앱의 경우 Azure AD는 ADAL(Active Directory 인증 라이브러리)을 제공합니다. ADAL의 유일한 용도는 앱이 쉽게 액세스 토큰을 가져오도록 하는 것입니다. 이 작업이 얼마나 쉬운지 보여 드리기 위해 이 문서에서는 다음 작업을 수행하는 DirectorySearcher Windows 스토어 앱을 빌드해 보겠습니다.

* [OAuth 2.0 인증 프로토콜](https://msdn.microsoft.com/library/azure/dn645545.aspx)을 사용하여 Azure AD Graph API를 호출하기 위한 액세스 토큰을 가져옵니다.
* 지정된 UPN(사용자 계정 이름)을 가진 사용자를 디렉터리에서 검색합니다.
* 사용자를 로그아웃합니다.

## <a name="before-you-get-started"></a>시작하기 전에
* [기본 프로젝트](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/skeleton.zip)를 다운로드하거나 [완성된 샘플](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip)을 다운로드하세요. 각 다운로드는 Visual Studio 2015 솔루션입니다.
* 또한 사용자를 만들고 앱을 등록할 Azure AD 테넌트도 필요합니다. 테넌트가 아직 없는 경우 [얻는 방법을 알아보세요](active-directory-howto-tenant.md).

준비기 완료되면 다음 세 섹션의 절차를 수행합니다.

## <a name="step-1-register-the-directorysearcher-app"></a>1단계: DirectorySearcher 앱 등록
앱에서 토큰을 가져올 수 있도록 먼저 Azure AD 테넌트에 앱을 등록하고 Azure AD Graph API에 액세스할 수 있는 권한을 부여해야 합니다. 방법은 다음과 같습니다.

1. [Azure Portal](https://portal.azure.com)에 로그인합니다.
2. 위쪽 막대에서 계정을 클릭합니다. 그런 다음 **디렉터리** 목록에서 앱을 등록할 Active Directory 테넌트를 선택합니다.
3. 왼쪽 창에서 **모든 서비스**를 클릭한 다음, **Azure Active Directory**를 선택합니다.
4. **앱 등록**을 클릭하고 **추가**를 선택합니다.
5. 프롬프트에 따라 **네이티브 클라이언트 응용 프로그램**을 만듭니다.
  * **이름**은 사용자에게 앱에 대해 설명합니다.
  * **리디렉션 URI**는 Azure AD가 토큰 응답을 반환하는 데 사용하는 구성표 및 문자열의 조합입니다. 이제 자리 표시자 값(예: **http://DirectorySearcher**)을 입력합니다. 이 값은 나중에 바꿀 것입니다.
6. 등록이 완료되면 Azure AD가 앱에 고유한 응용 프로그램 ID를 할당합니다. **응용 프로그램** 탭에서 이 값을 복사해 둡니다. 나중에 이 값이 필요합니다.
7. **설정** 페이지에서 **필요한 사용 권한**, **추가**를 차례로 선택합니다.
8. **Azure Active Directory** 앱의 경우 API로 **Microsoft Graph**를 선택합니다.
9. **위임된 권한**에서 **로그인한 사용자로 디렉터리 액세스** 권한을 추가합니다. 이렇게 하면 앱에서 사용자의 Graph API를 쿼리할 수 있습니다.

## <a name="step-2-install-and-configure-adal"></a>2단계: ADAL 설치 및 구성
Azure AD에 앱이 있으므로 ADAL을 설치하고 ID 관련 코드를 작성할 수 있습니다. ADAL이 Azure AD와 통신할 수 있도록 앱 등록에 대한 정보를 제공합니다.

1. 패키지 관리자 콘솔을 사용하여 ADAL을 DirectorySearcher 프로젝트에 추가합니다.

    ```
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

2. DirectorySearcher 프로젝트에서 MainPage.xaml.cs를 엽니다.
3. **구성 값** 영역의 값을 Azure Portal에서 입력한 값으로 대체합니다. 코드에서 ADAL을 사용할 때마다 이러한 값을 참조합니다.
  * *테넌트*는 Azure AD 테넌트의 도메인(예: contoso.onmicrosoft.com)입니다.
  * *clientId*는 포털에서 복사한 앱의 클라이언트 ID입니다.
4. 이제 Windows 스토어 앱의 콜백 URI를 검색해야 합니다. 이 줄의 `MainPage` 메서드에 중단점을 설정합니다.
    ```
    redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
    ```
5. 솔루션을 빌드하고 모든 패키지 참조가 복원되었는지 확인합니다. 일부 패키지가 누락된 경우 NuGet 패키지 관리자를 열고 누락된 패키지를 복원합니다.
6. 앱을 실행하고, 중단점을 만나면 `redirectUri` 값을 복사해 둡니다. 이 값은 다음과 비슷한 형식입니다.

    ```
    ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
    ```

7. Azure Portal에서 앱의 **설정** 탭으로 돌아가서 **RedirectUri**를 이전 값으로 추가합니다.  

## <a name="step-3-use-adal-to-get-tokens-from-azure-ad"></a>3단계: ADAL을 사용하여 Azure AD에서 토큰 가져오기
ADAL에서 확인되는 기본 원칙은 액세스 토큰이 필요할 때마다 앱이 `authContext.AcquireToken(…)`을 호출하고 나머지 작업은 ADAL이 수행한다는 것입니다.  

1. ADAL의 기본 클래스인 앱의 `AuthenticationContext`를 초기화합니다. 이 작업은 Azure AD와 통신하는 데 필요한 좌표를 ADAL에 전달하고 토큰 캐시 방법을 알려줍니다.

    ```csharp
    public MainPage()
    {
        ...

        authContext = new AuthenticationContext(authority);
    }
    ```

2. `Search(...)` 메서드를 찾습니다. 이 메서드는 사용자가 앱의 UI에서 **검색** 단추를 클릭할 때 호출됩니다. 이 메서드는 Azure AD Graph API에 해당 UPN이 지정된 검색어로 시작하는 사용자를 쿼리하라는 get 요청을 만듭니다. Graph API를 쿼리하려면 요청의 **Authorization** 헤더에 액세스 토큰을 포함합니다. 여기로 ADAL이 들어옵니다.

    ```csharp
    private async void Search(object sender, RoutedEventArgs e)
    {
        ...
        AuthenticationResult result = null;
        try
        {
            result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectURI, new PlatformParameters(PromptBehavior.Auto, false));
        }
        catch (AdalException ex)
        {
            if (ex.ErrorCode != "authentication_canceled")
            {
                ShowAuthError(string.Format("If the error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", ex.ErrorCode, ex.Message));
            }
            return;
        }
        ...
    }
    ```
    앱에서 `AcquireTokenAsync(...)`을 호출하여 토큰을 요청하면 ADAL은 사용자에게 자격 증명을 요구하지 않고 토큰을 반환하려고 시도합니다. ADAL은 사용자가 토큰을 가져오기 위해 로그인해야 한다고 판단할 경우 로그인 대화 상자를 표시하고, 사용자의 자격 증명을 수집하고, 인증 성공 시 토큰을 반환합니다. 어떤 이유로든 ADAL이 토큰을 반환할 수 없는 경우 *AuthenticationResult* 상태는 오류가 됩니다.
3. 이제 방금 획득한 액세스 토큰을 사용해 보겠습니다. 또한 `Search(...)` 메서드에서 **Authorization** 헤더의 Graph API get 요청에 이 토큰을 연결합니다.

    ```csharp
    // Add the access token to the Authorization header of the call to the Graph API, and call the Graph API.
    httpClient.DefaultRequestHeaders.Authorization = new HttpCredentialsHeaderValue("Bearer", result.AccessToken);

    ```
4. `AuthenticationResult` 개체를 사용하여 앱에 사용자에 대한 정보(예: 사용자 ID)를 표시할 수 있습니다.

    ```csharp
    // Update the page UI to represent the signed-in user
    ActiveUser.Text = result.UserInfo.DisplayableId;
    ```
5. ADAL을 사용하여 앱에서 사용자를 로그아웃할 수도 있습니다. 사용자가 **로그아웃** 단추를 클릭하면 다음 `AcquireTokenAsync(...)` 호출이 로그인 보기를 표시하도록 합니다. ADAL을 사용하면 이 작업은 토큰 캐시를 지우는 것 만큼 쉽습니다.

    ```csharp
    private void SignOut()
    {
        // Clear session state from the token cache.
        authContext.TokenCache.Clear();

        ...
    }
    ```

## <a name="whats-next"></a>다음 단계
이제 사용자를 인증하고 OAuth 2.0을 사용하여 Web API를 안전하게 호출하고, 사용자에 대한 기본 정보를 가져올 수 있는 Windows 스토어 앱이 작동하고 있습니다.

아직 사용자로 테넌트를 채우지 않은 경우 지금 채울 수 있습니다.
1. DirectorySearcher 앱을 실행하고 해당 사용자 중 하나로 로그인합니다.
2. 해당 UPN에 따라 다른 사용자를 검색합니다.
3. 앱을 닫았다가 다시 실행합니다. 사용자의 세션을 그대로 유지하는 방법을 알아두세요.
4. 마우스 오른쪽 단추를 클릭하고 아래쪽 막대를 표시하여 로그아웃했다가 다른 사용자로 다시 로그인합니다.

ADAL은 이 모든 일반적인 ID 기능을 앱에 쉽게 통합할 수 있습니다. 또한 캐시 관리, OAuth 프로토콜 지원, 사용자에게 로그인 UI 제공, 만료된 토큰 새로 고침 등의 모든 귀찮은 작업을 대신 처리합니다. 사용자는 단일 API 호출 `authContext.AcquireToken*(…)`만 알고 있으면 됩니다.

참조용 자료로 [완성된 샘플](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip)(사용자 구성 값 제외)을 다운로드하세요.

이제 추가 ID 시나리오로 이동할 수 있습니다. 예를 들어 [Azure AD를 사용하여 .NET Web API 보안 유지](active-directory-devquickstarts-webapi-dotnet.md)를 시도해 보세요.

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
