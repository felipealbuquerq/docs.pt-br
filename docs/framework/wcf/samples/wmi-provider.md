---
title: Provedor de WMI
ms.date: 03/30/2017
ms.assetid: 462f0db3-f4a4-4a4b-ac26-41fc25c670a4
ms.openlocfilehash: 89e2d370919519953e714cb0d0020587b3f53c9d
ms.sourcegitcommit: 581ab03291e91983459e56e40ea8d97b5189227e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2019
ms.locfileid: "70038509"
---
# <a name="wmi-provider"></a>Provedor de WMI
Este exemplo demonstra como coletar dados de serviços Windows Communication Foundation (WCF) em tempo de execução usando o provedor de Instrumentação de Gerenciamento do Windows (WMI) que é incorporado ao WCF. Além disso, este exemplo demonstra como adicionar um objeto WMI definido pelo usuário a um serviço. O exemplo ativa o provedor WMI para o [introdução](../../../../docs/framework/wcf/samples/getting-started-sample.md) e demonstra como coletar dados do `ICalculator` serviço em tempo de execução.  
  
 O WMI é a implementação do padrão Web-Based Enterprise Management (WBEM) da Microsoft. Para obter mais informações sobre o SDK do WMI, consulte [Instrumentação de gerenciamento do Windows](/windows/desktop/WmiSdk/wmi-start-page). O WBEM é um padrão do setor para a forma como os aplicativos expõem a instrumentação de gerenciamento para ferramentas de gerenciamento externas.  
  
 O WCF implementa um provedor WMI, um componente que expõe a instrumentação em tempo de execução por meio de uma interface compatível com o WBEM. As ferramentas de gerenciamento podem se conectar aos serviços por meio da interface em tempo de execução. O WCF expõe atributos de serviços como endereços, associações, comportamentos e ouvintes.  
  
 O provedor WMI interno é ativado no arquivo de configuração do aplicativo. Isso é feito por meio `wmiProviderEnabled` do atributo [ \<do > de diagnóstico](../../../../docs/framework/configure-apps/file-schema/wcf/diagnostics.md) na [ \<seção System. ServiceModel >](../../../../docs/framework/configure-apps/file-schema/wcf/system-servicemodel.md) , conforme mostrado na seguinte configuração de exemplo:  
  
```xml  
<system.serviceModel>  
    ...  
    <diagnostics wmiProviderEnabled="true" />  
    ...  
</system.serviceModel>  
```  
  
 Essa entrada de configuração expõe uma interface WMI. Os aplicativos de gerenciamento agora podem se conectar por meio dessa interface e acessar a instrumentação de gerenciamento do aplicativo.  
  
## <a name="custom-wmi-object"></a>Objeto WMI personalizado  
 Adicionar objetos WMI a um serviço torna possível revelar informações definidas pelo usuário juntamente com as informações internas do provedor WMI. Isso é feito por meio da publicação do esquema do serviço no WMI usando o aplicativo InstallUtil. exe. Instruções para fazer isso, juntamente com mais detalhes, podem ser encontradas nas instruções de instalação no final do tópico.  
  
