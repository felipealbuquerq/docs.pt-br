---
title: <c> – Guia de Programação em C#
ms.custom: seodec18
ms.date: 07/20/2015
f1_keywords:
- c
- <c>
helpviewer_keywords:
- text, marking as code [C#]
- code, marking text as [C#]
- c C# XML tag
- <c> C# XML tag
ms.assetid: aad5b16e-a29e-445e-bd0d-eea0b138d7b2
ms.openlocfilehash: c881b9ec577fb04381f9e10c7df8f9d0da44cd66
ms.sourcegitcommit: 986f836f72ef10876878bd6217174e41464c145a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69588175"
---
# <a name="c-c-programming-guide"></a>\<c> (Guia de Programação em C#)
## <a name="syntax"></a>Sintaxe  
  
```xml  
<c>text</c>  
```  
  
## <a name="parameters"></a>Parâmetros  
 `text`  
 O texto que você deseja indicar como código.  
  
## <a name="remarks"></a>Comentários  
 A marca \<c> oferece uma maneira de indicar que o texto em uma descrição deve ser marcado como código. Use [\<code>](./code.md) para indicar várias linhas como código.  
  
 Compile com [/doc](../../language-reference/compiler-options/doc-compiler-option.md) para processar comentários de documentação em um arquivo.  
  
## <a name="example"></a>Exemplo  
 [!code-csharp[csProgGuideDocComments#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#2)]  
  
## <a name="see-also"></a>Consulte também

- [Guia de Programação em C#](../index.md)
- [Marcas recomendadas para comentários de documentação](./recommended-tags-for-documentation-comments.md)
