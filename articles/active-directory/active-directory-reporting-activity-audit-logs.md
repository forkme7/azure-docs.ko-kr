---
title: Azure Active Directory 포털의 감사 작업 보고서 | Microsoft Docs
description: Azure Active Directory 포털의 감사 작업 보고서 소개
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: a1f93126-77d1-4345-ab7d-561066041161
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/31/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 97ea32a1e0f8815accff6201251771ab8c088859
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/03/2018
---
# <a name="audit-activity-reports-in-the-azure-active-directory-portal"></a>Azure Active Directory 포털의 감사 작업 보고서 

Azure Active Directory(Azure AD)에서 보고를 통해 사용자 환경의 작동 방법을 결정하는 데 필요한 모든 정보를 얻을 수 있습니다.

Azure AD의 보고 아키텍처는 다음 구성 요소로 구성됩니다.

- **활동** 
    - **로그인 활동** – 관리되는 응용 프로그램 및 사용자 로그인 활동의 사용량에 대한 정보
    - **감사 로그** - 사용자 및 그룹 관리, 관리되는 응용 프로그램 및 디렉터리 활동에 대한 시스템 작업 정보
- **보안** 
    - **위험한 로그인** - 위험한 로그인은 사용자 계정의 정당한 소유자가 아닌 사용자에 의해 수행된 로그인 시도에 대한 지표입니다. 자세한 내용은 위험한 로그인을 참조하세요.
    - **위험 플래그가 지정된 사용자** - 위험한 사용자는 손상되었을 수 있는 사용자 계정에 대한 표시기입니다. 자세한 내용은 위험 플래그가 지정된 사용자를 참조하세요.

이 항목에서는 감사 활동에 대한 개요를 제공합니다.
 
## <a name="who-can-access-the-data"></a>데이터에 액세스할 수 있는 사용자는 누구인가요?
* 보안 관리 또는 보안 판독기 역할의 사용자
* 전역 관리자
* 개별 사용자(비관리자)는 자신의 활동을 볼 수 있습니다.


## <a name="audit-logs"></a>감사 로그

Azure Active Directory의 감사 로그는 규정 준수를 위한 시스템 활동의 기록을 제공합니다.  
모든 감사 데이터에 대한 첫 번째 진입점은 **Azure Active Directory**의 **활동** 섹션에 있는 **감사 로그**입니다.

![감사 로그](./media/active-directory-reporting-activity-audit-logs/61.png "감사 로그")

감사 로그에는 다음 항목을 보여 주는 기본 목록 보기가 있습니다.

- 발생 날짜와 시간
- 활동의 초기자/행위자(*누가*) 
- 활동(*무엇*) 
- 대상

![감사 로그](./media/active-directory-reporting-activity-audit-logs/18.png "감사 로그")

도구 모음에서 **열**을 클릭하여 목록 보기를 사용자 지정할 수 있습니다.

![감사 로그](./media/active-directory-reporting-activity-audit-logs/19.png "감사 로그")

그러면 추가 필드를 표시하거나 이미 표시된 필드를 제거할 수 있습니다.

![감사 로그](./media/active-directory-reporting-activity-audit-logs/21.png "감사 로그")


목록 보기에서 항목을 클릭하면 사용할 수 있는 세부 정보를 모두 얻을 수 있습니다.

![감사 로그](./media/active-directory-reporting-activity-audit-logs/22.png "감사 로그")


## <a name="filtering-audit-logs"></a>감사 로그 필터링

보고된 데이터를 자신에게 적합한 수준으로 좁히려면 다음 필드를 사용하여 감사 데이터를 필터링할 수 있습니다.

- 날짜 범위
- 초기자(작업자)
- Category
- 활동 리소스 종류
- 작업

![감사 로그](./media/active-directory-reporting-activity-audit-logs/23.png "감사 로그")


**날짜 범위** 필터를 사용하면 반환되는 데이터의 시간 범위를 정의할 수 있습니다.  
가능한 값은 다음과 같습니다.

- 1개월
- 7 일
- 24시간
- 사용자 지정

사용자 지정 시간 범위를 선택하면 시작 시간과 종료 시간을 구성할 수 있습니다.

**초기자** 필터를 사용하면 작업자 이름이나 UPN(사용자 계정 이름)을 정의할 수 있습니다.

**범주** 필터를 사용하면 다음 필터 중 하나를 선택할 수 있습니다.

- 모두
- 핵심 범주
- 핵심 디렉터리
- 셀프 서비스 암호 관리
- 셀프 서비스 그룹 관리
- 계정 프로비전 - 자동화된 암호 롤오버
- 사용자 초대
- MIM 서비스
- ID 보호
- B2C

**활동 리소스 종류** 필터를 사용하면 다음 필터 중 하나를 선택할 수 있습니다.

- 모두 
- 그룹
- 디렉터리
- 사용자
- 응용 프로그램
- 정책
- 장치
- 기타

**그룹**을 **활동 리소스 종류**로 선택하면 다음과 같은 **원본**도 제공할 수 있는 추가 필터 범주가 생깁니다.

- Azure AD
- O365


**활동** 필터는 사용자가 선택한 범주와 활동 리소스 종류를 기반으로 합니다. 보려는 특정 활동을 선택하거나 모두 선택할 수 있습니다. 

Graph API https://graph.windows.net/$tenantdomain/activities/auditActivityTypes?api-version=beta(여기서 $tenantdomain = 도메인 이름)를 사용하여 모든 감사 작업 목록을 가져오거나 [감사 보고서 이벤트](active-directory-reporting-audit-events.md) 문서를 참조할 수 있습니다.


## <a name="audit-logs-shortcuts"></a>감사 로그 바로 가기

**Azure Active Directory** 외에도 Azure Portal에서는 감사 데이터에 대한 다음 두 개의 추가 진입점을 제공합니다.

- 개요
- Enterprise 응용 프로그램

### <a name="users-and-groups-audit-logs"></a>사용자 및 그룹 감사 로그

사용자 및 그룹 기반 감사 보고서를 사용하여 다음과 같은 질문에 대한 답을 얻을 수 있습니다.

