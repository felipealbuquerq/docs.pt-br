---
title: Método ICorDebugManagedCallback::UpdateModuleSymbols
ms.date: 03/30/2017
api_name:
- ICorDebugManagedCallback.UpdateModuleSymbols
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugManagedCallback::UpdateModuleSymbols
helpviewer_keywords:
- UpdateModuleSymbols method [.NET Framework debugging]
- ICorDebugManagedCallback::UpdateModuleSymbols method [.NET Framework debugging]
ms.assetid: 0863f644-58e8-45a0-b0c3-a28e99b20938
topic_type:
- apiref
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 7a15e3ab0d50763ad53b1caa921035239868fec1
ms.sourcegitcommit: 7f616512044ab7795e32806578e8dc0c6a0e038f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67761235"
---
# <a name="icordebugmanagedcallbackupdatemodulesymbols-method"></a>Método ICorDebugManagedCallback::UpdateModuleSymbols
Notifica o depurador para que os símbolos para um módulo de tempo de execução de linguagem comum foram alterados.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT UpdateModuleSymbols (  
    [in] ICorDebugAppDomain *pAppDomain,  
    [in] ICorDebugModule    *pModule,  
    [in] IStream            *pSymbolStream  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `pAppDomain`  
 [in] Um ponteiro para um objeto ICorDebugAppDomain que representa o domínio de aplicativo que contém o módulo no qual os símbolos foram alterados.  
  
 `pModule`  
 [in] Um ponteiro para um objeto ICorDebugModule que representa o módulo no qual os símbolos foram alterados.  
  
 `pSymbolStream`  
 [in] Um ponteiro para uma COM Win32 `IStream` objeto que contém os símbolos modificados.  
  
## <a name="remarks"></a>Comentários  
 Esse método fornece uma oportunidade para atualizar a exibição do depurador de símbolos do módulo, chamando [isymunmanagedreader:: Updatesymbolstore](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedreader-updatesymbolstore-method.md) ou [isymunmanagedreader:: Replacesymbolstore](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedreader-replacesymbolstore-method.md).  
  
 Esse retorno de chamada pode ocorrer várias vezes para o mesmo módulo.  
  
 Um depurador deve tentar associar pontos de interrupção não associados no nível da origem.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** Confira [Requisitos de sistema](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Cabeçalho:** CorDebug.idl, CorDebug.h  
  
 **Biblioteca:** CorGuids.lib  
  
 **Versões do .NET Framework:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Consulte também

- [Interface ICorDebugManagedCallback](../../../../docs/framework/unmanaged-api/debugging/icordebugmanagedcallback-interface.md)
