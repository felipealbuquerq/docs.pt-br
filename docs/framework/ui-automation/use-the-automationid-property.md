---
title: Usar a propriedade AutomationID
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- AutomationId property
- UI Automation, AutomationId property
- properties, AutomationId
ms.assetid: a24e807b-d7c3-4e93-ac48-80094c4e1c90
ms.openlocfilehash: 3d1e514b1ff5f71982a3c0d35cfc190ff9327b4e
ms.sourcegitcommit: 581ab03291e91983459e56e40ea8d97b5189227e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2019
ms.locfileid: "70044088"
---
# <a name="use-the-automationid-property"></a>Usar a propriedade AutomationID
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]recentes sobre [o, consulte API de automação do Windows: Automação](https://go.microsoft.com/fwlink/?LinkID=156746)da interface do usuário.  
  
 Este tópico contém cenários e código de exemplo que mostram como e quando <xref:System.Windows.Automation.AutomationElement.AutomationIdProperty> o pode ser usado para localizar um elemento dentro [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] da árvore.  
  
 <xref:System.Windows.Automation.AutomationElement.AutomationIdProperty>identifica exclusivamente um elemento de automação de interface do usuário de seus irmãos. Para obter mais informações sobre identificadores de propriedade relacionados à identificação de controle, consulte [visão geral das propriedades de automação da interface do usuário](../../../docs/framework/ui-automation/ui-automation-properties-overview.md).  
  
> [!NOTE]
> <xref:System.Windows.Automation.AutomationElement.AutomationIdProperty>não garante uma identidade exclusiva em toda a árvore; Normalmente, ele precisa de informações de contêiner e escopo para ser útil. Por exemplo, um aplicativo pode conter um controle de menu com vários itens de menu de nível superior que, por sua vez, têm vários itens de menu filho. Esses itens de menu secundários podem ser identificados por um esquema genérico, como "Item1", "item 2" e assim por diante, permitindo identificadores duplicados para filhos em itens de menu de nível superior.  
  
## <a name="scenarios"></a>Cenários  
 Foram identificados três cenários principais de aplicativo cliente de automação da interface do usuário que <xref:System.Windows.Automation.AutomationElement.AutomationIdProperty> exigem o uso de para obter resultados precisos e consistentes ao Pesquisar elementos.  
  
> [!NOTE]
> <xref:System.Windows.Automation.AutomationElement.AutomationIdProperty>é compatível com todos os elementos de automação da interface do usuário no modo de exibição de controle, exceto janelas de aplicativo [!INCLUDE[TLA#tla_winclient](../../../includes/tlasharptla-winclient-md.md)] de nível superior, elementos de automação da interface do usuário derivados de controles que não [!INCLUDE[TLA#tla_win32](../../../includes/tlasharptla-win32-md.md)] têm uma ID ou x:UID, e elementos de automação da interface do usuário derivados de controles que Não tem uma ID de controle.  
  
#### <a name="use-a-unique-and-discoverable-automationid-to-locate-a-specific-element-in-the-ui-automation-tree"></a>Usar um AutomationID exclusivo e detectável para localizar um elemento específico na árvore de automação da interface do usuário  
  
- Use uma ferramenta como [!INCLUDE[TLA#tla_uispy](../../../includes/tlasharptla-uispy-md.md)] relatar o <xref:System.Windows.Automation.AutomationElement.AutomationIdProperty> de um [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] elemento de interesse. Esse valor pode ser copiado e colado em um aplicativo cliente, como um script de teste para testes automatizados subsequentes. Essa abordagem reduz e simplifica o código necessário para identificar e localizar um elemento no tempo de execução.  
  
> [!CAUTION]
> Em geral, você deve tentar obter apenas filhos diretos do <xref:System.Windows.Automation.AutomationElement.RootElement%2A>. Uma pesquisa por descendentes pode iterar por centenas ou até milhares de elementos, possivelmente resultando em um estouro de pilha. Se você estiver tentando obter um elemento específico em um nível inferior, deverá iniciar a pesquisa na janela do aplicativo ou em um contêiner em um nível inferior.  
  
 [!code-csharp[UIAAutomationID_snip#100](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIAAutomationID_snip/CSharp/FindByAutomationID.xaml.cs#100)]
 [!code-vb[UIAAutomationID_snip#100](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIAAutomationID_snip/VisualBasic/FindByAutomationID.xaml.vb#100)]  
  
#### <a name="use-a-persistent-path-to-return-to-a-previously-identified-automationelement"></a>Usar um caminho persistente para retornar a um AutomationElement identificado anteriormente  
  
- Aplicativos cliente, de scripts de teste simples a utilitários de gravação e reprodução robustos, podem exigir acesso a elementos que não estão instanciados no momento, como uma caixa de diálogo de abertura de arquivo ou um item de menu e, portanto, não existem na árvore de automação da interface do usuário. Esses elementos só podem ser instanciados por meio de reprodução, ou "reproduzindo", uma sequência específica de ações de interface do usuário por meio do uso de propriedades de automação da interface do usuário, como AutomationID, padrões de controle e ouvintes de eventos.
  
 [!code-csharp[UIAAutomationID_snip#UIAWorkerThread](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIAAutomationID_snip/CSharp/FindByAutomationID.xaml.cs#uiaworkerthread)]
 [!code-vb[UIAAutomationID_snip#UIAWorkerThread](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIAAutomationID_snip/VisualBasic/FindByAutomationID.xaml.vb#uiaworkerthread)]  
[!code-csharp[UIAAutomationID_snip#Playback](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIAAutomationID_snip/CSharp/FindByAutomationID.xaml.cs#playback)]
[!code-vb[UIAAutomationID_snip#Playback](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIAAutomationID_snip/VisualBasic/FindByAutomationID.xaml.vb#playback)]  
  
#### <a name="use-a-relative-path-to-return-to-a-previously-identified-automationelement"></a>Usar um caminho relativo para retornar a um AutomationElement identificado anteriormente  
  
- Em determinadas circunstâncias, como o AutomationID só é garantido como exclusivo entre irmãos, vários elementos na árvore de automação da interface do usuário podem ter valores de propriedade AutomationID idênticos. Nessas situações, os elementos podem ser identificados exclusivamente com base em um pai e, se necessário, um avô. Por exemplo, um desenvolvedor pode fornecer uma barra de menus com vários itens de menu cada um com vários itens de menu filho em que os filhos são identificados com AutomationID sequenciais, como "Item1", "Item2" e assim por diante. Cada item de menu poderia, então, ser identificado exclusivamente por seu AutomationID junto com o AutomationID de seu pai e, se necessário, seu avô.  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Windows.Automation.AutomationElement.AutomationIdProperty>
- [Visão geral de árvore de automação de interface do usuário](../../../docs/framework/ui-automation/ui-automation-tree-overview.md)
- [Localizar um elemento de automação de interface do usuário com base em uma condição de propriedade](../../../docs/framework/ui-automation/find-a-ui-automation-element-based-on-a-property-condition.md)
