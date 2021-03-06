---
title: Linux VM MSI를 사용하여 Azure Cosmos DB 액세스
description: Linux VM에서 시스템 할당 MSI(관리 서비스 ID)를 사용하여 Azure Cosmos DB에 액세스하는 프로세스를 안내하는 자습서입니다.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/09/2018
ms.author: skwan
ms.openlocfilehash: 692bc5eb401ccda36ef42006de509144170f7757
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2018
---
# <a name="use-a-linux-vm-msi-to-access-azure-cosmos-db"></a>Linux VM MSI를 사용하여 Azure Cosmos DB 액세스 

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]


이 자습서에서는 Linux VM MSI를 만들어 사용하는 방법을 보여줍니다. 다음 방법에 대해 알아봅니다.

> [!div class="checklist"]
> * MSI가 활성화된 Linux VM 만들기 
> * Cosmos DB 계정 만들기
> * Cosmos DB 계정에서 컬렉션을 만듭니다.
> * Azure Cosmos DB 인스턴스에 대한 MSI 액세스 권한 부여
> * Linux VM MSI의 `principalID` 검색
> * 액세스 토큰을 가져와 Azure Resource Manager를 호출하는 데 사용
> * Cosmos DB 호출을 위해 Azure Resource Manager에서 액세스 키 가져오기

## <a name="prerequisites"></a>필수 조건

