---
title: Método IMetaDataImport::GetModuleFromScope
ms.date: 03/30/2017
api_name:
- IMetaDataImport.GetModuleFromScope
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::GetModuleFromScope
helpviewer_keywords:
- GetModuleFromScope method [.NET Framework metadata]
- IMetaDataImport::GetModuleFromScope method [.NET Framework metadata]
ms.assetid: add68d3f-45fd-4bef-af94-eb5273f26b11
topic_type:
- apiref
author: mairaw
ms.author: mairaw
ms.openlocfilehash: 05ce699669095e9c0b45882b18a01ec326640038
ms.sourcegitcommit: 7f616512044ab7795e32806578e8dc0c6a0e038f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67779008"
---
# <a name="imetadataimportgetmodulefromscope-method"></a>Método IMetaDataImport::GetModuleFromScope
Obtém os metadados de um token para o módulo referenciado no escopo atual de metadados.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetModuleFromScope (  
   [out] mdModule    *pmd  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `pmd`  
 [out] Um ponteiro para o token que representa o módulo referenciado no escopo atual de metadados.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** Confira [Requisitos de sistema](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor.h  
  
 **Biblioteca:** Incluído como um recurso em mscoree. dll  
  
 **Versões do .NET Framework:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Consulte também

- [Interface IMetaDataImport](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-interface.md)
- [Interface IMetaDataImport2](../../../../docs/framework/unmanaged-api/metadata/imetadataimport2-interface.md)
