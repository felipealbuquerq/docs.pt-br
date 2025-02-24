---
title: Implementando o padrão de controle Scroll de automação de interface do usuário
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, Scroll control pattern
- control patterns, Scroll
- Scroll control pattern
ms.assetid: 73d64242-6cbb-424c-92dd-dc69530b7899
ms.openlocfilehash: 22bb78040b023a59fd46f0a2be45659d6d7220b8
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69914518"
---
# <a name="implementing-the-ui-automation-scroll-control-pattern"></a>Implementando o padrão de controle Scroll de automação de interface do usuário
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]recentes sobre [o, consulte API de automação do Windows: Automação](https://go.microsoft.com/fwlink/?LinkID=156746)da interface do usuário.  
  
 Este tópico apresenta diretrizes e convenções para implementação <xref:System.Windows.Automation.Provider.IScrollProvider>, incluindo informações sobre eventos e propriedades. Links para referências adicionais são listados no final do tópico.  
  
 O <xref:System.Windows.Automation.ScrollPattern> padrão de controle é usado para dar suporte a um controle que atua como um contêiner rolável para uma coleção de objetos filho. O controle não é necessário para usar barras de rolagem para dar suporte à funcionalidade de rolagem, embora normalmente faça isso.  
  
 ![Controle de rolagem sem barras de rolagem.](../../../docs/framework/ui-automation/media/uia-scrollpattern-without-scrollbars.PNG "UIA_ScrollPattern_Without_Scrollbars")  
Exemplo de um controle de rolagem que não usa barras de rolagem  
  
 Para obter exemplos de controles que implementam esse controle, consulte [mapeamento de padrão de controle para clientes de automação da interface do usuário](../../../docs/framework/ui-automation/control-pattern-mapping-for-ui-automation-clients.md).  
  
<a name="Implementation_Guidelines_and_Conventions"></a>   
## <a name="implementation-guidelines-and-conventions"></a>Diretrizes e convenções de implementação  
 Ao implementar o padrão de controle de rolagem, observe as seguintes diretrizes e convenções:  
  
- O filho desse controle deve implementar <xref:System.Windows.Automation.Provider.IScrollItemProvider>.  
  
- As barras de rolagem de um controle de contêiner não <xref:System.Windows.Automation.ScrollPattern> dão suporte ao padrão de controle. Em vez disso, <xref:System.Windows.Automation.RangeValuePattern> eles devem oferecer suporte ao padrão de controle.  
  
- Quando a rolagem é medida em percentuais, todos os valores ou quantidades relacionados à formatura de rolagem devem ser normalizados para um intervalo de 0 a 100.  
  
- <xref:System.Windows.Automation.ScrollPatternIdentifiers.HorizontallyScrollableProperty>e <xref:System.Windows.Automation.ScrollPatternIdentifiers.VerticallyScrollableProperty> são independentes <xref:System.Windows.Automation.AutomationElement.IsEnabledProperty>do.  
  
- Se <xref:System.Windows.Automation.ScrollPatternIdentifiers.HorizontallyScrollableProperty> <xref:System.Windows.Automation.ScrollPatternIdentifiers.NoScroll> <xref:System.Windows.Automation.ScrollPatternIdentifiers.HorizontalScrollPercentProperty> , em seguida ,<xref:System.Windows.Automation.ScrollPatternIdentifiers.HorizontalViewSizeProperty> deve ser definido como 100% e deve ser definido como.  =  `false` Da mesma forma <xref:System.Windows.Automation.ScrollPatternIdentifiers.VerticallyScrollableProperty> , se  =  `false` <xref:System.Windows.Automation.ScrollPatternIdentifiers.VerticalScrollPercentProperty> deve ser definido como 100 por cento e deve ser definido como <xref:System.Windows.Automation.ScrollPatternIdentifiers.NoScroll>. <xref:System.Windows.Automation.ScrollPatternIdentifiers.VerticalViewSizeProperty> Isso permite que um cliente de automação da interface do usuário use esses <xref:System.Windows.Automation.ScrollPattern.SetScrollPercent%2A> valores de propriedade dentro do método, evitando uma [condição de corrida](https://support.microsoft.com/default.aspx?scid=kb;en-us;317723) se uma direção na qual o cliente não está interessado em rolar for ativada.  
  
- <xref:System.Windows.Automation.Provider.IScrollProvider.HorizontalScrollPercent%2A>é específico da localidade. A definição de HorizontalScrollPercent = 100,0 deve definir o local de rolagem do controle como o equivalente de sua posição mais à direita para idiomas como o inglês que são lidos da esquerda para a direita. Como alternativa, para idiomas como o árabe que são lidos da direita para a esquerda, a definição de HorizontalScrollPercent = 100,0 deve definir o local de rolagem para a posição mais à esquerda.  
  
<a name="Required_Members_for_IScrollProvider"></a>   
## <a name="required-members-for-iscrollprovider"></a>Membros necessários para IScrollProvider  
 As propriedades e os métodos a seguir são necessários <xref:System.Windows.Automation.Provider.IScrollProvider>para implementar o.  
  
|Membro necessário|Tipo de membro|Observações|  
|---------------------|-----------------|-----------|  
|<xref:System.Windows.Automation.Provider.IScrollProvider.HorizontalScrollPercent%2A>|Propriedade|Nenhum|  
|<xref:System.Windows.Automation.Provider.IScrollProvider.VerticalScrollPercent%2A>|Propriedade|Nenhum|  
|<xref:System.Windows.Automation.Provider.IScrollProvider.HorizontalViewSize%2A>|Propriedade|Nenhum|  
|<xref:System.Windows.Automation.Provider.IScrollProvider.VerticalViewSize%2A>|Propriedade|Nenhum|  
|<xref:System.Windows.Automation.Provider.IScrollProvider.HorizontallyScrollable%2A>|Propriedade|Nenhum|  
|<xref:System.Windows.Automation.Provider.IScrollProvider.VerticallyScrollable%2A>|Propriedade|Nenhum|  
|<xref:System.Windows.Automation.Provider.IScrollProvider.Scroll%2A>|Método|Nenhum|  
|<xref:System.Windows.Automation.Provider.IScrollProvider.SetScrollPercent%2A>|Método|Nenhum|  
  
 Este padrão de controle não tem eventos associados.  
  
<a name="Exceptions"></a>   
## <a name="exceptions"></a>Exceções  
 Os provedores devem lançar as seguintes exceções.  
  
|Tipo de exceção|Condição|  
|--------------------|---------------|  
|<xref:System.ArgumentException>|<xref:System.Windows.Automation.Provider.IScrollProvider.Scroll%2A>gera essa exceção se um controle der <xref:System.Windows.Automation.ScrollAmount.SmallIncrement> suporte a valores exclusivamente para rolagem horizontal ou vertical, <xref:System.Windows.Automation.ScrollAmount.LargeIncrement> mas um valor é passado.|  
|<xref:System.ArgumentException>|<xref:System.Windows.Automation.Provider.IScrollProvider.SetScrollPercent%2A>gera essa exceção quando um valor que não pode ser convertido em um Double é passado.|  
|<xref:System.ArgumentOutOfRangeException>|<xref:System.Windows.Automation.Provider.IScrollProvider.SetScrollPercent%2A>gera essa exceção quando um valor maior que 100 ou menor que 0 é passado (exceto-1 que é equivalente a <xref:System.Windows.Automation.ScrollPatternIdentifiers.NoScroll>).|  
|<xref:System.InvalidOperationException>|<xref:System.Windows.Automation.Provider.IScrollProvider.Scroll%2A> E<xref:System.Windows.Automation.Provider.IScrollProvider.SetScrollPercent%2A> gerar essa exceção quando for feita uma tentativa de rolar em uma direção sem suporte.|  
  
## <a name="see-also"></a>Consulte também

- [Visão geral de padrões de controle de automação da interface do usuário](../../../docs/framework/ui-automation/ui-automation-control-patterns-overview.md)
- [Suporte a padrões de controle em um provedor de automação de interface do usuário](../../../docs/framework/ui-automation/support-control-patterns-in-a-ui-automation-provider.md)
- [Padrões de controle de automação de interface do usuário para clientes](../../../docs/framework/ui-automation/ui-automation-control-patterns-for-clients.md)
- [Visão geral de árvore de automação de interface do usuário](../../../docs/framework/ui-automation/ui-automation-tree-overview.md)
- [Usar o cache em automação de interface do usuário](../../../docs/framework/ui-automation/use-caching-in-ui-automation.md)
