---
title: Programação assíncrona com Async e Await (Visual Basic)
ms.date: 07/20/2015
ms.assetid: bd7e462b-583b-4395-9c36-45aa9e61072c
ms.openlocfilehash: 0d8810da424b0759dcfba882efe462514a14145a
ms.sourcegitcommit: b1cfd260928d464d91e20121f9bdba7611c94d71
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67505960"
---
# <a name="asynchronous-programming-with-async-and-await-visual-basic"></a>Programação assíncrona com Async e Await (Visual Basic)

É possível evitar gargalos de desempenho e aprimorar a resposta geral do seu aplicativo usando a programação assíncrona. No entanto, as técnicas tradicionais para escrever aplicativos assíncronos podem ser complicadas, dificultando sua escrita, depuração e manutenção.

O Visual Studio 2012 apresenta uma abordagem simplificada, programação assíncrona, que aproveita o suporte assíncrono no .NET Framework 4.5 e superior, bem como no Windows Runtime. O compilador faz o trabalho difícil que o desenvolvedor costumava fazer, e seu aplicativo mantém a estrutura lógica que se assemelha ao código síncrono. Como resultado, você obtém todas as vantagens da programação assíncrona com uma fração do esforço.

Este tópico oferece uma visão geral de quando e como usar a programação assíncrona e inclui links para tópicos de suporte que contêm detalhes e exemplos.

## <a name="BKMK_WhentoUseAsynchrony"></a> A assincronia melhora a capacidade de resposta

A assincronia é essencial para atividades potencialmente causadoras de bloqueios, como quando seu aplicativo acessa a Web. O acesso a um recurso da Web às vezes é lento ou atrasado. Se tal atividade for bloqueada dentro de um processo síncrono, todo o aplicativo deverá esperar. Em um processo assíncrono, o aplicativo poderá prosseguir com outro trabalho que não dependa do recurso da Web até a tarefa potencialmente causadora do bloqueio terminar.

A tabela a seguir mostra a áreas típicas onde a programação assíncrona melhora a resposta. As APIs listadas do .NET Framework 4.5 e do Windows Runtime contêm métodos que dão suporte à programação assíncrona.

|Área do aplicativo|APIs de suporte que contêm métodos assíncronos|
|----------------------|------------------------------------------------|
|Acesso à Web|<xref:System.Net.Http.HttpClient>, <xref:Windows.Web.Syndication.SyndicationClient>|
|Trabalhando com arquivos|<xref:Windows.Storage.StorageFile>, <xref:System.IO.StreamWriter>, <xref:System.IO.StreamReader>, <xref:System.Xml.XmlReader>|
|Trabalhando com imagens|<xref:Windows.Media.Capture.MediaCapture>, <xref:Windows.Graphics.Imaging.BitmapEncoder>, <xref:Windows.Graphics.Imaging.BitmapDecoder>|
|Programação WCF|[Operações síncronas e assíncronas](../../../../framework/wcf/synchronous-and-asynchronous-operations.md)|
|||

A assincronia é especialmente importante para aplicativos que acessam o thread de interface de usuário porque todas as atividades relacionadas à interface do usuário normalmente compartilham um único thread. Se um processo for bloqueado em um aplicativo síncrono, todos serão bloqueados. Seu aplicativo para de responder, o que poderia levar você a concluir que ele falhou quando, na verdade, está apenas aguardando.

Quando você usa métodos assíncronos, o aplicativo continua a responder à interface do usuário. Você poderá redimensionar ou minimizar uma janela, por exemplo, ou fechar o aplicativo se você não desejar aguardar sua conclusão.

A abordagem baseada em assincronia adiciona o equivalente de uma transmissão automática à lista de opções disponíveis para escolha ao criar operações assíncronas. Ou seja, você obtém todos os benefícios da programação assíncrona tradicional, mas com muito menos esforço do desenvolvedor.

## <a name="BKMK_HowtoWriteanAsyncMethod"></a> Os métodos assíncronos são mais fáceis de escrever

As palavras-chave [Async](../../../../visual-basic/language-reference/modifiers/async.md) e [Await](../../../../visual-basic/language-reference/operators/await-operator.md) no Visual Basic são a parte central da programação assíncrona. Ao usar essas duas palavras-chave, você pode usar recursos do .NET Framework ou do Windows Runtime para criar um método assíncrono quase que tão facilmente como cria um método síncrono. Os métodos assíncronos que você define usando `Async` e `Await` são chamados de métodos assíncronos.

