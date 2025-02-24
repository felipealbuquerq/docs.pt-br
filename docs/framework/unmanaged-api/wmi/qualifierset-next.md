---
title: Função QualifierSet_Next (referência de API não gerenciada)
description: A função QualifierSet_Next recupera o próximo qualificador em uma enumeração.
ms.date: 11/06/2017
api_name:
- QualifierSet_Next
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- QualifierSet_Next
helpviewer_keywords:
- QualifierSet_Next function [.NET WMI and performance counters]
topic_type:
- Reference
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: f97a19f236b87a7f4c5b2014aca6ee4abd338c63
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/07/2019
ms.locfileid: "70798288"
---
# <a name="qualifierset_next-function"></a>Função QualifierSet_Next
Recupera o próximo qualificador em uma enumeração que começou com uma chamada para a função [QualifierSet_BeginEnumeration](qualifierset-beginenumeration.md).   

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT QualifierSet_Next (
   [in] int                  vFunc, 
   [in] IWbemQualifierSet*   ptr, 
   [in] LONG                 lFlags,
   [out] BSTR*               pstrName,        
   [out] VARIANT*            pVal,
   [out] LONG*               plFlavor                 
); 
```  

## <a name="parameters"></a>Parâmetros

`vFunc`   
no Este parâmetro não é usado.

`ptr`   
no Um ponteiro para uma instância de [IWbemQualifierSet](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemqualifierset) .

`lFlags`   
[in] Reservado. Esse parâmetro deve ser 0.

`pstrName`   
fora O nome do qualificador. Se `null`, esse parâmetro será ignorado; caso contrário `pstrName` , não deverá apontar para um `BSTR` erro válido ou ocorrerá um vazamento de memória. Se não for NULL, a função sempre alocará um novo `BSTR` quando retornar. `WBEM_S_NO_ERROR`

`pVal`   
fora Quando for bem-sucedido, o valor do qualificador. Se a função falhar, a `VARIANT` apontada para `pVal` by não será modificada. Se esse parâmetro for `null`, o parâmetro será ignorado.

`plFlavor`   
fora Um ponteiro para um longo que recebe o tipo de qualificador. Se as informações do tipo não forem desejadas, `null`esse parâmetro poderá ser. 

## <a name="return-value"></a>Valor retornado

Os valores a seguir retornados por essa função são definidos no arquivo de cabeçalho *WbemCli. h* ou você pode defini-los como constantes em seu código:

|Constante  |Valor  |Descrição  |
|---------|---------|---------|
|`WBEM_E_INVALID_PARAMETER` | 0x80041008 | Um parâmetro não é válido. |
|`WBEM_E_UNEXPECTED` | 0x8004101d | O chamador não chamou [QualifierSet_BeginEnumeration](qualifierset-beginenumeration.md). |
|`WBEM_E_OUT_OF_MEMORY` | 0x80041006 | Não há memória suficiente disponível para iniciar uma nova enumeração. |
| `WBEM_S_NO_MORE_DATA` | 0x40005 | Nenhum qualificador mais é deixado na enumeração. |
|`WBEM_S_NO_ERROR` | 0 | A chamada de função foi bem-sucedida.  |
  
## <a name="remarks"></a>Comentários

Essa função encapsula uma chamada para o método [IWbemQualifierSet:: Next](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemqualifierset-next) .

Você chama a `QualifierSet_Next` função repetidamente para enumerar todos os qualificadores até que a `WBEM_S_NO_MORE_DATA`função seja retornada. Para encerrar a enumeração antecipadamente, chame a função [QualifierSet_EndEnumeration](qualifierset-endenumeration.md) .

A ordem dos qualificadores retornados durante a enumeração é indefinida.

## <a name="requirements"></a>Requisitos  
 **Compatíveis** Confira [Requisitos de sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** WMINet_Utils.idl  
  
 **Versões do .NET Framework:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]  
  
## <a name="see-also"></a>Consulte também

- [WMI e contadores de desempenho (referência de API não gerenciada)](index.md)