## <a name="accessing-wmi-information"></a>Acessando informações do WMI  
 Os dados do WMI podem ser acessados de várias maneiras diferentes. A Microsoft fornece APIs WMI para scripts, Visual Basic aplicativos C++ , aplicativos e o .NET Framework (https://docs.microsoft.com/windows/desktop/wmisdk/using-wmi).  
  
 Este exemplo usa dois scripts java: um para enumerar serviços em execução no computador junto com algumas de suas propriedades e o segundo para exibir dados WMI definidos pelo usuário. O script abre uma conexão com o provedor WMI, analisa dados e exibe os dados coletados.  
  
 Inicie o exemplo para criar uma instância em execução de um serviço WCF. Enquanto o serviço estiver em execução, execute cada script Java usando o seguinte comando no prompt de comando:  
  
```  
cscript EnumerateServices.js  
```  
  
 O script acessa a instrumentação contida no serviço e produz a seguinte saída:  
  
```  
Microsoft (R) Windows Script Host Version 5.6  
Copyright © Microsoft Corporation 1996-2001. All rights reserved.  
  
1 service(s) found.  
|-PID:           5776  
|-DistinguishedName:  CalculatorService@http://localhost/ServiceModelSamples/service.svc  
|-Endpoints:     1 endpoints  
  |-CalculatorService.ICalculator@http://localhost/ServiceModelSamples/service.svc  
    |-Address:                        http://localhost/ServiceModelSamples/service.svc  
    |-CounterInstanceName:  
    |-AddressHeaders:                 0  
    |-ContractType:                   Contract.Name='ICalculator'  
    |-BindingElements:                4 bindings  
      |-BindingElements[0]  
        |-Type:                       TransactionFlowBindingElement  
      |-BindingElements[1]  
        |-Type:                       SymmetricSecurityBindingElement  
      |-BindingElements[2]  
        |-Type:                       TextMessageEncodingBindingElement  
        |-MaxReadPoolSize:            64  
        |-MaxWritePoolSize:           16  
      |-BindingElements[3]  
        |-Type:                       HttpTransportBindingElement  
        |-ManualAddressing:           false  
        |-MaxBufferSize:              65536  
        |-AllowCookies:               false  
        |-AuthenticationScheme:       Anonymous  
        |-BypassProxyOnLocal:         false  
        |-HostNameComparisonMode:     StrongWildcard  
        |-ProxyAddress:               null  
        |-ProxyAuthenticationScheme:  Anonymous  
        |-Realm:  
        |-TransferMode:               Buffered  
        |-UseDefaultWebProxy:         true  
|-Behaviors:     5 behaviors  
      |-Behavior[0]  
      |-Type:                       ServiceBehaviorAttribute  
        |-AddressFilterMode:               Exact  
        |-AutomaticSessionShutdown:        true  
        |-ConcurrencyMode:                 Single  
        |-IncludeExceptionDetailInFaults:  false  
        |-InstanceContextMode:             PerSession  
        |-TransactionIsolationLevel:       Unspecified  
        |-TransactionTimeout:              null  
        |-ValidateMustUnderstand:          true  
      |-Behavior[1]  
      |-Type:                       AspNetCompatibilityRequirementsAttribute  
      |-Behavior[2]  
      |-Type:                       ServiceDebugBehavior  
      |-Behavior[3]  
      |-Type:                       ServiceAuthorizationBehavior  
      |-Behavior[4]  
      |-Type:                       Behavior  
```  
  
 Em seguida, execute o segundo script Java para exibir os dados do WMI definidos pelo usuário:  
  
```  
cscript EnumerateCustomObjects.js  
```  
  
 O script acessa a instrumentação definida pelo usuário contida nos serviços e produz a seguinte saída:  
  
```  
1 WMIObject(s) found.  
|-PID:           30285bfd-9d66-4c4e-9be2-310499c5cef5  
|-InstanceId:    3839  
|-WMIInfo:       User Defined WMI Information.  
```  
  
 A saída mostra que há um único serviço em execução no computador. O serviço expõe um ponto de extremidade que `ICalculator` implementa o contrato. As configurações do comportamento e da Associação implementadas pelo ponto de extremidade são listadas como a soma de elementos individuais da pilha de mensagens.  
  
 O WMI não se limita a expor a instrumentação de gerenciamento da infraestrutura do WCF. O aplicativo pode expor seus próprios itens de dados específicos de domínio por meio do mesmo mecanismo. O WMI é um mecanismo unificado para inspeção e controle de um serviço Web.  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Para compilar a C# edição do ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](../../../../docs/framework/wcf/samples/building-the-samples.md).  
  
3. Publique o esquema de serviços no WMI executando o InstallUtil. exe (os locais padrão para InstallUtil. exe são "%WINDIR%\Microsoft.NET\Framework\v4.0.30319") no arquivo Service. dll no diretório de hospedagem. Esta etapa só precisa ser executada quando foram feitas alterações no arquivo Service. dll.
  
4. Para executar o exemplo em uma configuração de computador único ou entre computadores, siga as instruções em [executando os exemplos de Windows Communication Foundation](../../../../docs/framework/wcf/samples/running-the-samples.md).  
  
    > [!NOTE]
    > Se você instalou o WCF depois de instalar o ASP.NET, talvez seja necessário executar "% WINDIR% \ Microsoft. Net\Framework\v3.0\Windows Communication Foundation\servicemodelreg.exe "-r-x para conceder à conta ASPNET permissão para publicar objetos WMI.  
  
5. Exiba os dados do exemplo na superfície por meio do WMI usando os comandos `cscript EnumerateServices.js` : `cscript EnumerateCustomObjects.js`ou.  
  
> [!IMPORTANT]
> Os exemplos podem mais ser instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://go.microsoft.com/fwlink/?LinkId=150780) para baixar todos os Windows Communication Foundation (WCF) [!INCLUDE[wf1](../../../../includes/wf1-md.md)] e exemplos. Este exemplo está localizado no seguinte diretório.  
>   
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Management\WMIProvider`  
  
## <a name="see-also"></a>Consulte também

- [Exemplos de monitoramento do AppFabric](https://go.microsoft.com/fwlink/?LinkId=193959)
