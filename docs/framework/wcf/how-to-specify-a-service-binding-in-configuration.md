---
title: 'Como: especificar uma associação de serviço na configuração'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 885037f7-1c2b-4d7a-90d9-06b89be172f2
ms.openlocfilehash: d3224b1d732fb82ffe68e8ce0bd410850004cb95
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69967164"
---
# <a name="how-to-specify-a-service-binding-in-configuration"></a>Como: especificar uma associação de serviço na configuração
Neste exemplo, um `ICalculator` contrato é definido para um serviço de calculadora básica, o serviço é implementado `CalculatorService` na classe e, em seguida, seu ponto de extremidade é configurado no arquivo Web. config, onde é especificado que o serviço usa <xref:System.ServiceModel.BasicHttpBinding> o . Para obter uma descrição de como configurar esse serviço usando o código em vez de uma configuração [, consulte Como: Especifique uma associação de serviço no](../../../docs/framework/wcf/how-to-specify-a-service-binding-in-code.md)código.  
  
 Geralmente, é a prática recomendada especificar a associação e as informações de endereço de forma declarativa na configuração, em vez de imperativa no código. A definição de pontos de extremidade no código geralmente não é prática porque as associações e os endereços para um serviço implantado são normalmente diferentes daqueles usados enquanto o serviço está sendo desenvolvido. Em geral, manter as informações de vinculação e endereçamento do código permite que elas sejam alteradas sem a necessidade de recompilar ou reimplantar o aplicativo.  
  
 Todas as etapas de configuração a seguir podem ser executadas usando a [ferramenta do editor de configuração (SvcConfigEditor. exe)](../../../docs/framework/wcf/configuration-editor-tool-svcconfigeditor-exe.md).  
  
 Para a cópia de origem deste exemplo, consulte [BasicBinding](../../../docs/framework/wcf/samples/basicbinding.md).  
  
### <a name="to-specify-the-basichttpbinding-to-use-to-configure-the-service"></a>Para especificar o BasicHttpBinding a ser usado para configurar o serviço  
  
1. Defina um contrato de serviço para o tipo de serviço.  
  
     [!code-csharp[C_HowTo_ConfigureServiceBinding#1](../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_configureservicebinding/cs/source.cs#1)]
     [!code-vb[C_HowTo_ConfigureServiceBinding#1](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_configureservicebinding/vb/source.vb#1)]  
  
2. Implemente o contrato de serviço em uma classe de serviço.  
  
     [!code-csharp[C_HowTo_ConfigureServiceBinding#2](../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_configureservicebinding/cs/source.cs#2)]
     [!code-vb[C_HowTo_ConfigureServiceBinding#2](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_configureservicebinding/vb/source.vb#2)]  
  
    > [!NOTE]
    > As informações de endereço ou de associação não são especificadas dentro da implementação do serviço. Além disso, o código não precisa ser escrito para buscar essas informações do arquivo de configuração.  
  
3. Crie um arquivo Web. config para configurar um ponto de extremidade `CalculatorService` para o que <xref:System.ServiceModel.WSHttpBinding>usa o.  
  
    ```xml  
    <?xml version="1.0" encoding="utf-8" ?>  
    <configuration>  
      <system.serviceModel>  
        <services>  
          <service name=" CalculatorService" >  
            <endpoint   
            <!-- Leave the address blank to be populated by default -->  
            <!-- from the hosting environment,in this case IIS, so -->  
            <!-- the address will just be that of the IIS Virtual -->  
            <!-- Directory. -->  
                address=""   
            <!-- Specify the binding type -->  
                binding="wsHttpBinding"  
            <!-- Specify the binding configuration name for that -->  
            <!-- binding type. This is optional but useful if you -->  
            <!-- want to modify the properties of the binding. -->  
            <!-- The bindingConfiguration name Binding1 is defined -->  
            <!-- below in the bindings element. -->  
                bindingConfiguration="Binding1"  
                contract="ICalculator" />  
          </service>  
        </services>  
        <bindings>  
          <wsHttpBinding>  
            <binding name="Binding1">  
              <!-- Binding property values can be modified here. -->  
              <!-- See the next procedure. -->  
            </binding>  
          </wsHttpBinding>  
       </bindings>  
      </system.serviceModel>  
    </configuration>  
    ```  
  
4. Crie um arquivo Service. svc que contenha a linha a seguir e coloque-o em seu diretório virtual Serviços de Informações da Internet (IIS).  
  
    ```  
    <%@ServiceHost language=c# Service="CalculatorService" %>   
    ```  
  
### <a name="to-modify-the-default-values-of-the-binding-properties"></a>Para modificar os valores padrão das propriedades de associação  
  
1. Para modificar um dos valores de propriedade padrão do <xref:System.ServiceModel.WSHttpBinding>, crie um novo `<binding name="Binding1">` nome de configuração de associação, dentro do [ \<elemento WSHttpBinding >](../../../docs/framework/configure-apps/file-schema/wcf/wshttpbinding.md) e defina os novos valores para os atributos da Associação nesta associação elementos. Por exemplo, para alterar os valores de tempo limite de abertura e fechamento padrão de 1 minuto para 2 minutos, adicione o seguinte ao arquivo de configuração.  
  
    ```xml  
    <wsHttpBinding>  
      <binding name="Binding1"  
               closeTimeout="00:02:00"  
               openTimeout="00:02:00">  
      </binding>  
    </wsHttpBinding>  
    ```  
  
## <a name="see-also"></a>Consulte também

- [Usando associações para configurar serviços e clientes](../../../docs/framework/wcf/using-bindings-to-configure-services-and-clients.md)
- [Especificando um endereço do ponto de extremidade](../../../docs/framework/wcf/specifying-an-endpoint-address.md)
