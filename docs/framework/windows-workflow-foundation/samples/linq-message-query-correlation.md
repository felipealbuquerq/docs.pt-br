---
title: Correlação de consulta de mensagem LINQ
ms.date: 03/30/2017
ms.assetid: b746872e-57b1-4514-b337-53398a0e0deb
ms.openlocfilehash: b758296970340890403557770f91237c953f5d91
ms.sourcegitcommit: 581ab03291e91983459e56e40ea8d97b5189227e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2019
ms.locfileid: "70038138"
---
# <a name="linq-message-query-correlation"></a>Correlação de consulta de mensagem LINQ
Este exemplo demonstra como fazer correlação conteudo base que usa uma implementação personalizada de <xref:System.ServiceModel.Dispatcher.MessageQuery> diferentemente de sistema forneceu <xref:System.ServiceModel.XPathMessageQuery>.  
  
## <a name="demonstrates"></a>Demonstra  
 <xref:System.ServiceModel.Dispatcher.MessageQuery>personalizado, correlação conteudo base.  
  
## <a name="discussion"></a>Discussão  
 Este exemplo mostra como estender a classe base de <xref:System.ServiceModel.Dispatcher.MessageQuery> para fins de correlação. A implementação personalizada, `LinqMessageQuery`, permite que os usuários forneçam um XName para localizar dentro de mensagem usando XLinq. Os dados recuperados pela consulta são usados para formar a chave de correlação para distribuir mensagens à instância apropriado de fluxo de trabalho.  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Este exemplo exibe um serviço de fluxo de trabalho usando pontos de extremidade HTTP. Para executar esse exemplo, as ACLs de URL adequadas devem ser adicionadas (consulte Configurando [http e HTTPS](https://go.microsoft.com/fwlink/?LinkId=70353) para obter detalhes), seja executando o Visual Studio como administrador ou executando o comando a seguir em um prompt elevado para adicionar as ACLs apropriadas. Certifique-se de que seu domínio e nome de usuário são substituídos.  
  
    ```  
    netsh http add urlacl url=http://+:8000/ user=%DOMAIN%\%UserName%  
    ```  
  
2. Uma vez que o URL ACLs é adicionado, use as seguintes etapas.  
  
    1. Compile a solução.  
  
    2. Defina vários projetos de inicialização clicando com o botão direito do mouse na solução e selecionando **definir projetos de inicialização**. Adicionar **serviço** e **cliente** (nessa ordem) como vários projetos de inicialização.  
  
    3. Execute o aplicativo. O console de cliente mostra um fluxo de trabalho que envia um pedido e que recebe a identificação de ordem de compra e então confirma posteriormente ordem. A janela de serviço (as solicitações que estão sendo processadas.  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://go.microsoft.com/fwlink/?LinkId=150780) para baixar todos os Windows Communication Foundation (WCF) [!INCLUDE[wf1](../../../../includes/wf1-md.md)] e exemplos. Este exemplo está localizado no seguinte diretório.  
>   
> `<InstallDrive>:\WF_WCF_Samples\WF\Scenario\Services\LinqMessageQueryCorrelation`
