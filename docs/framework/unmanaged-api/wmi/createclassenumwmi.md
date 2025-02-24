---
title: Função CreateClassEnumWmi (referência de API não gerenciada)
description: A função CreateClassEnumWmi retorna um enumerador para todas as classes que atendem aos critérios especificados.
ms.date: 11/06/2017
api_name:
- CreateClassEnumWmi
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- CreateClassEnumWmi
helpviewer_keywords:
- CreateClassEnumWmi function [.NET WMI and performance counters]
topic_type:
- Reference
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: a696a6f02f6d3a5afbcb45e5566e4b667739e2c5
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/07/2019
ms.locfileid: "70798732"
---
# <a name="createclassenumwmi-function"></a>Função CreateClassEnumWmi
Retorna um enumerador para todas as classes que satisfaçam os critérios de seleção especificados.

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>Sintaxe

```cpp
HRESULT CreateClassEnumWmi (
   [in] BSTR                    strSuperclass,
   [in] long                    lFlags,
   [in] IWbemContext*           pCtx,
   [out] IEnumWbemClassObject** ppEnum,
   [in] DWORD                   authLevel,
   [in] DWORD                   impLevel,
   [in] IWbemServices*          pCurrentNamespace,
   [in] BSTR                    strUser,
   [in] BSTR                    strPassword,
   [in] BSTR                    strAuthority
);
```

## <a name="parameters"></a>Parâmetros

`strSuperclass`\
no Se não `null` estiver ou em branco, especifica o nome de uma classe pai; o enumerador retorna apenas subclasses dessa classe. Se estiver `null` ou em branco e `lFlags` for WBEM_FLAG_SHALLOW, retornará somente classes de nível superior (classes sem classe pai). Se estiver `null` ou em branco e `lFlags` for `WBEM_FLAG_DEEP`, retorna todas as classes no namespace.

`lFlags`\
no Uma combinação de sinalizadores que afetam o comportamento dessa função. Os valores a seguir são definidos no arquivo de cabeçalho *WbemCli. h* ou você pode defini-los como constantes em seu código:

|Constante  |Valor  |Descrição  |
|---------|---------|---------|
| `WBEM_FLAG_USE_AMENDED_QUALIFIERS` | 0x20000 | Se definido, a função recupera os qualificadores corrigidos armazenados no namespace localizado da localidade da conexão atual. <br/> Se não for definido, a função recuperará somente os qualificadores armazenados no namespace imediato. |
| `WBEM_FLAG_DEEP` | 0 | A enumeração inclui todas as subclasses na hierarquia, mas não essa classe. |
| `WBEM_FLAG_SHALLOW` | 1 | A enumeração inclui apenas instâncias puras dessa classe e exclui todas as instâncias de subclasses que fornecem propriedades não encontradas nesta classe. |
| `WBEM_FLAG_RETURN_IMMEDIATELY` | 0x10 | O sinalizador causa uma chamada de semisynchronous. |
| `WBEM_FLAG_FORWARD_ONLY` | 0x20 | A função retorna um enumerador somente encaminhamento. Normalmente, enumeradores somente de encaminhamento são mais rápidos e usam menos memória do que enumeradores convencionais, mas não permitem que as chamadas [clonem](clone.md). |
| `WBEM_FLAG_BIDIRECTIONAL` | 0 | O WMI retém ponteiros para objetos na enumeração até que sejam liberados. |

Os sinalizadores recomendados são `WBEM_FLAG_RETURN_IMMEDIATELY` e `WBEM_FLAG_FORWARD_ONLY` para obter o melhor desempenho.

`pCtx`\
no Normalmente, esse valor é `null`. Caso contrário, é um ponteiro para uma instância de [IWbemContext](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemcontext) que pode ser usada pelo provedor que está fornecendo as classes solicitadas.

`ppEnum`\
fora Recebe o ponteiro para o enumerador.

`authLevel`\
no O nível de autorização.

`impLevel`\
no O nível de representação.

`pCurrentNamespace`\
no Um ponteiro para um objeto [IWbemServices](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemservices) que representa o namespace atual.

`strUser`\
no O nome de usuário. Consulte a função [ConnectServerWmi](connectserverwmi.md) para obter mais informações.

`strPassword`\
no A senha. Consulte a função [ConnectServerWmi](connectserverwmi.md) para obter mais informações.

`strAuthority`\
no O nome de domínio do usuário. Consulte a função [ConnectServerWmi](connectserverwmi.md) para obter mais informações.

## <a name="return-value"></a>Valor retornado

Os valores a seguir retornados por essa função são definidos no arquivo de cabeçalho *WbemCli. h* ou você pode defini-los como constantes em seu código:

|Constante  |Valor  |Descrição  |
|---------|---------|---------|
| `WBEM_E_ACCESS_DENIED` | 0x80041003 | O usuário não tem permissão para exibir uma ou mais das classes que a função pode retornar. |
| `WBEM_E_FAILED` | 0x80041001 | Ocorreu um erro não especificado. |
| `WBEM_E_INVALID_CLASS` | 0x80041010 | `strSuperClass` não existe. |
| `WBEM_E_INVALID_PARAMETER` | 0x80041008 | Um parâmetro não é válido. |
| `WBEM_E_OUT_OF_MEMORY` | 0x80041006 | Não há memória disponível suficiente para concluir a operação. |
| `WBEM_E_SHUTTING_DOWN` | 0x80041033 | O WMI provavelmente foi interrompido e reiniciado. Chame [ConnectServerWmi](connectserverwmi.md) novamente. |
| `WBEM_E_TRANSPORT_FAILURE` | 0x80041015 | O link RPC (chamada de procedimento remoto) entre o processo atual e o WMI falhou. |
|`WBEM_S_NO_ERROR` | 0 | A chamada de função foi bem-sucedida.  |

## <a name="remarks"></a>Comentários

Essa função encapsula uma chamada para o método [IWbemServices:: CreateClassEnum](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemservices-createclassenum) .

Se a chamada de função falhar, você poderá obter informações adicionais sobre o erro chamando a função [GetErrorInfo](geterrorinfo.md) .

## <a name="requirements"></a>Requisitos

**Compatíveis** Confira [Requisitos de sistema](../../get-started/system-requirements.md).

**Cabeçalho:** WMINet_Utils.idl

**Versões do .NET Framework:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>Consulte também

- [WMI e contadores de desempenho (referência de API não gerenciada)](index.md)
