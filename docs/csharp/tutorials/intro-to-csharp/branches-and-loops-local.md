---
title: Tutorial Branches e loops – introdução ao C#
description: Neste tutorial sobre branches e loops, você escreve código em C# para explorar a sintaxe de linguagem que dá suporte a branches e loops condicionais para execução repetida de instruções.
ms.date: 10/31/2017
ms.custom: mvc
ms.openlocfilehash: d329a871265ae42918fbf81c42be6667710e4c75
ms.sourcegitcommit: 205b9a204742e9c77256d43ac9d94c3f82909808
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70850811"
---
# <a name="learn-conditional-logic-with-branch-and-loop-statements"></a>Saiba mais sobre lógica condicional com instruções branch e loop

Este tutorial ensina a escrever código que examina variáveis e muda o caminho de execução com base nessas variáveis. Escreva o código em C# e veja os resultados da compilação e da execução. O tutorial contém uma série de lições que exploram construções de branches e loops em C#. Estas lições ensinam os princípios básicos da linguagem C#.

Este tutorial espera que você tenha um computador que possa usar para desenvolvimento. O tutorial do .NET [Olá, mundo em 10 minutos](https://dotnet.microsoft.com/learn/dotnet/hello-world-tutorial/intro) tem instruções para configurar seu ambiente de desenvolvimento local em Mac, PC ou Linux. Uma visão geral dos comandos que você usará está em [Familiarize-se com as ferramentas de desenvolvimento](local-environment.md), com links para obter mais detalhes.

## <a name="make-decisions-using-the-if-statement"></a>Tome decisões usando a instrução `if`

Crie um diretório chamado **branches-tutorial**. Torne-o o diretório atual e execute `dotnet new console -n BranchesAndLoops -o .`. Esse comando cria um novo aplicativo de console .NET Core no diretório atual.

Abra **Program.cs** em seu editor favorito e substitua a linha `Console.WriteLine("Hello World!");` pelo seguinte código:

```csharp
int a = 5;
int b = 6;
if (a + b > 10)
    Console.WriteLine("The answer is greater than 10.");
```

Experimente este código digitando `dotnet run` na sua janela do console. Você deve ver a mensagem "A resposta é maior do que 10." impressa no console.

Modifique a declaração de `b` para que a soma seja inferior a 10:

```csharp
int b = 3;
```

Digite `dotnet run` novamente. Como a resposta é inferior a 10, nada é impresso. A **condição** que você está testando é falsa. Não há qualquer código para execução, porque você escreveu apenas uma das ramificações possíveis para uma instrução `if`: a ramificação verdadeira.

> [!TIP]
> À medida que explora C# (ou qualquer linguagem de programação), você cometerá erros ao escrever o código. O compilador encontrará e reportará esses erros. Verifique atentamente a saída do erro e o código que gerou o erro. O erro do compilador geralmente pode ajudá-lo a localizar o problema.

Este primeiro exemplo mostra o poder dos tipos `if` e Booliano. Um *Booliano* é uma variável que pode ter um dos dois valores: `true` ou `false`. C# define um tipo especial, `bool` para variáveis Boolianas. A instrução `if` verifica o valor de um `bool`. Quando o valor é `true`, a instrução após `if` é executada. Caso contrário, é ignorada.

Esse processo de verificação de condições e execução de instruções com base nessas condições é muito eficiente.

## <a name="make-if-and-else-work-together"></a>Faça if e else funcionam juntas

Para executar um código diferente nos branches true e false, crie um branch `else` que será executado quando a condição for false. Experimente isto. Adicione as duas últimas linhas do código abaixo ao seu método `Main` (você já deve ter os quatro primeiros):

```csharp
int a = 5;
int b = 3;
if (a + b > 10)
    Console.WriteLine("The answer is greater than 10");
else
    Console.WriteLine("The answer is not greater than 10");
```

A instrução após a palavra-chave `else` é executada somente quando a condição que estiver sendo testada for `false`. A combinação de `if` e `else` com condições Boolianas fornece todos os recursos que você precisa para lidar com uma condição `true` e `false`.

> [!IMPORTANT]
> O recuo sob as instruções `if` e `else` é para leitores humanos.
> A linguagem C# não considera recuos ou espaços em branco como significativos.
> A instrução após a palavra-chave `if` ou `else` será executada com base na condição. Todos os exemplos neste tutorial seguem uma prática comum para recuar linhas com base no fluxo de controle de instruções.

Como o recuo não é significativo, você precisa usar `{` e `}` para indicar quando você quer que mais de uma instrução faça parte do bloco executado condicionalmente. Os programadores em C# normalmente usam essas chaves em todas as cláusulas `if` e `else`. O exemplo a seguir é igual ao que você acabou de criar. Modifique o código acima para coincidir com o código a seguir:

```csharp
int a = 5;
int b = 3;
if (a + b > 10)
{
    Console.WriteLine("The answer is greater than 10");
}
else
{
    Console.WriteLine("The answer is not greater than 10");
}
```

> [!TIP]
> No restante deste tutorial, todos os exemplos de código incluem as chaves, seguindo as práticas aceitas.

Você pode testar condições mais complicadas. Adicione o seguinte código ao seu método `Main` após o código que você escreveu até agora:

```csharp
int c = 4;
if ((a + b + c > 10) && (a == b))
{
    Console.WriteLine("The answer is greater than 10");
    Console.WriteLine("And the first number is equal to the second");
}
else
{
    Console.WriteLine("The answer is not greater than 10");
    Console.WriteLine("Or the first number is not equal to the second");
}
```

O símbolo `==` testa a *igualdade*. Usar `==` distingue o teste de igualdade de atribuição, que você viu em `a = 5`.

O `&&` representa "e". Isso significa que as duas condições devem ser verdadeiras para executar a instrução no branch verdadeiro.  Estes exemplos também mostram que você pode ter várias instruções em cada branch condicional, desde que você coloque-as entre `{` e `}`.

Você também pode usar `||` para representar "ou". Adicione o código a seguir após o que você escreveu até o momento:

```csharp
if ((a + b + c > 10) || (a == b))
{
    Console.WriteLine("The answer is greater than 10");
    Console.WriteLine("Or the first number is equal to the second");
}
else
{
    Console.WriteLine("The answer is not greater than 10");
    Console.WriteLine("And the first number is not equal to the second");
}
```

Modifique os valores de `a`, `b` e `c` e alterne entre `&&` e `||` para explorar. Você obterá mais compreensão de como os operadores `&&` e `||` funcionam.

Você terminou a primeira etapa. Antes de iniciar a próxima seção, vamos passar o código atual para um método separado. Isso facilita o começo do trabalho com um exemplo novo. Renomeie seu método `Main` como `ExploreIf` e escreva um novo método `Main` que chama `ExploreIf`. Quando você terminar, seu código deverá parecer com isto:

```csharp
using System;

namespace BranchesAndLoops
{
    class Program
    {
        static void ExploreIf()
        {
            int a = 5;
            int b = 3;
            if (a + b > 10)
            {
                Console.WriteLine("The answer is greater than 10");
            }
            else
            {
                Console.WriteLine("The answer is not greater than 10");
            }

            int c = 4;
            if ((a + b + c > 10) && (a > b))
            {
                Console.WriteLine("The answer is greater than 10");
                Console.WriteLine("And the first number is greater than the second");
            }
            else
            {
                Console.WriteLine("The answer is not greater than 10");
                Console.WriteLine("Or the first number is not greater than the second");
            }

            if ((a + b + c > 10) || (a > b))
            {
                Console.WriteLine("The answer is greater than 10");
                Console.WriteLine("Or the first number is greater than the second");
            }
            else
            {
                Console.WriteLine("The answer is not greater than 10");
                Console.WriteLine("And the first number is not greater than the second");
            }
        }

        static void Main(string[] args)
        {
            ExploreIf();
        }
    }
}
```

Comente a chamada para `ExploreIf()`. Isso tornará a saída menos congestionada enquanto você trabalha nesta seção:

```csharp
//ExploreIf();
```

O `//` inicia um **comentário** em C#. Os comentários são qualquer texto que você queira manter em seu código-fonte, mas não queria executar como código. O compilador não gera qualquer código executável a partir dos comentários.

## <a name="use-loops-to-repeat-operations"></a>Use loops para repetir operações

Nesta seção, você usa **loops** repetir as instruções. Tente este código em seu método `Main`:

```csharp
int counter = 0;
while (counter < 10)
{
    Console.WriteLine($"Hello World! The counter is {counter}");
    counter++;
}
```

A instrução `while` verifica uma condição e executa a instrução, ou bloco de instruções, após o `while`. Ela verifica repetidamente a condição e executa essas instruções até que a condição seja falsa.

Há outro operador novo neste exemplo. O `++` após a variável `counter` é o operador **increment**. Ele adiciona 1 ao valor de `counter` e armazena esse valor na variável `counter`.

> [!IMPORTANT]
> Verifique se a condição de loop `while` muda para false ao executar o código. Caso contrário, crie um **loop infinito**, para que seu programa nunca termine. Isso não é demonstrado neste exemplo, porque você tem que forçar o programa a encerrar usando **CTRL-C** ou outros meios.

O loop `while` testa a condição antes de executar o código seguindo `while`. O loop `do` ... `while` executa o código primeiro e, em seguida, verifica a condição. O loop do while é mostrado no código a seguir:

```csharp
int counter = 0;
do
{
    Console.WriteLine($"Hello World! The counter is {counter}");
    counter++;
} while (counter < 10);
```

Esse loop `do` e o loop `while` anterior produzem a mesma saída.

## <a name="work-with-the-for-loop"></a>Trabalhar com o loop for

O loop **for** é usado normalmente em C#. Tente este código em seu método Main():

```csharp
for (int index = 0; index < 10; index++)
{
    Console.WriteLine($"Hello World! The index is {index}");
}
```

Ele faz o mesmo trabalho que o loop `while` e o loop `do` que você já usou. A instrução `for` tem três partes que controlam o modo como ela funciona.

A primeira parte é o **inicializador for**: `int index = 0;` declara que `index` é a variável do loop, e define seu valor inicial como `0`.

A parte central é a **condição for**: `index < 10` declara que este loop `for` continuará sendo executado desde que o valor do contador seja inferior a 10.

A parte final é o **iterador for**: `index++` especifica como modificar a variável de loop depois de executar o bloco após a instrução `for`. Aqui, ela especifica que `index` deve ser incrementado com 1 sempre que o bloco for executado.

Experimente você mesmo. Tente o seguinte:

- Altere o inicializador para iniciar em um valor diferente.
- Altere a condição para parar em um valor diferente.

Quando terminar, vamos escrever um código para usar o que você aprendeu.

## <a name="combine-branches-and-loops"></a>Combinar branches e loops

Agora que você viu a instrução `if` e as construções de loop na linguagem C#, verifique se você pode escrever o código C# para encontrar a soma de todos os inteiros de 1 a 20 divisíveis por 3.  Veja algumas dicas:

- O operador `%` retorna o restante de uma operação de divisão.
- A instrução `if` retorna a condição para ver se um número deve ser parte da soma.
- O loop `for` pode ajudar você a repetir uma série de etapas para todos os números de 1 a 20.

Tente você mesmo. Depois verifique como você fez. Você deve obter 63 como resposta. Veja uma resposta possível [exibindo o código completo no GitHub](https://github.com/dotnet/samples/tree/master/csharp/branches-quickstart/Program.cs#L46-L54).

Você concluiu o tutorial "branches e loops".

Continue com o tutorial [Matrizes e coleções](arrays-and-collections.md) em seu próprio ambiente de desenvolvimento.

Saiba mais sobre esses conceitos nestes tópicos:

- [Instrução If e else](../../language-reference/keywords/if-else.md)
- [Instrução while](../../language-reference/keywords/while.md)
- [Instrução Do](../../language-reference/keywords/do.md)
- [Instrução for](../../language-reference/keywords/for.md)
