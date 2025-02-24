---
title: Processar tarefas assíncronas conforme elas são concluídas
ms.date: 09/12/2018
ms.assetid: 25331850-35a7-43b3-ab76-3908e4346b9d
ms.openlocfilehash: 35b4e42d7da5b8bc9069083ffc47d990bcb637a8
ms.sourcegitcommit: 986f836f72ef10876878bd6217174e41464c145a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69595588"
---
# <a name="start-multiple-async-tasks-and-process-them-as-they-complete-c"></a>Iniciar várias tarefas assíncronas e processá-las na conclusão (C#)

Usando <xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType>, você pode iniciar várias tarefas ao mesmo tempo e processá-las individualmente conforme elas foram concluídas, em vez de processá-las na ordem em que foram iniciadas.

O exemplo a seguir usa uma consulta para criar uma coleção de tarefas. Cada tarefa baixa o conteúdo de um site especificado. Em cada iteração de um loop "while", uma chamada esperada para `WhenAny` retorna a tarefa na coleção de tarefas que concluir o download primeiro. Essa tarefa é removida da coleção e processada. O loop é repetido até que a coleção não contenha mais tarefas.

> [!NOTE]
> Para executar os exemplos, você precisa ter o Visual Studio (2012 ou mais recente) e o .NET Framework 4.5 ou posterior instalados no seu computador.

## <a name="download-an-example-solution"></a>Baixar um exemplo de solução

Baixe o projeto completo do WPF (Windows Presentation Foundation) em [Amostra assíncrona: Ajustando o aplicativo](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea) e, em seguida, siga estas etapas.

> [!TIP]
> Se você não quiser baixar o projeto, poderá examinar o arquivo MainWindow.xaml.cs no final deste tópico.

1. Extraia os arquivos baixados do arquivo .zip e, em seguida, inicie o Visual Studio.

2. Na barra de menus, escolha **Arquivo** > **Abrir** > **Projeto/Solução**.

3. Na caixa de diálogo **Abrir Projeto**, abra a pasta em que está o código de exemplo que você baixou e, em seguida, abra o arquivo de solução (.sln) de AsyncFineTuningCS.

4. No **Gerenciador de Soluções**, abra o menu de atalho do projeto **ProcessTasksAsTheyFinish** e escolha **Definir como Projeto de Inicialização**.

5. Escolha a tecla **F5** para executar o programa (ou pressione as teclas **Ctrl**+**F5** para executar o programa sem depurá-lo).

6. Execute o projeto várias vezes para verificar se os tamanhos baixados não aparecem sempre na mesma ordem.

## <a name="create-the-program-yourself"></a>Crie o programa sozinho

