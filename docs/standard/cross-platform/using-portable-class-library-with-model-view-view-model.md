---
title: Usando a Biblioteca de Classes Portátil com Modelo MVVM
ms.date: 07/18/2018
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Portable Class Library [.NET Framework], and MVVM
- MVVM, and Portable Class Library
ms.assetid: 41a0b9f8-15a2-431a-bc35-e310b2953b03
author: mairaw
ms.author: mairaw
ms.openlocfilehash: b53be90764c6537fb27cb1b5ed781a68e69effa0
ms.sourcegitcommit: b1cfd260928d464d91e20121f9bdba7611c94d71
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67504668"
---
# <a name="using-portable-class-library-with-model-view-view-model"></a>Usando a Biblioteca de Classes Portátil com Modelo MVVM
Você pode usar o .NET Framework [biblioteca de classes portátil](../../../docs/standard/cross-platform/cross-platform-development-with-the-portable-class-library.md) para implementar o padrão Model-View-View modelo (MVVM) e compartilhar assemblies entre várias plataformas.

[!INCLUDE[standard](../../../includes/pcl-to-standard.md)]

 MVVM é um padrão de aplicativo que isola a interface do usuário da lógica de negócios subjacentes. Você pode implementar as classes de modelo e exibição do modelo em um projeto de biblioteca de classes portátil no Visual Studio 2012 e, em seguida, criar exibições personalizadas para diferentes plataformas. Essa abordagem permite que você grave os dados de modelo e a lógica de negócios apenas uma vez e usá-la de código do .NET Framework, Silverlight, Windows Phone, e [!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)] aplicativos, conforme mostrado na ilustração a seguir.

 ![Mostra a biblioteca de classes portátil com assemblies de compartilhamento do MVVM entre plataformas.](./media/using-portable-class-library-with-model-view-view-model/mvvm-share-assemblies-across-platforms.png)

 Este tópico fornece informações gerais sobre o padrão MVVM. Ele só fornece informações sobre como usar a biblioteca de classes portátil para implementar o MVVM. Para obter mais informações sobre o MVVM, consulte o [MVVM início rápido usando o Prism Library 5.0 para WPF](https://docs.microsoft.com/previous-versions/msp-n-p/gg430857(v=pandp.40)).

## <a name="classes-that-support-mvvm"></a>Classes que dão suporte ao MVVM
 Quando você tem como alvo o .NET Framework 4.5, [!INCLUDE[net_win8_profile](../../../includes/net-win8-profile-md.md)], Silverlight ou Windows Phone 7.5 para o seu projeto de biblioteca de classes portátil, as classes a seguir estão disponíveis para implementar o padrão MVVM:

- Classe <xref:System.Collections.ObjectModel.ObservableCollection%601?displayProperty=nameWithType>

- Classe <xref:System.Collections.ObjectModel.ReadOnlyObservableCollection%601?displayProperty=nameWithType>

- Classe <xref:System.Collections.Specialized.INotifyCollectionChanged?displayProperty=nameWithType>

- Classe <xref:System.Collections.Specialized.NotifyCollectionChangedAction?displayProperty=nameWithType>

- Classe <xref:System.Collections.Specialized.NotifyCollectionChangedEventArgs?displayProperty=nameWithType>

- Classe <xref:System.Collections.Specialized.NotifyCollectionChangedEventHandler?displayProperty=nameWithType>

- Classe <xref:System.ComponentModel.DataErrorsChangedEventArgs?displayProperty=nameWithType>

- Classe <xref:System.ComponentModel.INotifyDataErrorInfo?displayProperty=nameWithType>

- Classe <xref:System.ComponentModel.INotifyPropertyChanged?displayProperty=nameWithType>

- Classe <xref:System.Windows.Input.ICommand?displayProperty=nameWithType>

- Todas as classes no <xref:System.ComponentModel.DataAnnotations?displayProperty=nameWithType> namespace

## <a name="implementing-mvvm"></a>Implementação do MVVM
 Para implementar o MVVM, você normalmente cria o modelo e o modelo de exibição em um projeto de biblioteca de classes portátil, porque um projeto de biblioteca de classes portátil não pode fazer referência a um projeto não portátil. O modelo e o modelo de exibição podem ser no mesmo projeto ou em projetos separados. Se você usar projetos separados, adicione uma referência do projeto de modelo de exibição para o projeto de modelo.

 Depois de compilar o modelo e exibir projetos de modelo, você pode referenciar os assemblies no aplicativo que contém a exibição. Se o modo de exibição interage apenas com o modelo de exibição, você só precisará fazer referência ao assembly que contém o modelo de exibição.

### <a name="model"></a>Modelo
 O exemplo a seguir mostra uma classe de modelo simplificado que pode residir em um projeto de biblioteca de classes portátil.

 [!code-csharp[PortableClassLibraryMVVM#1](../../../samples/snippets/csharp/VS_Snippets_CLR/portableclasslibrarymvvm/cs/customer.cs#1)]
 [!code-vb[PortableClassLibraryMVVM#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/portableclasslibrarymvvm/vb/customer.vb#1)]

 O exemplo a seguir mostra uma maneira simples de popular, recuperar e atualizar os dados em um projeto de biblioteca de classes portátil. Em um aplicativo real, você recuperaria os dados de uma fonte, como um serviço do Windows Communication Foundation (WCF).

 [!code-csharp[PortableClassLibraryMVVM#2](../../../samples/snippets/csharp/VS_Snippets_CLR/portableclasslibrarymvvm/cs/customerrepository.cs#2)]
 [!code-vb[PortableClassLibraryMVVM#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/portableclasslibrarymvvm/vb/customerrepository.vb#2)]

### <a name="view-model"></a>Modelo de exibição
 Uma classe base para modelos de exibição com frequência é adicionada ao implementar o padrão MVVM. O exemplo a seguir mostra uma classe base.

 [!code-csharp[PortableClassLibraryMVVM#3](../../../samples/snippets/csharp/VS_Snippets_CLR/portableclasslibrarymvvm/cs/viewmodelbase.cs#3)]
 [!code-vb[PortableClassLibraryMVVM#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/portableclasslibrarymvvm/vb/viewmodelbase.vb#3)]

 Uma implementação do <xref:System.Windows.Input.ICommand> interface é frequentemente usada com o padrão MVVM. O exemplo a seguir mostra uma implementação da interface <xref:System.Windows.Input.ICommand>.

 [!code-csharp[PortableClassLibraryMVVM#4](../../../samples/snippets/csharp/VS_Snippets_CLR/portableclasslibrarymvvm/cs/relaycommand.cs#4)]
 [!code-vb[PortableClassLibraryMVVM#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/portableclasslibrarymvvm/vb/relaycommand.vb#4)]

 O exemplo a seguir mostra um modelo de exibição simplificada.

 [!code-csharp[PortableClassLibraryMVVM#5](../../../samples/snippets/csharp/VS_Snippets_CLR/portableclasslibrarymvvm/cs/mainpageviewmodel.cs#5)]
 [!code-vb[PortableClassLibraryMVVM#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/portableclasslibrarymvvm/vb/customerviewmodel.vb#5)]  
  
### <a name="view"></a>Exibir  
 De um aplicativo do .NET Framework 4.5, [!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)] aplicativo, o aplicativo com base no Silverlight ou o aplicativo de Windows Phone 7.5, você pode referenciar o assembly que contém os projetos de modelo do modelo e exibição.  Você, em seguida, crie uma exibição que interage com o modelo de exibição. O exemplo a seguir mostra um aplicativo Windows Presentation Foundation (WPF) simplificada que recupera e atualiza os dados do modelo de exibição. Você pode criar modos de exibição semelhantes no Silverlight, Windows Phone, ou [!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)] aplicativos.  
  
 [!code-xaml[PortableClassLibraryMVVM#6](../../../samples/snippets/csharp/VS_Snippets_CLR/portableclasslibrarymvvm/cs/mainwindow.xaml#6)]  
  
## <a name="see-also"></a>Consulte também

- [Biblioteca de classes portátil](../../../docs/standard/cross-platform/cross-platform-development-with-the-portable-class-library.md)
