---
title: Implementar tarefas em segundo plano em microsserviços com IHostedService e a classe BackgroundService
description: Arquitetura de microsserviços do .NET para aplicativos .NET em contêineres | Compreenda as novas opções para usar IHostedService e BackgroundService para implementar tarefas em segundo plano em microsserviços do .NET Core.
ms.date: 01/07/2019
ms.openlocfilehash: b3dca8db6568e6e8429645d6b433886d1d289b95
ms.sourcegitcommit: cf9515122fce716bcfb6618ba366e39b5a2eb81e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69038134"
---
# <a name="implement-background-tasks-in-microservices-with-ihostedservice-and-the-backgroundservice-class"></a>Implementar tarefas em segundo plano em microsserviços com IHostedService e a classe BackgroundService

Tarefas em segundo plano e trabalhos agendados são algo que talvez você precise implementar, eventualmente, em um aplicativo baseado em microsserviço em ou em qualquer tipo de aplicativo. A diferença ao usar uma arquitetura de microsserviços é que você pode implementar um único processo/contêiner de microsserviço para hospedar essas tarefas em segundo plano para que você possa expandir/reduzir conforme necessário ou até mesmo garantir que sejam executadas como uma única instância daquele processo de microsserviço/contêiner.

De um ponto de vista genérico, no .NET Core, chamamos esses tipos de tarefas de *Serviços Hospedados*, porque são serviços/lógica que você hospeda em seu host/aplicativo/microsserviço. Observe que, neste caso, o serviço hospedado simplesmente significa uma classe com a lógica de tarefa em segundo plano.

Desde o .NET Core 2.0, a estrutura fornece uma nova interface chamada <xref:Microsoft.Extensions.Hosting.IHostedService>, ajudando você a implementar serviços hospedados facilmente. A ideia básica é que você pode registrar várias tarefas de segundo plano (serviços hospedados), que são executadas em segundo plano enquanto o host da Web ou o host está em execução, conforme mostra a imagem 6-26.

![O ASP.NET Core 1.x e 2.x dá suporte a IWebHost para processos em segundo plano em aplicativos Web, o .NET Core 2.1 dá suporte a IHost para processos em segundo plano com aplicativos de console simples.](./media/image26.png)

**Figura 6-26**. Usando IHostedService em WebHost versus um Host

Observe a diferença entre `WebHost` e `Host`.

Um `WebHost` (classe base implementando `IWebHost`) no ASP.NET Core 2.0 é o artefato de infraestrutura que você usa para fornecer recursos de servidor HTTP ao seu processo, como se você estivesse implementando um aplicativo Web MVC ou um serviço de API da Web. Ele fornece todos os benefícios da nova infraestrutura no ASP.NET Core, permitindo que você use a injeção de dependência, insira middleware no pipeline da solicitação etc. e use com precisão esses `IHostedServices` para tarefas em segundo plano.

Um `Host` (classe base que implementa `IHost`) foi introduzido no .NET Core 2.1. Basicamente, um `Host` permite que você tenha uma infraestrutura semelhante àquela que você tem com `WebHost` (injeção de dependência, serviços hospedados etc.), mas, nesse caso, você quer apenas ter um processo simples e mais leve como o host, sem nada relacionado ao MVC, à API da Web ou aos recursos de servidor HTTP.

Portanto, você pode escolher e criar um processo de host especializado com IHost para lidar com os serviços hospedados e nada mais, como um microsserviço feito apenas para hospedar o `IHostedServices` ou você pode, como alternativa, estender um ASP.NET Core `WebHost` existente, como um aplicativo MVC ou API da Web do ASP.NET Core existente.

Cada abordagem tem vantagens e desvantagens, dependendo de suas necessidades de negócios e escalabilidade. A conclusão é basicamente que, se as tarefas em segundo plano não tiverem nenhuma relação com HTTP (IWebHost), você deverá usar IHost.

## <a name="registering-hosted-services-in-your-webhost-or-host"></a>Registro de serviços hospedados em seu WebHost ou Host

Vamos analisar detalhadamente a interface `IHostedService`, já que seu uso é muito semelhante em um `WebHost` ou em um `Host`.

SignalR é um exemplo de um artefato usando serviços hospedados, mas você também pode usá-lo para itens muito mais simples, como:

- Uma tarefa em segundo plano sondando um banco de dados em busca de alterações.
- Uma tarefa agendada atualizando algum cache periodicamente.
- Uma implementação de QueueBackgroundWorkItem que permite que uma tarefa seja executada em um thread em segundo plano.
- Processamento de mensagens de uma fila de mensagens em segundo plano de um aplicativo Web enquanto se compartilham serviços comuns como `ILogger`.
- Uma tarefa em segundo plano iniciada com `Task.Run()`.

Você pode basicamente descarregar qualquer uma dessas ações para uma tarefa em segundo plano com base em IHostedService.

