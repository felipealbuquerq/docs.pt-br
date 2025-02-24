---
title: Rastreamento ETW
ms.date: 03/30/2017
ms.assetid: ac99a063-e2d2-40cc-b659-d23c2f783f92
ms.openlocfilehash: c484e3438ad3512bd6186297f3edf8068a60795e
ms.sourcegitcommit: 581ab03291e91983459e56e40ea8d97b5189227e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2019
ms.locfileid: "70044998"
---
# <a name="etw-tracing"></a>Rastreamento ETW
Este exemplo demonstra como implementar o rastreamento de ponta a ponta (e2e) usando o ETW (rastreamento de eventos para Windows) e `ETWTraceListener` o que é fornecido com este exemplo. O exemplo é baseado na [introdução](../../../../docs/framework/wcf/samples/getting-started-sample.md) e inclui o rastreamento ETW.  
  
> [!NOTE]
> Os procedimentos de instalação e as instruções de compilação para esse exemplo estão localizadas no final deste tópico.  
  
 Este exemplo pressupõe que você esteja familiarizado com o [rastreamento e o registro de mensagens](../../../../docs/framework/wcf/samples/tracing-and-message-logging.md).  
  
 Cada origem de rastreamento no <xref:System.Diagnostics> modelo de rastreamento pode ter vários ouvintes de rastreamento que determinam onde e como os dados são rastreados. O tipo de ouvinte define o formato no qual os dados de rastreamento são registrados. O exemplo de código a seguir mostra como adicionar o ouvinte à configuração.  
  
```xml  
<system.diagnostics>  
    <sources>  
        <source name="System.ServiceModel"   
             switchValue="Verbose,ActivityTracing"  
             propagateActivity="true">  
            <listeners>  
                <add type=  
                   "System.Diagnostics.DefaultTraceListener"  
                   name="Default">  
                   <filter type="" />  
                </add>  
                <add name="ETW">  
                    <filter type="" />  
                </add>  
            </listeners>  
        </source>  
    </sources>  
    <sharedListeners>  
        <add type=  
            "Microsoft.ServiceModel.Samples.EtwTraceListener, ETWTraceListener"  
            name="ETW" traceOutputOptions="Timestamp">  
            <filter type="" />  
       </add>  
    </sharedListeners>  
</system.diagnostics>  
```  
  
 Antes de usar esse ouvinte, uma sessão de rastreamento ETW deve ser iniciada. Essa sessão pode ser iniciada usando logman. exe ou tracelog. exe. Um arquivo SetupETW. bat está incluído neste exemplo para que você possa configurar a sessão de rastreamento ETW junto com um arquivo CleanupETW. bat para fechar a sessão e concluir o arquivo de log.  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico. Para obter mais informações sobre essas ferramentas, consulte<https://go.microsoft.com/fwlink/?LinkId=56580>  
  
 Ao usar o ETWTraceListener, os rastreamentos são registrados em arquivos. etl binários. Com o rastreamento de ServiceModel ativado, todos os rastreamentos gerados aparecem no mesmo arquivo. Use a [ferramenta Visualizador de rastreamento de serviço (SvcTraceViewer. exe)](../../../../docs/framework/wcf/service-trace-viewer-tool-svctraceviewer-exe.md) para exibir arquivos de log. ETL e. svclog Connector. O visualizador cria uma exibição de ponta a ponta do sistema que torna possível rastrear uma mensagem de sua origem para seu destino e ponto de consumo.  
  
 O ouvinte de rastreamento ETW dá suporte ao log circular. Para habilitar esse recurso, vá para **Iniciar**, **executar** e digite `cmd` para iniciar um console de comando. No comando a seguir, substitua o `<logfilename>` parâmetro pelo nome do arquivo de log.  
  
```  
logman create trace Wcf -o <logfilename> -p "{411a0819-c24b-428c-83e2-26b41091702e}" -f bincirc -max 1000  
```  
  
 As `-f` opções `-max` e são opcionais. Eles especificam o formato circular binário e o tamanho máximo do log de 1000 MB, respectivamente. A `-p` opção é usada para especificar o provedor de rastreamento. Em nosso exemplo, `"{411a0819-c24b-428c-83e2-26b41091702e}"` é o GUID para "provedor de exemplo de ETW XML".  
  
 Para iniciar a sessão, digite o comando a seguir.  
  
```  
Logman start Wcf  
```  
  
 Depois de concluir o registro em log, você pode interromper a sessão com o comando a seguir.  
  
```  
Logman stop Wcf  
```  
  
 Esse processo gera logs circulares binários que você pode processar com sua ferramenta de escolha, incluindo a [ferramenta do Visualizador de rastreamento de serviço (SvcTraceViewer. exe)](../../../../docs/framework/wcf/service-trace-viewer-tool-svctraceviewer-exe.md) ou tracerpt.  
  
 Você também pode examinar o exemplo de [rastreamento circular](../../../../docs/framework/wcf/samples/circular-tracing.md) para obter mais informações sobre um ouvinte alternativo para executar o log circular.  
  
### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Certifique-se de ter executado o [procedimento de configuração única para os exemplos de Windows Communication Foundation](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Para compilar a solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](../../../../docs/framework/wcf/samples/building-the-samples.md).  
  
    > [!NOTE]
    > Para usar os comandos RegisterProvider. bat, SetupETW. bat e CleanupETW. bat, você deve executar sob uma conta de administrador local. Se você estiver usando [!INCLUDE[wv](../../../../includes/wv-md.md)] o ou posterior, também deverá executar o prompt de comando com privilégios elevados. Para fazer isso, clique com o botão direito do mouse no ícone do prompt de comando e clique em **Executar como administrador**.  
  
3. Antes de executar o exemplo, execute RegisterProvider. bat no cliente e no servidor. Isso configura o arquivo resultante ETWTracingSampleLog. ETL para gerar rastreamentos que podem ser lidos pelo Visualizador de rastreamento de serviço. Esse arquivo pode ser encontrado na pasta C:\Logs. Se essa pasta não existir, ela deverá ser criada ou nenhum rastreamento será gerado. Em seguida, execute SetupETW. bat nos computadores cliente e servidor para iniciar a sessão de rastreamento ETW. O arquivo SetupETW. bat pode ser encontrado na pasta CS\Client.  
  
4. Para executar o exemplo em uma configuração de computador único ou entre computadores, siga as instruções em [executando os exemplos de Windows Communication Foundation](../../../../docs/framework/wcf/samples/running-the-samples.md).  
  
5. Quando o exemplo for concluído, execute CleanupETW. bat para concluir a criação do arquivo ETWTracingSampleLog. etl.  
  
6. Abra o arquivo ETWTracingSampleLog. etl de dentro do Visualizador de rastreamento de serviço. Você será solicitado a salvar o arquivo formatado binário como um arquivo. svclog Connector.  
  
7. Abra o arquivo. svclog Connector recém-criado de dentro do Visualizador de rastreamento de serviço para exibir os rastreamentos ETW e ServiceModel.  
  
> [!IMPORTANT]
> Os exemplos podem mais ser instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://go.microsoft.com/fwlink/?LinkId=150780) para baixar todos os Windows Communication Foundation (WCF) [!INCLUDE[wf1](../../../../includes/wf1-md.md)] e exemplos. Este exemplo está localizado no seguinte diretório.  
>   
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Management\AnalyticTrace`  
  
## <a name="see-also"></a>Consulte também

- [Exemplos de monitoramento do AppFabric](https://go.microsoft.com/fwlink/?LinkId=193959)
