---
title: Visão geral da criação de controle
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- controls [WPF], authoring overview
- authoring overview for controls [WPF]
ms.assetid: 3d864748-cff0-4e63-9b23-d8e5a635b28f
ms.openlocfilehash: 61cd7b61a586afa2addd55acff7cac6d16d92a1f
ms.sourcegitcommit: c70542d02736e082e8dac67dad922c19249a8893
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/05/2019
ms.locfileid: "70374533"
---
# <a name="control-authoring-overview"></a>Visão geral da criação de controle

A extensibilidade do modelo de controle do [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] reduz consideravelmente a necessidade de criar um novo controle. No entanto, em alguns casos, você ainda precisará criar um controle personalizado. Este tópico aborda os recursos que minimizam sua necessidade de criar um controle personalizado e os diferentes modelos de criação no [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)]. Este tópico também demonstra como criar um novo controle.

<a name="when_to_write_a_new_control"></a>

## <a name="alternatives-to-writing-a-new-control"></a>Alternativas a escrever um novo controle

Historicamente, se você quisesse obter uma experiência personalizada de um controle existente, você era limitado a alterar as propriedades padrão do controle, como a cor da tela de fundo, a largura da borda e o tamanho da fonte. Se você quisesse estender a aparência ou o comportamento de um controle além desses parâmetros predefinidos, você precisaria criar um novo controle, geralmente herdando um controle existente e substituindo o método responsável por desenhar o controle.  Embora ainda seja uma opção, o [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] lhe permite personalizar os controles existentes usando seu modelo de conteúdo sofisticado, estilos, modelos e disparadores. A lista a seguir fornece exemplos de como esses recursos podem ser usados para criar experiências personalizadas e consistentes sem ter de criar um novo controle.

- **Conteúdo sofisticado.** Muitos dos controles padrão do [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] dão suporte a conteúdo sofisticado. Por exemplo, a propriedade Content de um <xref:System.Windows.Controls.Button> é do tipo <xref:System.Object>e, teoricamente, qualquer coisa pode ser exibida <xref:System.Windows.Controls.Button>em um.  Para que um botão exiba uma imagem e um texto, você pode adicionar uma imagem e <xref:System.Windows.Controls.TextBlock> uma <xref:System.Windows.Controls.StackPanel> a <xref:System.Windows.Controls.ContentControl.Content%2A> e atribuir a <xref:System.Windows.Controls.StackPanel> à propriedade. Como os controles podem exibir dados arbitrários e elementos visuais do [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)], há menos necessidade de criar um novo controle ou modificar um controle existente para dar suporte a uma visualização complexa. Para obter mais informações sobre o modelo de <xref:System.Windows.Controls.Button> conteúdo para o e outros [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]modelos de conteúdo no, consulte [modelo de conteúdo do WPF](wpf-content-model.md).

- **Estilos.** Um <xref:System.Windows.Style> é uma coleção de valores que representam as propriedades de um controle. Usando estilos, você pode criar uma representação reutilizável da aparência e comportamento de um controle desejado sem escrever um novo controle. Por exemplo, suponha que você queira que todos os <xref:System.Windows.Controls.TextBlock> seus controles tenham uma fonte vermelha, Arial com um tamanho de fonte de 14. Você pode criar um estilo como um recurso e definir as propriedades adequadas de acordo com isso. Em seguida <xref:System.Windows.Controls.TextBlock> , cada um que você adicionar ao seu aplicativo terá a mesma aparência.

- **Modelos de dados.** Um <xref:System.Windows.DataTemplate> permite que você personalize como os dados são exibidos em um controle. Por exemplo, um <xref:System.Windows.DataTemplate> pode ser usado para especificar como os dados são exibidos em <xref:System.Windows.Controls.ListBox>um.  Para obter um exemplo disso, consulte [Visão geral de modelagem de dados](../data/data-templating-overview.md).  Além de personalizar a aparência dos dados, um <xref:System.Windows.DataTemplate> pode incluir elementos da interface do usuário, o que proporciona muita flexibilidade em UIs personalizadas.  Por exemplo, usando um <xref:System.Windows.DataTemplate>, você pode criar um <xref:System.Windows.Controls.ComboBox> no qual cada item contém uma caixa de seleção.

