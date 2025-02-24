---
title: WCF Data Services 4.5
ms.date: 03/30/2017
helpviewer_keywords:
- Astoria
- WCF Data Services, getting started
ms.assetid: 73d2bec3-7c92-4110-b905-11bb0462357a
ms.openlocfilehash: 017fe2177cf824d461b4c51ea805f75b6ddbe064
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/07/2019
ms.locfileid: "70779990"
---
# <a name="wcf-data-services-45"></a>WCF Data Services 4.5

WCF Data Services (anteriormente conhecido como "ADO.NET Data Services") é um componente da .NET Framework que permite que você crie serviços que usam o [!INCLUDE[ssODataFull](../../../../includes/ssodatafull-md.md)] para expor e consumir dados pela Web ou intranet usando a semântica do estado de [reapresentação transferência (REST)](https://go.microsoft.com/fwlink/?LinkId=113919). OData expõem dados como recursos que são endereçáveis por URIs. Os dados são acessados e alterados usando os verbos HTTP padrão GET, PUT, POST e DELETE. O OData usa as convenções de relacionamento de entidade do [modelo de dados de entidade](../adonet/entity-data-model.md) para expor recursos como conjuntos de entidades relacionadas por associações.

WCF Data Services usa o protocolo OData para endereçar e atualizar recursos. Dessa forma, você pode acessar esses serviços de qualquer cliente que ofereça suporte a OData. O OData permite que você solicite e grave dados em recursos usando formatos de transferência bem conhecidos: Atom, um conjunto de padrões para a troca e atualização de dados como XML e JavaScript Object Notation (JSON), um formato de troca de dados baseado em texto usado extensivamente em aplicativos AJAX.

WCF Data Services pode expor dados originados de várias fontes como feeds OData. As ferramentas do Visual Studio facilitam a criação de um serviço baseado em OData usando um modelo de dados do ADO.NET Entity Framework. Você também pode criar feeds OData com base em classes Common Language Runtime (CLR) e até mesmo dados de associação tardia ou não tipada.

WCF Data Services também inclui um conjunto de bibliotecas de cliente, uma para aplicativos cliente .NET Framework geral e outra especificamente para aplicativos baseados no Silverlight. Essas bibliotecas de cliente fornecem um modelo de programação baseado em objeto quando você acessa um feed OData de ambientes como o .NET Framework e o Silverlight.

## <a name="where-should-i-start"></a>Onde devo começar?

Dependendo de seus interesses, considere a introdução ao WCF Data Services em um dos tópicos a seguir.

Desejo ir diretamente para...

- [Quickstart](quickstart-wcf-data-services.md) (Início rápido)

- [Introdução](getting-started-with-wcf-data-services.md)

- [Silverlight Quickstart](https://go.microsoft.com/fwlink/?LinkID=192782) (Início rápido do Silverlight)

- [Silverlight Quickstart for Windows Phone Development](https://go.microsoft.com/fwlink/?LinkID=214535) (Guia de início rápido do Silverlight para desenvolvimento do Windows Phone)

Mostrar apenas alguns códigos...

- [Quickstart](quickstart-wcf-data-services.md) (Início rápido)

- [Como: Executar consultas do serviço de dados](how-to-execute-data-service-queries-wcf-data-services.md)

- [Como: Associar dados a elementos de Windows Presentation Foundation](bind-data-to-wpf-elements-wcf-data-services.md)

Quero saber mais sobre o OData...

- [Artigos Apresentando o OData](https://go.microsoft.com/fwlink/?LinkId=220867)

- [Site do Protocolo Open Data](https://go.microsoft.com/fwlink/?LinkID=184554)

- [OData SDK](https://go.microsoft.com/fwlink/?LinkID=185248)

- [OData perguntas frequentes](https://go.microsoft.com/fwlink/?LinkId=185867)

Quero assistir a alguns vídeos...

- [Beginner's Guide to WCF Data Services](https://go.microsoft.com/fwlink/?LinkId=220864) (Guia do iniciante do WCF Data Services)

- [WCF Data Services Developer Videos](https://go.microsoft.com/fwlink/?LinkId=220861) (Vídeos para desenvolvedores do WCF Data Services)

- [OData Site de desenvolvedores](https://go.microsoft.com/fwlink/?LinkId=185866)

Quero ver exemplos de ponta a ponta...

- [WCF Data Services Documentation Samples on MSDN Samples Gallery](https://go.microsoft.com/fwlink/?LinkID=220865) (Amostras de documentação do WCF Data Services na galeria de amostras do MSDN)

- [Other WCF Data Services Samples on MSDN Samples Gallery](https://go.microsoft.com/fwlink/?LinkId=220866) (Outras amostras do WCF Data Services na galeria de amostras do MSDN)

- [OData SDK](https://go.microsoft.com/fwlink/?LinkID=185248)

Como ele se integra ao Visual Studio?

- [Generating the Data Service Client Library](generating-the-data-service-client-library-wcf-data-services.md) (Gerando a biblioteca de clientes do serviço de dados)

- [Creating the Data Service](creating-the-data-service.md) (Criando o serviço de dados)

- [Entity Framework Provider](entity-framework-provider-wcf-data-services.md) (Provedor de Entity Framework)

O que posso fazer com ele?

- [Visão geral](wcf-data-services-overview.md)

- [Artigos Apresentando o OData](https://go.microsoft.com/fwlink/?LinkId=220867)

- [Application Scenarios](application-scenarios-wcf-data-services.md) (Cenários de aplicativo)

Quero usar o Silverlight...

- [Silverlight Quickstart](https://go.microsoft.com/fwlink/?LinkID=192782) (Início rápido do Silverlight)

- [WCF Data Services (Silverlight)](https://go.microsoft.com/fwlink/?LinkID=143149)

- [Getting Started with Silverlight](https://go.microsoft.com/fwlink/?LinkId=148366) (Introdução ao Silverlight)

Quero usar LINQ...

- [Querying the Data Service](querying-the-data-service-wcf-data-services.md) (Consultando o serviço de dados)

- [LINQ Considerations](linq-considerations-wcf-data-services.md) (Considerações sobre LINQ)

- [Como: Executar consultas do serviço de dados](how-to-execute-data-service-queries-wcf-data-services.md)

Ainda preciso de mais algumas informações...

- [Blog da equipe do WCF Data Services](https://go.microsoft.com/fwlink/?LinkID=150511)

- [Recursos](wcf-data-services-resources.md)

- [WCF Data Services Developer Center](https://go.microsoft.com/fwlink/?LinkId=220868) (Central de desenvolvedores do WCF Data Services)

- [Site do Protocolo Open Data](https://go.microsoft.com/fwlink/?LinkID=184554)

## <a name="in-this-section"></a>Nesta seção

[Visão geral](wcf-data-services-overview.md)

Fornece uma visão geral dos recursos e funcionalidades disponíveis no WCF Data Services.

[O que há de novo no WCF Data Services 5,0](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/ee373845(v=vs.103))

Descreve a nova funcionalidade no WCF Data Services e suporte para novos recursos OData.

[Introdução](getting-started-with-wcf-data-services.md)

Descreve como expor e consumir feeds OData usando WCF Data Services.

[Defining WCF Data Services](defining-wcf-data-services.md) (Definindo o WCF Data Services)

Descreve como criar e configurar um serviço de dados que expõe feeds OData.

[WCF Data Services Client Library](wcf-data-services-client-library.md) (Biblioteca de clientes do WCF Data Services)

Descreve como usar bibliotecas de cliente para consumir feeds OData de um aplicativo cliente .NET Framework.

## <a name="see-also"></a>Consulte também

- [Representational State Transfer (REST)](https://go.microsoft.com/fwlink/?LinkId=113919) [REST (Transferência de Estado Representacional)]
