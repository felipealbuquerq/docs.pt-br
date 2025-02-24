---
title: Resiliência e a alta disponibilidade em microsserviços
description: Microsserviços precisam ser projetados para resistir a falhas de dependências e de rede transitória e precisam ser resilientes para alcançar alta disponibilidade.
ms.date: 09/20/2018
ms.openlocfilehash: bb1bef0c9cc08e43aed80a29effe89587fb296f6
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68675053"
---
# <a name="resiliency-and-high-availability-in-microservices"></a>Resiliência e a alta disponibilidade em microsserviços

Lidar com falhas inesperadas é um dos problemas mais difíceis de resolver, especialmente em um sistema distribuído. Grande parte do código que os desenvolvedores gravam envolve tratamento de exceções, e também é nisso que a maior parte do tempo é gasta no teste. O problema envolve mais do que escrever código para lidar com falhas. O que acontece quando o computador em que o microsserviço está em execução falha? Não só é necessário detectar a falha desse microsserviço (um problema de disco rígido em si), como você também precisa de algo para reiniciar seu microsserviço.

Um microsserviço precisa ser resiliente a falhas e conseguir reiniciar geralmente em outro computador para disponibilidade. Essa resiliência também se resume ao estado que foi salvo em nome de microsserviço, do qual os microsserviço podem recuperar esse estado, e a se o microsserviço pode reiniciar com êxito. Em outras palavras, deve haver resiliência na capacidade de computação (o processo pode reiniciar a qualquer momento), bem como resiliência no estado ou nos dados (sem perda de dados e os dados permanecem consistentes).

Os problemas de resiliência são abordados durante outros cenários, como quando ocorrem falhas durante uma atualização de aplicativo. O microsserviço, trabalhando com o sistema de implantação, precisa determinar se pode continuar a avançar para a versão mais recente ou, em vez disso, reverter para uma versão anterior para manter um estado consistente. Questões como se computadores suficientes estão disponíveis para continuar avançando e como recuperar versões anteriores do microsserviço precisam ser consideradas. Isso requer que o microsserviço emita informações de integridade para que o aplicativo geral e o orquestrador possam tomar essas decisões.

