---
title: Método ISymUnmanagedWriter2::DefineLocalVariable2
ms.date: 03/30/2017
api_name:
- ISymUnmanagedWriter2.DefineLocalVariable2
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedWriter2::DefineLocalVariable2
helpviewer_keywords:
- ISymUnmanagedWriter2::DefineLocalVariable2 method [.NET Framework debugging]
- DefineLocalVariable2 method [.NET Framework debugging]
ms.assetid: e774eefe-858c-4362-8d2d-28ebf2ba1a24
topic_type:
- apiref
author: mairaw
ms.author: mairaw
ms.openlocfilehash: 8fa385805d3e2dca8fef3e1490b2c67dd0583373
ms.sourcegitcommit: 7f616512044ab7795e32806578e8dc0c6a0e038f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67755060"
---
# <a name="isymunmanagedwriter2definelocalvariable2-method"></a>Método ISymUnmanagedWriter2::DefineLocalVariable2
Define uma única variável no escopo léxico atual. Esse método pode ser chamado várias vezes para uma variável de mesmo nome que tem vários residências ao longo de um escopo. Nesse caso, no entanto, os valores de `startOffset` e `endOffset` parâmetros não devem se sobrepor.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT DefineLocalVariable2(  
    [in] const WCHAR  *name,  
    [in] ULONG32      attributes,  
    [in] mdSignature  sigToken,  
    [in] ULONG32      addrKind,  
    [in] ULONG32      addr1,  
    [in] ULONG32      addr2,  
    [in] ULONG32      addr3,  
    [in] ULONG32      startOffset,  
    [in] ULONG32      endOffset);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `name`  
 [in] O nome da variável local.  
  
 `attributes`  
 [in] Atributos da variável local.  
  
 `sigToken`  
 [in] O token de metadados da assinatura.  
  
 `addrKind`  
 [in] O tipo de endereço.  
  
 `addr1`  
 [in] O primeiro endereço para a especificação de parâmetro.  
  
 `addr2`  
 [in] O segundo endereço para a especificação de parâmetro.  
  
 `addr3`  
 [in] O terceiro endereço para a especificação de parâmetro.  
  
 `startOffset`  
 [in] O deslocamento inicial da variável. Esse parâmetro é opcional. Se for 0, esse parâmetro será ignorado e a variável será definida ao longo de todo o escopo. Se for um valor diferente de zero, a variável estará dentro dos deslocamentos do escopo atual.  
  
 `endOffset`  
 [in] O deslocamento final da variável. Esse parâmetro é opcional. Se for 0, esse parâmetro será ignorado e a variável será definida ao longo de todo o escopo. Se for um valor diferente de zero, a variável estará dentro dos deslocamentos do escopo atual.  
  
## <a name="return-value"></a>Valor de retorno  
 S_OK se o método for bem-sucedido; Caso contrário, E_FAIL ou algum outro código de erro.  
  
## <a name="requirements"></a>Requisitos  
 **Cabeçalho:** CorSym.idl  
  
## <a name="see-also"></a>Consulte também

- [Interface ISymUnmanagedWriter2](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter2-interface.md)
- [Método DefineLocalVariable](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter-definelocalvariable-method.md)
