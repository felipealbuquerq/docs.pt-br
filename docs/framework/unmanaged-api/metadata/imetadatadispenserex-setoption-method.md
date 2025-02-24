---
title: Método IMetaDataDispenserEx::SetOption
ms.date: 03/30/2017
api_name:
- IMetaDataDispenserEx.SetOption
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataDispenserEx::SetOption
helpviewer_keywords:
- IMetaDataDispenserEx::SetOption method [.NET Framework metadata]
- SetOption method [.NET Framework metadata]
ms.assetid: 9f1c7ccd-7fb2-41d8-aa00-24b823376527
topic_type:
- apiref
author: mairaw
ms.author: mairaw
ms.openlocfilehash: 6916e6344fe5c112b216ca753c372fa73a4d5af5
ms.sourcegitcommit: 7f616512044ab7795e32806578e8dc0c6a0e038f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67777706"
---
# <a name="imetadatadispenserexsetoption-method"></a>Método IMetaDataDispenserEx::SetOption
Define a opção especificada para um determinado valor para o escopo de metadados atual. A opção controla como as chamadas para o escopo de metadados atual são tratadas.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT SetOption (  
    [in] REFGUID optionId,   
    [in] const VARIANT *pValue  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `optionId`  
 [in] Um ponteiro para um GUID que especifica a opção a ser definido.  
  
 `pValue`  
 [in] O valor a ser usado para definir a opção. O tipo deste valor deve ser uma variante do tipo da opção especificada.  
  
## <a name="remarks"></a>Comentários  
 A tabela a seguir lista os GUIDs disponíveis que o `optionId` parâmetro pode apontar para e os valores válidos correspondentes para o `pValue` parâmetro.  
  
|GUID|Descrição|`pValue` Parâmetro|  
|----------|-----------------|------------------------|  
|MetaDataCheckDuplicatesFor|Controla quais itens estão marcados para duplicatas. Sempre que você chama um [IMetaDataEmit](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-interface.md) método que cria um novo item, você pode solicitar o método para verificar se o item já existe no escopo atual. Por exemplo, você pode verificar a existência de `mdMethodDef` itens; nesse caso, quando você chama [imetadataemit:: Definemethod](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-definemethod-method.md), ele verificará que o método ainda não existir no escopo atual. Essa verificação usa a chave que identifica exclusivamente um determinado método: assinatura, nome e tipo de pai.|Deve ser uma variante do tipo UI4 e deve conter uma combinação dos valores da [CorCheckDuplicatesFor](../../../../docs/framework/unmanaged-api/metadata/corcheckduplicatesfor-enumeration.md) enumeração.|  
|MetaDataRefToDefCheck|Controla quais referenciado itens é convertidas em definições. Por padrão, o mecanismo de metadados otimizará o código ao converter um item referenciado em sua definição, se o item referenciado é definido no escopo atual.|Deve ser uma variante do tipo UI4 e deve conter uma combinação dos valores da [CorRefToDefCheck](../../../../docs/framework/unmanaged-api/metadata/correftodefcheck-enumeration.md) enumeração.|  
|MetaDataNotificationForTokenMovement|Controles de qual token remapeia que ocorrem durante uma mesclagem de metadados geram retornos de chamada. Use o [imetadataemit:: Sethandler](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-sethandler-method.md) método para estabelecer sua [IMapToken](../../../../docs/framework/unmanaged-api/metadata/imaptoken-interface.md) interface.|Deve ser uma variante do tipo UI4 e deve conter uma combinação dos valores da [CorNotificationForTokenMovement](../../../../docs/framework/unmanaged-api/metadata/cornotificationfortokenmovement-enumeration.md) enumeração.|  
|MetaDataSetENC|Controla o comportamento de editar e continuar (ENC). Apenas um modo de comportamento pode ser definido por vez.|Deve ser uma variante do tipo UI4 e deve conter um valor igual a [CorSetENC](../../../../docs/framework/unmanaged-api/metadata/corsetenc-enumeration.md) enumeração. O valor não é uma máscara de bits.|  
|MetaDataErrorIfEmitOutOfOrder|Controla quais erros emitidos-fora de ordem geram retornos de chamada. Emitir metadados fora de ordem não é fatal; No entanto, se você emitir metadados em uma ordem que é preferida pelo mecanismo de metadados, os metadados são mais compacto e, portanto, podem ser pesquisados com mais eficiência. Use o `IMetaDataEmit::SetHandler` método para estabelecer sua [IMetaDataError](../../../../docs/framework/unmanaged-api/metadata/imetadataerror-interface.md) interface.|Deve ser uma variante do tipo UI4 e deve conter uma combinação dos valores da [CorErrorIfEmitOutOfOrder](../../../../docs/framework/unmanaged-api/metadata/corerrorifemitoutoforder-enumeration.md) enumeração.|  
|MetaDataImportOption|Controla quais tipos de itens que foram excluídos durante um ENC são recuperados por um enumerador.|Deve ser uma variante do tipo UI4 e deve conter uma combinação dos valores da [enumeração CorImportOptions](../../../../docs/framework/unmanaged-api/metadata/corimportoptions-enumeration.md) enumeração.|  
|MetaDataThreadSafetyOptions|Controla se o mecanismo de metadados obtém leitor/gravador bloqueios, garantindo assim o acesso thread-safe. Por padrão, o mecanismo pressupõe que o acesso é single-thread do chamador, portanto, não há bloqueios são obtidos. Os clientes são responsáveis por manter a sincronização de thread adequado ao usar a API de metadados.|Deve ser uma variante do tipo UI4 e deve conter um valor igual a [CorThreadSafetyOptions](../../../../docs/framework/unmanaged-api/metadata/corthreadsafetyoptions-enumeration.md) enumeração. O valor não é uma máscara de bits.|  
|MetaDataGenerateTCEAdapters|Controla se o importador da biblioteca deve gerar os adaptadores de evento firmemente acopladas (TCE) para contêineres de ponto de conexão de COM.|Deve ser uma variante do tipo BOOL. Se `pValue` é definido como `true`, importador da biblioteca gera os adaptadores TCE.|  
|MetaDataTypeLibImportNamespace|Especifica um namespace não padrão para a biblioteca de tipos que está sendo importado.|Deve ser um valor nulo ou uma variante do tipo BSTR. Se `pValue` é um valor nulo, o namespace atual é definido como nulo; caso contrário, o namespace atual será definido como a cadeia de caracteres que é mantida no tipo BSTR da variante.|  
|MetaDataLinkerOptions|Controla se o vinculador deve gerar um assembly ou um arquivo de módulo do .NET Framework.|Deve ser uma variante do tipo UI4 e deve conter uma combinação dos valores da [CorLinkerOptions](../../../../docs/framework/unmanaged-api/metadata/corlinkeroptions-enumeration.md) enumeração.|  
|MetaDataRuntimeVersion|Especifica a versão do common language runtime em relação ao qual essa imagem foi criada. A versão é armazenada como uma cadeia de caracteres, como "v1.0.3705".|Deve ser um valor nulo, um valor VT_EMPTY ou uma variante do tipo BSTR. Se `pValue` é nulo, a versão de tempo de execução é definida como null. Se `pValue` é VT_EMPTY, a versão é definida como um valor padrão, que é desenhado da versão do mscorwks. dll dentro do qual o código de metadados está em execução. Caso contrário, a versão de tempo de execução é definida como a cadeia de caracteres que é mantida no tipo BSTR da variante.|  
|MetaDataMergerOptions|Especifica opções para mesclagem de metadados.|Deve ser uma variante do tipo UI4 e deve conter uma combinação dos valores da `MergeFlags` enumeração, que é descrita no arquivo corhdr. H.|  
|MetaDataPreserveLocalRefs|Desabilita otimização referências locais nas definições.|Deve conter uma combinação dos valores da [CorLocalRefPreservation](../../../../docs/framework/unmanaged-api/metadata/corlocalrefpreservation-enumeration.md) enumeração.|  
  
## <a name="requirements"></a>Requisitos  
 **Plataforma:** Confira [Requisitos de sistema](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor.h  
  
 **Biblioteca:** Usado como um recurso em mscoree. dll  
  
 **Versões do .NET Framework:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Consulte também

- [Interface IMetaDataDispenserEx](../../../../docs/framework/unmanaged-api/metadata/imetadatadispenserex-interface.md)
- [Interface IMetaDataDispenser](../../../../docs/framework/unmanaged-api/metadata/imetadatadispenser-interface.md)
