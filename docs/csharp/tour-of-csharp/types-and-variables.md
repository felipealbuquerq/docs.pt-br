---
title: Tipos e variáveis do C# - um tour pela linguagem C#
description: Saiba mais sobre como definir tipos e declarar variáveis em C#
ms.date: 08/10/2016
ms.assetid: f8a8051e-0049-43f1-b594-9c84cc7b1224
ms.openlocfilehash: 22a91b101d5361091b09217d4562703851c86940
ms.sourcegitcommit: 6f28b709592503d27077b16fff2e2eacca569992
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70105582"
---
# <a name="types-and-variables"></a>Tipos e variáveis

Há dois tipos em C#: *tipos de referência* e *tipos de valor*. As variáveis de tipos de valor contêm diretamente seus dados enquanto variáveis de tipos de referência armazenam referências a seus dados, o último sendo conhecido como objetos. Com tipos de referência, é possível que duas variáveis referenciem o mesmo objeto e, portanto, é possível que operações em uma variável afetem o objeto referenciado por outra variável. Com tipos de valor, cada variável tem sua própria cópia dos dados e não é possível que operações em uma variável afetem a outra (exceto no caso de variáveis de parâmetros `ref` e `out`).

Os tipos de valor do C# são divididos em *tipos simples*, *tipos de enum*, *tipos struct* e *tipos de valor anulável*. Os tipos de referência do C# são divididos em *tipos de classe*, *tipos de interface*, *tipos de matriz* e *tipos delegados*.

O exemplo a seguir fornece uma visão geral do sistema de tipos do C#.

- [Tipos de valor][ValueTypes]
  - [Tipos simples][SimpleTypes]
    - Integral com sinal: `sbyte`, `short`, `int`,`long`
    - Integral sem sinal: `byte`, `ushort`, `uint`,`ulong`
    - Caracteres Unicode: `char`
    - Ponto flutuante binário de IEEE: `float`, `double`
    - Ponto flutuante decimal de alta precisão: `decimal`
    - Booliano: `bool`
  - [Tipos enumerados][EnumTypes]
    - Tipos definidos pelo usuário do formulário `enum E {...}`
  - [Tipos struct][StructTypes]
    - Tipos definidos pelo usuário do formulário `struct S {...}`
  - [Tipos de valor anuláveis][NullableTypes]
    - Extensões de todos os outros tipos de valor com um valor `null`
- [Tipos de referência][ReferenceTypes]
  - [Tipos de classe][ClassTypes]
    - Classe base definitiva de todos os outros tipos: `object`
    - Cadeia de caracteres Unicode: `string`
    - Tipos definidos pelo usuário do formulário `class C {...}`
  - [Tipos de interface][InterfaceTypes]
    - Tipos definidos pelo usuário do formulário `interface I {...}`
  - [Tipos de matriz][ArrayTypes]
    - Unidimensional e multidimensional, por exemplo, `int[]` e `int[,]`
  - [Tipos delegados][DelegateTypes]
    - Tipos definidos pelo usuário do formulário `delegate int D(...)`

[ValueTypes]: ../language-reference/keywords/value-types-table.md
[SimpleTypes]: ../language-reference/keywords/value-types.md#simple-types
[EnumTypes]: ../language-reference/keywords/enum.md
[StructTypes]: ../language-reference/keywords/struct.md
[NullableTypes]: ../programming-guide/nullable-types/index.md
[ReferenceTypes]: ../language-reference/keywords/reference-types.md
[ClassTypes]: ../language-reference/keywords/class.md
[InterfaceTypes]: ../language-reference/keywords/interface.md
[DelegateTypes]: ../language-reference/keywords/delegate.md
[ArrayTypes]: ../programming-guide/arrays/index.md

Para obter mais informações sobre tipos numéricos, veja a tabela [Tipos integrais](../language-reference/builtin-types/integral-numeric-types.md) e [Tipos de ponto flutuante](../language-reference/builtin-types/floating-point-numeric-types.md).

O tipo `bool` do C# é usado para representar valores boolianos — valores que são `true` ou `false`.

O processamento de cadeia de caracteres e caracteres em C# usa codificação Unicode. O tipo `char` representa uma unidade de código UTF-16 e o tipo `string` representa uma sequência de unidades de código UTF-16.

Os programas em C# usam *declarações de tipos* para criar novos tipos. Uma declaração de tipo especifica o nome e os membros do novo tipo. Cinco das categorias do C# de tipos são tipos definidos pelo usuário: tipos de classe, tipos struct, tipos de interface, tipos enum e tipos delegados.

Um tipo `class` define uma estrutura de dados que contém membros de dados (campos) e membros de função (métodos, propriedades e outros). Os tipos de classe dão suporte à herança única e ao polimorfismo, mecanismos nos quais as classes derivadas podem estender e especializar as classes base.