Além disso, a resiliência está relacionada a como sistemas baseados em nuvem devem se comportar. Conforme mencionado, um sistema baseado em nuvem deve compreender falhas e tentar se recuperar automaticamente delas. Por exemplo, no caso de falhas de rede ou um contêiner, aplicativos cliente ou serviços cliente devem ter uma estratégia de repetição de envio de mensagens ou novas tentativas de solicitações, já que, em muitos casos, as falhas na nuvem são parciais. A seção [Implementando aplicativos resilientes](../implement-resilient-applications/index.md) neste guia aborda como lidar com falhas parciais. Descreve técnicas como novas tentativas com retirada exponencial ou o padrão de Disjuntor no .NET Core usando bibliotecas como [Polly](https://github.com/App-vNext/Polly), que oferece uma grande variedade de políticas para lidar com esse assunto.

## <a name="health-management-and-diagnostics-in-microservices"></a>Gerenciamento de integridade e diagnóstico em microsserviços

Pode parecer óbvio, e isso muitas vezes é negligenciado, mas um microsserviço deve informar sua integridade e seu diagnóstico. Caso contrário, há pouca percepção de uma perspectiva de operações. Correlacionar eventos de diagnóstico em um conjunto de serviços independentes e lidar com desvios de relógio do computador para compreender a ordem de eventos é um desafio. Da mesma maneira que você interage com um microsserviço sobre protocolos e formatos de dados estabelecidos, existe a necessidade de padronização de como registrar em log a integridade e eventos de diagnóstico que, por fim, terminam em um repositório de eventos para consulta e exibição. Em uma abordagem de microsserviços, é essencial que diferentes equipes concordem com um formato de log único. Deve haver uma abordagem consistente para exibir eventos de diagnóstico no aplicativo.

### <a name="health-checks"></a>Verificações de integridade

Integridade é diferente de diagnóstico. Integridade é sobre o microsserviço relatar seu estado atual para executar as ações apropriadas. Um bom exemplo é trabalhar com mecanismos de atualização e implantação para manter a disponibilidade. Embora um serviço possa não estar íntegro no momento devido a uma falha de processo ou reinicialização do computador, o serviço ainda pode estar operacional. A última coisa de que você precisa é tornar isso pior executando uma atualização. A melhor abordagem é fazer uma investigação primeiro ou aguardar até o microsserviço se recuperar. Eventos de integridade de um microsserviço ajudam-nos a tomar decisões informadas e, na verdade, ajudam a criar serviços com autorrecuperação.

Na seção [Implementação de verificações de integridade nos serviços ASP.NET Core](../implement-resilient-applications/monitor-app-health.md#implement-health-checks-in-aspnet-core-services) deste guia, explicamos como usar uma nova biblioteca ASP.NET HealthChecks em seus microsserviços para que eles possam relatar o próprio estado a um serviço de monitoramento para executar as ações apropriadas.

Você também tem a opção de usar uma excelente biblioteca de código-fonte aberto chamada Pulse vencer, disponível no [GitHub](https://github.com/Xabaril/BeatPulse) e como um [pacote do NuGet](https://www.nuget.org/packages/BeatPulse/). Essa biblioteca também faz verificações de integridade, mas com uma diferença, pois ela lida com dois tipos de verificação:

- **Atividade**: verifica se o microsserviço está ativo, ou seja, se ele é capaz de aceitar solicitações e responder. 
- **Preparação**: verifica se as dependências do microsserviço (banco de dados, serviços de fila, etc.) estão prontas, para que o microsserviço possa fazer o que deve ser feito. 

### <a name="using-diagnostics-and-logs-event-streams"></a>Usando fluxos de eventos de logs e diagnóstico

Logs fornecem informações sobre como um aplicativo ou serviço está sendo executado, incluindo exceções, avisos e mensagens informativas simples. Normalmente, cada log está em um formato de texto com uma linha por evento, embora exceções também muitas vezes mostrem o rastreamento de pilha em várias linhas.

Em aplicativos baseados em servidor monolíticos, você pode simplesmente gravar logs de um arquivo no disco (um arquivo de log) e então analisá-lo com qualquer ferramenta. Uma vez que a execução do aplicativo está limitada a um servidor ou VM fixo, geralmente não é complexo demais analisar o fluxo de eventos. No entanto, em um aplicativo distribuído em que vários serviços são executados em vários nós em um cluster do orquestrador, poder correlacionar eventos distribuídos é um desafio.

Um aplicativo baseado em microsserviço não deve tentar armazenar o fluxo de saída de eventos nem arquivos de log por si só, nem tentar gerenciar o roteamento de eventos para um local central. Ele deve ser transparente, o que significa que cada processo deve gravar apenas seu fluxo de eventos em uma saída padrão que, por baixo, será coletado pela infraestrutura do ambiente de execução em que está sendo executado. Um exemplo desses roteadores do fluxo de evento é [Microsoft.Diagnostic.EventFlow](https://github.com/Azure/diagnostics-eventflow), que coleta os fluxos de eventos de várias fontes e publica-os em sistemas de saída. Eles podem incluir uma saída simples padrão para um ambiente de desenvolvimento ou sistemas de nuvem como o [Azure Monitor](https://azure.microsoft.com/services/monitor//) e o [Diagnóstico do Azure](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostics-extension-overview). Também há boas plataformas e ferramentas de análise de log de terceiros que podem pesquisar, alertar, relatar e monitorar logs, inclusive em tempo real, como [Splunk](https://www.splunk.com/goto/Splunk_Log_Management?ac=ga_usa_log_analysis_phrase_Mar17&_kk=logs%20analysis&gclid=CNzkzIrex9MCFYGHfgodW5YOtA).

### <a name="orchestrators-managing-health-and-diagnostics-information"></a>Orquestradores gerenciando informações de integridade e diagnóstico

Quando você cria um aplicativo baseado em microsserviço, precisa lidar com a complexidade. Logicamente, é simples lidar com um único microsserviço, mas dezenas ou centenas de tipos e milhares de instâncias de microsserviços são um problema complexo. Não envolve apenas criar sua arquitetura de microsserviço: você também precisará de alta disponibilidade, capacidade de endereçamento, resiliência, integridade e diagnóstico se você quiser ter um sistema estável e coeso.

![Orquestradores fornecem uma plataforma de suporte para a execução de seus microsserviços.](./media/image22.png)

**Figura 4-22**. Uma Plataforma de Microsserviço é fundamental para o gerenciamento de integridade do aplicativo

É muito difícil você mesmo resolver os problemas complexos mostrados na Figura 4-22. As equipes de desenvolvimento devem se concentrar na solução de problemas de negócios e na criação de aplicativos personalizados com abordagens baseadas em microsserviço. Elas não devem se concentrar na solução de problemas de infraestrutura complexa; se fizessem isso, o custo de qualquer aplicativo baseado em microsserviço seria enorme. Portanto, há plataformas orientadas a microsserviços, conhecidas como orquestradores ou clusters de microsserviço, que tentam resolver os problemas de disco rígidos de criar e executar um serviço e usar os recursos de infraestrutura com eficiência. Isso reduz as complexidades da criação de aplicativos que usam uma abordagem de microsserviços.

Orquestradores diferentes podem parecer semelhantes, mas o diagnóstico e as verificações de integridade oferecidos por eles diferem em termos de recursos e estado de maturidade, às vezes dependendo da plataforma de sistema operacional, conforme explicado na próxima seção.

## <a name="additional-resources"></a>Recursos adicionais

- **O aplicativo de doze fatores. XI. Logs: tratar logs como fluxos de eventos** \
  <https://12factor.net/logs>

- Repositório do GitHub da **Biblioteca EventFlow de Diagnóstico da Microsoft**. \
  <https://github.com/Azure/diagnostics-eventflow>

- **O que é o Diagnóstico do Azure** \
  <https://docs.microsoft.com/azure/azure-diagnostics>

- **Conectar computadores Windows ao serviço do Azure Monitor** \
  <https://docs.microsoft.com/azure/azure-monitor/platform/agent-windows>

- **Registrando em log aquilo que importa: usando o bloco de aplicativo de registro em log semântico** \
  <https://docs.microsoft.com/previous-versions/msp-n-p/dn440729(v=pandp.60)>

- Site oficial do **Splunk**. \
  <https://www.splunk.com/>

- API de **classe EventSource** para ETW (Rastreamento de Eventos para Windows) \
  [https://docs.microsoft.com/dotnet/api/system.diagnostics.tracing.eventsource](xref:System.Diagnostics.Tracing.EventSource)

>[!div class="step-by-step"]
>[Anterior](microservice-based-composite-ui-shape-layout.md)
>[Próximo](scalable-available-multi-container-microservice-applications.md)
