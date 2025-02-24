---
title: 'Como: Adicionar instaladores ao aplicativo de serviço'
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Service applications, deploying
- installation components, adding to services
- installers, adding to services
- Windows Service applications, adding installers
- services, adding installers
- ServiceInstaller class, adding installers to services
- ServiceProcessInstaller class, adding installers to services
ms.assetid: 8b698e9a-b88e-4f44-ae45-e0c5ea0ae5a8
author: ghogen
ms.openlocfilehash: 335bff660e401d8fbaf531f1c1f3ccc166d1c70a
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69952508"
---
# <a name="how-to-add-installers-to-your-service-application"></a>Como: Adicionar instaladores ao aplicativo de serviço
O Visual Studio tem componentes de instalação que podem instalar recursos associados com seus aplicativos de serviço. Os componentes de instalação registram um serviço individual no sistema em que ele está sendo instalado e informam o Gerenciador de Controle de Serviços que o serviço existe. Ao trabalhar com um aplicativo de serviço, você pode selecionar um link na janela Propriedades para adicionar automaticamente os instaladores apropriados ao projeto.  
  
> [!NOTE]
> Os valores de propriedade para o serviço são copiados da classe de serviço para a classe de instalador. Se você atualizar os valores de propriedade na classe de serviço, eles não serão atualizados automaticamente no instalador.  
  
 Quando você adiciona um instalador ao projeto, uma nova classe (que, por padrão, é chamada `ProjectInstaller`) é criada no projeto e instâncias dos componentes de instalação apropriados são criados dentro dela. Essa classe age como um ponto central para todos os componentes de instalação que o projeto precisa. Por exemplo, se você adicionar um segundo serviço ao aplicativo e clicar no link Adicionar Instalador, não será criada uma segunda classe de instalador. Nesse caso, o componente de instalação adicional necessário para o segundo serviço será adicionado à classe existente.  
  
 Você não precisa realizar nenhuma codificação especial dentro dos instaladores para fazer com que seus serviços sejam instalados corretamente. No entanto, ocasionalmente, será interessante modificar o conteúdo dos instaladores se você precisar adicionar uma funcionalidade especial ao processo de instalação.  
  
> [!NOTE]
> As caixas de diálogo e os comandos de menu que você vê podem ser diferentes dos descritos na Ajuda, dependendo da sua edição ou das configurações ativas. Para alterar as configurações, escolha **Importar e Exportar Configurações** no menu **Ferramentas**. Para obter mais informações, confira [Personalizar o IDE do Visual Studio](/visualstudio/ide/personalizing-the-visual-studio-ide).  
  
### <a name="to-add-installers-to-your-service-application"></a>Para adicionar instaladores ao seu aplicativo de serviço  
  
1. No **Gerenciador de Soluções**, acesse a exibição **Design** do serviço para o qual deseja adicionar um componente de instalação.  
  
2. Clique na tela de fundo do designer para selecionar o serviço em si, em vez de um de seus conteúdos.  
  
3. Como o foco no designer, clique com o botão direito do mouse e clique em **Adicionar Instalador**.  
  
     A nova classe `ProjectInstaller` e dois componentes de instalação, <xref:System.ServiceProcess.ServiceProcessInstaller> e <xref:System.ServiceProcess.ServiceInstaller>, são adicionados ao projeto e os valores de propriedade do serviço são copiados para os componentes.  
  
4. Clique no componente <xref:System.ServiceProcess.ServiceInstaller> e verifique se o valor da propriedade <xref:System.ServiceProcess.ServiceInstaller.ServiceName%2A> está definido com o mesmo valor que o da propriedade <xref:System.ServiceProcess.ServiceInstaller.ServiceName%2A> no serviço em si.  
  
5. Para determinar como o serviço será iniciado, clique no componente <xref:System.ServiceProcess.ServiceInstaller> e defina a propriedade <xref:System.ServiceProcess.ServiceInstaller.StartType%2A> para o valor apropriado.  
  
    |Valor|Resultado|  
    |-----------|------------|  
    |<xref:System.ServiceProcess.ServiceStartMode.Manual>|O serviço precisará ser iniciado manualmente após a instalação. Para obter mais informações, confira [Como: Iniciar serviços](../../../docs/framework/windows-services/how-to-start-services.md).|  
    |<xref:System.ServiceProcess.ServiceStartMode.Automatic>|O serviço será iniciado sozinho, sempre que o computador for reiniciado.|  
    |<xref:System.ServiceProcess.ServiceStartMode.Disabled>|Não é possível iniciar o serviço.|  
  
6. Para determinar o contexto de segurança no qual o serviço será executado, clique no componente <xref:System.ServiceProcess.ServiceProcessInstaller> e defina os valores de propriedade apropriados. Para obter mais informações, confira [Como: Especificar o contexto de segurança para serviços](../../../docs/framework/windows-services/how-to-specify-the-security-context-for-services.md).  
  
7. Substitua todos os métodos para os quais você precise realizar um processamento personalizado.  
  
8. Execute as etapas 1 a 7 para cada serviço adicional no projeto.  
  
    > [!NOTE]
    > Para cada serviço adicional no projeto, você precisará adicionar um componente <xref:System.ServiceProcess.ServiceInstaller> adicional à classe `ProjectInstaller` do projeto. O componente <xref:System.ServiceProcess.ServiceProcessInstaller> adicionado na etapa três funciona com todos os instaladores de serviço individuais no projeto.  
  
## <a name="see-also"></a>Consulte também

- [Introdução aos Aplicativos de Serviço Windows](../../../docs/framework/windows-services/introduction-to-windows-service-applications.md)
- [Como: Instalar e desinstalar serviços](../../../docs/framework/windows-services/how-to-install-and-uninstall-services.md)
- [Como: Iniciar serviços](../../../docs/framework/windows-services/how-to-start-services.md)
- [Como: Especificar o contexto de segurança para serviços](../../../docs/framework/windows-services/how-to-specify-the-security-context-for-services.md)
