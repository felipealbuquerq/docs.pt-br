---
title: Monitoramento de recursos de domínio de aplicativo
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- monitoring managed memory use by application domain
- application domains, memory use
- memory use, monitoring
- application domains, resource monitoring
ms.assetid: 318bedf8-7f35-4f00-b34a-2b7b8e3fa315
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 8c7b9d7c2297fe30b02dc9782002413e9f38dc98
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64751536"
---
# <a name="application-domain-resource-monitoring"></a>Monitoramento de recursos de domínio de aplicativo

O ARM (Monitoramento de recursos de domínio do aplicativo) permite que os hosts monitorem o uso de CPU e memória por domínio do aplicativo. Isso é útil para hosts como o ASP.NET que usam vários domínios do aplicativo em um processo de longa execução. O host pode descarregar o domínio do aplicativo de um aplicativo que está prejudicando o desempenho de todo o processo, mas apenas se puder identificar o aplicativo problemático. O ARM fornece informações que podem ser usadas para ajudar a tomar essas decisões.

Por exemplo, um serviço de hospedagem pode ter vários aplicativos em execução em um servidor do ASP.NET. Se um aplicativo no processo começar a consumir muita memória ou muito tempo do processador, o serviço de hospedagem poderá usar ARM para identificar o domínio do aplicativo que está causando o problema.

O ARM é suficientemente leve para usar em aplicativos ativos. Você pode acessar as informações usando o ETW (Rastreamento de Eventos para Windows) ou diretamente por meio de APIs gerenciadas ou nativas.

## <a name="enabling-resource-monitoring"></a>Habilitar o monitoramento de recursos

O ARM pode ser habilitado de quatro maneiras: fornecendo um arquivo de configuração na inicialização do CLR (common language runtime), usando uma API de hospedagem não gerenciada, usando código gerenciado ou escutando eventos ETW do ARM.

Após a habilitação do ARM, ele começará coletando dados sobre todos os domínios do aplicativo no processo. Se um domínio do aplicativo tiver sido criado antes de o ARM ser habilitado, os dados cumulativos começarão quando o ARM for habilitado, não quando o domínio do aplicativo foi criado. Após a habilitação, o ARM não poderá ser desabilitado.

- Você pode habilitar o ARM na inicialização do CLR adicionando o elemento [\<appDomainResourceMonitoring>](../../../docs/framework/configure-apps/file-schema/runtime/appdomainresourcemonitoring-element.md) ao arquivo de configuração e configurando o atributo `enabled` como `true`. Um valor de `false` (o padrão) significa apenas que o ARM não está habilitado na inicialização; você pode ativá-lo mais tarde usando um dos outros mecanismos de ativação.

- O host pode habilitar o ARM solicitando a interface de hospedagem [ICLRAppDomainResourceMonitor](../../../docs/framework/unmanaged-api/hosting/iclrappdomainresourcemonitor-interface.md). Após a obtenção bem-sucedida dessa interface, o ARM será habilitado.

- O código gerenciado pode habilitar o ARM definindo a propriedade estática (`Shared` no Visual Basic) <xref:System.AppDomain.MonitoringIsEnabled%2A?displayProperty=nameWithType> como `true`. Assim que a propriedade for definida, o ARM estará habilitado.

- Você pode habilitar o ARM após a inicialização escutando eventos ETW. O ARM é habilitado e começa a acionar eventos para todos os domínios do aplicativo quando você habilita o provedor público `Microsoft-Windows-DotNETRuntime` usando a palavra-chave `AppDomainResourceManagementKeyword`. Para correlacionar os dados com domínios do aplicativo e threads, você também deve habilitar o provedor `Microsoft-Windows-DotNETRuntimeRundown` com a palavra-chave `ThreadingKeyword`.

## <a name="using-arm"></a>Usar ARM

O ARM fornece o tempo total do processador usado por um domínio do aplicativo e três tipos de informações sobre o uso da memória.

- **Tempo total do processador para um domínio do aplicativo, em segundos**: Isso é calculado pela adição dos tempos de thread relatados pelo sistema operacional de todos os threads que gastaram tempo sendo executados no domínio do aplicativo durante seu tempo de vida. Threads bloqueados ou em suspensão não usam o tempo do processador. Quando um thread chama um código nativo, o tempo gasto pelo thread no código nativo é incluído na contagem para o domínio de aplicativo em que a chamada foi feita.

  - API gerenciada: propriedade <xref:System.AppDomain.MonitoringTotalProcessorTime%2A?displayProperty=nameWithType>.

  - API de hospedagem: Método [ICLRAppDomainResourceMonitor::GetCurrentCpuTime](../../../docs/framework/unmanaged-api/hosting/iclrappdomainresourcemonitor-getcurrentcputime-method.md).

  - Eventos ETW: eventos `ThreadCreated`, `ThreadAppDomainEnter` e `ThreadTerminated`. Para saber mais sobre provedores e palavras-chave, consulte "Eventos de monitoramento de recursos AppDomain" em [Eventos ETW do CLR](../../../docs/framework/performance/clr-etw-events.md).

