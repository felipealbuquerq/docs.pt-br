---
title: Usando automação de interface do usuário para testes automatizados
ms.date: 03/30/2017
helpviewer_keywords:
- automated testing
- testing, UI Automation
- UI Automation, automated testing
ms.assetid: 3a0435c0-a791-4ad7-ba92-a4c1d1231fde
ms.openlocfilehash: d973dd59c1f0612962b89775e1fb2cf6bdd756ad
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69953925"
---
# <a name="using-ui-automation-for-automated-testing"></a>Usando automação de interface do usuário para testes automatizados
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]recentes sobre [o, consulte API de automação do Windows: Automação](https://go.microsoft.com/fwlink/?LinkID=156746)da interface do usuário.  
  
 Esta visão geral descreve [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] como o pode ser útil como uma estrutura para acesso programático em cenários de teste automatizado.  
  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]fornece um modelo de objeto unificado [!INCLUDE[TLA#tla_ui](../../../includes/tlasharptla-ui-md.md)] que permite que todas as estruturas exponham funcionalidades complexas e sofisticadas de maneira acessível e facilmente automatizada.  
  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]o foi desenvolvido como um sucessor do Microsoft Acessibilidade Ativa. Acessibilidade Ativa é uma estrutura existente projetada para fornecer uma solução para tornar os controles e os aplicativos acessíveis. A Acessibilidade Ativa não foi projetada com a automação de teste em mente, embora tenha evoluído nessa função devido aos requisitos muito semelhantes de acessibilidade e automação. [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]Além de fornecer soluções mais refinadas para acessibilidade, o também foi projetado especificamente para fornecer uma funcionalidade robusta para o teste automatizado. Por exemplo, Acessibilidade Ativa se baseia em uma única interface para expor informações sobre a interface do usuário e coletar as informações necessárias para produtos AT; [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] separa os dois modelos.  
  
 Um provedor e um cliente precisam implementar [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] para que ele seja útil como uma ferramenta de teste automatizada. Os provedores de automação da interface do usuário são aplicativos como o Microsoft Word, o Excel e outros aplicativos ou controles de [!INCLUDE[TLA#tla_win](../../../includes/tlasharptla-win-md.md)] terceiros baseados no sistema operacional. Os clientes de automação da interface do usuário incluem scripts de teste automatizados e aplicativos de tecnologia assistencial.  
  
> [!NOTE]
> A intenção desta visão geral é apresentar os novos e aprimorados recursos de teste automatizados do [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]. Esta visão geral não se destina a fornecer informações sobre recursos de acessibilidade e não abordará acessibilidade além de onde for necessário.  
  
<a name="Using_UI_Automation_During_Development"></a>   
## <a name="ui-automation-in-a-provider"></a>Automação da interface do usuário em um provedor  
 Para que [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] um seja automatizado, um desenvolvedor de um aplicativo ou controle deve examinar quais ações um usuário final pode executar no objeto usando [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] a interação padrão do teclado e do mouse.  
  
 Depois que essas ações-chave forem identificadas, [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] os padrões de controle correspondentes (ou seja, os padrões de controle que espelham a [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] funcionalidade e o comportamento do elemento) devem ser implementados no controle. Por exemplo, a interação do usuário com um controle caixa de combinação (como o diálogo Executar) geralmente envolve expandir e recolher a caixa de combinação para ocultar ou exibir uma lista de itens, selecionar um item dessa lista ou adicionar um novo valor por meio de entrada do teclado.  
  
> [!NOTE]
> Com outros modelos de acessibilidade, os desenvolvedores devem reunir informações diretamente de botões, menus ou outros controles individuais. Infelizmente, cada tipo de controle vem em dezenas de pequenas variações. Em outras palavras, embora dez variações de um botão de pressão possam funcionar da mesma forma e executar a mesma função, todas elas devem ser tratadas como controles exclusivos. Não é possível saber que esses controles são funcionalmente equivalentes. Padrões de controle foram desenvolvidos para representar esses comportamentos de controle comuns. Para obter mais informações, consulte [visão geral dos padrões de controle de automação da interface do usuário](../../../docs/framework/ui-automation/ui-automation-control-patterns-overview.md).  
  
<a name="Implementing_UI_Automation"></a>   
### <a name="implementing-ui-automation"></a>Implementando a automação da interface do usuário  
 Como mencionado anteriormente, sem o modelo unificado [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]fornecido pelo, as ferramentas de teste e os desenvolvedores precisam conhecer as informações específicas da estrutura para expor propriedades e comportamentos de controles nessa estrutura. Como pode haver várias estruturas de interface do usuário diferentes em um único momento dentro dos sistemas operacionais Windows, [!INCLUDE[TLA2#tla_win32](../../../includes/tla2sharptla-win32-md.md)]incluindo [!INCLUDE[TLA#tla_winforms](../../../includes/tlasharptla-winforms-md.md)], e Windows Presentation Foundation (WPF), pode ser uma tarefa assustadora de testar vários aplicativos controles que parecem semelhantes. Por exemplo, a tabela a seguir descreve os nomes de propriedade específicos da estrutura necessários para recuperar o nome (ou texto) associado a um controle de botão e mostra a propriedade [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] equivalente única.  
  
|Tipo de controle de automação da interface do usuário|Estrutura de interface do usuário|Propriedade específica da estrutura|Propriedade de automação da interface do usuário|  
|--------------------------------|------------------|---------------------------------|----------------------------|  
|Botão|Windows Presentation Foundation|Conteúdo|Nome da|  
|Botão|Win32|Legenda|Nome da|  
|Image|HTML|pressionando|Nome da|  
  
 Provedores de automação de interface do usuário são responsáveis por mapear as propriedades específicas da estrutura de [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] seus controles para as propriedades equivalentes.  
  
 Informações sobre como [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] implementar em um provedor podem ser encontradas em [provedores de automação de interface do usuário para código gerenciado](../../../docs/framework/ui-automation/ui-automation-providers-for-managed-code.md). Informações sobre como implementar padrões de controle estão disponíveis em [padrões de controle de automação de IU](../../../docs/framework/ui-automation/ui-automation-control-patterns.md) e padrão de texto de automação de [interface do usuário](../../../docs/framework/ui-automation/ui-automation-text-pattern.md).  
  
<a name="Testing_with_UI_Automation"></a>   
## <a name="ui-automation-in-a-client"></a>Automação da interface do usuário em um cliente  
 O objetivo de muitas ferramentas e cenários de teste automatizados é a manipulação consistente e reproduzível da interface do usuário. Isso pode envolver controles específicos de testes de unidade até a gravação e reprodução de scripts de teste que iteram por meio de uma série de ações genéricas em um grupo de controles.  
  
 Uma complicação que surge de aplicativos automatizados é a dificuldade de sincronizar um teste com um destino dinâmico. Por exemplo, um controle de caixa de listagem, como um contido no Gerenciador de tarefas do Windows, que exibe uma lista de aplicativos em execução no momento. Como os itens na caixa de listagem são dinamicamente atualizados fora do controle do aplicativo de teste, a tentativa de repetir a seleção de um item específico na caixa de listagem com qualquer consistência é impossível. Problemas semelhantes também podem surgir ao tentar repetir alterações de foco simples em um [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] que está fora do controle do aplicativo de teste.  
  
<a name="Programmatic_Access"></a>   
### <a name="programmatic-access"></a>Acesso programático  
 O acesso programático fornece a capacidade de imitar, por meio de código, qualquer interação e experiência exposta por entrada tradicional de mouse e teclado. [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]habilita o acesso programático por meio de cinco componentes:  
  
- A [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore facilita a navegação por meio da estrutura [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)]do. A árvore é criada a partir da coleção de hWnds. Para obter mais informações, consulte [visão geral da árvore de automação da interface do usuário](../../../docs/framework/ui-automation/ui-automation-tree-overview.md)  
  
- Os [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)]elementos de automação são componentes individuais no. Muitas vezes, eles podem ser mais granulares do que um hWnd. Para obter mais informações, consulte [visão geral de tipos de controle de automação da interface do usuário](../../../docs/framework/ui-automation/ui-automation-control-types-overview.md).  
  
- As propriedades de automação fornecem informações [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] específicas sobre elementos. Para obter mais informações, consulte [visão geral das propriedades de automação da interface do usuário](../../../docs/framework/ui-automation/ui-automation-properties-overview.md).  
  
- Padrões de controle definem um aspecto específico da funcionalidade de um controle; Eles podem consistir de informações de propriedade, método, evento e estrutura. Para obter mais informações, consulte [visão geral dos padrões de controle de automação da interface do usuário](../../../docs/framework/ui-automation/ui-automation-control-patterns-overview.md).  
  
- Os eventos de automação fornecem notificações e informações de eventos. Para obter mais informações, consulte [visão geral dos eventos de automação da interface do usuário](../../../docs/framework/ui-automation/ui-automation-events-overview.md).  
  
<a name="Key_properties_critical_to_test_automation"></a>   
### <a name="key-properties-for-test-automation"></a>Propriedades de chave para automação de teste  
 A capacidade de identificar e subseqüentemente localizar qualquer controle dentro do [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] fornece a base para que [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)]os aplicativos de teste automatizados operem. Há várias [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] propriedades usadas por clientes e provedores que ajudam nesse caso.  
  
#### <a name="automationid"></a>AutomationID  
 Identifica exclusivamente um elemento Automation de seus irmãos. <xref:System.Windows.Automation.AutomationElement.AutomationIdProperty>Não é localizado, ao contrário de uma propriedade <xref:System.Windows.Automation.AutomationElement.NameProperty> , como que é normalmente localizada se um produto é enviado em vários idiomas. Consulte [usar a propriedade AutomationId](../../../docs/framework/ui-automation/use-the-automationid-property.md).  
  
> [!NOTE]
> <xref:System.Windows.Automation.AutomationElement.AutomationIdProperty>não garante uma identidade exclusiva em toda a árvore de automação. Por exemplo, um aplicativo pode conter um controle de menu com vários itens de menu de nível superior que, por sua vez, têm vários itens de menu filho. Esses itens de menu secundários podem ser identificados por um esquema genérico, como "Item1, item 2, Item3, etc.", permitindo identificadores duplicados para filhos em itens de menu de nível superior.  
  
#### <a name="controltype"></a>ControlType  
 Identifica o tipo de controle representado por um elemento de automação. Informações significativas podem ser inferidas do conhecimento do tipo de controle. Consulte [visão geral de tipos de controle de automação da interface do usuário](../../../docs/framework/ui-automation/ui-automation-control-types-overview.md).  
  
#### <a name="nameproperty"></a>Nome da  
 Essa é uma cadeia de caracteres de texto que identifica ou explica um controle. <xref:System.Windows.Automation.AutomationElement.NameProperty>deve ser usado com cautela, pois ele pode ser localizado. Consulte [visão geral das propriedades de automação da interface do usuário](../../../docs/framework/ui-automation/ui-automation-properties-overview.md).  
  
<a name="Steps_Required_To_Automate_the_UI_in_a_Test_Application"></a>   
### <a name="implementing-ui-automation-in-a-test-application"></a>Implementando a automação da interface do usuário em um aplicativo de teste  
  
|||  
|-|-|  
|Adicione as referências de automação da interface do usuário.|As [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] DLLs necessárias para clientes de automação da interface do usuário estão listadas aqui.<br /><br /> -UIAutomationClient. dll fornece acesso às APIs [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] do lado do cliente.<br />-UIAutomationClientSideProvider. dll fornece a capacidade de automatizar [!INCLUDE[TLA2#tla_win32](../../../includes/tla2sharptla-win32-md.md)] controles. Consulte [suporte à automação da interface do usuário para controles padrão](../../../docs/framework/ui-automation/ui-automation-support-for-standard-controls.md).<br />-UIAutomationTypes. dll fornece acesso aos tipos específicos definidos em [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)].|  
|Adicione o <xref:System.Windows.Automation> namespace.|Esse namespace contém tudo que os clientes de automação da interface do usuário [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] precisam para usar os recursos do, exceto a manipulação de texto.|  
|Adicione o <xref:System.Windows.Automation.Text> namespace.|Esse namespace contém tudo o que um cliente de automação da interface do usuário [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] precisa para usar os recursos de manipulação de texto.|  
|Localizar controles de interesse|Scripts de teste automatizados localizam elementos de automação da interface do usuário que representam controles de interesse na árvore de automação.<br /><br /> Há várias maneiras de obter elementos de automação da interface do usuário com código.<br /><br /> -Consulte o [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] usando uma <xref:System.Windows.Automation.Condition> instrução. Normalmente, esse é o local em que <xref:System.Windows.Automation.AutomationElement.AutomationIdProperty> o idioma neutro é usado. **Observação:**  Um <xref:System.Windows.Automation.AutomationElement.AutomationIdProperty> pode ser obtido usando uma ferramenta como inspecionar. exe que é capaz de discriminar as [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] Propriedades de um controle. <br /><br /> -Use a <xref:System.Windows.Automation.TreeWalker> classe para atravessar toda [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] a árvore ou um subconjunto dela.<br />-Controle o foco.<br />-Use o hWnd do controle.<br />-Use o local da tela, como o local do cursor do mouse.<br /><br /> Consulte [obtendo elementos de automação da interface do usuário](../../../docs/framework/ui-automation/obtaining-ui-automation-elements.md)|  
|Obter padrões de controle|Padrões de controle expõem comportamentos comuns para controles funcionalmente semelhantes.<br /><br /> Depois de localizar os controles que exigem testes, os scripts de teste automatizados obtêm os padrões de controle de interesse desses elementos de automação da interface do usuário. Por exemplo, o <xref:System.Windows.Automation.InvokePattern> padrão de controle para a funcionalidade de botão <xref:System.Windows.Automation.WindowPattern> típica ou o padrão de controle para a funcionalidade de janela.<br /><br /> Consulte [visão geral dos padrões de controle de automação da interface do usuário](../../../docs/framework/ui-automation/ui-automation-control-patterns-overview.md).|  
|Automatizar a interface do usuário|Os scripts de teste automatizados agora [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] podem controlar qualquer interesse [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] de uma estrutura usando as informações [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] e a funcionalidade expostas pelos padrões de controle.|  
  
<a name="Supporting_tools"></a>   
## <a name="related-tools-and-technologies"></a>Ferramentas e tecnologias relacionadas  
 Há uma série de ferramentas e tecnologias relacionadas que dão suporte a testes automatizados com [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]o.  
  
- O inspecionar. exe é um aplicativo de GUI (interface gráfica do usuário) que pode [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ser usado para coletar informações para desenvolvimento e depuração de provedor e cliente. O inspeciona. exe está incluído no SDK do Windows.  
  
- O MSAABridge [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] expõe informações para acessibilidade ativa clientes. O objetivo principal da ponte [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] para o acessibilidade ativa é permitir que os clientes acessibilidade ativa existentes interajam com qualquer estrutura implementada. [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]  
  
<a name="Security"></a>   
## <a name="security"></a>Segurança  
 Para obter informações de segurança, consulte [visão geral de segurança de automação da interface do usuário](../../../docs/framework/ui-automation/ui-automation-security-overview.md).  
  
## <a name="see-also"></a>Consulte também

- [UI Automation Fundamentals](../../../docs/framework/ui-automation/index.md) (Fundamentos da Automação da Interface do Usuário)