Um tipo `struct` é semelhante a um tipo de classe que representa uma estrutura com membros de dados e membros da função. No entanto, diferentemente das classes, structs são tipos de valor e, normalmente, não precisam de alocação de heap. Os tipos de estrutura não dão suporte à herança especificada pelo usuário, e todos os tipos de structs são herdados implicitamente do tipo `object`.

Um tipo `interface` define um contrato como um conjunto nomeado de membros da função pública. Um `class` ou `struct` que implementa um `interface` deve fornecer implementações de membros da função da interface. Um `interface` pode herdar de várias interfaces base e um `class` ou `struct` pode implementar várias interfaces.

Um tipo `delegate` representa referências aos métodos com uma lista de parâmetros e tipo de retorno específicos. Delegados possibilitam o tratamento de métodos como entidades que podem ser atribuídos a variáveis e passadas como parâmetros. Os delegados são análogos aos tipos de função fornecidos pelas linguagens funcionais. Eles também são parecidos com o conceito de ponteiros de função em outras linguagens, mas, ao contrário dos ponteiros de função, os delegados são orientados a objetos e fortemente tipados.

Os tipos `class`, `struct`, `interface` e `delegate` dão suporte a genéricos e podem ser parametrizados com outros tipos.

Um tipo `enum` é um tipo distinto com constantes nomeadas. Cada tipo `enum` tem um tipo subjacente, que deve ser um dos oito tipos integrais. O conjunto de valores de um tipo `enum` é o mesmo que o conjunto de valores do tipo subjacente.

O C# dá suporte a matrizes uni e multidimensionais de qualquer tipo. Ao contrário dos tipos listados acima, os tipos de matriz não precisam ser declarados antes de serem usados. Em vez disso, os tipos de matriz são construídos seguindo um nome de tipo entre colchetes. Por exemplo, `int[]` é uma matriz unidimensional de `int`, `int[,]` é uma matriz bidimensional de `int`, e `int[][]` é uma matriz unidimensional da matriz unidimensional de `int`.

Os tipos de valor anulável também não precisam ser declarados antes de serem usados. Para cada tipo de valor não nulo `T` há um tipo de valor anulável correspondente `T?`, que pode conter um valor adicional, `null`. Por exemplo, `int?` é um tipo que pode conter qualquer número inteiro de 32 bits ou o valor `null`.

O sistema de tipos do C# é unificado, de modo que um valor de qualquer tipo pode ser tratado como um `object`. Cada tipo no C#, direta ou indiretamente, deriva do tipo de classe `object`, e `object` é a classe base definitiva de todos os tipos. Os valores de tipos de referência são tratados como objetos simplesmente exibindo os valores como tipo `object`. Os valores de tipos de valor são tratados como objetos, executando *conversão boxing* e *operações de conversão unboxing*. No exemplo a seguir, um valor `int` é convertido em `object` e volta novamente ao `int`.

[!code-csharp[Boxing](../../../samples/snippets/csharp/tour/types-and-variables/Program.cs#L1-L10)]

Quando um valor de um tipo de valor é convertido para o tipo `object`, uma instância `object`, também chamada de "caixa", é alocada para armazenar o valor e o valor é copiado na caixa. Por outro lado, quando uma referência `object` é convertida em um tipo de valor, é verificado se o `object` referenciado é uma caixa do tipo de valor correto e, se a verificação for bem-sucedida, o valor na caixa será copiado.

O sistema de tipo unificado do C# significa que os tipos de valor podem se tornar objetos “sob demanda”. Devido à unificação, as bibliotecas de finalidade geral que usam o tipo `object` podem ser usadas com os tipos de referência e os tipos de valor.

Existem vários tipos de *variáveis* no C#, incluindo campos, elementos de matriz, variáveis locais e parâmetros. As variáveis representam os locais de armazenamento e cada variável tem um tipo que determina quais valores podem ser armazenados na variável, conforme mostrado abaixo.

- Tipo de valor não nulo
  - Um valor de tipo exato
- Tipos de valor anulável
  - Um valor `null` ou um valor do tipo exato
- objeto
  - Uma referência `null`, uma referência a um objeto de qualquer tipo de referência ou uma referência a um valor de qualquer tipo de valor demarcado
- Tipo de classe
  - Uma referência `null`, uma referência a uma instância desse tipo de classe ou uma referência a uma instância de uma classe derivada desse tipo de classe
- Tipo de interface
  - Uma referência `null`, uma referência a uma instância de um tipo de classe que implementa esse tipo de interface ou uma referência a um valor demarcado de um tipo de valor que implementa esse tipo de interface
- Tipo de matriz
  - Uma referência `null`, uma referência a uma instância desse tipo de matriz ou uma referência a uma instância de um tipo de matriz compatível
- Tipo delegate
  - Uma referência `null` ou uma referência a uma instância de um tipo de delegado compatível

> [!div class="step-by-step"]
> [Anterior](program-structure.md)
> [Próximo](expressions.md)