- **Total de alocações gerenciadas feitas por um domínio do aplicativo durante seu tempo de vida, em bytes**: O total de alocações nem sempre reflete o uso de memória por um domínio do aplicativo, porque os objetos alocados podem ser de curta duração. No entanto, se um aplicativo alocar e liberar grandes quantidades de objetos, o custo das alocações poderá ser considerável.

  - API gerenciada: propriedade <xref:System.AppDomain.MonitoringTotalAllocatedMemorySize%2A?displayProperty=nameWithType>.

  - API de hospedagem: Método [ICLRAppDomainResourceMonitor::GetCurrentAllocated](../../../docs/framework/unmanaged-api/hosting/iclrappdomainresourcemonitor-getcurrentallocated-method.md).

  - Eventos ETW: evento `AppDomainMemAllocated`, campo `Allocated`.

- **Memória gerenciada, em bytes, que é referenciada por um domínio do aplicativo e que restou após a coleta completa de bloqueio mais recente**: Esse número será preciso somente após uma coleta completa de bloqueio. (Isso é diferente das coletas simultâneas, que ocorrem em segundo plano e não bloqueiam o aplicativo). Por exemplo, a sobrecarga do método <xref:System.GC.Collect?displayProperty=nameWithType> causa uma coleta de bloqueio completa.

  - API gerenciada: propriedade <xref:System.AppDomain.MonitoringSurvivedMemorySize%2A?displayProperty=nameWithType>.

  - API de hospedagem: Método [ICLRAppDomainResourceMonitor::GetCurrentSurvived](../../../docs/framework/unmanaged-api/hosting/iclrappdomainresourcemonitor-getcurrentsurvived-method.md), parâmetro `pAppDomainBytesSurvived`.

  - Eventos ETW: evento `AppDomainMemSurvived`, campo `Survived`.

- **Total de memória gerenciada, em bytes, que é referenciada pelo processo e que restou após a coleta completa de bloqueio mais recente**: A memória restante dos domínios do aplicativo individuais pode ser comparada com esse número.

  - API gerenciada: propriedade <xref:System.AppDomain.MonitoringSurvivedProcessMemorySize%2A?displayProperty=nameWithType>.

  - API de hospedagem: Método [ICLRAppDomainResourceMonitor::GetCurrentSurvived](../../../docs/framework/unmanaged-api/hosting/iclrappdomainresourcemonitor-getcurrentsurvived-method.md), parâmetro `pTotalBytesSurvived`.

  - Eventos ETW: evento `AppDomainMemSurvived`, campo `ProcessSurvived`.

### <a name="determining-when-a-full-blocking-collection-occurs"></a>Determinar quando uma coleta de bloqueio completa ocorreu

Para determinar quando as contagens de memória restante são precisas, você precisa saber quando ocorreu uma coleta de bloqueio completa. O método para fazer isso depende da API usada para examinar as estatísticas do ARM.

#### <a name="managed-api"></a>API gerenciada

Se você usar as propriedades da classe <xref:System.AppDomain>, poderá usar o método <xref:System.GC.RegisterForFullGCNotification%2A?displayProperty=nameWithType> para se registrar e receber uma notificação de coletas completas. O limite usado não é importante, pois você está aguardando a conclusão de uma coleta, e não a aproximação de uma coleta. Depois, você pode chamar o método <xref:System.GC.WaitForFullGCComplete%2A?displayProperty=nameWithType>, que bloqueia até que uma coleta completa seja concluída. Você pode criar um thread que chama o método em um loop e que realiza qualquer análise necessária sempre que o método retornar.

Como alternativa, você pode chamar o método <xref:System.GC.CollectionCount%2A?displayProperty=nameWithType> periodicamente para verificar se a contagem de coletas de geração 2 aumentou. Dependendo da frequência de sondagem, talvez essa técnica não seja uma indicação tão precisa da ocorrência de uma coleta completa.

#### <a name="hosting-api"></a>API de hospedagem

Se você usar a API de hospedagem não gerenciada, o host deverá passar ao CLR uma implementação da interface [IHostGCManager](../../../docs/framework/unmanaged-api/hosting/ihostgcmanager-interface.md). O CLR chama o método [Ihostgcmanager](../../../docs/framework/unmanaged-api/hosting/ihostgcmanager-suspensionending-method.md) quando ele retoma a execução de threads que foram suspensos durante uma coleta. O CLR passa a geração da coleta concluída como um parâmetro do método, para que o host possa determinar se a coleta foi completa ou parcial. Sua implementação do método [Ihostgcmanager](../../../docs/framework/unmanaged-api/hosting/ihostgcmanager-suspensionending-method.md) pode consultar a existência de memória restante, a fim de certificar-se de que as contagens são recuperadas assim que forem atualizadas.

## <a name="see-also"></a>Consulte também

- <xref:System.AppDomain.MonitoringIsEnabled%2A?displayProperty=nameWithType>
- [Interface ICLRAppDomainResourceMonitor](../../../docs/framework/unmanaged-api/hosting/iclrappdomainresourcemonitor-interface.md)
- [\<appDomainResourceMonitoring>](../../../docs/framework/configure-apps/file-schema/runtime/appdomainresourcemonitoring-element.md)
- [Eventos de CLR ETW](../../../docs/framework/performance/clr-etw-events.md)
