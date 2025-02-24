---
title: 'Passo a passo: armazenar dados de aplicativo em cache em um aplicativo WPF'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- walkthroughs [WPF], caching
- caching [.NET Framework]
- caching [WPF]
ms.assetid: dac2c9ce-042b-4d23-91eb-28f584415cef
ms.openlocfilehash: 2609a54ce8ba2076c35567fe5bc1d9961f6fef3f
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69942055"
---
# <a name="walkthrough-caching-application-data-in-a-wpf-application"></a>Passo a passo: armazenar dados de aplicativo em cache em um aplicativo WPF
O cache permite que você armazene dados na memória para acesso rápido. Quando os dados são acessados novamente, os aplicativos podem obter os dados do cache, em vez de recuperá-los da fonte original. Isso pode melhorar o desempenho e a escalabilidade. Além disso, o cache torna os dados disponíveis quando a fonte de dados está temporariamente indisponível.

 O .NET Framework fornece classes que permitem que você use o Caching em aplicativos .NET Framework. Essas classes estão localizadas no <xref:System.Runtime.Caching> namespace.

> [!NOTE]
> O <xref:System.Runtime.Caching> namespace é novo no .NET Framework 4. Esse namespace torna o Caching disponível para todos os aplicativos .NET Framework. Nas versões anteriores do .NET Framework, o Caching estava disponível apenas no <xref:System.Web> namespace e, portanto, exigia uma dependência em classes ASP.net.

 Este tutorial mostra como usar a funcionalidade de cache que está disponível no .NET Framework como parte de um [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] aplicativo. No passo a passo, você armazenará em cache o conteúdo de um arquivo de texto.

 As tarefas ilustradas nesta explicação passo a passo incluem o seguinte:

- Criação de um projeto de aplicativo do WPF.

- Adicionando uma referência ao .NET Framework 4.

- Inicialização de um cache.

- Adição de uma entrada de cache que contém o conteúdo de um arquivo de texto.

- Fornecimento de uma política de remoção para a entrada de cache.

- Monitoramento do caminho do arquivo armazenado em cache e notificação da instância de cache a respeito de alterações ao item monitorado.

## <a name="prerequisites"></a>Pré-requisitos
 Para concluir este passo a passo, você precisará de:

- Microsoft Visual Studio 2010.

- Um arquivo de texto que contenha uma pequena quantidade de texto. (Você exibirá o conteúdo do arquivo de texto em uma caixa de mensagem). O código ilustrado no passo a passo presume que você esteja trabalhando com o seguinte arquivo:

     `c:\cache\cacheText.txt`

     No entanto, você pode usar qualquer arquivo de texto e fazer pequenas alterações no código neste passo a passo.

## <a name="creating-a-wpf-application-project"></a>Criação de um projeto de aplicativo do WPF
 Você começará criando um projeto de aplicativo do WPF.

#### <a name="to-create-a-wpf-application"></a>Para criar um aplicativo do WPF

1. Inicie o Visual Studio.

2. No menu **Arquivo**, clique em **Novo** e, em seguida, clique em **Novo Projeto**.

     A caixa de diálogo **Novo Projeto** é exibida.

