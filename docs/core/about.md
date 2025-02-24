---
title: Sobre o .NET Core
description: Saiba mais sobre o .NET Core.
author: richlander
ms.date: 08/01/2018
ms.openlocfilehash: ea9253bacf2bcee63430cd45f2a9ed412ce629e7
ms.sourcegitcommit: 205b9a204742e9c77256d43ac9d94c3f82909808
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70849131"
---
# <a name="about-net-core"></a>Sobre o .NET Core

O .NET Core tem as seguintes características:

- **Multiplataforma:** Executado nos [sistemas operacionais](https://github.com/dotnet/core/blob/master/os-lifecycle-policy.md) Windows, macOS e Linux.
- **Consistente entre arquiteturas:** Executa o código com o mesmo comportamento em várias arquiteturas, incluindo x64, x86 e ARM.
- **Ferramentas de linha de comando:**  Inclui ferramentas de linha de comando fáceis de usar, para desenvolvimento local e em cenários de integração contínua.
- **Implantação flexível:** Pode ser incluído no aplicativo ou instalado lado a lado (instalações de todo o usuário ou de todo o sistema). Pode ser usado com os [contêineres do Docker](docker/index.md).
- **Compatível:** o .NET Core é compatível com o .NET Framework, o Xamarin e o Mono por meio do [.NET Standard](../standard/net-standard.md).
- **Software livre:** A plataforma .NET Core é um software livre que usa licenças do MIT e Apache 2. O .NET Core é um projeto do [.NET Foundation](https://dotnetfoundation.org/).
- **Suporte da Microsoft:** a Microsoft dá suporte ao .NET Core por meio do [Suporte do .NET Core](https://dotnet.microsoft.com/platform/support/policy).

## <a name="languages"></a>Linguagens

As linguagens C#, Visual Basic e F# podem ser usadas para escrever aplicativos e bibliotecas para o .NET Core. Essas linguagens são ou podem ser integradas aos seus editores de texto e IDEs favoritos, incluindo [Visual Studio](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link), [Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp), Sublime Text e Vim. Essa integração é fornecida, em parte, pelas equipes dos projetos [OmniSharp](https://www.omnisharp.net/) e [Ionide](http://ionide.io).

## <a name="apis"></a>APIs

O .NET Core expõe APIs para vários cenários, incluindo:

- Tipos primitivos, como [bool](../csharp/language-reference/keywords/bool.md) e [int](../csharp/language-reference/builtin-types/integral-numeric-types.md).
- Coleções, como <xref:System.Collections.Generic.List%601?displayProperty=nameWithType> e <xref:System.Collections.Generic.Dictionary%602?displayProperty=nameWithType>.
- Tipos de utilitário, como <xref:System.Net.Http.HttpClient?displayProperty=nameWithType> e <xref:System.IO.FileStream?displayProperty=nameWithType>.
- Tipos de dados, como <xref:System.Data.DataSet?displayProperty=nameWithType>, e [DbSet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/).
- Tipos de alto desempenho, como <xref:System.Numerics.Vector?displayProperty=nameWithType> e [Pipelines](https://devblogs.microsoft.com/dotnet/system-io-pipelines-high-performance-io-in-net/).

O .NET Core oferece compatibilidade com as APIs do .NET Framework e do Mono por meio da implementação da especificação [.NET Standard](../standard/net-standard.md).

## <a name="frameworks"></a>Estruturas

Várias estruturas já foram criadas com base no .NET Core:

- [ASP.NET Core](/aspnet/core/)
- [Plataforma Universal do Windows (UWP) do Windows 10](https://developer.microsoft.com/windows)
- [Tizen](https://developer.tizen.org/development/training/.net-application)

## <a name="composition"></a>Composição

O .NET Core é composto pelas seguintes partes:

- Um [tempo de execução do .NET Core](https://github.com/dotnet/coreclr) que oferece um sistema de tipos, carregamento de assembly, um coletor de lixo, interoperabilidade nativa e outros serviços básicos. As [bibliotecas de estruturas do .NET Core](https://github.com/dotnet/corefx) fornecem tipos de dados primitivos, tipos de composição de aplicativos e utilitários fundamentais.
- O [tempo de execução do ASP.NET](https://github.com/aspnet/home), que fornece uma estrutura para a criação de aplicativos modernos conectados à Internet e baseados em nuvem, como aplicativos Web, aplicativos de IoT e back-ends móveis.
- As [ferramentas da CLI do .NET Core](https://github.com/dotnet/cli) e os compiladores de linguagem ([Roslyn](https://github.com/dotnet/roslyn) e [F#](https://github.com/microsoft/visualfsharp)) que permitem a experiência de desenvolvedor do .NET Core.
- A [ferramenta dotnet](https://github.com/dotnet/core-setup), que é usada para iniciar aplicativos do .NET Core e ferramentas da CLI. Ela seleciona e hospeda o tempo de execução, fornece uma política de carregamento de assembly e inicia aplicativos e ferramentas.

Esses componentes são distribuídos das seguintes maneiras:

- [Tempo de execução do .NET Core](https://dotnet.microsoft.com/download) – inclui as bibliotecas de tempo de execução e de estruturas do .NET Core.
- [Tempo de execução do ASP.NET Core](https://dotnet.microsoft.com/download) – inclui bibliotecas de tempo de execução e de estruturas do ASP.NET Core e do .NET Core.
- [SDK do .NET Core](https://dotnet.microsoft.com/download) – inclui as ferramentas da CLI do .NET, o tempo de execução do ASP.NET Core e o tempo de execução e a estrutura do .NET Core.

### <a name="open-source"></a>Código Aberto

O [.NET Core](https://github.com/dotnet/core) é um software livre ([licença MIT](https://github.com/dotnet/core/blob/master/LICENSE.TXT)) que foi concedido ao [.NET Foundation](https://dotnetfoundation.org) pela Microsoft em 2014. Agora ele é um dos projetos mais ativos do .NET Foundation. Ele pode ser adotado livremente por pessoas e empresas, incluindo para fins pessoais, comerciais ou acadêmicos. Várias empresas usam o .NET Core como parte de aplicativos, ferramentas, novas plataformas e serviços de hospedagem. Algumas dessas empresas fazer contribuições significativas para o .NET Core no GitHub e fornecem diretrizes sobre a direção de produto como parte do [.NET Foundation Technical Steering Group](https://dotnetfoundation.org/blog/tsg-welcome).

### <a name="designed-for-adaptability"></a>Projetado para adaptabilidade

O .NET Core foi criado como um produto muito semelhante, porém único, em relação a outros produtos .NET. Ele foi projetado para permitir uma ampla adaptabilidade a novas cargas de trabalho e plataformas. Ele já tem várias portas de sistemas operacionais e de CPU disponíveis e pode ser portado para muitas outras.

O produto é dividido em várias partes que podem se adaptar a novas plataformas em momentos diferentes. O tempo de execução e as bibliotecas fundamentais específicas de plataformas devem ser transportados como uma unidade. Bibliotecas independentes de plataforma devem funcionar no estado em que se encontra em todas as plataformas como-está em todas as plataformas desde a construção. Há uma tendência de projeto em reduzir as implementações específicas de plataforma para aumentar a eficiência dos desenvolvedores, preferindo código C# neutro com relação à plataforma sempre que um algoritmo ou API puder ser implementado dessa maneira de forma total ou parcial.

As pessoas geralmente perguntam como o .NET Core é implementado para dar suporte a vários sistemas operacionais. Eles normalmente perguntam se há implementações separadas ou se a [compilação condicional](https://en.wikipedia.org/wiki/Conditional_compilation) é usada. A resposta é ambas, com uma forte tendência para a compilação condicional.

Veja no gráfico a seguir que a maior parte do [CoreFX](https://github.com/dotnet/corefx) é um código neutro compartilhado entre todas as plataformas. O código neutro de plataforma pode ser implementado como um único assembly portátil usado em todas as plataformas.

![CoreFX: Linhas de código por plataforma](../images/corefx-platforms-loc.png)

Implementações de Windows e Unix são semelhantes em tamanho. O Windows tem uma implementação maior, pois o CoreFX implementa alguns recursos exclusivos do Windows como o [Microsoft.Win32.Registry](https://github.com/dotnet/corefx/tree/master/src/Microsoft.Win32.Registry), mas ainda não implementa nenhum conceito exclusivo do Unix. Você também verá que a maioria das implementações do Linux e do macOS são compartilhadas com uma implementação do Unix, enquanto as implementações específicas de Linux e macOS são semelhantes em tamanho.

Há uma combinação de bibliotecas específicas ou neutras de plataforma no .NET Core. Você pode observar esse padrão em alguns exemplos:

- O [CoreCLR](https://github.com/dotnet/coreclr) é específico de plataforma. Ele é baseado nos subsistemas do sistema operacional, como o gerenciador de memória e o agendador de thread.
- O [System.IO](https://github.com/dotnet/corefx/tree/master/src/System.IO) e o [System.Security.Cryptography.Algorithms](https://github.com/dotnet/corefx/tree/master/src/System.Security.Cryptography.Algorithms) são específicos da plataforma, pois as APIs de armazenamento e de criptografia são diferentes em cada sistema operacional.
- O [System.Collections](https://github.com/dotnet/corefx/tree/master/src/System.Collections) e [System.LINQ](https://github.com/dotnet/corefx/tree/master/src/System.Linq) são neutros de plataforma, considerando que eles podem criar e operar em estruturas de dados.

## <a name="comparisons-to-other-net-implementations"></a>Comparações com outras implementações do .NET

Talvez seja mais fácil entender o tamanho e a forma do .NET Core ao compará-lo com as implementações do .NET existentes.

### <a name="comparison-with-net-framework"></a>Comparação com o .NET Framework

O .NET foi anunciado pela primeira vez pela Microsoft em 2000 e continuou evoluindo a partir daí. O .NET Framework foi a principal implementação do .NET produzida pela Microsoft durante esse período de quase duas décadas.

Principais diferenças entre o .NET Core e o .NET Framework:

- **Modelos de aplicativo** – o .NET Core não dá suporte a todos os modelos de aplicativo do .NET Framework. Ele não dá suporte especificamente a Web Forms do ASP.NET e a ASP.NET MVC, mas dá suporte ao ASP.NET Core MVC. Foi anunciado que o [.NET Core 3 dará suporte ao WPF e ao Windows Forms](https://devblogs.microsoft.com/dotnet/net-core-3-and-support-for-windows-desktop-applications/).
- **APIs** – o .NET Core contém um grande subconjunto da biblioteca de classes base .NET Framework, com aspectos diferentes (os nomes de assembly são diferentes, os membros expostos nos tipos são diferentes em casos principais). Essas diferenças exigem alterações para portar o código para o .NET Core em alguns casos (confira [microsoft/dotnet-apiport](https://github.com/microsoft/dotnet-apiport)). O .NET Core implementa a especificação de API [.NET Standard](../standard/net-standard.md).
- **Subsistemas** – O.NET Core implementa um subconjunto dos subsistemas no .NET Framework com o objetivo de proporcionar uma implementação e um modelo de programação mais simples. Por exemplo, não há suporte para CAS (Segurança de Acesso do Código), porém há suporte para reflexão.
- **Plataformas** – O .NET Framework dá suporte a Windows e Windows Server, enquanto o .NET Core também dá suporte a macOS e Linux.
- **Software Livre** – O .NET Core é um software livre, enquanto apenas um [subconjunto somente leitura do .NET Framework](https://github.com/microsoft/referencesource) é um software livre.

Embora o .NET Core seja único e apresente diferenças significativas em relação ao .NET Framework e a outras implementações do .NET, é muito fácil compartilhar o código entre essas implementações usando técnicas de compartilhamento de binários ou de fontes.

Como o .NET Core é compatível com a instalação lado a lado, e seu tempo de execução é completamente independente do .NET Framework, ele pode ser instalado em computadores com o .NET Framework instalado sem problemas.

### <a name="comparison-with-mono"></a>Comparação com Mono

O [Mono](https://www.mono-project.com/) é a implementação .NET de plataforma cruzada e de [software livre](https://github.com/mono/mono) original, sendo fornecido pela primeira vez em 2004. Ele pode ser considerado um clone de comunidade do .NET Framework. A equipe de projeto do Mono contava com [padrões .NET](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/dotnet-standards.md) (especificamente o ECMA 335) publicados pela Microsoft a fim de fornecer uma implementação compatível.

Principais diferenças entre o .NET Core e o Mono:

- **Modelos de aplicativo** – O Mono dá suporte a um subconjunto de modelos de aplicativo do .NET Framework (por exemplo, o Windows Forms) e a alguns outros (como o [Xamarin.iOS](https://www.xamarin.com/platform)) por meio do produto Xamarin. O .NET Core não dá suporte a eles.
- **APIs** – O Mono dá suporte a um [grande subconjunto](http://docs.go-mono.com/?link=root%3a%2fclasslib) das APIs do .NET Framework, usando os mesmos nomes de assembly e fatoração.
- **Plataformas** – O Mono dá suporte a várias plataformas e CPUs.
- **Software Livre** – O Mono e o .NET Core usam a licença MIT e são projetos do .NET Foundation.
- **Foco** – o principal foco do Mono nos últimos anos são as plataformas móveis, enquanto o .NET Core se concentra em cargas de trabalho de nuvem e da área de trabalho.
