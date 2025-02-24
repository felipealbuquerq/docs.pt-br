---
title: 'Como: especificar credenciais de segurança de canal'
ms.date: 03/30/2017
ms.assetid: f8e03f47-9c4f-4dd5-8f85-429e6d876119
ms.openlocfilehash: 3d6131a7488d9932118a988095791dd06fd46c49
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69933456"
---
# <a name="how-to-specify-channel-security-credentials"></a>Como: especificar credenciais de segurança de canal
O moniker do serviço Windows Communication Foundation (WCF) permite que aplicativos COM chamem serviços WCF. A maioria dos serviços WCF exige que o cliente especifique credenciais para autenticação e autorização. Ao chamar um serviço WCF de um cliente WCF, você pode especificar essas credenciais em código gerenciado ou em um arquivo de configuração de aplicativo. Ao chamar um serviço WCF de um aplicativo com, você pode usar a <xref:System.ServiceModel.ComIntegration.IChannelCredentials> interface para especificar as credenciais. Este tópico ilustra várias maneiras de especificar credenciais usando a <xref:System.ServiceModel.ComIntegration.IChannelCredentials> interface.  
  
> [!NOTE]
> <xref:System.ServiceModel.ComIntegration.IChannelCredentials>é uma interface baseada em IDispatch e você não terá a funcionalidade do IntelliSense no ambiente do Visual Studio.  
  
 Este artigo usará o serviço WCF definido no [exemplo de segurança de mensagem](../../../../docs/framework/wcf/samples/message-security-sample.md).  
  
### <a name="to-specify-a-client-certificate"></a>Para especificar um certificado de cliente  
  
1. Execute o arquivo setup. bat no diretório segurança da mensagem para criar e instalar os certificados de teste necessários.  
  
2. Abra o projeto de segurança da mensagem.  
  
3. Adicione `[ServiceBehavior(Namespace="http://Microsoft.ServiceModel.Samples")]` à definição `ICalculator` de interface.  
  
4. Adicione `bindingNamespace="http://Microsoft.ServiceModel.Samples"` à marca do ponto de extremidade no app. config do serviço.  
  
5. Crie o exemplo de segurança de mensagem e execute Service. exe. Use o Internet Explorer e navegue até o URI do serviço http://localhost:8000/ServiceModelSamples/Service) (para garantir que o serviço esteja funcionando.  
  
6. Abra Visual Basic 6,0 e crie um novo arquivo Standard. exe. Adicione um botão ao formulário e clique duas vezes no botão para adicionar o seguinte código ao manipulador de cliques:  
  
    ```  
        monString = "service:mexAddress=http://localhost:8000/ServiceModelSamples/Service?wsdl"  
        monString = monString + ", address=http://localhost:8000/ServiceModelSamples/Service"  
        monString = monString + ", contract=ICalculator, contractNamespace=http://Microsoft.ServiceModel.Samples"  
        monString = monString + ", binding=BasicHttpBinding_ICalculator, bindingNamespace=http://Microsoft.ServiceModel.Samples"  
  
        Set monikerProxy = GetObject(monString)  
  
        'Set the Service Certificate.  
     monikerProxy.ChannelCredentials.SetServiceCertificateAuthentication "CurrentUser", "NoCheck", "PeerOrChainTrust"  
    monikerProxy.ChannelCredentials.SetDefaultServiceCertificateFromStore "CurrentUser", "TrustedPeople", "FindBySubjectName", "localhost"  
  
        'Set the Client Certificate.  
        monikerProxy.ChannelCredentials.SetClientCertificateFromStoreByName "CN=client.com", "CurrentUser", "My"  
        MsgBox monikerProxy.Add(3, 4)  
    ```  
  
7. Execute o aplicativo Visual Basic e verifique os resultados.  
  
     O aplicativo Visual Basic exibirá uma caixa de mensagem com o resultado de chamar Add (3, 4). <xref:System.ServiceModel.ComIntegration.IChannelCredentials.SetClientCertificateFromFile%28System.String%2CSystem.String%2CSystem.String%29>ou <xref:System.ServiceModel.ComIntegration.IChannelCredentials.SetClientCertificateFromStoreByName%28System.String%2CSystem.String%2CSystem.String%29> também pode ser usado no lugar de <xref:System.ServiceModel.ComIntegration.IChannelCredentials.SetClientCertificateFromStore%28System.String%2CSystem.String%2CSystem.String%2CSystem.Object%29> para definir o certificado do cliente:  
  
    ```  
    monikerProxy.ChannelCredentials.SetClientCertificateFromFile "C:\MyClientCert.pfx", "password", "DefaultKeySet"  
    ```  
  
> [!NOTE]
> Para que essa chamada funcione, o certificado do cliente precisa ser confiável no computador em que o cliente está sendo executado.  
  
> [!NOTE]
> Se o moniker estiver malformado ou se o serviço estiver indisponível, a `GetObject` chamada para retornará um erro dizendo "sintaxe inválida". Se você receber esse erro, verifique se o moniker que você está usando está correto e se o serviço está disponível.  
  
### <a name="to-specify-user-name-and-password"></a>Para especificar o nome de usuário e a senha  
  
1. Modifique o arquivo app. config do serviço para usar `wsHttpBinding`o. Isso é necessário para a validação de nome de usuário e senha:  

