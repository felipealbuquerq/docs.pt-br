---
title: Aplicativo do Console
description: Este tutorial ensina vários recursos no .NET Core e da linguagem C#.
ms.date: 03/06/2017
ms.assetid: 883cd93d-50ce-4144-b7c9-2df28d9c11a0
ms.openlocfilehash: 4324b25daa253b3d2446955ad9ca57c2f0294f0c
ms.sourcegitcommit: 205b9a204742e9c77256d43ac9d94c3f82909808
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70851016"
---
# <a name="console-application"></a>Aplicativo do Console

Este tutorial ensina vários recursos no .NET Core e da linguagem C#. Você aprenderá:

- As noções básicas da CLI (Interface de Linha de Comando) do .NET Core
- A estrutura de um aplicativo de console C#
- E/S do Console
- Fundamentos das APIs de E/S de arquivo no .NET
- Os fundamentos da programação assíncrona controlada por tarefas no .NET Core

Você compilará um aplicativo que lê um arquivo de texto e exibe o conteúdo desse arquivo de texto no console. A saída para o console é conduzida a fim de corresponder à leitura em voz alta. É possível acelerar ou diminuir o ritmo pressionando as teclas "<" (menor que) ou ">" (maior que).

Há vários recursos neste tutorial. Vamos compilá-los individualmente.

## <a name="prerequisites"></a>Pré-requisitos

