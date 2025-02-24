---
title: Extensões de marcação e XAML WPF
ms.date: 03/30/2017
helpviewer_keywords:
- brace character [WPF]
- Binding markup extensions [WPF]
- RelativeSource markup extensions [WPF]
- XAML [WPF], markup extensions
- markup extensions [WPF]
- nesting extension syntax [WPF]
- curly brace characters ({})
- TemplateBinding markup extensions [WPF]
- StaticResource markup extensions [WPF]
- literal curly brace characters ({})
- characters [WPF], curly brace
- DynamicResource markup extensions [WPF]
ms.assetid: 618dc745-8b14-4886-833f-486d2254bb78
ms.openlocfilehash: f0eb4a90b09f49ced45fa8453356e1d6fb3b4af1
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69965274"
---
# <a name="markup-extensions-and-wpf-xaml"></a>Extensões de marcação e XAML WPF
Este tópico apresenta o conceito de extensões de marcação para XAML, incluindo regras de sintaxe, finalidade e o modelo de objeto de classe subjacente. As extensões de marcação são um recurso geral da linguagem XAML e da implementação .NET de serviços XAML. Este tópico detalha, especificamente, as extensões de marcação para uso em XAML do WPF.  

<a name="XAML_Processors_and_Markup_Extensions"></a>   
## <a name="xaml-processors-and-markup-extensions"></a>Processadores XAML e extensões de marcação  
 De modo geral, um analisador XAML pode interpretar um valor de atributo como cadeia de caracteres literal que pode ser convertida em um primitivo ou convertê-lo em objeto por algum meio. Um desses meios é fazendo referência a um conversor de tipo; isso é documentado no tópico [TypeConverters e XAML](typeconverters-and-xaml.md). No entanto, há cenários em que outro comportamento é necessário. Por exemplo, um processador XAML pode ser instruído de que um valor de um atributo não deve resultar em um novo objeto no grafo de objeto. Em vez disso, o atributo deve resultar em um grafo de objeto que faz referência a um objeto já construído em outra parte do grafo ou em um objeto estático. Outro cenário é que um processador XAML pode ser instruído a usar uma sintaxe que fornece argumentos não padrão para o construtor de um objeto. Estes são os tipos de cenários em que uma extensão de marcação pode fornecer a solução.  
  
<a name="Basic_Markup_Extension_Syntax"></a>   
## <a name="basic-markup-extension-syntax"></a>Sintaxe de extensão de marcação básica  
 Uma extensão de marcação pode ser implementada para fornecer valores para propriedades em uso de atributo e/ou para propriedades em uso de elemento de propriedade.  
  
 Quando é usada para fornecer um valor de atributo, a sintaxe que distingue uma sequência de extensão de marcação para um processador XAML é a presença das chaves de abertura e fechamento ({ e }). Em seguida, o tipo de extensão de marcação é identificado pelo token de cadeia de caracteres imediatamente após a chave de abertura.  
  
 Quando usada na sintaxe de elemento de propriedade, uma extensão de marcação é visualmente igual a qualquer outro elemento utilizado para fornecer um valor de elemento de propriedade: uma declaração do elemento XAML que faz referência à classe de extensão de marcação como um elemento, entre colchetes angulares (<>).  
  
<a name="XAML_Defined_Markup_Extensions"></a>   
## <a name="xaml-defined-markup-extensions"></a>Extensões de marcação definidas de XAML  
 Existem várias extensões de marcação que não são específicas à implementação de XAML do WPF, mas que são implementações de intrínsecos ou recursos de XAML como linguagem. Essas extensões de marcação são implementadas no assembly System.Xaml como parte dos serviços de XAML gerais do .NET Framework XAML e ficam dentro do namespace de XAML de linguagem XAML. Em termos de uso de marcação comum, essas extensões de marcação normalmente podem ser identificadas pelo prefixo `x:` no uso. A <xref:System.Windows.Markup.MarkupExtension> classe base (também definida em System. XAML) fornece o padrão que todas as extensões de marcação devem usar para serem suportadas em leitores XAML e gravadores XAML, incluindo no WPF XAML.  
  
- `x:Type`fornece o <xref:System.Type> objeto para o tipo nomeado. Esse recurso é usado com mais frequência em estilos e modelos. Para ver os detalhes, consulte [Extensão de marcação x:Type](../../xaml-services/x-type-markup-extension.md).  
  
- O `x:Static` produz valores estáticos. Os valores vêm de entidades de código de tipo de valor que não são, diretamente, o tipo de valor de uma propriedade de destino, mas podem ser avaliados para esse tipo. Para ver os detalhes, consulte [Extensão de marcação x:Static](../../xaml-services/x-static-markup-extension.md).  
  
