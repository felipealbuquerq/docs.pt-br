---
title: Visão geral do XAML (WPF)
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- user interfaces [XAML]
- classes [XAML]
- root element [XAML]
- XAML [WPF], about XAML
- base classes [XAML]
- routed events [WPF]
- code-behind files [WPF], XAML
- collection properties [XAML]
- events [XAML]
- property element [XAML]
- content models [XAML]
- Extensible Application Markup Language (see XAML)
- attribute syntax [XAML]
ms.assetid: a80db4cd-dd0f-479f-a45f-3740017c22e4
ms.openlocfilehash: ee5318b8ba1284f2805b80b3e41fab3ae739158c
ms.sourcegitcommit: 3eeea78f52ca771087a6736c23f74600cc662658
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68672004"
---
# <a name="xaml-overview-wpf"></a>Visão geral do XAML (WPF)

Este tópico descreve os recursos da linguagem XAML e demonstra como você pode usar o XAML para escrever aplicativos [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)]. Este tópico descreve especificamente o XAML como implementado por [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]. O XAML em si é um conceito de linguagem maior que [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)].  

<a name="what_is_xaml"></a>   
## <a name="what-is-xaml"></a>O que é XAML?  
 O XAML é uma linguagem de marcação declarativa. Conforme aplicado ao modelo de programação .NET Framework, o XAML simplifica a criação [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] de um para um aplicativo .NET Framework. Você pode criar elementos [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] visíveis na marcação XAML declarativa e, em seguida, separar [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] a definição da lógica de tempo de execução usando arquivos de código-behind que são adicionados à marcação por meio de definições de classe parcial. O XAML representa diretamente a instanciação de objetos em um conjunto específico de tipos de suporte definidos em assemblies. Isso é diferente da maioria das outras linguagens de marcação, que são geralmente uma linguagem interpretada sem um vínculo direto para um sistema de tipos de suporte. O XAML habilita um fluxo de trabalho em que partes separadas podem trabalhar na [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] e na lógica de um aplicativo, usando ferramentas potencialmente diferentes.  
  
 Quando representados como texto, arquivos XAML são arquivos XML que geralmente tem a extensão `.xaml`. Os arquivos podem ser codificados por qualquer codificação XML, mas a codificação como UTF-8 é a usual.  
  
 O exemplo a seguir mostra como você pode criar um botão como parte de um [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)]. Este exemplo destina-se apenas a fornecer uma amostra de como o XAML representa metáforas de programação de [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] (ele não é um exemplo completo).  
  
 [!code-xaml[XAMLOvwSupport#DirtSimple](~/samples/snippets/csharp/VS_Snippets_Wpf/XAMLOvwSupport/CSharp/page2.xaml#dirtsimple)]  
  
<a name="xaml_syntax_in_brief"></a>   
## <a name="xaml-syntax-in-brief"></a>Sintaxe XAML em breve  
 As seções a seguir explicam as formas básicas de sintaxe XAML e fornecem um exemplo de marcação curta. Essas seções não se destinam a fornecer informações completas sobre cada forma de sintaxe, por exemplo, de como esses itens são representados no sistema de tipos de suporte. Para obter mais informações sobre as características específicas de sintaxe XAML para cada uma das formas de sintaxe apresentadas neste tópico, consulte [Sintaxe XAML em detalhes](xaml-syntax-in-detail.md).  
  
 Grande parte do material nas próximas seções será elementar para você caso você tenha familiaridade anterior com a linguagem XML. Isso é consequência de um dos princípios básicos de design do XAML.  A linguagem XAML define os conceitos próprios, mas esses conceitos funcionam dentro da linguagem XML e do formulário de marcação.  
  
### <a name="xaml-object-elements"></a>Elementos de objeto XAML  
 Normalmente, um elemento de objeto declara uma instância de um tipo. Esse tipo é definido em assemblies que fornecem os tipos de suporte para uma tecnologia que usa XAML como uma linguagem.  
  
 A sintaxe de elemento de objeto sempre começa com um colchete de abertura (\<). Isso é seguido pelo nome do tipo em que você deseja criar uma instância. (O nome pode incluir um prefixo, um conceito que será explicado posteriormente.) Depois disso, você pode opcionalmente declarar atributos no elemento do objeto. Para concluir a marca de elemento de objeto, encerre com um colchete angular de fechamento (>). Você pode usar um formulário de autofechamento que não tem nenhum conteúdo, concluindo a marca com uma barra invertida e um colchete angular de fechamento em sucessão (/ >). Por exemplo, examine novamente o snippet de marcação mostrado anteriormente:  
  
 [!code-xaml[XAMLOvwSupport#DirtSimple](~/samples/snippets/csharp/VS_Snippets_Wpf/XAMLOvwSupport/CSharp/page2.xaml#dirtsimple)]  
  
 Isso especifica dois elementos de objeto: `<StackPanel>` (com conteúdo e, posteriormente, uma marca de fechamento) e `<Button .../>` (o formulário de autofechamento, com vários atributos). Cada um dos elementos de objeto `StackPanel` e `Button` mapeia para o nome de uma classe que é definido pelo [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] e é parte dos assemblies do [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]. Quando você especifica uma marca de elemento de objeto, você cria uma instrução para o processamento XAML criar uma nova instância. Cada instância é criada chamando o construtor sem parâmetros do tipo subjacente ao analisar e carregar o XAML.  
  
### <a name="attribute-syntax-properties"></a>Sintaxe de atributo (Propriedades)  
 As propriedades de um objeto geralmente podem ser expressas como atributos do elemento de objeto. Uma sintaxe de atributo nomeia a propriedade que está sendo definida na sintaxe de atributo, seguida pelo operador de atribuição (=). O valor de um atributo é sempre especificado como uma cadeia de caracteres entre aspas.  
  
 A sintaxe de atributo é a sintaxe de configuração de propriedade mais simplificada e é a sintaxe mais intuitiva para os desenvolvedores que já usaram linguagens de marcação anteriormente. Por exemplo, a marcação a seguir cria um botão com texto vermelho e uma tela de fundo azul, além de texto de exibição especificado como `Content`.  
  
 [!code-xaml[XAMLOvwSupport#BlueRedButton](~/samples/snippets/csharp/VS_Snippets_Wpf/XAMLOvwSupport/CSharp/Page1.xaml#blueredbutton)]  
  
### <a name="property-element-syntax"></a>Sintaxe do elemento Property  
 Para algumas propriedades de um elemento de objeto, a sintaxe de atributo não é possível porque o objeto ou as informações necessárias para fornecer o valor da propriedade não podem ser expressos adequadamente dentro das restrições de aspas e cadeias de caracteres da sintaxe de atributo. Nesses casos é possível usar uma sintaxe diferente, conhecida como sintaxe de elemento de propriedade.  
  
 A sintaxe para a marca de início de elemento de propriedade é `<`*typeName*`.`*propertyName*`>`. Em geral, o conteúdo dessa marca é um elemento Object do tipo que a propriedade usa como seu valor. Depois de especificar o conteúdo, você deve fechar o elemento de propriedade com uma marca de fim. A sintaxe para a marca de fim é `</`*typeName*`.`*propertyName*`>`.  
  
 Se uma sintaxe de atributo é possível, usá-la é normalmente mais conveniente e permite uma marcação mais compacta, mas isso geralmente é apenas uma questão de estilo e não uma limitação técnica. O exemplo a seguir mostra as mesmas propriedades que foram definidas no exemplo de sintaxe de atributo anterior sendo definidas novamente, mas desta vez usando sintaxe de elemento de propriedade para todas as propriedades do `Button`.  
  
 [!code-xaml[XAMLOvwSupport#BlueRedButtonPE](~/samples/snippets/csharp/VS_Snippets_Wpf/XAMLOvwSupport/CSharp/Page1.xaml#blueredbuttonpe)]  
  
### <a name="collection-syntax"></a>Sintaxe da coleção  
 A linguagem XAML inclui algumas otimizações que produzem uma marcação mais legível. Uma otimização desse tipo é tal que, se uma determinada propriedade utiliza um tipo de coleção, os itens que você declara na marcação como elementos filho no valor dessa propriedade tornam-se parte da coleção. Nesse caso, uma coleção de elementos de objeto filho é o valor que está sendo definido para a propriedade de coleção.  
  
 O exemplo a seguir mostra a sintaxe de coleção para definir <xref:System.Windows.Media.GradientBrush.GradientStops%2A> valores da propriedade:  
  
```xaml  
<LinearGradientBrush>  
  <LinearGradientBrush.GradientStops>  
    <!-- no explicit new GradientStopCollection, parser knows how to find or create -->  
    <GradientStop Offset="0.0" Color="Red" />  
    <GradientStop Offset="1.0" Color="Blue" />  
  </LinearGradientBrush.GradientStops>  
</LinearGradientBrush>  
```  
  
### <a name="xaml-content-properties"></a>Propriedades de conteúdo XAML  
 O XAML especifica um recurso de linguagem em que uma classe pode atribuir exatamente uma de suas propriedades para ser a propriedade de conteúdo XAML. Elementos filho desse elemento de objeto são usados para definir o valor dessa propriedade de conteúdo. Em outras palavras, para a propriedade de conteúdo exclusivamente, você pode omitir um elemento de propriedade ao definir essa propriedade na marcação XAML e produzir uma metáfora de pai/filho mais visível na marcação.  
  
 Por exemplo, <xref:System.Windows.Controls.Border> especifica uma propriedade de conteúdo <xref:System.Windows.Controls.Decorator.Child%2A>de. Os dois <xref:System.Windows.Controls.Border> elementos a seguir são tratados de forma idêntica. O primeiro aproveita a sintaxe de propriedade de conteúdo e omite o elemento de propriedade `Border.Child`. O segundo mostra `Border.Child` explicitamente.  
  
```xaml  
<Border>  
  <TextBox Width="300"/>  
</Border>  
<!--explicit equivalent-->  
<Border>  
  <Border.Child>  
    <TextBox Width="300"/>  
  </Border.Child>  
</Border>  
```  
  
 Como uma regra da linguagem XAML, o valor de uma propriedade de conteúdo XAML deve ser fornecido inteiramente antes ou inteiramente depois de todos os outros elementos de propriedade nesse elemento de objeto. Por exemplo, a marcação a seguir não compila:  
  
```xaml
<Button>I am a   
  <Button.Background>Blue</Button.Background>  
  blue button</Button>  
```  
  
 Para obter mais informações sobre essa restrição em propriedades de conteúdo XAML, consulte a seção "Propriedades de conteúdo XAML" de [Sintaxe XAML em detalhes](xaml-syntax-in-detail.md).  
  
### <a name="text-content"></a>Conteúdo de texto  
 Um número pequeno de elementos XAML pode processar texto diretamente como seu conteúdo. Para habilitar isso, um dos casos a seguir deve ser verdadeiro:  
  
- A classe deve declarar uma propriedade de conteúdo e essa propriedade de conteúdo deve ser de um tipo atribuível a uma cadeia de caracteres (o tipo <xref:System.Object>pode ser). Por exemplo, qualquer <xref:System.Windows.Controls.ContentControl> usa <xref:System.Windows.Controls.ContentControl.Content%2A> como sua propriedade Content e é do tipo <xref:System.Object>, e isso dá suporte ao <xref:System.Windows.Controls.Button>seguinte uso em uma <xref:System.Windows.Controls.ContentControl> prática, como: `<Button>Hello</Button>`.  
  
- O tipo deve declarar um conversor de tipo e, nesse caso, o conteúdo de texto é usado como texto de inicialização para esse conversor de tipo. Por exemplo, `<Brush>Blue</Brush>`. Esse caso é menos comum na prática.  
  
- O tipo deve ser um primitivo conhecido de linguagem XAML.  
  
### <a name="content-properties-and-collection-syntax-combined"></a>Propriedades de conteúdo e sintaxe de coleção combinadas  
 Considere este exemplo:  
  
```xaml  
<StackPanel>  
  <Button>First Button</Button>  
  <Button>Second Button</Button>  
</StackPanel>  
```  
  
 Aqui, cada <xref:System.Windows.Controls.Button> um é um elemento filho <xref:System.Windows.Controls.StackPanel>de. Isso é uma marcação otimizada e intuitiva que omite duas marcas por dois motivos diferentes.  
  
- **Elemento de Propriedade StackPanel. Children omitido:** <xref:System.Windows.Controls.StackPanel> deriva de <xref:System.Windows.Controls.Panel>. <xref:System.Windows.Controls.Panel>define <xref:System.Windows.Controls.Panel.Children%2A?displayProperty=nameWithType> como sua propriedade de conteúdo XAML.  
  
- **Elemento de objeto UIElementcollection omitido:** A <xref:System.Windows.Controls.Panel.Children%2A?displayProperty=nameWithType> propriedade usa o tipo <xref:System.Windows.Controls.UIElementCollection>, que implementa <xref:System.Collections.IList>. A marca do elemento da coleção pode ser omitida, com base nas regras XAML para processamento de <xref:System.Collections.IList>coleções, como. (Nesse caso, <xref:System.Windows.Controls.UIElementCollection> na verdade não pode ser instanciado porque não expõe um construtor sem parâmetros, e é por isso que o <xref:System.Windows.Controls.UIElementCollection> elemento Object é mostrado comentado).  
  
```xaml  
<StackPanel>  
  <StackPanel.Children>  
    <!--<UIElementCollection>-->  
    <Button>First Button</Button>  
    <Button>Second Button</Button>  
    <!--</UIElementCollection>-->  
  </StackPanel.Children>  
</StackPanel>  
```  
  
### <a name="attribute-syntax-events"></a>Sintaxe de atributo (eventos)  
 A sintaxe de atributo também pode ser usada para os membros que são eventos, em vez de propriedades. Nesse caso, o nome do atributo é o nome do evento. A implementação de WPF de eventos para XAML, o valor do atributo é o nome de um manipulador que implementa o delegado do evento. Por exemplo, a marcação a seguir atribui um manipulador para o <xref:System.Windows.Controls.Primitives.ButtonBase.Click> evento a um <xref:System.Windows.Controls.Button> criado na marcação:  
  
 [!code-xaml[XAMLOvwSupport#ButtonWithCodeBehind](~/samples/snippets/csharp/VS_Snippets_Wpf/XAMLOvwSupport/CSharp/page3.xaml#buttonwithcodebehind)]  
  
 Há muito mais envolvido no uso de eventos e XAML no WPF do que apenas esse exemplo da sintaxe de atributo. Por exemplo, você talvez esteja se perguntando o que o `ClickHandler` referenciado aqui representa e como ele está definido. Isso será explicado na seção [Eventos e XAML Code-Behind](xaml-overview-wpf.md#events_and_xaml_codebehind), posteriormente neste tópico.  
  
<a name="case_and_white space_in_xaml"></a>   
## <a name="case-and-white-space-in-xaml"></a>Caso e espaço em branco em XAML  
 Em termos gerais, o XAML diferencia maiúsculas e minúsculas. Para fins de resolução de tipos de suporte, o XAML WPF diferencia maiúsculas e minúsculas pelas mesmas regras que o CLR diferencia maiúsculas e minúsculas. Elementos de objeto, elementos de propriedade e nomes de atributo devem ser especificados usando diferenciação de maiúsculas e minúsculas quando comparados pelo nome ao tipo subjacente no assembly ou a um membro de um tipo. Primitivos e palavras-chave de linguagem XAML também diferenciam maiúsculas e minúsculas. Os valores nem sempre diferenciam maiúsculas e minúsculas. A diferenciação de maiúsculas e minúsculas em valores dependerá do comportamento do conversor de tipo associado com a propriedade que recebe o valor ou do tipo de valor da propriedade. Por exemplo, as propriedades que usam <xref:System.Boolean> o tipo podem usar `true` um `True` ou como valores equivalentes, mas somente porque a conversão de tipo de analisador XAML do <xref:System.Boolean> WPF nativo para a cadeia de caracteres já permite isso como equivalentes.  
  
 Os processadores e serializadores WPF XAML ignorarão ou descartarão todos os espaços em branco não significativos e normalizarão qualquer espaço em branco significativo. Isso é consistente com as recomendações de comportamento de espaço em branco padrão da especificação XAML. Esse comportamento geralmente é importante apenas quando você especifica cadeias de caracteres dentro de propriedades de conteúdo XAML. Em termos mais simples, o XAML converte caracteres de espaço, avanço de linha e tabulação em espaços e, em seguida, mantém um espaço se este é encontrado em qualquer uma das extremidades de uma cadeia de caracteres contígua. A explicação completa do tratamento de espaço em branco XAML não é abordada neste tópico. Para obter detalhes, confira [processamento de espaço em branco em XAML](../../xaml-services/whitespace-processing-in-xaml.md).  
  
<a name="markup_extensions"></a>   
## <a name="markup-extensions"></a>Extensões de marcação  
 Extensões de marcação são um conceito de linguagem XAML. Quando usados para fornecer o valor de uma sintaxe de atributo, chaves (`{` e `}`) indicam um uso de extensão de marcação. Esse uso direciona o processamento XAML para escape do tratamento geral de valores de atributo como uma cadeia de caracteres literal ou um valor conversível em cadeia de caracteres.  
  
 As extensões de marcação mais comuns usadas na programação de aplicativos [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] são [Binding](binding-markup-extension.md) (usada para expressões de vinculação de dados) e as referências de recurso [StaticResource](staticresource-markup-extension.md) e [DynamicResource](dynamicresource-markup-extension.md). Usando extensões de marcação, você pode usar a sintaxe de atributo para fornecer valores para propriedades mesmo que essas propriedades não deem suporte a uma sintaxe de atributo em geral. Extensões de marcação geralmente usam tipos de expressão intermediários para habilitar recursos como o adiamento de valores ou fazem referência a outros objetos que só estão presentes em tempo de execução.  
  
 Por exemplo, a marcação a seguir define o valor da <xref:System.Windows.FrameworkElement.Style%2A> propriedade usando a sintaxe do atributo. A <xref:System.Windows.FrameworkElement.Style%2A> propriedade usa uma instância <xref:System.Windows.Style> da classe, que, por padrão, não pôde ser instanciada por uma cadeia de caracteres de sintaxe de atributo. Mas nesse caso, o atributo referencia uma extensão de marcação específica, [StaticResource](staticresource-markup-extension.md). Quando essa extensão de marcação é processada, ela retorna uma referência a um estilo que foi anteriormente instanciado como um recurso com chave em um dicionário de recursos.  
  
 [!code-xaml[FEResourceSH_snip#XAMLOvwShortResources](~/samples/snippets/csharp/VS_Snippets_Wpf/FEResourceSH_snip/CS/page1.xaml#xamlovwshortresources)]  
[!code-xaml[FEResourceSH_snip#XAMLOvwShortResources2](~/samples/snippets/csharp/VS_Snippets_Wpf/FEResourceSH_snip/CS/page1.xaml#xamlovwshortresources2)]  
[!code-xaml[FEResourceSH_snip#XAMLOvwShortResources3](~/samples/snippets/csharp/VS_Snippets_Wpf/FEResourceSH_snip/CS/page1.xaml#xamlovwshortresources3)]  
  
 Para uma referência listando todas as extensões de marcação para XAML implementado especificamente no WPF, consulte [Extensões XAML WPF](wpf-xaml-extensions.md). Para obter uma listagem de referência das extensões de marcação que são definidas pelo System. XAML e estão mais amplamente disponíveis para .NET Framework implementações [XAML, consulte XAML namespace (x:) Recursos](../../xaml-services/xaml-namespace-x-language-features.md)de linguagem. Para obter mais informações sobre conceitos de extensão de marcação, consulte [Extensões de marcação e XAML WPF](markup-extensions-and-wpf-xaml.md).  
  
<a name="type_converters"></a>   
## <a name="type-converters"></a>Conversores de tipo  
 Na seção [Resumo sobre sintaxe XAML](xaml-overview-wpf.md#xaml_syntax_in_brief), foi mencionado que o valor do atributo deve poder ser definido por uma cadeia de caracteres. O tratamento nativo básico de como as <xref:System.String> cadeias de caracteres são convertidas em outros tipos de objeto ou valores primitivos baseia-se no próprio tipo, além do processamento nativo para determinados tipos <xref:System.DateTime> , como ou <xref:System.Uri>. Mas muitos tipos [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] ou membros desses tipos estendem o comportamento de processamento de atributos de cadeias de caracteres básicos, de modo que instâncias de tipos de objeto mais complexos podem ser especificadas como cadeias de caracteres e atributos.  
  
 A <xref:System.Windows.Thickness> estrutura é um exemplo de um tipo que tem uma conversão de tipo habilitada para usos XAML. <xref:System.Windows.Thickness>indica medidas em um retângulo aninhado e é usado como o valor para propriedades como <xref:System.Windows.FrameworkElement.Margin%2A>. Ao colocar um conversor de tipo <xref:System.Windows.Thickness>no, todas as propriedades que <xref:System.Windows.Thickness> usam um são mais fáceis de especificar em XAML porque podem ser especificadas como atributos. O exemplo a seguir usa uma conversão de tipo e uma sintaxe de atributo para fornecer <xref:System.Windows.FrameworkElement.Margin%2A>um valor para:  
  
 [!code-xaml[XAMLOvwSupport#MarginTCE](~/samples/snippets/csharp/VS_Snippets_Wpf/XAMLOvwSupport/CSharp/page7.xaml#margintce)]  
  
 O exemplo de sintaxe de atributo anterior é equivalente ao seguinte exemplo de sintaxe mais detalhada, <xref:System.Windows.FrameworkElement.Margin%2A> em que o é definido por meio da sintaxe <xref:System.Windows.Thickness> do elemento de propriedade que contém um elemento Object. As quatro propriedades de chave <xref:System.Windows.Thickness> de são definidas como atributos na nova instância:  
  
 [!code-xaml[XAMLOvwSupport#MarginVerbose](~/samples/snippets/csharp/VS_Snippets_Wpf/XAMLOvwSupport/CSharp/page7.xaml#marginverbose)]  
  
> [!NOTE]
> Há também um número limitado de objetos em que a conversão de tipo é a única maneira pública de definir uma propriedade para esse tipo sem envolver uma subclasse, porque o próprio tipo não tem um construtor com parâmetros. Um exemplo é <xref:System.Windows.Input.Cursor>.  
  
 Para obter mais informações sobre como a conversão de tipo e seu uso da sintaxe de atributo têm suporte, consulte [TypeConverters e XAML](typeconverters-and-xaml.md).  
  
<a name="xaml_root_elements_and_xaml_namespaces"></a>   
## <a name="xaml-root-elements-and-xaml-namespaces"></a>Elementos raiz XAML e namespaces XAML  
 Um arquivo XAML deve ter apenas um elemento raiz para que ele seja tanto um arquivo [!INCLUDE[TLA2#tla_xml](../../../../includes/tla2sharptla-xml-md.md)] bem formado quanto um arquivo XAML válido. Para cenários típicos do WPF, você usa um elemento raiz que tem um significado proeminente no modelo de aplicativo do WPF ( <xref:System.Windows.Window> por <xref:System.Windows.Controls.Page> exemplo, ou para <xref:System.Windows.ResourceDictionary> uma página, para um dicionário <xref:System.Windows.Application> externo ou para a definição do aplicativo). O exemplo a seguir mostra o elemento raiz de um arquivo XAML típico para [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] uma página, com o elemento raiz <xref:System.Windows.Controls.Page>de.  
  
 [!code-xaml[XAMLOvwSupport#RootOnly](~/samples/snippets/csharp/VS_Snippets_Wpf/XAMLOvwSupport/CSharp/page2.xaml#rootonly)]  
[!code-xaml[XAMLOvwSupport#RootOnly2](~/samples/snippets/csharp/VS_Snippets_Wpf/XAMLOvwSupport/CSharp/page2.xaml#rootonly2)]  
  
 O elemento raiz também contém os atributos `xmlns` e `xmlns:x`. Esses atributos indicam a um processador XAML quais namespaces XAML contêm as definições de tipo para tipos de suporte que a marcação referenciará como elementos. O atributo `xmlns` indica especificamente o namespace XAML padrão. Dentro do namespace XAML padrão, os elementos de objeto na marcação podem ser especificados sem um prefixo. Para a maioria dos cenários de aplicativo [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] e para quase todos os exemplos fornecidos nas seções do [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] do [!INCLUDE[TLA2#tla_sdk](../../../../includes/tla2sharptla-sdk-md.md)], o namespace XAML padrão é mapeado para o namespace [!INCLUDE[TLA#tla_wpfxmlnsv1](../../../../includes/tlasharptla-wpfxmlnsv1-md.md)] do [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]. O atributo `xmlns:x` indica um namespace XAML adicional, que mapeia o namespace [!INCLUDE[TLA#tla_xamlxmlnsv1](../../../../includes/tlasharptla-xamlxmlnsv1-md.md)] da linguagem XAML.  
  
 Esse uso de `xmlns` para definir um escopo para uso e mapeamento de um namescope é consistente com a especificação do XML 1.0. Namescopes XAML são diferentes de namescopes XML apenas porque um namescope XAML também implica algo sobre como os elementos do namescope têm suporte de tipos quando se trata de resolução de tipo e análise do XAML.  
  
 Observe que os atributos `xmlns` são estritamente necessários somente no elemento raiz de cada arquivo XAML. As definições do `xmlns` serão aplicadas a todos os elementos descendentes do elemento raiz (esse comportamento é novamente consistente com a especificação XML 1.0 para `xmlns`). Atributos `xmlns` também são permitidos em outros elementos abaixo da raiz e se aplicariam a quaisquer elementos descendentes do elemento definidor. No entanto, definição ou redefinição frequente de namespaces XAML pode resultar em um estilo de marcação XAML difícil de ler.  
  
 A implementação [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] do seu processador XAML inclui uma infraestrutura com reconhecimento dos assemblies de núcleo do WPF. O assemblies de núcleo do [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] são conhecidos por conter os tipos que dão suporte a mapeamentos do [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] para o namespace XAML padrão. Isso é habilitado por meio da configuração que faz parte de seu arquivo de build do projeto e dos sistemas de projeto e build do WPF. Portanto, a declaração de namespace XAML padrão como o `xmlns` padrão é tudo o que é necessário para fazer referência a elementos XAML que vêm de assemblies [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)].  
  
### <a name="the-x-prefix"></a>O prefixo x:  
 No exemplo de elemento raiz anterior, o prefixo `x:` foi usado para mapear o namespace XAML [!INCLUDE[TLA#tla_xamlxmlnsv1](../../../../includes/tlasharptla-xamlxmlnsv1-md.md)], que é o namespace XAML dedicado que dá suporte a constructos de linguagem XAML. Esse prefixo `x:` é usado para mapear esse namespace XAML nos modelos para projetos, em exemplos e na documentação em todo este [!INCLUDE[TLA2#tla_sdk](../../../../includes/tla2sharptla-sdk-md.md)]. O namespace XAML para a linguagem XAML contém várias construções de programação que você usará com muita frequência em seu XAML. A seguir está uma lista dos constructos de programação de prefixo `x:` mais comuns que você usará:  
  
- [x:Key](../../xaml-services/x-key-directive.md): Define uma chave exclusiva para cada recurso em um <xref:System.Windows.ResourceDictionary> (ou conceitos de dicionário semelhantes em outras estruturas). `x:Key` provavelmente será responsável por 90% dos usos de `x:` que você verá na marcação de um aplicativo WPF típico.  
  
- [x:Class](../../xaml-services/x-class-directive.md): Especifica o namespace CLR e o nome de classe para a classe que fornece code-behind para uma página XAML. Você deve ter uma classe desse tipo para dar suporte a code-behind segundo o modelo de programação do WPF e, portanto, você quase sempre vê `x:` mapeado, mesmo que não existam recursos.  
  
- [x:Name](../../xaml-services/x-name-directive.md): Especifica um nome de objeto de tempo de execução para a instância que existe no código de tempo de execução depois que um elemento de objeto é processado. Em geral, você usará frequentemente uma propriedade equivalente definida pelo WPF para [X:Name](../../xaml-services/x-name-directive.md). Essas propriedades mapeiam especificamente para uma propriedade de suporte a CLR e, portanto, são mais convenientes para programação de aplicativo, em que você frequentemente usa código em tempo de execução para localizar os elementos nomeados do XAML inicializado. A propriedade mais comum é <xref:System.Windows.FrameworkElement.Name%2A?displayProperty=nameWithType>. Você ainda pode usar [x:Name](../../xaml-services/x-name-directive.md) quando a propriedade de nível <xref:System.Windows.FrameworkElement.Name%2A> de estrutura do WPF equivalente não tiver suporte em um tipo específico. Isso ocorre em determinados cenários de animação.  
  
- [x:static](../../xaml-services/x-static-markup-extension.md): Habilita uma referência que retorna um valor estático que não é, de outra forma, uma propriedade compatível com XAML.  
  
- [x:Type](../../xaml-services/x-type-markup-extension.md): Constrói uma <xref:System.Type> referência com base em um nome de tipo. Isso é usado para especificar atributos que usam <xref:System.Type>, <xref:System.Windows.Style.TargetType%2A?displayProperty=nameWithType>como, embora frequentemente a propriedade tenha cadeia de caracteres<xref:System.Type> nativa para conversão de forma que o uso da extensão de marcação [x:Type](../../xaml-services/x-type-markup-extension.md) seja opcional.  
  
 Há constructos de programação adicionais no prefixo `x:`/namespace XAML que não são tão comuns. Para obter detalhes, [consulte XAML namespace (x:) Recursos](../../xaml-services/xaml-namespace-x-language-features.md)de linguagem.  
  
<a name="custom_prefixes_and_custom_types_in_xaml"></a>   
## <a name="custom-prefixes-and-custom-types-in-xaml"></a>Prefixos personalizados e tipos personalizados em XAML  
 Para seus próprios assemblies personalizados ou para assemblies fora do núcleo do WPF de PresentationCore, PresentationFramework e WindowsBase, você pode especificar o assembly como parte de um mapeamento `xmlns` personalizado. Você pode então referenciar tipos desse assembly em seu XAML, desde que esse tipo esteja corretamente implementado para dar suporte aos usos XAML que você está tentando executar.  
  
 A seguir, um exemplo bem básico de como prefixos personalizados funcionam na marcação XAML. O prefixo `custom` é definido na marca do elemento raiz e mapeado para um assembly específico que é empacotado e está disponível com o aplicativo. Esse assembly contém um tipo `NumericUpDown`, que é implementado para dar suporte ao uso geral do XAML, bem como usar uma herança de classe que permite sua inserção, nesse momento específico, em um modelo de conteúdo XAML WPF. Uma instância desse controle `NumericUpDown` é declarada como um elemento de objeto, usando o prefixo para que um analisador XAML saiba qual namespace XAML contém o tipo e, portanto, a localização do assembly de suporte que contém a definição de tipo.  
  
```xaml
<Page  
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"   
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"   
    xmlns:custom="clr-namespace:NumericUpDownCustomControl;assembly=CustomLibrary"  
    >  
  <StackPanel Name="LayoutRoot">  
    <custom:NumericUpDown Name="numericCtrl1" Width="100" Height="60"/>  
...  
  </StackPanel>  
</Page>  
```  
  
 Para obter mais informações sobre tipos personalizados em XAML, consulte [XAML e classes personalizadas para WPF](xaml-and-custom-classes-for-wpf.md).  
  
 Para obter mais informações sobre como os namespaces XML e os namespaces do código de suporte em assemblies são relacionados, consulte [Namespaces XAML e mapeamento de namespace para XAML WPF](xaml-namespaces-and-namespace-mapping-for-wpf-xaml.md).  
  
<a name="events_and_xaml_codebehind"></a>   
## <a name="events-and-xaml-code-behind"></a>Eventos e código XAML-por trás  
 A maioria dos aplicativos [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] consistem em ambos marcação XAML e code-behind. Dentro de um projeto, o XAML é escrito como `.xaml` um arquivo e uma linguagem CLR, como o Microsoft Visual Basic C# ou é usado para gravar um arquivo code-behind. Quando um arquivo XAML é compilado com marcação como parte dos modelos de aplicativos e programação WPF, o local do arquivo XAML code-behind para um arquivo XAML é identificado por meio da especificação de um namespace e classe como o atributo `x:Class` do elemento raiz do XAML.  
  
 Nos exemplos até agora você viu vários botões, mas nenhum deles tinha ainda qualquer comportamento lógico associado. O mecanismo de nível de aplicativo primário para adicionar um comportamento para um elemento de objeto é usar um evento existente da classe de elemento e escrever um manipulador específico para o evento que é invocado quando esse evento é acionado em tempo de execução. O nome do evento e o nome do manipulador a usar são especificados na marcação, enquanto o código que implementa o manipulador é definido no code-behind.  
  
 [!code-xaml[XAMLOvwSupport#ButtonWithCodeBehind](~/samples/snippets/csharp/VS_Snippets_Wpf/XAMLOvwSupport/CSharp/page3.xaml#buttonwithcodebehind)]  
  
 [!code-csharp[XAMLOvwSupport#ButtonWithCodeBehindHandler](~/samples/snippets/csharp/VS_Snippets_Wpf/XAMLOvwSupport/CSharp/page3.xaml.cs#buttonwithcodebehindhandler)]
 [!code-vb[XAMLOvwSupport#ButtonWithCodeBehindHandler](~/samples/snippets/visualbasic/VS_Snippets_Wpf/XAMLOvwSupport/VisualBasic/Page1.xaml.vb#buttonwithcodebehindhandler)]  
  
 Observe que o arquivo code-behind usa o namespace CLR `ExampleNamespace` e declara `ExamplePage` como uma classe parcial dentro desse namespace. Isso é comparável ao valor do atributo `x:Class` de `ExampleNamespace`.`ExamplePage` que foi fornecido na raiz da marcação. O compilador de marcação do WPF criará uma classe parcial para qualquer arquivo XAML compilado, derivando uma classe do tipo de elemento raiz. Quando você fornece code-behind que também define a mesma classe parcial, o código resultante é combinado dentro do mesmo namespace e classe do aplicativo compilado.  
  
 Para obter mais informações sobre requisitos de programação code-behind no WPF, consulte a seção "Code-behind, manipulador de eventos e requisitos de classe parcial" de [Code-Behind e XAML no WPF](code-behind-and-xaml-in-wpf.md).  
  
 Se você não deseja criar um arquivo code-behind separado, você também pode embutir seu código em um arquivo XAML. No entanto, o código embutido é uma técnica menos versátil que tem limitações significativas. Para obter detalhes, consulte [Code-behind e XAML no WPF](code-behind-and-xaml-in-wpf.md).  
  
### <a name="routed-events"></a>Eventos roteados  
 Um recurso de evento específico que é fundamental para [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] é um evento roteado. Eventos roteados habilitam um elemento para manipular um evento que foi acionado por um elemento diferente, desde que os elementos estejam conectados por meio de uma relação de árvore. Ao especificar o tratamento de eventos com um atributo XAML, o evento roteado pode ser escutado e manipulado em qualquer elemento, incluindo elementos que não listam esse evento específico na tabela de membros de classe. Isso é feito qualificando o atributo de nome do evento com o nome de classe proprietária. Por exemplo, o pai `StackPanel` no exemplo em `StackPanel` andamento `Button.Click` <xref:System.Windows.Controls.Primitives.ButtonBase.Click>  /  `Button` poderia registrar um manipulador para o evento do botão do elemento `StackPanel` filho, especificando o atributo no o elemento Object, com o nome do manipulador como o valor do atributo. Para obter mais informações sobre como eventos roteados funcionam, consulte [Visão geral de eventos roteados](routed-events-overview.md).  
  
<a name="x_name_and_xaml_named_elements"></a>   
## <a name="xaml-named-elements"></a>Elementos nomeados XAML  
 Por padrão, a instância do objeto criada em um grafo de objeto pelo processamento de um elemento de objeto XAML não tem um identificador exclusivo nem uma referência de objeto. Por outro lado, se você chamar um construtor no código, você quase sempre usará o resultado do construtor para definir uma variável para a instância construída, de modo que você poderá fazer referência à instância posteriormente no seu código. Para oferecer acesso padronizado a objetos que criados por meio de uma definição de marcação, o XAML define o [atributo X:Name](../../xaml-services/x-name-directive.md). Você pode definir o valor do atributo `x:Name` em qualquer elemento de objeto. Em seu code-behind, o identificador escolhido é equivalente a uma variável de instância que faz referência à instância construída. Em todos os aspectos, elementos nomeados funcionam como se fossem instâncias de objeto (o nome faz referência a essa instância) e o code-behind pode fazer referência aos elementos nomeados para manipular interações de tempo de execução dentro do aplicativo. Essa conexão entre instâncias e variáveis é realizada pelo compilador de marcação XAML do WPF e, mais especificamente, envolve recursos e padrões <xref:System.Windows.Markup.IComponentConnector.InitializeComponent%2A> como esse não será discutido em detalhes neste tópico.  
  
 Elementos XAML de nível de estrutura do WPF <xref:System.Windows.FrameworkElement.Name%2A> herdam uma propriedade, que é equivalente ao `x:Name` atributo XAML definido. Algumas outras classes também fornecem equivalentes de nível de propriedade para `x:Name`, que também geralmente é definido como uma propriedade `Name`. Em termos gerais, se você não encontrar uma propriedade `Name` na tabela de membros de seu elemento/tipo escolhido, use `x:Name` em vez disso. Os `x:Name` valores fornecerão um identificador para um elemento XAML que pode ser usado em tempo de execução, seja por subsistemas específicos ou por métodos de utilitário <xref:System.Windows.FrameworkElement.FindName%2A>, como.  
  
 O exemplo a seguir <xref:System.Windows.FrameworkElement.Name%2A> define em <xref:System.Windows.Controls.StackPanel> um elemento. Em seguida, um manipulador em <xref:System.Windows.Controls.Button> um no <xref:System.Windows.Controls.StackPanel> que faz <xref:System.Windows.Controls.StackPanel> referência ao por meio `buttonContainer` de sua referência <xref:System.Windows.FrameworkElement.Name%2A>de instância, conforme definido por.  
  
 [!code-xaml[XAMLOvwSupport#NamedE](~/samples/snippets/csharp/VS_Snippets_Wpf/XAMLOvwSupport/CSharp/page7.xaml#namede)]  
[!code-xaml[XAMLOvwSupport#NamedE2](~/samples/snippets/csharp/VS_Snippets_Wpf/XAMLOvwSupport/CSharp/page7.xaml#namede2)]  
  
 [!code-csharp[XAMLOvwSupport#NameCode](~/samples/snippets/csharp/VS_Snippets_Wpf/XAMLOvwSupport/CSharp/page7.xaml.cs#namecode)]
 [!code-vb[XAMLOvwSupport#NameCode](~/samples/snippets/visualbasic/VS_Snippets_Wpf/XAMLOvwSupport/VisualBasic/Page1.xaml.vb#namecode)]  
  
 Assim como uma variável, o nome XAML para uma instância é regido por um conceito do escopo, de modo que pode ser imposto que os nomes sejam exclusivos dentro de um determinado escopo previsível. A marcação primária que define uma página denota um namescope XAML exclusivo, com o limite de namescopes XAML sendo o elemento raiz dessa página. No entanto, outras fontes de marcação como estilos ou modelos dentro de estilos podem interagir com uma página em tempo de execução e essas fontes de marcação geralmente têm seus próprios namescopes XAML, que não necessariamente conectam-se com o namescope XAML da página. Para obter mais informações `x:Name` e namescopes XAML, consulte <xref:System.Windows.FrameworkElement.Name%2A>, [diretiva x:Name](../../xaml-services/x-name-directive.md)ou [namescopes XAML do WPF](wpf-xaml-namescopes.md).  
  
<a name="attached_properties_and_attached_events"></a>   
## <a name="attached-properties-and-attached-events"></a>Propriedades anexadas e eventos anexados  
 O XAML Especifica um recurso de linguagem que permite que determinadas propriedades ou eventos sejam especificados em qualquer elemento, independentemente de a propriedade ou evento existir nas definições do tipo para o elemento em que ele está sendo definido. A versão de propriedades desse recurso é chamada de uma propriedade anexada, a versão de eventos é chamada de um evento anexado. Conceitualmente, você pode pensar em propriedades anexadas e eventos anexados como membros globais que podem ser definidos em qualquer instância de objeto/elemento XAML. No entanto, esse elemento/classe ou uma infraestrutura maior deve dar suporte a um repositório de propriedades de suporte para os valores anexados.  
  
 Propriedades anexadas em XAML normalmente são usadas por meio da sintaxe de atributo. Na sintaxe de atributo, você especifica uma propriedade anexada na forma *ownerType*.*propertyName*.  
  
 Superficialmente, isso é semelhante a um uso de elemento de propriedade; neste caso, no entanto, o *ownerType* que você especifica é sempre um tipo diferente do elemento de objeto em que a propriedade anexada está sendo definida. *ownerType* é o tipo que fornece os métodos acessadores que são exigidos por um processador XAML para obter ou definir o valor da propriedade anexada.  
  
 O cenário mais comum para propriedades anexadas é habilitar que elementos filho relatem um valor da propriedade para o respectivo elemento pai.  
  
 O exemplo a seguir ilustra <xref:System.Windows.Controls.DockPanel.Dock%2A?displayProperty=nameWithType> a propriedade anexada. A <xref:System.Windows.Controls.DockPanel> classe define os acessadores <xref:System.Windows.Controls.DockPanel.Dock%2A?displayProperty=nameWithType> para e, portanto, possui a propriedade anexada. A <xref:System.Windows.Controls.DockPanel> classe também inclui uma lógica que itera seus elementos filho e verifica especificamente cada elemento em busca de um valor <xref:System.Windows.Controls.DockPanel.Dock%2A?displayProperty=nameWithType>definido de. Se um valor for encontrado, esse valor será usado durante o layout para posicionar os elementos filho. O uso da <xref:System.Windows.Controls.DockPanel.Dock%2A?displayProperty=nameWithType> Propriedade anexada e esse recurso de posicionamento é, na verdade, o <xref:System.Windows.Controls.DockPanel> cenário motivador para a classe.  
  
 [!code-xaml[XAMLOvwSupport#DockAP](~/samples/snippets/csharp/VS_Snippets_Wpf/XAMLOvwSupport/CSharp/page8.xaml#dockap)]  
  
 Em [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)], a maioria ou todas as propriedades anexadas são implementadas como propriedades de dependência. Para obter detalhes, consulte [Visão geral das propriedades anexadas](attached-properties-overview.md).  
  
 Eventos anexados usam uma forma *ownerType*.*eventName* semelhante de sintaxe de atributo. Assim como nos eventos não anexados, o valor do atributo para um evento anexado em XAML especifica o nome do método manipulador que é invocado quando o evento é manipulado no elemento. Usos de evento anexado no XAML WPF são menos comuns. Para obter mais informações, consulte [Visão geral de eventos anexados](attached-events-overview.md).  
  
<a name="base_classes_and_xaml"></a>   
## <a name="base-types-and-xaml"></a>Tipos base e XAML  
 O WPF XAML subjacente e seu namespace XAML é uma coleção de tipos que correspondem a objetos CLR, além de elementos de marcação para XAML. No entanto, nem todas as classes podem ser mapeadas para elementos. Classes abstratas, como <xref:System.Windows.Controls.Primitives.ButtonBase>e determinadas classes de base não abstratas, são usadas para herança no modelo de objetos CLR. Classes base, incluindo aquelas abstratas, são importantes para o desenvolvimento de XAML, porque cada um dos elementos XAML concretos herda os membros de alguma classe base na sua hierarquia. Frequentemente esses membros incluem propriedades que podem ser definidas como atributos no elemento ou eventos que podem ser manipulados. <xref:System.Windows.FrameworkElement>é a classe base [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] concreta do [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] no nível da estrutura do WPF. Ao criar [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)], você usará várias classes de forma, painel, decorador ou controle, que derivam <xref:System.Windows.FrameworkElement>de. Uma classe base relacionada, <xref:System.Windows.FrameworkContentElement>, oferece suporte a elementos orientados a documentos que funcionam bem para uma apresentação de layout de fluxo, usando APIs que <xref:System.Windows.FrameworkElement>espelham as APIs deliberadamente. A combinação de atributos no nível de elemento e um modelo de objeto CLR fornece um conjunto de propriedades comuns que são configuráveis na maioria dos elementos XAML concretos, independentemente do elemento XAML específico e seu tipo subjacente.  
  
<a name="xaml_security"></a>   
## <a name="xaml-security"></a>Segurança XAML  
 XAML é uma linguagem de marcação que representa diretamente a instanciação e execução de objetos. Portanto, elementos criados em XAML têm a mesma capacidade que o código gerado equivalente no que se refere a interagir com recursos de sistema (acesso a rede e E/S do sistema de arquivos, por exemplo).  
  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]dá suporte à .NET Framework 4 CAS (segurança de acesso ao código) da estrutura de segurança. Isso significa que conteúdo do [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] em execução na zona da Internet tem permissões de execução reduzidas. "XAML avulso" (páginas de XAML não compilado interpretado na hora do carregamento por um visualizador XAML) e [!INCLUDE[TLA#tla_xbap](../../../../includes/tlasharptla-xbap-md.md)] geralmente são executados nesta zona da Internet e usam o mesmo conjunto de permissões.  No entanto, o XAML carregado em um aplicativo totalmente confiável tem o mesmo acesso aos recursos de sistema que o aplicativo host. Para obter mais informações, consulte [Segurança parcialmente confiável do WPF](../wpf-partial-trust-security.md).  
  
<a name="loading_xaml_from_code"></a>   
## <a name="loading-xaml-from-code"></a>Carregando XAML do código  
 XAML pode ser usado para definir toda a interface do usuário, mas às vezes também é apropriado definir apenas uma parte da interface do usuário em XAML. Essa funcionalidade pode ser usada para habilitar personalização parcial, armazenamento local de informações, uso do XAML para fornecer um objeto comercial ou uma variedade de cenários possíveis. A chave para esses cenários é a <xref:System.Windows.Markup.XamlReader> classe e seu <xref:System.Windows.Markup.XamlReader.Load%2A> método. A entrada é um arquivo XAML e a saída é um objeto que representa toda a árvore de objetos em tempo de execução que foi criada com base nessa marcação. Em seguida, você pode inserir o objeto para ser uma propriedade de outro objeto que já existe no aplicativo. Desde que a propriedade seja uma propriedade adequada no modelo de conteúdo que tem recursos de exibição e que notificará o mecanismo de execução de que novo conteúdo foi adicionado no aplicativo, você poderá modificar o conteúdo de um aplicativo em execução facilmente carregando em XAML. Observe que essa funcionalidade geralmente só está disponível em aplicativos de confiança total, devido às implicações de segurança óbvias de carregar arquivos em aplicativos enquanto estes são executados.  
  
<a name="whats_next"></a>   
## <a name="whats-next"></a>O que vem a seguir  
 Este tópico fornece uma introdução básica a conceitos de sintaxe XAML e terminologia como ela se aplica ao WPF. Para obter mais informações sobre os termos usados aqui, consulte [Sintaxe XAML em detalhes](xaml-syntax-in-detail.md).  
  
 Se você ainda não tiver feito isso, experimente o passo a passos no tópico [do tutorial: Meu primeiro aplicativo](../getting-started/walkthrough-my-first-wpf-desktop-application.md)de área de trabalho do WPF. Quando você criar o aplicativo centrado em marcação descrito pelo tutorial, o exercício ajudará a reforçar muitos dos conceitos descritos neste tópico.  
  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]usa um modelo de aplicativo específico baseado na <xref:System.Windows.Application> classe. Para obter detalhes, consulte [Visão geral do gerenciamento de aplicativo](../app-development/application-management-overview.md).  
  
 [Criando um aplicativo WPF](../app-development/building-a-wpf-application-wpf.md) fornece mais detalhes sobre como criar aplicativos que incluem XAML da linha de comando e com [!INCLUDE[TLA#tla_visualstu](../../../../includes/tlasharptla-visualstu-md.md)].  
  
 [Visão geral das propriedades de dependência](dependency-properties-overview.md) fornece mais informações sobre a versatilidade das propriedades no [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] e apresenta o conceito de propriedades de dependência.  
  
## <a name="see-also"></a>Consulte também

- [Sintaxe XAML em detalhes](xaml-syntax-in-detail.md)
- [XAML e classes personalizadas para WPF](xaml-and-custom-classes-for-wpf.md)
- [Namespace XAML (x:) Recursos de idioma](../../xaml-services/xaml-namespace-x-language-features.md)
- [Extensões XAML WPF](wpf-xaml-extensions.md)
- [Visão geral de elementos base](base-elements-overview.md)
- [Árvores no WPF](trees-in-wpf.md)
