---
title: Enumeração CorDebugMappingResult
ms.date: 03/30/2017
api_name:
- CorDebugMappingResult
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- CorDebugMappingResult
helpviewer_keywords:
- CorDebugMappingResult enumeration [.NET Framework debugging]
ms.assetid: 701281dd-2936-45c8-a1f0-3bf7332b093b
topic_type:
- apiref
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: c2042d0936359a85d203375c42be0d8a096f004e
ms.sourcegitcommit: 7f616512044ab7795e32806578e8dc0c6a0e038f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67739756"
---
# <a name="cordebugmappingresult-enumeration"></a>Enumeração CorDebugMappingResult
Fornece os detalhes sobre como o valor do ponteiro de instrução (IP) foi obtido.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef enum CorDebugMappingResult {  
    MAPPING_PROLOG              = 0x1,  
    MAPPING_EPILOG              = 0x2,  
    MAPPING_NO_INFO             = 0x4,  
    MAPPING_UNMAPPED_ADDRESS    = 0x8,  
    MAPPING_EXACT               = 0x10,  
    MAPPING_APPROXIMATE         = 0x20,  
} CorDebugMappingResult;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`MAPPING_PROLOG`|O código nativo está no prólogo, portanto, o valor do IP é 0.|  
|`MAPPING_EPILOG`|O código nativo está em um epílogo, portanto, o valor do IP é o endereço da última instrução do método.|  
|`MAPPING_NO_INFO`|Nenhuma informação de mapeamento está disponível para o método, portanto, o valor do IP é 0.|  
|`MAPPING_UNMAPPED_ADDRESS`|Embora haja informações de mapeamento para o método, o endereço atual não pode ser mapeado para o código Microsoft intermediate language (MSIL). O valor do IP é 0.|  
|`MAPPING_EXACT`|O método mapeia exatamente para o código MSIL ou o quadro foi interpretado, portanto, o valor do IP é preciso.|  
|`MAPPING_APPROXIMATE`|O método foi mapeado com êxito, mas o valor do IP pode ser aproximado.|  
  
## <a name="remarks"></a>Comentários  
 Você pode usar o [icordebugilframe:: Getip](../../../../docs/framework/unmanaged-api/debugging/icordebugilframe-getip-method.md) método para obter o valor do ponteiro de instrução.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** Confira [Requisitos de sistema](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Cabeçalho:** CorDebug.idl, CorDebug.h  
  
 **Biblioteca:** CorGuids.lib  
  
 **Versões do .NET Framework:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Consulte também

- [Declarando enumerações](../../../../docs/framework/unmanaged-api/debugging/debugging-enumerations.md)