- O `x:Null` especifica `null` como um valor para uma propriedade e pode ser utilizado para atributos ou valores de elemento de propriedade. Para ver os detalhes, consulte [Extensão de marcação x:Null](../../xaml-services/x-null-markup-extension.md).  
  
- O `x:Array` fornece suporte para a criação de matrizes gerais na sintaxe XAML, para casos em que o suporte da coleção oferecido por modelos de controle e elementos base do WPF não é usado deliberadamente. Para ver os detalhes, consulte [Extensão de marcação x:Array](../../xaml-services/x-array-markup-extension.md).  
  
> [!NOTE]
> O prefixo `x:` é utilizado para o mapeamento típico do namespace de XAML dos intrínsecos da linguagem XAML, no elemento raiz de uma produção ou arquivo XAML. Por exemplo, os modelos do Visual Studio para aplicativos do WPF iniciam um arquivo `x:` XAML usando esse mapeamento. Você poderia escolher um token de prefixo diferente no seu próprio mapeamento do namespace de XAML; porém, essa documentação assumirá o mapeamento padrão do `x:` como meio para identificar as entidades que são uma parte definida do namespace de XAML para a linguagem XAML, ao contrário do namespace padrão do WPF ou de outros namespaces XAML não relacionados a uma estrutura específica.  
  
<a name="WPF_Specific_Markup_Extensions"></a>   
## <a name="wpf-specific-markup-extensions"></a>Extensões de marcação específicas do WPF  
 As extensões de marcação mais comuns usadas na programação do WPF são aquelas que dão suporte a referências de recurso (`StaticResource` e `DynamicResource`) e aquelas que dão suporte à vinculação de dados (`Binding`).  
  
- O `StaticResource` fornece um valor para uma propriedade ao substituir o valor de um recurso já definido. Por fim, uma avaliação do `StaticResource` é feita no tempo de carregamento do XAML e não tem acesso ao grafo de objeto no tempo de execução. Para ver os detalhes, consulte [Extensão de marcação StaticResource](staticresource-markup-extension.md).  
  
- O `DynamicResource` fornece um valor para uma propriedade ao adiar esse valor para ser uma referência de tempo de execução a um recurso. Uma referência de recurso dinâmico força uma nova pesquisa sempre que tal recurso é acessado e tem acesso ao grafo de objeto no tempo de execução. Para obter esse acesso, o conceito do `DynamicResource` tem suporte por propriedades de dependência no sistema de propriedade do WPF, bem como por expressões avaliadas. Portanto, o `DynamicResource` pode ser usado somente para um destino de propriedade de dependência. Para ver os detalhes, consulte [Extensão de marcação DynamicResource](dynamicresource-markup-extension.md).  
  
- O `Binding` fornece um valor associado a dados para uma propriedade, usando o contexto de dados que se aplica ao objeto pai no tempo de execução. Essa extensão de marcação é relativamente complexa, porque possibilita uma sintaxe substancial embutida para especificar uma vinculação de dados. Para ver os detalhes, consulte [Extensão de marcação Binding](binding-markup-extension.md).  
  
- `RelativeSource`fornece informações de origem para <xref:System.Windows.Data.Binding> um que pode navegar por várias relações possíveis na árvore de objetos de tempo de execução. São oferecidas fontes especializadas para associações criadas em modelos multiuso ou criadas em código sem conhecimento completo da árvore de objetos adjacente. Para ver os detalhes, consulte [RelativeSource MarkupExtension](relativesource-markupextension.md).  
  