A maneira como você adiciona um ou vários `IHostedServices` no `WebHost` ou `Host` é registrando-os por meio da DI (injeção de dependência) padrão em um `WebHost` do ASP.NET Core (ou em um `Host` no .NET Core 2.1 e posterior). Basicamente, você deve registrar os serviços hospedados no método `ConfigureServices()` familiar da classe `Startup`, como no código a seguir de um WebHost ASP.NET típico.

```csharp
public IServiceProvider ConfigureServices(IServiceCollection services)
{
    //Other DI registrations;

    // Register Hosted Services
    services.AddSingleton<IHostedService, GracePeriodManagerService>();
    services.AddSingleton<IHostedService, MyHostedServiceB>();
    services.AddSingleton<IHostedService, MyHostedServiceC>();
    //...
}
```

Nesse código, o serviço hospedado `GracePeriodManagerService` é o código real do microsserviço de negócios de Ordenação em eShopOnContainers, enquanto os outros dois são apenas dois exemplos adicionais.

A execução da tarefa em segundo plano `IHostedService` é coordenada com o tempo de vida do aplicativo (ou seja, host ou microsserviço). Você registra tarefas quando o aplicativo é iniciado e você tem a oportunidade de fazer alguma ação normal ou limpeza quando o aplicativo está sendo desligado.

Sem usar `IHostedService`, você sempre pode iniciar um thread em segundo plano para executar qualquer tarefa. A diferença está precisamente no tempo de desligamento do aplicativo quando esse thread seria simplesmente seria interrompido sem a oportunidade de executar as ações de limpeza normais.

## <a name="the-ihostedservice-interface"></a>A interface IHostedService

Quando você registra um `IHostedService`, o .NET Core chamará os métodos `StartAsync()` e `StopAsync()` de seu tipo `IHostedService` durante o início e a parada do aplicativo, respectivamente. Especificamente, o início é chamado depois que o servidor foi iniciado e `IApplicationLifetime.ApplicationStarted` foi disparado.

O `IHostedService`, conforme definido no .NET Core, tem a seguinte aparência.

```csharp
namespace Microsoft.Extensions.Hosting
{
    //
    // Summary:
    //     Defines methods for objects that are managed by the host.
    public interface IHostedService
    {
        //
        // Summary:
        // Triggered when the application host is ready to start the service.
        Task StartAsync(CancellationToken cancellationToken);
        //
        // Summary:
        // Triggered when the application host is performing a graceful shutdown.
        Task StopAsync(CancellationToken cancellationToken);
    }
}
```

Como você pode imaginar, é possível criar várias implementações de IHostedService e registrá-las no método `ConfigureService()` no contêiner de ID, como mostrado anteriormente. Todos esses serviços hospedados serão iniciados e interrompidos junto com o aplicativo/microsserviço.

Como desenvolvedor, você é responsável por lidar com a ação de parada de seus serviços quando o método `StopAsync()` é disparado pelo host.

## <a name="implementing-ihostedservice-with-a-custom-hosted-service-class-deriving-from-the-backgroundservice-base-class"></a>Implementando IHostedService com uma classe de serviço hospedado personalizado derivado da classe base BackgroundService

Vá em frente e crie sua classe de serviço hospedado personalizada do zero e implemente o `IHostedService`, como você precisa fazer ao usar o .NET Core 2.0.

No entanto, como a maioria das tarefas em segundo plano terá necessidades semelhantes em relação ao gerenciamento de tokens de cancelamento e outras operações típicas, há uma classe base abstrata conveniente da qual você pode derivar, chamada `BackgroundService` (disponível no .NET Core 2.1).

Essa classe fornece o trabalho principal necessário para configurar a tarefa em segundo plano.

O próximo código é a classe base abstrata BackgroundService, conforme implementada no .NET Core.

```csharp
// Copyright (c) .NET Foundation. Licensed under the Apache License, Version 2.0.
/// <summary>
/// Base class for implementing a long running <see cref="IHostedService"/>.
/// </summary>
public abstract class BackgroundService : IHostedService, IDisposable
{
    private Task _executingTask;
    private readonly CancellationTokenSource _stoppingCts =
                                                   new CancellationTokenSource();

    protected abstract Task ExecuteAsync(CancellationToken stoppingToken);

    public virtual Task StartAsync(CancellationToken cancellationToken)
    {
        // Store the task we're executing
        _executingTask = ExecuteAsync(_stoppingCts.Token);

        // If the task is completed then return it,
        // this will bubble cancellation and failure to the caller
        if (_executingTask.IsCompleted)
        {
            return _executingTask;
        }

        // Otherwise it's running
        return Task.CompletedTask;
    }

    public virtual async Task StopAsync(CancellationToken cancellationToken)
    {
        // Stop called without start
        if (_executingTask == null)
        {
            return;
        }

        try
        {
            // Signal cancellation to the executing method
            _stoppingCts.Cancel();
        }
        finally
        {
            // Wait until the task completes or the stop token triggers
            await Task.WhenAny(_executingTask, Task.Delay(Timeout.Infinite,
                                                          cancellationToken));
        }

    }

    public virtual void Dispose()
    {
        _stoppingCts.Cancel();
    }
}
```