Você precisará configurar seu computador para executar o .NET Core. Você pode encontrar as instruções de instalação na página de [downloads do .NET Core](https://dotnet.microsoft.com/download) . Execute esse aplicativo no Windows, Linux, macOS ou em um contêiner do Docker.
Será necessário instalar o editor de código de sua preferência.

## <a name="create-the-application"></a>Criar o aplicativo

A primeira etapa é criar um novo aplicativo. Abra um prompt de comando e crie um novo diretório para seu aplicativo. Torne ele o diretório atual. Digite o comando `dotnet new console` no prompt de comando. Isso cria os arquivos iniciais de um aplicativo "Olá, Mundo" básico.

Antes de começar as modificações, vamos percorrer as etapas para execução do aplicativo simples Hello World. Depois de criar o aplicativo, digite `dotnet restore` no prompt de comando. Esse comando executa o processo de restauração do pacote NuGet. O NuGet é um gerenciador de pacotes do .NET. Esse comando baixa qualquer uma das dependências ausentes para seu projeto. Como esse é um novo projeto, nenhuma das dependências foram aplicadas, portanto, a primeira execução baixará a estrutura do .NET Core. Após essa etapa inicial, você só precisará executar o `dotnet restore` ao adicionar novos pacotes dependentes, ou atualizar as versões de qualquer uma de suas dependências.

[!INCLUDE[DotNet Restore Note](~/includes/dotnet-restore-note.md)]

Depois de restaurar os pacotes, execute `dotnet build`. Isso executa o mecanismo de compilação e cria o executável de seu aplicativo. Por fim, execute `dotnet run` para executar o aplicativo.

O código simples do aplicativo Hello World está totalmente em Program.cs. Abra esse arquivo com o seu editor de texto favorito. Estamos prestes a fazer nossas primeiras alterações.
Na parte superior do arquivo, confira uma instrução using:

```csharp
using System;
```

Essa instrução informa ao compilador que quaisquer tipos do namespace `System` estão dentro do escopo. Assim como em outras linguagens orientadas a objeto que você pode ter usado, o C# usa namespaces para organizar tipos. Este programa Olá, Mundo não é diferente. Você pode ver que o programa é encerrado no namespace com o nome baseado no nome do diretório atual. Para este tutorial, vamos alterar o nome do namespace para `TeleprompterConsole`:

```csharp
namespace TeleprompterConsole
```

## <a name="reading-and-echoing-the-file"></a>Como ler e exibir o arquivo

O primeiro recurso a ser adicionado é a capacidade de ler um arquivo de texto e a exibição de todo esse texto para um console. Primeiro, vamos adicionar um arquivo de texto. Copie o arquivo [sampleQuotes.txt](https://github.com/dotnet/samples/raw/master/csharp/getting-started/console-teleprompter/sampleQuotes.txt) do repositório do GitHub para este [exemplo](https://github.com/dotnet/samples/tree/master/csharp/getting-started/console-teleprompter) no diretório de seu projeto. Isso servirá como o script de seu aplicativo. Se desejar obter informações sobre como baixar o aplicativo de exemplo para este tópico, consulte as instruções no tópico [Exemplos e Tutoriais](../../samples-and-tutorials/index.md#viewing-and-downloading-samples).

Em seguida, adicione o seguinte método em sua classe `Program` (logo abaixo do método `Main`):

```csharp
static IEnumerable<string> ReadFrom(string file)
{
    string line;
    using (var reader = File.OpenText(file))
    {
        while ((line = reader.ReadLine()) != null)
        {
            yield return line;
        }
    }
}
```

Esse método usa tipos de dois namespaces novos. Para que isso seja compilado, será necessário adicionar as duas linhas a seguir na parte superior do arquivo:

```csharp
using System.Collections.Generic;
using System.IO;
```

A interface <xref:System.Collections.Generic.IEnumerable%601> é definida no namespace <xref:System.Collections.Generic>. A classe <xref:System.IO.File> é definida no namespace <xref:System.IO>.

Esse método é um tipo especial de método C# chamado de *Método iterador*. Os métodos enumeradores retornam sequências que são avaliadas lentamente. Isso significa que cada item na sequência é gerado conforme a solicitação do código que está consumindo a sequência. Os métodos enumeradores contêm uma ou mais instruções [`yield return`](../language-reference/keywords/yield.md). O objeto retornado pelo método `ReadFrom` contém o código para gerar cada item na sequência. Neste exemplo, isso envolve a leitura da próxima linha de texto do arquivo de origem e o retorno dessa cadeia de caracteres. Toda vez que o código de chamada solicita o próximo item da sequência, o código lê a próxima linha de texto do arquivo e a retorna. Após a leitura completa do arquivo, a sequência indicará que não há mais itens.

Há dois outros elementos da sintaxe em C# que podem ser novidade para você. A instrução [`using`](../language-reference/keywords/using-statement.md) nesse método gerencia a limpeza de recursos. A variável inicializada na instrução `using` (`reader`, neste exemplo) deve implementar a interface <xref:System.IDisposable>. Essa interface define um único método, `Dispose`, que deve ser chamado quando o recurso for liberado. O compilador gera essa chamada quando a execução atingir a chave de fechamento da instrução `using`. O código gerado pelo compilador garante que o recurso seja liberado, mesmo se uma exceção for lançada do código no bloco definido pela instrução using.

A variável `reader` é definida usando a palavra-chave `var`. [`var`](../language-reference/keywords/var.md) define uma *variável local de tipo implícito*. Isso significa que o tipo da variável é determinado pelo tipo de tempo de compilação do objeto atribuído à variável. Aqui, esse é o valor retornado do método <xref:System.IO.File.OpenText(System.String)>, que é um objeto <xref:System.IO.StreamReader>.

Agora, vamos preencher o código para ler o arquivo no método `Main`:

```csharp
var lines = ReadFrom("sampleQuotes.txt");
foreach (var line in lines)
{
    Console.WriteLine(line);
}
```

Execute o programa (usando `dotnet run`, e você poderá ver todas as linhas impressa no console).

## <a name="adding-delays-and-formatting-output"></a>Adicionar atrasos e formatar a saída

O que você possui está sendo exibido muito rápido para permitir a leitura em voz alta. Agora você precisa adicionar os atrasos na saída. Ao começar, você compilará parte do código principal que permite o processamento assíncrono. No entanto, essas primeiras etapas seguirão alguns antipadrões. Os antipadrões são indicados nos comentários durante a adição do código, e o código será atualizado em etapas posteriores.

Há duas etapas nesta seção. Primeiro, você atualizará o método iterador a fim de retornar palavras individuais em vez de linhas inteiras. Isso é feito com estas modificações. Substitua a instrução `yield return line;` pelo seguinte código:

```csharp
var words = line.Split(' ');
foreach (var word in words)
{
    yield return word + " ";
}
yield return Environment.NewLine;
```

Em seguida, será necessário modificar a forma como você consume as linhas do arquivo, e adicionar um atraso depois de escrever cada palavra. Substitua a instrução `Console.WriteLine(line)` no método `Main` pelo seguinte bloco:

```csharp
Console.Write(line);
if (!string.IsNullOrWhiteSpace(line))
{
    var pause = Task.Delay(200);
    // Synchronously waiting on a task is an
    // anti-pattern. This will get fixed in later
    // steps.
    pause.Wait();
}
```

A classe <xref:System.Threading.Tasks.Task> é o namespace <xref:System.Threading.Tasks>, portanto, você precisa adicionar essa instrução `using` na parte superior do arquivo:

```csharp
using System.Threading.Tasks;
```

Execute o exemplo e verifique a saída. Agora, cada palavra única é impressa, seguida por um atraso de 200 ms. No entanto, a saída exibida mostra alguns problemas, pois o arquivo de texto de origem contém várias linhas com mais de 80 caracteres sem uma quebra de linha. Isso pode ser difícil de ler durante a rolagem da tela. Mas também é fácil de corrigir. Apenas mantenha o controle do comprimento de cada linha e gere uma nova linha sempre que o comprimento atingir um certo limite. Declare uma variável local após a declaração de `words` no método `ReadFrom` que contém o comprimento da linha:

```csharp
var lineLength = 0;
```

Em seguida, adicione o seguinte código após a instrução `yield return word + " ";` (antes da chave de fechamento):

```csharp
lineLength += word.Length + 1;
if (lineLength > 70)
{
    yield return Environment.NewLine;
    lineLength = 0;
}
```

Execute o exemplo e você poderá ler em voz alta de acordo com o ritmo pré-configurado.

## <a name="async-tasks"></a>Tarefas assíncronas

Nesta etapa final, você adicionará o código para gravar a saída de forma assíncrona em uma tarefa, enquanto executa também outra tarefa para ler a entrada do usuário, casos ele queira acelerar ou diminuir o ritmo da exibição do texto ou interromper a exibição do texto por completo. Essa etapa tem alguns passos e, no final, você terá todas as atualizações necessárias.
A primeira etapa é criar um método de retorno <xref:System.Threading.Tasks.Task> assíncrono que representa o código que você criou até agora para ler e exibir o arquivo.

Adicione este método à sua classe `Program` (ele é obtido do corpo de seu método `Main`):

```csharp
private static async Task ShowTeleprompter()
{
    var words = ReadFrom("sampleQuotes.txt");
    foreach (var word in words)
    {
        Console.Write(word);
        if (!string.IsNullOrWhiteSpace(word))
        {
            await Task.Delay(200);
        }
    }
}
```

Você observará duas alterações. Primeiro, no corpo do método, em vez de chamar <xref:System.Threading.Tasks.Task.Wait> para aguardar de forma síncrona a conclusão de uma tarefa, essa versão usa a palavra-chave `await`. Para fazer isso, você precisa adicionar o modificador `async` à assinatura do método. Esse método retorna `Task`. Observe que não há instruções return que retornam um objeto `Task`. Em vez disso, esse objeto `Task` é criado pelo código gerado pelo compilador quando você usa o operador `await`. Você pode imaginar que esse método retorna quando atinge um `await`. A `Task` retornada indica que o trabalho não foi concluído.
O método será retomado quando a tarefa em espera for concluída. Após a execução completa, a `Task` retornada indicará a conclusão.
O código de chamada pode monitorar essa `Task` retornada para determinar quando ela foi concluída.

Chame esse novo método em seu método `Main`:

```csharp
ShowTeleprompter().Wait();
```

Aqui, em `Main`, o código aguarda de forma síncrona. Use o operador `await` em vez de esperar de forma síncrona sempre que possível. Porém, no método `Main` do aplicativo de console, não é possível usar o operador `await`. Isso resultaria no encerramento do aplicativo antes da conclusão de todas as tarefas.

> [!NOTE]
> Caso use o C# 7.1 ou posterior, você poderá criar aplicativos de console com o método [`async` `Main`](../whats-new/csharp-7-1.md#async-main).

Em seguida, é necessário escrever o segundo método assíncrono a ser lido no Console e ficar atento às teclas "<" (menor que), ">" (maior que) e "X" ou "x". Este é o método que você adiciona à tarefa:

```csharp
private static async Task GetInput()
{
    var delay = 200;
    Action work = () =>
    {
        do {
            var key = Console.ReadKey(true);
            if (key.KeyChar == '>')
            {
                delay -= 10;
            }
            else if (key.KeyChar == '<')
            {
                delay += 10;
            }
            else if (key.KeyChar == 'X' || key.KeyChar == 'x')
            {
                break;
            }
        } while (true);
    };
    await Task.Run(work);
}
```

Isso cria uma expressão lambda para representar um delegado <xref:System.Action> que lê uma chave no Console e modifica uma variável local que representa o atraso quando o usuário pressiona as teclas "<" (menor que) ou ">" (maior que). O método de delegado termina quando o usuário pressiona a tecla "X" ou "x", que permitem ao usuário interromper a exibição de texto a qualquer momento.
Esse método usa <xref:System.Console.ReadKey> para bloquear e aguardar até que o usuário pressione uma tecla.

Para concluir esse recurso, você precisa criar um novo método de retorno `async Task` que inicia essas duas tarefas (`GetInput` e `ShowTeleprompter`) e também gerencia os dados compartilhados entre essas tarefas.

É hora de criar uma classe que pode manipular os dados compartilhados entre essas duas tarefas. Essa classe contém duas propriedades públicas: o atraso e um sinalizador `Done` para indicar que o arquivo foi lido completamente:

```csharp
namespace TeleprompterConsole
{
    internal class TelePrompterConfig
    {
        public int DelayInMilliseconds { get; private set; } = 200;

        public void UpdateDelay(int increment) // negative to speed up
        {
            var newDelay = Min(DelayInMilliseconds + increment, 1000);
            newDelay = Max(newDelay, 20);
            DelayInMilliseconds = newDelay;
        }

        public bool Done { get; private set; }

        public void SetDone()
        {
            Done = true;
        }
    }
}
```

Coloque essa classe em um novo arquivo e cerque-a pelo namespace `TeleprompterConsole`, conforme mostrado acima. Também é necessário adicionar uma instrução `using static` para que você possa fazer referência ao método `Min` e `Max` sem os nomes de classe ou namespace delimitadores. Uma instrução [`using static`](../language-reference/keywords/using-static.md) importa os métodos de uma classe. Isso é o oposto das instruções `using` usadas até este ponto, as quais importaram todas as classes de um namespace.

```csharp
using static System.Math;
```

Em seguida, atualize os métodos `ShowTeleprompter` e `GetInput` para usar o novo objeto `config`. Escreva um método final `async` de retorno de `Task` para iniciar as duas tarefas e sair quando a primeira tarefa for concluída:

```csharp
private static async Task RunTeleprompter()
{
    var config = new TelePrompterConfig();
    var displayTask = ShowTeleprompter(config);

    var speedTask = GetInput(config);
    await Task.WhenAny(displayTask, speedTask);
}
```

O novo método aqui é a chamada <xref:System.Threading.Tasks.Task.WhenAny(System.Threading.Tasks.Task[])>. Isso cria uma `Task` que termina assim que qualquer uma das tarefas na lista de argumentos for concluída.

Depois, atualize os métodos `ShowTeleprompter` e `GetInput` para usar o objeto `config` para o atraso:

```csharp
private static async Task ShowTeleprompter(TelePrompterConfig config)
{
    var words = ReadFrom("sampleQuotes.txt");
    foreach (var word in words)
    {
        Console.Write(word);
        if (!string.IsNullOrWhiteSpace(word))
        {
            await Task.Delay(config.DelayInMilliseconds);
        }
    }
    config.SetDone();
}

private static async Task GetInput(TelePrompterConfig config)
{
    Action work = () =>
    {
        do {
            var key = Console.ReadKey(true);
            if (key.KeyChar == '>')
                config.UpdateDelay(-10);
            else if (key.KeyChar == '<')
                config.UpdateDelay(10);
            else if (key.KeyChar == 'X' || key.KeyChar == 'x')
                config.SetDone();
        } while (!config.Done);
    };
    await Task.Run(work);
}
```

Essa nova versão de `ShowTeleprompter` chama um novo método na classe `TeleprompterConfig`. Agora, você precisa atualizar `Main` para chamar `RunTeleprompter` em vez de `ShowTeleprompter`:

```csharp
RunTeleprompter().Wait();
```

## <a name="conclusion"></a>Conclusão

Este tutorial mostrou a você alguns recursos da linguagem C# e as bibliotecas .NET Core relacionadas ao trabalho em aplicativos de Console.
Use esse conhecimento como base para explorar mais sobre a linguagem e sobre as classes apresentadas aqui. Você já viu os fundamentos de E/S do Arquivo e do Console, uso com bloqueio e sem bloqueio da programação assíncrona controlada por tarefa, um tour pela linguagem C# e como os programas em C# são organizados, além da Interface de linha de comando e ferramentas do .NET Core.

Para obter mais informações sobre E/S de arquivo, consulte o tópico [E/S de arquivo e de fluxo](../../standard/io/index.md). Para obter mais informações sobre o modelo de programação assíncrona usado neste tutorial, consulte os tópicos [Programação assíncrona controlada por tarefas](../..//standard/parallel-programming/task-based-asynchronous-programming.md) e [Programação assíncrona](../async.md).
