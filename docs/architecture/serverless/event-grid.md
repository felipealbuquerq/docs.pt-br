---
title: Grade de eventos do Azure-aplicativos sem servidor
description: A grade de eventos do Azure é uma solução sem servidor para entrega e roteamento de eventos confiáveis em grande escala em um modelo de pagamento por evento.
author: JEREMYLIKNESS
ms.author: jeliknes
ms.date: 06/26/2018
ms.openlocfilehash: 4970130ede0c96c645129ee6c8c7d54cb1114042
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/30/2019
ms.locfileid: "69577569"
---
# <a name="event-grid"></a>Grade de eventos

A [grade de eventos do Azure](/azure/event-grid/overview) fornece a infraestrutura sem servidor para aplicativos baseados em eventos. Você pode publicar na grade de eventos de qualquer fonte e consumir mensagens de qualquer plataforma. A grade de eventos também tem suporte interno para eventos de recursos do Azure para simplificar a integração com seus aplicativos. Por exemplo, você pode assinar eventos de armazenamento de BLOBs para notificar seu aplicativo quando um arquivo for carregado. Em seguida, seu aplicativo pode publicar uma mensagem de grade de eventos personalizada consumida por outros aplicativos na nuvem ou locais. A grade de eventos foi criada para lidar de forma confiável em grande escala. Você Obtém os benefícios de publicar e assinar mensagens sem a sobrecarga de configurar a infraestrutura necessária.

![Logotipo da grade de eventos](./media/event-grid-logo.png)

Os principais recursos da grade de eventos incluem:

* Roteamento de eventos totalmente gerenciado.
* Entrega de eventos quase em tempo real em escala.
* Cobertura ampla dentro e fora do Azure.

## <a name="scenarios"></a>Cenários

A grade de eventos aborda vários cenários diferentes. Esta seção aborda três dos mais comuns.

### <a name="ops-automation"></a>Automação de operações

![Automação de operações](./media/ops-automation.png)

