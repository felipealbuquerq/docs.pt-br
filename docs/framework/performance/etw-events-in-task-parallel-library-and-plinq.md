---
title: Eventos ETW na biblioteca de tarefas paralelas e em PLINQ
ms.date: 03/30/2017
helpviewer_keywords:
- tasks, ETW events
ms.assetid: 87a9cff5-d86f-4e44-a06e-d12764d0dce2
author: mairaw
ms.author: mairaw
ms.openlocfilehash: 611ad0a6f4ec8b8c63010938372b733a0ac66052
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69955762"
---
# <a name="etw-events-in-task-parallel-library-and-plinq"></a>Eventos ETW na biblioteca de tarefas paralelas e em PLINQ

A Biblioteca de Paralelismo de Tarefas e o PLINQ geram eventos ETW (Rastreamento de Eventos para Windows) que podem ser usados para criar o perfil e solucionar problemas de aplicativos usando ferramentas como o Windows Performance Analyzer. No entanto, na maioria dos cenários, a melhor maneira de criar o perfil de código de aplicativo paralelo é usar o [Visualizador](/visualstudio/profiling/concurrency-visualizer) de simultaneidade no Visual Studio.

## <a name="task-parallel-library-etw-events"></a>Eventos ETW da Biblioteca de Paralelismo de Tarefas

Na estrutura de EVENT_HEADER, o GUID de ProviderId para os eventos gerados por <xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType>, <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> e <xref:System.Threading.Tasks.Parallel.Invoke%2A?displayProperty=nameWithType> é:

```
0x2e5dba47, 0xa3d2, 0x4d16, 0x8e, 0xe0, 0x66, 0x71, 0xff, 0xdc, 0xd7, 0xb5
```

### <a name="parallel-loop-begin"></a>Início do loop paralelo

EVENT_DESCRIPTOR.Task = 1

EVENT_DESCRIPTOR.Id = 1

#### <a name="user-data"></a>Dados de usuário

|**Nome**|**Tipo**|**Descrição**|
|--------------|--------------|---------------------|
|OriginatingTaskSchedulerID|<xref:System.Int32?displayProperty=nameWithType>|A ID do TaskScheduler que iniciou o loop.|
|OriginatingTaskID|<xref:System.Int32?displayProperty=nameWithType>|A ID da tarefa que iniciou o loop.|
|ForkJoinContextID|<xref:System.Int32?displayProperty=nameWithType>|Um identificador exclusivo usado para indicar o aninhamento e pares de eventos com semântica de fork/junção.|
|OperationType|<xref:System.Int32?displayProperty=nameWithType>|Indica o tipo de loop:<br /><br /> 1 = ParallelInvoke<br /><br /> 2 = ParallelFor<br /><br /> 3 = ParallelForEach|
|InclusiveFrom|<xref:System.Int64?displayProperty=nameWithType>|O valor inicial do contador de loops|
|ExclusiveTo|<xref:System.Int64?displayProperty=nameWithType>|O valor final do contador de loops|

### <a name="parallel-loop-end"></a>Fim do loop paralelo
 EVENT_DESCRIPTOR.Task = 2

 EVENT_DESCRIPTOR.Id = 2

#### <a name="user-data"></a>Dados de usuário

|**Nome**|**Tipo**|**Descrição**|
|--------------|--------------|---------------------|
|OriginatingTaskSchedulerID|<xref:System.Int32?displayProperty=nameWithType>|A ID do TaskScheduler que iniciou o loop.|
|OriginatingTaskID|<xref:System.Int32?displayProperty=nameWithType>|A ID da tarefa que iniciou o loop.|
|ForkJoinContextID|<xref:System.Int32?displayProperty=nameWithType>|Um identificador exclusivo usado para indicar o aninhamento e pares de eventos com semântica de fork/junção.|
|totalIterations|<xref:System.Int64?displayProperty=nameWithType>|O número total de iterações|

### <a name="parallel-invoke-begin"></a>Início da invocação paralela
 EVENT_DESCRIPTOR.Task = 3

 EVENT_DESCRIPTOR.Id = 3