- O `TemplateBinding` permite que um modelo de controle use valores para propriedades modeladas que vêm de propriedades definidas pelo modelo de objeto da classe que usará o modelo. Em outras palavras, a propriedade dentro da definição de modelo pode acessar um contexto que existe somente depois que o modelo é aplicado. Para ver os detalhes, consulte [Extensão de marcação TemplateBinding](templatebinding-markup-extension.md). Para obter mais informações sobre o uso prático de `TemplateBinding`, consulte [Estilos com a amostra ControlTemplates](https://github.com/Microsoft/WPF-Samples/tree/master/Styles%20&%20Templates/IntroToStylingAndTemplating).  
  
- O `ColorConvertedBitmap` dá suporte a um cenário relativamente avançado de geração de imagens. Para ver os detalhes, consulte [Extensão de marcação ColorConvertedBitmap](colorconvertedbitmap-markup-extension.md).  
  
- O `ComponentResourceKey` e o `ThemeDictionary` dão suporte a aspectos da pesquisa de recursos, especialmente para recursos e temas empacotados com controles personalizados. Para obter mais informações, consulte [Extensão de marcação ComponentResourceKey](componentresourcekey-markup-extension.md), [Extensão de marcação ThemeDictionary](themedictionary-markup-extension.md) ou [Visão geral de criação do controle](../controls/control-authoring-overview.md).  
  
<a name="StarExtension"></a>   
## <a name="extension-classes"></a>\*Classes de extensão  
 Para a linguagem XAML geral e extensões de marcação específicas do WPF, o comportamento de cada extensão de marcação é identificado para um processador XAML por `*Extension` meio de uma classe derivada <xref:System.Windows.Markup.MarkupExtension>de e <xref:System.Windows.Markup.StaticExtension.ProvideValue%2A> fornece uma implementação do forma. Esse método em cada extensão fornece o objeto que é retornado após a avaliação da extensão de marcação. Normalmente, o objeto retornado é avaliado com base em vários tokens de cadeia de caracteres que são passados para a extensão de marcação.  
  
 Por exemplo, a <xref:System.Windows.StaticResourceExtension> classe fornece a implementação de superfície da pesquisa de recurso real para <xref:System.Windows.Markup.MarkupExtension.ProvideValue%2A> que sua implementação retorne o objeto solicitado, com a entrada dessa implementação específica sendo uma cadeia de caracteres que é usada para Pesquise o recurso por seu `x:Key`. A maior parte dos detalhes da implementação não será importante se você estiver usando uma extensão de marcação existente.  
  
 Algumas extensões de marcação não usam argumentos de token de cadeia de caracteres. Isso ocorre porque retornam um valor estático ou consistente ou porque o contexto para o valor que deve ser retornado está disponível por meio de um dos serviços passados através do parâmetro `serviceProvider`.  
  
 O padrão de nomenclatura do `*Extension` é para conveniência e consistência. Não é necessário para que um processador XAML possa identificar essa classe como suporte para uma extensão de marcação. Contanto que sua codebase inclua System. XAML e use .NET Framework implementações de serviços XAML, tudo o que é necessário para ser reconhecido como uma extensão de marcação XAML <xref:System.Windows.Markup.MarkupExtension> é derivar de e para dar suporte a uma sintaxe de construção. O WPF define a extensão de marcação – habilitando classes que `*Extension` não seguem o padrão de <xref:System.Windows.Data.Binding>nomenclatura, por exemplo. Normalmente, o motivo disso é que a classe dá suporte a cenários além do suporte à extensão de marcação puro. No caso do <xref:System.Windows.Data.Binding>, essa classe dá suporte ao acesso em tempo de execução para métodos e propriedades do objeto em cenários que não têm nada a ver com XAML.  
  
### <a name="extension-class-interpretation-of-initialization-text"></a>Interpretação de classe de extensão do texto de inicialização  
 Os tokens de cadeia de caracteres após o nome da extensão de marcação e ainda dentro de chaves são interpretados pelo processador XAML em uma das seguintes maneiras:  
  
- Uma vírgula sempre representa o separador ou delimitador de tokens individuais.  
  
- Se os tokens individuais separados não contiverem nenhum sinal de igual, cada token será tratado como um argumento do construtor. Cada parâmetro de construtor deve ser fornecido como o tipo esperado por essa assinatura e na ordem correta esperada por ela.  
  
    > [!NOTE]
    > Um processador XAML deve chamar o construtor que corresponde à contagem de argumento do número de pares. Por esse motivo, se você estiver implementando uma extensão de marcação personalizada, não forneça vários construtores com a mesma contagem de argumentos. O comportamento de um processador XAML se existir mais de um caminho do construtor de extensão de marcação com a mesma contagem de parâmetros não é definido, mas você deverá prever que um processador XAML está autorizado a lançar uma exceção em uso caso tal situação exista nas definições do tipo de extensão de marcação.  
  
- Se os tokens separados individuais contiverem sinais de igual, um processador XAML primeiro chamará o construtor sem parâmetros para a extensão de marcação. A seguir, cada par name=value será interpretado como um nome de propriedade que existe na extensão de marcação e um valor para designar à propriedade.  
  
- Se houver um resultado paralelo entre o comportamento do construtor e o comportamento de configuração da propriedade em uma extensão de marcação, não importará qual comportamento será utilizado. O mais comum é usar os pares *property*`=`*value* para extensões de marcação que têm mais de uma propriedade configurável, pelo menos porque torna a marcação mais intencional e diminui a probabilidade de transpor acidentalmente os parâmetros do construtor. (Quando pares property=value são especificados, as propriedades poderão ficar em qualquer ordem.) Além disso, não há nenhuma garantia de que uma extensão de marcação fornecerá um parâmetro do construtor que defina cada uma das propriedades configuráveis. Por exemplo, <xref:System.Windows.Data.Binding> é uma extensão de marcação, com muitas propriedades que são configuradas por meio da extensão no formulário de <xref:System.Windows.Data.Binding> *valor* de *Propriedade*`=`, mas dá suporte apenas a dois construtores: um construtor sem parâmetros e um Isso define um caminho inicial.  
  
- Uma vírgula literal não pode ser passada para uma extensão de marcação sem escape.  
  
<a name="EscapeSequences"></a>   
## <a name="escape-sequences-and-markup-extensions"></a>Sequências de escape e extensões de marcação  
 A manipulação de atributos em um processador XAML utiliza as chaves como indicadores de uma sequência de extensão de marcação. Também é possível produzir um valor de atributo de caracteres de chave literais, se necessário, digitando uma sequência de escape usando um par de chaves vazias seguido da chave literal. [ Consulte{} sequência de escape – extensão de marcação](../../xaml-services/escape-sequence-markup-extension.md).  
  
<a name="Nesting"></a>   
## <a name="nesting-markup-extensions-in-xaml-usage"></a>Aninhamento de extensões de marcação no uso do XAML  
 Há suporte para o aninhamento de várias extensões de marcação; cada extensão de marcação será avaliada detalhadamente em primeiro lugar. Considere, por exemplo, o uso a seguir:  
  
```  
<Setter Property="Background"  
  Value="{DynamicResource {x:Static SystemColors.ControlBrushKey}}" />  
```  
  
 Nesse uso, o demonstrativo `x:Static` é avaliado em primeiro lugar e retorna uma cadeia de caracteres. Em seguida, essa cadeia de caracteres é utilizada como argumento para `DynamicResource`.  
  
## <a name="markup-extensions-and-property-element-syntax"></a>Extensões de marcação e sintaxe de elemento de propriedade  
 Quando usada como um elemento de objeto que preenche um valor de elemento de propriedade, uma classe de extensão de marcação é visualmente indistinguível de um elemento de objeto do tipo típico que pode ser usado em XAML. A diferença prática entre um elemento de objeto típico e uma extensão de marcação é que a extensão de marcação é avaliada como um valor tipado ou diferida como uma expressão. Portanto, os mecanismos para todos os possíveis erros de tipo de valores da propriedade para a extensão de marcação serão diferentes, de modo semelhante a como uma propriedade de associação tardia é tratada em outros modelos de programação. Um elemento de objeto comum será avaliado para correspondência de tipo com relação à propriedade de destino definida quando o XAML é analisado.  
  
 Quando usadas na sintaxe do elemento de objeto para preencher um elemento de propriedade, a maioria das extensões de marcação não teria conteúdo ou nenhuma sintaxe de elemento de propriedade adicional em seu interior. Portanto, a marca do elemento de objeto seria fechada e não forneceria nenhum elemento filho. Sempre que algum elemento de objeto é encontrado por um processador XAML, o construtor dessa classe é chamado, o que instancia o objeto criado do elemento analisado. Uma classe de extensão de marcação não é diferente: se você quiser que sua extensão de marcação seja utilizável na sintaxe do elemento de objeto, você deve fornecer um construtor sem parâmetros. Algumas extensões de marcação existentes têm pelo menos um valor da propriedade necessário que deve ser especificado para uma inicialização eficaz. Nesse caso, o valor da propriedade normalmente é fornecido como um atributo de propriedade no elemento de objeto. No namespace [XAML (x:) Os recursos](../../xaml-services/xaml-namespace-x-language-features.md) de linguagem e as páginas de referência de [extensões XAML do WPF](wpf-xaml-extensions.md) , extensões de marcação que têm propriedades obrigatórias (e os nomes das propriedades obrigatórias) serão indicadas. As páginas de referência também indicarão se alguma sintaxe de elemento de objeto ou alguma sintaxe de atributo não está autorizada para extensões de marcação específicas. Um caso notável é a [Extensão de marcação x:Array](../../xaml-services/x-array-markup-extension.md), que não pode dar suporte à sintaxe do atributo, porque o conteúdo dessa matriz deve ser especificado dentro da marcação como conteúdo. Como o conteúdo da matriz é manipulado como objetos gerais, nenhum conversor de tipo padrão para o atributo é viável. Além disso, a [Extensão de marcação x:Array](../../xaml-services/x-array-markup-extension.md) exige um parâmetro `type`.  
  
## <a name="see-also"></a>Consulte também

- [Visão geral de XAML (WPF)](xaml-overview-wpf.md)
- [Namespace XAML (x:) Recursos de idioma](../../xaml-services/xaml-namespace-x-language-features.md)
- [Extensões XAML WPF](wpf-xaml-extensions.md)
- [Extensão de marcação StaticResource](staticresource-markup-extension.md)
- [Extensão de marcação de associação](binding-markup-extension.md)
- [Extensão de marcação DynamicResource](dynamicresource-markup-extension.md)
- [Extensão de marcação x:Type](../../xaml-services/x-type-markup-extension.md)