Este exemplo adiciona ao código que é desenvolvido em [Cancelar tarefas assíncronas restantes após uma ser concluída (C#)](./cancel-remaining-async-tasks-after-one-is-complete.md) e usa a mesma interface do usuário.

Para compilar o exemplo por conta própria, passo a passo, siga as instruções na seção [Baixando o exemplo](./cancel-remaining-async-tasks-after-one-is-complete.md#downloading-the-example), mas defina **CancelAfterOneTask** como o projeto de inicialização. Adicione as alterações neste tópico ao método `AccessTheWebAsync` naquele projeto. As alterações estão marcadas com asteriscos.

O projeto **CancelAfterOneTask** já inclui uma consulta que, quando executada, cria uma coleção de tarefas. Cada chamada para `ProcessURLAsync` no código a seguir retorna um <xref:System.Threading.Tasks.Task%601>, em que `TResult` é um inteiro:

```csharp
IEnumerable<Task<int>> downloadTasksQuery = from url in urlList select ProcessURL(url, client, ct);
```

No arquivo MainWindow.xaml.cs do projeto, faça as seguintes alterações ao método `AccessTheWebAsync`.

- Execute a consulta aplicando <xref:System.Linq.Enumerable.ToList%2A?displayProperty=nameWithType> em vez de <xref:System.Linq.Enumerable.ToArray%2A>.

    ```csharp
    List<Task<int>> downloadTasks = downloadTasksQuery.ToList();
    ```

- Adicione um loop `while` que executa as seguintes etapas para cada tarefa na coleção:

    1. Espera uma chamada para `WhenAny` para identificar a primeira tarefa na coleção a concluir o download.

        ```csharp
        Task<int> firstFinishedTask = await Task.WhenAny(downloadTasks);
        ```

    2. Remove a tarefa da coleção.

        ```csharp
        downloadTasks.Remove(firstFinishedTask);
        ```

    3. Espera `firstFinishedTask`, que é retornado por uma chamada para `ProcessURLAsync`. A variável `firstFinishedTask` é uma <xref:System.Threading.Tasks.Task%601> em que `TReturn` é um inteiro. A tarefa já foi concluída, mas você espera para recuperar o tamanho do site baixado, como mostra o exemplo a seguir.

        ```csharp
        int length = await firstFinishedTask;
        resultsTextBox.Text += $"\r\nLength of the download:  {length}";
        ```

Execute o programa várias vezes para verificar se os tamanhos baixados não aparecem sempre na mesma ordem.

> [!CAUTION]
> Você pode usar `WhenAny` em um loop, conforme descrito no exemplo, para resolver problemas que envolvem um número pequeno de tarefas. No entanto, outras abordagens são mais eficientes se você tiver um número grande de tarefas para processar. Para obter mais informações e exemplos, consulte [Processando tarefas quando elas são concluídas](https://blogs.msdn.microsoft.com/pfxteam/2012/08/02/processing-tasks-as-they-complete/).

## <a name="complete-example"></a>Exemplo completo

O código a seguir é o texto completo do arquivo MainWindow.xaml.cs para o exemplo. Os asteriscos marcam os elementos que foram adicionados para esse exemplo. Além disso, observe que você deve adicionar uma referência para <xref:System.Net.Http>.

Baixe o projeto em [Amostra assíncrona: Ajustando o aplicativo](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea).

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Data;
using System.Windows.Documents;
using System.Windows.Input;
using System.Windows.Media;
using System.Windows.Media.Imaging;
using System.Windows.Navigation;
using System.Windows.Shapes;

// Add a using directive and a reference for System.Net.Http.
using System.Net.Http;

// Add the following using directive.
using System.Threading;

namespace ProcessTasksAsTheyFinish
{
    public partial class MainWindow : Window
    {
        // Declare a System.Threading.CancellationTokenSource.
        CancellationTokenSource cts;

        public MainWindow()
        {
            InitializeComponent();
        }

        private async void startButton_Click(object sender, RoutedEventArgs e)
        {
            resultsTextBox.Clear();

            // Instantiate the CancellationTokenSource.
            cts = new CancellationTokenSource();

            try
            {
                await AccessTheWebAsync(cts.Token);
                resultsTextBox.Text += "\r\nDownloads complete.";
            }
            catch (OperationCanceledException)
            {
                resultsTextBox.Text += "\r\nDownloads canceled.\r\n";
            }
            catch (Exception)
            {
                resultsTextBox.Text += "\r\nDownloads failed.\r\n";
            }

            cts = null;
        }

        private void cancelButton_Click(object sender, RoutedEventArgs e)
        {
            if (cts != null)
            {
                cts.Cancel();
            }
        }

        async Task AccessTheWebAsync(CancellationToken ct)
        {
            HttpClient client = new HttpClient();

            // Make a list of web addresses.
            List<string> urlList = SetUpURLList();

            // ***Create a query that, when executed, returns a collection of tasks.
            IEnumerable<Task<int>> downloadTasksQuery =
                from url in urlList select ProcessURL(url, client, ct);

            // ***Use ToList to execute the query and start the tasks.
            List<Task<int>> downloadTasks = downloadTasksQuery.ToList();

            // ***Add a loop to process the tasks one at a time until none remain.
            while (downloadTasks.Count > 0)
            {
                    // Identify the first task that completes.
                    Task<int> firstFinishedTask = await Task.WhenAny(downloadTasks);

                    // ***Remove the selected task from the list so that you don't
                    // process it more than once.
                    downloadTasks.Remove(firstFinishedTask);

                    // Await the completed task.
                    int length = await firstFinishedTask;
                    resultsTextBox.Text += $"\r\nLength of the download:  {length}";
            }
        }

        private List<string> SetUpURLList()
        {
            List<string> urls = new List<string>
            {
                "https://msdn.microsoft.com",
                "https://msdn.microsoft.com/library/windows/apps/br211380.aspx",
                "https://msdn.microsoft.com/library/hh290136.aspx",
                "https://msdn.microsoft.com/library/dd470362.aspx",
                "https://msdn.microsoft.com/library/aa578028.aspx",
                "https://msdn.microsoft.com/library/ms404677.aspx",
                "https://msdn.microsoft.com/library/ff730837.aspx"
            };
            return urls;
        }

        async Task<int> ProcessURL(string url, HttpClient client, CancellationToken ct)
        {
            // GetAsync returns a Task<HttpResponseMessage>.
            HttpResponseMessage response = await client.GetAsync(url, ct);

            // Retrieve the website contents from the HttpResponseMessage.
            byte[] urlContents = await response.Content.ReadAsByteArrayAsync();

            return urlContents.Length;
        }
    }
}

// Sample Output:

// Length of the download:  226093
// Length of the download:  412588
// Length of the download:  175490
// Length of the download:  204890
// Length of the download:  158855
// Length of the download:  145790
// Length of the download:  44908
// Downloads complete.
```

## <a name="see-also"></a>Consulte também

- <xref:System.Threading.Tasks.Task.WhenAny%2A>
- [Ajuste fino de seu aplicativo assíncrono (C#)](./fine-tuning-your-async-application.md)
- [Programação assíncrona com async e await (C#)](./index.md)
- [Exemplo de Async: ajuste do seu aplicativo](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea)
