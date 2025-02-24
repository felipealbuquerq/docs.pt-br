---
title: Visão geral da entrada
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- commands [WPF]
- input [WPF], overview
- keyboard focus [WPF]
- keyboard input [WPF]
- touch events [WPF]
- event routing [WPF]
- touch input [WPF]
- manipulation [WPF]
- logical focus [WPF]
- stylus input [WPF]
- text input [WPF]
- input events [WPF], handling
- WPF [WPF], input overview
- manipulation events [WPF]
- mouse input [WPF]
- mouse capture [WPF]
- focus [WPF]
- mouse position [WPF]
ms.assetid: ee5258b7-6567-415a-9b1c-c0cbe46e79ef
ms.openlocfilehash: 5eaf83f259abe4ee574dfd4d2269dfa1e9373c94
ms.sourcegitcommit: 9ee6cd851b6e176a5811ea28ed0d5935c71950f9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68869070"
---
# <a name="input-overview"></a>Visão geral da entrada
<a name="introduction"></a>O [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] subsistema fornece uma API poderosa para obter a entrada de uma variedade de dispositivos, incluindo o mouse, o teclado, o toque e a caneta. Este tópico descreve os serviços fornecidos pelo [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] e explica a arquitetura dos sistemas de entrada.

<a name="input_api"></a>
## <a name="input-api"></a>API de entrada
 A exposição da API de entrada primária é encontrada nas classes do elemento <xref:System.Windows.UIElement>base <xref:System.Windows.ContentElement>: <xref:System.Windows.FrameworkElement>,, <xref:System.Windows.FrameworkContentElement>e.  Para obter mais informações sobre os elementos base, consulte [Visão geral de elementos base](base-elements-overview.md).  Essas classes fornecem funcionalidade para eventos de entrada relacionados a pressionamentos de tecla, botões do mouse, roda do mouse, movimento do mouse, gerenciamento de foco e captura do mouse, para citar alguns. Colocando a API de entrada nos elementos base, em vez de tratar todos os eventos de entrada como um serviço, a arquitetura de entrada permite que os eventos de entrada sejam originados por um objeto específico na interface do usuário e ofereçam suporte a um esquema de roteamento de eventos, no qual mais de um elemento tem um opp ortunity para manipular um evento de entrada. Muitos eventos de entrada têm um par de eventos associados a eles.  Por exemplo, o evento Key down é associado aos <xref:System.Windows.Input.Keyboard.KeyDown> eventos e. <xref:System.Windows.Input.Keyboard.PreviewKeyDown>  A diferença nesses eventos está em como eles são roteados para o elemento de destino.  Eventos de visualização são roteados por túnel pela árvore de elementos, do elemento raiz ao elemento de destino.  Eventos por propagação propagam-se para cima, do elemento de destino até o elemento raiz.  O roteamento de eventos no [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] é discutido em mais detalhes posteriormente nesta visão geral e na [Visão geral de eventos roteados](routed-events-overview.md).

