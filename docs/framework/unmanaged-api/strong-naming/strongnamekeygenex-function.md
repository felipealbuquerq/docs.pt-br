---
title: Função StrongNameKeyGenEx
ms.date: 03/30/2017
api_name:
- StrongNameKeyGenEx
api_location:
- mscoree.dll
api_type:
- DLLExport
f1_keywords:
- StrongNameKeyGenEx
helpviewer_keywords:
- StrongNameKeyGenEx function [.NET Framework strong naming]
ms.assetid: 36bd10b9-9857-45f3-8d3b-0da091d6169e
topic_type:
- apiref
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: f9ab908866402bd7a883114466f32921321a5ee6
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/07/2019
ms.locfileid: "70799004"
---
# <a name="strongnamekeygenex-function"></a>Função StrongNameKeyGenEx
Gera um novo par de chaves pública/privada com o tamanho de chave especificado, para uso de nome forte.  
  
 Esta função foi preterida. Em vez disso, use o método [ICLRStrongName:: StrongNameKeyGenEx](../hosting/iclrstrongname-strongnamekeygenex-method.md) .  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
BOOLEAN StrongNameKeyGenEx (  
    [in]  LPCWSTR   wszKeyContainer,  
    [in]  DWORD     dwFlags,  
    [in]  DWORD     dwKeySize,  
    [out] BYTE      **ppbKeyBlob,  
    [out] ULONG     *pcbKeyBlob  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `wszKeyContainer`  
 no O nome do contêiner de chave solicitado. `wszKeyContainer`deve ser uma cadeia de caracteres não vazia ou NULL para gerar um nome temporário.  
  
 `dwFlags`  
 no Especifica se a chave deve ser desregistrada. Há suporte para os seguintes valores:  
  
- 0x00000000-usado quando `wszKeyContainer` é nulo para gerar um nome de contêiner de chave temporário.  
  
- 0x00000001 (`SN_LEAVE_KEY`) – especifica que a chave deve ser registrada.  
  
 `dwKeySize`  
 no O tamanho solicitado da chave, em bits.  
  
 `ppbKeyBlob`  
 fora O par de chaves pública/privada retornado.  
  
 `pcbKeyBlob`  
 fora O tamanho, em bytes, de `ppbKeyBlob`.  
  
## <a name="return-value"></a>Valor de retorno  
 `true`após a conclusão bem-sucedida; caso contrário `false`,.  
  
## <a name="remarks"></a>Comentários  
 O .NET Framework versões 1,0 e 1,1 exigem um `dwKeySize` de 1024 bits para assinar um assembly com um nome forte; a versão 2,0 adiciona suporte para chaves de bits de 2048.  
  
 Depois que a chave for recuperada, você deverá chamar a função [StrongNameFreeBuffer](strongnamefreebuffer-function.md) para liberar a memória alocada.  
  
 Se a `StrongNameKeyGenEx` função não for concluída com êxito, chame a função [StrongNameErrorInfo](strongnameerrorinfo-function.md) para recuperar o último erro gerado.  
  
## <a name="requirements"></a>Requisitos  
 **Compatíveis** Confira [Requisitos de sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** StrongName.h  
  
 **Biblioteca** Incluído como um recurso em MsCorEE. dll  
  
 **Versões do .NET Framework:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Consulte também

- [Método StrongNameKeyGenEx](../hosting/iclrstrongname-strongnamekeygenex-method.md)
- [Método StrongNameKeyGen](../hosting/iclrstrongname-strongnamekeygen-method.md)
- [Interface ICLRStrongName](../hosting/iclrstrongname-interface.md)