- **Modelos de controle.** Muitos controles em [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] usam um <xref:System.Windows.Controls.ControlTemplate> para definir a estrutura e a aparência do controle, o que separa a aparência de um controle da funcionalidade do controle. Você pode alterar drasticamente a aparência de um controle redefinindo seu <xref:System.Windows.Controls.ControlTemplate>.  Por exemplo, suponha que você deseja um controle que se pareça com um alerta. Esse controle tem uma interface do usuário e uma e funcionalidade simples.  O controle é composto de três círculos, sendo que apenas um deles pode ser aceso por vez. Após alguns reflexos, você pode perceber que <xref:System.Windows.Controls.RadioButton> um oferece a funcionalidade de apenas um selecionado de cada vez, mas a aparência padrão <xref:System.Windows.Controls.RadioButton> do parece nada como as luzes de um alerta.  Como o <xref:System.Windows.Controls.RadioButton> usa um modelo de controle para definir sua aparência, é fácil redefinir o <xref:System.Windows.Controls.ControlTemplate> para se ajustar aos requisitos do controle e usar botões de opção para fazer seu alerta.

  > [!NOTE]
  > Embora um <xref:System.Windows.Controls.RadioButton> possa usar um <xref:System.Windows.DataTemplate>, um <xref:System.Windows.DataTemplate> não é suficiente neste exemplo.  O <xref:System.Windows.DataTemplate> define a aparência do conteúdo de um controle. No caso de <xref:System.Windows.Controls.RadioButton>a, o conteúdo é o que aparece à direita do círculo que indica se o <xref:System.Windows.Controls.RadioButton> está selecionado.  No exemplo do alerta, o botão de opção precisa ser apenas um círculo que pode se "acender." Como o requisito de aparência para o alerta é tão diferente da aparência padrão do <xref:System.Windows.Controls.RadioButton>, é necessário redefinir o. <xref:System.Windows.Controls.ControlTemplate>  Em geral, <xref:System.Windows.DataTemplate> a é usada para definir o conteúdo (ou dados) de um controle e uma <xref:System.Windows.Controls.ControlTemplate> é usada para definir como um controle é estruturado.

- **Gatilhos.** Um <xref:System.Windows.Trigger> permite que você altere dinamicamente a aparência e o comportamento de um controle sem criar um novo controle. Por exemplo, suponha que você tenha <xref:System.Windows.Controls.ListBox> vários controles em seu aplicativo e queira que os itens <xref:System.Windows.Controls.ListBox> em cada um sejam negrito e vermelho quando forem selecionados. Seu primeiro instinto pode ser criar uma classe que herda de <xref:System.Windows.Controls.ListBox> e substituir o <xref:System.Windows.Controls.Primitives.Selector.OnSelectionChanged%2A> método para alterar a aparência do item selecionado, mas uma abordagem melhor é adicionar um gatilho a um estilo de um <xref:System.Windows.Controls.ListBoxItem> que altera a aparência de o item selecionado. Um gatilho permite que você altere os valores da propriedade ou execute ações com base no valor de uma propriedade. Um <xref:System.Windows.EventTrigger> permite que você execute ações quando um evento ocorre.

Para obter mais informações sobre estilos, modelos e gatilhos, consulte [Estilo e modelagem](styling-and-templating.md).

Em geral, se o seu controle espelha a funcionalidade de um controle existente mas você deseja que seu controle tenha uma aparência diferente, você deve primeiro considerar se você pode ou não usar qualquer um dos métodos abordados nesta seção para alterar a aparência do controle existente.

<a name="models_for_control_authoring"></a>

## <a name="models-for-control-authoring"></a>Modelos para criação de controles

O modelo de conteúdo sofisticado, estilos, modelos e gatilhos minimizam a necessidade de criar um novo controle. No entanto, se você precisar criar um novo controle, será importante compreender os diferentes modelos de criação no [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]. O [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] fornece três modelos gerais para criar um controle, cada um deles fornece um conjunto de recursos e um nível de flexibilidade diferentes. As classes base para os três modelos são <xref:System.Windows.Controls.UserControl>, <xref:System.Windows.Controls.Control>e <xref:System.Windows.FrameworkElement>.

### <a name="deriving-from-usercontrol"></a>Derivar de UserControl

A maneira mais simples de criar um controle no [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] é derivar de <xref:System.Windows.Controls.UserControl>. Ao criar um controle herdado do <xref:System.Windows.Controls.UserControl>, você adiciona componentes existentes <xref:System.Windows.Controls.UserControl>ao, nomeie os componentes e os manipuladores de eventos de referência no [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)]. Em seguida, você pode referenciar os elementos nomeados e definir os manipuladores de eventos no código. Este modelo de desenvolvimento é muito semelhante ao modelo usado para o desenvolvimento de aplicativos no [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)].

