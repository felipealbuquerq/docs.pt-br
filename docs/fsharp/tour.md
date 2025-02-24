---
title: Tour do F#
description: Examine alguns dos principais recursos da linguagem em que este tour com exemplos de código de programação F#.
ms.date: 11/06/2018
ms.openlocfilehash: 35b811b580cd7c3b4a620f45b602150a92479052
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68630062"
---
# <a name="tour-of-f"></a>Tour de F\#

É a melhor maneira de saber mais sobre o F# ler e gravar o código F#. Este artigo atuará como um tour por alguns dos principais recursos da linguagem F# e lhe dar alguns trechos de código que pode ser executado em seu computador. Para saber mais sobre como configurar um ambiente de desenvolvimento, confira [introdução](./tutorials/getting-started/index.md).

Há dois conceitos principais em F#: tipos e funções.  Este tour enfatiza os recursos do idioma que se enquadram nesses dois conceitos.

## <a name="executing-the-code-online"></a>Executando o código online

Se você não tiver F# instalado em seu computador, poderá executar todos os exemplos em seu navegador com [ F# try no Webassembly](https://tryfsharp.fsbolero.io/). Fable é um dialeto do F# que é executado diretamente no seu navegador. Para exibir os exemplos que seguem no repl, confira os **exemplos > saiba > Tour do F#**  na barra de menus à esquerda do Fable repl.

## <a name="functions-and-modules"></a>Funções e módulos

São as partes mais fundamentais de qualquer programa em F# ***funções*** organizados em ***módulos***.  [Funções](./language-reference/functions/index.md) realizar o trabalho em entradas para produzir saídas, e eles estão organizados sob [módulos](./language-reference/modules.md), que são a principal maneira de agrupar as coisas em F#.  Eles são definidos usando a [ `let` Associação](./language-reference/functions/let-bindings.md), que atribui um nome à função e define seus argumentos.