Ao derivar da classe base abstrata anterior, graças àquela implementação herdada, você só precisa implementar o método `ExecuteAsync()` em sua própria classe de serviço hospedado personalizada, como no seguinte código simplificado eShopOnContainers que está sondando um banco de dados e publicando eventos de integração no Barramento de Eventos quando necessário.

```csharp
public class GracePeriodManagerService : BackgroundService
{
    private readonly ILogger<GracePeriodManagerService> _logger;
    private readonly OrderingBackgroundSettings _settings;

    private readonly IEventBus _eventBus;

    public GracePeriodManagerService(IOptions<OrderingBackgroundSettings> settings,
                                     IEventBus eventBus,
                                     ILogger<GracePeriodManagerService> logger)
    {
        //Constructor’s parameters validations...
    }

    protected override async Task ExecuteAsync(CancellationToken stoppingToken)
    {
        _logger.LogDebug($"GracePeriodManagerService is starting.");

        stoppingToken.Register(() =>
            _logger.LogDebug($" GracePeriod background task is stopping."));

        while (!stoppingToken.IsCancellationRequested)
        {
            _logger.LogDebug($"GracePeriod task doing background work.");

            // This eShopOnContainers method is querying a database table
            // and publishing events into the Event Bus (RabbitMQ / ServiceBus)
            CheckConfirmedGracePeriodOrders();

            await Task.Delay(_settings.CheckUpdateTime, stoppingToken);
        }

        _logger.LogDebug($"GracePeriod background task is stopping.");
    }

    .../...
}
```

Neste caso específico para eShopOnContainers, que está executando um método de aplicativo que está consultando uma tabela de banco de dados procurando pedidos com um estado específico e ao aplicar as alterações, ele está publicando eventos de integração por meio de um barramento de evento (sob ele, pode estar usando RabbitMQ ou Barramento de Serviço do Azure).

Obviamente, você pode executar qualquer outra tarefa em segundo plano de negócios em vez disso.

Por padrão, o token de cancelamento é definido com um tempo limite de 5 segundos, embora você possa alterar esse valor ao criar seu `WebHost` usando a extensão `UseShutdownTimeout` do `IWebHostBuilder`. Isso significa que o nosso serviço deve cancelar dentro de 5 segundos, caso contrário, ele será encerrado mais abruptamente.

O código a seguir alteraria esse tempo para 10 segundos.

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

### <a name="summary-class-diagram"></a>Diagrama de classe de resumo

A imagem a seguir mostra um resumo visual das classes e interfaces envolvidas na implementação do IHostedServices.

![Diagrama de classe: IWebHost e IHost podem hospedar muitos serviços, herdados de BackgroundService, que implementa IHostedService.](./media/image27.png)

**Figura 6-27**. Diagrama de classe mostrando as várias classes e interfaces relacionadas ao IHostedService

### <a name="deployment-considerations-and-takeaways"></a>Pontos importantes e considerações sobre implantação

É importante observar que a maneira como você implanta o ASP.NET Core `WebHost` ou .NET Core `Host` pode afetar a solução final. Por exemplo, se você implantar o `WebHost` no IIS ou em um Serviço de Aplicativo do Azure normal, o host poderá desligar devido a reciclagens do pool de aplicativos. Porém, se você estiver implantando eu host como um contêiner em um orquestrador como Kubernetes ou Service Fabric, poderá controlar o número garantido de instâncias ativas do seu host. Além disso, poderá considerar outras abordagens na nuvem feitas especialmente para esses cenários, como Azure Functions. Por fim, se você precisar que o serviço esteja em execução o tempo todo e estiver implantando em um Windows Server, você poderá usar um Serviço Windows.

Porém, mesmo para um `WebHost` implantado em um pool de aplicativos, há cenários como repopulação ou liberação do cache em memória do aplicativo em que isso ainda seria aplicável.

A interface `IHostedService` fornece uma maneira conveniente de iniciar tarefas em segundo plano em um aplicativo Web ASP.NET Core (no .NET Core 2.0) ou em qualquer processo/host (começando com o .NET Core 2.1 com `IHost`). O principal benefício é a oportunidade que você obtém com o cancelamento normal para o código de limpeza de suas tarefas em segundo plano quando o host em si está sendo desligado.

#### <a name="additional-resources"></a>Recursos adicionais

- **Criar uma tarefa agendada no ASP.NET Core/Standard 2.0** <br/>
    <https://blog.maartenballiauw.be/post/2017/08/01/building-a-scheduled-cache-updater-in-aspnet-core-2.html>

- **Implementando IHostedService no ASP.NET Core 2.0** <br/>
    <https://www.stevejgordon.co.uk/asp-net-core-2-ihostedservice>

- **Amostra de GenericHost usando o ASP.NET Core 2.1** <br/>
    <https://github.com/aspnet/Hosting/tree/release/2.1/samples/GenericHostSample>

>[!div class="step-by-step"]
>[Anterior](test-aspnet-core-services-web-apps.md)
>[Próximo](implement-api-gateways-with-ocelot.md)