2. Defina o `clientCredentialType` como nome de usuário:  

3. Abra Visual Basic 6,0 e crie um novo arquivo Standard. exe. Adicione um botão ao formulário e clique duas vezes no botão para adicionar o seguinte código ao manipulador de cliques:  
  
    ```  
    monString = "service:mexAddress=http://localhost:8000/ServiceModelSamples/Service?wsdl"  
    monString = monString + ", address=http://localhost:8000/ServiceModelSamples/Service"  
    monString = monString + ", contract=ICalculator, contractNamespace=http://Microsoft.ServiceModel.Samples"  
    monString = monString + ", binding=WSHttpBinding_ICalculator, bindingNamespace=http://Microsoft.ServiceModel.Samples"  
  
    Set monikerProxy = GetObject(monString)  
  
    monikerProxy.ChannelCredentials.SetServiceCertificateAuthentication "CurrentUser", "NoCheck", "PeerOrChainTrust"  
    monikerProxy.ChannelCredentials.SetUserNameCredential "username", "password"  
  
    MsgBox monikerProxy.Add(3, 4)  
    ```  
  
4. Execute o aplicativo Visual Basic e verifique os resultados. O aplicativo Visual Basic exibirá uma caixa de mensagem com o resultado de chamar Add (3, 4).  
  
    > [!NOTE]
    > A associação especificada no moniker do serviço neste exemplo foi alterada para WSHttpBinding_ICalculator. Observe também que você deve fornecer um nome de usuário e senha válidos na chamada para <xref:System.ServiceModel.ComIntegration.IChannelCredentials.SetUserNameCredential%28System.String%2CSystem.String%29>.  
  
### <a name="to-specify-windows-credentials"></a>Para especificar as credenciais do Windows  
  
1. Defina `clientCredentialType` como Windows no arquivo app. config do serviço:  

2. Abra Visual Basic 6,0 e crie um novo arquivo Standard. exe. Adicione um botão ao formulário e clique duas vezes no botão para adicionar o seguinte código ao manipulador de cliques:  
  
    ```  
    monString = "service:mexAddress=http://localhost:8000/ServiceModelSamples/Service?wsdl"  
    monString = monString + ", address=http://localhost:8000/ServiceModelSamples/Service"  
    monString = monString + ", contract=ICalculator, contractNamespace=http://Microsoft.ServiceModel.Samples"  
    monString = monString + ", binding=WSHttpBinding_ICalculator, bindingNamespace=http://Microsoft.ServiceModel.Samples"  
    monString = monString + ", upnidentity=domain\userID"  
  
    Set monikerProxy = GetObject(monString)  
     monikerProxy.ChannelCredentials.SetWindowsCredential "domain", "userID", "password", 1, True  
  
    MsgBox monikerProxy.Add(3, 4)  
    ```  
  
3. Execute o aplicativo Visual Basic e verifique os resultados. O aplicativo Visual Basic exibirá uma caixa de mensagem com o resultado de chamar Add (3, 4).  
  
    > [!NOTE]
    > Você deve substituir "Domain", "userID" e "password" por valores válidos.  
  
### <a name="to-specify-an-issue-token"></a>Para especificar um token de problema  
  
1. Tokens de emissão são usados somente para aplicativos que usam segurança federada. Para obter mais informações sobre segurança federada, consulte [Federação e tokens emitidos](../../../../docs/framework/wcf/feature-details/federation-and-issued-tokens.md) e [exemplo de Federação](../../../../docs/framework/wcf/samples/federation-sample.md).  
  
     O exemplo de código a seguir Visual Basic ilustra como chamar <xref:System.ServiceModel.ComIntegration.IChannelCredentials.SetIssuedToken%28System.String%2CSystem.String%2CSystem.String%29> o método:  
  
    ```  
        monString = "service:mexAddress=http://localhost:8000/ServiceModelSamples/Service?wsdl"  
        monString = monString + ", address=http://localhost:8000/SomeService/Service"  
        monString = monString + ", contract=ICalculator, contractNamespace=http://SomeService.Samples"  
        monString = monString + ", binding=WSHttpBinding_ISomeContract, bindingNamespace=http://SomeService.Samples"  
  
        Set monikerProxy = GetObject(monString)  
    monikerProxy.SetIssuedToken("http://somemachine/sts", "bindingType", "binding")  
    ```  
  
     Para obter mais informações sobre os parâmetros para esse método, <xref:System.ServiceModel.ComIntegration.IChannelCredentials.SetIssuedToken%28System.String%2CSystem.String%2CSystem.String%29>consulte.  
  
## <a name="see-also"></a>Consulte também

- [Federação](../../../../docs/framework/wcf/feature-details/federation.md)
- [Como: Configurar credenciais em um Serviço de Federação](../../../../docs/framework/wcf/feature-details/how-to-configure-credentials-on-a-federation-service.md)
- [Como: Criar um cliente federado](../../../../docs/framework/wcf/feature-details/how-to-create-a-federated-client.md)
- [Segurança de mensagem](../../../../docs/framework/wcf/feature-details/message-security-in-wcf.md)
- [Associações e segurança](../../../../docs/framework/wcf/feature-details/bindings-and-security.md)
