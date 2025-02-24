---
title: Constantes – Guia de Programação em C#
ms.custom: seodec18
ms.date: 07/20/2015
helpviewer_keywords:
- C# language, constants
- constants [C#]
ms.assetid: 1fb39621-1738-49b1-a1b3-8587f109123f
ms.openlocfilehash: 7da86a8999f6cc36a7b71f70fd92a363673824b5
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69924541"
---
# <a name="constants-c-programming-guide"></a>Constantes (Guia de Programação em C#)
As constantes são valores imutáveis que são conhecidos no tempo de compilação e não são alterados durante a vida útil do programa. Constantes são declaradas com o modificador [const](../../language-reference/keywords/const.md). Apenas os tipos C# internos (excluindo <xref:System.Object?displayProperty=nameWithType>) podem ser declarados como `const`. Para obter uma lista dos tipos internos, consulte [Tabela de tipos internos](../../language-reference/keywords/built-in-types-table.md). Tipos definidos pelo usuário, incluindo classes, struct e matrizes, não podem ser `const`. Use o modificador [readonly](../../language-reference/keywords/readonly.md) para criar uma classe, struct ou matriz que é inicializada uma vez em tempo de execução (por exemplo, em um construtor) e, assim, não pode ser alterada.  
  
 O C# não dá suporte aos métodos `const`, propriedades ou eventos.  
  
 O tipo de enumeração permite que você defina constantes nomeadas para tipos internos integrais (por exemplo `int`, `uint`, `long` e assim por diante). Para obter mais informações, consulte [enum](../../language-reference/keywords/enum.md).  
  
 As constantes devem ser inicializadas conforme elas são declaradas. Por exemplo:  
  
 [!code-csharp[csProgGuideObjects#64](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#64)]  
  
 Neste exemplo, a constante `months` sempre é 12 e não pode ser alterada até mesmo pela própria classe. Na verdade, quando o compilador encontra um identificador constante no código-fonte C# (por exemplo, `months`), ele substitui o valor literal diretamente no código de IL (linguagem intermediária) que ele produz. Como não há nenhum endereço variável associado a uma constante em tempo de execução, os campos `const` não podem ser passados por referência e não podem aparecer como um l-value em uma expressão.  
  
> [!NOTE]
> Tenha cuidado ao fazer referência a valores constantes definidos em outro código como DLLs. Se uma nova versão da DLL definir um novo valor para a constante, seu programa ainda conterá o valor literal antigo até que ele seja recompilado com a nova versão.  
  
 Várias constantes do mesmo tipo podem ser declaradas ao mesmo tempo, por exemplo:  
  
 [!code-csharp[csProgGuideObjects#65](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#65)]  
  
 A expressão que é usada para inicializar uma constante poderá fazer referência a outra constante se ela não criar uma referência circular. Por exemplo:  
  
 [!code-csharp[csProgGuideObjects#66](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#66)]  
  
 As constantes podem ser marcadas como [public](../../language-reference/keywords/public.md), [private](../../language-reference/keywords/private.md), [protected](../../language-reference/keywords/protected.md), [internal](../../language-reference/keywords/internal.md), [protected internal](../../language-reference/keywords/protected-internal.md) ou [private protected](../../language-reference/keywords/private-protected.md). Esses modificadores de acesso definem como os usuários da classe podem acessar a constante. Para obter mais informações, consulte [Modificadores de Acesso](./access-modifiers.md).  
  
 As constantes são acessadas como se fossem campos [static](../../language-reference/keywords/static.md) porque o valor da constante é o mesmo para todas as instâncias do tipo. Você não usa a palavra-chave `static` para declará-las. As expressões que não estão na classe que define a constante devem usar o nome de classe, um período e o nome da constante para acessar a constante. Por exemplo:  
  
 [!code-csharp[csProgGuideObjects#67](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#67)]  
  
## <a name="c-language-specification"></a>Especificação da Linguagem C#  
 [!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]  
  
## <a name="see-also"></a>Consulte também

- [Guia de Programação em C#](../index.md)
- [Classes e Structs](./index.md)
- [Propriedades](./properties.md)
- [Tipos](../types/index.md)
- [readonly](../../language-reference/keywords/readonly.md)
- [Imutabilidade em C# parte um: tipos de imutabilidade](https://blogs.msdn.microsoft.com/ericlippert/2007/11/13/immutability-in-c-part-one-kinds-of-immutability)
