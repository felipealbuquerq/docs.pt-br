---
title: Aplicativos Application Insights-sem servidor
description: Application Insights é uma plataforma de diagnóstico sem servidor que permite aos desenvolvedores detectar, fazer triagem e diagnosticar problemas em aplicativos Web, aplicativos móveis, aplicativos de área de trabalho e microservices.
author: JEREMYLIKNESS
ms.author: jeliknes
ms.date: 06/26/2018
ms.openlocfilehash: 1f5b99fba448c2c1c12139524ffdcd3708b3c956
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/30/2019
ms.locfileid: "69577719"
---
# <a name="telemetry-with-application-insights"></a>Telemetria com Application Insights

[Application insights](https://docs.microsoft.com/azure/application-insights) é uma plataforma de diagnóstico sem servidor que permite aos desenvolvedores detectar, fazer triagem e diagnosticar problemas em aplicativos Web, aplicativos móveis, aplicativos de área de trabalho e microservices. Você pode ativar Application Insights para aplicativos de funções simplesmente invertendo uma opção no Portal. O Application Insights fornece todos esses recursos sem a necessidade de configurar um servidor ou configurar seu próprio banco de dados. Todos os recursos de Application Insights são fornecidos como um serviço que se integra automaticamente aos seus aplicativos.

![Logotipo de Application Insights](./media/application-insights-logo.png)

Adicionar Application Insights a aplicativos existentes é tão fácil quanto adicionar uma chave de instrumentação às configurações do seu aplicativo. Com Application Insights você pode:

* Crie gráficos personalizados e alertas com base em métricas, como o número de invocações de função, o tempo necessário para executar uma função e exceções
* Analisar falhas e exceções de servidor
* Detalhar o desempenho por operação e medir o tempo necessário para chamar dependências de terceiros
* Monitore o uso da CPU, a memória e as taxas em todos os servidores que hospedam seus aplicativos de funções
* Exibir uma transmissão ao vivo de métricas, incluindo contagem de solicitações e latência para seus aplicativos de funções
* Use a [análise](https://docs.microsoft.com/azure/application-insights/app-insights-analytics) para pesquisar, consultar e criar gráficos personalizados sobre seus dados de função

![Gerenciador de métricas](./media/metrics-explorer.png)

Além da telemetria interna, também é possível gerar telemetria personalizada. O trecho de código a seguir cria um cliente de telemetria personalizado usando a chave de instrumentação definida para o aplicativo de funções:

```csharp
public static TelemetryClient telemetry = new TelemetryClient()
{
    InstrumentationKey = Environment.GetEnvironmentVariable("APPINSIGHTS_INSTRUMENTATIONKEY")
};
```

O código a seguir mede quanto tempo leva para inserir uma nova linha em uma instância de [armazenamento de tabela do Azure](https://docs.microsoft.com/azure/cosmos-db/table-storage-overview) :

```csharp
var operation = TableOperation.Insert(entry);
var startTime = DateTime.UtcNow;
var timer = System.Diagnostics.Stopwatch.StartNew();
await table.ExecuteAsync(operation);
telemetry.TrackDependency("AzureTableStorageInsert", "Insert", startTime, timer.Elapsed, true);
```

O grafo de desempenho resultante é mostrado:

![Telemetria personalizada](./media/custom-telemetry.png)

A telemetria personalizada revela que o tempo médio para inserir uma nova linha é de 32,6 milissegundos.

O Application Insights fornece uma maneira poderosa e conveniente de registrar em log a telemetria detalhada sobre seus aplicativos sem servidor. Você tem controle total sobre o nível de rastreamento e registro em log que é fornecido. Você pode acompanhar estatísticas personalizadas, como eventos, dependências e exibição de página. Por fim, a poderosa análise permite que você escreva consultas que façam perguntas importantes e gere gráficos e ideias avançadas.

Para obter mais informações, consulte [monitorar Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-monitoring).

>[!div class="step-by-step"]
>[Anterior](azure-functions.md)
>[Próximo](logic-apps.md)
