---
title: <message> de <basicHttpBinding>
ms.date: 03/30/2017
ms.assetid: 51cdd329-6461-471a-8747-56c2299b61e5
ms.openlocfilehash: bce80d96b1bcec0d580f2de3fe88d5fd6ad0a3b5
ms.sourcegitcommit: 093571de904fc7979e85ef3c048547d0accb1d8a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70400271"
---
# <a name="message-of-basichttpbinding"></a>\<> de mensagem \<de BasicHttpBinding >
Define as configurações de segurança em nível de mensagem do [ \<> BasicHttpBinding](basichttpbinding.md).  
  
[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<> de System. serviceModel**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[ **\<associações >** ](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[ **\<basicHttpBinding >** ](basichttpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **\<> de associação**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[ **\<> de segurança**](security-of-basichttpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **\<> de mensagem**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<message algorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15"
         clientCredentialType="UserName/Certificate" />
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|algorithmSuite|Define a criptografia de mensagem e os algoritmos de encapsulamento de chaves. Esse atributo é do tipo <xref:System.ServiceModel.Security.SecurityAlgorithmSuite>, que especifica os algoritmos e os tamanhos de chave. Esses algoritmos são mapeados para aqueles especificados na especificação do WS-SecurityPolicy (Security Policy Language).<br /><br /> O valor padrão é `Basic256`.|  
|clientCredentialType|Especifica o tipo de credencial a ser usado ao executar a autenticação de cliente usando a segurança baseada em mensagem. O padrão é `UserName`.|  
  
## <a name="clientcredentialtype-attribute"></a>Atributo clientCredentialtype  
  
|Valor|Descrição|  
|-----------|-----------------|  
|UserName|-Requer que o cliente seja autenticado no servidor com uma credencial de nome de usuário. Essa credencial precisa ser especificada usando o [ \<> ClientCredentials](clientcredentials.md).<br />-O WCF não dá suporte ao envio de um resumo de senha ou à derivação de chaves usando senhas e usando essas chaves para segurança de mensagem. Portanto, o WCF impõe que o transporte seja protegido ao usar credenciais de nome de usuário. Para o `basicHttpBinding`, isso requer a configuração de um canal SSL.|  
|Certificate|Requer que o cliente seja autenticado no servidor usando um certificado. Nesse caso, a credencial do cliente precisa ser especificada usando [ \<ClientCredentials >](clientcredentials.md) e o [ \<> clientCertificate](clientcertificate-of-servicecredentials.md). Além disso, ao usar o modo de segurança de mensagem, o cliente precisa ser provisionado com o certificado de serviço. Nesse caso, a credencial de serviço precisa ser especificada <xref:System.ServiceModel.Description.ClientCredentials> usando o `ClientCredentials` elemento Class ou Behavior e especificando o certificado de serviço usando o [ \<>](servicecertificate-of-servicecredentials.md)do Service-Certificate.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<security>](security-of-basichttpbinding.md)|Define os recursos de segurança para o [ \<> BasicHttpBinding](basichttpbinding.md).|  
  
## <a name="example"></a>Exemplo  
 Este exemplo demonstra como implementar um aplicativo que usa a basicHttpBinding e a segurança da mensagem. No exemplo de configuração a seguir para um serviço, a definição do ponto de extremidade especifica basicHttpBinding e faz referência `Binding1`a uma configuração de associação denominada. O certificado que o serviço usa para se autenticar para o cliente é definido na `behaviors` seção do arquivo de configuração sob o `serviceCredentials` elemento. O modo de validação que se aplica ao certificado que o cliente usa para se autenticar para o serviço também é definido `behaviors` na seção sob `clientCertificate` o elemento.  
  
 Os mesmos detalhes de ligação e segurança são especificados no arquivo de configuração do cliente.  
  
```xml  
<system.serviceModel>
  <services>
    <service name="Microsoft.ServiceModel.Samples.CalculatorService"
             behaviorConfiguration="CalculatorServiceBehavior">
      <host>
        <baseAddresses>
          <add baseAddress="http://localhost:8000/ServiceModelSamples/service" />
        </baseAddresses>
      </host>
      <!-- this endpoint is exposed at the base address provided by host: http://localhost:8000/ServiceModelSamples/service -->
      <endpoint address=""
                binding="basicHttpBinding"
                bindingConfiguration="Binding1"
                contract="Microsoft.ServiceModel.Samples.ICalculator" />
      <!-- the mex endpoint is exposed at http://localhost:8000/ServiceModelSamples/service/mex -->
      <endpoint address="mex"
                binding="mexHttpBinding"
                contract="IMetadataExchange" />
    </service>
  </services>
  <bindings>
    <basicHttpBinding>
    <!-- This configuration defines the SecurityMode as Message and
         the clientCredentialType as Certificate. -->
      <binding name="Binding1">
        <security mode = "Message">
          <message clientCredentialType="Certificate" />
        </security>
      </binding>
    </basicHttpBinding>
  </bindings>
  <!--For debugging purposes set the includeExceptionDetailInFaults attribute to true-->
  <behaviors>
    <serviceBehaviors>
      <behavior name="CalculatorServiceBehavior">
        <serviceMetadata httpGetEnabled="True" />
        <serviceDebug includeExceptionDetailInFaults="False" />
        <!-- The serviceCredentials behavior allows one to define a service certificate.
             A service certificate is used by a client to authenticate the service and provide message protection.
             This configuration references the "localhost" certificate installed during the setup instructions. -->
        <serviceCredentials>
          <serviceCertificate findValue="localhost"
                              storeLocation="LocalMachine"
                              storeName="My"
                              x509FindType="FindBySubjectName" />
          <clientCertificate>
            <!-- Setting the certificateValidationMode to PeerOrChainTrust means that if the certificate
               is in the user's Trusted People store, then it will be trusted without performing a
               validation of the certificate's issuer chain. This setting is used here for convenience so that the
               sample can be run without having to have certificates issued by a certification authority (CA).
               This setting is less secure than the default, ChainTrust. The security implications of this
               setting should be carefully considered before using PeerOrChainTrust in production code. -->
            <authentication certificateValidationMode="PeerOrChainTrust" />
          </clientCertificate>
        </serviceCredentials>
      </behavior>
    </serviceBehaviors>
  </behaviors>
</system.serviceModel>
```  
  
## <a name="see-also"></a>Consulte também

- <xref:System.ServiceModel.BasicHttpMessageSecurity>
- <xref:System.ServiceModel.Configuration.BasicHttpSecurityElement.Message%2A>
- <xref:System.ServiceModel.BasicHttpSecurity.Message%2A>
- <xref:System.ServiceModel.Configuration.BasicHttpMessageSecurityElement>
- [Protegendo serviços e clientes](../../../wcf/feature-details/securing-services-and-clients.md)
- [Associações](../../../wcf/bindings.md)
- [Configurando associações fornecidas pelo sistema](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [Usando associações para configurar serviços e clientes](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](../../../misc/binding.md)