### <a name="keyboard-and-mouse-classes"></a>Classes de mouse e teclado
 Além da API de entrada nas classes de elemento base, a classe <xref:System.Windows.Input.Keyboard> e <xref:System.Windows.Input.Mouse> as classes fornecem API adicional para trabalhar com entrada de teclado e mouse.

 Exemplos de API de entrada na <xref:System.Windows.Input.Keyboard> classe são a <xref:System.Windows.Input.Keyboard.Modifiers%2A> Propriedade, que retorna o <xref:System.Windows.Input.ModifierKeys> botão atualmente pressionado e o <xref:System.Windows.Input.Keyboard.IsKeyDown%2A> método, que determina se uma chave especificada é pressionada.

 O exemplo a seguir usa <xref:System.Windows.Input.Keyboard.GetKeyStates%2A> o método para determinar se <xref:System.Windows.Input.Key> um está no estado inoperante.

 [!code-csharp[keyargssnippetsample#KeyEventArgsKeyBoardGetKeyStates](~/samples/snippets/csharp/VS_Snippets_Wpf/KeyArgsSnippetSample/CSharp/Window1.xaml.cs#keyeventargskeyboardgetkeystates)]
 [!code-vb[keyargssnippetsample#KeyEventArgsKeyBoardGetKeyStates](~/samples/snippets/visualbasic/VS_Snippets_Wpf/KeyArgsSnippetSample/visualbasic/window1.xaml.vb#keyeventargskeyboardgetkeystates)]

 Exemplos de API de entrada na <xref:System.Windows.Input.Mouse> classe são <xref:System.Windows.Input.Mouse.MiddleButton%2A>, que obtém o estado do botão do meio do mouse e <xref:System.Windows.Input.Mouse.DirectlyOver%2A>, que obtém o elemento no qual o ponteiro do mouse está no momento.

 O exemplo a seguir determina se <xref:System.Windows.Input.Mouse.LeftButton%2A> o no mouse está <xref:System.Windows.Input.MouseButtonState.Pressed> no estado.

 [!code-csharp[mouserelatedsnippets#MouseRelatedSnippetsGetLeftButtonMouse](~/samples/snippets/csharp/VS_Snippets_Wpf/MouseRelatedSnippets/CSharp/Window1.xaml.cs#mouserelatedsnippetsgetleftbuttonmouse)]
 [!code-vb[mouserelatedsnippets#MouseRelatedSnippetsGetLeftButtonMouse](~/samples/snippets/visualbasic/VS_Snippets_Wpf/MouseRelatedSnippets/visualbasic/window1.xaml.vb#mouserelatedsnippetsgetleftbuttonmouse)]

 As <xref:System.Windows.Input.Mouse> classes <xref:System.Windows.Input.Keyboard> e são abordadas em mais detalhes em toda essa visão geral.

### <a name="stylus-input"></a>Entrada de caneta
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]tem suporte integrado para o <xref:System.Windows.Input.Stylus>.  O <xref:System.Windows.Input.Stylus> é uma entrada de caneta que se tornou [!INCLUDE[TLA#tla_tpc](../../../../includes/tlasharptla-tpc-md.md)]popular pelo.  [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]os aplicativos podem tratar a caneta como um mouse usando a API do mouse, mas [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] também expõe uma abstração de dispositivo de caneta que usa um modelo semelhante ao teclado e ao mouse.  Todas as APIs relacionadas à caneta contêm a palavra "Stylus".

 Já que a caneta pode atuar como um mouse, aplicativos que dão suporte apenas à entrada de mouse ainda podem obter algum nível de suporte à caneta automaticamente. Quando a caneta é usada desse modo, o aplicativo tem a oportunidade de manipular o evento de caneta apropriado e, em seguida, manipular o evento de mouse correspondente. Além disso, os serviços de alto nível, por exemplo, a entrada de tinta, também estão disponíveis por meio da abstração do dispositivo de caneta.  Para obter mais informações sobre a tinta como entrada, consulte [Introdução à tinta](getting-started-with-ink.md).

<a name="event_routing"></a>
## <a name="event-routing"></a>Roteamento de eventos
 Um <xref:System.Windows.FrameworkElement> pode conter outros elementos como elementos filho em seu modelo de conteúdo, formando uma árvore de elementos.  Em [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)], o elemento pai pode participar de uma entrada direcionada a seus elementos filho ou outros descendentes por meio da manipulação de eventos. Isso é especialmente útil para a criação de controles com base em controles menores, um processo conhecido como "composição de controle" ou "composição". Para obter mais informações sobre árvores de elementos e como árvores de elementos se relacionam a rotas de evento, consulte [Árvores no WPF](trees-in-wpf.md).

 Roteamento de eventos é o processo de encaminhamento de eventos para vários elementos para que um determinado objeto ou elemento ao longo da rota possa optar por oferecer uma resposta significativa (por meio de manipulação) a um evento que possa ter sido originado de um elemento diferente.  Eventos roteados usam um de três mecanismos de roteamento: direto, por propagação e por túnel.  No roteamento direto, o elemento de origem é o único elemento notificado e o evento não será roteado para nenhum outro elemento. No entanto, o evento roteado direto ainda oferece alguns recursos adicionais que estão presentes apenas para eventos roteados em vez de eventos CLR padrão. O roteamento por propagação ocorre de baixo para cima na árvore de elementos, notificando primeiro o elemento que originou o evento, depois o elemento pai e assim por diante.  O roteamento por túnel começa na raiz da árvore de elementos prossegue de cima para baixo, terminando com o elemento de origem original.  Para obter mais informações sobre eventos roteados, consulte [Visão geral de eventos roteados](routed-events-overview.md).

 Eventos [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] de entrada geralmente vêm em pares que consistem em um evento por túnel e um evento por propagação.  Eventos por túnel são diferenciados dos eventos por propagação com o prefixo "Preview".  Por exemplo, <xref:System.Windows.Input.Mouse.PreviewMouseMove> é a versão de túnel de um evento de movimentação do <xref:System.Windows.Input.Mouse.MouseMove> mouse e é a versão de bolha desse evento. Esse emparelhamento de eventos é uma convenção que é implementada no nível de elemento e não é uma funcionalidade inerente ao sistema de eventos [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]. Para obter detalhes, consulte a seção Eventos WPF de entrada em [Visão geral de eventos roteados](routed-events-overview.md).

<a name="handling_input_events"></a>
## <a name="handling-input-events"></a>Manipulação de eventos de entrada
 Para receber a entrada em um elemento, um manipulador de eventos deve estar associado a esse evento específico.  Em [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] isso é fácil: você referencia o nome do evento como um atributo do elemento que escutará esse evento.  Em seguida, você deverá definir o valor do atributo para o nome do manipulador de eventos que você definir, com base em um delegado.  O manipulador de eventos deve ser escrito em código como C# e pode ser incluído em um arquivo code-behind.

 Eventos de teclado ocorrem quando o sistema operacional relata ações de tecla que ocorrem enquanto o foco do teclado está em um elemento. Tanto eventos de mouse quanto de caneta podem ser divididos em duas categorias: eventos relatam alterações na posição do ponteiro com relação ao elemento e eventos que relatam alterações no estado dos botões do dispositivo.

### <a name="keyboard-input-event-example"></a>Exemplo de evento de entrada do teclado
 O exemplo a seguir escuta um pressionamento de tecla de seta para a esquerda.  Um <xref:System.Windows.Controls.StackPanel> é criado com um <xref:System.Windows.Controls.Button>.  Um manipulador de eventos para escutar o pressionamento da tecla de seta para a <xref:System.Windows.Controls.Button> esquerda é anexado à instância.

 A primeira seção do exemplo cria o <xref:System.Windows.Controls.StackPanel> e o <xref:System.Windows.Controls.Button> e anexa o manipulador de eventos para o <xref:System.Windows.UIElement.KeyDown>.

 [!code-xaml[InputOvw#Input_OvwKeyboardExampleXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/InputOvw/CSharp/Page1.xaml#input_ovwkeyboardexamplexaml)]

 [!code-csharp[InputOvw#Input_OvwKeyboardExampleUICodeBehind](~/samples/snippets/csharp/VS_Snippets_Wpf/InputOvw/CSharp/Page1.xaml.cs#input_ovwkeyboardexampleuicodebehind)]
 [!code-vb[InputOvw#Input_OvwKeyboardExampleUICodeBehind](~/samples/snippets/visualbasic/VS_Snippets_Wpf/InputOvw/VisualBasic/Page1.xaml.vb#input_ovwkeyboardexampleuicodebehind)]

 A segunda seção é escrita em código e define o manipulador de eventos.  Quando a tecla de seta para a esquerda é <xref:System.Windows.Controls.Button> pressionada e o foco do teclado, o <xref:System.Windows.Controls.Control.Background%2A> manipulador é executado <xref:System.Windows.Controls.Button> e a cor do é alterada.  Se a tecla for pressionada, mas não for a tecla de seta para a <xref:System.Windows.Controls.Control.Background%2A> esquerda, a <xref:System.Windows.Controls.Button> cor do será alterada de volta para a cor inicial.

 [!code-csharp[InputOvw#Input_OvwKeyboardExampleHandlerCodeBehind](~/samples/snippets/csharp/VS_Snippets_Wpf/InputOvw/CSharp/Page1.xaml.cs#input_ovwkeyboardexamplehandlercodebehind)]
 [!code-vb[InputOvw#Input_OvwKeyboardExampleHandlerCodeBehind](~/samples/snippets/visualbasic/VS_Snippets_Wpf/InputOvw/VisualBasic/Page1.xaml.vb#input_ovwkeyboardexamplehandlercodebehind)]

### <a name="mouse-input-event-example"></a>Exemplo de evento de entrada do mouse
 No exemplo a seguir, a <xref:System.Windows.Controls.Control.Background%2A> cor de um <xref:System.Windows.Controls.Button> é alterada quando o ponteiro do mouse entra <xref:System.Windows.Controls.Button>no.  A <xref:System.Windows.Controls.Control.Background%2A> cor é restaurada quando o mouse sai <xref:System.Windows.Controls.Button>do.

 A primeira seção <xref:System.Windows.Controls.StackPanel> do exemplo cria o e o <xref:System.Windows.Controls.Button> controle e anexa os manipuladores de <xref:System.Windows.UIElement.MouseEnter> eventos aos eventos e <xref:System.Windows.UIElement.MouseLeave> ao <xref:System.Windows.Controls.Button>.

 [!code-xaml[InputOvw#Input_OvwMouseExampleXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/InputOvw/CSharp/Page1.xaml#input_ovwmouseexamplexaml)]

 [!code-csharp[InputOvw#Input_OvwMouseExampleUICodeBehind](~/samples/snippets/csharp/VS_Snippets_Wpf/InputOvw/CSharp/Page1.xaml.cs#input_ovwmouseexampleuicodebehind)]
 [!code-vb[InputOvw#Input_OvwMouseExampleUICodeBehind](~/samples/snippets/visualbasic/VS_Snippets_Wpf/InputOvw/VisualBasic/Page1.xaml.vb#input_ovwmouseexampleuicodebehind)]

 A segunda seção do exemplo é escrita em código e define os manipuladores de eventos.  Quando o mouse entra no <xref:System.Windows.Controls.Button>, a <xref:System.Windows.Controls.Control.Background%2A> cor do <xref:System.Windows.Controls.Button> é alterada para <xref:System.Windows.Media.Brushes.SlateGray%2A>.  Quando o mouse sai do <xref:System.Windows.Controls.Button>, a <xref:System.Windows.Controls.Control.Background%2A> cor do <xref:System.Windows.Controls.Button> é alterada de volta para <xref:System.Windows.Media.Brushes.AliceBlue%2A>.

 [!code-csharp[InputOvw#Input_OvwMouseExampleEneterHandler](~/samples/snippets/csharp/VS_Snippets_Wpf/InputOvw/CSharp/Page1.xaml.cs#input_ovwmouseexampleeneterhandler)]
 [!code-vb[InputOvw#Input_OvwMouseExampleEneterHandler](~/samples/snippets/visualbasic/VS_Snippets_Wpf/InputOvw/VisualBasic/Page1.xaml.vb#input_ovwmouseexampleeneterhandler)]

 [!code-csharp[InputOvw#Input_OvwMouseExampleLeaveHandler](~/samples/snippets/csharp/VS_Snippets_Wpf/InputOvw/CSharp/Page1.xaml.cs#input_ovwmouseexampleleavehandler)]
 [!code-vb[InputOvw#Input_OvwMouseExampleLeaveHandler](~/samples/snippets/visualbasic/VS_Snippets_Wpf/InputOvw/VisualBasic/Page1.xaml.vb#input_ovwmouseexampleleavehandler)]

<a name="text_input"></a>
## <a name="text-input"></a>Entrada de Texto
 O <xref:System.Windows.ContentElement.TextInput> evento permite que você ouça a entrada de texto de maneira independente de dispositivo. O teclado é o método primário de entrada de texto; no entanto, fala, manuscrito e outros dispositivos de entrada também podem gerar entrada de texto.

 Para entrada de teclado [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] , o primeiro envia <xref:System.Windows.ContentElement.KeyDown> os eventos apropriados /. <xref:System.Windows.ContentElement.KeyUp> Se esses eventos não forem manipulados e a chave for textual (em vez de uma chave de controle, como setas direcionais ou teclas de <xref:System.Windows.ContentElement.TextInput> função), um evento será gerado.  Nem sempre há um mapeamento um-para <xref:System.Windows.ContentElement.KeyDown> -um simples entre / <xref:System.Windows.ContentElement.KeyUp> os eventos <xref:System.Windows.ContentElement.TextInput> e porque vários pressionamentos de teclas podem gerar um único caractere de entrada de texto e pressionamentos de teclas únicos podem gerar vários caracteres seqüências.  Isso é especialmente verdadeiro para linguagens como chinês, japonês e coreano, que usam IMEs (Input Method Editors) para gerar os milhares de caracteres possíveis em seus alfabetos correspondentes.

 Quando [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] o envia <xref:System.Windows.ContentElement.KeyUp> um / evento,<xref:System.Windows.Input.KeyEventArgs.Key%2A> é definido como<xref:System.Windows.Input.Key.System?displayProperty=nameWithType> se os pressionamentos de teclas pudessem se tornar <xref:System.Windows.ContentElement.TextInput> parte de um evento (se ALT + S for pressionado, por exemplo). <xref:System.Windows.ContentElement.KeyDown> Isso permite que o código <xref:System.Windows.ContentElement.KeyDown> em um manipulador de eventos <xref:System.Windows.Input.Key.System?displayProperty=nameWithType> Verifique e, se encontrado, deixe o processamento para o manipulador do evento <xref:System.Windows.ContentElement.TextInput> gerado posteriormente. Nesses casos, as várias propriedades do <xref:System.Windows.Input.TextCompositionEventArgs> argumento podem ser usadas para determinar os pressionamentos de tecla originais. Da mesma forma, se um IME estiver <xref:System.Windows.Input.Key> ativo, o terá <xref:System.Windows.Input.Key.ImeProcessed?displayProperty=nameWithType>o valor <xref:System.Windows.Input.KeyEventArgs.ImeProcessedKey%2A> de e dará o pressionamento de tecla ou teclas originais.

 O exemplo a seguir define um manipulador para <xref:System.Windows.Controls.Primitives.ButtonBase.Click> o evento e um manipulador para <xref:System.Windows.UIElement.KeyDown> o evento.

 O primeiro segmento de código ou marcação cria a interface do usuário.

 [!code-xaml[InputOvw#Input_OvwTextInputXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/InputOvw/CSharp/Page1.xaml#input_ovwtextinputxaml)]

 [!code-csharp[InputOvw#Input_OvwTextInputUICodeBehind](~/samples/snippets/csharp/VS_Snippets_Wpf/InputOvw/CSharp/Page1.xaml.cs#input_ovwtextinputuicodebehind)]
 [!code-vb[InputOvw#Input_OvwTextInputUICodeBehind](~/samples/snippets/visualbasic/VS_Snippets_Wpf/InputOvw/VisualBasic/Page1.xaml.vb#input_ovwtextinputuicodebehind)]

 O segundo segmento de código contém os manipuladores de eventos.

 [!code-csharp[InputOvw#Input_OvwTextInputHandlersCodeBehind](~/samples/snippets/csharp/VS_Snippets_Wpf/InputOvw/CSharp/Page1.xaml.cs#input_ovwtextinputhandlerscodebehind)]
 [!code-vb[InputOvw#Input_OvwTextInputHandlersCodeBehind](~/samples/snippets/visualbasic/VS_Snippets_Wpf/InputOvw/VisualBasic/Page1.xaml.vb#input_ovwtextinputhandlerscodebehind)]

 Como os eventos de entrada emergim a rota de <xref:System.Windows.Controls.StackPanel> eventos, o recebe a entrada, independentemente de qual elemento tem o foco do teclado. O <xref:System.Windows.Controls.TextBox> controle é notificado primeiro e `OnTextInputKeyDown` o manipulador será chamado somente se <xref:System.Windows.Controls.TextBox> o não tiver manipulado a entrada. Se o <xref:System.Windows.UIElement.PreviewKeyDown> evento for usado em vez <xref:System.Windows.UIElement.KeyDown> do evento, o `OnTextInputKeyDown` manipulador será chamado primeiro.

 Neste exemplo, a lógica de tratamento é gravada duas vezes – uma vez para CTRL + O e novamente para o evento de clique do botão. Isso pode ser simplificado usando comandos em vez de manipular os eventos de entrada diretamente.  Comandos são discutidos nesta visão geral e na [Visão geral dos comandos](commanding-overview.md).

<a name="touch_and_manipulation"></a>
## <a name="touch-and-manipulation"></a>Toque e manipulação
 Novo hardware e a API no sistema operacional Windows 7 fornecem a aplicativos a capacidade de receber entradas de vários toques simultaneamente. [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] permite que os aplicativos detectem e respondam ao toque de maneira semelhante à que respondem a outras entradas como o mouse ou teclado, acionando eventos quando o toque ocorre.

 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] expõe dois tipos de eventos quando o toque ocorre: eventos de toque e eventos de manipulação. Eventos de toque fornecem dados brutos sobre cada dedo e seu movimento em uma tela touch. Eventos de manipulação interpretam a entrada como determinadas ações. Ambos os tipos de eventos são discutidos nesta seção.

### <a name="prerequisites"></a>Pré-requisitos
 Você precisará dos componentes a seguir para desenvolver um aplicativo que responde ao toque.

- Visual Studio 2010.

- Windows 7.

- Um dispositivo, como uma tela touch, que dê suporte a Windows Touch.

### <a name="terminology"></a>Terminologia
 Os termos a seguir são usados quando o toque é discutido.

- **Toque** é um tipo de entrada do usuário que é reconhecida pelo Windows 7. Geralmente o toque é iniciado colocando-se os dedos em uma tela sensível ao toque. Observe que dispositivos como um touchpad, comuns em computadores laptop, não dão suporte ao toque se o dispositivo simplesmente converte a posição e movimentação do dedo como entrada de mouse.

- **Multitoque** é toque que ocorre em mais de um ponto, simultaneamente. O Windows 7 e o [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] dão suporte a multitoque. Sempre que o toque é discutido na documentação do [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)], os conceitos se aplicam para multitoque.

- Uma **manipulação** ocorre quando o toque é interpretado como uma ação física que é aplicada a um objeto. Em [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)], eventos de manipulação interpretam a entrada como uma manipulação de translação, expansão ou rotação.

- Um `touch device` representa um dispositivo que produz a entrada de toque, por exemplo, um único dedo em uma tela touch.

### <a name="controls-that-respond-to-touch"></a>Controles que respondem ao toque
 É possível rolar pelos controles a seguir arrastando um dedo pelo controle, caso ele tenha conteúdo que está fora da exibição.

- <xref:System.Windows.Controls.ComboBox>

- <xref:System.Windows.Controls.ContextMenu>

- <xref:System.Windows.Controls.DataGrid>

- <xref:System.Windows.Controls.ListBox>

- <xref:System.Windows.Controls.ListView>

- <xref:System.Windows.Controls.MenuItem>

- <xref:System.Windows.Controls.TextBox>

- <xref:System.Windows.Controls.ToolBar>

- <xref:System.Windows.Controls.TreeView>

 O <xref:System.Windows.Controls.ScrollViewer> define a <xref:System.Windows.Controls.ScrollViewer.PanningMode%2A?displayProperty=nameWithType> Propriedade anexada que permite que você especifique se o movimento panorâmico por toque está habilitado horizontalmente, verticalmente, ambos ou nenhum deles. A <xref:System.Windows.Controls.ScrollViewer.PanningDeceleration%2A?displayProperty=nameWithType> propriedade especifica a rapidez com que a rolagem diminui quando o usuário levanta o dedo da tela touch. A <xref:System.Windows.Controls.ScrollViewer.PanningRatio%2A?displayProperty=nameWithType> Propriedade anexada Especifica a taxa de deslocamento de rolagem para converter deslocamento de manipulação.

### <a name="touch-events"></a>Eventos de Toque
 As classes base, <xref:System.Windows.UIElement>, <xref:System.Windows.UIElement3D>e <xref:System.Windows.ContentElement>, definem eventos que você pode assinar para que seu aplicativo responda ao toque. Eventos de toque são úteis quando seu aplicativo interpreta toque como algo diferente de manipular um objeto. Por exemplo, um aplicativo que permite que um usuário desenhe com um ou mais dedos assinaria eventos de toque.

 Todas as três classes definem os eventos a seguir, que se comportam da mesma forma independentemente da classe que os define.

- <xref:System.Windows.UIElement.TouchDown>

- <xref:System.Windows.UIElement.TouchMove>

- <xref:System.Windows.UIElement.TouchUp>

- <xref:System.Windows.UIElement.TouchEnter>

- <xref:System.Windows.UIElement.TouchLeave>

- <xref:System.Windows.UIElement.PreviewTouchDown>

- <xref:System.Windows.UIElement.PreviewTouchMove>

- <xref:System.Windows.UIElement.PreviewTouchUp>

- <xref:System.Windows.UIElement.GotTouchCapture>

- <xref:System.Windows.UIElement.LostTouchCapture>

 Assim como os eventos de teclado e mouse, os eventos de toque são eventos roteados. Os eventos que começam com `Preview` são eventos por túnel e os eventos que começam com `Touch` são eventos por propagação. Para obter mais informações sobre os eventos roteados, consulte [Visão geral de eventos roteados](routed-events-overview.md). Ao lidar com esses eventos, você pode obter a posição da entrada, em relação a qualquer elemento, chamando o <xref:System.Windows.Input.TouchEventArgs.GetTouchPoint%2A> método ou. <xref:System.Windows.Input.TouchEventArgs.GetIntermediateTouchPoints%2A>

 Para compreender a interação entre os eventos de toque, considere o cenário em que um usuário coloca um dedo em um elemento, move o dedo pelo elemento e, em seguida, levanta o dedo do elemento. A ilustração a seguir mostra a execução dos eventos por propagação (os eventos por túnel são omitidos por questão de simplicidade).

 ![A sequência de eventos de toque.](./media/ndp-touchevents.png "NDP_TouchEvents") Eventos de toque

 A lista a seguir descreve a sequência de eventos na ilustração anterior.

1. O <xref:System.Windows.UIElement.TouchEnter> evento ocorre uma vez quando o usuário coloca um dedo no elemento.

2. O <xref:System.Windows.UIElement.TouchDown> evento ocorre uma vez.

3. O <xref:System.Windows.UIElement.TouchMove> evento ocorre várias vezes enquanto o usuário move o dedo dentro do elemento.

4. O <xref:System.Windows.UIElement.TouchUp> evento ocorre uma vez quando o usuário levanta o dedo do elemento.

5. O <xref:System.Windows.UIElement.TouchLeave> evento ocorre uma vez.

 Quando mais de dois dedos são usados, os eventos ocorrem para cada dedo.

### <a name="manipulation-events"></a>Eventos de Manipulação
 Para casos em que um aplicativo permite que um usuário manipule um objeto, <xref:System.Windows.UIElement> a classe define eventos de manipulação. Diferentemente de eventos de toque que simplesmente relatam a posição do toque, os eventos de manipulação relatam como a entrada pode ser interpretada. Há três tipos de manipulação: rotação, translação e expansão. A lista a seguir descreve como invocar os três tipos de manipulação.

- Coloque um dedo em um objeto e mova o dedo na tela touch para invocar uma manipulação de translação. Geralmente, isso move o objeto.

- Coloque dois dedos em um objeto e mova os dedos mais próximos ou mais distantes um do outro para invocar uma manipulação de expansão. Isso geralmente redimensiona o objeto.

- Coloque dois dedos em um objeto e gire os dedos em torno um do outro para invocar uma manipulação de rotação. Isso geralmente gira o objeto.

 É possível que mais de um tipo de manipulação ocorram simultaneamente.

 Quando você faz com que objetos respondam a manipulações, você pode fazer com que o objeto pareça ter inércia. Isso pode fazer com que seus objetos simulem o mundo físico. Por exemplo, quando você empurra um livro por uma mesa, o livro continua a mover-se depois de você liberá-lo, caso você o empurre com força suficiente. [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] permite simular esse comportamento, acionando eventos de manipulação após os dedos do usuário liberarem o objeto.

 Para obter informações sobre como criar um aplicativo que permite que o usuário mova, redimensione e gire um objeto, [consulte explicação: Criando seu primeiro aplicativo](walkthrough-creating-your-first-touch-application.md)de toque.

 O <xref:System.Windows.UIElement> define os seguintes eventos de manipulação.

- <xref:System.Windows.UIElement.ManipulationStarting>

- <xref:System.Windows.UIElement.ManipulationStarted>

- <xref:System.Windows.UIElement.ManipulationDelta>

- <xref:System.Windows.UIElement.ManipulationInertiaStarting>

- <xref:System.Windows.UIElement.ManipulationCompleted>

- <xref:System.Windows.UIElement.ManipulationBoundaryFeedback>

 Por padrão, um <xref:System.Windows.UIElement> não recebe esses eventos de manipulação. Para receber eventos de manipulação em <xref:System.Windows.UIElement>um, <xref:System.Windows.UIElement.IsManipulationEnabled%2A?displayProperty=nameWithType> defina `true`como.

#### <a name="the-execution-path-of-manipulation-events"></a>O caminho de execução de eventos de manipulação
 Considere um cenário em que um usuário "lança" um objeto. O usuário coloca um dedo no objeto, move o dedo pela tela touch por uma distância curta e, em seguida, levanta o dedo enquanto ele está se movendo. O resultado disso é que o objeto se moverá sob o dedo do usuário e continuará a mover-se depois que o usuário levantar o dedo.

 A ilustração a seguir mostra o caminho de execução de eventos de manipulação e informações importantes sobre cada evento.

 ![A sequência de eventos de manipulação.](./media/ndp-manipulationevents.png "NDP_ManipulationEvents") Eventos de manipulação

 A lista a seguir descreve a sequência de eventos na ilustração anterior.

1. O <xref:System.Windows.UIElement.ManipulationStarting> evento ocorre quando o usuário coloca um dedo no objeto. Entre outras coisas, esse evento permite que você defina a <xref:System.Windows.Input.ManipulationStartingEventArgs.ManipulationContainer%2A> propriedade. Nos eventos subsequentes, a posição da manipulação será relativa ao <xref:System.Windows.Input.ManipulationStartingEventArgs.ManipulationContainer%2A>. Em eventos diferentes de <xref:System.Windows.UIElement.ManipulationStarting>, essa propriedade é somente leitura, portanto, o <xref:System.Windows.UIElement.ManipulationStarting> evento é a única vez que você pode definir essa propriedade.

2. O <xref:System.Windows.UIElement.ManipulationStarted> evento ocorre em seguida. Esse evento relata a origem da manipulação.

3. O <xref:System.Windows.UIElement.ManipulationDelta> evento ocorre várias vezes enquanto os dedos de um usuário se movem em uma tela touch. A <xref:System.Windows.Input.ManipulationDeltaEventArgs.DeltaManipulation%2A> propriedade<xref:System.Windows.Input.ManipulationDeltaEventArgs> da classe informa se a manipulação é interpretada como movimento, expansão ou tradução. É aqui que você realiza a maior parte do trabalho envolvido na manipulação de um objeto.

4. O <xref:System.Windows.UIElement.ManipulationInertiaStarting> evento ocorre quando os dedos do usuário perdem o contato com o objeto. Esse evento permite que você especifique a desaceleração das manipulações durante a inércia. Isso é para que seu objeto possa emular espaços físicos ou atributos diferentes, se você assim escolher. Por exemplo, suponha que seu aplicativo tem dois objetos que representam itens no mundo físico e um deles é mais pesado que o outro. Você pode fazer com que o objeto mais pesado desacelere mais rápido do que o objeto mais leve.

5. O <xref:System.Windows.UIElement.ManipulationDelta> evento ocorre várias vezes enquanto inércia ocorre. Observe que este evento ocorre quando os dedos do usuário se movem pela tela touch e quando [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] simula inércia. Em outras palavras, <xref:System.Windows.UIElement.ManipulationDelta> ocorre antes e depois do <xref:System.Windows.UIElement.ManipulationInertiaStarting> evento. A <xref:System.Windows.Input.ManipulationDeltaEventArgs.IsInertial%2A?displayProperty=nameWithType> propriedade informa se o <xref:System.Windows.UIElement.ManipulationDelta> evento ocorre durante inércia, para que você possa verificar essa propriedade e executar ações diferentes, dependendo de seu valor.

6. O <xref:System.Windows.UIElement.ManipulationCompleted> evento ocorre quando a manipulação e qualquer inércia termina. Ou seja, depois que todos <xref:System.Windows.UIElement.ManipulationDelta> os eventos ocorrerem <xref:System.Windows.UIElement.ManipulationCompleted> , o evento ocorrerá para sinalizar que a manipulação foi concluída.

 O <xref:System.Windows.UIElement> também define o <xref:System.Windows.UIElement.ManipulationBoundaryFeedback> evento. Esse evento ocorre quando o <xref:System.Windows.Input.ManipulationDeltaEventArgs.ReportBoundaryFeedback%2A> método é chamado <xref:System.Windows.UIElement.ManipulationDelta> no evento. O <xref:System.Windows.UIElement.ManipulationBoundaryFeedback> evento permite que aplicativos ou componentes forneçam comentários visuais quando um objeto atinge um limite. Por exemplo, a <xref:System.Windows.Window> classe manipula o <xref:System.Windows.UIElement.ManipulationBoundaryFeedback> evento para fazer com que a janela se mova ligeiramente quando sua borda for encontrada.

 Você pode cancelar a manipulação chamando o método <xref:System.Windows.Input.ManipulationStartingEventArgs.Cancel%2A> nos argumentos do evento em qualquer evento de manipulação, <xref:System.Windows.UIElement.ManipulationBoundaryFeedback> exceto o evento. Quando você chama <xref:System.Windows.Input.ManipulationStartingEventArgs.Cancel%2A>, os eventos de manipulação não são mais gerados e eventos de mouse ocorrem para toque. A tabela a seguir descreve a relação entre a hora em que a manipulação é cancelada e os eventos de mouse que ocorrem.

|O evento no qual Cancel é chamado|Os eventos de mouse que ocorrem para entrada e que já ocorreram|
|----------------------------------------|-----------------------------------------------------------------|
|<xref:System.Windows.UIElement.ManipulationStarting> e <xref:System.Windows.UIElement.ManipulationStarted>|Eventos de pressionamento do mouse.|
|<xref:System.Windows.UIElement.ManipulationDelta>|Eventos de pressionamento do mouse e eventos de movimentação do mouse.|
|<xref:System.Windows.UIElement.ManipulationInertiaStarting> e <xref:System.Windows.UIElement.ManipulationCompleted>|Pressionamento do mouse, movimentação do mouse e liberação do mouse.|

 Observe que se você chamar <xref:System.Windows.Input.ManipulationStartingEventArgs.Cancel%2A> quando a manipulação estiver em inércia, o método retornará `false` e a entrada não gerará eventos de mouse.

### <a name="the-relationship-between-touch-and-manipulation-events"></a>A relação entre eventos de toque e eventos de manipulação
 Um <xref:System.Windows.UIElement> sempre pode receber eventos de toque. Quando a <xref:System.Windows.UIElement.IsManipulationEnabled%2A> propriedade é definida como `true`, uma <xref:System.Windows.UIElement> pode receber eventos de toque e manipulação.  Se o <xref:System.Windows.UIElement.TouchDown> evento não for tratado (ou seja, a <xref:System.Windows.RoutedEventArgs.Handled%2A> propriedade for `false`), a lógica de manipulação capturará o toque para o elemento e gerará os eventos de manipulação. Se a <xref:System.Windows.RoutedEventArgs.Handled%2A> propriedade for definida como `true` no <xref:System.Windows.UIElement.TouchDown> evento, a lógica de manipulação não gerará eventos de manipulação. A ilustração a seguir mostra a relação entre os eventos de toque e os eventos de manipulação.

 ![Relação entre eventos de toque e manipulação](./media/ndp-touchmanipulateevents.png "NDP_TouchManipulateEvents") Eventos de toque e manipulação

 A lista a seguir descreve a relação entre os eventos de toque e de manipulação que é mostrada na ilustração anterior.

- Quando o primeiro dispositivo de toque gera <xref:System.Windows.UIElement.TouchDown> um evento em <xref:System.Windows.UIElement>um, a lógica de manipulação <xref:System.Windows.UIElement.CaptureTouch%2A> chama o método, que <xref:System.Windows.UIElement.GotTouchCapture> gera o evento.

- Quando ocorre, a lógica de manipulação chama o <xref:System.Windows.Input.Manipulation.AddManipulator%2A?displayProperty=nameWithType> método, que gera o <xref:System.Windows.UIElement.ManipulationStarting> evento. <xref:System.Windows.UIElement.GotTouchCapture>

- Quando os <xref:System.Windows.UIElement.TouchMove> eventos ocorrem, a lógica de manipulação gera <xref:System.Windows.UIElement.ManipulationDelta> os eventos que ocorrem antes <xref:System.Windows.UIElement.ManipulationInertiaStarting> do evento.

- Quando o último dispositivo de toque no elemento gera o <xref:System.Windows.UIElement.TouchUp> evento, a lógica de manipulação gera <xref:System.Windows.UIElement.ManipulationInertiaStarting> o evento.

<a name="focus"></a>
## <a name="focus"></a>Foco
 Há dois conceitos principais relativos ao foco em [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]: foco do teclado e foco lógico.

### <a name="keyboard-focus"></a>Foco do teclado
 O foco do teclado refere-se ao elemento que está recebendo entrada do teclado.  Em toda a área de trabalho, pode haver apenas um elemento que tem o foco do teclado.  No [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)], o elemento que tem o foco <xref:System.Windows.IInputElement.IsKeyboardFocused%2A> do teclado terá definido `true`como.  O método <xref:System.Windows.Input.Keyboard> <xref:System.Windows.Input.Keyboard.FocusedElement%2A> estático retorna o elemento que atualmente tem o foco do teclado.

 O foco do teclado pode ser obtido por tabulação para um elemento ou clicando com o mouse em determinados elementos, como <xref:System.Windows.Controls.TextBox>um.  O foco do teclado também pode ser obtido programaticamente usando <xref:System.Windows.Input.Keyboard.Focus%2A> o <xref:System.Windows.Input.Keyboard> método na classe.  <xref:System.Windows.Input.Keyboard.Focus%2A>tenta dar o foco do teclado do elemento especificado.  O elemento retornado por <xref:System.Windows.Input.Keyboard.Focus%2A> é o elemento que atualmente tem o foco do teclado.

 Para que um elemento obtenha o foco do teclado, <xref:System.Windows.UIElement.Focusable%2A> a propriedade e <xref:System.Windows.UIElement.IsVisible%2A> as propriedades devem ser definidas como **true**.  Algumas classes, <xref:System.Windows.Controls.Panel>como, foram <xref:System.Windows.UIElement.Focusable%2A> definidas como `false` por padrão; portanto, talvez você precise definir essa propriedade como `true` se desejar que o elemento seja capaz de obter o foco.

 O exemplo a seguir <xref:System.Windows.Input.Keyboard.Focus%2A> usa para definir o foco do <xref:System.Windows.Controls.Button>teclado em um.  O local recomendado para definir o foco inicial em um aplicativo está no <xref:System.Windows.FrameworkElement.Loaded> manipulador de eventos.

 [!code-csharp[focussample#FocusSampleSetFocus](~/samples/snippets/csharp/VS_Snippets_Wpf/FocusSample/CSharp/Window1.xaml.cs#focussamplesetfocus)]
 [!code-vb[focussample#FocusSampleSetFocus](~/samples/snippets/visualbasic/VS_Snippets_Wpf/FocusSample/visualbasic/window1.xaml.vb#focussamplesetfocus)]

 Para obter mais informações sobre o foco do teclado, consulte [Visão geral do foco](focus-overview.md).

### <a name="logical-focus"></a>Foco lógico
 Foco lógico refere-se <xref:System.Windows.Input.FocusManager.FocusedElement%2A?displayProperty=nameWithType> a em um escopo de foco.  Pode haver vários elementos que têm foco lógico em um aplicativo, mas pode haver apenas um elemento com foco lógico em um escopo de foco específico.

 Um escopo de foco é um elemento de contêiner que controla o <xref:System.Windows.Input.FocusManager.FocusedElement%2A> dentro de seu escopo.  Quando o foco deixar um escopo de foco, o elemento com foco perderá o foco do teclado mas manterá o foco lógico.  Quando o foco retornar para o escopo de foco, o elemento com foco obterá o foco do teclado.  Isso permite que o foco do teclado seja alterado entre vários escopos de foco, mas assegura que o elemento com foco dentro do escopo de foco permanecerá sendo o elemento com foco quando o foco retornar.

 Um elemento pode ser transformado em um escopo [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] de foco no <xref:System.Windows.Input.FocusManager> definindo a <xref:System.Windows.Input.FocusManager.IsFocusScope%2A> propriedade `true`anexada como, ou no código, definindo a propriedade anexada usando o <xref:System.Windows.Input.FocusManager.SetIsFocusScope%2A> método.

 O exemplo a seguir coloca <xref:System.Windows.Controls.StackPanel> um em um escopo de foco definindo <xref:System.Windows.Input.FocusManager.IsFocusScope%2A> a propriedade anexada.

 [!code-xaml[MarkupSnippets#MarkupIsFocusScopeXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/MarkupSnippets/CSharp/Window1.xaml#markupisfocusscopexaml)]

 [!code-csharp[FocusSnippets#FocusSetIsFocusScope](~/samples/snippets/csharp/VS_Snippets_Wpf/FocusSnippets/CSharp/Window1.xaml.cs#focussetisfocusscope)]
 [!code-vb[FocusSnippets#FocusSetIsFocusScope](~/samples/snippets/visualbasic/VS_Snippets_Wpf/FocusSnippets/visualbasic/window1.xaml.vb#focussetisfocusscope)]

 As classes [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] nas quais os escopos de foco são, <xref:System.Windows.Controls.ToolBar>por padrão <xref:System.Windows.Controls.ContextMenu>, são <xref:System.Windows.Window>, <xref:System.Windows.Controls.Menu>, e.

 Um elemento que tem o foco do teclado também terá foco lógico para o escopo de foco ao qual ele pertence; Portanto, definir o foco em um elemento com <xref:System.Windows.Input.Keyboard.Focus%2A> o método <xref:System.Windows.Input.Keyboard> na classe ou nas classes do elemento base tentará dar o foco do teclado do elemento e o foco lógico.

 Para determinar o elemento focalizado em um escopo de foco <xref:System.Windows.Input.FocusManager.GetFocusedElement%2A>, use. Para alterar o elemento focalizado de um escopo de foco <xref:System.Windows.Input.FocusManager.SetFocusedElement%2A>, use.

 Para obter mais informações sobre o foco lógico, consulte [Visão geral do foco](focus-overview.md).

<a name="mouse_position"></a>
## <a name="mouse-position"></a>Posição do mouse
 A [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] API de entrada fornece informações úteis em relação a espaços de coordenadas.  Por exemplo, a coordenada `(0,0)` é a coordenada superior esquerda, mas trata-se do canto superior esquerdo de qual elemento na árvore? O elemento que é o destino de entrada? O elemento ao qual você anexou o manipulador de eventos? Ou alguma outra coisa? Para evitar confusão, a [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] API de entrada requer que você especifique o quadro de referência ao trabalhar com coordenadas obtidas pelo mouse. O <xref:System.Windows.Input.Mouse.GetPosition%2A> método retorna a coordenada do ponteiro do mouse em relação ao elemento especificado.

<a name="mouse_capture"></a>
## <a name="mouse-capture"></a>Captura do mouse
 Dispositivos de mouse mantêm especificamente uma característica modal conhecida como captura do mouse. A captura do mouse é usada para manter um estado de entrada de transição quando uma operação do tipo "arrastar e soltar" é iniciada, de modo que outras operações que envolvem a posição nominal do ponteiro do mouse na tela não necessariamente ocorrem. Durante a ação de arrastar, o usuário não poderá clicar sem anular a ação do tipo "arrastar e soltar", o que torna a maioria das indicações mouseover inadequadas; enquanto isso, a captura do mouse é mantida pela origem da ação de arrastar. O sistema de entrada expõe APIs que podem determinar o estado de captura do mouse, bem como APIs que podem forçar a captura do mouse para um elemento específico ou limpar o estado de captura do mouse. Para obter mais informações sobre operações do tipo "arrastar e soltar", consulte [Visão geral de arrastar e soltar](drag-and-drop-overview.md).

<a name="commands"></a>
## <a name="commands"></a>Comandos
 Os comandos permitem a manipulação de entrada em um nível mais semântico que a entrada do dispositivo.  Os comandos são diretivas simples, como `Cut`, `Copy`, `Paste` ou `Open`.  Os comandos são úteis para centralizar sua lógica de comando.  O mesmo comando pode ser acessado <xref:System.Windows.Controls.Menu>de um, <xref:System.Windows.Controls.ToolBar>em um ou por meio de um atalho de teclado. Os comandos também fornecem um mecanismo para desabilitar os controles quando o comando se torna não disponível.

 <xref:System.Windows.Input.RoutedCommand>é a [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] implementação de <xref:System.Windows.Input.ICommand>.  Quando um <xref:System.Windows.Input.RoutedCommand> é executado, um <xref:System.Windows.Input.CommandManager.PreviewExecuted> e um <xref:System.Windows.Input.CommandManager.Executed> evento são gerados no destino do comando, que faz o túnel e o Bubble através da árvore de elementos, como outras entradas.  Se um destino do comando não for definido, o elemento com o foco do teclado será o destino do comando.  A lógica que executa o comando é anexada a <xref:System.Windows.Input.CommandBinding>um.  Quando um <xref:System.Windows.Input.CommandManager.Executed> evento atinge um <xref:System.Windows.Input.CommandBinding> para esse <xref:System.Windows.Input.CommandBinding> comando específico, o <xref:System.Windows.Input.ExecutedRoutedEventHandler> no é chamado.  Esse manipulador executa a ação do comando.

 Para obter mais informações sobre comandos, consulte [Visão geral dos comandos](commanding-overview.md).

 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]fornece uma biblioteca de comandos comuns que consistem <xref:System.Windows.Input.ApplicationCommands>em <xref:System.Windows.Input.MediaCommands>, <xref:System.Windows.Input.ComponentCommands>, <xref:System.Windows.Input.NavigationCommands>, e <xref:System.Windows.Documents.EditingCommands>, ou você pode definir seu próprio.

 O exemplo a seguir mostra <xref:System.Windows.Controls.MenuItem> como configurar um para que, quando ele for clicado, ele invocará o <xref:System.Windows.Input.ApplicationCommands.Paste%2A> comando <xref:System.Windows.Controls.TextBox>no, supondo que o tenha o foco do <xref:System.Windows.Controls.TextBox> teclado.

 [!code-xaml[CommandingOverviewSnippets#CommandingOverviewSimpleCommand](~/samples/snippets/csharp/VS_Snippets_Wpf/CommandingOverviewSnippets/CSharp/Window1.xaml#commandingoverviewsimplecommand)]

 [!code-csharp[CommandingOverviewSnippets#CommandingOverviewCommandTargetCodeBehind](~/samples/snippets/csharp/VS_Snippets_Wpf/CommandingOverviewSnippets/CSharp/Window1.xaml.cs#commandingoverviewcommandtargetcodebehind)]
 [!code-vb[CommandingOverviewSnippets#CommandingOverviewCommandTargetCodeBehind](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CommandingOverviewSnippets/visualbasic/window1.xaml.vb#commandingoverviewcommandtargetcodebehind)]

 Para obter mais informações sobre comandos em [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)], consulte [Visão geral dos comandos](commanding-overview.md).

<a name="the_input_system_and_base_elements"></a>
## <a name="the-input-system-and-base-elements"></a>O sistema de entrada e elementos base
 Eventos de entrada, como os eventos anexados definidos <xref:System.Windows.Input.Mouse>pelas <xref:System.Windows.Input.Keyboard>classes, <xref:System.Windows.Input.Stylus> e são gerados pelo sistema de entrada e injetados em uma determinada posição no modelo de objeto com base em testes de clique na árvore visual em tempo de execução.

 Cada um dos eventos que <xref:System.Windows.Input.Mouse>, <xref:System.Windows.Input.Keyboard>e <xref:System.Windows.Input.Stylus> definem como um evento anexado também é reexposto pelas classes <xref:System.Windows.UIElement> de elemento base e <xref:System.Windows.ContentElement> como um novo evento roteado. Os eventos roteados de elemento base são gerados por classes que manipulam o evento anexado original e reutilizam os dados do evento.

 Quando o evento de entrada passa a ser associado a um elemento de origem específico por meio da implementação de evento de entrada do seu elemento base, ele pode ser roteado pelo restante de uma rota de evento baseada em uma combinação de objetos de árvore visual e lógicos e pode também ser manipulado pelo código do aplicativo.  Em geral, é mais conveniente lidar com esses eventos de entrada relacionados ao dispositivo usando os eventos roteados <xref:System.Windows.ContentElement>em <xref:System.Windows.UIElement> e, porque você pode usar a sintaxe de manipulador de [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] eventos mais intuitiva no e no código. Em vez disso, você poderia optar por manipular o evento anexado que iniciou o processo, no entanto, você enfrentaria diversos problemas: o evento anexado poderia ser marcado como manipulado pela manipulação de classe do elemento base e, em vez da verdadeira sintaxe de evento, você precisaria usar os métodos acessadores para anexar manipuladores para eventos anexados.

<a name="whats_next"></a>
## <a name="whats-next"></a>Novidades
 Agora você tem várias técnicas para manipular a entrada em [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)].  Você também deve ter uma compreensão melhor dos vários tipos de eventos de entrada e os mecanismos de evento roteado usados pelo [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)].

 Há recursos adicionais disponíveis que explicam roteamento de eventos e elementos estruturais do [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] em mais detalhes. Consulte as visões gerais a seguir para obter mais informações: [Visão geral de comandos](commanding-overview.md), [Visão geral do foco](focus-overview.md), [Visão geral de elementos base](base-elements-overview.md), [Árvores no WPF](trees-in-wpf.md) e [Visão geral de eventos roteados](routed-events-overview.md).

## <a name="see-also"></a>Consulte também

- [Visão geral do foco](focus-overview.md)
- [Visão geral de comandos](commanding-overview.md)
- [Visão geral de eventos roteados](routed-events-overview.md)
- [Visão geral de elementos base](base-elements-overview.md)
- [Propriedades](properties-wpf.md)
