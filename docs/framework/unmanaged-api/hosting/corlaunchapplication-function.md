---
title: Função CorLaunchApplication
ms.date: 03/30/2017
api_name:
- CorLaunchApplication
api_location:
- mscoree.dll
- clr.dll
api_type:
- COM
f1_keywords:
- CorLaunchApplication
helpviewer_keywords:
- CorLaunchApplication function [.NET Framework hosting]
ms.assetid: 71f362a9-8fe2-47ce-9302-05a645cf3d7d
topic_type:
- apiref
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 9861de733a9acb43c7e2a4b4941f9945fc5f0ba7
ms.sourcegitcommit: 7f616512044ab7795e32806578e8dc0c6a0e038f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67758377"
---
# <a name="corlaunchapplication-function"></a>Função CorLaunchApplication
Inicia o aplicativo no caminho de rede especificado, usando os manifestos especificados e outros dados de aplicativo.  
  
 Essa função foi preterida no .NET Framework 4.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT CorLaunchApplication (  
    [in]  HOST_TYPE                dwClickOnceHost,  
    [in]  LPCWSTR                  pwzAppFullName,  
    [in]  DWORD                    dwManifestPaths,  
    [in]  LPCWSTR                 *ppwzManifestPaths,  
    [in]  DWORD                    dwActivationData,  
    [in]  LPCWSTR                 *ppwzActivationData,  
    [out] LPPROCESS_INFORMATION    lpProcessInformation  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `dwClickOnceHost`  
 [in] Um valor igual a [HOST_TYPE](../../../../docs/framework/unmanaged-api/hosting/host-type-enumeration.md) enumeração que especifica o tipo de host que está iniciando o aplicativo.  
  
 `pwzAppFullName`  
 [in] O nome completo do aplicativo que está sendo iniciado.  
  
 `dwManifestPaths`  
 [in] O número de caminhos do manifesto do aplicativo.  
  
 `ppwzManifestPaths`  
 [in] Uma matriz de cadeias de caracteres, cada um deles especifica um caminho para um manifesto do aplicativo que está sendo iniciado.  
  
 `dwActivationData`  
 [in] O número de itens de dados de ativação para o aplicativo que está sendo iniciado.  
  
 `ppwzActivationData`  
 [in] Uma matriz de cadeias de caracteres, cada um deles é um item de dados de ativação para o aplicativo que está sendo iniciado.  
  
 `lpProcessInformation`  
 [out] Um ponteiro para obter informações sobre o processo no qual o aplicativo foi carregado.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** Confira [Requisitos de sistema](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE.h  
  
 **Biblioteca:** MSCorEE.dll  
  
 **Versões do .NET Framework:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Consulte também

- [Funções de hospedagem CLR preteridas](../../../../docs/framework/unmanaged-api/hosting/deprecated-clr-hosting-functions.md)