Se criado corretamente, um <xref:System.Windows.Controls.UserControl> pode aproveitar os benefícios do conteúdo, dos estilos e dos gatilhos avançados. No entanto, se o controle <xref:System.Windows.Controls.UserControl>for herdado de, as pessoas que usam seu controle não poderão <xref:System.Windows.DataTemplate> usar <xref:System.Windows.Controls.ControlTemplate> um ou para personalizar sua aparência.  É necessário derivar da <xref:System.Windows.Controls.Control> classe ou de uma de suas classes derivadas (diferente de <xref:System.Windows.Controls.UserControl>) para criar um controle personalizado que dê suporte a modelos.

#### <a name="benefits-of-deriving-from-usercontrol"></a>Benefícios de derivar de UserControl

Considere a derivação <xref:System.Windows.Controls.UserControl> de se todas as seguintes opções se aplicarem:

- Você deseja criar o controle da mesma forma como cria um aplicativo.

- O controle consiste somente de componentes existentes.

- Não é necessário dar suporte a personalização complexa.

### <a name="deriving-from-control"></a>Derivar de Control

Derivar da <xref:System.Windows.Controls.Control> classe é o modelo usado pela maioria dos controles existentes [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] . Quando você cria um controle que herda da <xref:System.Windows.Controls.Control> classe, você define sua aparência usando modelos. Fazendo isso, você pode separar a lógica operacional da representação visual. Você também pode garantir a desassociação da interface do usuário e da lógica usando comandos e associações em vez de eventos e evitando a <xref:System.Windows.Controls.ControlTemplate> referência de elementos no sempre que possível.  Se a interface do usuário e a lógica do seu controle estiverem desacopladas corretamente, um usuário do seu controle poderá redefinir o <xref:System.Windows.Controls.ControlTemplate> controle para personalizar sua aparência. Embora criar um personalizado <xref:System.Windows.Controls.Control> não seja tão simples quanto criar um <xref:System.Windows.Controls.UserControl>, um personalizado <xref:System.Windows.Controls.Control> fornece mais flexibilidade.

#### <a name="benefits-of-deriving-from-control"></a>Benefícios de derivar de Control

Considere a derivação <xref:System.Windows.Controls.Control> de, em vez <xref:System.Windows.Controls.UserControl> de usar a classe, se qualquer uma das seguintes opções for aplicável:

- Você deseja que a aparência do seu controle seja personalizável por meio <xref:System.Windows.Controls.ControlTemplate>do.

- Você quiser que o controle dê suporte a diferentes temas.

### <a name="deriving-from-frameworkelement"></a>Derivar de FrameworkElement

Controles que derivam <xref:System.Windows.Controls.UserControl> de <xref:System.Windows.Controls.Control> ou contam com a composição de elementos existentes. Para muitos cenários, essa é uma solução aceitável, pois qualquer objeto que herde <xref:System.Windows.FrameworkElement> de pode estar em <xref:System.Windows.Controls.ControlTemplate>um. No entanto, há vezes em que a aparência de um controle requer mais do que a funcionalidade da composição simples de elementos. Para esses cenários, basear um componente <xref:System.Windows.FrameworkElement> no é a escolha certa.

Há dois métodos padrão para criar componentes <xref:System.Windows.FrameworkElement>baseados em criação: renderização direta e composição de elemento personalizado. A renderização direta envolve substituir <xref:System.Windows.UIElement.OnRender%2A> o método <xref:System.Windows.FrameworkElement> de e <xref:System.Windows.Media.DrawingContext> fornecer operações que definem explicitamente os elementos visuais do componente. Esse é o método usado pelo <xref:System.Windows.Controls.Image> e <xref:System.Windows.Controls.Border>pelo. A composição de elemento personalizado envolve o uso <xref:System.Windows.Media.Visual> de objetos do tipo para compor a aparência do seu componente. Para obter um exemplo, consulte [Usando objetos DrawingVisual](../graphics-multimedia/using-drawingvisual-objects.md). <xref:System.Windows.Controls.Primitives.Track>é um exemplo de um controle no [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] que usa composição de elemento personalizado. Também é possível misturar renderização direta e composição personalizada de elementos no mesmo controle.

#### <a name="benefits-of-deriving-from-frameworkelement"></a>Benefícios de derivar de FrameworkElement

Considere a derivação <xref:System.Windows.FrameworkElement> de se alguma das seguintes opções se aplicar:

- Você deseja ter controle preciso sobre a aparência de seu controle além do que é proporcionado pela composição simples de elementos.

- Você deseja definir a aparência de seu controle definindo sua própria lógica de renderização.

- Você quer compor elementos existentes de maneiras novas que vão além do que é possível com <xref:System.Windows.Controls.UserControl> o <xref:System.Windows.Controls.Control>e o.

<a name="control_authoring_basics"></a>

## <a name="control-authoring-basics"></a>Noções básicas de criação de controles

