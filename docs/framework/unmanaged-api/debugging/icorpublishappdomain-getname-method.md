---
title: Método ICorPublishAppDomain::GetName
ms.date: 03/30/2017
api_name:
- ICorPublishAppDomain.GetName
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorPublishAppDomain::GetName
helpviewer_keywords:
- GetName method, ICorPublishAppDomain interface [.NET Framework debugging]
- ICorPublishAppDomain::GetName method [.NET Framework debugging]
ms.assetid: 6ef8ac9b-9803-4b65-8b13-25f3e0b1bc6b
topic_type:
- apiref
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: b518a3be939c70b207a71d79a3d362dba26fd3d0
ms.sourcegitcommit: 7f616512044ab7795e32806578e8dc0c6a0e038f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67774193"
---
# <a name="icorpublishappdomaingetname-method"></a>Método ICorPublishAppDomain::GetName
Obtém o nome do domínio do aplicativo que é representado por esse [ICorPublishAppDomain](../../../../docs/framework/unmanaged-api/debugging/icorpublishappdomain-interface.md).  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetName (  
    [in]  ULONG32   cchName,   
    [out] ULONG32   *pcchName,  
    [out, size_is(cchName), length_is(*pcchName)]   
        WCHAR       *szName  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `cchName`  
 [in] O tamanho do `szName` matriz.  
  
 `pcchName`  
 [out] Um ponteiro para o número de caracteres largos, incluindo o caractere nulo, retornado no `szName` matriz.  
  
 `szName`  
 [out] Uma matriz na qual armazenar o nome.  
  
## <a name="remarks"></a>Comentários  
 Se `szName` não for nulo, o `GetName` método copia até `cchName` caracteres (incluindo o terminador nulo) em `szName`. Se não nulo é retornado em `pcchName`, o número real de caracteres no nome (incluindo o terminador nulo) é armazenado na `szName` matriz.  
  
 O `GetName` método retorna um S_OK HRESULT, independentemente de quantos caracteres foram copiado.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** Confira [Requisitos de sistema](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Cabeçalho:** CorPub.idl, CorPub.h  
  
 **Biblioteca:** CorGuids.lib  
  
 **Versões do .NET Framework:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Consulte também

- [Interface ICorPublishAppDomain](../../../../docs/framework/unmanaged-api/debugging/icorpublishappdomain-interface.md)
