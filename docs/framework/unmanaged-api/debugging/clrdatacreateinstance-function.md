---
title: Função CLRDataCreateInstance
ms.date: 03/30/2017
api_name:
- CLRDataCreateInstance
api_location:
- mscordbi.dll
- mscordacwks.dll
api_type:
- COM
f1_keywords:
- CLRDataCreateInstance
helpviewer_keywords:
- CLRDataCreateInstance function [.NET Framework debugging]
ms.assetid: 440bad90-5a88-45e7-9157-4596801d8d19
topic_type:
- apiref
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 5a839eb2edd36dc726c819a819fd4d427fbaea40
ms.sourcegitcommit: 7f616512044ab7795e32806578e8dc0c6a0e038f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67740995"
---
# <a name="clrdatacreateinstance-function"></a>Função CLRDataCreateInstance
Cria um objeto de interface para o item de destino especificado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT CLRDataCreateInstance (  
    [in]  REFIID           iid,   
    [in]  ICLRDataTarget  *target,   
    [out] void           **iface  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `iid`  
 [in] O identificador da interface a ser instanciado.  
  
 `target`  
 [in] Um ponteiro para um usuário implementada [ICLRDataTarget](../../../../docs/framework/unmanaged-api/debugging/iclrdatatarget-interface.md) objeto que representa o item de destino para o qual criar o objeto de interface.  
  
 `iface`  
 [out] Um ponteiro para o endereço do objeto de interface retornada.  
  
## <a name="remarks"></a>Comentários  
 O `ICLRDataTarget` objeto é implementado pelo gravador do aplicativo de depuração. A implementação depende do tipo de item de destino que está sendo representado. O item de destino pode ser um processo, despejo de memória, computador remoto e assim por diante.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** Confira [Requisitos de sistema](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Cabeçalho:** ClrData.idl  
  
 **Biblioteca:** CorGuids.lib  
  
 **Versões do .NET Framework:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Consulte também

- [Depurando funções estáticas globais](../../../../docs/framework/unmanaged-api/debugging/debugging-global-static-functions.md)
