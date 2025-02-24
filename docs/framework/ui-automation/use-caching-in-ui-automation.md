---
title: Usar armazenamento em cache em automação de interface do usuário
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- caching, UI Automation
- UI Automation, caching
ms.assetid: ec722dff-6009-4279-b86a-e18d3fa94ebf
ms.openlocfilehash: 38c7742f3e4691f29490e73b05616754415eac58
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69953900"
---
# <a name="use-caching-in-ui-automation"></a>Usar armazenamento em cache em automação de interface do usuário
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]recentes sobre [o, consulte API de automação do Windows: Automação](https://go.microsoft.com/fwlink/?LinkID=156746)da interface do usuário.  
  
 Esta seção mostra como implementar o cache de <xref:System.Windows.Automation.AutomationElement> Propriedades e padrões de controle.  
  
### <a name="activate-a-cache-request"></a>Ativar uma solicitação de cache  
  
1. Criará um <xref:System.Windows.Automation.CacheRequest>.  
  
2. Especifique propriedades e padrões para armazenar em cache <xref:System.Windows.Automation.CacheRequest.Add%2A>usando.  
  
3. Especifique o escopo do cache definindo a <xref:System.Windows.Automation.CacheRequest.TreeScope%2A> propriedade.  
  
4. Especifique a exibição da subárvore definindo a <xref:System.Windows.Automation.CacheRequest.TreeFilter%2A> propriedade.  
  
5. Defina a <xref:System.Windows.Automation.CacheRequest.AutomationElementMode%2A> Propriedade como <xref:System.Windows.Automation.AutomationElementMode.None> se você quiser aumentar a eficiência ao não recuperar uma referência completa para objetos. (Isso fará com que seja impossível recuperar os valores atuais desses objetos.)  
  
6. Ative a solicitação usando <xref:System.Windows.Automation.CacheRequest.Activate%2A> dentro de um `using` bloco (`Using` no Microsoft Visual Basic .net).  
  
 Depois de <xref:System.Windows.Automation.AutomationElement> obter objetos ou assinar eventos, desative a solicitação usando <xref:System.Windows.Automation.CacheRequest.Pop%2A> (se <xref:System.Windows.Automation.CacheRequest.Push%2A> tiver sido usado) ou descartando o objeto criado <xref:System.Windows.Automation.CacheRequest.Activate%2A>pelo. (Use <xref:System.Windows.Automation.CacheRequest.Activate%2A> em um `using` bloco (`Using` no Microsoft Visual Basic .net).  
  
### <a name="cache-automationelement-properties"></a>Propriedades AutomationElement do cache  
  
1. Enquanto um <xref:System.Windows.Automation.CacheRequest> estiver ativo, obtenha <xref:System.Windows.Automation.AutomationElement> objetos usando <xref:System.Windows.Automation.AutomationElement.FindFirst%2A> ou <xref:System.Windows.Automation.AutomationElement.FindAll%2A>; ou obtenha um <xref:System.Windows.Automation.AutomationElement> como a origem de um evento que você registrou para quando o <xref:System.Windows.Automation.CacheRequest> estava ativo. (Você também pode criar um cache passando um <xref:System.Windows.Automation.CacheRequest> para GetUpdatedCache ou um <xref:System.Windows.Automation.TreeWalker> dos métodos.)  
  
2. Use <xref:System.Windows.Automation.AutomationElement.GetCachedPropertyValue%2A> ou recupere uma propriedade <xref:System.Windows.Automation.AutomationElement.Cached%2A> da Propriedade do <xref:System.Windows.Automation.AutomationElement>.  
  
### <a name="obtain-cached-patterns-and-their-properties"></a>Obter padrões armazenados em cache e suas propriedades  
  
1. Enquanto um <xref:System.Windows.Automation.CacheRequest> estiver ativo, obtenha <xref:System.Windows.Automation.AutomationElement> objetos usando <xref:System.Windows.Automation.AutomationElement.FindFirst%2A> ou <xref:System.Windows.Automation.AutomationElement.FindAll%2A>; ou obtenha um <xref:System.Windows.Automation.AutomationElement> como a origem de um evento que você registrou para quando o <xref:System.Windows.Automation.CacheRequest> estava ativo. (Você também pode criar um cache passando um <xref:System.Windows.Automation.CacheRequest> para GetUpdatedCache ou um <xref:System.Windows.Automation.TreeWalker> dos métodos.)  
  
2. Use <xref:System.Windows.Automation.AutomationElement.GetCachedPattern%2A> ou<xref:System.Windows.Automation.AutomationElement.TryGetCachedPattern%2A> para recuperar um padrão armazenado em cache.  
  
3. Recupere os valores de propriedade `Cached` da Propriedade do padrão de controle.  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir mostra vários aspectos do cache <xref:System.Windows.Automation.CacheRequest.Activate%2A> , usando o <xref:System.Windows.Automation.CacheRequest>para ativar o.  
  
 [!code-csharp[UIAClient_snip#107](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIAClient_snip/CSharp/ClientForm.cs#107)]
 [!code-vb[UIAClient_snip#107](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIAClient_snip/VisualBasic/ClientForm.vb#107)]  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir mostra vários aspectos do cache <xref:System.Windows.Automation.CacheRequest.Push%2A> , usando o <xref:System.Windows.Automation.CacheRequest>para ativar o. Exceto quando você deseja aninhar solicitações de cache, é preferível usar <xref:System.Windows.Automation.CacheRequest.Activate%2A>.  
  
 [!code-csharp[UIAClient_snip#108](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIAClient_snip/CSharp/ClientForm.cs#108)]
 [!code-vb[UIAClient_snip#108](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIAClient_snip/VisualBasic/ClientForm.vb#108)]  
  
## <a name="see-also"></a>Consulte também

- [Armazenamento em cache em clientes de automação de interface do usuário](../../../docs/framework/ui-automation/caching-in-ui-automation-clients.md)
