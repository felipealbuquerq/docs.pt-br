---
title: Depurar fluxos de trabalho
ms.date: 03/30/2017
ms.assetid: b23b4814-ebb1-4c51-b7a9-469f4da7a96d
ms.openlocfilehash: 12b85260f8eab87fc9b98a99ca1192fd307313d9
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69915407"
---
# <a name="debugging-workflows"></a>Depurar fluxos de trabalho
[!INCLUDE[netfx_current_long](../../../includes/netfx-current-long-md.md)] oferece várias opções para depurar fluxos de trabalho em execução do ambiente de desenvolvimento. Fluxos de trabalho podem ser depurado no designer, em XAML, e no código.  
  
## <a name="debugging-in-the-workflow-designer"></a>Depuração no Designer de Fluxo de Trabalho  
 Os pontos de interrupção podem ser definidos em atividades no designer de fluxo de trabalho, realçando a atividade e pressionando **F9** ou usando o menu de contexto da atividade. A execução de fluxo de trabalho interrompe então quando o host de fluxo de trabalho é executado no modo de depuração. Na seguinte captura de tela, a execução de fluxo de trabalho é pausada em um ponto de interrupção. Para obter mais informações, consulte Depurando [fluxos de trabalho com o designer de fluxo de trabalho](/visualstudio/workflow-designer/debugging-workflows-with-the-workflow-designer).  
  
## <a name="debugging-in-xaml"></a>Depuração em XAML  
 Se um fluxo de trabalho pausou em um ponto de interrupção no designer, o fluxo de trabalho também pode ser depurado em XAML. Para exibir o ponto de execução em XAML, selecione **exibição XAML** no designer de fluxo de trabalho quando a execução do fluxo de trabalho for pausada. A depuração pode ser trocada de volta para o designer pela abertura o fluxo de trabalho no designer do gerenciador de solução. Para obter mais informações, confira [Como: Depurar XAML com o Designer de Fluxo de Trabalho](/visualstudio/workflow-designer/how-to-debug-xaml-with-the-workflow-designer).  
  
## <a name="debugging-in-code"></a>Depuração em código  
 Os pontos de interrupção de código podem ser usados em [!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] da mesma forma que podem ser usados em outros aplicativos obrigatórios. Clique na margem esquerda do painel de código para criar um ponto de interrupção de código ou pressione **F9** para posicionar um ponto de interrupção no local do cursor.  
  
## <a name="attaching-to-a-workflow-process"></a>Anexar a um processo de fluxo de trabalho  
 Fluxo de trabalho que depurar também suporta usando a infraestrutura do Visual Studio para anexar a um processo. Isso permite que o autor de fluxo de trabalho para depurar um execução de fluxo de trabalho em um ambiente diferente de host como Serviços de Informações da Internet (IIS) 7,0.  
  
## <a name="remote-debugging"></a>Depuração remota  
 A depuração remota do Windows Workflow Foundation (WF) funciona da mesma forma que a depuração remota para outros componentes do Visual Studio. Para obter informações sobre como usar a depuração [remota, consulte Como: Habilitar depuração](https://go.microsoft.com/fwlink/?LinkId=196257)remota.  
  
> [!NOTE]
> Se o aplicativo de fluxo de trabalho for direcionado para a arquitetura x86 e estiver hospedado em um computador que esteja executando um sistema operacional de 64 bits, a depuração remota não funcionará a menos que o Visual Studio esteja instalado no computador remoto ou o destino do aplicativo de fluxo de trabalho seja alterado para **Qualquer CPU**.  
  
## <a name="extending-the-workflow-debugging-service"></a>Estendendo o serviço de depuração de fluxo de trabalho  
 O serviço do depurador de fluxo de trabalho WF agora é público e pode ser usado para criar aplicativos personalizados como o monitoramento, a simulação, e depuração em um designer novamente hospedado. Para obter mais informações, consulte o tópico <xref:System.Activities.Presentation.Debug.DebuggerService>.