3. Em **Modelos Instalados**, selecione a linguagem de programação que você deseja usar (**Visual Basic** ou **Visual C#** ).

4. Na caixa de diálogo **Novo Projeto**, selecione **Aplicativo WPF**.

    > [!NOTE]
    > Se você não vir o modelo de **aplicativo do WPF** , certifique-se de que você está visando uma versão do .NET Framework que dá suporte ao WPF. Na caixa de diálogo **novo projeto** , selecione .NET Framework 4 na lista.

5. Na caixa de texto **Nome**, insira um nome para o projeto. Por exemplo, você pode inserir **WPFCaching**.

6. Marque a caixa de seleção **Criar diretório para a solução**.

7. Clique em **OK**.

     O WPF Designer é aberto modo de exibição de **Design** e exibe o arquivo MainWindow.xaml. O Visual Studio cria a pasta **meu projeto** , o arquivo Application. XAML e o arquivo MainWindow. XAML.

## <a name="targeting-the-net-framework-and-adding-a-reference-to-the-caching-assemblies"></a>Direcionamento ao .NET Framework e adição de uma referência aos assemblies de cache
 Por padrão, os aplicativos do WPF se destinam ao [!INCLUDE[net_client_v40_long](../../../../includes/net-client-v40-long-md.md)]. Para usar o <xref:System.Runtime.Caching> namespace em um aplicativo do WPF, o aplicativo deve ter como destino o .NET Framework 4 [!INCLUDE[net_client_v40_long](../../../../includes/net-client-v40-long-md.md)](não o) e deve incluir uma referência ao namespace.

 Portanto, a próxima etapa é alterar o destino de .NET Framework e adicionar uma referência ao <xref:System.Runtime.Caching> namespace.

> [!NOTE]
> O procedimento para alterar o destino do .NET Framework é diferente em um projeto do Visual Basic e em um projeto Visual C#.

#### <a name="to-change-the-target-net-framework-in-visual-basic"></a>Para alterar o .NET Framework de destino no Visual Basic

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no nome do projeto e, em seguida, clique em **Propriedades**.

     A janela Propriedades para o aplicativo é exibida.

2. Clique na guia **Compilar**.

3. Na parte inferior da janela, clique em **Opções Avançadas de Compilação...** .

     A caixa de diálogo **Configurações Avançadas do Compilador** é exibida.

4. Na lista **estrutura de destino (todas as configurações)** , selecione .NET Framework 4. (Não selecione [!INCLUDE[net_client_v40_long](../../../../includes/net-client-v40-long-md.md)]).

5. Clique em **OK**.

     A caixa de diálogo **Alteração da Estrutura de Destino** é exibida.

6. Na caixa de diálogo **Alteração da Estrutura de Destino** clique em **Sim**.

     O projeto é fechado e, em seguida, é reaberto.

7. Adicione uma referência ao assembly do cache, seguindo estas etapas:

    1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no nome do projeto e, em seguida, clique em **Adicionar Referência**.

    2. Selecione a guia **.NET**, selecione `System.Runtime.Caching` e, em seguida, clique em **OK**.

#### <a name="to-change-the-target-net-framework-in-a-visual-c-project"></a>Para alterar o .NET Framework de destino em um projeto do Visual C#

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no nome do projeto e, em seguida, clique em **Propriedades**.

     A janela Propriedades para o aplicativo é exibida.

2. Clique na guia **Aplicativo**.

3. Na lista **estrutura de destino** , selecione .NET Framework 4. (Não selecione **.NET Framework 4 Client Profile**).

4. Adicione uma referência ao assembly do cache, seguindo estas etapas:

    1. Clique com o botão direito do mouse na pasta **Referencias** e, em seguida, clique em **Adicionar Referência**.

    2. Selecione a guia **.NET**, selecione `System.Runtime.Caching` e, em seguida, clique em **OK**.

## <a name="adding-a-button-to-the-wpf-window"></a>Adicionar um botão à janela do WPF
 Em seguida, você adicionará um controle de botão e criará um manipulador de eventos para o evento `Click` do botão. Depois, você adicionará código para que quando o botão for clicado, o conteúdo do arquivo de texto será armazenado em cache e exibido.

#### <a name="to-add-a-button-control"></a>Para adicionar um controle de botão

1. No **Gerenciador de Soluções**, clique duas vezes no arquivo MainWindow.xaml para abri-lo.

2. Na **Caixa de ferramentas**, em **Controles Comuns do WPF**, arraste um controle `Button` para a janela `MainWindow`.

3. Na janela **Propriedades**, defina a propriedade `Content` do controle `Button` como **Obter Cache**.

## <a name="initializing-the-cache-and-caching-an-entry"></a>Inicializar o cache e armazenar uma entrada em cache
 Em seguida, você adicionará o código para realizar as seguintes tarefas:

- Crie uma instância da classe de cache, ou seja, você criará uma instância <xref:System.Runtime.Caching.MemoryCache> de um novo objeto.

- Especifique que o cache usa um <xref:System.Runtime.Caching.HostFileChangeMonitor> objeto para monitorar as alterações no arquivo de texto.

- Ler o arquivo de texto e armazenar seu conteúdo em cache como uma entrada de cache.

- Exibir o conteúdo do arquivo de texto armazenado em cache.

#### <a name="to-create-the-cache-object"></a>Para criar o objeto do cache

1. Clique duas vezes no botão que você acabou de adicionar para criar um manipulador de eventos no arquivo MainWindow.xaml.cs ou MainWindow.Xaml.vb.

2. Na parte superior do arquivo (antes da declaração de classe), adicione as seguintes instruções `Imports` (Visual Basic) ou `using` (C#):

    ```csharp
    using System.Runtime.Caching;
    using System.IO;
    ```

    ```vb
    Imports System.Runtime.Caching
    Imports System.IO
    ```

3. No manipulador de eventos, adicione o seguinte código para instanciar o objeto de cache:

    ```csharp
    ObjectCache cache = MemoryCache.Default;
    ```

    ```vb
    Dim cache As ObjectCache = MemoryCache.Default
    ```

     A <xref:System.Runtime.Caching.ObjectCache> classe é uma classe interna que fornece um cache de objeto na memória.

4. Adicione o seguinte código para ler o conteúdo de uma entrada de cache chamada `filecontents`:

    ```vb
    Dim fileContents As String = TryCast(cache("filecontents"), String)
    ```

    ```csharp
    string fileContents = cache["filecontents"] as string;
    ```

5. Adicione o seguinte código para verificar se a entrada de cache chamada `filecontents` existe:

    ```vb
    If fileContents Is Nothing Then

    End If
    ```

    ```csharp
    if (fileContents == null)
    {

    }
    ```

     Se a entrada de cache especificada não existe, leia o arquivo de texto e adicione-o como uma entrada de cache no cache.

6. No bloco, adicione o código a seguir para criar um novo <xref:System.Runtime.Caching.CacheItemPolicy> objeto que especifica que a entrada de cache expira após 10 segundos. `if/then`

    ```vb
    Dim policy As New CacheItemPolicy()
    policy.AbsoluteExpiration = DateTimeOffset.Now.AddSeconds(10.0)
    ```

    ```csharp
    CacheItemPolicy policy = new CacheItemPolicy();
    policy.AbsoluteExpiration = DateTimeOffset.Now.AddSeconds(10.0);
    ```

     Se nenhuma informação de remoção ou de expiração for fornecida, o <xref:System.Runtime.Caching.ObjectCache.InfiniteAbsoluteExpiration>padrão será, o que significa que as entradas de cache nunca expiram com base apenas em um tempo absoluto. Em vez disso, as entradas de cache expiram somente quando há pressão de memória. Como prática recomendada, você sempre deve fornecer explicitamente uma expiração absoluta ou deslizante.

7. Dentro do bloco `if/then` e depois do código adicionado na etapa anterior, adicione o seguinte código para criar uma coleção para os caminhos de arquivo que você deseja monitorar e para adicionar o caminho do arquivo de texto à coleção:

    ```vb
    Dim filePaths As New List(Of String)()
    filePaths.Add("c:\cache\cacheText.txt")
    ```

    ```csharp
    List<string> filePaths = new List<string>();
    filePaths.Add("c:\\cache\\cacheText.txt");
    ```

    > [!NOTE]
    > Se o arquivo de texto que você deseja usar não for `c:\cache\cacheText.txt`, especifique o caminho em que se encontra o arquivo de texto que você deseja usar.

8. Após o código que você adicionou na etapa anterior, adicione o seguinte código para adicionar um novo <xref:System.Runtime.Caching.HostFileChangeMonitor> objeto à coleção de monitores de alterações para a entrada de cache:

    ```vb
    policy.ChangeMonitors.Add(New HostFileChangeMonitor(filePaths))
    ```

    ```csharp
    policy.ChangeMonitors.Add(new HostFileChangeMonitor(filePaths));
    ```

     O <xref:System.Runtime.Caching.HostFileChangeMonitor> objeto monitora o caminho do arquivo de texto e notifica o cache se ocorrerem alterações. Neste exemplo, a entrada de cache expirará se o conteúdo do arquivo for alterado.

9. Após o código que você adicionou na etapa anterior, adicione o seguinte código para ler o conteúdo do arquivo de texto:

    ```vb
    fileContents = File.ReadAllText("c:\cache\cacheText.txt") & vbCrLf & DateTime.Now.ToString()
    ```

    ```csharp
    fileContents = File.ReadAllText("c:\\cache\\cacheText.txt") + "\n" + DateTime.Now;
    ```

     O carimbo de data/hora é adicionado para que você possa ver quando a entrada de cache expira.

10. Após o código que você adicionou na etapa anterior, adicione o seguinte código para inserir o conteúdo do arquivo no objeto de cache como uma <xref:System.Runtime.Caching.CacheItem> instância:

    ```vb
    cache.Set("filecontents", fileContents, policy)
    ```

    ```csharp
    cache.Set("filecontents", fileContents, policy);
    ```

     Você especifica informações sobre como a entrada de cache deve ser removida passando o <xref:System.Runtime.Caching.CacheItemPolicy> objeto que você criou anteriormente como um parâmetro.

11. Depois do bloco `if/then`, adicione o seguinte código para exibir o conteúdo do arquivo armazenado em cache em uma caixa de mensagem:

    ```vb
    MessageBox.Show(fileContents)
    ```

    ```csharp
    MessageBox.Show(fileContents);
    ```

12. No menu **Compilar**, clique em **Compilar WPFCaching** para compilar seu projeto.

## <a name="testing-caching-in-the-wpf-application"></a>Testando o cache no Aplicativo WPF
 Agora você pode testar o aplicativo.

#### <a name="to-test-caching-in-the-wpf-application"></a>Para testar o cache no aplicativo WPF

1. Pressione CTRL+F5 para executar o aplicativo.

     A janela `MainWindow` é exibida.

2. Clique em **Obter Cache**.

     O conteúdo em cache do arquivo de texto é exibido em uma caixa de mensagem. Observe o carimbo de data/hora no arquivo.

3. Feche a caixa de mensagem e, em seguida, clique em **Obter Cache** novamente.

     O carimbo de data/hora está inalterado. Isso indica que o conteúdo em cache está exibido.

4. Aguarde 10 segundos ou mais e, em seguida, clique em **Obter Cache** novamente.

     Agora, um novo carimbo de data/hora é exibido. Isso indica que a política deixou a entrada de cache expirar e que o novo conteúdo armazenado em cache fosse exibido.

5. Em um editor de texto, abra o arquivo de texto que você criou. Não faça nenhuma alteração ainda.

6. Feche a caixa de mensagem e, em seguida, clique em **Obter Cache** novamente.

     Observe o carimbo de data/hora novamente.

7. Faça uma alteração no arquivo de texto e, em seguida, salve o arquivo.

8. Feche a caixa de mensagem e, em seguida, clique em **Obter Cache** novamente.

     Essa caixa de mensagem contém o conteúdo atualizado do arquivo de texto e um novo carimbo de data/hora. Isso indica que o monitor de alteração do arquivo de host removeu a entrada de cache imediatamente após você alterar o arquivo, mesmo que o tempo limite de expiração absoluto não tenha se expirado.

    > [!NOTE]
    > Você pode aumentar o tempo de remoção para 20 segundos ou mais para permitir mais tempo para fazer uma alteração no arquivo.

## <a name="code-example"></a>Exemplo de código
 Depois de concluir este passo a passo, o código para o projeto que você criou será parecido com o exemplo a seguir.

 [!code-csharp[CachingWPFApplications#1](~/samples/snippets/csharp/VS_Snippets_Wpf/CachingWPFApplications/CSharp/MainWindow.xaml.cs#1)]
 [!code-vb[CachingWPFApplications#1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CachingWPFApplications/VisualBasic/MainWindow.xaml.vb#1)]

## <a name="see-also"></a>Consulte também

- <xref:System.Runtime.Caching.MemoryCache>
- <xref:System.Runtime.Caching.ObjectCache>
- <xref:System.Runtime.Caching>
- [Cache em aplicativos do .NET Framework](../../performance/caching-in-net-framework-applications.md)
