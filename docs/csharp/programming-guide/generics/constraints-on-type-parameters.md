---
title: Restrições a parâmetros de tipo – Guia de Programação em C#
ms.custom: seodec18
ms.date: 04/12/2018
helpviewer_keywords:
- generics [C#], type constraints
- type constraints [C#]
- type parameters [C#], constraints
- unbound type parameter [C#]
ms.openlocfilehash: bb545d9da73154c237f55809a3a72ff0f121ce1a
ms.sourcegitcommit: 4e2d355baba82814fa53efd6b8bbb45bfe054d11
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70253014"
---
# <a name="constraints-on-type-parameters-c-programming-guide"></a>Restrições a parâmetros de tipo (Guia de Programação em C#)

Restrições informam o compilador sobre as funcionalidades que um argumento de tipo deve ter. Sem nenhuma restrição, o argumento de tipo poderia ser qualquer tipo. O compilador pode assumir somente os membros de <xref:System.Object?displayProperty=nameWithType>, que é a classe base definitiva para qualquer tipo .NET. Para obter mais informações, consulte [Por que usar restrições](#why-use-constraints). Se o código de cliente tentar criar uma instância da classe usando um tipo não permitido por uma restrição, o resultado será um erro em tempo de compilação. Restrições são especificadas usando a palavra-chave contextual `where`. A tabela a seguir lista os sete tipos de restrições:

|Restrição|Descrição|
|----------------|-----------------|
|`where T : struct`|O argumento de tipo deve ser um tipo de valor. Qualquer valor de tipo com exceção de <xref:System.Nullable%601> pode ser especificado. Para obter mais informações sobre tipos que permitem valor nulo, consulte [Tipos que permitem valor nulo](../nullable-types/index.md).|
|`where T : class`|O argumento de tipo deve ser um tipo de referência. Essa restrição se aplica também a qualquer classe, interface, delegado ou tipo de matriz.|
|`where T : notnull`|O argumento de tipo deve ser um tipo não anulável. O argumento pode ser um tipo de referência não anulável em C# 8,0 ou posterior, ou um tipo de valor não anulável. Essa restrição se aplica também a qualquer classe, interface, delegado ou tipo de matriz.|
|`where T : unmanaged`|O argumento de tipo deve ser um [tipo não gerenciado](../../language-reference/builtin-types/unmanaged-types.md).|
|`where T : new()`|O argumento de tipo deve ter um construtor público sem parâmetros. Quando usado em conjunto com outras restrições, a restrição `new()` deve ser a última a ser especificada.|
|`where T :` *\<nome de classe base>*|O argumento de tipo deve ser ou derivar da classe base especificada.|
|`where T :` *\<nome da interface>*|O argumento de tipo deve ser ou implementar a interface especificada. Várias restrições de interface podem ser especificadas. A interface de restrição também pode ser genérica.|
|`where T : U`|O argumento de tipo fornecido para T deve ser ou derivar do argumento fornecido para U.|

Algumas das restrições são mutuamente exclusivas. Todos os tipos de valor devem ter um construtor sem parâmetros acessível. A `struct` restrição implica que `new()` a restrição e `new()` a restrição não podem ser combinadas com a `struct` restrição. A restrição `unmanaged` implica a restrição `struct`. A `unmanaged` restrição não pode ser combinada com as `struct` restrições `new()` ou.

## <a name="why-use-constraints"></a>Por que usar restrições

Ao restringir o parâmetro de tipo, aumenta-se a quantidade de operações e chamadas de método permitidas àqueles com suporte do tipo de restrição e de todos os tipos de sua hierarquia de herança. Ao criar classes ou métodos genéricos, se você estiver executando qualquer operação nos membros genéricos além da atribuição simples ou chamando quaisquer métodos sem suporte pelo <xref:System.Object?displayProperty=nameWithType>, precisará aplicar restrições ao parâmetro de tipo. Por exemplo, a restrição de classe base informa ao compilador que somente os objetos desse tipo ou derivados desse tipo serão usados como argumentos de tipo. Uma vez que o compilador tiver essa garantia, ele poderá permitir que métodos desse tipo sejam chamados na classe genérica. O exemplo de código a seguir demonstra a funcionalidade que pode ser adicionada à classe `GenericList<T>` (em [Introdução aos Genéricos](introduction-to-generics.md)) ao aplicar uma restrição de classe base.

[!code-csharp[using the class and struct constraints](~/samples/snippets/csharp/keywords/GenericWhereConstraints.cs#9)]

A restrição permite que a classe genérica use a propriedade `Employee.Name`. A restrição especifica que todos os itens do tipo `T` são um objeto `Employee` ou um objeto que herda de `Employee`.

Várias restrições podem ser aplicadas ao mesmo parâmetro de tipo e as restrições em si podem ser tipos genéricos, da seguinte maneira:

[!code-csharp[using the class and struct constraints](~/samples/snippets/csharp/keywords/GenericWhereConstraints.cs#10)]

Ao aplicar a restrição `where T : class`, evite os operadores `==` e `!=` no parâmetro de tipo, pois esses operadores testarão somente a identidade de referência e não a igualdade de valor. Esse comportamento ocorrerá mesmo se esses operadores forem sobrecarregados em um tipo usado como argumento. O código a seguir ilustra esse ponto; a saída é false, muito embora a classe <xref:System.String> sobrecarregue o operador `==`.

[!code-csharp[using the class and struct constraints](~/samples/snippets/csharp/keywords/GenericWhereConstraints.cs#11)]

O compilador só sabe que `T` é um tipo de referência em tempo de compilação e deve usar os operadores padrão que são válidos para todos os tipos de referência. Caso seja necessário testar a igualdade de valor, a maneira recomendada é também aplicar a restrição `where T : IEquatable<T>` ou `where T : IComparable<T>` e implementar a interface em qualquer classe que seja usada para construir a classe genérica.

## <a name="constraining-multiple-parameters"></a>Restringindo vários parâmetros

É possível aplicar restrições a vários parâmetros e várias restrições a um único parâmetro, conforme mostrado no exemplo a seguir:

[!code-csharp[using the class and struct constraints](~/samples/snippets/csharp/keywords/GenericWhereConstraints.cs#12)]

## <a name="unbounded-type-parameters"></a>Parâmetros de tipo não associado

 Os parâmetros de tipo que não têm restrições, como o T na classe pública `SampleClass<T>{}`, são denominados “parâmetros de tipo não associado”. Os parâmetros de tipo não associado têm as seguintes regras:

- Os `!=` operadores `==` e não podem ser usados porque não há garantia de que o argumento de tipo concreto dará suporte a esses operadores.
- Eles podem ser convertidos para e de `System.Object` ou explicitamente convertidos para qualquer tipo de interface.
- Você pode compará-los com [nulo](../../language-reference/keywords/null.md). Se um parâmetro não associado for comparado a `null`, a comparação sempre retornará false se o argumento de tipo for um tipo de valor.

## <a name="type-parameters-as-constraints"></a>Parâmetros de tipo como restrições

O uso de um parâmetro de tipo genérico como uma restrição será útil quando uma função membro com parâmetro de tipo próprio tiver que restringir esse parâmetro para o parâmetro de tipo do tipo recipiente, conforme mostrado no exemplo a seguir:

[!code-csharp[using the class and struct constraints](~/samples/snippets/csharp/keywords/GenericWhereConstraints.cs#13)]

No exemplo anterior, `T` é uma restrição de tipo no contexto do método `Add` e um parâmetro de tipo não associado no contexto da classe `List`.

Parâmetros de tipo também podem ser usados como restrições em definições de classe genérica. O parâmetro de tipo deve ser declarado entre colchetes angulares junto com quaisquer outros parâmetros de tipo:

[!code-csharp[using the class and struct constraints](~/samples/snippets/csharp/keywords/GenericWhereConstraints.cs#14)]

A utilidade dos parâmetros de tipo como restrições com classes genéricas é limitada, pois o compilador não pode presumir nada sobre o parâmetro de tipo, exceto que ele deriva de `System.Object`. Use parâmetros de tipo como restrições em classes genéricas em cenários nos quais deseja impor uma relação de herança entre dois parâmetros de tipo.

## <a name="notnull-constraint"></a>Restrição não nula

A partir C# do 8,0, você pode usar `notnull` a restrição para especificar que o argumento de tipo deve ser um tipo de valor não anulável ou um tipo de referência não anulável. A `notnull` restrição só pode ser usada em um `nullable enable` contexto. O compilador gerará um aviso se você adicionar `notnull` a restrição em um contexto alheios anulável. 

Ao contrário de outras restrições, quando um argumento de tipo `notnull` viola a restrição, o compilador gera um aviso quando esse código é compilado `nullable enable` em um contexto. Se o código for compilado em um contexto alheios anulável, o compilador não gerará nenhum aviso ou erro.

## <a name="unmanaged-constraint"></a>Restrição não gerenciada

Começando com o C# 7.3, você pode usar a restrição `unmanaged` para especificar que o parâmetro de tipo deve ser um [tipo não gerenciado](../../language-reference/builtin-types/unmanaged-types.md). A restrição `unmanaged` permite que você escreva rotinas reutilizáveis para trabalhar com tipos que podem ser manipulados como blocos de memória, conforme mostrado no exemplo a seguir:

[!code-csharp[using the unmanaged constraint](~/samples/snippets/csharp/keywords/GenericWhereConstraints.cs#15)]

O método anterior deve ser compilado em um contexto `unsafe` porque ele usa o operador `sizeof` em um tipo não conhecido como um tipo interno. Sem a restrição `unmanaged`, o operador `sizeof` não está disponível.

## <a name="delegate-constraints"></a>Restrições de delegado

Também começando com o C# 7.3, você pode usar <xref:System.Delegate?displayProperty=nameWithType> ou <xref:System.MulticastDelegate?displayProperty=nameWithType> como uma restrição de classe base. O CLR sempre permitia essa restrição, mas a linguagem C# não a permite. A restrição `System.Delegate` permite que você escreva código que funcione com delegados de uma maneira fortemente tipada. O código a seguir define um método de extensão que combina dois delegados, desde que eles sejam do mesmo tipo:

[!code-csharp[using the delegate constraint](~/samples/snippets/csharp/keywords/GenericWhereConstraints.cs#16)]

Você pode usar o método acima para combinar delegados que são do mesmo tipo:

[!code-csharp[using the unmanaged constraint](~/samples/snippets/csharp/keywords/GenericWhereConstraints.cs#17)]

Se você remover a marca de comentário na última linha, ela não será compilada. Ambos `first` e`test` são tipos delegados, mas são tipos delegados diferentes.

## <a name="enum-constraints"></a>Restrições de enum

Começando com o C# 7.3, você também pode especificar o tipo <xref:System.Enum?displayProperty=nameWithType> como uma restrição de classe base. O CLR sempre permitia essa restrição, mas a linguagem C# não a permite. Genéricos usando `System.Enum` fornecem programação fortemente tipada para armazenar em cache os resultados do uso de métodos estáticos em `System.Enum`. O exemplo a seguir localiza todos os valores válidos para um tipo enum e, em seguida, cria um dicionário que mapeia esses valores para sua representação de cadeia de caracteres.

[!code-csharp[using the unmanaged constraint](~/samples/snippets/csharp/keywords/GenericWhereConstraints.cs#18)]

Os métodos usados fazem uso de reflexão, o que tem implicações de desempenho. Você pode chamar esse método para criar uma coleção que é armazenada em cache e reutilizada, em vez de repetir as chamadas que exigem reflexão.

Você pode usá-lo conforme mostrado no exemplo a seguir para criar uma enum e compilar um dicionário de seus nomes e valores:

[!code-csharp[using the unmanaged constraint](~/samples/snippets/csharp/keywords/GenericWhereConstraints.cs#19)]

[!code-csharp[using the unmanaged constraint](~/samples/snippets/csharp/keywords/GenericWhereConstraints.cs#20)]

## <a name="see-also"></a>Consulte também

- <xref:System.Collections.Generic>
- [Guia de Programação em C#](../index.md)
- [Introdução aos genéricos](./index.md)
- [Classes genéricas](./generic-classes.md)
- [Restrição new](../../language-reference/keywords/new-constraint.md)
