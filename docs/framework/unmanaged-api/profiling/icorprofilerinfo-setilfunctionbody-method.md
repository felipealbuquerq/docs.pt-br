---
title: Método ICorProfilerInfo::SetILFunctionBody
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo.SetILFunctionBody
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo::SetILFunctionBody
helpviewer_keywords:
- ICorProfilerInfo::SetILFunctionBody method [.NET Framework profiling]
- SetILFunctionBody method [.NET Framework profiling]
ms.assetid: b159c712-00f4-4fc7-a990-40bf9f642e8f
topic_type:
- apiref
author: mairaw
ms.author: mairaw
ms.openlocfilehash: abe0a0fc177c9ec89f4621e7defb5330c911034b
ms.sourcegitcommit: 7f616512044ab7795e32806578e8dc0c6a0e038f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67778610"
---
# <a name="icorprofilerinfosetilfunctionbody-method"></a>Método ICorProfilerInfo::SetILFunctionBody
Substitui o corpo da função especificada no módulo especificado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT SetILFunctionBody(  
    [in] ModuleID    moduleId,  
    [in] mdMethodDef methodid,  
    [in] LPCBYTE     pbNewILMethodHeader);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `moduleId`  
 [in] A ID do módulo no qual a função reside.  
  
 `methodid`  
 [in] O token da função para o qual substituir o corpo.  
  
 `pbNewILMethodHeader`  
 [in] O novo cabeçalho para a função.  
  
## <a name="remarks"></a>Comentários  
 O `SetILFunctionBody` método substitui o endereço virtual relativo da função nos metadados para que ele aponta para o corpo da função nova e ajusta as estruturas de dados interna conforme necessário.  
  
 O `SetILFunctionBody` método pode ser chamado em apenas as funções que nunca foi compiladas por um compilador just-in-time (JIT).  
  
 Use o [ICorProfilerInfo:: Getilfunctionbodyallocator](../../../../docs/framework/unmanaged-api/profiling/icorprofilerinfo-getilfunctionbodyallocator-method.md) método para alocar espaço para o novo método garantir que o buffer é compatível.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** Confira [Requisitos de sistema](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf.idl, CorProf.h  
  
 **Biblioteca:** CorGuids.lib  
  
 **Versões do .NET Framework:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Consulte também

- [Interface ICorProfilerInfo](../../../../docs/framework/unmanaged-api/profiling/icorprofilerinfo-interface.md)
