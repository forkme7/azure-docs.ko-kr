---
title: Azure CDN(Content Delivery Network) 제품 기능 | Microsoft Docs
description: 각 Azure CDN(Content Delivery Network) 제품이 지원하는 기능에 대해 알아봅니다.
services: cdn
documentationcenter: ''
author: dksimpson
manager: akucer
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 03/23/2018
ms.author: rli
ms.custom: mvc
ms.openlocfilehash: 40638e2180b63c90fbcbe15cc2c1e944a97e166e
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/05/2018
---
# <a name="azure-cdn-product-features"></a>Azure CDN 제품 기능

Azure CDN(Content Delivery Network) 제품에는 **Akamai의 Azure CDN Standard**, **Verizon의 Azure CDN Standard**, **Verizon의 Azure CDN Premium** 등의 세 가지가 있습니다. 다음 표는 각 제품에 사용할 수 있는 기능을 비교합니다.

| **성능 기능 및 최적화** | Standard Akamai | Standard Verizon | Premium Verizon |
| --- | --- | --- | --- |
| [동적 사이트 가속](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration) | **&#x2713;**  | **&#x2713;** | **&#x2713;** |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[동적 사이트 가속 - 적응 이미지 압축](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#adaptive-image-compression-akamai-only) | **&#x2713;**  |  |  |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[동적 사이트 가속 - 개체 프리페치](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#object-prefetch-akamai-only) | **&#x2713;**  |  |  |
| [비디오 스트리밍 최적화](https://docs.microsoft.com/azure/cdn/cdn-media-streaming-optimization) | **&#x2713;**  | \* |  \* |
| [큰 파일 최적화](https://docs.microsoft.com/azure/cdn/cdn-large-file-optimization) | **&#x2713;**  | \* |  \* |
| [GSLB(전역 서버 부하 분산)](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-load-balancing-azure) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [빠른 삭제](cdn-purge-endpoint.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [자산 미리 로드](cdn-preload-endpoint.md) | |**&#x2713;** |**&#x2713;** |
| 캐시/헤더 설정([캐싱 규칙](cdn-caching-rules.md)사용) |**&#x2713;** |**&#x2713;** | |
| 캐시/헤더 설정( [규칙 엔진](cdn-rules-engine.md)사용) | | |**&#x2713;** |
| [쿼리 문자열 캐싱](cdn-query-string.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| IPv4/IPv6 이중 스택 |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [HTTP/2 지원](cdn-http2.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
||||
 **보안** | **Standard Akamai** | **Standard Verizon** | **Premium Verizon** | 
| CDN 끝점에 HTTPS 지원 |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [사용자 지정 도메인 HTTPS](cdn-custom-ssl.md) | |**&#x2713;** |**&#x2713;** |
| [사용자 지정 도메인 이름 지원](cdn-map-content-to-custom-domain.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [지역 필터링](cdn-restrict-access-by-country.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [토큰 인증](cdn-token-auth.md)|  |  |**&#x2713;**| 
| [DDOS 보호](https://www.us-cert.gov/ncas/tips/ST04-015) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
||||
| **분석 및 보고** | **Standard Akamai** | **Standard Verizon** | **Premium Verizon** | 
| [Azure 진단 로그](cdn-azure-diagnostic-logs.md) | **&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Verizon의 핵심 보고서](cdn-analyze-usage-patterns.md) | |**&#x2713;** |**&#x2713;** |
| [Verizon의 사용자 지정 보고서](cdn-verizon-custom-reports.md) | |**&#x2713;** |**&#x2713;** |
| [고급 HTTP 보고서](cdn-advanced-http-reports.md) | | |**&#x2713;** |
| [실시간 통계](cdn-real-time-stats.md) | | |**&#x2713;** |
| [에지 노드 성능](cdn-edge-performance.md) | | |**&#x2713;** |
| [실시간 경고](cdn-real-time-alerts.md) | | |**&#x2713;** |
||||
| **사용 편의성** | **Standard Akamai** | **Standard Verizon** | **Premium Verizon** | 
| [Storage](cdn-create-a-storage-account-with-cdn.md), [Cloud Services](cdn-cloud-service-with-cdn.md), [Web Apps](../app-service/app-service-web-tutorial-content-delivery-network.md), [Media Services](../media-services/media-services-portal-manage-streaming-endpoints.md)와 같은 Azure와 간편하게 통합 |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [REST API](https://msdn.microsoft.com/library/mt634456.aspx), [.NET](cdn-app-dev-net.md), [Node.js](cdn-app-dev-node.md) 또는 [PowerShell](cdn-manage-powershell.md)을 통한 관리 |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [사용자 지정이 가능한 규칙 기반의 콘텐츠 배달 엔진](cdn-rules-engine.md) | | |**&#x2713;** |
| URL 리디렉션/다시 쓰기([규칙 엔진](cdn-rules-engine.md)사용) | | |**&#x2713;** |
| 모바일 장치 규칙( [규칙 엔진](cdn-rules-engine.md)사용) | | |**&#x2713;** |

\*Verizon은 일반 웹 배달 최적화를 통해 큰 파일 및 미디어를 배달하도록 직접 지원합니다.