A grade de eventos pode ajudar a acelerar a automação e simplificar a imposição de políticas notificando a [automação do Azure](https://docs.microsoft.com/azure/automation) quando a infraestrutura é provisionada.

### <a name="application-integration"></a>Integração de aplicativos

![Integração de aplicativos](./media/app-integration.png)

Você pode usar a grade de eventos para conectar seu aplicativo a outros serviços. Usando protocolos HTTP padrão, até mesmo aplicativos herdados podem ser facilmente modificados para publicar mensagens de grade de eventos. Os ganchos da Web estão disponíveis para outros serviços e plataformas para consumir mensagens da grade de eventos.

### <a name="serverless-apps"></a>Aplicativos sem servidor

![Aplicativos sem servidor](./media/serverless-apps.png)

A grade de eventos pode disparar Azure Functions, aplicativos lógicos ou seu próprio código personalizado. Uma grande vantagem de usar a grade de eventos é que ela usa um mecanismo de *Push* para enviar mensagens quando ocorrem eventos. A arquitetura de push consome menos recursos e dimensiona melhor do que os mecanismos de sondagem. A sondagem deve verificar se há atualizações em um intervalo regular.

## <a name="event-grid-vs-other-azure-messaging-services"></a>Grade de eventos versus outros serviços de mensagens do Azure

O Azure fornece vários serviços de mensagens, incluindo [hubs de eventos](https://docs.microsoft.com/azure/event-hubs) e [barramento de serviço](https://docs.microsoft.com/azure/service-bus-messaging). Cada um foi projetado para atender a um conjunto específico de casos de uso. O diagrama a seguir fornece uma visão geral de alto nível das diferenças entre os serviços.

![Comparação de mensagens do Azure](./media/azure-messaging-services.png)

Para obter uma comparação mais detalhada, consulte [comparar serviços de mensagens](https://docs.microsoft.com/azure/event-grid/compare-messaging-services).

## <a name="performance-targets"></a>Metas de desempenho

Usando a grade de eventos, você pode aproveitar as seguintes garantias de desempenho:

* Latência de ponta a ponta de subsegundos no 99 º percentil.
* 99,99% de disponibilidade.
* 10 milhões eventos por segundo por região.
* 100 milhões assinaturas por região.
* 50-latência do Publicador MS.
* repetição de 24 horas com retirada exponencial para entrega garantida na janela de um dia.
* Failover regional transparente.

## <a name="event-grid-schema"></a>Esquema de grade de eventos

A grade de eventos usa um esquema padrão para encapsular eventos personalizados. O esquema é como um envelope que encapsula o elemento de dados personalizado. Aqui está um exemplo de mensagem de grade de eventos:

```json
[{
    "id": "03e24f21-a955-43cc-8921-1f61a6081ce0",
    "eventType": "myCustomEvent",
    "subject": "foo/bar/12",
    "eventTime": "2018-09-22T10:36:01+00:00",
    "data": {
        "favoriteColor": "blue",
        "favoriteAnimal": "panther",
        "favoritePlanet": "Jupiter"
    },
    "dataVersion": "1.0"
}]
```

Tudo sobre a mensagem é padrão, exceto `data` a propriedade. Você pode inspecionar a mensagem e usar `eventType` e `dataVersion` para desserializar a parte personalizada da carga.

## <a name="azure-resources"></a>Recursos do Azure

Uma grande vantagem de usar a grade de eventos são as mensagens automáticas produzidas pelo Azure. No Azure, os recursos são publicados automaticamente em um *tópico* que permite que você assine vários eventos. A tabela a seguir lista os tipos de recursos, tipos de mensagem e eventos que estão disponíveis automaticamente.

| Recurso do Azure | Tipo de evento | Descrição |
| -------------- | ---------- | ----------- |
| Assinatura do Azure | Microsoft.Resources.ResourceWriteSuccess | Gerado quando uma operação de criação ou atualização de recurso é realizada com sucesso. |
| | Microsoft.Resources.ResourceWriteFailure | Gerado quando uma operação de criação ou atualização de recurso falha. |
| | Microsoft.Resources.ResourceWriteCancel | Gerado quando uma operação de criação ou atualização de recurso é cancelada. |
|  | Microsoft.Resources.ResourceDeleteSuccess | Gerado quando uma operação de exclusão de recurso é realizada com sucesso. |
|  | Microsoft.Resources.ResourceDeleteFailure | Gerado quando uma operação de exclusão de recurso falha. |
| | Microsoft.Resources.ResourceDeleteCancel | Gerado quando uma operação de exclusão de recurso é cancelada. Esse evento ocorre quando uma implantação de modelo é cancelada. |
| Armazenamento de Blob | Microsoft.Storage.BlobCreated | Gerado quando um blob é criado. |
| | Microsoft.Storage.BlobDeleted | Gerado quando um blob é excluído. |
| Hubs de eventos | Microsoft.EventHub.CaptureFileCreated | Gerado quando um arquivo de captura é criado.
| Hub IoT | Microsoft.Devices.DeviceCreated | Publicado quando um dispositivo é registrado em um hub IoT. |
| | Microsoft.Devices.DeviceDeleted | Publicado quando um dispositivo é excluído de um hub IoT. |
| Grupos de recursos | Microsoft.Resources.ResourceWriteSuccess | Gerado quando uma operação de criação ou atualização de recurso é realizada com sucesso. |
| | Microsoft.Resources.ResourceWriteFailure | Gerado quando uma operação de criação ou atualização de recurso falha. |
| | Microsoft.Resources.ResourceWriteCancel | Gerado quando uma operação de criação ou atualização de recurso é cancelada. |
| | Microsoft.Resources.ResourceDeleteSuccess | Gerado quando uma operação de exclusão de recurso é realizada com sucesso. |
| | Microsoft.Resources.ResourceDeleteFailure | Gerado quando uma operação de exclusão de recurso falha. |
| | Microsoft.Resources.ResourceDeleteCancel | Gerado quando uma operação de exclusão de recurso é cancelada. Esse evento ocorre quando uma implantação de modelo é cancelada. |

Para obter mais informações, consulte [esquema de eventos da grade de eventos do Azure](https://docs.microsoft.com/azure/event-grid/event-schema).

Você pode acessar a grade de eventos de qualquer tipo de aplicativo, mesmo um que seja executado localmente.

## <a name="conclusion"></a>Conclusão

Neste capítulo, você aprendeu sobre a plataforma sem servidor do Azure que é composta de Azure Functions, aplicativos lógicos e grade de eventos. Você pode usar esses recursos para criar uma arquitetura de aplicativo totalmente sem servidor ou criar uma solução híbrida que interaja com outros recursos de nuvem e servidores locais. Combinado com uma plataforma de dados sem servidor, como o [Azure SQL](https://docs.microsoft.com/azure/sql-database) ou o [CosmosDB](https://docs.microsoft.com/azure/cosmos-db/introduction), você pode criar aplicativos nativos de nuvem totalmente gerenciados.

## <a name="recommended-resources"></a>Recursos recomendados

* [Planos do serviço de aplicativo](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview)
* [Application Insights](https://docs.microsoft.com/azure/application-insights)
* [Análise de Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-analytics)
* [Azure Traga seu aplicativo para a nuvem com Azure Functions sem servidor](https://channel9.msdn.com/events/Connect/2017/E102)
* [Grade de Eventos do Azure](https://docs.microsoft.com/azure/event-grid/overview)
* [Esquema de evento da grade de eventos do Azure](https://docs.microsoft.com/azure/event-grid/event-schema)
* [Hubs de eventos do Azure](https://docs.microsoft.com/azure/event-hubs)
* [Documentação do Azure Functions](https://docs.microsoft.com/azure/azure-functions)
* [Conceitos de Azure Functions de gatilhos e associações](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings)
* [Aplicativos Lógicos do Azure](https://docs.microsoft.com/azure/logic-apps)
* [Barramento de Serviço do Azure](https://docs.microsoft.com/azure/service-bus-messaging)
* [Armazenamento de tabelas do Azure](https://docs.microsoft.com/azure/cosmos-db/table-storage-overview)
* [Comparar as funções 1. x e 2. x](https://docs.microsoft.com/azure/azure-functions/functions-versions)
* [Conectando-se a fontes de dados locais com o gateway de dados local do Azure](https://docs.microsoft.com/azure/analysis-services/analysis-services-gateway)
* [Criar sua primeira função no portal do Azure](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function)
* [Criar sua primeira função usando o CLI do Azure](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function-azure-cli)
* [Criar sua primeira função usando o Visual Studio](https://docs.microsoft.com/azure/azure-functions/functions-create-your-first-function-visual-studio)
* [Idiomas com suporte de funções](https://docs.microsoft.com/azure/azure-functions/supported-languages)
* [Azure Functions de monitor](https://docs.microsoft.com/azure/azure-functions/functions-monitoring)
* [Trabalhar com Proxies do Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-proxies)

>[!div class="step-by-step"]
>[Anterior](logic-apps.md)
>[Próximo](durable-azure-functions.md)
