---
title: Âncoras em expressões regulares do .NET
description: Saiba como usar as âncoras em padrões de expressões regulares.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- atomic zero-width assertions
- regular expressions, anchors
- regular expressions, atomic zero-width assertions
- anchors, in regular expressions
- metacharacters, atomic zero-width assertions
- metacharacters, anchors
- .NET Framework regular expressions, anchors
- .NET Framework regular expressions, atomic zero-width assertions
ms.assetid: 336391f6-2614-499b-8b1b-07a6837108a7
author: rpetrusha
ms.author: ronpet
ms.custom: seodec18
ms.openlocfilehash: 5f722977928604e5876e52a7329eef5c933bf2a7
ms.sourcegitcommit: 581ab03291e91983459e56e40ea8d97b5189227e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2019
ms.locfileid: "70046464"
---
# <a name="anchors-in-regular-expressions"></a>Âncoras em expressões regulares
<a name="top"></a> Âncoras ou asserções atômicas de largura zero, especificam uma posição na cadeia de caracteres em que uma correspondência deve ocorrer. Quando você usa uma âncora na sua expressão de pesquisa, o mecanismo de expressões regulares não avança pela cadeia de caracteres ou consome caracteres, ele procura uma correspondência apenas na posição especificada. Por exemplo, `^` Especifica que a correspondência deve começar no início de uma linha ou cadeia de caracteres. Portanto, a expressão regular `^http:` corresponde a "http:" apenas quando ele ocorre no início de uma linha. A tabela a seguir lista as âncoras com suporte pelas expressões regulares no .NET.  
  