- 어떤 종류의 업데이트가 사용자에게 적용되나요?

- 얼마나 많은 사용자가 변경되었나요?

- 얼마나 많은 암호가 변경되었나요?

- 관리자가 디렉터리에서 무엇을 수행했나요?

- 추가된 그룹은 무엇인가요?

- 멤버 자격이 변경된 그룹이 있나요?

- 그룹의 소유자가 변경되었나요?

- 어떤 라이선스가 그룹 또는 사용자에 할당되었나요?

사용자 및 그룹과 관련된 감사 데이터를 검토하려면 **사용자 및 그룹**의 **활동** 섹션에 있는 **감사 로그**에서 필터링된 보기를 찾을 수 있습니다. 이 진입점에는 미리 선택된 **활동 리소스 종류**로 **사용자 및 그룹**이 있습니다.

![감사 로그](./media/active-directory-reporting-activity-audit-logs/93.png "감사 로그")

### <a name="enterprise-applications-audit-logs"></a>Enterprise 응용 프로그램 감사 로그

응용 프로그램 기반 감사 보고서를 사용하여 다음과 같은 질문에 대한 답을 얻을 수 있습니다.

* 추가되거나 업데이트된 응용 프로그램은 무엇인가요?
* 삭제된 응용 프로그램은 무엇인가요?
* 응용 프로그램에 대한 서비스 원칙이 변경되었나요?
* 응용 프로그램의 이름이 변경되었나요?
* 누가 응용 프로그램에 동의했나요?

응용 프로그램과 관련된 감사 데이터를 검토하려면 **Enterprise 응용 프로그램** 블레이드의 **활동** 섹션에 있는 **감사 로그**에서 필터링된 보기를 찾을 수 있습니다. 이 진입점에는 미리 선택된 **활동 리소스 종류**인 **엔터프라이즈 응용 프로그램**이 있습니다.

![감사 로그](./media/active-directory-reporting-activity-audit-logs/134.png "감사 로그")

이 보기는 **그룹** 또는 **사용자**로 더 자세히 필터링할 수 있습니다.

![감사 로그](./media/active-directory-reporting-activity-audit-logs/25.png "감사 로그")



## <a name="azure-ad-audit-activity-list"></a>Azure AD 감사 활동 목록

이 섹션에서는 기록 가능한 모든 활동 목록을 제공합니다. 