[!code-fsharp[BasicFunctions](../../samples/snippets/fsharp/tour.fs#L101-L133)]

`let`as associações também são como você associa um valor a um nome, semelhante a uma variável em outros idiomas.  `let`as associações são ***imutáveis*** por padrão, o que significa que, uma vez que um valor ou uma função esteja associado a um nome, ela não poderá ser alterada no local.  Isso está em contraste com variáveis em outras linguagens, que são ***mutáveis***, o que significa que seus valores podem ser alterados em qualquer ponto no tempo.  Se você precisar de uma associação mutável, poderá usar `let mutable ...` a sintaxe.

[!code-fsharp[Immutability](../../samples/snippets/fsharp/tour.fs#L75-L94)]

## <a name="numbers-booleans-and-strings"></a>Números, Boolianos e cadeias de caracteres

Como uma linguagem .NET, F# oferece suporte à mesma subjacente [tipos primitivos](./language-reference/primitive-types.md) que existem no .NET.

Aqui está como vários tipos numéricos são representados em F#:

[!code-fsharp[Numbers](../../samples/snippets/fsharp/tour.fs#L49-L68)]

Veja quais valores Boolianos e a lógica condicional básica se assemelham a:

[!code-fsharp[Bools](../../samples/snippets/fsharp/tour.fs#L142-L152)]

E aqui está a aparência da manipulação de [cadeia de caracteres](./language-reference/strings.md) básica:

[!code-fsharp[Strings](../../samples/snippets/fsharp/tour.fs#L158-L180)]

## <a name="tuples"></a>Tuplas

[As tuplas](./language-reference/tuples.md) são muito importante em F#.  Eles são um agrupamento de valores não nomeados, mas ordenados, que podem ser tratados como próprios valores.  Considere-os como valores que são agregados de outros valores.  Eles têm muitos usos, como, convenientemente, retornar vários valores de uma função ou agrupar valores para uma conveniência ad hoc.

[!code-fsharp[Tuples](../../samples/snippets/fsharp/tour.fs#L186-L203)]

A partir do F# 4.1, você também pode criar `struct` tuplas.  Eles também interoperam totalmente com o c# 7/Visual Basic 15 tuplas, que `struct` também são tuplas:

[!code-fsharp[Tuples](../../samples/snippets/fsharp/tour.fs#L205-L218)]

É importante observar que, como `struct` as tuplas são tipos de valor, elas não podem ser convertidas implicitamente em tuplas de referência ou vice-versa.  Você deve converter explicitamente entre uma tupla de referência e struct.

## <a name="pipelines-and-composition"></a>Pipelines e composição

Redirecionar operadores como `|>` são usadas amplamente durante o processamento de dados em F#. Esses operadores são funções que permitem que você estabeleça "pipelines" de funções de maneira flexível. O exemplo a seguir percorre como você pode aproveitar esses operadores para criar um pipeline funcional simples:

[!code-fsharp[Pipelines](../../samples/snippets/fsharp/tour.fs#L227-L282)]

O exemplo anterior feito uso de muitos recursos do F#, incluindo funções de processamento de lista, funções de primeira classe, e [aplicação parcial](./language-reference/functions/index.md#partial-application-of-arguments). Embora uma compreensão profunda de cada um desses conceitos possa se tornar um pouco avançada, é claro que a facilidade das funções pode ser usada para processar dados ao criar pipelines.

## <a name="lists-arrays-and-sequences"></a>Listas, matrizes e sequências

Listas, matrizes e sequências são três tipos de coleção principal na biblioteca de núcleo do F#.

[Listas](./language-reference/lists.md) são coleções ordenadas e imutáveis de elementos do mesmo tipo.  Elas são listas vinculadas individualmente, o que significa que elas são destinadas à enumeração, mas uma opção ruim para acesso aleatório e concatenação se forem grandes.  Isso em contraste com listas em outras linguagens populares, que normalmente não usam uma lista vinculada individualmente para representar listas.

[!code-fsharp[Lists](../../samples/snippets/fsharp/tour.fs#L309-L359)]

As [matrizes](./language-reference/arrays.md) são de tamanho fixo e de coleções *mutáveis* de elementos do mesmo tipo.  Eles oferecem suporte ao acesso aleatório rápido de elementos e são mais rápidos do que listas do F# porque eles são contíguos apenas blocos de memória.

[!code-fsharp[Arrays](../../samples/snippets/fsharp/tour.fs#L368-L407)]

As [sequências](./language-reference/sequences.md) são uma série lógica de elementos, todos do mesmo tipo.  Esses são um tipo mais geral do que listas e matrizes, capazes de ser sua "exibição" em qualquer série lógica de elementos.  Elas também se destacam porque podem ser ***lentas***, o que significa que os elementos podem ser computados somente quando forem necessários.

[!code-fsharp[Sequences](../../samples/snippets/fsharp/tour.fs#L418-L452)]

## <a name="recursive-functions"></a>Funções Recursivas

Processamento de coleções ou sequências de elementos normalmente é feito com [recursão](./language-reference/functions/index.md#recursive-functions) em F#.  Embora o F# tem suporte para loops e programação imperativa, a recursão é preferencial porque é mais fácil de garantir a correção.

> [!NOTE]
> O exemplo a seguir usa a correspondência de padrões por meio `match` da expressão.  Esse constructo fundamental é abordado posteriormente neste artigo.

[!code-fsharp[RecursiveFunctions](../../samples/snippets/fsharp/tour.fs#L461-L500)]

F# também tem suporte completo para a otimização de chamada Tail, que é uma maneira de otimizar chamadas recursivas, para que eles são simplesmente tão rápidos quanto um constructo de loop.

## <a name="record-and-discriminated-union-types"></a>Tipos de União discriminados e de registro

Registro e tipos de união são dois tipos de dados fundamental usados no código F# e geralmente são a melhor maneira de representar dados em um programa em F#.  Embora isso o torne semelhante às classes em outras linguagens, uma de suas principais diferenças é que elas têm semânticas de igualdade estrutural.  Isso significa que eles são "nativamente" comparáveis e a igualdade é simples, basta verificar se um é igual ao outro.

Os [registros](./language-reference/records.md) são uma agregação de valores nomeados, com membros opcionais (como métodos).  Se você estiver familiarizado com C# o ou o Java, eles devem se sentir semelhantes a pocos ou POJOs, apenas com igualdade estrutural e menos cerimônias.

[!code-fsharp[Records](../../samples/snippets/fsharp/tour.fs#L507-L559)]

A partir do F# 4.1, você também pode representar os registros como `struct`s.  Isso é feito com o `[<Struct>]` atributo:

[!code-fsharp[Records](../../samples/snippets/fsharp/tour.fs#L561-L568)]

[Uniões discriminadas (UDS)](./language-reference/discriminated-unions.md) são valores que podem ser um número de formulários ou casos nomeados.  Os dados armazenados no tipo podem ser um dos vários valores distintos.

[!code-fsharp[Unions](../../samples/snippets/fsharp/tour.fs#L575-L631)]

Você também pode usar UDS como *uniões discriminadas por um único caso*, para ajudar com a modelagem de domínio em tipos primitivos.  Muitas vezes, cadeias de caracteres e outros tipos primitivos são usados para representar algo e, portanto, recebem um significado específico.  No entanto, usar apenas a representação primitiva dos dados pode resultar na atribuição incorreta de um valor incorreto!  Representar cada tipo de informação como uma União de caso único distinta pode impor a exatidão nesse cenário.

[!code-fsharp[Unions](../../samples/snippets/fsharp/tour.fs#L633-L654)]

Como demonstra o exemplo acima, para obter o valor subjacente em uma união discriminada de um único caso, você deve desencapsulá-lo explicitamente.

Além disso, o UDS também oferece suporte a definições recursivas, permitindo que você represente árvores e dados recursivos inerentemente.  Por exemplo, veja como você pode representar uma árvore de pesquisa binária com `exists` o `insert` e o functions.

[!code-fsharp[Unions](../../samples/snippets/fsharp/tour.fs#L656-L683)]

Como UDS permite que você represente a estrutura recursiva da árvore no tipo de dados, operar nessa estrutura recursiva é simples e garante a exatidão.  Também há suporte para a correspondência de padrões, conforme mostrado abaixo.

Além disso, você pode representar UDS `struct`como s com `[<Struct>]` o atributo:

[!code-fsharp[Unions](../../samples/snippets/fsharp/tour.fs#L685-L696)]

No entanto, há duas coisas importantes para ter em mente ao fazer isso:

1. Uma struct DU não pode ser definida recursivamente.
2. Um struct DU deve ter nomes exclusivos para cada um de seus casos.

A falha em seguir os itens acima resultará em um erro de compilação.

## <a name="pattern-matching"></a>Correspondência padrão

[Correspondência de padrão](./language-reference/pattern-matching.md) é o recurso da linguagem F# que permite que a correção para operar em tipos de F#.  Nos exemplos acima, você provavelmente observou um pouco de `match x with ...` sintaxe.  Esse constructo permite ao compilador, que pode entender a "forma" de tipos de dados, para forçá-lo a considerar todos os casos possíveis ao usar um tipo de dados por meio do que é conhecido como correspondência de padrão exaustiva.  Isso é incrivelmente eficiente para a exatidão e pode ser usado de forma inteligente para "levantar" o que normalmente seria uma preocupação com o tempo de execução na hora da compilação.

[!code-fsharp[PatternMatching](../../samples/snippets/fsharp/tour.fs#L705-L742)]

Algo que você talvez tenha notado é o uso do `_` padrão.  Isso é conhecido como o [padrão curinga](./language-reference/pattern-matching.md#wildcard-pattern), que é uma maneira de dizer "não me importo com o que algo é".  Embora seja conveniente, você pode ignorar acidentalmente a correspondência de padrões exaustivos e não se beneficiará mais de imposição de tempo de compilação `_`se não tiver cuidado ao usar o.  Ela é melhor usada quando você não se importa com determinadas partes de um tipo decomposto quando a correspondência de padrões, ou a cláusula final, quando você tiver enumerado todos os casos significativos em uma expressão de correspondência de padrões.

[Padrões ativos](./language-reference/active-patterns.md) são outra construção avançada a ser usada com correspondência de padrões.  Eles permitem que você particione dados de entrada em formulários personalizados, decompondo-os no local de chamada de correspondência de padrões.  Eles também podem ser parametrizados, permitindo, portanto, definir a partição como uma função.  Expandir o exemplo anterior para dar suporte a padrões ativos tem uma aparência semelhante a esta:

[!code-fsharp[ActivePatterns](../../samples/snippets/fsharp/tour.fs#L764-L786)]

## <a name="optional-types"></a>Tipos opcionais

Um caso especial de tipos de união discriminada é o tipo de opção, que é tão útil que é uma parte da biblioteca principal F#.

[O tipo de opção](./language-reference/options.md) é um tipo que representa um dos dois casos: um valor ou nada.  Ele é usado em qualquer cenário em que um valor pode ou não ser resultado de uma operação específica.  Isso força você a considerar os dois casos, tornando-o uma preocupação em tempo de compilação em vez de uma preocupação em tempo de execução.  Em vez disso, eles são usados `null` em APIs em que o é usado para representar "nada", eliminando assim `NullReferenceException` a necessidade de se preocupar em muitas circunstâncias.

[!code-fsharp[Options](../../samples/snippets/fsharp/tour.fs#L789-L814)]

## <a name="units-of-measure"></a>Unidades de medida

Um recurso exclusivo do sistema de tipos do F# é a capacidade de fornecer contexto para literais numéricos por meio de unidades de medida.

As [unidades de medida](./language-reference/units-of-measure.md) permitem associar um tipo numérico a uma unidade, como medidores, e fazer com que as funções executem trabalho em unidades em vez de literais numéricos.  Isso permite que o compilador Verifique se os tipos de literais numéricos passados fazem sentido em um determinado contexto, eliminando assim os erros de tempo de execução associados a esse tipo de trabalho.

[!code-fsharp[UnitsOfMeasure](../../samples/snippets/fsharp/tour.fs#L817-L842)]

A biblioteca de F# Core define muitos tipos de unidade de SI e conversões de unidade.  Para saber mais, confira o [Namespace Microsoft.FSharp.Data.UnitSystems.si](https://msdn.microsoft.com/visualfsharpdocs/conceptual/microsoft.fsharp.data.unitsystems.si-namespace-%5bfsharp%5d).

## <a name="classes-and-interfaces"></a>Classes e interfaces

F# também tem suporte completo para classes do .NET [Interfaces](./language-reference/interfaces.md), [Classes abstratas](./language-reference/abstract-classes.md), [herança](./language-reference/inheritance.md)e assim por diante.

[Classes](./language-reference/classes.md) são tipos que representam objetos .net, que podem ter propriedades, métodos e eventos como seus [Membros](./language-reference/members/index.md).

[!code-fsharp[Classes](../../samples/snippets/fsharp/tour.fs#L845-L880)]

Definir classes genéricas também é muito simples.

[!code-fsharp[Classes](../../samples/snippets/fsharp/tour.fs#L883-L908)]

Para implementar uma interface, você pode usar `interface ... with` a sintaxe ou uma [expressão de objeto](./language-reference/object-expressions.md).

[!code-fsharp[Classes](../../samples/snippets/fsharp/tour.fs#L911-L934)]

## <a name="which-types-to-use"></a>Quais tipos usar

A presença de classes, registros, uniões discriminadas e tuplas leva a uma pergunta importante: o que você deve usar?  Como a maioria de tudo na vida, a resposta depende de suas circunstâncias.

As tuplas são ótimas para retornar vários valores de uma função e usar uma agregação ad hoc de valores como um valor em si.

Os registros são um "Step Up" de tuplas, com rótulos nomeados e suporte para membros opcionais.  Eles são ótimos para uma representação de insuficiência de dados em trânsito por meio de seu programa.  Como eles têm igualdade estrutural, eles são fáceis de usar com comparação.

As uniões discriminadas têm muitos usos, mas o principal benefício é poder utilizá-los em conjunto com correspondência de padrões para considerar todas as "formas" possíveis que os dados podem ter.  

As classes são excelentes por um grande número de motivos, como quando você precisa representar informações e também vincular essas informações à funcionalidade.  Como regra prática, quando você tem funcionalidade que está vinculada conceitualmente a alguns dados, usar classes e os princípios da programação orientada a objeto é um grande benefício.  As classes também são o tipo de dados preferencial ao interoperar com C# e Visual Basic, pois essas linguagens usam classes para quase tudo.

## <a name="next-steps"></a>Próximas etapas

Agora que você viu alguns dos principais recursos da linguagem, você deve estar pronto para gravar seus programas em F# primeiro!  Confira [introdução](./tutorials/getting-started/index.md) para saber como configurar seu ambiente de desenvolvimento e escrever algum código.

As próximas etapas para saber mais podem ser o que você gosta, mas recomendamos [a introdução à programação F# funcional no](./introduction-to-functional-programming/index.md) para se familiarizar com os principais conceitos de programação funcional.  Eles estarão essenciais na criação de programas robustos em F#.

Além disso, confira a [referência da linguagem F#](./language-reference/index.md) para ver um conjunto abrangente de conteúdo conceitual em F#.
