---
title: Constructos de alternância em expressões regulares do .NET
description: Saiba como usar constructos de alternância para a correspondência condicional em expressões regulares.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- either/or matching
- alternative matching patterns
- regular expressions, alternation constructs
- alternation constructs
- optional matching patterns
- constructs, alternation
- .NET Framework regular expressions, alternation constructs
ms.assetid: 071e22e9-fbb0-4ecf-add1-8d2424f9f2d1
author: rpetrusha
ms.author: ronpet
ms.custom: seodec18
ms.openlocfilehash: 560597770d667cf8c7668bf2338ac4bac3eb192f
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69968568"
---
# <a name="alternation-constructs-in-regular-expressions"></a>Construtores de alternância em expressões regulares
<a name="top"></a> Os constructos de alternância modificam uma expressão regular para permitir uma correspondência condicional ou do tipo um/ou outro. O .NET dá suporte a três constructos de alternância:  
  
- [Correspondência de padrões com &#124;](#Either_Or)  
  
- [Correspondência condicional com (?(expression)yes&#124;no)](#Conditional_Expr)  
  
- [Correspondência condicional com base em um grupo capturado válido](#Conditional_Group)  
  
<a name="Either_Or"></a>   
## <a name="pattern-matching-with-124"></a>Correspondência de padrões com &#124;  
 Você pode usar o caractere de barra vertical (`|`) para corresponder a qualquer um de uma série de padrões, no qual o caractere `|` separa cada padrão.  
  
 Como a classe de caracteres positivos, o caractere `|` pode ser usado para corresponder a qualquer um de vários caracteres únicos. O exemplo a seguir usa uma classe de caracteres positivos e um padrão do tipo um/ou outro correspondendo ao caractere `|` para localizar ocorrências das palavras “gray” ou “grey” em uma cadeia de caracteres. Nesse caso, o caractere `|` produz uma expressão regular que é mais detalhada.  
  
 [!code-csharp[RegularExpressions.Language.Alternation#1](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.alternation/cs/alternation1.cs#1)]
 [!code-vb[RegularExpressions.Language.Alternation#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.alternation/vb/alternation1.vb#1)]  
  
 A expressão regular que usa o caractere `|`, `\bgr(a|e)y\b` é interpretada conforme mostrado na tabela a seguir.  
  
|Padrão|DESCRIÇÃO|  
|-------------|-----------------|  
|`\b`|Iniciar em um limite de palavra.|  
|`gr`|Corresponder aos caracteres "gr".|  
|<code>(a&#124;e)</code>|Corresponder a um "a" ou "e".|  
|`y\b`|Corresponder a um “y” em um limite de palavra.|  
  
 O caractere `|` também pode ser usado para executar uma correspondência do tipo um/ou outro com vários caracteres ou subexpressões, que podem incluir qualquer combinação de literais de caracteres e elementos de linguagem de expressão regular. (A classe de caracteres não fornece essa funcionalidade.) O exemplo a seguir usa o caractere `|` para extrair um SSN (cadastro de pessoas físicas) dos EUA, que é um número de 9 dígitos com o formato *ddd*-*dd*-*dddd* ou um EIN (Número de Identificação de Empregador) dos EUA, que é um número de 9 dígitos com o formato *dd*-*ddddddd*.  
  
 [!code-csharp[RegularExpressions.Language.Alternation#2](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.alternation/cs/alternation2.cs#2)]
 [!code-vb[RegularExpressions.Language.Alternation#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.alternation/vb/alternation2.vb#2)]  
  
 A expressão regular `\b(\d{2}-\d{7}|\d{3}-\d{2}-\d{4})\b` é interpretada conforme mostrado na tabela a seguir.  
  
|Padrão|DESCRIÇÃO|  
|-------------|-----------------|  
|`\b`|Iniciar em um limite de palavra.|  
|<code>(\d{2}-\d{7}&#124;\d{3}-\d{2}-\d{4})</code>|Corresponde a uma das seguintes opções: dois dígitos decimais seguidos por um hífen seguido por sete dígitos decimais ou três dígitos decimais, um hífen, dois dígitos decimais, outro hífen e quatro dígitos decimais.|  
|`\d`|Termina a correspondência em um limite de palavra.|  
  
 [Voltar ao início](#top)  
  
<a name="Conditional_Expr"></a>   
## <a name="conditional-matching-with-an-expression"></a>Correspondência condicional com uma expressão  
 Este elemento de linguagem tenta corresponder a um dos dois padrões dependendo de se ele pode corresponder a um padrão inicial. A sintaxe é:  
  
 `(?(` *expressão* `)` *sim* `|` *não* `)`  
  
 em que *expression* é o padrão inicial para correspondência, *yes* é o padrão para correspondência se *expression* for correspondida e *no* é o padrão opcional para correspondência se *expression* não for correspondida. O mecanismo de expressões regulares trata a *expressão* como uma asserção de largura zero, isto é, o mecanismo de expressões regulares não avança no fluxo de entrada após avaliar a *expressão*. Portanto, esse constructo é equivalente ao seguinte:  
  
 `(?(?=` *expressão* `)` *sim* `|` *não* `)`  
  
 em que `(?=`*expression*`)` é um constructo de asserção de largura zero. (Para saber mais, confira [Constructos de agrupamento](../../../docs/standard/base-types/grouping-constructs-in-regular-expressions.md)). Como o mecanismo de expressões regulares interpreta *expression* como uma âncora (uma asserção de largura zero), *expression* deve ser uma asserção de largura zero (para obter mais informações, confira [Âncoras](../../../docs/standard/base-types/anchors-in-regular-expressions.md)) ou uma subexpressão que também está contida em *Yes*. Caso contrário, o padrão *sim* não pode ser correspondido.  
  
> [!NOTE]
> Se *expression* for um grupo de captura nomeado ou numerado, o constructo de alternância é interpretado como um teste de captura. Para saber mais, confira a seção a seguir [Correspondência condicional com base em um grupo capturado válido](#Conditional_Group). Em outras palavras, o mecanismo de expressões regulares não tenta corresponder a subcadeia de caracteres capturada, mas em vez disso, testa a presença ou ausência do grupo.  
  
 O exemplo a seguir é uma variação do exemplo que aparece na seção [E/Ou Correspondência de Padrões com &#124;](#Either_Or). Ele usa a correspondência condicional para determinar se os três primeiros caracteres após um limite de palavra são dois dígitos seguidos por um hífen. Se forem, ele tenta corresponder a um EIN (Número de Identificação de Empregador) dos EUA. Se não, ele tenta corresponder a um SSN (cadastro de pessoas físicas).  
  
 [!code-csharp[RegularExpressions.Language.Alternation#3](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.alternation/cs/alternation3.cs#3)]
 [!code-vb[RegularExpressions.Language.Alternation#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.alternation/vb/alternation3.vb#3)]  
  
 O padrão da expressão regular `\b(?(\d{2}-)\d{2}-\d{7}|\d{3}-\d{2}-\d{4})\b` é interpretado conforme mostrado na tabela a seguir.  
  
|Padrão|DESCRIÇÃO|  
|-------------|-----------------|  
|`\b`|Iniciar em um limite de palavra.|  
|`(?(\d{2}-)`|Determinar se os próximos três caracteres são compostos por dois dígitos seguidos por um hífen.|  
|`\d{2}-\d{7}`|Se o padrão anterior corresponder, corresponder a dois dígitos seguidos por um hífen seguido por sete dígitos.|  
|`\d{3}-\d{2}-\d{4}`|Se o padrão anterior não corresponder, corresponder a três dígitos decimais, um hífen, dois dígitos decimais, outro hífen e quatro dígitos decimais.|  
|`\b`|Corresponder a um limite de palavra.|  
  
 [Voltar ao início](#top)  
  
<a name="Conditional_Group"></a>   
## <a name="conditional-matching-based-on-a-valid-captured-group"></a>Correspondência Condicional com Base em um Grupo Capturado Válido  
 Este elemento de linguagem tenta corresponder a um dos dois padrões dependendo de se ele correspondeu a um grupo de captura especificado. A sintaxe é:  
  
 `(?(` *nome* `)` *sim* `|` *não* `)`  
  
 ou  
  
 `(?(` *number* `)` *yes* `|` *no* `)`  
  
 em que *name* é o nome e *number* é o número de um grupo de captura, *yes* é a expressão para correspondência se *name* ou *number* tiver uma correspondência e *no* for a expressão opcional para correspondência se não houver uma.  
  
 Se *name* não corresponder ao nome de um grupo de captura que é usado no padrão de expressão regular, o constructo de alternância será interpretado como teste de expressão, conforme explicado na seção anterior. Normalmente, isso significa que *expression* é avaliada como `false`. Se *number* não corresponder a um grupo de captura numerado que é usado no padrão de expressão regular, o mecanismo de expressões regulares gerará um <xref:System.ArgumentException>.  
  
 O exemplo a seguir é uma variação do exemplo que aparece na seção [E/Ou Correspondência de Padrões com &#124;](#Either_Or). Ele usa um grupo de captura chamado `n2` que consiste em dois dígitos seguidos por um hífen. O constructo de alternância testa se este grupo de captura foi correspondido na cadeia de caracteres de entrada. Em caso afirmativo, o constructo de alternância tenta corresponder aos sete últimos dígitos de um EIN (Número de Identificação de Empregador) dos EUA. Se não tiver, ele tenta corresponder a um SSN (cadastro de pessoas físicas).  
  
 [!code-csharp[RegularExpressions.Language.Alternation#4](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.alternation/cs/alternation4.cs#4)]
 [!code-vb[RegularExpressions.Language.Alternation#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.alternation/vb/alternation4.vb#4)]  
  
 O padrão da expressão regular `\b(?<n2>\d{2}-)?(?(n2)\d{7}|\d{3}-\d{2}-\d{4})\b` é interpretado conforme mostrado na tabela a seguir.  
  
|Padrão|DESCRIÇÃO|  
|-------------|-----------------|  
|`\b`|Iniciar em um limite de palavra.|  
|`(?<n2>\d{2}-)?`|Corresponder a zero ou uma ocorrência de dois dígitos seguidos por um hífen. Atribua um nome ao grupo de captura `n2`.|  
|`(?(n2)`|Testar se `n2` foi correspondido na cadeia de caracteres de entrada.|  
|`\d{7}`|Se `n2` tiver sido correspondido, corresponder a sete dígitos decimais.|  
|<code>&#124;\d{3}-\d{2}-\d{4}</code>|Se `n2` não tiver sido correspondido, corresponder a três dígitos decimais, um hífen, dois dígitos decimais, outro hífen e quatro dígitos decimais.|  
|`\b`|Corresponder a um limite de palavra.|  
  
 Uma variação desse exemplo que usa um grupo numerado em vez de um grupo nomeado é mostrada no exemplo a seguir. O padrão da expressão regular é `\b(\d{2}-)?(?(1)\d{7}|\d{3}-\d{2}-\d{4})\b`.  
  
 [!code-csharp[RegularExpressions.Language.Alternation#5](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.alternation/cs/alternation5.cs#5)]
 [!code-vb[RegularExpressions.Language.Alternation#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.alternation/vb/alternation5.vb#5)]  
  
## <a name="see-also"></a>Consulte também

- [Linguagem de expressão regular – referência rápida](../../../docs/standard/base-types/regular-expression-language-quick-reference.md)
