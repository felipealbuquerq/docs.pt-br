---
title: Método IMetaDataInfo::GetFileMapping
ms.date: 03/30/2017
api_name:
- IMetaDataInfo.GetFileMapping
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataInfo::GetFileMapping
helpviewer_keywords:
- IMetaDataInfo::GetFileMapping method [.NET Framework metadata]
- GetFileMapping method [.NET Framework metadata]
ms.assetid: 2868dfec-c992-4606-88bb-a8e0b6b18271
topic_type:
- apiref
author: mairaw
ms.author: mairaw
ms.openlocfilehash: 22af95ef4bd1fca0a8253faa6ce0e1c7a862054d
ms.sourcegitcommit: 7f616512044ab7795e32806578e8dc0c6a0e038f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67782656"
---
# <a name="imetadatainfogetfilemapping-method"></a>Método IMetaDataInfo::GetFileMapping
Obtém a região de memória do arquivo mapeado e o tipo de mapeamento.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetFileMapping (  
    [out] const void           **ppvData,   
    [out] ULONGLONG            *pcbData,   
    [out] DWORD                *pdwMappingType  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `ppvData`  
 [out] Um ponteiro para o início do arquivo mapeado.  
  
 `pcbData`  
 [out] O tamanho da região mapeada. Se `pdwMappingType` é `fmFlat`, esse é o tamanho do arquivo.  
  
 `pdwMappingType`  
 [out] Um [CorFileMapping](../../../../docs/framework/unmanaged-api/metadata/corfilemapping-enumeration.md) valor que indica o tipo de mapeamento. A implementação atual do common language runtime (CLR) sempre retorna `fmFlat`. Outros valores são reservados para uso futuro. No entanto, você sempre deve verificar o valor retornado, porque outros valores podem ser habilitados em versões futuras, ou versões de serviço.  
  
## <a name="return-value"></a>Valor de retorno  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|`S_OK`|Todas as saídas são preenchidas.|  
|`E_INVALIDARG`|NULL foi passado como um valor de argumento.|  
|`COR_E_NOTSUPPORTED`|A implementação CLR não pode fornecer informações sobre a região de memória. Isso pode ocorrer pelos seguintes motivos:<br /><br /> -O escopo de metadados foi aberto com o `ofWrite` ou `ofCopyMemory` sinalizador.<br />-O escopo de metadados foi aberto sem o `ofReadOnly` sinalizador.<br />-A [imetadatadispenser:: Openscopeonmemory](../../../../docs/framework/unmanaged-api/metadata/imetadatadispenser-openscopeonmemory-method.md) método foi usado para abrir somente a parte de metadados do arquivo.<br />-O arquivo não é um arquivo executável portátil (PE). **Observação:**  Essas condições dependem da implementação do CLR e provavelmente enfraquecida em futuras versões do CLR.|  
  
## <a name="remarks"></a>Comentários  
 A memória que `ppvData` pontos a serem é válido somente quando o escopo de metadados subjacente está aberto.  
  
 Para que esse método funcione, quando você mapeia os metadados de um arquivo em disco na memória chamando o [imetadatadispenser:: Openscope](../../../../docs/framework/unmanaged-api/metadata/imetadatadispenser-openscope-method.md) método, você deve especificar o `ofReadOnly` sinalizador e você não deve especificar o `ofWrite` ou `ofCopyMemory` sinalizador.  
  
 A escolha do tipo de mapeamento de arquivo para cada escopo é específica para uma determinada implementação do CLR. Ele não pode ser definido pelo usuário. A implementação atual do CLR sempre retorna `fmFlat` em `pdwMappingType`, mas isso pode mudar em futuras versões do CLR ou em futuras versões de serviço de uma determinada versão. Você sempre deve verificar o valor retornado `pdwMappingType`, porque os tipos diferentes terão diferentes layouts e deslocamentos.  
  
 Não há suporte para a passagem de NULL para qualquer um dos três parâmetros. O método retorna `E_INVALIDARG`, e nenhuma das saídas são preenchidas. Ignorar o tipo de mapeamento ou o tamanho da região pode resultar em encerramento anormal do programa.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** Confira [Requisitos de sistema](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor.h  
  
 **Biblioteca:** Usado como um recurso em mscoree. dll  
  
 **Versões do .NET Framework:** [!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>Consulte também

- [Interface IMetaDataInfo](../../../../docs/framework/unmanaged-api/metadata/imetadatainfo-interface.md)
- [Enumeração CorFileMapping](../../../../docs/framework/unmanaged-api/metadata/corfilemapping-enumeration.md)