Conforme discutido anteriormente, um dos recursos mais poderosos do [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] é a capacidade de ir além da configuração das propriedades básicas de um controle para alterar sua aparência e comportamento, ainda assim não precisando criar um controle personalizado. Os estilos, vinculação de dados e recursos de gatilho são possibilitados pelo sistema de propriedades do [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] e o sistema de eventos do [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]. As seções a seguir descrevem algumas práticas recomendadas que devem ser seguidas independentemente do modelo que você usar para criar o controle personalizado, de modo que os usuários do seu controle personalizado possam usar esses recursos exatamente como fariam para controle incluído no [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)].

### <a name="use-dependency-properties"></a>Usar propriedades de dependência

Quando uma propriedade é uma propriedade de dependência, é possível fazer o seguinte:

- Defina a propriedade em um estilo.

- Associe a propriedade a uma fonte de dados.

- Use um recurso dinâmico como o valor da propriedade.

- Anime a propriedade.

Se você quiser que uma propriedade de seu controle dê suporte a qualquer parte dessa funcionalidade, você deverá implementá-la como uma propriedade de dependência. O exemplo a seguir define uma propriedade de dependência chamada `Value`, fazendo o seguinte:

- Defina um <xref:System.Windows.DependencyProperty> identificador chamado `ValueProperty` como um `public` `static` campo`readonly` .

- Registre o nome da propriedade com o sistema de propriedades, <xref:System.Windows.DependencyProperty.Register%2A?displayProperty=nameWithType>chamando, para especificar o seguinte:

  - O nome da propriedade.

  - O tipo da propriedade.

  - O tipo que é proprietário da propriedade.

  - Os metadados da propriedade. Os metadados contêm o valor padrão da propriedade, a <xref:System.Windows.CoerceValueCallback> e a <xref:System.Windows.PropertyChangedCallback>.

- Defina uma propriedade de wrapper de `Value`CLR chamada, que é o mesmo nome usado para registrar a propriedade de dependência, implementando os acessadores `get` e `set` a propriedade. Observe que o `get` e `set` os acessadores <xref:System.Windows.DependencyObject.GetValue%2A> só <xref:System.Windows.DependencyObject.SetValue%2A> chamam e, respectivamente. É recomendável que os acessadores de propriedades de dependência não contenham lógica [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] adicional, pois clientes e podem ignorar <xref:System.Windows.DependencyObject.GetValue%2A> os <xref:System.Windows.DependencyObject.SetValue%2A> acessadores e chamar e diretamente. Por exemplo, quando uma propriedade é associada a uma fonte de dados, o acessador `set` da propriedade não é chamado.  Em vez de adicionar lógica adicional aos acessadores get e Set, use <xref:System.Windows.ValidateValueCallback>os <xref:System.Windows.CoerceValueCallback>delegados, <xref:System.Windows.PropertyChangedCallback> e para responder ou verificar o valor quando ele for alterado.  Para obter mais informações sobre esses retornos de chamada, consulte [Retornos de chamada de propriedade de dependência e validação](../advanced/dependency-property-callbacks-and-validation.md).

- Defina um método para o <xref:System.Windows.CoerceValueCallback> nome `CoerceValue`. `CoerceValue` garante que `Value` seja maior ou igual a `MinValue` e menor ou igual a `MaxValue`.

- Defina um método para o <xref:System.Windows.PropertyChangedCallback>, chamado `OnValueChanged`. `OnValueChanged`Cria um <xref:System.Windows.RoutedPropertyChangedEventArgs%601> objeto e se prepara para gerar o `ValueChanged` evento roteado. Eventos roteados são abordados na próxima seção.