아직 Azure 계정이 없으면 계속하기 전에 [평가판 계정](https://azure.microsoft.com)에 등록해야 합니다.

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

이 자습서의 CLI 스크립트 예제는 두 가지 옵션을 통해 실행할 수 있습니다.

- Azure Portal에서 또는 각 코드 블록의 오른쪽 상단 모서리에 있는 **사용해 보세요** 단추를 통해 [Azure Cloud Shell](~/articles/cloud-shell/overview.md)을 사용합니다.
- 로컬 CLI 콘솔을 사용하려는 경우 [CLI 2.0의 최신 버전(2.0.23 이상)을 설치](https://docs.microsoft.com/cli/azure/install-azure-cli)합니다.

## <a name="sign-in-to-azure"></a>Azure에 로그인

[https://portal.azure.com](https://portal.azure.com)에서 Azure Portal에 로그인합니다.

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>새 리소스 그룹에 Linux 가상 머신 만들기

이 자습서에서는 Linux VM이 활성화된 새 MSI를 만듭니다.

MSI 기반 VM을 만들려면:

1. Azure CLI를 로컬 콘솔에서 사용하는 경우 [az login](/cli/azure/reference-index#az_login)을 사용하여 먼저 Azure에 로그인합니다. VM을 배포하려는 Azure 구독과 연결된 계정을 사용하세요.

   ```azurecli-interactive
   az login
   ```

2. [az group create](/cli/azure/group/#az_group_create)를 사용하여 VM 및 관련 리소스를 포함하고 배포하는 데 사용할 [리소스 그룹](../../azure-resource-manager/resource-group-overview.md#terminology)을 만듭니다. 대신 사용하려는 리소스 그룹이 이미 있다면 이 단계를 건너뛰어도 됩니다.

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. [az vm create](/cli/azure/vm/#az_vm_create)를 사용하여 VM을 만듭니다. 다음 예제에서는 `--assign-identity` 매개 변수의 요청에 따라 MSI를 사용하여 *myVM*이라는 VM을 만듭니다. ph x="1" /> 및 `--admin-password` 매개 변수는 가상 머신 로그인을 위한 관리자 이름 및 암호 계정을 지정합니다. 이러한 값은 사용자 환경에 적절하게 업데이트합니다. 

   ```azurecli-interactive 
   az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter --generate-ssh-keys --assign-identity --admin-username azureuser --admin-password myPassword12

## Create a Cosmos DB account 

If you don't already have one, create a Cosmos DB account. You can skip this step and use an existing Cosmos DB account. 

1. Click the **+/Create new service** button found on the upper left-hand corner of the Azure portal.
2. Click **Databases**, then **Azure Cosmos DB**, and a new "New account" panel  displays.
3. Enter an **ID** for the Cosmos DB account, which you use later.  
4. **API** should be set to "SQL." The approach described in this tutorial can be used with the other available API types, but the steps in this tutorial are for the SQL API.
5. Ensure the **Subscription** and **Resource Group** match the ones you specified when you created your VM in the previous step.  Select a **Location** where Cosmos DB is available.
6. Click **Create**.

## Create a collection in the Cosmos DB account

Next, add a data collection in the Cosmos DB account that you can query in later steps.

1. Navigate to your newly created Cosmos DB account.
2. On the **Overview** tab click the **+/Add Collection** button, and an "Add Collection" panel slides out.
3. Give the collection a database ID, collection ID, select a storage capacity, enter a partition key, enter a throughput value, then click **OK**.  For this tutorial, it is sufficient to use "Test" as the database ID and collection ID, select a fixed storage capacity and lowest throughput (400 RU/s).  

## Retrieve the `principalID` of the Linux VM's MSI

To gain access to the Cosmos DB account access keys from the Resource Manager in the following section, you need to retrieve the `principalID` of the Linux VM's MSI.  Be sure to replace the `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>` (resource group in which you VM resides), and `<VM NAME>` parameter values with your own values.

```azurecli-interactive
az resource show --id /subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAMe> --api-version 2017-12-01
```
응답은 시스템에서 할당한 MSI의 세부 정보를 포함합니다(다음 섹션에서 사용할 보안 주체 ID 기록).

```bash  
{
    "id": "/subscriptions/<SUBSCRIPTION ID>/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAMe>",
  "identity": {
    "principalId": "6891c322-314a-4e85-b129-52cf2daf47bd",
    "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533f8",
    "type": "SystemAssigned"
 }

```
## <a name="grant-your-linux-vm-msi-access-to-the-cosmos-db-account-access-keys"></a>Cosmos DB 계정 액세스 키에 Linux VM MSI 액세스 부여

Cosmos DB는 Azure AD 인증을 기본적으로 지원하지 않습니다. 그러나 MSI를 사용하여 Resource Manager에서 Cosmos DB 액세스 키를 검색한 다음, 해당 키를 사용하여 Cosmos DB에 액세스할 수 있습니다. 이 단계에서 Cosmos DB 계정에 키에 대한 MSI 액세스를 부여합니다.

Azure CLI를 사용하여 Azure Resource Manager의 Cosmos DB 계정에 대해 MSI ID 액세스를 부여하려면 작업 환경의 `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>` 및 `<COSMOS DB ACCOUNT NAME>` 값을 업데이트합니다. `<MSI PRINCIPALID>`를 [Linux VM MSI의 principalID 검색](#retrieve-the-principalID-of-the-linux-VM's-MSI)의 `az resource show` 명령에서 반환된 `principalId` 속성으로 바꿉니다.  Cosmos DB는 액세스 키를 사용할 때 두 가지 수준의 세분성: 계정에 대한 읽기/쓰기 액세스 및 계정에 대한 읽기 전용 액세스를 지원합니다.  계정에 대한 읽기/쓰기 키를 가져오려는 경우 `DocumentDB Account Contributor` 역할을 할당하거나 계정에 대한 읽기 전용 키를 가져오려는 경우 `Cosmos DB Account Reader Role` 역할을 할당합니다.

```azurecli-interactive
az role assignment create --assignee <MSI PRINCIPALID> --role '<ROLE NAME>' --scope "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.DocumentDB/databaseAccounts/<COSMODS DB ACCOUNT NAME>"
```

응답에는 생성된 역할 할당에 대한 세부 정보가 포함됩니다.

```
{
  "id": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.DocumentDB/databaseAccounts/<COSMOS DB ACCOUNT>/providers/Microsoft.Authorization/roleAssignments/5b44e628-394e-4e7b-bbc3-d6cd4f28f15b",
  "name": "5b44e628-394e-4e7b-bbc3-d6cd4f28f15b",
  "properties": {
    "principalId": "c0833082-6cc3-4a26-a1b1-c4b5f90a981f",
    "roleDefinitionId": "/subscriptions/<SUBSCRIPTION ID>/providers/Microsoft.Authorization/roleDefinitions/fbdf93bf-df7d-467e-a4d2-9458aa1360c8",
    "scope": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.DocumentDB/databaseAccounts/<COSMOS DB ACCOUNT>"
  },
  "resourceGroup": "<RESOURCE GROUP>",
  "type": "Microsoft.Authorization/roleAssignments"
}
```

## <a name="get-an-access-token-using-the-linux-vms-msi-and-use-it-to-call-azure-resource-manager"></a>Linux VM의 MSI를 사용하여 액세스 토큰을 가져온 다음, Azure Resource Manager를 호출하는 데 사용

자습서의 나머지 부분에서는 이전에 만든 VM에서 작업합니다.

아래의 단계를 완료하려면 SSH 클라이언트가 필요합니다. Windows를 사용 중인 경우 [Linux용 Windows 하위 시스템](https://msdn.microsoft.com/commandline/wsl/install_guide)에서 SSH 클라이언트를 사용할 수 있습니다. SSH 클라이언트의 키 구성에 대한 도움이 필요하면 [Azure에서 Windows를 통해 SSH 키를 사용하는 방법](../../virtual-machines/linux/ssh-from-windows.md) 또는 [Azure에서 Linux VM용 SSH 공개 및 개인 키 쌍을 만들고 사용하는 방법](../../virtual-machines/linux/mac-create-ssh-keys.md)을 참조하세요.

1. Azure Portal에서 **Virtual Machines**, Linux 가상 머신으로 이동한 후 **개요** 페이지 위쪽의 **연결**을 클릭합니다. VM에 연결하기 위한 문자열을 복사합니다. 
2. SSH 클라이언트를 사용하여 VM에 연결합니다.  
3. 그리고 나면 **Linux VM**을 만들 때 추가했던 **암호**를 입력하라는 메시지가 표시됩니다. 이제 정상적으로 로그인되어야 합니다.  
4. CURL을 사용하여 Azure Resource Manager에 대한 액세스 토큰을 가져옵니다. 
     
    ```bash
    curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -H Metadata:true   
    ```
 
    > [!NOTE]
    > 이전 요청에서 "resource" 매개 변수의 값은 Azure AD에 필요한 값과 정확하게 일치해야 합니다. Azure Resource Manager 리소스 ID를 사용할 때는 URI에 후행 슬래시를 포함해야 합니다.
    > 다음 응답에서는 간단히 나타내기 위해 access_token 요소를 축약했습니다.
    
    ```bash
    {"access_token":"eyJ0eXAiOi...",
     "expires_in":"3599",
     "expires_on":"1518503375",
     "not_before":"1518499475",
     "resource":"https://management.azure.com/",
     "token_type":"Bearer",
     "client_id":"1ef89848-e14b-465f-8780-bf541d325cd5"}
     ```
    
## <a name="get-access-keys-from-azure-resource-manager-to-make-cosmos-db-calls"></a>Cosmos DB 호출을 위해 Azure Resource Manager에서 액세스 키 가져오기  

이제 CURL을 사용하여 이전 섹션에서 검색한 액세스 토큰을 사용하여 Resource Manager를 호출하여 Cosmos DB 계정 액세스 키를 검색합니다. 액세스 키가 있으면 Cosmos DB를 쿼리할 수 있습니다. `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>` 및 `<COSMOS DB ACCOUNT NAME>` 매개 변수 값을 원하는 값으로 바꾸세요. `<ACCESS TOKEN>` 값을 앞서 검색한 액세스 토큰으로 바꿉니다.  읽기/쓰기 키를 검색하려는 경우 키 작업 유형 `listKeys`를 사용합니다.  읽기 전용 키를 검색하려는 경우 키 작업 유형 `readonlykeys`를 사용합니다.

```bash 
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.DocumentDB/databaseAccounts/<COSMOS DB ACCOUNT NAME>/<KEY OPERATION TYPE>?api-version=2016-03-31' -X POST -d "" -H "Authorization: Bearer <ACCESS TOKEN>" 
```

> [!NOTE]
> 이전 URL의 텍스트는 대/소문자를 구분하므로 리소스 그룹을 적절하게 반영하는 대/소문자를 사용하는지 확인하세요. 또한 이 요청은 GET 요청이 아닌 POST 요청임을 기억해야 하며, -d(NULL일 수 있음)를 사용하여 길이 제한을 캡처하는 값을 전달해야 합니다.  

CURL 응답에서는 키 목록을 제공합니다.  예를 들어 읽기 전용 키를 가져오는 경우:  

```bash 
{"primaryReadonlyMasterKey":"bWpDxS...dzQ==",
"secondaryReadonlyMasterKey":"38v5ns...7bA=="}
```

이제 Cosmos DB 계정에 대한 액세스 키가 있으므로 Cosmos DB SDK에 전달하고 계정에 액세스하는 호출을 만들 수 있습니다.  빠른 예제의 경우 Azure CLI에 액세스 키를 전달할 수 있습니다.  Azure Portal에서 Cosmos DB 계정 블레이드의 **개요** 탭에서 <COSMOS DB CONNECTION URL>을 가져올 수 있습니다.  <ACCESS KEY>를 위에서 얻은 값으로 바꿉니다.

```bash
az cosmosdb collection show -c <COLLECTION ID> -d <DATABASE ID> --url-connection "<COSMOS DB CONNECTION URL>" --key <ACCESS KEY>
```

이 CLI 명령은 컬렉션에 대한 세부 정보를 반환합니다.

```bash
{
  "collection": {
    "_conflicts": "conflicts/",
    "_docs": "docs/",
    "_etag": "\"00006700-0000-0000-0000-5a8271e90000\"",
    "_rid": "Es5SAM2FDwA=",
    "_self": "dbs/Es5SAA==/colls/Es5SAM2FDwA=/",
    "_sprocs": "sprocs/",
    "_triggers": "triggers/",
    "_ts": 1518498281,
    "_udfs": "udfs/",
    "id": "Test",
    "indexingPolicy": {
      "automatic": true,
      "excludedPaths": [],
      "includedPaths": [
        {
          "indexes": [
            {
              "dataType": "Number",
              "kind": "Range",
              "precision": -1
            },
            {
              "dataType": "String",
              "kind": "Range",
              "precision": -1
            },
            {
              "dataType": "Point",
              "kind": "Spatial"
            }
          ],
          "path": "/*"
        }
      ],
      "indexingMode": "consistent"
    }
  },
  "offer": {
    "_etag": "\"00006800-0000-0000-0000-5a8271ea0000\"",
    "_rid": "f4V+",
    "_self": "offers/f4V+/",
    "_ts": 1518498282,
    "content": {
      "offerIsRUPerMinuteThroughputEnabled": false,
      "offerThroughput": 400
    },
    "id": "f4V+",
    "offerResourceId": "Es5SAM2FDwA=",
    "offerType": "Invalid",
    "offerVersion": "V2",
    "resource": "dbs/Es5SAA==/colls/Es5SAM2FDwA=/"
  }
}
```

## <a name="next-steps"></a>다음 단계

- MSI의 개요는 [Azure 리소스용 MSI(관리 서비스 ID)](overview.md)를 참조하세요.

