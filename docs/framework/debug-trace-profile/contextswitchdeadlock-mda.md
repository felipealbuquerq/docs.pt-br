---
title: MDA contextSwitchDeadlock
ms.date: 03/30/2017
helpviewer_keywords:
- deadlocks [.NET Framework]
- pumping messages
- STA message pumping
- single-threaded COM components
- MDAs (managed debugging assistants), context switching deadlocks
- managed debugging assistants (MDAs), context switching deadlocks
- ContextSwitchDeadlock MDA
- message pumping
- context switching deadlocks
ms.assetid: 26dfaa15-9ddb-4b0a-b6da-999bba664fa6
author: mairaw
ms.author: mairaw
ms.openlocfilehash: c1a0e2a6c7851b261baa3e02f6431e7a4ff697e4
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64660317"
---
# <a name="contextswitchdeadlock-mda"></a>MDA contextSwitchDeadlock

O MDA (assistente para depuração gerenciada) `contextSwitchDeadlock` é ativado quando um deadlock é detectado durante uma tentativa de transição de contexto COM.

## <a name="symptoms"></a>Sintomas

O sintoma mais comum é que uma chamada em um componente COM não gerenciado de um código gerenciado não retorna.  Outro sintoma é que o uso da memória aumenta com o tempo.

## <a name="cause"></a>Causa

A causa mais provável é que um thread de STA (single-threaded apartment) não está bombeando as mensagens. O thread de STA está esperando sem bombear as mensagens ou está realizando operações longas e não está permitindo o bombeamento da fila de mensagens.

O aumento do uso da memória com o tempo é causado por uma tentativa do thread do finalizador de chamar `Release` em um componente COM não gerenciado e pela falta de retorno desse componente.  Isso impede que o finalizador recupere outros objetos.

Por padrão, STA é o modelo de threading para o thread principal dos aplicativos do console do Visual Basic. Esse MDA é ativado se um thread de STA usar a interoperabilidade de COM direta ou indiretamente por meio do Common Language Runtime ou de um controle de terceiros.  Para evitar a ativação desse MDA em um aplicativo do console do Visual Basic, aplique o atributo <xref:System.MTAThreadAttribute> ao método principal ou modifique o aplicativo para bombear as mensagens.

É possível que esse MDA seja ativado erroneamente quando todas as condições a seguir forem atendidas:

- Um aplicativo cria componentes COM de threads de STA direta ou indiretamente por meio das bibliotecas.

- O aplicativo foi interrompido no depurador e o usuário continuou o aplicativo ou realizou uma operação de etapa.

- A depuração não gerenciada não está habilitada.

Para determinar se o MDA está sendo ativado erroneamente, desabilite todos os pontos de interrupção, reinicie o aplicativo e permita que ele seja executado sem parar. Se o MDA não for ativado, é provável que a ativação inicial era falsa. Nesse caso, desabilite o MDA para evitar interferências na sessão de depuração.

> [!NOTE]
> Esse MDA está no conjunto padrão para o Visual Studio. Para obter informações sobre como desabilitar MDAs, consulte [diagnosticando erros com assistentes para depuração gerenciada](../../../docs/framework/debug-trace-profile/diagnosing-errors-with-managed-debugging-assistants.md#enable-and-disable-mdas).

## <a name="resolution"></a>Resolução

Siga as regras de COM em relação ao bombeamento das mensagens de STA.

## <a name="effect-on-the-runtime"></a>Efeito sobre o tempo de execução

Esse MDA não tem efeito sobre o CLR. Ele apenas relata dados sobre contextos de COM.

## <a name="output"></a>Saída

Uma mensagem descrevendo o contexto atual e o contexto de destino.

## <a name="configuration"></a>Configuração

```xml
<mdaConfig>
  <assistants>
    <contextSwitchDeadlock />
  </assistants>
</mdaConfig>
```

## <a name="see-also"></a>Consulte também

- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- [Diagnosticando erros com Assistentes de Depuração Gerenciados](../../../docs/framework/debug-trace-profile/diagnosing-errors-with-managed-debugging-assistants.md)
- [Marshaling de interoperabilidade](../../../docs/framework/interop/interop-marshaling.md)