|서비스 이름|감사 범주|활동 리소스 종류|활동|
|---|---|---|---|
|계정 프로비전|응용 프로그램 관리|응용 프로그램|관리|
|계정 프로비전|응용 프로그램 관리|응용 프로그램|디렉터리 작업|
|계정 프로비전|응용 프로그램 관리|응용 프로그램|내보내기|
|계정 프로비전|응용 프로그램 관리|응용 프로그램|가져오기|
|계정 프로비전|응용 프로그램 관리|응용 프로그램|기타|
|계정 프로비전|응용 프로그램 관리|응용 프로그램|프로세스 위탁|
|계정 프로비전|응용 프로그램 관리|응용 프로그램|동기화 규칙 작업|
|응용 프로그램 프록시|응용 프로그램 관리|응용 프로그램|응용 프로그램 추가|
|응용 프로그램 프록시|리소스|리소스|응용 프로그램 SSL 인증서 추가|
|응용 프로그램 프록시|리소스|리소스|SSL 바인딩 삭제|
|응용 프로그램 프록시|응용 프로그램 관리|응용 프로그램|응용 프로그램 삭제|
|응용 프로그램 프록시|디렉터리 관리|디렉터리|데스크톱 Sso 사용 안 함|
|응용 프로그램 프록시|디렉터리 관리|디렉터리|특정 도메인에 데스크톱 Sso 사용 안 함|
|응용 프로그램 프록시|디렉터리 관리|디렉터리|응용 프로그램 프록시 사용 안 함|
|응용 프로그램 프록시|디렉터리 관리|디렉터리|통과 인증 사용 안 함|
|응용 프로그램 프록시|디렉터리 관리|디렉터리|데스크톱 Sso 사용|
|응용 프로그램 프록시|디렉터리 관리|디렉터리|특정 도메인에 데스크톱 Sso 사용|
|응용 프로그램 프록시|디렉터리 관리|디렉터리|응용 프로그램 프록시 사용|
|응용 프로그램 프록시|디렉터리 관리|디렉터리|통과 인증 사용|
|응용 프로그램 프록시|리소스|리소스|커넥터 등록|
|응용 프로그램 프록시|응용 프로그램 관리|응용 프로그램|응용 프로그램 업데이트|
|응용 프로그램 프록시|응용 프로그램 관리|응용 프로그램|응용 프로그램 Single Sign-On 모드 업데이트|
|자동화된 암호 롤오버|응용 프로그램 관리|응용 프로그램|자동화된 암호 롤오버|
|B2C|응용 프로그램 관리|응용 프로그램|V2 응용 프로그램 권한 추가|
|B2C|권한 부여|권한 부여|V2 응용 프로그램 권한 추가|
|B2C|키|키|ASCII 비밀에 따라 CPIM 키 컨테이너에 키 추가|
|B2C|권한 부여|권한 부여|ASCII 비밀에 따라 CPIM 키 컨테이너에 키 추가|
|B2C|키|키|CPIM 키 컨테이너에 키 추가|
|B2C|권한 부여|권한 부여|CPIM 키 컨테이너에 키 추가|
|B2C|리소스|리소스|AdminPolicyDatas-RemoveResources|
|B2C|권한 부여|권한 부여|AdminPolicyDatas-SetResources|
|B2C|리소스|리소스|AdminPolicyDatas-SetResources|
|B2C|리소스|리소스|AdminUserJourneys-GetResources|
|B2C|권한 부여|권한 부여|AdminUserJourneys-GetResources|
|B2C|권한 부여|권한 부여|AdminUserJourneys-RemoveResources|
|B2C|리소스|리소스|AdminUserJourneys-RemoveResources|
|B2C|리소스|리소스|AdminUserJourneys-SetResources|
|B2C|권한 부여|권한 부여|AdminUserJourneys-SetResources|
|B2C|권한 부여|권한 부여|IdentityProvider 만들기|
|B2C|리소스|리소스|IdentityProvider 만들기|
|B2C|권한 부여|권한 부여|V1 응용 프로그램 만들기|
|B2C|응용 프로그램 관리|응용 프로그램|V1 응용 프로그램 만들기|
|B2C|응용 프로그램 관리|응용 프로그램|V2 응용 프로그램 만들기|
|B2C|권한 부여|권한 부여|V2 응용 프로그램 만들기|
|B2C|디렉터리 관리|디렉터리|테넌트에 사용자 지정 도메인 만들기|
|B2C|권한 부여|권한 부여|테넌트에 사용자 지정 도메인 만들기|
|B2C|권한 부여|권한 부여|새 AdminUserJourney 만들기|
|B2C|리소스|리소스|새 AdminUserJourney 만들기|
|B2C|리소스|리소스|지역화된 리소스 json 만들기|
|B2C|권한 부여|권한 부여|지역화된 리소스 json 만들기|
|B2C|권한 부여|권한 부여|새 사용자 지정 IDP 만들기|
|B2C|리소스|리소스|새 사용자 지정 IDP 만들기|
|B2C|리소스|리소스|새 IDP 만들기|
|B2C|권한 부여|권한 부여|새 IDP 만들기|
|B2C|권한 부여|권한 부여|B2C 디렉터리 리소스 만들기 또는 업데이트|
|B2C|리소스|리소스|B2C 디렉터리 리소스 만들기 또는 업데이트|
|B2C|리소스|리소스|정책 만들기|
|B2C|권한 부여|권한 부여|정책 만들기|
|B2C|권한 부여|권한 부여|trustFramework 정책 만들기|
|B2C|리소스|리소스|trustFramework 정책 만들기|
|B2C|권한 부여|권한 부여|구성 가능한 접두사로 trustFramework 정책 만들기|
|B2C|리소스|리소스|구성 가능한 접두사로 trustFramework 정책 만들기|
|B2C|리소스|리소스|사용자 특성 만들기|
|B2C|권한 부여|권한 부여|사용자 특성 만들기|
|B2C|권한 부여|권한 부여|CreateTrustFrameworkPolicy|
|B2C|리소스|리소스|CreateTrustFrameworkPolicy|
|B2C|권한 부여|권한 부여|새 AdminUserJourney 만들기 또는 업데이트|
|B2C|리소스|리소스|IDP 삭제|
|B2C|권한 부여|권한 부여|IDP 삭제|
|B2C|리소스|리소스|IdentityProvider 삭제|
|B2C|권한 부여|권한 부여|IdentityProvider 삭제|
|B2C|응용 프로그램 관리|응용 프로그램|V1 응용 프로그램 삭제|
|B2C|권한 부여|권한 부여|V1 응용 프로그램 삭제|
|B2C|응용 프로그램 관리|응용 프로그램|V2 응용 프로그램 삭제|
|B2C|권한 부여|권한 부여|V2 응용 프로그램 삭제|
|B2C|권한 부여|권한 부여|V2 응용 프로그램 권한 부여 삭제|
|B2C|응용 프로그램 관리|응용 프로그램|V2 응용 프로그램 권한 부여 삭제|
|B2C|리소스|리소스|B2C 디렉터리 리소스 삭제|
|B2C|권한 부여|권한 부여|B2C 디렉터리 리소스 삭제|
|B2C|키|키|CPIM 키 컨테이너 삭제|
|B2C|권한 부여|권한 부여|CPIM 키 컨테이너 삭제|
|B2C|키|키|키 컨테이너 삭제|
|B2C|리소스|리소스|trustFramework 정책 삭제|
|B2C|권한 부여|권한 부여|trustFramework 정책 삭제|
|B2C|리소스|리소스|사용자 특성 삭제|
|B2C|권한 부여|권한 부여|사용자 특성 삭제|
|B2C|권한 부여|권한 부여|B2C 기능 사용|
|B2C|디렉터리 관리|디렉터리|B2C 기능 사용|
|B2C|리소스|리소스|리소스 그룹에서 B2C 디렉터리 리소스 가져오기|
|B2C|리소스|리소스|구독에서 B2C 디렉터리 리소스 가져오기|
|B2C|권한 부여|권한 부여|구독에서 B2C 디렉터리 리소스 가져오기|
|B2C|권한 부여|권한 부여|사용자 지정 IDP 가져오기|
|B2C|리소스|리소스|사용자 지정 IDP 가져오기|
|B2C|리소스|리소스|IDP 가져오기|
|B2C|권한 부여|권한 부여|IDP 가져오기|
|B2C|권한 부여|권한 부여|V1 및 V2 응용 프로그램 가져오기|
|B2C|응용 프로그램 관리|응용 프로그램|V1 및 V2 응용 프로그램 가져오기|
|B2C|권한 부여|권한 부여|V1 응용 프로그램 가져오기|
|B2C|응용 프로그램 관리|응용 프로그램|V1 응용 프로그램 가져오기|
|B2C|권한 부여|권한 부여|V1 응용 프로그램 가져오기|
|B2C|응용 프로그램 관리|응용 프로그램|V1 응용 프로그램 가져오기|
|B2C|응용 프로그램 관리|응용 프로그램|V2 응용 프로그램 가져오기|
|B2C|권한 부여|권한 부여|V2 응용 프로그램 가져오기|
|B2C|응용 프로그램 관리|응용 프로그램|V2 응용 프로그램 가져오기|
|B2C|권한 부여|권한 부여|V2 응용 프로그램 가져오기|
|B2C|리소스|리소스|B2C drectory 리소스 가져오기|
|B2C|권한 부여|권한 부여|B2C drectory 리소스 가져오기|
|B2C|권한 부여|권한 부여|테넌트의 사용자 지정 도메인 목록 가져오기|
|B2C|디렉터리 관리|디렉터리|테넌트의 사용자 지정 도메인 목록 가져오기|
|B2C|권한 부여|권한 부여|사용자 경험 가져오기|
|B2C|리소스|리소스|사용자 경험 가져오기|
|B2C|리소스|리소스|사용자 경험에 허용된 응용 프로그램 클레임 가져오기|
|B2C|권한 부여|권한 부여|사용자 경험에 허용된 응용 프로그램 클레임 가져오기|
|B2C|리소스|리소스|사용자 경험에 허용된 자체 어설션된 클레임 가져오기|
|B2C|권한 부여|권한 부여|사용자 경험에 허용된 자체 어설션된 클레임 가져오기|
|B2C|리소스|리소스|허용된 자체 어설션된 정책 클레임 가져오기|
|B2C|권한 부여|권한 부여|허용된 자체 어설션된 정책 클레임 가져오기|
|B2C|리소스|리소스|사용 가능한 출력 클레임 목록 가져오기|
|B2C|권한 부여|권한 부여|사용 가능한 출력 클레임 목록 가져오기|
|B2C|리소스|리소스|사용자 경험에 대한 콘텐츠 정의 가져오기|
|B2C|권한 부여|권한 부여|사용자 경험에 대한 콘텐츠 정의 가져오기|
|B2C|권한 부여|권한 부여|특정 관리 흐름에 대한 idp 가져오기|
|B2C|리소스|리소스|특정 관리 흐름에 대한 idp 가져오기|
|B2C|키|키|JWK의 키 컨테이너 활성 키 메타데이터 가져오기|
|B2C|권한 부여|권한 부여|JWK의 키 컨테이너 활성 키 메타데이터 가져오기|
|B2C|키|키|키 컨테이너 메타데이터 가져오기|
|B2C|리소스|리소스|모든 관리 흐름 목록 가져오기|
|B2C|권한 부여|권한 부여|모든 관리 흐름 목록 가져오기|
|B2C|권한 부여|권한 부여|모든 사용자의 모든 관리 흐름에 대한 태그 목록 가져오기|
|B2C|리소스|리소스|모든 사용자의 모든 관리 흐름에 대한 태그 목록 가져오기|
|B2C|리소스|리소스|사용자의 테넌트 목록 가져오기|
|B2C|권한 부여|권한 부여|사용자의 테넌트 목록 가져오기|
|B2C|리소스|리소스|로컬 계정의 자체 어설션된 클레임 가져오기|
|B2C|권한 부여|권한 부여|로컬 계정의 자체 어설션된 클레임 가져오기|
|B2C|리소스|리소스|지역화된 리소스 json 가져오기|
|B2C|권한 부여|권한 부여|지역화된 리소스 json 가져오기|
|B2C|권한 부여|권한 부여|Microsoft.AzureActiveDirectory 리소스 공급자의 작업 가져오기|
|B2C|리소스|리소스|Microsoft.AzureActiveDirectory 리소스 공급자의 작업 가져오기|
|B2C|리소스|리소스|정책 가져오기|
|B2C|권한 부여|권한 부여|정책 가져오기|
|B2C|리소스|리소스|정책 가져오기|
|B2C|권한 부여|권한 부여|정책 가져오기|
|B2C|디렉터리 관리|디렉터리|테넌트의 리소스 속성 가져오기|
|B2C|권한 부여|권한 부여|테넌트의 리소스 속성 가져오기|
|B2C|리소스|리소스|지원되는 IDP 목록 가져오기|
|B2C|권한 부여|권한 부여|지원되는 IDP 목록 가져오기|
|B2C|리소스|리소스|지원되는 사용자 경험 IDP 목록 가져오기|
|B2C|권한 부여|권한 부여|지원되는 사용자 경험 IDP 목록 가져오기|
|B2C|디렉터리 관리|디렉터리|테넌트 정보 가져오기|
|B2C|권한 부여|권한 부여|테넌트 정보 가져오기|
|B2C|디렉터리 관리|디렉터리|테넌트에서 허용하는 기능 가져오기|
|B2C|권한 부여|권한 부여|테넌트에서 허용하는 기능 가져오기|
|B2C|권한 부여|권한 부여|테넌트에서 정의한 사용자 지정 IDP 목록 가져오기|
|B2C|리소스|리소스|테넌트에서 정의한 사용자 지정 IDP 목록 가져오기|
|B2C|리소스|리소스|테넌트에서 정의한 IDP 목록 가져오기|
|B2C|권한 부여|권한 부여|테넌트에서 정의한 IDP 목록 가져오기|
|B2C|리소스|리소스|테넌트에서 정의한 로컬 IDP 목록 가져오기|
|B2C|권한 부여|권한 부여|테넌트에서 정의한 로컬 IDP 목록 가져오기|
|B2C|리소스|리소스|리소스 만들기에 대한 사용자의 테넌트 세부 정보 가져오기|
|B2C|권한 부여|권한 부여|리소스 만들기에 대한 사용자의 테넌트 세부 정보 가져오기|
|B2C|권한 부여|권한 부여|테넌트 목록 가져오기|
|B2C|권한 부여|권한 부여|TenantDomains 가져오기|
|B2C|디렉터리 관리|디렉터리|TenantDomains 가져오기|
|B2C|리소스|리소스|CPIM에 지원되는 기본 문화권 가져오기|
|B2C|권한 부여|권한 부여|CPIM에 지원되는 기본 문화권 가져오기|
|B2C|리소스|리소스|관리 흐름 세부 정보 가져오기|
|B2C|권한 부여|권한 부여|관리 흐름 세부 정보 가져오기|
|B2C|권한 부여|권한 부여|이 테넌트의 UserJourneys 목록 가져오기|
|B2C|리소스|리소스|이 테넌트의 UserJourneys 목록 가져오기|
|B2C|권한 부여|권한 부여|CPIM에 지원되는 문화권 집합 가져오기|
|B2C|리소스|리소스|CPIM에 지원되는 문화권 집합 가져오기|
|B2C|권한 부여|권한 부여|trustFramework 정책 가져오기|
|B2C|리소스|리소스|trustFramework 정책 가져오기|
|B2C|권한 부여|권한 부여|trustFramework 정책을 xml로 가져오기|
|B2C|리소스|리소스|trustFramework 정책을 xml로 가져오기|
|B2C|리소스|리소스|사용자 특성 가져오기|
|B2C|권한 부여|권한 부여|사용자 특성 가져오기|
|B2C|권한 부여|권한 부여|사용자 특성 가져오기|
|B2C|리소스|리소스|사용자 특성 가져오기|
|B2C|권한 부여|권한 부여|사용자 경험 목록 가져오기|
|B2C|리소스|리소스|사용자 경험 목록 가져오기|
|B2C|권한 부여|권한 부여|GetIEFPolicies|
|B2C|리소스|리소스|GetIEFPolicies|
|B2C|권한 부여|권한 부여|GetIdentityProviders|
|B2C|리소스|리소스|GetIdentityProviders|
|B2C|리소스|리소스|GetTrustFrameworkPolicy|
|B2C|권한 부여|권한 부여|GetTrustFrameworkPolicy|
|B2C|키|키|jwk 형식으로 CPIM 키 컨테이너 가져오기|
|B2C|권한 부여|권한 부여|jwk 형식으로 CPIM 키 컨테이너 가져오기|
|B2C|키|키|테넌트의 키 컨테이너 목록을 가져오기|
|B2C|권한 부여|권한 부여|테넌트의 키 컨테이너 목록을 가져오기|
|B2C|권한 부여|권한 부여|테넌트 형식 가져오기|
|B2C|디렉터리 관리|디렉터리|테넌트 형식 가져오기|
|B2C|인증|인증|응용 프로그램에 액세스 토큰 발급|
|B2C|인증|인증|응용 프로그램에 권한 부여 코드 발급|
|B2C|기타|기타|응용 프로그램에 권한 부여 코드 발급|
|B2C|인증|인증|응용 프로그램에 id_token 발급|
|B2C|기타|기타|응용 프로그램에 id_token 발급|
|B2C|권한 부여|권한 부여|MigrateTenantMetadata|
|B2C|리소스|리소스|MigrateTenantMetadata|
|B2C|리소스|리소스|리소스 이동|
|B2C|권한 부여|권한 부여|IdentityProvider 패치|
|B2C|리소스|리소스|IdentityProvider 패치|
|B2C|리소스|리소스|PutTrustFrameworkPolicy|
|B2C|권한 부여|권한 부여|PutTrustFrameworkPolicy|
|B2C|권한 부여|권한 부여|PutTrustFrameworkpolicy|
|B2C|리소스|리소스|PutTrustFrameworkpolicy|
|B2C|리소스|리소스|사용자 경험 제거|
|B2C|권한 부여|권한 부여|사용자 경험 제거|
|B2C|권한 부여|권한 부여|CPIM 키 컨테이너 백업 복원|
|B2C|키|키|CPIM 키 컨테이너 백업 복원|
|B2C|응용 프로그램 관리|응용 프로그램|V2 응용 프로그램 권한 부여 검색|
|B2C|권한 부여|권한 부여|V2 응용 프로그램 권한 부여 검색|
|B2C|응용 프로그램 관리|응용 프로그램|현재 테넌트의 V2 응용 프로그램 서비스 사용자 검색|
|B2C|권한 부여|권한 부여|현재 테넌트의 V2 응용 프로그램 서비스 사용자 검색|
|B2C|키|키|키 컨테이너 저장|
|B2C|권한 부여|권한 부여|사용자 지정 IDP 업데이트|
|B2C|리소스|리소스|사용자 지정 IDP 업데이트|
|B2C|리소스|리소스|IDP 업데이트|
|B2C|권한 부여|권한 부여|IDP 업데이트|
|B2C|리소스|리소스|로컬 IDP 업데이트|
|B2C|권한 부여|권한 부여|로컬 IDP 업데이트|
|B2C|응용 프로그램 관리|응용 프로그램|V1 응용 프로그램 업데이트|
|B2C|권한 부여|권한 부여|V1 응용 프로그램 업데이트|
|B2C|응용 프로그램 관리|응용 프로그램|V2 응용 프로그램 업데이트|
|B2C|권한 부여|권한 부여|V2 응용 프로그램 업데이트|
|B2C|응용 프로그램 관리|응용 프로그램|V2 응용 프로그램 권한 부여 업데이트|
|B2C|권한 부여|권한 부여|V2 응용 프로그램 권한 부여 업데이트|
|B2C|리소스|리소스|B2C 디렉터리 리소스 업데이트|
|B2C|리소스|리소스|정책 업데이트|
|B2C|권한 부여|권한 부여|정책 업데이트|
|B2C|리소스|리소스|구독 상태 업데이트|
|B2C|리소스|리소스|사용자 특성 업데이트|
|B2C|권한 부여|권한 부여|사용자 특성 업데이트|
|B2C|키|키|CPIM 암호화 키 업로드|
|B2C|권한 부여|권한 부여|CPIM 암호화 키 업로드|
|B2C|권한 부여|권한 부여|사용자 권한 부여: 테넌트 기능 집합에 API를 사용하지 않음|
|B2C|권한 부여|권한 부여|사용자 권한 부여: 사용자에게 '테넌트 관리자' 액세스 권한 부여|
|B2C|권한 부여|권한 부여|사용자 권한 부여: 사용자에게 '인증된 사용자' 액세스 권한 부여|
|B2C|인증|인증|로컬 계정 자격 증명 유효성 검사|
|B2C|리소스|리소스|리소스 이동 유효성 검사|
|B2C|인증|인증|사용자 인증 유효성 검사|
|B2C|디렉터리 관리|디렉터리|B2C 기능을 사용하도록 설정되었는지 확인|
|B2C|권한 부여|권한 부여|B2C 기능을 사용하도록 설정되었는지 확인|
|B2C|권한 부여|권한 부여|기능을 사용하도록 설정되었는지 확인|
|B2C|디렉터리 관리|디렉터리|기능을 사용하도록 설정되었는지 확인|
|핵심 디렉터리|응용 프로그램 관리|응용 프로그램|OAuth2PermissionGrant 추가|
|핵심 디렉터리|관리 단위 관리|AdministrativeUnit|관리 단위 추가|
|핵심 디렉터리|사용자 관리|사용자|사용자에게 앱 역할 할당 권한 부여 추가|
|핵심 디렉터리|그룹 관리|그룹|그룹에 앱 역할 할당 추가|
|핵심 디렉터리|응용 프로그램 관리|응용 프로그램|서비스 주체에 앱 역할 할당 추가|
|핵심 디렉터리|응용 프로그램 관리|응용 프로그램|응용 프로그램 추가|
|핵심 디렉터리|리소스|리소스|장치 추가|
|핵심 디렉터리|리소스|리소스|장치 구성 추가|
|핵심 디렉터리|역할 관리|역할|역할에 적격 멤버 추가|
|핵심 디렉터리|그룹 관리|그룹|그룹 추가|
|핵심 디렉터리|관리 단위 관리|AdministrativeUnit|관리 장치에 멤버 추가|
|핵심 디렉터리|그룹 관리|그룹|그룹에 멤버 추가|
|핵심 디렉터리|역할 관리|역할|역할에 멤버 추가|
|핵심 디렉터리|응용 프로그램 관리|응용 프로그램|응용 프로그램에 소유자 추가|
|핵심 디렉터리|그룹 관리|그룹|그룹에 소유자 추가|
|핵심 디렉터리|정책 관리|정책|정책에 소유자 추가|
|핵심 디렉터리|응용 프로그램 관리|응용 프로그램|서비스 주체에 소유자 추가|
|핵심 디렉터리|디렉터리 관리|디렉터리|회사에 파트너 추가|
|핵심 디렉터리|정책 관리|정책|정책 추가|
|핵심 디렉터리|응용 프로그램 관리|응용 프로그램|서비스 주체에 정책 추가|
|핵심 디렉터리|리소스|리소스|장치에 등록된 소유자 추가|
|핵심 디렉터리|리소스|리소스|장치에 등록된 사용자 추가|
|핵심 디렉터리|역할 관리|역할|역할 정의에 역할 할당 추가|
|핵심 디렉터리|역할 관리|역할|템플릿에서 역할 추가|
|핵심 디렉터리|역할 관리|역할|역할에 범위가 지정된 멤버 추가|
|핵심 디렉터리|응용 프로그램 관리|응용 프로그램|서비스 주체 추가|
|핵심 디렉터리|응용 프로그램 관리|응용 프로그램|서비스 주체 자격 증명 추가|
|핵심 디렉터리|디렉터리 관리|디렉터리|확인되지 않은 도메인 추가|
|핵심 디렉터리|사용자 관리|사용자|사용자 추가|
|핵심 디렉터리|사용자 관리|사용자|사용자 강력한 인증 전화 앱 세부 정보 추가|
|핵심 디렉터리|디렉터리 관리|디렉터리|확인된 도메인 추가|
|핵심 디렉터리|사용자 관리|사용자|사용자 라이선스 변경|
|핵심 디렉터리|사용자 관리|사용자|사용자 암호 변경|
|핵심 디렉터리|응용 프로그램 관리|응용 프로그램|응용 프로그램에 동의|
|핵심 디렉터리|사용자 관리|사용자|페더레이션 사용자를 관리되는 사용자로 변환|
|핵심 디렉터리|사용자 관리|사용자|사용자에 대한 응용 프로그램 암호 만들기|
|핵심 디렉터리|디렉터리 관리|디렉터리|회사 만들기|
|핵심 디렉터리|디렉터리 관리|디렉터리|회사 설정 만들기|
|핵심 디렉터리|그룹 관리|그룹|그룹 설정 만들기|
|핵심 디렉터리|관리 단위 관리|AdministrativeUnit|관리 장치 삭제|
|핵심 디렉터리|응용 프로그램 관리|응용 프로그램|응용 프로그램 삭제|
|핵심 디렉터리|사용자 관리|사용자|사용자에 대한 응용 프로그램 암호 삭제|
|핵심 디렉터리|디렉터리 관리|디렉터리|회사 설정 삭제|
|핵심 디렉터리|리소스|리소스|장치 삭제|
|핵심 디렉터리|리소스|리소스|장치 구성 삭제|
|핵심 디렉터리|그룹 관리|그룹|그룹 삭제|
|핵심 디렉터리|그룹 관리|그룹|그룹 설정 삭제|
|핵심 디렉터리|정책 관리|정책|정책 삭제|
|핵심 디렉터리|사용자 관리|사용자|사용자 삭제|
|핵심 디렉터리|디렉터리 관리|디렉터리|파트너 강등|
|핵심 디렉터리|리소스|리소스|장치가 더 이상 규정을 준수하지 않음|
|핵심 디렉터리|리소스|리소스|장치가 더 이상 관리되지 않음|
|핵심 디렉터리|디렉터리 관리|디렉터리|디렉터리 삭제됨|
|핵심 디렉터리|디렉터리 관리|디렉터리|디렉터리가 영구적으로 삭제됨|
|핵심 디렉터리|디렉터리 관리|디렉터리|디렉터리 삭제가 예약됨|
|핵심 디렉터리|사용자 관리|사용자|계정 사용 안 함|
|핵심 디렉터리|사용자 관리|사용자|강력한 인증 사용|
|핵심 디렉터리|그룹 관리|그룹|사용자에게 그룹 기반 라이선스 적용 완료|
|핵심 디렉터리|응용 프로그램 관리|응용 프로그램|응용 프로그램 영구 삭제|
|핵심 디렉터리|그룹 관리|그룹|그룹 영구 삭제|
|핵심 디렉터리|사용자 관리|사용자|사용자 영구 삭제|
|핵심 디렉터리|디렉터리 관리|디렉터리|회사를 파트너로 수준 올리기|
|핵심 디렉터리|디렉터리 관리|디렉터리|권한 관리 속성 제거|
|핵심 디렉터리|응용 프로그램 관리|응용 프로그램|OAuth2PermissionGrant 제거|
|핵심 디렉터리|그룹 관리|그룹|그룹에서 앱 역할 할당 제거|
|핵심 디렉터리|응용 프로그램 관리|응용 프로그램|서비스 주체에서 앱 역할 할당 제거|
|핵심 디렉터리|사용자 관리|사용자|사용자에게서 앱 역할 할당 제거|
|핵심 디렉터리|역할 관리|역할|역할에서 적격 멤버 제거|
|핵심 디렉터리|관리 단위 관리|AdministrativeUnit|관리 장치에서 멤버 제거|
|핵심 디렉터리|그룹 관리|그룹|그룹에서 멤버 제거|
|핵심 디렉터리|역할 관리|역할|역할에서 멤버 제거|
|핵심 디렉터리|응용 프로그램 관리|응용 프로그램|응용 프로그램에서 소유자 제거|
|핵심 디렉터리|그룹 관리|그룹|그룹에서 소유자 제거|
|핵심 디렉터리|응용 프로그램 관리|응용 프로그램|서비스 주체에서 소유자 제거|
|핵심 디렉터리|디렉터리 관리|디렉터리|회사에서 파트너 제거|
|핵심 디렉터리|정책 관리|정책|정책 자격 증명 제거|
|핵심 디렉터리|응용 프로그램 관리|응용 프로그램|서비스 주체에서 정책 제거|
|핵심 디렉터리|리소스|리소스|장치에서 등록된 소유자 제거|
|핵심 디렉터리|리소스|리소스|장치에서 등록된 사용자 제거|
|핵심 디렉터리|역할 관리|역할|역할 정의에서 역할 할당 제거|
|핵심 디렉터리|역할 관리|역할|역할에서 범위가 지정된 멤버 제거|
|핵심 디렉터리|응용 프로그램 관리|응용 프로그램|서비스 주체 제거|
|핵심 디렉터리|응용 프로그램 관리|응용 프로그램|서비스 주체 자격 증명 제거|
|핵심 디렉터리|디렉터리 관리|디렉터리|확인되지 않은 도메인 제거|
|핵심 디렉터리|사용자 관리|사용자|사용자 강력한 인증 전화 앱 세부 정보 제거|
|핵심 디렉터리|디렉터리 관리|디렉터리|확인된 도메인 제거|
|핵심 디렉터리|사용자 관리|사용자|사용자 암호 재설정|
|핵심 디렉터리|그룹 관리|그룹|그룹 복원|
|핵심 디렉터리|응용 프로그램 관리|응용 프로그램|응용 프로그램 복원|
|핵심 디렉터리|사용자 관리|사용자|사용자 복원|
|핵심 디렉터리|응용 프로그램 관리|응용 프로그램|동의 철회|
|핵심 디렉터리|디렉터리 관리|디렉터리|회사 정보 설정|
|핵심 디렉터리|디렉터리 관리|디렉터리|DirSync 기능 설정|
|핵심 디렉터리|디렉터리 관리|디렉터리|DirSyncEnabled 플래그 지정|
|핵심 디렉터리|디렉터리 관리|디렉터리|파트너 관계 설정|
|핵심 디렉터리|디렉터리 관리|디렉터리|실수로 인한 삭제 임계값 설정|
|핵심 디렉터리|디렉터리 관리|디렉터리|회사에 허용된 데이터 위치 설정|
|핵심 디렉터리|디렉터리 관리|디렉터리|회사에 다국적 기능 활성화 설정|
|핵심 디렉터리|디렉터리 관리|디렉터리|테넌트에 디렉터리 기능 설정|
|핵심 디렉터리|디렉터리 관리|디렉터리|도메인 인증 설정|
|핵심 디렉터리|디렉터리 관리|디렉터리|도메인에 페더레이션 설정 지정|
|핵심 디렉터리|사용자 관리|사용자|사용자 암호 강제 변경 설정|
|핵심 디렉터리|그룹 관리|그룹|그룹 라이선스 설정|
|핵심 디렉터리|그룹 관리|그룹|사용자에서 관리될 그룹 설정|
|핵심 디렉터리|디렉터리 관리|디렉터리|암호 정책 설정|
|핵심 디렉터리|디렉터리 관리|디렉터리|권한 관리 속성 설정|
|핵심 디렉터리|사용자 관리|사용자|사용자 관리자 설정|
|핵심 디렉터리|사용자 관리|사용자|사용자 인증 토큰 메타데이터 설정 사용|
|핵심 디렉터리|그룹 관리|그룹|사용자에게 그룹 기반 라이선스 적용 시작|
|핵심 디렉터리|그룹 관리|그룹|그룹 라이선스 다시 계산을 트리거|
|핵심 디렉터리|사용자 관리|사용자|StsRefreshTokenValidFrom 타임스탬프 업데이트|
|핵심 디렉터리|관리 단위 관리|AdministrativeUnit|관리 장치 업데이트|
|핵심 디렉터리|응용 프로그램 관리|응용 프로그램|응용 프로그램 업데이트|
|핵심 디렉터리|디렉터리 관리|디렉터리|회사 업데이트|
|핵심 디렉터리|디렉터리 관리|디렉터리|회사 설정 업데이트|
|핵심 디렉터리|리소스|리소스|장치 업데이트|
|핵심 디렉터리|리소스|리소스|장치 구성 업데이트|
|핵심 디렉터리|디렉터리 관리|디렉터리|도메인 업데이트|
|핵심 디렉터리|사용자 관리|사용자|외부 암호 업데이트|
|핵심 디렉터리|응용 프로그램 관리|응용 프로그램|외부 암호 업데이트|
|핵심 디렉터리|그룹 관리|그룹|그룹 업데이트|
|핵심 디렉터리|그룹 관리|그룹|그룹 설정 업데이트|
|핵심 디렉터리|정책 관리|정책|정책 업데이트|
|핵심 디렉터리|역할 관리|역할|역할 업데이트|
|핵심 디렉터리|응용 프로그램 관리|응용 프로그램|서비스 주체 업데이트|
|핵심 디렉터리|사용자 관리|사용자|사용자 업데이트|
|핵심 디렉터리|디렉터리 관리|디렉터리|도메인 확인|
|핵심 디렉터리|디렉터리 관리|디렉터리|메일로 확인된 도메인 확인|
|ID 보호|사용자 관리|사용자|관리자가 임시 암호 생성|
|ID 보호|사용자 관리|사용자|관리자는 사용자에게 암호 재설정 요구|
|ID 보호|기타|기타|단일 위험 이벤트 유형 다운로드|
|ID 보호|기타|기타|관리자 및 주간 다이제스트 옵트인 상태 다운로드|
|ID 보호|기타|기타|모든 위험 이벤트 유형 다운로드|
|ID 보호|기타|기타|무료 사용자 위험 이벤트 다운로드|
|ID 보호|기타|기타|위험 플래그가 지정된 사용자 다운로드|
|ID 보호|디렉터리 관리|디렉터리|온보딩|
|ID 보호|정책 관리|정책|MFA 등록 정책 설정|
|ID 보호|정책 관리|정책|로그인 위험 정책 설정|
|ID 보호|정책 관리|정책|사용자 위험 정책 설정|
|ID 보호|디렉터리 관리|디렉터리|경고 설정 업데이트|
|ID 보호|디렉터리 관리|디렉터리|주간 다이제스트 설정 업데이트|
|사용자 초대|사용자 관리|사용자|외부 사용자를 응용 프로그램에 할당|
|사용자 초대|기타|기타|Batch 초대가 처리됨|
|사용자 초대|기타|기타|Batch 초대가 업로드됨|
|사용자 초대|사용자 관리|사용자|이메일이 전송되지 않음, 사용자 구독 취소됨|
|사용자 초대|사용자 관리|사용자|외부 사용자 초대|
|사용자 초대|사용자 관리|사용자|외부 사용자 초대 사용|
|사용자 초대|사용자 관리|사용자|바이럴 테넌트 만들기|
|사용자 초대|사용자 관리|사용자|바이럴 사용자 만들기|
|MIM(Microsoft Identity Manager)|그룹 관리|그룹|멤버 추가|
|MIM(Microsoft Identity Manager)|그룹 관리|그룹|그룹 만들기|
|MIM(Microsoft Identity Manager)|그룹 관리|그룹|그룹 삭제|
|MIM(Microsoft Identity Manager)|그룹 관리|그룹|멤버 제거|
|MIM(Microsoft Identity Manager)|그룹 관리|그룹|그룹 업데이트|
|MIM(Microsoft Identity Manager)|사용자 관리|사용자|사용자 암호 등록|
|MIM(Microsoft Identity Manager)|사용자 관리|사용자|사용자 암호 재설정|
|Privileged Identity Management|역할 관리|역할|AccessReview_Review|
|Privileged Identity Management|역할 관리|역할|AccessReview_Update|
|Privileged Identity Management|역할 관리|역할|ActivationAborted|
|Privileged Identity Management|역할 관리|역할|ActivationApproved|
|Privileged Identity Management|역할 관리|역할|ActivationCanceled|
|Privileged Identity Management|역할 관리|역할|ActivationRequested|
|Privileged Identity Management|역할 관리|역할|추가됨|
|Privileged Identity Management|역할 관리|역할|할당|
|Privileged Identity Management|역할 관리|역할|상승|
|Privileged Identity Management|역할 관리|역할|제거됨|
|Privileged Identity Management|역할 관리|역할|역할 설정 변경 사항|
|Privileged Identity Management|역할 관리|역할|ScanAlertsNow|
|Privileged Identity Management|역할 관리|역할|등록|
|Privileged Identity Management|역할 관리|역할|승격 해제|
|Privileged Identity Management|역할 관리|역할|UpdateAlertSettings|
|Privileged Identity Management|역할 관리|역할|UpdateCurrentState|
|셀프 서비스 그룹 관리|그룹 관리|그룹|보류 중인 그룹 가입 요청 승인|
|셀프 서비스 그룹 관리|그룹 관리|그룹|보류 중인 그룹 가입 요청 취소|
|셀프 서비스 그룹 관리|그룹 관리|그룹|수명 주기 관리 정책 만들기|
|셀프 서비스 그룹 관리|그룹 관리|그룹|보류 중인 그룹 가입 요청 삭제|
|셀프 서비스 그룹 관리|그룹 관리|그룹|보류 중인 그룹 가입 요청 거부|
|셀프 서비스 그룹 관리|그룹 관리|그룹|그룹 갱신|
|셀프 서비스 그룹 관리|그룹 관리|그룹|그룹 가입 요청|
|셀프 서비스 그룹 관리|그룹 관리|그룹|동적 그룹 속성 설정|
|셀프 서비스 그룹 관리|그룹 관리|그룹|수명 주기 관리 정책 업데이트|
|셀프 서비스 암호 관리|사용자 관리|사용자|셀프 서비스 암호 재설정에서 차단|
|셀프 서비스 암호 관리|사용자 관리|사용자|암호 변경(셀프 서비스)|
|셀프 서비스 암호 관리|디렉터리 관리|디렉터리|디렉터리에 대한 암호 쓰기 사용 안 함|
|셀프 서비스 암호 관리|디렉터리 관리|디렉터리|디렉터리에 대한 암호 쓰기 사용|
|셀프 서비스 암호 관리|사용자 관리|사용자|암호 재설정(관리자)|
|셀프 서비스 암호 관리|사용자 관리|사용자|암호 재설정(셀프 서비스)|
|셀프 서비스 암호 관리|사용자 관리|사용자|셀프 서비스 암호 재설정 흐름 작업 진행률|
|셀프 서비스 암호 관리|사용자 관리|사용자|셀프 서비스 암호 재설정 흐름 작업 진행률|
|셀프 서비스 암호 관리|사용자 관리|사용자|사용자 계정 잠금 해제(셀프 서비스)|
|셀프 서비스 암호 관리|사용자 관리|사용자|사용자가 셀프 서비스 암호 재설정 등록|
|사용 약관|정책 관리|정책|사용 약관에 동의|
|사용 약관|정책 관리|정책|사용 약관 만들기|
|사용 약관|정책 관리|정책|사용 약관 거부|
|사용 약관|정책 관리|정책|사용 약관 삭제|
|사용 약관|정책 관리|정책|사용 약관 편집|
|사용 약관|정책 관리|정책|사용 약관 게시|
|사용 약관|정책 관리|정책|사용 약관 게시 취소|




## <a name="next-steps"></a>다음 단계

보고 개요는 [Azure Active Directory 보고](active-directory-reporting-azure-portal.md)를 참조하세요.

