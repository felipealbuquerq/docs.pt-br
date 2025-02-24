---
title: Novidades no C# 8.0 – Guia do C#
description: Obtenha uma visão geral dos novos recursos disponíveis no C# 8.0. Este artigo está atualizado com a versão prévia 5.
ms.date: 09/04/2019
ms.openlocfilehash: b281c55a5911d81503a6af80e393469be1124280
ms.sourcegitcommit: c70542d02736e082e8dac67dad922c19249a8893
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/05/2019
ms.locfileid: "70374004"
---
# <a name="whats-new-in-c-80"></a>Novidades no C# 8.0

Há vários aprimoramentos da linguagem C# que você já pode experimentar.

- [Membros somente leitura](#readonly-members)
- [Membros da interface padrão](#default-interface-members)
- [Aprimoramentos de correspondência de padrões](#more-patterns-in-more-places):
  - [Expressões Switch](#switch-expressions)
  - [Padrões da propriedade](#property-patterns)
  - [Padrões de tupla](#tuple-patterns)
  - [Padrões posicionais](#positional-patterns)
- [Declarações using](#using-declarations)
- [Funções locais estáticas](#static-local-functions)
- [Estruturas ref descartáveis](#disposable-ref-structs)
- [Tipos de referência nula](#nullable-reference-types)
- [Fluxos assíncronos](#asynchronous-streams)
- [Índices e intervalos](#indices-and-ranges)
- [Tipos construídos não gerenciados](#unmanaged-constructed-types)
- [Aprimoramento de cadeias de caracteres idênticas interpoladas](#enhancement-of-interpolated-verbatim-strings)

> [!NOTE]
> Este artigo foi atualizado pela última vez para o C# 8.0 versão prévia 5.

O restante deste artigo descreve rapidamente esses recursos. Quando houver artigos detalhados disponíveis, forneceremos links para esses tutoriais e visões gerais. Você pode explorar esses recursos em seu ambiente usando a ferramenta global `dotnet try`:

1. Instale a ferramenta global [dotnet-try](https://github.com/dotnet/try/blob/master/README.md#setup).
1. Clone o repositório [dotnet/try-samples](https://github.com/dotnet/try-samples).
1. Defina o diretório atual do subdiretório *csharp8* para o repositório *try-samples*.
1. Execute `dotnet try`.

## <a name="readonly-members"></a>Membros somente leitura

É possível aplicar o modificador `readonly` a qualquer membro de um struct. Ele indica que o membro não modifica o estado. É mais granular do que aplicar o modificador `readonly` a uma declaração `struct`.  Considere o seguinte struct mutável:

```csharp
public struct Point
{
    public double X { get; set; }
    public double Y { get; set; }
    public double Distance => Math.Sqrt(X * X + Y * Y);

    public override string ToString() =>
        $"({X}, {Y}) is {Distance} from the origin";
}
```

Como a maioria dos structs, o método `ToString()` não modifica o estado. É possível indicar isso adicionando o modificador `readonly` à declaração de `ToString()`:

```csharp
public readonly override string ToString() =>
    $"({X}, {Y}) is {Distance} from the origin";
```

A alteração anterior gera um aviso do compilador, porque o `ToString` acessa a propriedade `Distance`, que não está marcada como `readonly`:

```console
warning CS8656: Call to non-readonly member 'Point.Distance.get' from a 'readonly' member results in an implicit copy of 'this'
```

O compilador avisa quando há a necessidade de criar uma cópia de defesa.  A propriedade `Distance` não altera o estado, portanto, é possível corrigir esse aviso adicionando o modificador `readonly` à declaração:

```csharp
public readonly double Distance => Math.Sqrt(X * X + Y * Y);
```

Observe que o modificador `readonly` é necessário em uma propriedade somente leitura. O compilador não pressupõe que os acessadores `get` não modificam o estado; é necessário declarar `readonly` explicitamente. O compilador impõe como regra que membros `readonly` não modificam o estado. O método a seguir não será compilado, a menos que você remova o modificador `readonly`:

```csharp
public readonly void Translate(int xOffset, int yOffset)
{
    X += xOffset;
    Y += yOffset;
}
```

Esse recurso permite que você especifique sua intenção de design para que o compilador possa impô-la e faça otimizações com base nessa intenção.

## <a name="default-interface-members"></a>Membros da interface padrão

Agora é possível adicionar membros a interfaces e fornecer uma implementação para esses membros. Esse recurso de linguagem permite que os autores de API adicionem métodos a uma interface em versões posteriores sem interromper a fonte ou a compatibilidade binária com implementações existentes dessa interface. As implementações existentes *herdam* a implementação padrão. Esse recurso também permite que o C# interopere com APIs que direcionam o Android ou o Swift, que dão suporte a recursos semelhantes. Membros da interface padrão também permitem cenários semelhantes a um recurso de linguagem de “características”.

Os membros da interface padrão afetam muitos cenários e elementos de linguagem. Nosso primeiro tutorial aborda [como atualizar uma interface com implementações padrão](../tutorials/default-interface-members-versions.md). Outros tutoriais e atualizações de referência chegarão a tempo para a versão geral.

## <a name="more-patterns-in-more-places"></a>Mais padrões em mais partes

Com a **correspondência de padrões**, você recebe ferramentas para fornecer funcionalidades dependentes da forma em tipos de dados relacionados, mas diferentes. O C# 7.0 introduziu uma sintaxe para padrões de tipo e padrões de constante usando a expressão [`is`](../language-reference/keywords/is.md) e a instrução [`switch`](../language-reference/keywords/switch.md). Esses recursos representaram os primeiros passos em direção ao suporte a paradigmas de programação, em que os dados e a funcionalidade vivem separados. À medida que o setor se aproxima mais de microsserviços e de outras arquiteturas baseadas em nuvem, outras ferramentas de linguagem de tornam necessárias.

O C# 8.0 expande esse vocabulário, para que você possa usar mais expressões de padrão em mais partes do seu código. Considere esses recursos quando seus dados e funcionalidades estiverem separados. Considere a correspondência de padrões quando seus algoritmos dependerem de um fato diferente do tipo de tempo de execução de um objeto. Essas técnicas fornecem outra maneira de expressar designs.

Além dos novos padrões em novas partes, o C# 8.0 adiciona **padrões recursivos**. O resultado de qualquer expressão padrão é uma expressão. Um padrão recursivo é simplesmente uma expressão padrão aplicada à saída de outra expressão padrão.

### <a name="switch-expressions"></a>Expressões switch

Frequentemente, uma instrução [`switch`](../language-reference/keywords/switch.md) produz um valor em cada um de seus blocos `case`. As **expressões switch** permitem que você use a sintaxe de expressão mais concisa. Há menos palavras-chave `case` e `break` repetidas, e menos chaves.  Por exemplo, considere a enumeração a seguir que lista as cores do arco-íris:

```csharp
public enum Rainbow
{
    Red,
    Orange,
    Yellow,
    Green,
    Blue,
    Indigo,
    Violet
}
```

Se seu aplicativo definiu um tipo `RGBColor` que é construído a partir dos componentes `R`, `G` e `B`, você poderia converter um valor `Rainbow` em seus valores RGB usando o método a seguir que contém uma expressão de opção:

```csharp
public static RGBColor FromRainbow(Rainbow colorBand) =>
    colorBand switch
    {
        Rainbow.Red    => new RGBColor(0xFF, 0x00, 0x00),
        Rainbow.Orange => new RGBColor(0xFF, 0x7F, 0x00),
        Rainbow.Yellow => new RGBColor(0xFF, 0xFF, 0x00),
        Rainbow.Green  => new RGBColor(0x00, 0xFF, 0x00),
        Rainbow.Blue   => new RGBColor(0x00, 0x00, 0xFF),
        Rainbow.Indigo => new RGBColor(0x4B, 0x00, 0x82),
        Rainbow.Violet => new RGBColor(0x94, 0x00, 0xD3),
        _              => throw new ArgumentException(message: "invalid enum value", paramName: nameof(colorBand)),
    };
```

Há vários aprimoramentos de sintaxe aqui:

- A variável vem antes da palavra-chave `switch`. A ordem diferente facilita distinguir visualmente a expressão switch da instrução switch.
- Os elementos `case` e `:` são substituídos por `=>`. É mais conciso e intuitivo.
- O caso `default` é substituído por um descarte `_`.
- Os corpos são expressões, não instruções.

Compare isso com o código equivalente usando a instrução clássica `switch`:

```csharp
public static RGBColor FromRainbowClassic(Rainbow colorBand)
{
    switch (colorBand)
    {
        case Rainbow.Red:
            return new RGBColor(0xFF, 0x00, 0x00);
        case Rainbow.Orange:
            return new RGBColor(0xFF, 0x7F, 0x00);
        case Rainbow.Yellow:
            return new RGBColor(0xFF, 0xFF, 0x00);
        case Rainbow.Green:
            return new RGBColor(0x00, 0xFF, 0x00);
        case Rainbow.Blue:
            return new RGBColor(0x00, 0x00, 0xFF);
        case Rainbow.Indigo:
            return new RGBColor(0x4B, 0x00, 0x82);
        case Rainbow.Violet:
            return new RGBColor(0x94, 0x00, 0xD3);
        default:
            throw new ArgumentException(message: "invalid enum value", paramName: nameof(colorBand));
    };
}
```

### <a name="property-patterns"></a>Padrões da propriedade

O **padrão da propriedade** permite que você compare as propriedades do objeto examinado. Considere um site de comércio eletrônico que deve calcular o imposto da venda com base no endereço do comprador. Esse cálculo não é uma responsabilidade principal de uma classe `Address`. Ele mudará ao longo do tempo, provavelmente com mais frequência do que as alterações de formato de endereço. O valor do imposto depende da propriedade `State` do endereço. O método a seguir usa o padrão de propriedade para calcular o imposto da venda de acordo com o endereço e o preço:

```csharp
public static decimal ComputeSalesTax(Address location, decimal salePrice) =>
    location switch
    {
        { State: "WA" } => salePrice * 0.06M,
        { State: "MN" } => salePrice * 0.75M,
        { State: "MI" } => salePrice * 0.05M,
        // other cases removed for brevity...
        _ => 0M
    };
```

A correspondência de padrão cria uma sintaxe concisa para expressar esse algoritmo.

### <a name="tuple-patterns"></a>Padrões de tupla

Alguns algoritmos dependem de várias entradas. **Padrões de tupla** permitem que você alterne com base em vários valores, expressadas como uma [tupla](../tuples.md).  O código a seguir mostra uma expressão de comutador para o jogo *pedra, papel, tesoura*:

```csharp
public static string RockPaperScissors(string first, string second)
    => (first, second) switch
    {
        ("rock", "paper") => "rock is covered by paper. Paper wins.",
        ("rock", "scissors") => "rock breaks scissors. Rock wins.",
        ("paper", "rock") => "paper covers rock. Paper wins.",
        ("paper", "scissors") => "paper is cut by scissors. Scissors wins.",
        ("scissors", "rock") => "scissors is broken by rock. Rock wins.",
        ("scissors", "paper") => "scissors cuts paper. Scissors wins.",
        (_, _) => "tie"
    };
```

As mensagens indicam o vencedor. O caso de descarte representa três combinações para empates ou outras entradas de texto.

### <a name="positional-patterns"></a>Padrões posicionais

Alguns tipos incluem um método `Deconstruct` que desconstrói suas propriedades em variáveis discretas. Quando um método `Deconstruct` é acessível, você pode usar **padrões posicionais** para inspecionar as propriedades do objeto e usar essas propriedades para um padrão.  Considere a seguinte classe `Point`, que inclui um método `Deconstruct` para criar variáveis discretas para `X` e `Y`:

```csharp
public class Point
{
    public int X { get; }
    public int Y { get; }

    public Point(int x, int y) => (X, Y) = (x, y);

    public void Deconstruct(out int x, out int y) =>
        (x, y) = (X, Y);
}
```

Além disso, considere a seguinte enumeração que representa várias posições de um quadrante:

```csharp
public enum Quadrant
{
    Unknown,
    Origin,
    One,
    Two,
    Three,
    Four,
    OnBorder
}
```

O método a seguir usa o **padrão posicional** para extrair os valores de `x` e `y`. Em seguida, ele usa uma cláusula `when` para determinar o `Quadrant` do ponto:

```csharp
static Quadrant GetQuadrant(Point point) => point switch
{
    (0, 0) => Quadrant.Origin,
    var (x, y) when x > 0 && y > 0 => Quadrant.One,
    var (x, y) when x < 0 && y > 0 => Quadrant.Two,
    var (x, y) when x < 0 && y < 0 => Quadrant.Three,
    var (x, y) when x > 0 && y < 0 => Quadrant.Four,
    var (_, _) => Quadrant.OnBorder,
    _ => Quadrant.Unknown
};
```

O padrão de discard na opção anterior encontra a correspondência quando `x` ou `y` é 0, mas não ambos. Uma expressão switch deve produzir um valor ou lançar uma exceção. Se não houver correspondência em nenhum dos casos, a expressão switch gerará uma exceção. O compilador gerará um aviso se você não cobrir todos os casos possíveis em sua expressão switch.

Explore técnicas de correspondência de padrões neste [tutorial avançado sobre correspondência de padrões](../tutorials/pattern-matching.md).

## <a name="using-declarations"></a>Declarações using

Uma **declaração using** é uma declaração de variável precedida pela palavra-chave `using`. Ele informa ao compilador que a variável que está sendo declarada deve ser descartada ao final do escopo delimitador. Por exemplo, considere o seguinte código que grava um arquivo de texto:

```csharp
static void WriteLinesToFile(IEnumerable<string> lines)
{
    using var file = new System.IO.StreamWriter("WriteLines2.txt");
    foreach (string line in lines)
    {
        if (!line.Contains("Second"))
        {
            file.WriteLine(line);
        }
    }
// file is disposed here
}
```

No exemplo anterior, o arquivo é descartado quando a chave de fechamento do método é atingida. Esse é o final do escopo no qual `file` é declarado. O código anterior equivale ao código a seguir que usa as [instruções using](../language-reference/keywords/using-statement.md) clássicas:

```csharp
static void WriteLinesToFile(IEnumerable<string> lines)
{
    using (var file = new System.IO.StreamWriter("WriteLines2.txt"))
    {
        foreach (string line in lines)
        {
            if (!line.Contains("Second"))
            {
                file.WriteLine(line);
            }
        }
    } // file is disposed here
}
```

No exemplo anterior, o arquivo é descartado quando a chave de fechamento associada à instrução `using` é atingida.

Em ambos os casos, o compilador gera a chamada para `Dispose()`. O compilador gera um erro se a expressão na instrução `using` não for descartável.

## <a name="static-local-functions"></a>Funções locais estáticas

Agora você pode adicionar o modificador `static` para funções locais a fim de garantir que essa função local não capture (faça referência) às variáveis no escopo delimitador. Essa ação gera `CS8421`, "Uma função local estática não pode conter uma referência à \<variable>". 

Considere o código a seguir. A função local `LocalFunction` acessa a variável `y`, declarada no escopo delimitador (o método `M`). Portanto, `LocalFunction` não pode ser declarada com o modificador `static`:

```csharp
int M()
{
    int y;
    LocalFunction();
    return y;

    void LocalFunction() => y = 0;
}
```

O código a seguir contém uma função local estática. Ela pode ser estática porque não acesse as variáveis no escopo delimitador:

```csharp
int M()
{
    int y = 5;
    int x = 7;
    return Add(x, y);

    static int Add(int left, int right) => left + right;
}
```

## <a name="disposable-ref-structs"></a>Estruturas ref descartáveis

Uma `struct` declarada com o modificador `ref` não pode implementar quaisquer interfaces e, portanto, não pode implementar <xref:System.IDisposable>. Portanto, para permitir que uma `ref struct` seja descartada, ela deve ter um método `void Dispose()` acessível. Isso também se aplica às declarações `readonly ref struct`.

## <a name="nullable-reference-types"></a>Tipos de referência anuláveis

Dentro de um contexto de anotação anulável, qualquer variável de um tipo de referência é considerado um **tipo de referência não anulável**. Se você quiser indicar que uma variável pode ser nula, acrescente o nome do tipo com o `?` para declarar a variável como um **tipo de referência anulável**.

Para tipos de referência não anuláveis, o compilador usa a análise de fluxo para garantir que as variáveis locais sejam inicializadas como um valor não nulo quando declaradas. Os campos devem ser inicializados durante a construção. O compilador gera um aviso se a variável não for definida por uma chamada para qualquer um dos construtores disponíveis ou por um inicializador. Além disso, os tipos de referência não anuláveis não podem receber um valor que possa ser nulo.

Os tipos de referência anuláveis não são verificados para garantir que não tenham sido atribuídos ou inicializados como nulos. No entanto, o compilador usa a análise de fluxo para garantir que qualquer variável de um tipo de referência anulável passe por verificação com relação a um nulo antes de ser acessada ou atribuída a um tipo de referência não anulável.

Saiba mais sobre o recurso na visão geral sobre [Tipos de referência anuláveis](../nullable-references.md). Experimente-o em um novo aplicativo neste [tutorial de tipos de referência anuláveis](../tutorials/nullable-reference-types.md). Saiba mais sobre as etapas para migrar uma base de códigos existente para usar tipos de referência anuláveis no [tutorial sobre como migrar um aplicativo para usar tipos de referência anuláveis](../tutorials/upgrade-to-nullable-references.md).

## <a name="asynchronous-streams"></a>Fluxos assíncronos

A partir do C# 8.0, você pode criar e consumir fluxos de forma assíncrona. Um método que retorna um fluxo assíncrono tem três propriedades:

1. Ele é declarado com o modificador `async`.
1. Ela retorna um <xref:System.Collections.Generic.IAsyncEnumerable%601>.
1. O método contém instruções `yield return` para retornar elementos sucessivos no fluxo assíncrono.

O consumo de um fluxo assíncrono exige que você adicione a palavra-chave `await` antes da palavra-chave `foreach` quando você enumera os elementos do fluxo. A adição da palavra-chave `await` exige o método que enumera o fluxo assíncrono seja declarado com o modificador `async` e retorne um tipo permitido para um método `async`. Normalmente, isso significa retornar um <xref:System.Threading.Tasks.Task> ou <xref:System.Threading.Tasks.Task%601>. Também pode ser <xref:System.Threading.Tasks.ValueTask> ou <xref:System.Threading.Tasks.ValueTask%601>. Um método pode consumir e produzir um fluxo assíncrono, o que significa que retornaria um <xref:System.Collections.Generic.IAsyncEnumerable%601>. O código a seguir gera uma sequência de 0 a 19, esperando 100 ms entre a geração de cada número:

```csharp
public static async System.Collections.Generic.IAsyncEnumerable<int> GenerateSequence()
{
    for (int i = 0; i < 20; i++)
    {
        await Task.Delay(100);
        yield return i;
    }
}
```

Você enumeraria a sequência usando a instrução `await foreach`:

```csharp
await foreach (var number in GenerateSequence())
{
    Console.WriteLine(number);
}
```

Experimente você mesmo os fluxos assíncronos em nosso tutorial sobre como [criar e consumir fluxos assíncronos](../tutorials/generate-consume-asynchronous-stream.md).

## <a name="indices-and-ranges"></a>Índices e intervalos

Intervalos e índices fornecem uma sintaxe sucinta para especificar subintervalos em uma matriz, <xref:System.Span%601> ou <xref:System.ReadOnlySpan%601>.

Esse suporte a idioma depende de dois novos tipos e de dois novos operadores:

- <xref:System.Index?displayProperty=nameWithType> representa um índice em uma sequência.
- O operador `^`, que especifica que um índice é relativo ao final da sequência.
- <xref:System.Range?displayProperty=nameWithType> representa um subintervalo de uma sequência.
- O operador Range (`..`), que especifica o início e o final de um intervalo como seus operandos.

Vamos começar com as regras para índices. Considere uma matriz `sequence`. O índice `0` é o mesmo que `sequence[0]`. O índice `^0` é o mesmo que `sequence[sequence.Length]`. Observe que `sequence[^0]` gera uma exceção, assim como `sequence[sequence.Length]` faz. Para qualquer número `n`, o índice `^n` é o mesmo que `sequence.Length - n`.

Um intervalo especifica o *início* e o *final* de um intervalo. O início do intervalo é inclusivo, mas o final do intervalo é exclusivo, o que significa que o *início* está incluído no intervalo, mas o *final* não está incluído no intervalo. O intervalo `[0..^0]` representa todo o intervalo, assim como `[0..sequence.Length]` representa todo o intervalo. 

Vamos analisar alguns exemplos. Considere a matriz a seguir, anotada com seu índice do início e do final:

```csharp
var words = new string[]
{
                // index from start    index from end
    "The",      // 0                   ^9
    "quick",    // 1                   ^8
    "brown",    // 2                   ^7
    "fox",      // 3                   ^6
    "jumped",   // 4                   ^5
    "over",     // 5                   ^4
    "the",      // 6                   ^3
    "lazy",     // 7                   ^2
    "dog"       // 8                   ^1
};              // 9 (or words.Length) ^0
```

Recupere a última palavra com o índice `^1`:

```csharp
Console.WriteLine($"The last word is {words[^1]}");
// writes "dog"
```

O código a seguir cria um subintervalo com as palavras "quick", "brown" e "fox". Ele inclui `words[1]` até `words[3]`. O elemento `words[4]` não está no intervalo.

```csharp
var quickBrownFox = words[1..4];
```

O código a seguir cria um subintervalo com "lazy" e "dog". Ele inclui `words[^2]` e `words[^1]`. O índice final `words[^0]` não está incluído:

```csharp
var lazyDog = words[^2..^0];
```

Os exemplos a seguir criam intervalos abertos para o início, fim ou ambos:

```csharp
var allWords = words[..]; // contains "The" through "dog".
var firstPhrase = words[..4]; // contains "The" through "fox"
var lastPhrase = words[6..]; // contains "the", "lazy" and "dog"
```

Você também pode declarar intervalos como variáveis:

```csharp
Range phrase = 1..4;
```

Em seguida, o intervalo pode ser usado dentro dos caracteres `[` e `]`:

```csharp
var text = words[phrase];
```

Você pode explorar mais sobre índices e intervalos do tutorial sobre [índices e intervalos](../tutorials/ranges-indexes.md).

## <a name="unmanaged-constructed-types"></a>Tipos construídos não gerenciados

No C# 7,3 e anterior, um tipo construído (um tipo que inclui pelo menos um argumento de tipo) não pode ser um [tipo não gerenciado](../language-reference/builtin-types/unmanaged-types.md). A partir C# de 8,0, um tipo de valor construído não será gerenciado se ele contiver campos apenas de tipos não gerenciados.

Por exemplo, dada a seguinte definição do tipo genérico `Coords<T>` :

```csharp
public struct Coords<T>
{
    public T X;
    public T Y;
}
```

o `Coords<int>` tipo é um tipo não gerenciado em C# 8,0 e posterior. Como para qualquer tipo não gerenciado, você pode criar um ponteiro para uma variável desse tipo ou [alocar um bloco de memória na pilha](../language-reference/operators/stackalloc.md) para instâncias desse tipo:

```csharp
Span<Coords<int>> coordinates = stackalloc[]
{
    new Coords<int> { X = 0, Y = 0 },
    new Coords<int> { X = 0, Y = 3 },
    new Coords<int> { X = 4, Y = 0 }
};
```

Para obter mais informações, consulte [tipos não gerenciados](../language-reference/builtin-types/unmanaged-types.md).

## <a name="enhancement-of-interpolated-verbatim-strings"></a>Aprimoramento de cadeias de caracteres idênticas interpoladas

A `$` ordem dos tokens e `@` nas cadeias de caracteres `$@"..."` idênticas [interpoladas](../language-reference/tokens/interpolated.md) pode `@$"..."` ser any: e são cadeias de caracteres idênticas interpoladas válidas. Em versões C# anteriores, o `$` token deve aparecer antes do `@` token.