O exemplo a seguir mostra um método assíncrono. Quase tudo no código deve ser completamente familiar para você. Os comentários chamam os recursos que você deve adicionar para criar a assincronia.

O arquivo de exemplo completo do WPF (Windows Presentation Foundation) pode ser encontrado no final deste tópico. Você também pode baixar o exemplo de [Exemplo de assincronia: exemplo de “Programação assíncrona com Async e Await”](https://code.msdn.microsoft.com/Async-Sample-Example-from-9b9f505c).

```vb
' Three things to note in the signature:
'  - The method has an Async modifier.
'  - The return type is Task or Task(Of T). (See "Return Types" section.)
'    Here, it is Task(Of Integer) because the return statement returns an integer.
'  - The method name ends in "Async."
Async Function AccessTheWebAsync() As Task(Of Integer)

    ' You need to add a reference to System.Net.Http to declare client.
    Dim client As HttpClient = New HttpClient()

    ' GetStringAsync returns a Task(Of String). That means that when you await the
    ' task you'll get a string (urlContents).
    Dim getStringTask As Task(Of String) = client.GetStringAsync("https://msdn.microsoft.com")

    ' You can do work here that doesn't rely on the string from GetStringAsync.
    DoIndependentWork()

    ' The Await operator suspends AccessTheWebAsync.
    '  - AccessTheWebAsync can't continue until getStringTask is complete.
    '  - Meanwhile, control returns to the caller of AccessTheWebAsync.
    '  - Control resumes here when getStringTask is complete.
    '  - The Await operator then retrieves the string result from getStringTask.
    Dim urlContents As String = Await getStringTask

    ' The return statement specifies an integer result.
    ' Any methods that are awaiting AccessTheWebAsync retrieve the length value.
    Return urlContents.Length
End Function
```

Se `AccessTheWebAsync` não tiver nenhum trabalho que possa fazer entre chamar `GetStringAsync` e aguardar a conclusão, você poderá simplificar o código ao chamar e esperar na instrução única a seguir.

```vb
Dim urlContents As String = Await client.GetStringAsync()
```

 As características a seguir resumem o que transforma o exemplo anterior em um método assíncrono.

- A assinatura do método inclui um modificador `Async`.

- O nome de um método assíncrono, por convenção, termina com um sufixo "Async".

- O tipo de retorno é um dos seguintes tipos:

  - <xref:System.Threading.Tasks.Task%601> se o método possui uma instrução de retorno em que o operando tem o tipo TResult.

  - <xref:System.Threading.Tasks.Task> se o método não possui instrução de retorno alguma ou se ele possui uma instrução de retorno sem operando.

  - [Sub](../../../../visual-basic/programming-guide/language-features/procedures/sub-procedures.md) se você estiver escrevendo um manipulador de eventos assíncronos.

  Para obter mais informações, consulte “Tipos e parâmetros de retorno” mais adiante neste tópico.

- O método geralmente inclui pelo menos uma expressão await, a qual marca um ponto onde o método não pode continuar até que a operação assíncrona aguardada seja concluída. Enquanto isso, o método é suspenso e o controle retorna para o chamador do método. A próxima seção deste tópico ilustra o que acontece no ponto de suspensão.

Em métodos assíncronos, você usa as palavras-chave e os tipos fornecidos para indicar o que deseja fazer, e o compilador faz o resto, inclusive acompanhar o que deve acontecer quando o controle retorna a um ponto de espera em um método suspenso. Alguns processos de rotina, como loops e a manipulação de exceções, podem ser difíceis de manipular em um código assíncrono tradicional. Em um método assíncrono, você escreve esses elementos da mesma forma que faria em uma solução síncrona, e o problema é resolvido.

Para obter mais informações sobre assincronia nas versões anteriores do .NET Framework, consulte [Programação assíncrona do .NET Framework tradicional e TPL](../../../../standard/parallel-programming/tpl-and-traditional-async-programming.md).

## <a name="BKMK_WhatHappensUnderstandinganAsyncMethod"></a> O que acontece em um método assíncrono

O mais importante que você deve compreender na programação assíncrona é a forma como o fluxo de controle avança de um método para outro. O diagrama a seguir pode ser usado para conduzi-lo pelo processo.

![Diagrama que mostra o rastreamento de um programa assíncrono.](./media/index/navigation-trace-async-program.png)

Os números no diagrama correspondem às etapas a seguir.

1. Um manipulador de eventos chama e aguarda o método assíncrono `AccessTheWebAsync`.

2. `AccessTheWebAsync` cria uma instância de <xref:System.Net.Http.HttpClient> e chama o método assíncrono <xref:System.Net.Http.HttpClient.GetStringAsync%2A> para baixar o conteúdo de um site como uma cadeia de caracteres.

3. Algo acontece em `GetStringAsync` que suspende o andamento. Talvez ele deva aguardar o download de um site ou alguma outra atividade causadora de bloqueio. Para evitar o bloqueio de recursos, `GetStringAsync` transfere o controle para seu chamador, `AccessTheWebAsync`.

     `GetStringAsync` retorna um <xref:System.Threading.Tasks.Task%601>, em que TResult é uma cadeia de caracteres e `AccessTheWebAsync` atribui a tarefa à variável `getStringTask`. A tarefa representa o processo contínuo para a chamada a `GetStringAsync`, com um compromisso de produzir um valor de cadeia de caracteres real quando o trabalho estiver concluído.

4. Como o `getStringTask` ainda não foi esperado, `AccessTheWebAsync` pode continuar com outro trabalho que não depende do resultado final de `GetStringAsync`. O trabalho é representado por uma chamada ao método síncrono `DoIndependentWork`.

5. `DoIndependentWork` é um método síncrono que faz seu trabalho e retorna ao seu chamador.

6. `AccessTheWebAsync` está sem trabalho que ele possa executar sem um resultado de `getStringTask`. Em seguida, `AccessTheWebAsync` deseja calcular e retornar o comprimento da cadeia de caracteres baixada, mas o método não poderá calcular o valor enquanto o método tiver a cadeia de caracteres.

     Portanto, `AccessTheWebAsync` usa um operador await para suspender seu andamento e para transferir o controle para o método que chamou `AccessTheWebAsync`. `AccessTheWebAsync` retorna um `Task<int>` (`Task(Of Integer)` no Visual Basic) para o chamador. A tarefa representa uma promessa de produzir um resultado inteiro que é o comprimento da cadeia de caracteres baixada.

    > [!NOTE]
    > Se `GetStringAsync` (e portanto `getStringTask`) for concluído antes que `AccessTheWebAsync` o espere, o controle permanecerá em `AccessTheWebAsync`. A despesa de suspender e então retornar para `AccessTheWebAsync` seria desperdiçada caso o processo assíncrono chamado (`getStringTask`) já tivesse sido concluído e AccessTheWebSync não tivesse que aguardar o resultado final.

     Dentro do chamador (manipulador de eventos neste exemplo), o processamento do padrão continua. O chamador pode fazer outro trabalho que não dependa do resultado de `AccessTheWebAsync` antes de aguardar o resultado, ou o chamador pode aguardar imediatamente.   O manipulador de eventos está aguardando `AccessTheWebAsync` e `AccessTheWebAsync` está aguardando `GetStringAsync`.

7. `GetStringAsync` completa e produz um resultado de cadeia de caracteres. O resultado da cadeia de caracteres não é retornado pela chamada para `GetStringAsync` da maneira que você poderia esperar. (Lembre-se de que o método já retornou uma tarefa na etapa 3.) Em vez disso, o resultado da cadeia de caracteres é armazenado na tarefa que representa a conclusão do método, `getStringTask`. O operador await recupera o resultado de `getStringTask`. A instrução de atribuição atribui o resultado retornado a `urlContents`.

8. Quando `AccessTheWebAsync` tem o resultado da cadeia de caracteres, o método pode calcular o comprimento da cadeia de caracteres. Em seguida, o trabalho de `AccessTheWebAsync` também é concluído e o manipulador de eventos de espera poderá retomar. No exemplo completo no final do tópico, é possível confirmar que o manipulador de eventos recuperou e imprimiu o valor do comprimento do resultado.

Se você não tiver experiência em programação assíncrona, considere por um minuto a diferença entre o comportamento síncrono e o assíncrono. Um método síncrono retorna quando seu trabalho é concluído (etapa 5), mas um método assíncrono retorna um valor de tarefa quando seu trabalho está suspenso (etapas 3 e 6). Quando o método assíncrono eventualmente concluir seu trabalho, a tarefa será marcada como concluída e o resultado, se houver, será armazenado na tarefa.

Para obter mais informações sobre o fluxo de controle, consulte [Fluxo de controle em programas assíncronos (Visual Basic)](../../../../visual-basic/programming-guide/concepts/async/control-flow-in-async-programs.md).

## <a name="BKMK_APIAsyncMethods"></a> Métodos de assíncronos da API

Você pode estar curioso para saber onde encontrar métodos como `GetStringAsync` que oferecem suporte à programação assíncrona. O .NET Framework 4.5 ou superior contém muitos membros que funcionam com `Async` e `Await`. É possível identificar esses membros pelo sufixo “Async” que é acrescentado ao nome do membro e um tipo de retorno de <xref:System.Threading.Tasks.Task> ou de <xref:System.Threading.Tasks.Task%601>. Por exemplo, a classe `System.IO.Stream` contém métodos como <xref:System.IO.Stream.CopyToAsync%2A>, <xref:System.IO.Stream.ReadAsync%2A> e <xref:System.IO.Stream.WriteAsync%2A>, juntamente com os métodos síncronos <xref:System.IO.Stream.CopyTo%2A>, <xref:System.IO.Stream.Read%2A> e <xref:System.IO.Stream.Write%2A>.

O Windows Runtime também contém vários métodos que você pode usar com `Async` e `Await` em aplicativos do Windows. Para obter mais informações e métodos de exemplo, consulte [chamar APIs assíncronas em C# ou o Visual Basic](/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic), [programação assíncrona (aplicativos de tempo de execução do Windows)](https://docs.microsoft.com/previous-versions/windows/apps/hh464924(v=win.10)), e [WhenAny: A ponte entre o .NET Framework e o tempo de execução do Windows](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2013/jj635140(v=vs.120)).

## <a name="BKMK_Threads"></a> Threads

Os métodos assíncronos destinam-se a ser operações não causadoras de bloqueios. Uma expressão `Await` em um método assíncrono não bloqueia o thread atual enquanto a tarefa aguardada está em execução. Em vez disso, a expressão anterior assina o restante do método como uma continuação e retorna o controle para o chamador do método assíncrono.

As palavras-chave `Async` e `Await` não fazem com que threads adicionais sejam criados. Os métodos assíncronos não exigem multithreading, pois um método assíncrono não executa em seu próprio thread. O método é executado no contexto de sincronização atual e usa tempo no thread somente quando o método está ativo. É possível usar <xref:System.Threading.Tasks.Task.Run%2A?displayProperty=nameWithType> para mover o trabalho de CPU associado a um thread em segundo plano, mas um thread em segundo plano não ajuda com um processo que está apenas aguardando que os resultados tornem-se disponíveis.

A abordagem baseada em async para a programação assíncrona é preferível às abordagens existentes em quase todos os casos. Em particular, essa abordagem é melhor do que <xref:System.ComponentModel.BackgroundWorker> para operações de vinculado à e/S porque o código é mais simples e você não precisa se proteger contra condições de corrida. Em combinação com <xref:System.Threading.Tasks.Task.Run%2A?displayProperty=nameWithType>, a programação async é melhor do que <xref:System.ComponentModel.BackgroundWorker> para operações associadas à CPU porque a programação async separa os detalhes de coordenação da execução do seu código do trabalho que `Task.Run` transfere ao threadpool.

## <a name="BKMK_AsyncandAwait"></a> Async e Await

Se especificar que um método é um método assíncrono usando um modificador [Async](../../../../visual-basic/language-reference/modifiers/async.md), você habilitará os dois recursos a seguir.

- O método assíncrono marcado pode usar [Await](../../../../visual-basic/language-reference/operators/await-operator.md) para designar pontos de suspensão. O operador await informa ao compilador que o método assíncrono não poderá continuar além daquele ponto até que o processo assíncrono aguardado seja concluído. Enquanto isso, o controle retorna para o chamador do método assíncrono.

  A suspensão de um método assíncrono em uma expressão `Await` não constitui uma saída de método e os blocos de `Finally` não são executados.

- O método assíncrono marcado pode ele próprio ser aguardado por métodos que o chamam.

Um método assíncrono normalmente contém uma ou mais ocorrências de um operador `Await`, mas a ausência de expressões `Await` não causa um erro do compilador. Se um método assíncrono não usa um operador `Await` para marcar um ponto de suspensão, o método é executado da mesma forma que o método síncrono, independentemente do modificador `Async`. O compilador emite um aviso para esses métodos.

`Async` e `Await` são palavras-chave contextuais. Para obter mais informações e exemplos, consulte os seguintes tópicos:

- [Async](../../../../visual-basic/language-reference/modifiers/async.md)

- [Operador Await](../../../../visual-basic/language-reference/operators/await-operator.md)

## <a name="BKMK_ReturnTypesandParameters"></a> Tipos e parâmetros de retorno

Na programação .NET Framework, um método assíncrono costuma retornar um <xref:System.Threading.Tasks.Task> ou um <xref:System.Threading.Tasks.Task%601>. Dentro de um método assíncrono, um operador `Await` é aplicado a uma tarefa que é retornada de uma chamada para outro método assíncrono.

Você especificará <xref:System.Threading.Tasks.Task%601> como o tipo de retorno se o método contiver uma instrução [Return](../../../../visual-basic/language-reference/statements/return-statement.md) que especifica um operando do tipo `TResult`.

Você usará `Task` como o tipo de retorno caso o método não possua nenhuma instrução return ou tenha uma instrução return que não retorna um operando.

O exemplo a seguir mostra como você declara e chama um método que retorna um <xref:System.Threading.Tasks.Task%601> ou <xref:System.Threading.Tasks.Task>.

```vb
' Signature specifies Task(Of Integer)
Async Function TaskOfTResult_MethodAsync() As Task(Of Integer)

    Dim hours As Integer
    ' . . .
    ' Return statement specifies an integer result.
    Return hours
End Function

' Calls to TaskOfTResult_MethodAsync
Dim returnedTaskTResult As Task(Of Integer) = TaskOfTResult_MethodAsync()
Dim intResult As Integer = Await returnedTaskTResult
' or, in a single statement
Dim intResult As Integer = Await TaskOfTResult_MethodAsync()

' Signature specifies Task
Async Function Task_MethodAsync() As Task

    ' . . .
    ' The method has no return statement.
End Function

' Calls to Task_MethodAsync
Task returnedTask = Task_MethodAsync()
Await returnedTask
' or, in a single statement
Await Task_MethodAsync()
```

Cada tarefa retornada representa um trabalho em andamento. Uma tarefa encapsula informações sobre o estado do processo assíncrono e, consequentemente, o resultado final do processo ou a exceção que o processo apresenta quando não é bem-sucedido.

Um método assíncrono também pode ser um método `Sub`. Esse tipo de retorno é usado principalmente para definir manipuladores de eventos, nos quais o tipo de retorno é necessário. Os manipuladores de eventos assíncronos geralmente servem como o ponto de partida para programas assíncronos.

Um método assíncrono que é um procedimento `Sub` não pode ser esperado, e o chamador não pode capturar nenhuma exceção acionada pelo método.

O método assíncrono não pode declarar parâmetros [ByRef](../../../../visual-basic/language-reference/modifiers/byref.md), mas pode chamar métodos com tais parâmetros.

Para obter mais informações e exemplos, consulte [Tipos de retorno assíncronos (Visual Basic)](../../../../visual-basic/programming-guide/concepts/async/async-return-types.md). Para obter mais informações sobre como capturar exceções nos métodos assíncronos, consulte a [Instrução Try... Catch... Finally](../../../../visual-basic/language-reference/statements/try-catch-finally-statement.md).

As APIs assíncronas na programação do Windows Runtime têm um dos seguintes tipos de retorno, que são semelhantes às tarefas:

- <xref:Windows.Foundation.IAsyncOperation%601>, que corresponde a <xref:System.Threading.Tasks.Task%601>

- <xref:Windows.Foundation.IAsyncAction>, que corresponde a <xref:System.Threading.Tasks.Task>

- <xref:Windows.Foundation.IAsyncActionWithProgress%601>

- <xref:Windows.Foundation.IAsyncOperationWithProgress%602>

Para obter mais informações e um exemplo, consulte [chamar APIs assíncronas em c# ou Visual Basic](/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic).

## <a name="BKMK_NamingConvention"></a> Convenção de nomenclatura

Por convenção, deve-se acrescentar "Async" aos nomes dos métodos que têm um modificador `Async`.

É possível ignorar a convenção quando um evento, uma classe base ou um contrato de interface sugere um nome diferente. Por exemplo, você não deve renomear manipuladores de eventos comuns, como `Button1_Click`.

## <a name="BKMK_RelatedTopics"></a> Tópicos relacionados e exemplos (Visual Studio)

|Título|Descrição|Amostra|
|-----------|-----------------|------------|
|[Passo a passo: Acessando a Web usando Async e Await (Visual Basic)](../../../../visual-basic/programming-guide/concepts/async/walkthrough-accessing-the-web-by-using-async-and-await.md)|Mostra como converter uma solução síncrona do WPF em uma solução assíncrona do WPF. O aplicativo baixa uma série de sites.|[Exemplo de Async: acesso do passo a passo da Web](https://code.msdn.microsoft.com/Async-Sample-Accessing-the-9c10497f)|
|[Como: Estender o passo a passo assíncronas usando Task. WhenAll (Visual Basic)](../../../../visual-basic/programming-guide/concepts/async/how-to-extend-the-async-walkthrough-by-using-task-whenall.md)|Adiciona <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType> à explicação passo a passo anterior. O uso de `WhenAll` inicia todos os downloads ao mesmo tempo.||
|[Como: Fazer várias solicitações da Web em paralelo usando Async e Await (Visual Basic)](../../../../visual-basic/programming-guide/concepts/async/how-to-make-multiple-web-requests-in-parallel-by-using-async-and-await.md)|Demonstra como iniciar várias tarefas ao mesmo tempo.|[Exemplo de Async: fazer várias solicitações da Web em paralelo](https://code.msdn.microsoft.com/Async-Make-Multiple-Web-49adb82e)|
|[Tipos de retorno assíncronos (Visual Basic)](../../../../visual-basic/programming-guide/concepts/async/async-return-types.md)|Ilustra os tipos que os métodos assíncronos podem retornar e explica quando cada tipo é apropriado.||
|[Fluxo de controle em programas assíncronos (Visual Basic)](../../../../visual-basic/programming-guide/concepts/async/control-flow-in-async-programs.md)|Rastreia em detalhes o fluxo de controle por meio de uma sucessão de expressões de espera em um programa assíncrono.|[Exemplo de Async: fluxo de controle em programas assíncronos](https://code.msdn.microsoft.com/Async-Sample-Control-Flow-5c804fc0)|
|[Ajustando seu aplicativo assíncrono (Visual Basic)](../../../../visual-basic/programming-guide/concepts/async/fine-tuning-your-async-application.md)|Mostra como adicionar a seguinte funcionalidade à sua solução assíncrona:<br /><br /> - [Cancelar uma tarefa assíncrona ou uma lista de tarefas (Visual Basic)](../../../../visual-basic/programming-guide/concepts/async/cancel-an-async-task-or-a-list-of-tasks.md)<br />- [Cancelar tarefas assíncronas após um período (Visual Basic)](../../../../visual-basic/programming-guide/concepts/async/cancel-async-tasks-after-a-period-of-time.md)<br />- [Cancelar as demais tarefas assíncronas depois que uma delas estiver concluída (Visual Basic)](../../../../visual-basic/programming-guide/concepts/async/cancel-remaining-async-tasks-after-one-is-complete.md)<br />- [Iniciar várias tarefas assíncronas e processá-las na conclusão (Visual Basic)](../../../../visual-basic/programming-guide/concepts/async/start-multiple-async-tasks-and-process-them-as-they-complete.md)|[Exemplo de Async: ajuste do seu aplicativo](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea)|
|[Tratando a reentrada em aplicativos assíncronos (Visual Basic)](../../../../visual-basic/programming-guide/concepts/async/handling-reentrancy-in-async-apps.md)|Mostra como manipular casos em que uma operação assíncrona ativa é reiniciada enquanto está em execução.||
|[WhenAny: ponte entre o .NET Framework e o Windows Runtime](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2013/jj635140(v=vs.120))|Mostra como criar uma ponte entre tipos de tarefa no .NET Framework e IAsyncOperations no tempo de execução do Windows para que você possa usar <xref:System.Threading.Tasks.Task.WhenAny%2A> com um método de tempo de execução do Windows.|[Exemplo de Async: ponte entre o .NET e o Windows Runtime (AsTask e WhenAny)](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2013/jj635140(v=vs.120))|
|Cancelamento de Async: ponte entre o .NET Framework e o Windows Runtime|Mostra como criar uma ponte entre tipos de tarefa no .NET Framework e IAsyncOperations no tempo de execução do Windows para que você possa usar <xref:System.Threading.CancellationTokenSource> com um método de tempo de execução do Windows.|[Exemplo de Async: ponte entre o .NET e o Windows Runtime (AsTask e Cancellation)](https://code.msdn.microsoft.com/Async-Sample-Bridging-9479eca3)|
|[Usando o Async para acessar arquivos (Visual Basic)](../../../../visual-basic/programming-guide/concepts/async/using-async-for-file-access.md)|Lista e demonstra as vantagens de usar async e await para acessar arquivos.||
|[TAP (Padrão Assíncrono Baseado em Tarefa)](../../../../standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap.md)|Descreve um novo padrão de assincronia no .NET Framework. O padrão baseia-se nos tipos <xref:System.Threading.Tasks.Task> e <xref:System.Threading.Tasks.Task%601>.||
|[Vídeos sobre assincronia no Channel 9](https://channel9.msdn.com/search?term=async+&type=All)|Fornece links para uma variedade de vídeos sobre programação assíncrona.||

## <a name="BKMK_CompleteExample"></a> Exemplo completo

O código a seguir é o arquivo MainWindow.xaml.vb do aplicativo WPF (Windows Presentation Foundation) discutido neste tópico. Baixe o exemplo de [Exemplo de Async: exemplo de “Programação assíncrona com Async e Await”](https://code.msdn.microsoft.com/Async-Sample-Example-from-9b9f505c).

```vb
' Add an Imports statement and a reference for System.Net.Http
Imports System.Net.Http

Class MainWindow

    ' Mark the event handler with async so you can use Await in it.
    Private Async Sub StartButton_Click(sender As Object, e As RoutedEventArgs)

        ' Call and await separately.
        'Task<int> getLengthTask = AccessTheWebAsync();
        '' You can do independent work here.
        'int contentLength = await getLengthTask;

        Dim contentLength As Integer = Await AccessTheWebAsync()

        ResultsTextBox.Text &=
            String.Format(vbCrLf & "Length of the downloaded string: {0}." & vbCrLf, contentLength)
    End Sub

    ' Three things to note in the signature:
    '  - The method has an Async modifier.
    '  - The return type is Task or Task(Of T). (See "Return Types" section.)
    '    Here, it is Task(Of Integer) because the return statement returns an integer.
    '  - The method name ends in "Async."
    Async Function AccessTheWebAsync() As Task(Of Integer)

        ' You need to add a reference to System.Net.Http to declare client.
        Dim client As HttpClient = New HttpClient()

        ' GetStringAsync returns a Task(Of String). That means that when you await the
        ' task you'll get a string (urlContents).
        Dim getStringTask As Task(Of String) = client.GetStringAsync("https://msdn.microsoft.com")

        ' You can do work here that doesn't rely on the string from GetStringAsync.
        DoIndependentWork()

        ' The Await operator suspends AccessTheWebAsync.
        '  - AccessTheWebAsync can't continue until getStringTask is complete.
        '  - Meanwhile, control returns to the caller of AccessTheWebAsync.
        '  - Control resumes here when getStringTask is complete.
        '  - The Await operator then retrieves the string result from getStringTask.
        Dim urlContents As String = Await getStringTask

        ' The return statement specifies an integer result.
        ' Any methods that are awaiting AccessTheWebAsync retrieve the length value.
        Return urlContents.Length
    End Function

    Sub DoIndependentWork()
        ResultsTextBox.Text &= "Working . . . . . . ." & vbCrLf
    End Sub
End Class

' Sample Output:

' Working . . . . . . .

' Length of the downloaded string: 41763.
```

## <a name="see-also"></a>Consulte também

- [Operador Await](../../../../visual-basic/language-reference/operators/await-operator.md)
- [Async](../../../../visual-basic/language-reference/modifiers/async.md)
