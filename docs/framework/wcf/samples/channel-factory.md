---
title: Fábrica de canais
ms.date: 03/30/2017
ms.assetid: 09b53aa1-b13c-476c-a461-e82fcacd2a8b
ms.openlocfilehash: cd56c47223f0c98e48bd92376c9bbe9db6d2202e
ms.sourcegitcommit: 581ab03291e91983459e56e40ea8d97b5189227e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2019
ms.locfileid: "70045726"
---
# <a name="channel-factory"></a>Fábrica de canais

Este exemplo demonstra como um aplicativo cliente pode criar um canal com a <xref:System.ServiceModel.ChannelFactory> classe em vez de um cliente gerado. Este exemplo é baseado no [introdução](../../../../docs/framework/wcf/samples/getting-started-sample.md) que implementa um serviço de calculadora.

> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.

Este exemplo usa a <xref:System.ServiceModel.ChannelFactory%601> classe para criar um canal para um ponto de extremidade de serviço. Normalmente, para criar um canal para um ponto de extremidade de serviço, você gera um tipo de cliente com a [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md) e cria uma instância do tipo gerado. Você também pode criar um canal usando a <xref:System.ServiceModel.ChannelFactory%601> classe, conforme demonstrado neste exemplo. O serviço criado pelo código de exemplo a seguir é idêntico ao serviço no [introdução](../../../../docs/framework/wcf/samples/getting-started-sample.md).

```csharp
EndpointAddress address = new EndpointAddress("http://localhost/servicemodelsamples/service.svc");
WSHttpBinding binding = new WSHttpBinding();
ChannelFactory<ICalculator> factory = new
                    ChannelFactory<ICalculator>(binding, address);
ICalculator channel = factory.CreateChannel();
```

> [!IMPORTANT]
> Se você estiver executando este exemplo em um cenário de máquina cruzada, deverá substituir "localhost" no código anterior pelo nome totalmente qualificado do computador que está executando o serviço. Este exemplo não usa a configuração para definir o endereço do ponto de extremidade, portanto, isso deve ser feito no código.

Depois que o canal é criado, as operações de serviço podem ser invocadas exatamente como com um cliente gerado.

```csharp
// Call the Add service operation.
double value1 = 100.00D;
double value2 = 15.99D;
double result = channel.Add(value1, value2);
Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);
```

Para fechar o canal, primeiro ele deve ser convertido em uma <xref:System.ServiceModel.IClientChannel> interface. Isso ocorre porque o canal como gerado é declarado no `ICalculator` aplicativo cliente usando a interface, que tem métodos como `Add` e `Subtract` , mas não `Close`. O `Close` método se origina <xref:System.ServiceModel.ICommunicationObject> na interface.

```csharp
// Close the channel.
 ((IClientChannel)client).Close();
```

Quando você executa o exemplo, as solicitações de operação e as respostas são exibidas na janela do console do cliente. Pressione ENTER na janela do cliente para desligar o aplicativo cliente.

```
Add(100,15.99) = 115.99
Subtract(145,76.54) = 68.46
Multiply(9,81.25) = 731.25
Divide(22,7) = 3.14285714285714

Press <ENTER> to terminate client.
```

### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo

1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md).

2. Para compilar a C# edição do ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](../../../../docs/framework/wcf/samples/building-the-samples.md). Observe que esse exemplo não habilita a publicação de metadados. Primeiro, você deve habilitar a publicação de metadados para este exemplo para regenerar o tipo de cliente.

3. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](../../../../docs/framework/wcf/samples/running-the-samples.md).

### <a name="to-run-the-sample-cross-machine"></a>Para executar o exemplo de computador cruzado

1. Substitua "localhost" no código a seguir pelo nome totalmente qualificado do computador que está executando o serviço.

    ```csharp
    EndpointAddress address = new EndpointAddress("http://localhost/servicemodelsamples/service.svc");
    ```

> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://go.microsoft.com/fwlink/?LinkId=150780) para baixar todos os Windows Communication Foundation (WCF) [!INCLUDE[wf1](../../../../includes/wf1-md.md)] e exemplos. Este exemplo está localizado no seguinte diretório.
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Client\ChannelFactory`
