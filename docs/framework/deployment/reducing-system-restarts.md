---
title: Reduzindo reinicializações do sistema durante instalações do .NET Framework 4.5
ms.date: 03/30/2017
helpviewer_keywords:
- .NET Framework, reducing system restarts
- installing .NET Framework
- installation [.NET Framework]
ms.assetid: 7aa8cb72-dee9-4716-ac54-b17b9ae8218f
author: mairaw
ms.author: mairaw
ms.openlocfilehash: a491b0efd38ed7ff37c8c704b6646dddede5efb3
ms.sourcegitcommit: 4735bb7741555bcb870d7b42964d3774f4897a6e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66379912"
---
# <a name="reducing-system-restarts-during-net-framework-45-installations"></a>Reduzindo reinicializações do sistema durante instalações do .NET Framework 4.5
O instalador do .NET Framework 4.5 usa o [Gerenciador de Reinicialização](https://go.microsoft.com/fwlink/?LinkId=231425) para impedir que o sistema seja reiniciado sempre que possível durante a instalação. Se o programa de instalação do aplicativo instala o .NET Framework, ele pode interagir com o Gerenciador de Reinicialização para aproveitar esse recurso. Para obter mais informações, confira [Como: Acompanhar o progresso do Instalador do .NET Framework 4.5](../../../docs/framework/deployment/how-to-get-progress-from-the-dotnet-installer.md).  
  
## <a name="reasons-for-a-restart"></a>Motivos para uma reinicialização  
 A instalação do .NET Framework 4.5 exigirá uma reinicialização do sistema se um aplicativo do .NET Framework 4 estiver em uso durante a instalação. Isso ocorre porque o .NET Framework 4.5 substitui arquivos do .NET Framework 4 e requer que esses arquivos estejam disponíveis durante a instalação. Em muitos casos, a reinicialização pode ser evitada detectando e fechando preventivamente aplicativos do .NET Framework 4 que estão em uso. No entanto, alguns aplicativos do sistema não devem ser fechados. Nesses casos, a reinicialização não pode ser evitada.  
  
## <a name="end-user-experience"></a>Experiência do usuário final  
 Um usuário final que está fazendo uma instalação completa do .NET Framework 4.5 terá a oportunidade de evitar uma reinicialização do sistema se o instalador detectar os aplicativos do .NET Framework 4 em uso. Uma mensagem lista todos os aplicativos do .NET Framework 4 em execução e oferece a opção de fechar esses aplicativos antes da instalação. Se o usuário confirmar, esses aplicativos serão fechados pelo instalador e uma reinicialização do sistema é evitada. Se o usuário não responder à mensagem dentro de um determinado período, a instalação continuará sem fechar os aplicativos.  
  
 Se o Gerenciador de Reinicialização detectar uma situação que exigirá uma reinicialização do sistema, mesmo que os aplicativos em execução sejam fechados, a mensagem não será exibida.  
  
 ![A caixa de diálogo Fechar Aplicativo listando os programas em execução no momento.](./media/reducing-system-restarts/close-application-dialog.png)  
  
## <a name="using-a-chained-installer"></a>Usando um instalador encadeado  
 Se você quer redistribuir o .NET Framework com seu aplicativo, mas deseja usar seu próprio programa de instalação e interface do usuário, você pode incluir (encadear) o processo de instalação do .NET Framework no seu processo de instalação. Para obter mais informações sobre instalações encadeadas, consulte [Guia de implantação para desenvolvedores](../../../docs/framework/deployment/deployment-guide-for-developers.md). Para reduzir as reinicializações do sistema em instalações encadeadas, o instalador do .NET Framework fornece ao programa de instalação a lista de aplicativos a serem fechados. O programa de instalação deve fornecer essas informações para o usuário por meio de uma interface do usuário, como uma caixa de mensagem, obter a resposta do usuário e passar a resposta de volta para o instalador do .NET Framework. Para obter um exemplo de um instalador encadeado, confira o artigo [Como: Acompanhar o progresso do Instalador do .NET Framework 4.5](../../../docs/framework/deployment/how-to-get-progress-from-the-dotnet-installer.md).  
  
 Se você estiver usando um instalador encadeado, mas não quiser fornecer sua própria caixa de mensagem para fechar os aplicativos, você poderá usar as opções `/showrmui` e `/passive` na linha de comando quando encadear o processo de instalação do .NET Framework. Quando você usa essas opções juntas, o instalador mostra a caixa de mensagem para fechar os aplicativos se eles puderem ser fechados para evitar uma reinicialização do sistema. Essa caixa de mensagem se comporta no modo passivo da mesma forma que na interface do usuário completa. Consulte [Guia de implantação para desenvolvedores](../../../docs/framework/deployment/deployment-guide-for-developers.md) para obter o conjunto completo de opções de linha de comando para o .NET Framework redistribuível.  
  
## <a name="see-also"></a>Consulte também

- [Implantação](../../../docs/framework/deployment/index.md)
- [Guia de implantação para desenvolvedores](../../../docs/framework/deployment/deployment-guide-for-developers.md)
- [Como: Acompanhar o progresso do Instalador do .NET Framework 4.5](../../../docs/framework/deployment/how-to-get-progress-from-the-dotnet-installer.md)
