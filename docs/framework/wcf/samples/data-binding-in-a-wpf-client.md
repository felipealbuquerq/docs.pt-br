---
title: Associação de dados em um cliente do Windows Presentation Foundation
ms.date: 03/30/2017
ms.assetid: bb8c8293-5973-4aef-9b07-afeff5d3293c
ms.openlocfilehash: 791afee9772a6f06e57fdd09ad8a47db2bd8ca63
ms.sourcegitcommit: 581ab03291e91983459e56e40ea8d97b5189227e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2019
ms.locfileid: "70045110"
---
# <a name="data-binding-in-a-windows-presentation-foundation-client"></a>Associação de dados em um cliente do Windows Presentation Foundation
Este exemplo demonstra o uso de associação de dados em um cliente Windows Presentation Foundation (WPF). O exemplo usa um serviço de Windows Communication Foundation (WCF) que gera aleatoriamente uma matriz de álbuns para retornar ao cliente. Cada álbum tem um nome, um preço e uma lista de faixas de álbuns. As faixas do álbum têm um nome e uma duração. As informações retornadas pelo serviço são associadas automaticamente à interface do usuário (IU) fornecida pelo cliente do Windows Presentation Foundation (WPF).  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
 A vinculação de dados permite que uma fonte de dados seja associada automaticamente a uma interface do usuário. Isso simplifica o modelo de programação porque não requer que você atualize programaticamente cada elemento de interface do usuário com os dados de um objeto de dados ou de uma matriz de objetos de dados. Você pode associar um objeto a um único elemento de interface do usuário ou a uma matriz a um controle que usa várias entradas `ListBox`, como um. O código a seguir mostra como associar dados ao `DataContext` de um elemento de interface do usuário.  
  
```  
// Event handler executed when call is complete  
void client_GetAlbumListCompleted(object sender, GetAlbumListCompletedEventArgs e)  
{  
    // This is on the UI thread, myPanel can be accessed directly  
    myPanel.DataContext = e.Result;   
}  
```  
  
 No exemplo `DataContext` anterior, o para o `grid` elemento layout chamado `myPanel` `GetAlbumList` é definido como os dados retornados pelo método. O `DataContext` permite que os elementos herdem informações de seus elementos pai sobre a fonte de dados usada para associação, bem como outras características da associação, como o caminho. A linha de código deve ser executada toda vez que os dados no servidor são atualizados. Por exemplo, ele é executado quando a janela é inicializada e quando um novo álbum é adicionado.  
  
 No exemplo de código XAML a `ListBox` seguir, especifica. `ItemsSource="{Binding }"`  
  
```xml  
<ListBox   
          ItemTemplate="{StaticResource AlbumStyle}"  
          ItemsSource="{Binding }"   
          IsSynchronizedWithCurrentItem="true" />  
```  
  
 Isso especifica que os dados vinculados ao elemento de interface de usuário de nível superior também estão associados a esse controle (ou seja, a matriz de álbuns). Além disso, `ItemTemplate="{StaticResource AlbumStyle}"` especifica o modelo de dados a ser usado para cada item `ListBox`no. Você também pode definir modelos de dados para especificar como os dados devem ser formatados. Esses modelos de dados podem ser reutilizados para outros elementos da interface do usuário no aplicativo, a vantagem é que o modelo de dados é definido e mantido em um único local.  
  
 O `AlbumStyle` modelo de dados cria uma grade com dois `TextBlock`s lado a lado. Um especifica o nome do álbum e o outro número de faixas no álbum.  
  
```xaml  
<DataTemplate x:Key="AlbumStyle">  
    <Grid>  
        <Grid.ColumnDefinitions>  
            <ColumnDefinition Width="260" />  
            <ColumnDefinition Width="60" />  
        </Grid.ColumnDefinitions>  
        <TextBlock Grid.Column="0" TextContent="{Binding Path=Title}" />  
        <TextBlock Grid.Column="1" TextContent="{Binding Path=Tracks#.Count}" HorizontalAlignment="Right" />  
    </Grid>  
</DataTemplate>  
```  
  
 O código XAML a seguir cria um `ListBox`segundo.  
  
```xaml  
<ListBox Grid.Row="2"   
            Grid.ColumnSpan="2"   
            ItemTemplate="{StaticResource TrackStyle}"  
            ItemsSource="{Binding Path=Tracks}" />  
```  
  
 O código especifica um caminho para o `ItemsSource`. Isso indica que os dados associados a esse controle não são os dados de nível superior, mas uma propriedade dos dados de nível superior chamados `Tracks`. Essa propriedade representa a matriz de faixas contidas no álbum. Além disso, um nome `DataTemplate` `TrackStyle` diferente é especificado. O layout do `TrackStyle` modelo é semelhante ao `AlbumStyle` do modelo, mas os `TextBlock`s estão vinculados a propriedades diferentes. Isso ocorre porque os dois modelos são usados com objetos de dados diferentes.  
  
### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Para compilar a C# edição do ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](../../../../docs/framework/wcf/samples/building-the-samples.md).  
  
3. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](../../../../docs/framework/wcf/samples/running-the-samples.md).  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://go.microsoft.com/fwlink/?LinkId=150780) para baixar todos os Windows Communication Foundation (WCF) [!INCLUDE[wf1](../../../../includes/wf1-md.md)] e exemplos. Este exemplo está localizado no seguinte diretório.  
>   
> `<InstallDrive>:\WF_WCF_Samples\WCF\Scenario\DataBinding\WPFDataBinding`  
