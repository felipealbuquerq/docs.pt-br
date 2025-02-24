---
title: Método ICorDebugController::Continue
ms.date: 03/30/2017
api_name:
- ICorDebugController.Continue
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugController::Continue
helpviewer_keywords:
- Continue method [.NET Framework debugging]
- ICorDebugController::Continue method [.NET Framework debugging]
ms.assetid: 8684cd06-ad3e-48ef-832e-15320e1f43a2
topic_type:
- apiref
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: c3a4e98a7265bda288b20b1cee1a10ab11990e8e
ms.sourcegitcommit: 7f616512044ab7795e32806578e8dc0c6a0e038f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67748889"
---
# <a name="icordebugcontrollercontinue-method"></a>Método ICorDebugController::Continue
Retoma a execução de threads gerenciados após uma chamada para [método Stop](../../../../docs/framework/unmanaged-api/debugging/icordebugcontroller-stop-method.md).  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT Continue (  
    [in] BOOL fIsOutOfBand  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `fIsOutOfBand`  
 [in] Definido como `true` se continuar a partir de um evento de out-of-band; caso contrário, defina como `false`.  
  
## <a name="remarks"></a>Comentários  
 `Continue` continua o processo após uma chamada para o `ICorDebugController::Stop` método.  
  
 Ao fazer a depuração de modo misto, não chame `Continue` no Win32 eventos de thread, a menos que você está continuando a partir de um evento de fora da banda.  
  
 Uma *eventos em banda* é um evento gerenciado ou um evento não gerenciado normal durante o qual o depurador oferece suporte a interação com o estado do processo gerenciado. Nesse caso, o depurador recebe o [icordebugunmanagedcallback:: Debugevent](../../../../docs/framework/unmanaged-api/debugging/icordebugunmanagedcallback-debugevent-method.md) retorno de chamada com seus `fOutOfBand` parâmetro definido como `false`.  
  
 Uma *evento out-of-band* é um evento não gerenciado durante o qual a interação com o estado do processo gerenciado é impossível enquanto o processo é interrompido devido a evento. Nesse caso, o depurador recebe o `ICorDebugUnmanagedCallback::DebugEvent` retorno de chamada com seus `fOutOfBand` parâmetro definido como `true`.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** Confira [Requisitos de sistema](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Cabeçalho:** CorDebug.idl, CorDebug.h  
  
 **Biblioteca:** CorGuids.lib  
  
 **Versões do .NET Framework:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Consulte também