#### <a name="user-data"></a>Dados de usuário

|**Nome**|**Tipo**|**Descrição**|
|--------------|--------------|---------------------|
|OriginatingTaskSchedulerID|<xref:System.Int32?displayProperty=nameWithType>|A ID do TaskScheduler que iniciou o loop.|
|OriginatingTaskID|<xref:System.Int32?displayProperty=nameWithType>|A ID da tarefa que iniciou o loop.|
|ForkJoinContextID|<xref:System.Int32?displayProperty=nameWithType>|Um identificador exclusivo usado para indicar o aninhamento e pares de eventos com semântica de fork/junção.|
|totalIterations|<xref:System.Int64?displayProperty=nameWithType>|O número total de iterações|
|operationType|<xref:System.Int32?displayProperty=nameWithType>|Indica o tipo de loop:<br /><br /> 1 = ParallelInvoke<br /><br /> 2 = ParallelFor<br /><br /> 3 = ParallelForEach|
|ActionCount|<xref:System.Int32?displayProperty=nameWithType>|O número de ações que serão executadas na invocação paralela.|

### <a name="parallel-invoke-end"></a>Fim da invocação paralela
 EVENT_DESCRIPTOR.Task = 4

 EVENT_DESCRIPTOR.Id = 4

#### <a name="user-data"></a>Dados de usuário

|**Nome**|**Tipo**|**Descrição**|
|--------------|--------------|---------------------|
|OriginatingTaskSchedulerID|<xref:System.Int32?displayProperty=nameWithType>|A ID do TaskScheduler que iniciou o loop.|
|OriginatingTaskID|<xref:System.Int32?displayProperty=nameWithType>|A ID da tarefa que iniciou o loop.|
|ForkJoinContextID|<xref:System.Int32?displayProperty=nameWithType>|Um identificador exclusivo usado para indicar o aninhamento e pares de eventos com semântica de fork/junção.|

## <a name="plinq-etw-events"></a>Eventos ETW do PLINQ
 O GUID de EVENT_HEADER.ProviderId do PLINQ é:

```
0x159eeeec, 0x4a14, 0x4418, 0xa8, 0xfe, 0xfa, 0xab, 0xcd, 0x98, 0x78, 0x87
```

### <a name="parallel-query-begin"></a>Início da consulta paralela
 EVENT_DESCRIPTOR.Task = 1

 EVENT_DESCRIPTOR.Id = 1

#### <a name="user-data"></a>Dados de usuário

|**Nome**|**Tipo**|**Descrição**|
|--------------|--------------|---------------------|
|OriginatingTaskSchedulerID|<xref:System.Int32?displayProperty=nameWithType>|A ID do TaskScheduler que iniciou o loop.|
|OriginatingTaskID|<xref:System.Int32?displayProperty=nameWithType>|A ID da tarefa que iniciou o loop.|
|QueryID|<xref:System.Int32?displayProperty=nameWithType>|Um identificador exclusivo de consulta.|

### <a name="parallel-query-end"></a>Fim da consulta paralela
 EVENT_DESCRIPTOR.Task = 2

 EVENT_DESCRIPTOR.Id = 2

#### <a name="user-data"></a>Dados de usuário

|**Nome**|**Tipo**|**Descrição**|
|--------------|--------------|---------------------|
|OriginatingTaskSchedulerID|<xref:System.Int32?displayProperty=nameWithType>|A ID do TaskScheduler que iniciou o loop.|
|OriginatingTaskID|<xref:System.Int32?displayProperty=nameWithType>|A ID da tarefa que iniciou o loop.|
|QueryID|<xref:System.Int32?displayProperty=nameWithType>|Um identificador exclusivo de consulta.|

## <a name="see-also"></a>Consulte também

- [Eventos ETW no .NET Framework](../../../docs/framework/performance/etw-events.md)
- [TPL (Biblioteca de Paralelismo de Tarefas)](../../standard/parallel-programming/task-parallel-library-tpl.md)
- [PLINQ (LINQ paralelo)](../../standard/parallel-programming/parallel-linq-plinq.md)
