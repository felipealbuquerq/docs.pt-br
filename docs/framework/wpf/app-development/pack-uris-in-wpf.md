---
title: URIs "pack://" no WPF
ms.date: 03/30/2017
helpviewer_keywords:
- pack URI scheme [WPF]
- loading embedded resources [WPF]
- URIs (Uniform Resource Identifiers)
- Uniform Resource Identifiers (URIs)
- loading non-resource files
- application management [WPF]
ms.assetid: 43adb517-21a7-4df3-98e8-09e9cdf764c4
ms.openlocfilehash: ad928fb223ce22c65bb86a78c7d4cd006651a2d5
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69950751"
---
# <a name="pack-uris-in-wpf"></a>URIs "pack://" no WPF

No Windows Presentation Foundation (WPF), [!INCLUDE[TLA#tla_uri#plural](../../../../includes/tlasharptla-urisharpplural-md.md)] o é usado para identificar e carregar arquivos de várias maneiras, incluindo o seguinte:

- Especificando o [!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)] a ser mostrado quando um aplicativo é iniciado pela primeira vez.

- Carregar imagens.

- Navegar até as páginas.

- Carregar arquivos de dados não executáveis.

Além disso [!INCLUDE[TLA2#tla_uri#plural](../../../../includes/tla2sharptla-urisharpplural-md.md)] , o pode ser usado para identificar e carregar arquivos de uma variedade de locais, incluindo o seguinte:

- O assembly atual.

- Um assembly referenciado.

- Um local relativo a um assembly.

- O site de origem do aplicativo.

Para fornecer um mecanismo consistente para identificar e carregar esses tipos de arquivos desses locais, [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] o aproveita a extensibilidade do esquema de *URI de pacote*. Este tópico fornece uma visão geral do esquema, aborda como construir o pacote [!INCLUDE[TLA2#tla_uri#plural](../../../../includes/tla2sharptla-urisharpplural-md.md)] para uma variedade de cenários, discute absoluto e relativo [!INCLUDE[TLA2#tla_uri#plural](../../../../includes/tla2sharptla-urisharpplural-md.md)] e [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] resolução, antes de mostrar como usar o pacote [!INCLUDE[TLA2#tla_uri#plural](../../../../includes/tla2sharptla-urisharpplural-md.md)] de ambas as marcações e o código.

<a name="The_Pack_URI_Scheme"></a>

## <a name="the-pack-uri-scheme"></a>O esquema de URI de pacote

O esquema [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] de pacote é usado pela especificação OPC ( [Open Packaging Conventions](https://go.microsoft.com/fwlink/?LinkID=71255) ), que descreve um modelo para organizar e identificar conteúdo. Os principais elementos desse modelo são pacotes e partes, onde um *pacote* é um contêiner lógico para uma ou mais *partes*lógicas. A imagem a seguir ilustra esse conceito.

![Diagrama de partes e pacote](./media/pack-uris-in-wpf/wpf-package-parts-diagram.png)

Para identificar partes, a especificação OPC aproveita a extensibilidade do RFC 2396 (Uniform Resource Identifiers (URI): Sintaxe genérica) para definir o esquema [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] de pacote.

O esquema especificado por um [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] é definido por seu prefixo; http, FTP e arquivo são exemplos bem conhecidos. O esquema [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] de pacote usa "Pack" como seu esquema e contém dois componentes: autoridade e caminho. A seguir está o formato de um pacote [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)].

pack://*authority*/*path*

A *autoridade* especifica o tipo de pacote no qual uma parte está contida, enquanto o *caminho* especifica o local de uma parte dentro de um pacote.

Esse conceito é ilustrado pela figura a seguir:

![Relação entre pacote, autoridade e caminho](./media/pack-uris-in-wpf/wpf-relationship-diagram.png)

Os pacotes e peças são análogos a aplicativos e arquivos, pois um aplicativo (pacote) pode incluir um ou mais arquivos (peças), incluindo:

- Arquivos de recursos que são compilados no assembly local.

- Arquivos de recursos que são compilados em um assembly referenciado.

- Arquivos de recursos que são compilados em um assembly de referência.

- Arquivos de conteúdo.

- Arquivos de site de origem.

Para acessar esses tipos de arquivos, [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] o dá suporte a duas autoridades: Application:///e siteoforigin:///. A autoridade application:/// identifica arquivos de dados de aplicativo que são conhecidos no tempo de compilação, incluindo arquivos de recurso e conteúdo. A autoridade siteoforigin:/// identifica arquivos de site de origem. O escopo de cada autoridade é mostrado na figura a seguir.

![Diagrama de URI de pacote](./media/pack-uris-in-wpf/wpf-pack-uri-scheme.png)

> [!NOTE]
> O componente de autoridade de um [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] pacote é um [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] inserido que aponta para um pacote e deve estar em conformidade com o RFC 2396. Além disso, o caractere “/” deve ser substituído pelo caractere “,”; caracteres reservados como “%” e “?” devem ser ignorados. Para ver os detalhes, consulte o OPC.

As seções a seguir explicam como construir [!INCLUDE[TLA2#tla_uri#plural](../../../../includes/tla2sharptla-urisharpplural-md.md)] o pacote usando essas duas autoridades em conjunto com os caminhos apropriados para identificar recursos, conteúdo e site de arquivos de origem.

<a name="Resource_File_Pack_URIs___Local_Assembly"></a>

## <a name="resource-file-pack-uris"></a>URIs de pacote de arquivo de recurso

Os arquivos de recurso são configurados como itens do MSBuild `Resource` e são compilados em assemblies. O WPF dá suporte à construção de URIs de pacote que podem ser usados para identificar arquivos de recursos que são compilados no assembly local ou compilados em um assembly que é referenciado do assembly local.

<a name="Local_Assembly_Resource_File"></a>

### <a name="local-assembly-resource-file"></a>Arquivo de recurso de assembly local

O pacote [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] para um arquivo de recurso que é compilado no assembly local usa a seguinte autoridade e caminho:

- **Autoridade**: application:///.

- **Caminho**: O nome do arquivo de recurso, incluindo seu caminho, relativo à raiz da pasta do projeto do assembly local.

O exemplo a seguir mostra o [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] pacote para [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] um arquivo de recurso que está localizado na raiz da pasta do projeto do assembly local.

`pack://application:,,,/ResourceFile.xaml`

O exemplo a seguir mostra o [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] pacote para [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] um arquivo de recurso que está localizado em uma subpasta da pasta do projeto do assembly local.

`pack://application:,,,/Subfolder/ResourceFile.xaml`

<a name="Resource_File_Pack_URIs___Referenced_Assembly"></a>

### <a name="referenced-assembly-resource-file"></a>Arquivo de recurso de assembly referenciado

O pacote [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] para um arquivo de recurso que é compilado em um assembly referenciado usa a seguinte autoridade e caminho:

- **Autoridade**: application:///.

- **Caminho**: O nome de um arquivo de recurso que é compilado em um assembly referenciado. O caminho deve estar de acordo com o seguinte formato:

  *AssemblyShortName*{ *;Version*]{ *;PublicKey*];component/*Path*

  - **AssemblyShortName**: o nome curto do assembly referenciado.

  - **;Version** [opcional]: a versão do assembly referenciado que contém o arquivo de recurso. É usada quando são carregados dois ou mais assemblies referenciados com o mesmo nome curto.

  - **;PublicKey** [opcional]: a chave pública que foi usada para assinar o assembly referenciado. É usada quando são carregados dois ou mais assemblies referenciados com o mesmo nome curto.

  - **;component**: especifica que o assembly referenciado é referenciado a partir do assembly local.

  - **/Path**: o nome do arquivo de recurso, incluindo o caminho, em relação à raiz da pasta de projeto do assembly referenciado.

O exemplo a seguir mostra o [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] pacote para [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] um arquivo de recurso que está localizado na raiz da pasta do projeto do assembly referenciado.

`pack://application:,,,/ReferencedAssembly;component/ResourceFile.xaml`

O exemplo a seguir mostra o [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] pacote para [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] um arquivo de recurso que está localizado em uma subpasta da pasta do projeto do assembly referenciado.

`pack://application:,,,/ReferencedAssembly;component/Subfolder/ResourceFile.xaml`

O exemplo a seguir mostra o [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] pacote para [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] um arquivo de recurso que está localizado na pasta raiz de uma pasta de projeto de assembly específica de versão referenciada.

`pack://application:,,,/ReferencedAssembly;v1.0.0.1;component/ResourceFile.xaml`

Observe que a sintaxe [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] do pacote para arquivos de recurso de assembly referenciados pode ser usada somente com a autoridade Application:///. Por exemplo, não há suporte para o seguinte [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)]no.

`pack://siteoforigin:,,,/SomeAssembly;component/ResourceFile.xaml`

<a name="Content_File_Pack_URIs"></a>

## <a name="content-file-pack-uris"></a>URIs de pacote de arquivo de conteúdo

O pacote [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] para um arquivo de conteúdo usa a seguinte autoridade e caminho:

- **Autoridade**: application:///.

- **Caminho**: O nome do arquivo de conteúdo, incluindo seu caminho relativo ao local do sistema de arquivos do assembly executável principal do aplicativo.

O exemplo a seguir mostra o [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] pacote para [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] um arquivo de conteúdo, localizado na mesma pasta que o assembly executável.

`pack://application:,,,/ContentFile.xaml`

O exemplo a seguir mostra o [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] pacote para [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] um arquivo de conteúdo, localizado em uma subpasta que é relativa ao assembly executável do aplicativo.

`pack://application:,,,/Subfolder/ContentFile.xaml`

> [!NOTE]
> Não é possível navegar para arquivos de conteúdo HTML. O [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] esquema dá suporte apenas à navegação para arquivos HTML que estão localizados no site de origem.

<a name="The_siteoforigin_____Authority"></a>

## <a name="site-of-origin-pack-uris"></a>URIs de pacote de site de origem

O pacote [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] para um arquivo de site de origem usa a seguinte autoridade e caminho:

- **Autoridade**: siteoforigin:///.

- **Caminho**: O nome do arquivo de site de origem, incluindo seu caminho relativo ao local do qual o assembly executável foi iniciado.

O exemplo a seguir mostra o [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] pacote para [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] um arquivo de site de origem, armazenado no local do qual o assembly executável é iniciado.

`pack://siteoforigin:,,,/SiteOfOriginFile.xaml`

O exemplo a seguir mostra o [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] pacote para [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] um arquivo de site de origem, armazenado na subpasta que é relativa ao local do qual o assembly executável do aplicativo é iniciado.

`pack://siteoforigin:,,,/Subfolder/SiteOfOriginFile.xaml`

<a name="Page_Files"></a>

## <a name="page-files"></a>Arquivos de paginação

Os arquivos XAML que são configurados como itens do MSBuild `Page` são compilados em assemblies da mesma maneira que os arquivos de recursos. Consequentemente, `Page` os itens do MSBuild podem ser identificados usando URIs de pacote para arquivos de recurso.

Os tipos de [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] arquivos que normalmente são configurados`Page` como itens do MSBuild têm um dos seguintes como seu elemento raiz:

- <xref:System.Windows.Window?displayProperty=nameWithType>

- <xref:System.Windows.Controls.Page?displayProperty=nameWithType>

- <xref:System.Windows.Navigation.PageFunction%601?displayProperty=nameWithType>

- <xref:System.Windows.ResourceDictionary?displayProperty=nameWithType>

- <xref:System.Windows.Documents.FlowDocument?displayProperty=nameWithType>

- <xref:System.Windows.Controls.UserControl?displayProperty=nameWithType>

<a name="Absolute_vs_Relative_Pack_URIs"></a>

## <a name="absolute-vs-relative-pack-uris"></a>Absoluto versus URIs de pacote relativas

Um pacote [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] totalmente qualificado inclui o esquema, a autoridade e o caminho, e é considerado um pacote [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)]absoluto. Como uma simplificação para os [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] desenvolvedores, os elementos normalmente permitem que você defina atributos apropriados com [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)]um pacote relativo, que inclui apenas o caminho.

Por exemplo, considere o seguinte pacote [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] absoluto para um arquivo de recurso no assembly local.

`pack://application:,,,/ResourceFile.xaml`

O pacote [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] relativo que se refere a esse arquivo de recurso seria o seguinte.

`/ResourceFile.xaml`

> [!NOTE]
> Como os arquivos de site de origem não estão associados a assemblies, eles só podem ser referenciados com [!INCLUDE[TLA2#tla_uri#plural](../../../../includes/tla2sharptla-urisharpplural-md.md)]o pacote absoluto.

Por padrão, um pacote [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] relativo é considerado relativo ao local da marcação ou código que contém a referência. No entanto, se uma barra invertida à esquerda for usada [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] , a referência de pacote relativa será considerada relativa à raiz do aplicativo. Considere, por exemplo, a estrutura de projeto a seguir.

`App.xaml`

`Page2.xaml`

`\SubFolder`

`+ Page1.xaml`

`+ Page2.xaml`

Se página1. XAML contiver [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] um que faça referência a \SubFolder\Page2.XAML *raiz*, a referência poderá usar o [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)]seguinte pacote relativo.

`Page2.xaml`

Se página1. XAML contiver [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] um que faça referência a \Page2.XAML *raiz*, a referência poderá usar o [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)]seguinte pacote relativo.

`/Page2.xaml`

<a name="Pack_URI_Resolution"></a>

## <a name="pack-uri-resolution"></a>Resolução de URI de pacote

O formato do pacote [!INCLUDE[TLA2#tla_uri#plural](../../../../includes/tla2sharptla-urisharpplural-md.md)] torna possível que o pacote [!INCLUDE[TLA2#tla_uri#plural](../../../../includes/tla2sharptla-urisharpplural-md.md)] para diferentes tipos de arquivos tenha a mesma aparência. Por exemplo, considere o seguinte pacote [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)]absoluto.

`pack://application:,,,/ResourceOrContentFile.xaml`

Esse pacote [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] absoluto pode se referir a um arquivo de recurso no assembly local ou um arquivo de conteúdo. O mesmo é verdadeiro para o seguinte relativo [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)].

`/ResourceOrContentFile.xaml`

Para determinar o tipo de arquivo ao qual um pacote [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] se refere, [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] o resolve [!INCLUDE[TLA2#tla_uri#plural](../../../../includes/tla2sharptla-urisharpplural-md.md)] os arquivos de recurso em assemblies locais e arquivos de conteúdo usando a seguinte heurística:

1. Investigue os metadados do assembly para <xref:System.Windows.Resources.AssemblyAssociatedContentFileAttribute> um atributo que corresponda ao [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)]pacote.

2. Se o <xref:System.Windows.Resources.AssemblyAssociatedContentFileAttribute> atributo for encontrado, o caminho do pacote [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] fará referência a um arquivo de conteúdo.

3. Se o <xref:System.Windows.Resources.AssemblyAssociatedContentFileAttribute> atributo não for encontrado, investigue os arquivos de recurso definidos que são compilados no assembly local.

4. Se um arquivo de recurso que corresponde ao caminho do pacote [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] for encontrado, o caminho do pacote [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] se referirá a um arquivo de recurso.

5. Se o recurso não for encontrado, o criado <xref:System.Uri> internamente será inválido.

[!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)]a resolução não se aplica [!INCLUDE[TLA2#tla_uri#plural](../../../../includes/tla2sharptla-urisharpplural-md.md)] ao que se refere ao seguinte:

- Arquivos de conteúdo em assemblies referenciados: esses tipos de arquivo não [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)]são suportados pelo.

- Arquivos inseridos em assemblies referenciados: [!INCLUDE[TLA2#tla_uri#plural](../../../../includes/tla2sharptla-urisharpplural-md.md)] que os identificam são exclusivos porque incluem o nome do assembly referenciado e o `;component` sufixo.

- Arquivos de site de origem [!INCLUDE[TLA2#tla_uri#plural](../../../../includes/tla2sharptla-urisharpplural-md.md)] : que os identificam são exclusivos porque são os únicos arquivos que podem ser identificados pelo [!INCLUDE[TLA2#tla_uri#plural](../../../../includes/tla2sharptla-urisharpplural-md.md)] pacote que contêm a autoridade siteoforigin:///.

Uma simplificação que [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] a resolução de pacote permite é que o código seja, de certa forma, independente dos locais dos arquivos de conteúdo e de recursos. Por exemplo, se você tiver um arquivo de recurso no assembly local que é reconfigurado para ser um arquivo de conteúdo, o pacote [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] para o recurso permanecerá o mesmo, assim como o código que usa o [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)]pacote.

<a name="Programming_with_Pack_URIs"></a>

## <a name="programming-with-pack-uris"></a>Programando com URIs de pacote

Muitas [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] classes implementam propriedades que podem ser definidas com [!INCLUDE[TLA2#tla_uri#plural](../../../../includes/tla2sharptla-urisharpplural-md.md)]o pacote, incluindo:

- <xref:System.Windows.Application.StartupUri%2A?displayProperty=nameWithType>

- <xref:System.Windows.Controls.Frame.Source%2A?displayProperty=nameWithType>

- <xref:System.Windows.Navigation.NavigationWindow.Source%2A?displayProperty=nameWithType>

- <xref:System.Windows.Documents.Hyperlink.NavigateUri%2A?displayProperty=nameWithType>

- <xref:System.Windows.Window.Icon%2A?displayProperty=nameWithType>

- <xref:System.Windows.Controls.Image.Source%2A?displayProperty=nameWithType>

Essas propriedades podem ser definidas a partir de marcação e código. Esta seção demonstra as construções básicas para ambos e, depois, mostra exemplos de cenários comuns.

<a name="Using_Pack_URIs_in_Markup"></a>

### <a name="using-pack-uris-in-markup"></a>Utilizando URIs de pacote em marcação

Um pacote [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] é especificado na marcação definindo o elemento de um atributo com o pacote [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)]. Por exemplo:

`<element attribute="pack://application:,,,/File.xaml" />`

A tabela 1 ilustra o vários pacotes [!INCLUDE[TLA2#tla_uri#plural](../../../../includes/tla2sharptla-urisharpplural-md.md)] absolutos que você pode especificar na marcação.

Tabela 1: URIs de pacote absolutos na marcação

|Arquivo|Pacote absoluto[!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)]|
|----------|-------------------------------------------------------------------------------------------------------------------------|
|Arquivo de recurso - assembly local|`"pack://application:,,,/ResourceFile.xaml"`|
|Arquivo de recurso em subpasta - assembly local|`"pack://application:,,,/Subfolder/ResourceFile.xaml"`|
|Arquivo de recurso - assembly referenciado|`"pack://application:,,,/ReferencedAssembly;component/ResourceFile.xaml"`|
|Arquivo de recurso em subpasta de assembly referenciado|`"pack://application:,,,/ReferencedAssembly;component/Subfolder/ResourceFile.xaml"`|
|Arquivo de recurso em assembly referenciado com controle de versão|`"pack://application:,,,/ReferencedAssembly;v1.0.0.0;component/ResourceFile.xaml"`|
|Arquivo de conteúdo|`"pack://application:,,,/ContentFile.xaml"`|
|Arquivo de conteúdo em subpasta|`"pack://application:,,,/Subfolder/ContentFile.xaml"`|
|Arquivo de site de origem|`"pack://siteoforigin:,,,/SOOFile.xaml"`|
|Arquivo de site de origem em subpasta|`"pack://siteoforigin:,,,/Subfolder/SOOFile.xaml"`|

A tabela 2 ilustra os vários pacotes [!INCLUDE[TLA2#tla_uri#plural](../../../../includes/tla2sharptla-urisharpplural-md.md)] relativos que você pode especificar na marcação.

Tabela 2: URIs de pacote relativas na marcação

|Arquivo|Pacote relativo[!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)]|
|----------|-------------------------------------------------------------------------------------------------------------------------|
|Arquivo de recurso em assembly local|`"/ResourceFile.xaml"`|
|Arquivo de recurso em subpasta de assembly local|`"/Subfolder/ResourceFile.xaml"`|
|Arquivo de recurso em assembly referenciado|`"/ReferencedAssembly;component/ResourceFile.xaml"`|
|Arquivo de recurso em subpasta de assembly referenciado|`"/ReferencedAssembly;component/Subfolder/ResourceFile.xaml"`|
|Arquivo de conteúdo|`"/ContentFile.xaml"`|
|Arquivo de conteúdo em subpasta|`"/Subfolder/ContentFile.xaml"`|

<a name="Using_Pack_URIs_in_Code"></a>

### <a name="using-pack-uris-in-code"></a>Utilizando URIs de pacote em código

Você especifica um pacote [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] no código instanciando a <xref:System.Uri> classe e passando o pacote [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] como um parâmetro para o construtor. Isso é demonstrado no exemplo a seguir.

```csharp
Uri uri = new Uri("pack://application:,,,/File.xaml");
```

Por padrão, a <xref:System.Uri> classe considera o [!INCLUDE[TLA2#tla_uri#plural](../../../../includes/tla2sharptla-urisharpplural-md.md)] pacote como absoluto. Consequentemente, uma exceção é gerada quando uma instância da <xref:System.Uri> classe é criada com um pacote [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)]relativo.

```csharp
Uri uri = new Uri("/File.xaml");
```

Felizmente, a <xref:System.Uri.%23ctor%28System.String%2CSystem.UriKind%29> sobrecarga <xref:System.Uri> do construtor da classe aceita um parâmetro do tipo <xref:System.UriKind> para permitir que você especifique se um pacote [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] é absoluto ou relativo.

```csharp
// Absolute URI (default)
Uri absoluteUri = new Uri("pack://application:,,,/File.xaml", UriKind.Absolute);
// Relative URI
Uri relativeUri = new Uri("/File.xaml",
                        UriKind.Relative);
```

Você deve especificar somente <xref:System.UriKind.Absolute> ou <xref:System.UriKind.Relative> quando tiver certeza de que o pacote [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] fornecido é um ou outro. Se você não souber o tipo de pacote [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] usado, como quando um usuário insere um pacote [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] em tempo de execução, use <xref:System.UriKind.RelativeOrAbsolute> em vez disso.

```csharp
// Relative or Absolute URI provided by user via a text box
TextBox userProvidedUriTextBox = new TextBox();
Uri uri = new Uri(userProvidedUriTextBox.Text, UriKind.RelativeOrAbsolute);
```

A tabela 3 ilustra os vários pacotes [!INCLUDE[TLA2#tla_uri#plural](../../../../includes/tla2sharptla-urisharpplural-md.md)] relativos que você pode especificar no código usando <xref:System.Uri?displayProperty=nameWithType>.

Tabela 3: URIs de pacote absoluto no código

|Arquivo|Pacote absoluto[!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)]|
|----------|-------------------------------------------------------------------------------------------------------------------------|
|Arquivo de recurso - assembly local|`Uri uri = new Uri("pack://application:,,,/ResourceFile.xaml", UriKind.Absolute);`|
|Arquivo de recurso em subpasta - assembly local|`Uri uri = new Uri("pack://application:,,,/Subfolder/ResourceFile.xaml", UriKind.Absolute);`|
|Arquivo de recurso - assembly referenciado|`Uri uri = new Uri("pack://application:,,,/ReferencedAssembly;component/ResourceFile.xaml", UriKind.Absolute);`|
|Arquivo de recurso em subpasta de assembly referenciado|`Uri uri = new Uri("pack://application:,,,/ReferencedAssembly;component/Subfolder/ResourceFile.xaml", UriKind.Absolute);`|
|Arquivo de recurso em assembly referenciado com controle de versão|`Uri uri = new Uri("pack://application:,,,/ReferencedAssembly;v1.0.0.0;component/ResourceFile.xaml", UriKind.Absolute);`|
|Arquivo de conteúdo|`Uri uri = new Uri("pack://application:,,,/ContentFile.xaml", UriKind.Absolute);`|
|Arquivo de conteúdo em subpasta|`Uri uri = new Uri("pack://application:,,,/Subfolder/ContentFile.xaml", UriKind.Absolute);`|
|Arquivo de site de origem|`Uri uri = new Uri("pack://siteoforigin:,,,/SOOFile.xaml", UriKind.Absolute);`|
|Arquivo de site de origem em subpasta|`Uri uri = new Uri("pack://siteoforigin:,,,/Subfolder/SOOFile.xaml", UriKind.Absolute);`|

A tabela 4 ilustra os vários pacotes [!INCLUDE[TLA2#tla_uri#plural](../../../../includes/tla2sharptla-urisharpplural-md.md)] relativos que você pode especificar no código <xref:System.Uri?displayProperty=nameWithType>usando.

Tabela 4: URIs de pacote relativas no código

|Arquivo|Pacote relativo[!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)]|
|----------|-------------------------------------------------------------------------------------------------------------------------|
|Arquivo de recurso - assembly local|`Uri uri = new Uri("/ResourceFile.xaml", UriKind.Relative);`|
|Arquivo de recurso em subpasta - assembly local|`Uri uri = new Uri("/Subfolder/ResourceFile.xaml", UriKind.Relative);`|
|Arquivo de recurso - assembly referenciado|`Uri uri = new Uri("/ReferencedAssembly;component/ResourceFile.xaml", UriKind.Relative);`|
|Arquivo de recurso em subpasta - assembly referenciado|`Uri uri = new Uri("/ReferencedAssembly;component/Subfolder/ResourceFile.xaml", UriKind.Relative);`|
|Arquivo de conteúdo|`Uri uri = new Uri("/ContentFile.xaml", UriKind.Relative);`|
|Arquivo de conteúdo em subpasta|`Uri uri = new Uri("/Subfolder/ContentFile.xaml", UriKind.Relative);`|

<a name="Common_Pack_URI_Scenarios"></a>

### <a name="common-pack-uri-scenarios"></a>Cenários comuns de URI de pacote

As seções anteriores discutiram como construir o Pack [!INCLUDE[TLA2#tla_uri#plural](../../../../includes/tla2sharptla-urisharpplural-md.md)] para identificar recursos, conteúdo e site de arquivos de origem. No [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)], essas construções são usadas de várias maneiras, e as seções a seguir abrangem vários usos comuns.

<a name="Specifying_the_UI_to_Show_when_an_Application_Starts"></a>

#### <a name="specifying-the-ui-to-show-when-an-application-starts"></a>Especificando a interface do usuário para mostrar quando um aplicativo foi iniciado

<xref:System.Windows.Application.StartupUri%2A>Especifica o primeiro [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] a ser mostrado quando [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] um aplicativo é iniciado. Para aplicativos autônomos, [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] o pode ser uma janela, conforme mostrado no exemplo a seguir.

[!code-xaml[PackURIOverviewSnippets#StartupUriWindow](~/samples/snippets/csharp/VS_Snippets_Wpf/PackURIOverviewSnippets/CS/Copy of App.xaml#startupuriwindow)]

Aplicativos autônomos [!INCLUDE[TLA#tla_xbap#plural](../../../../includes/tlasharptla-xbapsharpplural-md.md)] e também podem especificar uma página como a interface do usuário inicial, conforme mostrado no exemplo a seguir.

[!code-xaml[PackURIOverviewSnippets#StartupUriPage](~/samples/snippets/csharp/VS_Snippets_Wpf/PackURIOverviewSnippets/CS/App.xaml#startupuripage)]

Se o aplicativo for um aplicativo autônomo e uma página for especificada com <xref:System.Windows.Application.StartupUri%2A>, [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] o abrirá um <xref:System.Windows.Navigation.NavigationWindow> para hospedar a página. Para [!INCLUDE[TLA2#tla_xbap#plural](../../../../includes/tla2sharptla-xbapsharpplural-md.md)]o, a página é mostrada no navegador de host.

<a name="Navigating_to_a_Page"></a>

#### <a name="navigating-to-a-page"></a>Navegando até uma página

O exemplo a seguir mostra como navegar até uma página.

[!code-xaml[NavigationOverviewSnippets#HyperlinkXAML1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithHyperlink.xaml#hyperlinkxaml1)]
[!code-xaml[NavigationOverviewSnippets#HyperlinkXAML2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithHyperlink.xaml#hyperlinkxaml2)]
[!code-xaml[NavigationOverviewSnippets#HyperlinkXAML3](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithHyperlink.xaml#hyperlinkxaml3)]

Para obter mais informações sobre as várias maneiras de navegar [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)]no, consulte [visão geral de navegação](navigation-overview.md).

<a name="Specifying_a_Window_Icon"></a>

#### <a name="specifying-a-window-icon"></a>Especificando um ícone de janela

O exemplo a seguir mostra como usar um URI para especificar um ícone de janela.

[!code-xaml[WindowIconSnippets#WindowIconSetXAML](~/samples/snippets/xaml/VS_Snippets_Wpf/WindowIconSnippets/XAML/MainWindow.xaml#windowiconsetxaml)]

Para obter mais informações, consulte <xref:System.Windows.Window.Icon%2A>.

<a name="Loading_Image__Audio__and_Video_Files"></a>

#### <a name="loading-image-audio-and-video-files"></a>Carregando arquivos de imagem, áudio e vídeo

[!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)]permite que os aplicativos usem uma ampla variedade de tipos de mídia, todos os quais podem ser identificados e carregados [!INCLUDE[TLA2#tla_uri#plural](../../../../includes/tla2sharptla-urisharpplural-md.md)]com o pacote, conforme mostrado nos exemplos a seguir.

[!code-xaml[MediaPlayerVideoSample#VideoPackURIAtSOO](~/samples/snippets/csharp/VS_Snippets_Wpf/MediaPlayerVideoSample/CS/HomePage.xaml#videopackuriatsoo)]

[!code-xaml[MediaPlayerAudioSample#AudioPackURIAtSOO](~/samples/snippets/csharp/VS_Snippets_Wpf/MediaPlayerAudioSample/CS/HomePage.xaml#audiopackuriatsoo)]

[!code-xaml[ImageSample#ImagePackURIContent](~/samples/snippets/csharp/VS_Snippets_Wpf/ImageSample/CS/HomePage.xaml#imagepackuricontent)]

Para obter mais informações sobre como trabalhar com conteúdo de mídia, consulte [gráficos e multimídia](../graphics-multimedia/index.md).

<a name="Loading_a_Resource_Dictionary_from_the_Site_of_Origin"></a>

#### <a name="loading-a-resource-dictionary-from-the-site-of-origin"></a>Carregando um dicionário de recursos a partir do site de origem

Os dicionários<xref:System.Windows.ResourceDictionary>de recursos () podem ser usados para dar suporte a temas de aplicativo. Uma maneira de criar e gerenciar temas é criando diferentes temas como dicionários de recursos localizados em um site de origem do aplicativo. Isso permite que temas sejam adicionados e atualizados sem recompilar e reimplantar um aplicativo. Esses dicionários de recursos podem ser identificados e carregados [!INCLUDE[TLA2#tla_uri#plural](../../../../includes/tla2sharptla-urisharpplural-md.md)]usando o pacote, que é mostrado no exemplo a seguir.

[!code-xaml[ResourceDictionarySnippets#ResourceDictionaryPackURI](~/samples/snippets/csharp/VS_Snippets_Wpf/ResourceDictionarySnippets/CS/App.xaml#resourcedictionarypackuri)]

Para obter uma visão geral dos [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)]temas no, consulte [estilização e modelagem](../controls/styling-and-templating.md).

## <a name="see-also"></a>Consulte também

- [Arquivos de recursos, de conteúdo e de dados de aplicativos do WPF](wpf-application-resource-content-and-data-files.md)
