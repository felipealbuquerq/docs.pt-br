---
title: 'Como: Criar uma união do C/C++ usando atributos (C#)'
ms.date: 07/20/2015
ms.assetid: 85f35e56-26e0-4d31-9f3a-89bd4005e71a
ms.openlocfilehash: fdadc9505b93f40c66001ac36345efada2edd270
ms.sourcegitcommit: 986f836f72ef10876878bd6217174e41464c145a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69595366"
---
# <a name="how-to-create-a-cc-union-by-using-attributes-c"></a>Como: Criar uma união do C/C++ usando atributos (C#)
Usando atributos, você pode personalizar como structs são dispostos na memória. Por exemplo, você pode criar o que é conhecido como uma união no C/C++ usando os atributos `StructLayout(LayoutKind.Explicit)` e `FieldOffset`.  
  
## <a name="example"></a>Exemplo  
 Neste segmento de código, todos os campos de `TestUnion` são iniciados no mesmo local na memória.  
  
```csharp  
// Add a using directive for System.Runtime.InteropServices.  
  
       [System.Runtime.InteropServices.StructLayout(LayoutKind.Explicit)]  
       struct TestUnion  
       {  
           [System.Runtime.InteropServices.FieldOffset(0)]  
           public int i;  
  
           [System.Runtime.InteropServices.FieldOffset(0)]  
           public double d;  
  
           [System.Runtime.InteropServices.FieldOffset(0)]  
           public char c;  
  
           [System.Runtime.InteropServices.FieldOffset(0)]  
           public byte b;  
       }  
```  
  
## <a name="example"></a>Exemplo  
 A seguir, temos outro exemplo em que os campos são iniciados em locais diferentes definidos explicitamente.  
  
```csharp  
// Add a using directive for System.Runtime.InteropServices.  
  
       [System.Runtime.InteropServices.StructLayout(LayoutKind.Explicit)]  
       struct TestExplicit  
       {  
           [System.Runtime.InteropServices.FieldOffset(0)]  
           public long lg;  
  
           [System.Runtime.InteropServices.FieldOffset(0)]  
           public int i1;  
  
           [System.Runtime.InteropServices.FieldOffset(4)]  
           public int i2;  
  
           [System.Runtime.InteropServices.FieldOffset(8)]  
           public double d;  
  
           [System.Runtime.InteropServices.FieldOffset(12)]  
           public char c;  
  
           [System.Runtime.InteropServices.FieldOffset(14)]  
           public byte b;  
       }  
```  
  
 Os dois campos inteiros, `i1` e `i2`, compartilham os mesmos locais de memória que `lg`. Esse tipo de controle sobre o layout do struct é útil ao usar a invocação de plataforma.  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Reflection>
- <xref:System.Attribute>
- [Guia de Programação em C#](../../index.md)
- [Atributos](../../../../standard/attributes/index.md)
- [Reflexão (C#)](../reflection.md)
- [Atributos (C#)](./index.md)
- [Criando atributos personalizados (C#)](./creating-custom-attributes.md)
- [Acessando atributos usando reflexão (C#)](./accessing-attributes-by-using-reflection.md)