[!code-csharp[UserControlNumericUpDown#DependencyProperty](~/samples/snippets/csharp/VS_Snippets_Wpf/UserControlNumericUpDown/CSharp/NumericUpDown.xaml.cs#dependencyproperty)]
[!code-vb[UserControlNumericUpDown#DependencyProperty](~/samples/snippets/visualbasic/VS_Snippets_Wpf/UserControlNumericUpDown/visualbasic/numericupdown.xaml.vb#dependencyproperty)]

Para obter mais informações, consulte [Propriedades de dependência personalizadas](../advanced/custom-dependency-properties.md).

### <a name="use-routed-events"></a>Usar eventos roteados

Assim como as propriedades de dependência estendem a noção de propriedades CLR com funcionalidade adicional, os eventos roteados estendem a noção de eventos CLR padrão. Quando você cria um novo controle do [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)], também é uma boa prática implementar seu evento como um evento roteado, porque um evento roteado dá suporte ao seguinte comportamento:

- Eventos podem ser manipulados em um pai de vários controles. Se um evento é um evento por propagação, um único pai na árvore de elementos pode assinar o evento. Em seguida, autores de aplicativos podem usar um manipulador para responder ao evento de vários controles. Por exemplo, se o controle for uma parte de cada item em um <xref:System.Windows.Controls.ListBox> (porque está incluído em um <xref:System.Windows.DataTemplate>), o desenvolvedor do aplicativo poderá definir o manipulador de eventos para o evento do seu <xref:System.Windows.Controls.ListBox>controle no. Sempre que o evento ocorre em qualquer um dos controles, o manipulador de eventos é chamado.

- Eventos roteados podem ser usados em <xref:System.Windows.EventSetter>um, o que permite que os desenvolvedores de aplicativos especifiquem o manipulador de um evento em um estilo.

- Eventos roteados podem ser usados em <xref:System.Windows.EventTrigger>um, o que é útil para animar [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]propriedades usando. Para obter mais informações, consulte [Visão geral de animação](../graphics-multimedia/animation-overview.md).

O exemplo a seguir define um evento roteado fazendo o seguinte:

- Defina um <xref:System.Windows.RoutedEvent> identificador chamado `ValueChangedEvent` como um `public` `static` campo`readonly` .

- Registre o evento roteado chamando o <xref:System.Windows.EventManager.RegisterRoutedEvent%2A?displayProperty=nameWithType> método. O exemplo especifica as seguintes informações ao chamar <xref:System.Windows.EventManager.RegisterRoutedEvent%2A>:

  - O nome do evento é `ValueChanged`.

  - A estratégia de roteamento <xref:System.Windows.RoutingStrategy.Bubble>é, o que significa que um manipulador de eventos na origem (o objeto que gera o evento) é chamado primeiro e os manipuladores de eventos nos elementos pai da origem são chamados sucessivamente, começando com o manipulador de eventos no mais próximo elemento pai.

  - O tipo do manipulador de eventos é <xref:System.Windows.RoutedPropertyChangedEventHandler%601>construído com um <xref:System.Decimal> tipo.

  - O tipo proprietário do evento é `NumericUpDown`.

- Declare um evento público chamado `ValueChanged` e inclua declarações de acessador de evento. O exemplo chama <xref:System.Windows.UIElement.AddHandler%2A> a `add` declaração de <xref:System.Windows.UIElement.RemoveHandler%2A> [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] acessador e na declaração de acessadorparausarosserviçosdeeventos.`remove`

- Criar um método virtual protegido denominado `OnValueChanged` que aciona o evento `ValueChanged`.

[!code-csharp[UserControlNumericUpDown#RoutedEvent](~/samples/snippets/csharp/VS_Snippets_Wpf/UserControlNumericUpDown/CSharp/NumericUpDown.xaml.cs#routedevent)]
[!code-vb[UserControlNumericUpDown#RoutedEvent](~/samples/snippets/visualbasic/VS_Snippets_Wpf/UserControlNumericUpDown/visualbasic/numericupdown.xaml.vb#routedevent)]

Para obter mais informações, consulte [Visão geral de eventos roteados](../advanced/routed-events-overview.md) e [Criar um evento roteado personalizado](../advanced/how-to-create-a-custom-routed-event.md).

### <a name="use-binding"></a>Usar associação

Para desacoplar a interface do usuário usada pelo controle da respectiva lógica, considere usar vinculação de dados. Isso é particularmente importante se você definir a aparência do seu controle usando um <xref:System.Windows.Controls.ControlTemplate>. Quando você usa vinculação de dados, você pode ser capaz de, diretamente do código, eliminar a necessidade de fazer referência a partes específicas da interface do usuário. É uma boa ideia evitar a referência de elementos que estão <xref:System.Windows.Controls.ControlTemplate> no porque quando o código faz referência a elementos que estão <xref:System.Windows.Controls.ControlTemplate> no e o <xref:System.Windows.Controls.ControlTemplate> é alterado, o elemento referenciado precisa ser incluído no novo. <xref:System.Windows.Controls.ControlTemplate>

O exemplo a seguir atualiza <xref:System.Windows.Controls.TextBlock> o `NumericUpDown` do controle, atribuindo um nome a ele e referenciando a TextBox pelo nome no código.

[!code-xaml[UserControlNumericUpDownSimple#UIRefMarkup](~/samples/snippets/csharp/VS_Snippets_Wpf/UserControlNumericUpDownSimple/CSharp/NumericUpDown.xaml#uirefmarkup)]

[!code-csharp[UserControlNumericUpDownSimple#UIRefCode](~/samples/snippets/csharp/VS_Snippets_Wpf/UserControlNumericUpDownSimple/CSharp/NumericUpDown.xaml.cs#uirefcode)]
[!code-vb[UserControlNumericUpDownSimple#UIRefCode](~/samples/snippets/visualbasic/VS_Snippets_Wpf/UserControlNumericUpDownSimple/VisualBasic/NumericUpDown.xaml.vb#uirefcode)]

O exemplo a seguir usa a associação para realizar a mesma coisa.

[!code-xaml[UserControlNumericUpDown#Binding](~/samples/snippets/csharp/VS_Snippets_Wpf/UserControlNumericUpDown/CSharp/NumericUpDown.xaml#binding)]

Para obter mais informações sobre vinculação de dados, veja [Noções básicas de vinculação de dados](../data/data-binding-overview.md).

### <a name="design-for-designers"></a>Design para designers

Para receber suporte a controles WPF personalizados no [!INCLUDE[wpfdesigner_current_long](../../../../includes/wpfdesigner-current-long-md.md)] (por exemplo, edição de propriedades com a janela Propriedades), siga estas diretrizes.  Para obter mais informações sobre como desenvolver [!INCLUDE[wpfdesigner_current_short](../../../../includes/wpfdesigner-current-short-md.md)]para o, consulte [criar XAML no Visual Studio](/visualstudio/designers/designing-xaml-in-visual-studio).

#### <a name="dependency-properties"></a>Propriedades de Dependência

Certifique-se de implementar `get` CLR `set` e acessadores, conforme descrito anteriormente, em "usar propriedades de dependência". Designers podem usar o wrapper para detectar a presença de uma propriedade de dependência mas eles, assim como o [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] e os clientes do controle, não precisam chamar os acessadores ao obter ou definir a propriedade.

#### <a name="attached-properties"></a>Propriedades anexadas

Você deve implementar propriedades anexadas nos controles personalizados usando as diretrizes a seguir:

- Ter um `public` `static` dosformulários`Property` *PropertyName* que estava criando usando o método.<xref:System.Windows.DependencyProperty.RegisterAttached%2A> `readonly` <xref:System.Windows.DependencyProperty> O nome da propriedade que é passado <xref:System.Windows.DependencyProperty.RegisterAttached%2A> para deve corresponder a *PropertyName*.

- Implemente um par de métodos CLR `public` `static` chamados `Set`*PropertyName* e `Get`*PropertyName*. Ambos os métodos devem aceitar uma classe derivada <xref:System.Windows.DependencyProperty> de como seu primeiro argumento. O método `Set`*PropertyName* também aceita um argumento cujo tipo corresponde ao tipo de dados registrado para a propriedade. O método `Get`*PropertyName* deve retornar um valor do mesmo tipo. Se o método `Set`*PropertyName* estiver ausente, a propriedade será somente leitura.

- `Set`*PropertyName* e `Get` *PropertyName* devem ser roteados diretamente <xref:System.Windows.DependencyObject.GetValue%2A> para <xref:System.Windows.DependencyObject.SetValue%2A> os métodos e no objeto de dependência de destino, respectivamente. Designers podem acessar a propriedade anexada chamando-a por meio dos métodos wrapper ou fazendo uma chamada direta para o objeto de dependência de destino.

Para obter mais informações sobre as propriedades anexadas, consulte [Visão geral das propriedades anexadas](../advanced/attached-properties-overview.md).

### <a name="define-and-use-shared-resources"></a>Definir e usar recursos compartilhados

Você pode incluir seu controle no mesmo assembly que seu aplicativo ou então você pode empacotar seu controle em um assembly separado, que pode ser usado em vários aplicativos. Geralmente, as informações discutidas neste tópico são aplicáveis independentemente do método que você usar.  No entanto, há uma diferença importante a observar.  Quando você coloca um controle no mesmo assembly que um aplicativo, você fica livre para adicionar recursos globais ao arquivo App.xaml. Mas um assembly que contém apenas controles não tem um <xref:System.Windows.Application> objeto associado a ele, portanto, um arquivo app. XAML não está disponível.

Quando um aplicativo procura um recurso, ele procura em três níveis na seguinte ordem:

1. O nível de elemento.

   O sistema começa com o elemento que referencia o recurso e pesquisa por recursos do pai lógico e assim por diante até o elemento raiz ser alcançado.

2. O nível de aplicativo.

   Recursos definidos pelo <xref:System.Windows.Application> objeto.

3. O nível de tema.

   Dicionários no nível de tema são armazenados em uma subpasta denominada Themes.  Os arquivos na pasta Themes correspondem aos temas.  Por exemplo, você pode ter Aero.NormalColor.xaml, Luna.NormalColor.xaml, Royale.NormalColor.xaml e assim por diante.  Você também pode ter um arquivo chamado generic.xaml.  Quando o sistema procura por um recurso no nível de temas, ele primeiro procura por esse recurso no arquivo específico do tema e, em seguida, procura por ele em generic.xaml.

Quando o controle está em um assembly separado do aplicativo, você deve colocar os recursos globais no nível de elemento ou no nível de tema. Os dois métodos têm suas vantagens.

#### <a name="defining-resources-at-the-element-level"></a>Definir recursos no nível de elemento

Você pode definir recursos compartilhados no nível de elemento criando um dicionário de recursos personalizados e mesclando-o com o dicionário de recursos do controle.  Ao usar esse método, você pode nomear o arquivo de recurso da maneira que quiser, sendo que tais arquivos podem estar na mesma pasta que os controles. Recursos no nível de elemento também podem usar cadeias de caracteres simples como chaves. O exemplo a seguir cria <xref:System.Windows.Media.LinearGradientBrush> um arquivo de recurso chamado Dictionary1. XAML.

[!code-xaml[SharedResources#1](~/samples/snippets/csharp/VS_Snippets_Wpf/SharedResources/CS/Dictionary1.xaml#1)]

Depois de ter definido seu dicionário, você precisa mesclá-lo com o dicionário de recursos do controle.  Você pode fazer isso usando [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ou código.

O exemplo a seguir mescla um dicionário de recursos usando [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)].

[!code-xaml[SharedResources#2](~/samples/snippets/csharp/VS_Snippets_Wpf/SharedResources/CS/ShapeResizer.xaml#2)]

A desvantagem dessa abordagem é que um <xref:System.Windows.ResourceDictionary> objeto é criado cada vez que você faz referência a ele.  Por exemplo, se você tiver 10 controles personalizados em sua biblioteca e mesclar os dicionários de recursos compartilhados para cada controle usando XAML, você criará 10 objetos idênticos <xref:System.Windows.ResourceDictionary> .  Você pode evitar isso criando uma classe estática que mescla os recursos no código e retorna o resultado <xref:System.Windows.ResourceDictionary>.

O exemplo a seguir cria uma classe que retorna um <xref:System.Windows.ResourceDictionary>Shared.

[!code-csharp[SharedResources#3](~/samples/snippets/csharp/VS_Snippets_Wpf/SharedResources/CS/SharedDictionaryManager.cs#3)]

O exemplo a seguir mescla o recurso compartilhado com os recursos de um controle personalizado no construtor do controle antes de chamar `InitializeComponent`.  Como o `SharedDictionaryManager.SharedDictionary` é uma propriedade estática, o <xref:System.Windows.ResourceDictionary> é criado apenas uma vez. Já que o dicionário de recursos foi mesclado antes de `InitializeComponent` ser chamado, os recursos estão disponíveis para o controle no seu arquivo [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)].

[!code-csharp[SharedResources#4](~/samples/snippets/csharp/VS_Snippets_Wpf/SharedResources/CS/ShapeResizer.xaml.cs#4)]

#### <a name="defining-resources-at-the-theme-level"></a>Definir recursos no nível de tema

[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] permite que você crie recursos para diferentes temas do Windows.  Como um autor de controle, você pode definir um recurso para um tema específico para alterar a aparência do controle dependendo de qual tema está em uso. Por exemplo, a aparência de um <xref:System.Windows.Controls.Button> no tema clássico do Windows (o tema padrão do Windows 2000) difere de um <xref:System.Windows.Controls.Button> no tema do Windows Luna (o tema padrão do Windows XP) <xref:System.Windows.Controls.ControlTemplate> porque o <xref:System.Windows.Controls.Button> usa um para cada tema.

Recursos específicos de um tema são mantidos em um dicionário de recursos com um nome de arquivo específico. Esses arquivos devem estar em uma pasta chamada `Themes`, que é uma subpasta da pasta que contém o controle. A tabela a seguir lista os arquivos de dicionário de recursos e o tema que está associado a cada arquivo:

|Nome do arquivo do dicionário de recursos|Tema do Windows|
|-----------------------------------|-------------------|
|`Classic.xaml`|Aparência do Tema Clássico do Windows 9x/2000 no Windows XP|
|`Luna.NormalColor.xaml`|Tema azul padrão no Windows XP|
|`Luna.Homestead.xaml`|Tema verde-oliva no Windows XP|
|`Luna.Metallic.xaml`|Tema prateado no Windows XP|
|`Royale.NormalColor.xaml`|Tema padrão no Windows XP Media Center Edition|
|`Aero.NormalColor.xaml`|Tema padrão no Windows Vista|

Você não precisa definir um recurso para cada tema. Se um recurso não está definido para um tema específico, o controle verifica `Classic.xaml` em busca do recurso. Se o recurso não está definido no arquivo que corresponde ao tema atual nem em `Classic.xaml`, o controle usa o recurso genérico, que está em um arquivo de dicionário de recursos chamado `generic.xaml`.  O arquivo `generic.xaml` está localizado na mesma pasta que os arquivos de dicionário de recursos específicos do tema. Embora `generic.xaml` não corresponda a um tema específico do Windows, ele ainda é um dicionário de nível de tema.

O [C#](https://github.com/dotnet/samples/tree/master/snippets/csharp/VS_Snippets_Wpf/CustomControlNumericUpDown/CSharp) exemplo ou [Visual Basic](https://github.com/dotnet/samples/tree/master/snippets/visualbasic/VS_Snippets_Wpf/CustomControlNumericUpDown/visualbasic) controle personalizado NumericUpDown com suporte de automação de interface de usuário e tema contém dois `NumericUpDown` dicionários de recurso para o controle: um está em Generic. XAML e o outro é em Luna. NormalColor. XAML. 

Ao colocar um em <xref:System.Windows.Controls.ControlTemplate> qualquer um dos arquivos de dicionário de recursos específicos do tema, você deve criar um construtor estático para seu controle e chamar <xref:System.Windows.DependencyProperty.OverrideMetadata%28System.Type%2CSystem.Windows.PropertyMetadata%29> o método no <xref:System.Windows.FrameworkElement.DefaultStyleKey%2A>, conforme mostrado no exemplo a seguir.

[!code-csharp[CustomControlNumericUpDownOneProject#StaticConstructor](~/samples/snippets/csharp/VS_Snippets_Wpf/CustomControlNumericUpDownOneProject/CSharp/NumericUpDown.cs#staticconstructor)]
[!code-vb[CustomControlNumericUpDownOneProject#StaticConstructor](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CustomControlNumericUpDownOneProject/visualbasic/numericupdown.vb#staticconstructor)]

##### <a name="defining-and-referencing-keys-for-theme-resources"></a>Definir e referenciar chaves para recursos de tema

Quando você definir um recurso no nível de elemento, você poderá atribuir uma cadeia de caracteres como sua chave e acessar o recurso pela cadeia de caracteres. Ao definir um recurso no nível do tema, você deve usar um <xref:System.Windows.ComponentResourceKey> como chave.  O exemplo a seguir define um recurso em generic.xaml.

[!code-xaml[ThemeResourcesControlLibrary#5](~/samples/snippets/csharp/VS_Snippets_Wpf/ThemeResourcesControlLibrary/CS/Themes/generic.xaml#5)]

O exemplo a seguir faz referência ao recurso especificando <xref:System.Windows.ComponentResourceKey> o como a chave.

[!code-xaml[ThemeResourcesControlLibrary#6](~/samples/snippets/csharp/VS_Snippets_Wpf/ThemeResourcesControlLibrary/CS/NumericUpDown.xaml#6)]

##### <a name="specifying-the-location-of-theme-resources"></a>Especificando o local dos recursos de tema

Para localizar os recursos de um controle, o aplicativo host precisa ter certeza de que o assembly contém recursos específicos do controle. Você pode fazer isso adicionando o <xref:System.Windows.ThemeInfoAttribute> ao assembly que contém o controle. O <xref:System.Windows.ThemeInfoAttribute> tem uma <xref:System.Windows.ThemeInfoAttribute.GenericDictionaryLocation%2A> propriedade que especifica o local dos recursos genéricos e uma <xref:System.Windows.ThemeInfoAttribute.ThemeDictionaryLocation%2A> propriedade que especifica o local dos recursos específicos do tema.

O exemplo a seguir define <xref:System.Windows.ThemeInfoAttribute.GenericDictionaryLocation%2A> as <xref:System.Windows.ThemeInfoAttribute.ThemeDictionaryLocation%2A> Propriedades e <xref:System.Windows.ResourceDictionaryLocation.SourceAssembly>como, para especificar que os recursos genéricos e específicos do tema estejam no mesmo assembly que o controle.

[!code-csharp[CustomControlNumericUpDown#ThemesSection](~/samples/snippets/csharp/VS_Snippets_Wpf/CustomControlNumericUpDown/CSharp/CustomControlLibrary/Properties/AssemblyInfo.cs#themessection)]
[!code-vb[CustomControlNumericUpDown#ThemesSection](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CustomControlNumericUpDown/visualbasic/customcontrollibrary/my project/assemblyinfo.vb#themessection)]

## <a name="see-also"></a>Consulte também

- [Criar o XAML no Visual Studio](/visualstudio/designers/designing-xaml-in-visual-studio)
- [URIs "pack://" no WPF](../app-development/pack-uris-in-wpf.md)
- [Personalização do controle](control-customization.md)
