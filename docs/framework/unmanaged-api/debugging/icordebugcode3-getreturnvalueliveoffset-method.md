---
title: Método ICorDebugCode3::GetReturnValueLiveOffset
ms.date: 03/30/2017
dev_langs:
- cpp
api_name:
- ICorDebugCode3.GetReturnValueLiveOffset
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugCode3::GetReturnValueLiveOffset
helpviewer_keywords:
- ICorDebugCode3::GetReturnValueLiveOffset method [.NET Framework debugging]
- GetReturnValueLiveOffset method [.NET Framework debugging]
ms.assetid: 8c2ff5d8-8c04-4423-b1e1-e1c8764b36d3
topic_type:
- apiref
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 5aa53d1c9d101544f532c51f43a8b47143117813
ms.sourcegitcommit: 37616676fde89153f563a485fc6159fc57326fc2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/23/2019
ms.locfileid: "69988279"
---
# <a name="icordebugcode3getreturnvalueliveoffset-method"></a>Método ICorDebugCode3::GetReturnValueLiveOffset
Para um deslocamento de IL especificado, obtém os deslocamentos nativos onde um ponto de interrupção deve ser colocado para que o depurador possa obter o valor de retorno de uma função.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp
HRESULT GetReturnValueLiveOffset(  
    [in] ULONG32 ILoffset,  
    [in] ULONG32 bufferSize,   
    [out] ULONG32 *pFetched,   
    [out, size_is(buffersize), length_is(*pFetched)] ULong32 pOffsets[]  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `ILoffset`  
 O deslocamento de IL. Ele deve ser um site de chamada de função ou a chamada de função falhará.  
  
 `bufferSize`  
 O número de bytes disponíveis para armazenar `pOffsets`.  
  
 `pFetched`  
 Um ponteiro para o número de deslocamentos realmente retornados. Normalmente, seu valor é 1, mas uma única instrução Il pode ser mapeada para `CALL` várias instruções de assembly.  
  
 `pOffsets`  
 Uma matriz de deslocamentos nativos. Normalmente, `pOffsets` contém um único deslocamento, embora uma única instrução de Il possa ser mapeada para vários mapas `CALL` para várias instruções de assembly.  
  
## <a name="remarks"></a>Comentários  
 Esse método é usado junto com o método [ICorDebugILFrame3:: GetReturnValueForILOffset](../../../../docs/framework/unmanaged-api/debugging/icordebugilframe3-getreturnvalueforiloffset-method.md) para obter o valor de retorno de um método que retorna um tipo de referência. Passar um deslocamento de IL para um site de chamada de função para esse método retorna um ou mais deslocamentos nativos. O depurador pode então definir pontos de interrupção nesses deslocamentos nativos na função. Quando o depurador atinge um dos pontos de interrupção, você pode passar o mesmo deslocamento de IL passado para esse método para o método [ICorDebugILFrame3:: GetReturnValueForILOffset](../../../../docs/framework/unmanaged-api/debugging/icordebugilframe3-getreturnvalueforiloffset-method.md) para obter o valor de retorno. O depurador deve limpar todos os pontos de interrupção definidos por ele.  
  
> [!WARNING]
> Os `ICorDebugCode3::GetReturnValueLiveOffset` métodos e [ICorDebugILFrame3:: GetReturnValueForILOffset](../../../../docs/framework/unmanaged-api/debugging/icordebugilframe3-getreturnvalueforiloffset-method.md) permitem obter informações de valor de retorno somente para tipos de referência. Não há suporte para a recuperação de informações de valor de retorno de tipos de valor ( <xref:System.ValueType>ou seja, todos os tipos que derivam de).  
  
 A função retorna os `HRESULT` valores mostrados na tabela a seguir.  
  
|Valor `HRESULT`|Descrição|  
|---------------------|-----------------|  
|`S_OK`|Êxito.|  
|`CORDBG_E_INVALID_OPCODE`|O site de deslocamento de IL fornecido não é uma instrução de chamada ou a `void`função retorna.|  
|`CORDBG_E_UNSUPPORTED`|O deslocamento IL fornecido é uma chamada adequada, mas não há suporte para o tipo de retorno para obter um valor de retorno.|  
  
 O `ICorDebugCode3::GetReturnValueLiveOffset` método está disponível apenas em sistemas baseados em x86 e amd64.  
  
## <a name="requirements"></a>Requisitos  
 **Compatíveis** Confira [Requisitos de sistema](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Cabeçalho:** CorDebug.idl, CorDebug.h  
  
 **Biblioteca** CorGuids.lib  
  
 **Versões do .NET Framework:** [!INCLUDE[net_current_v451plus](../../../../includes/net-current-v451plus-md.md)]  
  
## <a name="see-also"></a>Consulte também

- [Método GetReturnValueForILOffset](../../../../docs/framework/unmanaged-api/debugging/icordebugilframe3-getreturnvalueforiloffset-method.md)
- [Interface ICorDebugCode3](../../../../docs/framework/unmanaged-api/debugging/icordebugcode3-interface.md)
