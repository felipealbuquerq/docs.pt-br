---
title: Visão geral de painéis
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- Panel control [WPF], about Panel control
- controls [WPF], Panel
ms.assetid: f73644af-9941-4611-8754-6d4cef03fc44
ms.openlocfilehash: 5fe464f2b79fa1f7b0674c049110d32f2ad32335
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69944816"
---
# <a name="panels-overview"></a>Visão geral de painéis
<xref:System.Windows.Controls.Panel>elementos são componentes que controlam a renderização de elementos — seu tamanho e dimensões, sua posição e a organização de seu conteúdo filho. O [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] fornece vários elementos predefinidos <xref:System.Windows.Controls.Panel> , bem como a capacidade de construir elementos <xref:System.Windows.Controls.Panel> personalizados.  
  
 Este tópico contém as seções a seguir.  
  
- [A classe do painel](#Panels_view_from_10000_feet)  
  
- [Membros comuns do elemento de painel](#Panels_declared_members)  
  
- [Elementos de painel derivados](#Panels_derived_elements)  
  
- [Painéis de interface do usuário](#Panels_main_UI_elements)  
  
- [Elementos de painel aninhados](#Panels_nested_panel_elements)  
  
- [Elementos de painel personalizados](#Panels_custom_panel_elements)  
  
- [Suporte à localização/globalização](#Panels_global_localization)  
  
<a name="Panels_view_from_10000_feet"></a>   
## <a name="the-panel-class"></a>A classe do painel  
 <xref:System.Windows.Controls.Panel>é a classe base para todos os elementos que fornecem suporte de [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)]layout no. Elementos <xref:System.Windows.Controls.Panel> derivados são usados para posicionar e organizar elementos [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] no e no código.  
  
 O [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] inclui um conjunto abrangente de implementações de painel derivadas que possibilita vários layouts complexos. Essas classes derivadas expõem propriedades e métodos que permitem a maioria dos cenários padrão de [!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)]. Os desenvolvedores que não conseguem encontrar um comportamento de organização filho que atenda às suas necessidades podem criar novos layouts substituindo <xref:System.Windows.FrameworkElement.MeasureOverride%2A> os <xref:System.Windows.FrameworkElement.ArrangeOverride%2A> métodos e. Para obter mais informações sobre comportamentos de layout personalizados, consulte [Elementos de painel personalizados](#Panels_custom_panel_elements).  
  
<a name="Panels_declared_members"></a>   
## <a name="panel-common-members"></a>Membros comuns do painel  
 Todos <xref:System.Windows.Controls.Panel> os elementos oferecem suporte ao dimensionamento de base e às propriedades de posicionamento definidas <xref:System.Windows.FrameworkElement.VerticalAlignment%2A>por <xref:System.Windows.FrameworkElement.Margin%2A> <xref:System.Windows.FrameworkElement>, <xref:System.Windows.FrameworkElement.Height%2A>incluindo <xref:System.Windows.FrameworkElement.LayoutTransform%2A>, <xref:System.Windows.FrameworkElement.Width%2A> <xref:System.Windows.FrameworkElement.HorizontalAlignment%2A>,,, e. Para obter informações adicionais sobre posicionamento de propriedades <xref:System.Windows.FrameworkElement>definidas por, consulte [visão geral de alinhamento, margens e preenchimento](../advanced/alignment-margins-and-padding-overview.md).  
  
 <xref:System.Windows.Controls.Panel>expõe propriedades adicionais que são de importância crítica na compreensão e no uso do layout. A <xref:System.Windows.Controls.Panel.Background%2A> propriedade é usada para preencher a área entre os limites de um elemento de painel derivado com <xref:System.Windows.Media.Brush>um. <xref:System.Windows.Controls.Panel.Children%2A>representa a coleção filho de elementos dos quais <xref:System.Windows.Controls.Panel> o é composto. <xref:System.Windows.Controls.Panel.InternalChildren%2A>representa o conteúdo da <xref:System.Windows.Controls.Panel.Children%2A> coleção mais os membros gerados pela Associação de dados. Ambos consistem em <xref:System.Windows.Controls.UIElementCollection> um dos elementos filho hospedados no <xref:System.Windows.Controls.Panel>pai.  
  
 O painel também expõe <xref:System.Windows.Controls.Panel.ZIndex%2A?displayProperty=nameWithType> uma propriedade anexada que pode ser usada para obter uma ordem em camadas <xref:System.Windows.Controls.Panel>em um derivado. Os membros da <xref:System.Windows.Controls.Panel.Children%2A> coleção de um painel com um <xref:System.Windows.Controls.Panel.ZIndex%2A?displayProperty=nameWithType> valor mais alto aparecem na frente daqueles com um <xref:System.Windows.Controls.Panel.ZIndex%2A?displayProperty=nameWithType> valor mais baixo. Isso é particularmente útil para painéis como <xref:System.Windows.Controls.Canvas> e <xref:System.Windows.Controls.Grid> que permitem que os filhos compartilhem o mesmo espaço de coordenadas.  
  
 <xref:System.Windows.Controls.Panel>também define o <xref:System.Windows.Controls.Panel.OnRender%2A> método, que pode ser usado para substituir o comportamento de apresentação padrão de <xref:System.Windows.Controls.Panel>um.  
  
#### <a name="attached-properties"></a>Propriedades anexadas  
 Elementos do painel derivados fazem amplo uso de propriedades anexadas. Uma propriedade anexada é uma forma especializada de propriedade de dependência que não tem a propriedade de Common Language Runtime convencional (CLR) "wrapper". As propriedades anexadas têm uma sintaxe especializada em [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)], que pode ser vista em vários dos exemplos a seguir.  
  
 Uma das finalidades de uma propriedade anexada é permitir que os elementos filho armazenem valores exclusivos de uma propriedade que, na verdade, é definida por um elemento pai. Uma aplicação dessa funcionalidade é fazer com que os elementos filho informem o pai como desejam ser apresentados na [!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)], o que é extremamente útil para o layout do aplicativo. Para obter mais informações, consulte [Visão geral das propriedades anexadas](../advanced/attached-properties-overview.md).  
  
<a name="Panels_derived_elements"></a>   
## <a name="derived-panel-elements"></a>Elementos de painel derivados  
 Muitos objetos derivam <xref:System.Windows.Controls.Panel>de, mas nem todos são destinados para uso como provedores de layout de raiz. Há seis classes de painel definidas (<xref:System.Windows.Controls.Canvas> <xref:System.Windows.Controls.Grid>, <xref:System.Windows.Controls.DockPanel> <xref:System.Windows.Controls.StackPanel> <xref:System.Windows.Controls.VirtualizingStackPanel>,,, e <xref:System.Windows.Controls.WrapPanel>) projetadas especificamente para a criação de aplicativos [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)].  
  
 Cada elemento de painel encapsula sua própria funcionalidade especial, como visto na tabela a seguir.  
  
|Nome de elementos|Painel de interface do usuário?|Descrição|  
|------------------|---------------|-----------------|  
|<xref:System.Windows.Controls.Canvas>|Sim|Define uma área na qual você pode posicionar explicitamente elementos filho por coordenadas em relação <xref:System.Windows.Controls.Canvas> à área.|  
|<xref:System.Windows.Controls.DockPanel>|Sim|Define uma área dentro da qual você pode organizar elementos filho horizontal ou verticalmente, uns em relação aos outros.|  
|<xref:System.Windows.Controls.Grid>|Sim|Define uma área de grade flexível que consiste em colunas e linhas. Os elementos filho de <xref:System.Windows.Controls.Grid> a podem ser posicionados precisamente <xref:System.Windows.FrameworkElement.Margin%2A> usando a propriedade.|  
|<xref:System.Windows.Controls.StackPanel>|Sim|Organiza elementos filho em uma única linha que pode ser orientada horizontal ou verticalmente.|  
|<xref:System.Windows.Controls.Primitives.TabPanel>|Não|Manipula o layout de botões de guia em <xref:System.Windows.Controls.TabControl>um.|  
|<xref:System.Windows.Controls.Primitives.ToolBarOverflowPanel>|Não|Organiza o conteúdo dentro de <xref:System.Windows.Controls.ToolBar> um controle.|  
|<xref:System.Windows.Controls.Primitives.UniformGrid>|Não|<xref:System.Windows.Controls.Primitives.UniformGrid>é usado para organizar os filhos em uma grade com todos os tamanhos de célula iguais.|  
|<xref:System.Windows.Controls.VirtualizingPanel>|Não|Fornece uma classe base para painéis que podem “virtualizar” sua coleção filho.|  
|<xref:System.Windows.Controls.VirtualizingStackPanel>|Sim|Organiza e virtualiza o conteúdo em uma única linha que é orientada horizontal ou verticalmente.|  
|<xref:System.Windows.Controls.WrapPanel>|Sim|<xref:System.Windows.Controls.WrapPanel>posiciona os elementos filho na posição sequencial da esquerda para a direita, quebrando o conteúdo para a próxima linha na borda da caixa que o contém. A ordenação subsequente ocorre sequencialmente de cima para baixo ou da direita para a esquerda, dependendo do valor <xref:System.Windows.Controls.WrapPanel.Orientation%2A> da propriedade.|  
  
<a name="Panels_main_UI_elements"></a>   
## <a name="user-interface-panels"></a>Painéis de interface do usuário  
 Há seis classes de painel disponíveis no [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] que são otimizadas para [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] oferecer suporte <xref:System.Windows.Controls.Canvas>a <xref:System.Windows.Controls.DockPanel> <xref:System.Windows.Controls.StackPanel>cenários <xref:System.Windows.Controls.Grid> <xref:System.Windows.Controls.VirtualizingStackPanel>:,,, <xref:System.Windows.Controls.WrapPanel>, e. Esses elementos de painel são fáceis de usar, versáteis e extensíveis o suficiente para a maioria dos aplicativos.  
  
 Cada elemento <xref:System.Windows.Controls.Panel> derivado trata as restrições de dimensionamento de forma diferente. Entender como uma <xref:System.Windows.Controls.Panel> restrição de identificadores na direção horizontal ou vertical pode tornar o layout mais previsível.  
  
|**Nome do Painel**|**Dimensão x**|**Dimensão y**|  
|--------------------|----------------------|----------------------|  
|<xref:System.Windows.Controls.Canvas>|Restrito ao conteúdo|Restrito ao conteúdo|  
|<xref:System.Windows.Controls.DockPanel>|Restrito|Restrito|  
|<xref:System.Windows.Controls.StackPanel>(Orientação vertical)|Restrito|Restrito ao conteúdo|  
|<xref:System.Windows.Controls.StackPanel>(Orientação horizontal)|Restrito ao conteúdo|Restrito|  
|<xref:System.Windows.Controls.Grid>|Restrito|Restrito, exceto nos casos em que <xref:System.Windows.GridUnitType.Auto> se aplicar a linhas e colunas|  
|<xref:System.Windows.Controls.WrapPanel>|Restrito ao conteúdo|Restrito ao conteúdo|  
  
 Encontre abaixo descrições mais detalhadas e exemplos de uso de cada um desses elementos.  
  
<a name="Panels_overview_Canvas_subsection"></a>   
### <a name="canvas"></a>Tela  
 O <xref:System.Windows.Controls.Canvas> elemento permite o posicionamento do conteúdo de acordo com as coordenadas *x* e *y*absolutas. Os elementos podem ser desenhados em um local exclusivo; ou, se os elementos ocupam as mesmas coordenadas, a ordem em que aparecem na marcação determina a ordem na qual os elementos são desenhados.  
  
 <xref:System.Windows.Controls.Canvas>fornece o suporte de layout mais flexível de <xref:System.Windows.Controls.Panel>qualquer um. As propriedades Height e Width são usadas para definir a área da tela e os elementos internos recebem coordenadas absolutas relativas à área do pai <xref:System.Windows.Controls.Canvas>. Quatro propriedades anexadas <xref:System.Windows.Controls.Canvas.Left%2A?displayProperty=nameWithType>, <xref:System.Windows.Controls.Canvas.Top%2A?displayProperty=nameWithType> <xref:System.Windows.Controls.Canvas.Right%2A?displayProperty=nameWithType> , e <xref:System.Windows.Controls.Canvas.Bottom%2A?displayProperty=nameWithType>, permitem um controle fino do posicionamento do objeto <xref:System.Windows.Controls.Canvas>em um, permitindo que o desenvolvedor Posicione e organize elementos precisamente na tela.  
  
#### <a name="cliptobounds-within-a-canvas"></a>ClipToBounds em uma tela  
 <xref:System.Windows.Controls.Canvas>pode posicionar elementos filho em qualquer posição na tela, mesmo em coordenadas que estão fora de seu próprio definido <xref:System.Windows.FrameworkElement.Height%2A> e <xref:System.Windows.FrameworkElement.Width%2A>. Além disso <xref:System.Windows.Controls.Canvas> , o não é afetado pelo tamanho de seus filhos. Como resultado, é possível que um elemento filho sobredesenhe outros elementos fora do retângulo delimitador do pai <xref:System.Windows.Controls.Canvas>. O comportamento padrão de a <xref:System.Windows.Controls.Canvas> é permitir que os filhos sejam desenhados fora dos limites do pai <xref:System.Windows.Controls.Canvas>. Se esse comportamento for indesejável, <xref:System.Windows.UIElement.ClipToBounds%2A> a propriedade poderá ser definida `true`como. Isso faz <xref:System.Windows.Controls.Canvas> com que o clipe seja seu próprio tamanho. <xref:System.Windows.Controls.Canvas>é o único elemento layout que permite que os filhos sejam desenhados fora de seus limites.  
  
 Esse comportamento é ilustrado de forma gráfica na [Amostra de comparação de propriedades de largura](https://go.microsoft.com/fwlink/?LinkID=160050).  
  
#### <a name="defining-and-using-a-canvas"></a>Definindo e usando uma tela  
 Um <xref:System.Windows.Controls.Canvas> pode ser instanciado simplesmente usando [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] o ou o Code. O exemplo a seguir demonstra como usar <xref:System.Windows.Controls.Canvas> o para posicionar absolutamente o conteúdo. Esse código produz três quadrados de 100 pixels. O primeiro quadrado é vermelho e sua posição superior esquerda (*X, Y*) é especificada como (0, 0). O segundo quadrado é verde e sua posição superior esquerda é (100, 100), logo abaixo e à direita do primeiro quadrado. O terceiro quadrado é azul e sua posição superior esquerda é (50, 50), abrangendo o quadrante inferior direito do primeiro quadrado e o quadrante superior esquerdo do segundo. Como o terceiro quadrado é disposto por último, ele parece estar sobre os outros dois quadrados – ou seja, as partes sobrepostas assumem a cor da terceira caixa.  
  
 [!code-csharp[CanvasOvwSample#1](~/samples/snippets/csharp/VS_Snippets_Wpf/CanvasOvwSample/CSharp/Canvas_Ovw_Sample.cs#1)]
 [!code-vb[CanvasOvwSample#1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CanvasOvwSample/VisualBasic/canvas_vb.vb#1)]
 [!code-xaml[CanvasOvwSample#1](~/samples/snippets/xaml/VS_Snippets_Wpf/CanvasOvwSample/XAML/default.xaml#1)]  
  
 O aplicativo compilado produz uma nova [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] parecida com a mostrada a seguir.  
  
 ![Um elemento Canvas típico.](./media/panel-intro-canvas.PNG "panel_intro_canvas")  
  
<a name="Panels_overview_DockPanel_subsection"></a>   
### <a name="dockpanel"></a>DockPanel  
 O <xref:System.Windows.Controls.DockPanel> elemento usa a <xref:System.Windows.Controls.DockPanel.Dock%2A?displayProperty=nameWithType> Propriedade anexada como definida em elementos de conteúdo filho para posicionar o conteúdo ao longo das bordas de um contêiner. Quando <xref:System.Windows.Controls.DockPanel.Dock%2A?displayProperty=nameWithType> é definido como <xref:System.Windows.Controls.Dock.Top> ou <xref:System.Windows.Controls.Dock.Bottom>, ele posiciona elementos filho acima ou abaixo uns dos outros. Quando <xref:System.Windows.Controls.DockPanel.Dock%2A?displayProperty=nameWithType> é definido como <xref:System.Windows.Controls.Dock.Left> ou <xref:System.Windows.Controls.Dock.Right>, ele posiciona os elementos filho à esquerda ou à direita uns dos outros. A <xref:System.Windows.Controls.DockPanel.LastChildFill%2A> propriedade determina a posição do elemento final adicionado como um filho de um <xref:System.Windows.Controls.DockPanel>.  
  
 Você pode usar <xref:System.Windows.Controls.DockPanel> para posicionar um grupo de controles relacionados, como um conjunto de botões. Como alternativa, você pode usá-lo para criar um "painel" [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)], semelhante ao encontrado no Microsoft Outlook.  
  
#### <a name="sizing-to-content"></a>Dimensionando para o conteúdo  
 Se suas <xref:System.Windows.FrameworkElement.Height%2A> propriedades <xref:System.Windows.FrameworkElement.Width%2A> e não forem especificadas, <xref:System.Windows.Controls.DockPanel> tamanhos para seu conteúdo. O tamanho pode aumentar ou diminuir para acomodar o tamanho de seus elementos filho. No entanto, quando essas propriedades são especificadas e não há mais espaço para o próximo elemento filho especificado <xref:System.Windows.Controls.DockPanel> , o não exibe esse elemento filho ou elementos filho subsequentes e não mede os elementos filho subsequentes.  
  
#### <a name="lastchildfill"></a>LastChildFill  
 Por padrão, o último filho de um <xref:System.Windows.Controls.DockPanel> elemento irá "preencher" o espaço restante não alocado. Se esse comportamento não for desejado, defina a <xref:System.Windows.Controls.DockPanel.LastChildFill%2A> Propriedade como `false`.  
  
#### <a name="defining-and-using-a-dockpanel"></a>Definindo e usando um DockPanel  
 O exemplo a seguir demonstra como particionar o espaço <xref:System.Windows.Controls.DockPanel>usando um. Cinco <xref:System.Windows.Controls.Border> elementos são adicionados como filhos de um pai <xref:System.Windows.Controls.DockPanel>. Cada um deles usa uma propriedade de posicionamento <xref:System.Windows.Controls.DockPanel> diferente de um para particionar espaço. O elemento final “preenche” o espaço restante não alocado.  
  
 [!code-cpp[DockPanelOvwSample#1](~/samples/snippets/cpp/VS_Snippets_Wpf/DockPanelOvwSample/CPP/DockPanel_Ovw_Sample.cpp#1)]
 [!code-csharp[DockPanelOvwSample#1](~/samples/snippets/csharp/VS_Snippets_Wpf/DockPanelOvwSample/CSharp/DockPanel_Ovw_Sample.cs#1)]
 [!code-vb[DockPanelOvwSample#1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DockPanelOvwSample/VisualBasic/dockpanel_vb.vb#1)]
 [!code-xaml[DockPanelOvwSample#1](~/samples/snippets/xaml/VS_Snippets_Wpf/DockPanelOvwSample/XAML/default.xaml#1)]  
  
 O aplicativo compilado produz uma nova [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] parecida com a mostrada a seguir.  
  
 ![Um cenário de DockPanel típico.](./media/panel-intro-dockpanel.PNG "panel_intro_dockpanel")  
  
<a name="Panels_overview_Grid_subsection"></a>   
### <a name="grid"></a>Grade  
 O <xref:System.Windows.Controls.Grid> elemento mescla a funcionalidade de um posicionamento absoluto e controle de dados tabulares. Um <xref:System.Windows.Controls.Grid> permite que você Posicione e modele facilmente elementos de estilo. <xref:System.Windows.Controls.Grid>permite que você defina agrupamentos de linhas e colunas flexíveis e até mesmo fornece um mecanismo para compartilhar informações de dimensionamento entre vários <xref:System.Windows.Controls.Grid> elementos.  
  
#### <a name="how-is-grid-different-from-table"></a>Qual a diferença entre uma grade e uma tabela?  
 <xref:System.Windows.Documents.Table>e <xref:System.Windows.Controls.Grid> Compartilhe algumas funcionalidades comuns, mas cada uma é mais adequada para cenários diferentes. Um <xref:System.Windows.Documents.Table> foi projetado para uso no conteúdo do Flow (consulte [visão geral do documento de fluxo](../advanced/flow-document-overview.md) para obter mais informações sobre o conteúdo do fluxo). As grades são melhor utilizadas em formulários (basicamente em qualquer lugar fora do conteúdo de fluxo). Dentro de <xref:System.Windows.Documents.FlowDocument>um <xref:System.Windows.Documents.Table> , o oferece suporte a comportamentos de conteúdo de fluxo como paginação, refluxo de coluna e seleção de conteúdo enquanto um <xref:System.Windows.Controls.Grid> não faz isso. R <xref:System.Windows.Controls.Grid> , por outro lado, é mais bem usado fora <xref:System.Windows.Documents.FlowDocument> de um por muitos <xref:System.Windows.Controls.Grid> motivos, incluindo adicionar elementos com base em um índice <xref:System.Windows.Documents.Table> de linha e coluna, não. O <xref:System.Windows.Controls.Grid> elemento permite a disposição em camadas de conteúdo filho, permitindo que mais de um elemento exista em uma única "célula". <xref:System.Windows.Documents.Table>não oferece suporte a camadas. Os elementos filho de <xref:System.Windows.Controls.Grid> a podem ser absolutamente posicionados em relação à área de seus limites de "célula". <xref:System.Windows.Documents.Table>o não oferece suporte a esse recurso. Por fim, <xref:System.Windows.Controls.Grid> um é um peso mais <xref:System.Windows.Documents.Table>leve do que um.  
  
#### <a name="sizing-behavior-of-columns-and-rows"></a>Comportamento de dimensionamento de colunas e linhas  
 Colunas e linhas definidas dentro de <xref:System.Windows.Controls.Grid> um podem aproveitar <xref:System.Windows.GridUnitType.Star> o dimensionamento para distribuir o espaço restante proporcionalmente. Quando <xref:System.Windows.GridUnitType.Star> é selecionado como a altura ou a largura de uma linha ou coluna, essa coluna ou linha recebe uma proporção ponderada do espaço restante disponível. Isso é diferente <xref:System.Windows.GridUnitType.Auto>de, que distribuirá o espaço uniformemente com base no tamanho do conteúdo dentro de uma coluna ou linha. Esse valor é expresso como `*` ou `2*` ao usar o [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)]. No primeiro caso, a linha ou a coluna receberia uma vez o espaço disponível, no segundo caso, duas vezes e assim por diante. Ao combinar essa técnica para distribuir proporcionalmente o espaço com um <xref:System.Windows.FrameworkElement.HorizontalAlignment%2A> valor <xref:System.Windows.FrameworkElement.VerticalAlignment%2A> `Stretch` e, é possível particionar o espaço de layout por percentual do espaço da tela. <xref:System.Windows.Controls.Grid>é o único painel de layout que pode distribuir espaço dessa maneira.  
  
#### <a name="defining-and-using-a-grid"></a>Definindo e usando uma grade  
 O exemplo a seguir demonstra como criar um [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] semelhante ao encontrado na caixa de diálogo Executar disponível no menu Iniciar do Windows.  
  
 [!code-csharp[GridRunDialog#1](~/samples/snippets/csharp/VS_Snippets_Wpf/GridRunDialog/CSharp/window1.xaml.cs#1)]
 [!code-vb[GridRunDialog#1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/GridRunDialog/VisualBasic/grid_vb.vb#1)]  
  
 O aplicativo compilado produz uma nova [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] parecida com a mostrada a seguir.  
  
 ![Um elemento Grid típico.](./media/avalon-run-dialog.PNG "avalon_run_dialog")  
  
<a name="Panels_overview_StackPanel_subsection"></a>   
### <a name="stackpanel"></a>StackPanel  
 Um <xref:System.Windows.Controls.StackPanel> permite que você "empilhe" elementos em uma direção atribuída. A direção padrão da pilha é vertical. A <xref:System.Windows.Controls.StackPanel.Orientation%2A> propriedade pode ser usada para controlar o fluxo de conteúdo.  
  
#### <a name="stackpanel-vs-dockpanel"></a>StackPanel vs. DockPanel  
 Embora <xref:System.Windows.Controls.DockPanel> o também possa "empilhar" <xref:System.Windows.Controls.DockPanel> elementos <xref:System.Windows.Controls.StackPanel> filho e não produza resultados análogos em alguns cenários de uso. Por exemplo, a ordem dos elementos filho pode afetar seu tamanho em um <xref:System.Windows.Controls.DockPanel> , mas não em <xref:System.Windows.Controls.StackPanel>um. Isso ocorre porque <xref:System.Windows.Controls.StackPanel> as medidas na direção do empilhamento <xref:System.Double.PositiveInfinity>em, <xref:System.Windows.Controls.DockPanel> enquanto mede apenas o tamanho disponível.  
  
 O exemplo a seguir demonstra essa diferença básica.  
  
 [!code-cpp[StackPanelOvw4#1](~/samples/snippets/cpp/VS_Snippets_Wpf/StackPanelOvw4/CPP/StackPanel_Ovw_Sample4.cpp#1)]
 [!code-csharp[StackPanelOvw4#1](~/samples/snippets/csharp/VS_Snippets_Wpf/StackPanelOvw4/CSharp/StackPanel_Ovw_Sample4.cs#1)]
 [!code-vb[StackPanelOvw4#1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/StackPanelOvw4/VisualBasic/StackPanelSamp.vb#1)]
 [!code-xaml[StackPanelOvw4#1](~/samples/snippets/xaml/VS_Snippets_Wpf/StackPanelOvw4/XAML/default.xaml#1)]  
  
 A diferença no comportamento de renderização pode ser vista nesta imagem.  
  
 ![Captura StackPanel vs. Captura de tela de DockPanel](./media/layout-smiley-stackpanel.PNG "layout_smiley_stackpanel")  
  
#### <a name="defining-and-using-a-stackpanel"></a>Definindo e usando um StackPanel  
 O exemplo a seguir demonstra como usar um <xref:System.Windows.Controls.StackPanel> para criar um conjunto de botões posicionados verticalmente. Para posicionamento horizontal, defina a <xref:System.Windows.Controls.StackPanel.Orientation%2A> Propriedade como <xref:System.Windows.Controls.Orientation.Horizontal>.  
  
 [!code-csharp[StackPanel_ovw2#1](~/samples/snippets/csharp/VS_Snippets_Wpf/StackPanel_ovw2/CSharp/StackPanel_Ovw_Sample2.cs#1)]
 [!code-vb[StackPanel_ovw2#1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/StackPanel_ovw2/VisualBasic/StackPanelOvw.vb#1)]  
  
 O aplicativo compilado produz uma nova [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] parecida com a mostrada a seguir.  
  
 ![Um elemento StackPanel típico.](./media/panel-intro-stackpanel.PNG "panel_intro_stackpanel")  
  
<a name="Panels_overview_VirtualizingStackPanel_subsection"></a>   
#### <a name="virtualizingstackpanel"></a>VirtualizingStackPanel  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]também fornece uma variação do elemento <xref:System.Windows.Controls.StackPanel> que "virtualiza" automaticamente o conteúdo filho associado a dados. Nesse contexto, a palavra virtualizar refere-se a uma técnica pela qual um subconjunto de elementos é gerado de um grande número de itens de dados com base em quais itens estão visíveis na tela. É intensivo, tanto em termos de memória quanto de processador, gerar um grande número de elementos de interface do usuário quando apenas alguns podem estar na tela em um determinado momento. <xref:System.Windows.Controls.VirtualizingStackPanel>(por meio da <xref:System.Windows.Controls.VirtualizingPanel> <xref:System.Windows.Controls.ItemsControl> <xref:System.Windows.Controls.ListView> <xref:System.Windows.Controls.ItemContainerGenerator> funcionalidadefornecidapor)calculaositensvisíveisetrabalhacomodeum(comoou)paracriarapenaselementosparaitensvisíveis.<xref:System.Windows.Controls.ListBox>  
  
 O <xref:System.Windows.Controls.VirtualizingStackPanel> elemento é definido automaticamente como o host de itens para controles como o <xref:System.Windows.Controls.ListBox>. Ao hospedar uma coleção de associação de dados, o conteúdo é virtualizado automaticamente, desde que o conteúdo esteja dentro dos limites de <xref:System.Windows.Controls.ScrollViewer>um. Isso melhora o desempenho consideravelmente ao hospedar vários itens filho.  
  
 A marcação a seguir demonstra como usar um <xref:System.Windows.Controls.VirtualizingStackPanel> como um host de itens. A <xref:System.Windows.Controls.VirtualizingStackPanel.IsVirtualizingProperty?displayProperty=nameWithType> Propriedade anexada deve ser definida `true` como (padrão) para que a virtualização ocorra.  
  
 [!code-xaml[VirtualizingStackPanel_Intro#1](~/samples/snippets/csharp/VS_Snippets_Wpf/VirtualizingStackPanel_Intro/CS/default.xaml#1)]  
  
<a name="Panels_overview_WrapPanel"></a>   
### <a name="wrappanel"></a>WrapPanel  
 <xref:System.Windows.Controls.WrapPanel>é usado para posicionar elementos filho em posição sequencial da esquerda para a direita, quebrando o conteúdo para a próxima linha quando atingir a borda de seu contêiner pai. O conteúdo pode tem uma orientação horizontal ou vertical. <xref:System.Windows.Controls.WrapPanel>é útil para cenários de [!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)] fluxo simples. Também pode ser usado para aplicar um dimensionamento uniforme a todos os seus elementos filho.  
  
 O exemplo a seguir demonstra como criar um <xref:System.Windows.Controls.WrapPanel> para exibir <xref:System.Windows.Controls.Button> controles que são encapsulados quando atingem a borda de seu contêiner.  
  
 [!code-cpp[WrapPanel_Intro#1](~/samples/snippets/cpp/VS_Snippets_Wpf/WrapPanel_Intro/CPP/WrapPanel_Code.cpp#1)]
 [!code-csharp[WrapPanel_Intro#1](~/samples/snippets/csharp/VS_Snippets_Wpf/WrapPanel_Intro/CSharp/WrapPanel_Code.cs#1)]
 [!code-vb[WrapPanel_Intro#1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WrapPanel_Intro/VisualBasic/WrapPanel_vb.vb#1)]
 [!code-xaml[WrapPanel_Intro#1](~/samples/snippets/xaml/VS_Snippets_Wpf/WrapPanel_Intro/XAML/default.xaml#1)]  
  
 O aplicativo compilado produz uma nova [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] parecida com a mostrada a seguir.  
  
 ![Um elemento WrapPanel típico.](./media/wrappanel-element.PNG "WrapPanel_Element")  
  
<a name="Panels_nested_panel_elements"></a>   
## <a name="nested-panel-elements"></a>Elementos de painel aninhados  
 <xref:System.Windows.Controls.Panel>os elementos podem ser aninhados entre si para produzir layouts complexos. Isso pode ser muito útil em situações em que <xref:System.Windows.Controls.Panel> uma é ideal para uma parte de [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)]um, mas pode não atender às necessidades de [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)]uma parte diferente do.  
  
 Não há nenhum limite prático para a quantidade de aninhamento à qual o aplicativo pode dar suporte; no entanto, de modo geral, é melhor limitar o aplicativo a usar somente os painéis que são realmente necessários para o layout desejado. Em muitos casos, um <xref:System.Windows.Controls.Grid> elemento pode ser usado em vez de painéis aninhados devido à sua flexibilidade como um contêiner de layout. Isso pode aumentar o desempenho do aplicativo mantendo os elementos desnecessários fora da árvore.  
  
 O exemplo a seguir demonstra como criar um [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] que aproveita os elementos aninhados <xref:System.Windows.Controls.Panel> para atingir um layout específico. Nesse caso em particular, um <xref:System.Windows.Controls.DockPanel> elemento é usado para fornecer [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] estrutura, e elementos <xref:System.Windows.Controls.StackPanel> aninhados, <xref:System.Windows.Controls.Grid>a e a <xref:System.Windows.Controls.Canvas> são usados para posicionar os elementos filho precisamente dentro <xref:System.Windows.Controls.DockPanel>do pai.  
  
 [!code-csharp[Nested_Panels#1](~/samples/snippets/csharp/VS_Snippets_Wpf/Nested_Panels/CSharp/nestedpanels.cs#1)]
 [!code-vb[Nested_Panels#1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/Nested_Panels/VisualBasic/nestedpanels.vb#1)]  
  
 O aplicativo compilado produz uma nova [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] parecida com a mostrada a seguir.  
  
 ![Uma interface do usuário que aproveita os painéis aninhados.](./media/nested-panels.PNG "nested_panels")  
  
<a name="Panels_custom_panel_elements"></a>   
## <a name="custom-panel-elements"></a>Elementos de painel personalizados  
 Embora [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] o forneça uma matriz de controles de layout flexíveis, os comportamentos de layout personalizados também podem ser <xref:System.Windows.FrameworkElement.ArrangeOverride%2A> obtidos <xref:System.Windows.FrameworkElement.MeasureOverride%2A> pela substituição dos métodos e. O dimensionamento e o posicionamento personalizados podem ser feitos com a definição de novos comportamentos de posicionamento nesses métodos de substituição.  
  
 Da mesma forma, os comportamentos de layout personalizados baseados em classes <xref:System.Windows.Controls.Canvas> derivadas <xref:System.Windows.Controls.Grid>(como ou) podem ser definidos <xref:System.Windows.FrameworkElement.ArrangeOverride%2A> pela <xref:System.Windows.FrameworkElement.MeasureOverride%2A> substituição de seus métodos e.  
  
 A marcação a seguir demonstra como criar um elemento <xref:System.Windows.Controls.Panel> personalizado. Esse novo <xref:System.Windows.Controls.Panel>, definido como `PlotPanel`, dá suporte ao posicionamento de elementos filho por meio do uso de coordenadas *x-* e *y*embutidas em código. Neste exemplo, um <xref:System.Windows.Shapes.Rectangle> elemento (não mostrado) é posicionado no ponto de plotagem 50 (*x*) e 50 (*y*).  
  
 [!code-cpp[PlotPanel#1](~/samples/snippets/cpp/VS_Snippets_Wpf/PlotPanel/CPP/PlotPanel.cpp#1)]
 [!code-csharp[PlotPanel#1](~/samples/snippets/csharp/VS_Snippets_Wpf/PlotPanel/CSharp/PlotPanel.cs#1)]
 [!code-vb[PlotPanel#1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PlotPanel/VisualBasic/PlotPanel.vb#1)]  
  
 Para exibir uma implementação mais complexa de painel personalizado, consulte [Criar uma amostra de painel de encapsulamento com conteúdo personalizado](https://go.microsoft.com/fwlink/?LinkID=159979).  
  
<a name="Panels_global_localization"></a>   
## <a name="localizationglobalization-support"></a>Suporte à localização/globalização  
 O [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] dá suporte a vários recursos que ajudam na criação de uma [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] localizável.  
  
 Todos os elementos Panel oferecem suporte nativo <xref:System.Windows.FrameworkElement.FlowDirection%2A> à propriedade, que pode ser usado para refluir dinamicamente o conteúdo com base nas configurações de idioma ou localidade de um usuário. Para obter mais informações, consulte <xref:System.Windows.FrameworkElement.FlowDirection%2A>.  
  
 A <xref:System.Windows.Window.SizeToContent%2A> propriedade fornece um mecanismo que permite aos desenvolvedores de aplicativos prever as necessidades do localizadas [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)]. Usando o <xref:System.Windows.SizeToContent.WidthAndHeight> valor dessa propriedade, um pai <xref:System.Windows.Window> sempre dimensiona dinamicamente para ajustar o conteúdo e não é restrito pela altura artificial ou restrições de largura.  
  
 <xref:System.Windows.Controls.DockPanel>, <xref:System.Windows.Controls.Grid> [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)]e <xref:System.Windows.Controls.StackPanel> são todas boas opções para localizáveis. <xref:System.Windows.Controls.Canvas>Não é uma boa opção, no entanto, porque ele posiciona o conteúdo absolutamente, dificultando a localização.  
  
 Para obter mais informações sobre como criar aplicativos do [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] com [!INCLUDE[TLA#tla_ui#plural](../../../../includes/tlasharptla-uisharpplural-md.md)]s localizáveis, consulte a [Visão geral do uso de layout automático](../advanced/use-automatic-layout-overview.md).  
  
## <a name="see-also"></a>Consulte também

- [Passo a passo: Meu primeiro aplicativo da área de trabalho do WPF](../getting-started/walkthrough-my-first-wpf-desktop-application.md)
- [Exemplo de galeria de layout do WPF](https://go.microsoft.com/fwlink/?LinkID=160054)
- [Layout](../advanced/layout.md)
- [Exemplo da galeria de controles de WPF](https://go.microsoft.com/fwlink/?LinkID=160053)
- [Visão geral de alinhamento, margens e preenchimento](../advanced/alignment-margins-and-padding-overview.md)
- [Criar um exemplo de painel de quebra automática de conteúdo personalizado](https://go.microsoft.com/fwlink/?LinkID=159979)
- [Visão geral das propriedades anexadas](../advanced/attached-properties-overview.md)
- [Visão geral do uso de layout automático](../advanced/use-automatic-layout-overview.md)
- [Layout e design](../advanced/optimizing-performance-layout-and-design.md)
