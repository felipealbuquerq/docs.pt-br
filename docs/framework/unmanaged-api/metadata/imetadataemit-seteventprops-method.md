---
title: Método IMetaDataEmit::SetEventProps
ms.date: 03/30/2017
api_name:
- IMetaDataEmit.SetEventProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit::SetEventProps
helpviewer_keywords:
- IMetaDataEmit::SetEventProps method [.NET Framework metadata]
- SetEventProps method [.NET Framework metadata]
ms.assetid: 3b039e50-63ec-4730-99ff-2327408de477
topic_type:
- apiref
author: mairaw
ms.author: mairaw
ms.openlocfilehash: 271ecd7e757340becccb7bf52362487a2b277299
ms.sourcegitcommit: 7f616512044ab7795e32806578e8dc0c6a0e038f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67737188"
---
# <a name="imetadataemitseteventprops-method"></a>Método IMetaDataEmit::SetEventProps
Define ou atualiza o recurso especificado de um evento definido por uma chamada anterior ao [imetadataemit:: Defineevent](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-defineevent-method.md).  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT SetEventProps (  
    [in]  mdEvent     ev,   
    [in]  DWORD       dwEventFlags,   
    [in]  mdToken     tkEventType,   
    [in]  mdMethodDef mdAddOn,   
    [in]  mdMethodDef mdRemoveOn,   
    [in]  mdMethodDef mdFire,   
    [in]  mdMethodDef rmdOtherMethods[]   
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `ev`  
 [in] O token de evento.  
  
 `dwEventFlags`  
 [in] Sinalizadores de eventos. Esse é um bitmask de `CorEventAttr` valores.  
  
 `tkEventType`  
 [in] O token para a classe de evento. Isso é um `mdTypeDef` ou um `mdTypeRef` token.  
  
 `mdAddOn`  
 [in] O método usado para assinar o evento, ou null.  
  
 `mdRemoveOn`  
 [in] O método usado para cancelar a assinatura para o evento, ou nulo.  
  
 `mdFire`  
 [in] O método usado (por uma classe derivada) para gerar o evento.  
  
 `rmdOtherMethods[]`  
 [in] Uma matriz de tokens para outros métodos associados ao evento. O último elemento da matriz deve ser `mdMethodDefNil`.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** Confira [Requisitos de sistema](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor.h  
  
 **Biblioteca:** Usado como um recurso em mscoree. dll  
  
 **Versões do .NET Framework:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Consulte também

- [Interface IMetaDataEmit](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-interface.md)
- [Interface IMetaDataEmit2](../../../../docs/framework/unmanaged-api/metadata/imetadataemit2-interface.md)