|Âncora|DESCRIÇÃO|  
|------------|-----------------|  
|`^`|Por padrão, a correspondência deve ocorrer no início da cadeia de caracteres. No modo multilinha, deve ocorrer no início da linha. Para saber mais, veja [Início da cadeia de caracteres ou linha](#Start).|  
|`$`|Por padrão, a correspondência deve ocorrer no fim da cadeia de caracteres ou antes de `\n` no fim da cadeia de caracteres. No modo multilinha, deve ocorrer no fim da linha ou antes de `\n` no fim da linha. Para saber mais, veja [Fim da cadeia de caracteres ou linha](#End).|  
|`\A`|A correspondência deve ocorrer no início da cadeia de caracteres apenas (sem suporte a multilinha). Para saber mais, veja [Início da cadeia de caracteres apenas](#StartOnly).|  
|`\Z`|A correspondência deve ocorrer no final da cadeia de caracteres ou antes de `\n` no final da cadeia de caracteres. Para saber mais, veja [Final da Cadeia de Caracteres ou Antes de Terminar Nova Linha](#EndOrNOnly).|  
|`\z`|A correspondência deve ocorrer apenas no final da cadeia de caracteres. Para saber mais, veja [Fim da cadeia de caracteres apenas](#EndOnly).|  
|`\G`|A correspondência deve começar na posição em que a correspondência anterior foi encerrada. Para saber mais, veja [Correspondências contíguas](#Contiguous).|  
|`\b`|A correspondência deve ocorrer em um limite de palavra. Para saber mais, veja [Limite de palavra](#WordBoundary).|  
|`\B`|A correspondência não deve ocorrer em um limite de palavra. Para saber mais, veja [Limite não pertencente a palavras](#NonwordBoundary).|  
  
<a name="Start"></a>   
## <a name="start-of-string-or-line-"></a>Início da Cadeia de Caracteres ou Linha: ^  
 Por padrão, a âncora `^` especifica que o seguinte padrão deve começar na posição do primeiro caractere da cadeia de caracteres. Se você usar `^` com a opção <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType> (veja [Opções de expressões regulares](../../../docs/standard/base-types/regular-expression-options.md)), a correspondência deverá ocorrer no início de cada linha.  
  
 O exemplo a seguir usa a âncora `^` em uma expressão regular que extrai informações sobre os anos durante os quais algumas equipes de profissionais de beisebol existiram. O exemplo chama duas sobrecargas do método <xref:System.Text.RegularExpressions.Regex.Matches%2A?displayProperty=nameWithType>:  
  
- A chamada para a sobrecarga <xref:System.Text.RegularExpressions.Regex.Matches%28System.String%2CSystem.String%29> localiza apenas a primeira subcadeia de caracteres na cadeia de caracteres de entrada que corresponde ao padrão de expressão regular.  
  
- A chamada para a sobrecarga do <xref:System.Text.RegularExpressions.Regex.Matches%28System.String%2CSystem.String%2CSystem.Text.RegularExpressions.RegexOptions%29> com o parâmetro `options` definido como <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType> localiza todas as cinco subcadeias de caracteres.  
  
 [!code-csharp[Conceptual.RegEx.Language.Assertions#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.assertions/cs/startofstring1.cs#1)]
 [!code-vb[Conceptual.RegEx.Language.Assertions#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.assertions/vb/startofstring1.vb#1)]  
  
 O padrão de expressão regular `^((\w+(\s?)){2,}),\s(\w+\s\w+),(\s\d{4}(-(\d{4}|present))?,?)+` é definido conforme mostrado na tabela a seguir.  
  
|Padrão|DESCRIÇÃO|  
|-------------|-----------------|  
|`^`|Começa a correspondência no início da cadeia de caracteres de entrada (ou o início da linha se o método for chamado com a opção <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType>).|  
|`((\w+(\s?)){2,}`|Corresponde a um ou mais caracteres de palavra seguidos por zero ou um espaço, pelo menos, duas vezes. Este é o primeiro grupo de captura. Esta expressão também define um segundo e um terceiro grupo de captura: O segundo consiste na palavra capturada e o terceiro consiste no espaço em branco capturado.|  
|`,\s`|Corresponde a uma vírgula seguida por um caractere de espaço em branco.|  
|`(\w+\s\w+)`|Corresponde a um ou mais caracteres de palavra seguidos por um espaço, seguido por um ou mais caracteres de palavra. Este é o quarto grupo de captura.|  
|`,`|Corresponde a uma vírgula.|  
|`\s\d{4}`|Corresponde a um espaço seguido por quatro dígitos decimais.|  
|<code>(-(\d{4}&#124;present))?</code>|Corresponde a zero ou uma ocorrência de um hífen seguido por quatro dígitos decimais ou a cadeia de caracteres "present". Este é o sexto grupo de captura. Ele também inclui um sétimo grupo de captura.|  
|`,?`|Corresponde a zero ou uma ocorrência de uma vírgula.|  
|<code>(\s\d{4}(-(\d{4}&#124;present))?,?)+</code>|Corresponde a uma ou mais ocorrências do seguinte: um espaço, quatro dígitos decimais, zero ou uma ocorrência de um hífen seguido por quatro dígitos decimais ou a cadeia de caracteres "present" e zero ou uma vírgula. Este é o quinto grupo de captura.|  
  
 [Voltar ao início](#top)  
  
<a name="End"></a>   
## <a name="end-of-string-or-line-"></a>Final da Cadeia de Caracteres ou da Linha: $  
 A âncora `$` especifica que o padrão anterior deve ocorrer no final da cadeia de caracteres de entrada ou antes de `\n` no final da cadeia de caracteres de entrada.  
  
 Se você usar `$` com a opção <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType>, a correspondência também pode ocorrer no final de uma linha. Observe que `$` corresponde a `\n`, mas não corresponde a `\r\n` (a combinação de caracteres de nova linha e de retorno de carro ou CR/LF). De acordo com a combinação de caracteres CR/LF, inclua `\r?$` no padrão de expressão regular.  
  
 O exemplo a seguir adiciona a âncora `$` ao padrão de expressão regular usada no exemplo na seção [Início da cadeia de caracteres ou linha](#Start). Quando usado com a cadeia de caracteres de entrada original, que inclui cinco linhas de texto, o método <xref:System.Text.RegularExpressions.Regex.Matches%28System.String%2CSystem.String%29?displayProperty=nameWithType> não pode localizar uma correspondência, pois o final da primeira linha não corresponde ao padrão `$`. Quando a cadeia de caracteres de entrada original é dividida em uma matriz de cadeia de caracteres, o método <xref:System.Text.RegularExpressions.Regex.Matches%28System.String%2CSystem.String%29?displayProperty=nameWithType> obtém sucesso na correspondência de cada uma das cinco linhas. Quando o método <xref:System.Text.RegularExpressions.Regex.Matches%28System.String%2CSystem.String%2CSystem.Text.RegularExpressions.RegexOptions%29?displayProperty=nameWithType> for chamado com o parâmetro `options` definido como <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType>, nenhuma correspondência será encontrada porque o padrão de expressão regular não considera o elemento de retorno de carro (\u+000D). No entanto, quando o padrão de expressão regular é modificado substituindo `$` por `\r?$`, chamar o método <xref:System.Text.RegularExpressions.Regex.Matches%28System.String%2CSystem.String%2CSystem.Text.RegularExpressions.RegexOptions%29?displayProperty=nameWithType> com o parâmetro `options` definido como <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType> novamente encontra cinco correspondências.  
  
 [!code-csharp[Conceptual.RegEx.Language.Assertions#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.assertions/cs/endofstring1.cs#2)]
 [!code-vb[Conceptual.RegEx.Language.Assertions#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.assertions/vb/endofstring1.vb#2)]  
  
 [Voltar ao início](#top)  
  
<a name="StartOnly"></a>   
## <a name="start-of-string-only-a"></a>Apenas Início da Cadeia de Caracteres: \A  
 A âncora `\A` especifica que uma correspondência deve ocorrer no início da cadeia de caracteres de entrada. Ela é idêntica à âncora `^`, exceto que `\A` ignora a opção <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType>. Portanto, ela pode corresponder apenas ao início da primeira linha em uma cadeia de caracteres de entrada multilinhas.  
  
 O exemplo a seguir é semelhante aos exemplos das âncoras `^` e `$`. Ele usa a âncora `\A` em uma expressão regular que extrai informações sobre os anos durante os quais algumas equipes de profissionais de beisebol existiram. A cadeia de caracteres de entrada inclui cinco linhas. A chamada para o método <xref:System.Text.RegularExpressions.Regex.Matches%28System.String%2CSystem.String%2CSystem.Text.RegularExpressions.RegexOptions%29?displayProperty=nameWithType> localiza apenas a primeira subcadeia de caracteres na cadeia de caracteres de entrada que corresponde ao padrão de expressão regular. Como o exemplo mostra, a opção <xref:System.Text.RegularExpressions.RegexOptions.Multiline> não tem efeito.  
  
 [!code-csharp[Conceptual.RegEx.Language.Assertions#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.assertions/cs/startofstring2.cs#3)]
 [!code-vb[Conceptual.RegEx.Language.Assertions#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.assertions/vb/startofstring2.vb#3)]  
  
 [Voltar ao início](#top)  
  
<a name="EndOrNOnly"></a>   
## <a name="end-of-string-or-before-ending-newline-z"></a>Final da Cadeia de Caracteres ou Antes de Terminar Nova Linha: \ Z  
 A âncora `\Z` especifica que a correspondência deve ocorrer no final da cadeia de caracteres de entrada ou antes de `\n` no final da cadeia de caracteres de entrada. Ela é idêntica à âncora `$`, exceto que `\Z` ignora a opção <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType>. Portanto, em uma cadeia de caracteres multilinhas, ela pode corresponder apenas ao final da última linha ou à última linha antes de `\n`.  
  
 Observe que `\Z` corresponde a `\n` mas não corresponde a `\r\n` (a combinação de caracteres CR/LF). Para corresponder a CR/LF, inclua `\r?\Z` no padrão da expressão regular.  
  
 O exemplo a seguir usa a âncora `\Z` em uma expressão regular semelhante ao exemplo na seção [Início da Cadeia de Caracteres ou Linha](#Start), que extrai informações sobre os anos durante os quais algumas equipes de profissionais de beisebol existiram. A subexpressão `\r?\Z` na expressão regular `^((\w+(\s?)){2,}),\s(\w+\s\w+),(\s\d{4}(-(\d{4}|present))?,?)+\r?\Z` corresponde ao final de uma cadeia de caracteres e também corresponde a uma cadeia de caracteres que termina com `\n` ou `\r\n`. Como resultado, cada elemento da matriz corresponde ao padrão da expressão regular.  
  
 [!code-csharp[Conceptual.RegEx.Language.Assertions#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.assertions/cs/endofstring2.cs#4)]
 [!code-vb[Conceptual.RegEx.Language.Assertions#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.assertions/vb/endofstring2.vb#4)]  
  
 [Voltar ao início](#top)  
  
<a name="EndOnly"></a>   
## <a name="end-of-string-only-z"></a>Apenas Final da Cadeia de Caracteres: \ z  
 A âncora `\z` especifica que uma correspondência deve ocorrer no final da cadeia de caracteres de entrada. Como o elemento de linguagem `$`, o `\z` ignora a opção <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType>. Diferentemente do elemento de linguagem `\Z`, `\z` não corresponde ao caractere `\n` no final de uma cadeia de caracteres. Portanto, ele pode corresponder somente à última linha da cadeia de caracteres de entrada.  
  
 O exemplo a seguir usa a âncora `\z` em uma expressão regular que é de outro modo idêntica ao exemplo na seção anterior, que extrai informações sobre os anos durante os quais algumas equipes de profissionais de beisebol existiram. O exemplo tenta corresponder cada um dos cinco elementos em uma matriz de cadeia de caracteres com um padrão de expressão regular `^((\w+(\s?)){2,}),\s(\w+\s\w+),(\s\d{4}(-(\d{4}|present))?,?)+\r?\z`. Duas das cadeias de caracteres terminam com os caracteres de alimentação de linha e retorno de carro, uma termina com um caractere de alimentação de linha e duas terminam sem um caractere de retorno de carro nem um caractere de alimentação de linha. Como a saída mostra, apenas as cadeias de caracteres sem um caractere de retorno de carro ou de alimentação de linha correspondem ao padrão.  
  
 [!code-csharp[Conceptual.RegEx.Language.Assertions#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.assertions/cs/endofstring3.cs#5)]
 [!code-vb[Conceptual.RegEx.Language.Assertions#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.assertions/vb/endofstring3.vb#5)]  
  
 [Voltar ao início](#top)  
  
<a name="Contiguous"></a>   
## <a name="contiguous-matches-g"></a>Correspondências Contíguas: \ G  
 A âncora `\G` especifica que uma correspondência deve ocorrer no ponto em que a correspondência anterior foi encerrada. Quando você usa essa âncora com o método <xref:System.Text.RegularExpressions.Regex.Matches%2A?displayProperty=nameWithType> ou <xref:System.Text.RegularExpressions.Match.NextMatch%2A?displayProperty=nameWithType>, ela garante que todas as correspondências são contíguas.  
  
 O exemplo a seguir usa uma expressão regular para extrair os nomes de espécies de roedores de uma cadeia de caracteres delimitada por vírgulas.  
  
 [!code-csharp[Conceptual.RegEx.Language.Assertions#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.assertions/cs/contiguous1.cs#6)]
 [!code-vb[Conceptual.RegEx.Language.Assertions#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.assertions/vb/contiguous1.vb#6)]  
  
 A expressão regular `\G(\w+\s?\w*),?` é interpretada conforme mostrado na tabela a seguir.  
  
|Padrão|DESCRIÇÃO|  
|-------------|-----------------|  
|`\G`|Começa onde a última correspondência terminou.|  
|`\w+`|Corresponde a um ou mais caracteres de palavra.|  
|`\s?`|Corresponde a zero ou um espaço.|  
|`\w*`|Corresponder a zero ou mais caracteres de palavra.|  
|`(\w+\s?\w*)`|Corresponde a um ou mais caracteres de palavra seguidos por zero ou um espaço, seguido por zero ou mais caracteres de palavra. Este é o primeiro grupo de captura.|  
|`,?`|Corresponde a zero ou uma ocorrência de um caractere de vírgula literal.|  
  
 [Voltar ao início](#top)  
  
<a name="WordBoundary"></a>   
## <a name="word-boundary-b"></a>Limite de Palavra: \b  
 A âncora `\b` especifica que a correspondência deve ocorrer em um limite entre um caractere de palavra (o elemento de linguagem `\w`) e um caractere não pertencente a palavras (o elemento de linguagem `\W`). Os caracteres de palavra consistem em caracteres alfanuméricos e sublinhados. Um caractere não pertencente a palavras é qualquer caractere que não seja alfanumérico ou um sublinhado. (Para saber mais, veja [Classes de caracteres](../../../docs/standard/base-types/character-classes-in-regular-expressions.md).) A correspondência também pode ocorrer em um limite de palavra no início ou no final da cadeia de caracteres.  
  
 A âncora `\b` frequentemente é usada para garantir que uma subexpressão corresponda a uma palavra inteira, em vez de apenas ao início ou final de uma palavra. A expressão regular `\bare\w*\b` no exemplo a seguir ilustra esse uso. Ela corresponde a qualquer palavra que comece com a subcadeia de caracteres "are". A saída do exemplo também ilustra que `\b` corresponde ao início e ao final da cadeia de caracteres de entrada.  
  
 [!code-csharp[Conceptual.RegEx.Language.Assertions#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.assertions/cs/word1.cs#7)]
 [!code-vb[Conceptual.RegEx.Language.Assertions#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.assertions/vb/word1.vb#7)]  
  
 O padrão da expressão regular é interpretado conforme a tabela a seguir.  
  
|Padrão|DESCRIÇÃO|  
|-------------|-----------------|  
|`\b`|Começa a correspondência em um limite de palavra.|  
|`are`|Corresponde à subcadeia de caracteres “are”.|  
|`\w*`|Corresponder a zero ou mais caracteres de palavra.|  
|`\b`|Termina a correspondência em um limite de palavra.|  
  
 [Voltar ao início](#top)  
  
<a name="NonwordBoundary"></a>   
## <a name="non-word-boundary-b"></a>Limite de Não Palavra: \B  
 A âncora `\B` especifica que a correspondência não deve ocorrer em um limite de palavra. É o oposto da âncora `\b`.  
  
 O exemplo a seguir usa a âncora `\B` para localizar ocorrências da subcadeia de caracteres "qu" em uma palavra. O padrão de expressão regular `\Bqu\w+` corresponde a uma subcadeia de caracteres que começa com um "qu" que não inicia uma palavra e que continua até o final da palavra.  
  
 [!code-csharp[Conceptual.RegEx.Language.Assertions#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.assertions/cs/nonword1.cs#8)]
 [!code-vb[Conceptual.RegEx.Language.Assertions#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.assertions/vb/nonword1.vb#8)]  
  
 O padrão da expressão regular é interpretado conforme a tabela a seguir.  
  
|Padrão|DESCRIÇÃO|  
|-------------|-----------------|  
|`\B`|Não começa a correspondência em um limite de palavra.|  
|`qu`|Corresponde à subcadeia de caracteres “qu”.|  
|`\w+`|Corresponde a um ou mais caracteres de palavra.|  
  
## <a name="see-also"></a>Consulte também

- [Linguagem de expressão regular – referência rápida](../../../docs/standard/base-types/regular-expression-language-quick-reference.md)
- [Opções de expressões regulares](../../../docs/standard/base-types/regular-expression-options.md)
