---
title: Acessando os serviços WCF com um aplicativo cliente da Windows Store
ms.date: 03/30/2017
ms.assetid: e2002ef4-5dee-4a54-9d87-03b33d35fc52
ms.openlocfilehash: 9316a46f809eec21f73e8eeadb49baf1748c6ca0
ms.sourcegitcommit: 37616676fde89153f563a485fc6159fc57326fc2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/23/2019
ms.locfileid: "69988251"
---
# <a name="accessing-wcf-services-with-a-windows-store-client-app"></a>Acessando os serviços WCF com um aplicativo cliente da Windows Store
O Windows 8 apresenta um novo tipo de aplicativos chamados aplicativos da Windows Store. Esses aplicativos são criados em torno de uma interface de tela sensível ao toque. O .NET Framework 4.5 permite que aplicativos da Windows Store chamem serviços WCF.  
  
## <a name="wcf-support-in-windows-store-applications"></a>Suporte do WCF em aplicativos da Windows Store  
 Um subconjunto da funcionalidade do WCF está disponível a partir de um aplicativo da Windows Store. Consulte as seções a seguir para obter mais detalhes.  
  
> [!IMPORTANT]
> Use as APIs de agregação de WinRT em vez dessas expostas pelo WCF. Para obter mais informações, consulte [API de distribuição do WinRT](https://go.microsoft.com/fwlink/?LinkId=236265)  
  
> [!WARNING]
> Não há suporte para usar Adicionar Referência de Serviço para adicionar uma referência ao serviço Web para um componente de Tempo de Execução do Windows.  
  
### <a name="supported-bindings"></a>Associações com suporte  
 As seguintes associações do WCF têm suporte em aplicativos da Windows Store:  
  
1. <xref:System.ServiceModel.BasicHttpBinding>  
  
2. <xref:System.ServiceModel.NetTcpBinding>  
  
3. <xref:System.ServiceModel.NetHttpBinding>  
  
4. <xref:System.ServiceModel.Channels.CustomBinding>
  
 Os seguintes elementos de associação têm suporte em aplicativos da Windows Store  
  
1. <xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement>  
  
2. <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement>  
  
3. <xref:System.ServiceModel.Channels.ConnectionOrientedTransportBindingElement>  
  
4. <xref:System.ServiceModel.Channels.SslStreamSecurityBindingElement>  
  
5. <xref:System.ServiceModel.Channels.WindowsStreamSecurityBindingElement>  
  
6. <xref:System.ServiceModel.Channels.TcpTransportBindingElement>  
  
7. <xref:System.ServiceModel.Channels.HttpTransportBindingElement>  
  
8. <xref:System.ServiceModel.Channels.HttpsTransportBindingElement>  
  
9. <xref:System.ServiceModel.Channels.TransportSecurityBindingElement>  
  
 Codificações de texto e binários têm suporte. Todos os modos de transferência de WCF têm suporte. Para obter mais informações, consulte [streaming de transferência de mensagens](../../../../docs/framework/wcf/feature-details/streaming-message-transfer.md).  
  
### <a name="add-service-reference"></a>Adicionar Referência de Serviço  
 Para chamar um serviço WCF de um aplicativo da Windows Store, use o recurso Adicionar Referência de Serviço do Visual Studio 2012. Você observará algumas alterações na funcionalidade de Adicionar Referência de Serviço quando forem feitas dentro de um aplicativo da Windows Store. Nenhum arquivo de configuração é gerado primeiro. Os aplicativos da Windows Store não usam arquivos de configuração. Eles devem ser configurados no código. Este código de configuração pode ser localizado no arquivo References.cs gerado por Adicionar Referência de Serviço. Para ver esse arquivo, certifique-se de selecionar "Mostrar todos os arquivos" no Gerenciador de soluções. O arquivo será localizado nos nós de Referências de Serviço e, em seguida, Reference.svcmap dentro do projeto. Todas as operações geradas para os serviços WCF dentro de um aplicativo da Windows Store serão assíncronas usando o padrão assíncrono baseado em tarefas. Para obter mais informações, consulte [tarefas assíncronas – simplificar a programação assíncrona com tarefas](https://msdn.microsoft.com/magazine/ff959203.aspx).  
  
 Como a configuração agora é gerada no código, todas as alterações feitas no arquivo Reference.cs serão substituídas toda vez que a referência do serviço for atualizada. Para solucionar essa situação, o código de configuração é gerado dentro de um método parcial, que você pode implementar em sua classe de proxy cliente. O método parcial é declarado da seguinte maneira:  
  
```csharp  
static partial void Configure(System.ServiceModel.Description.ServiceEndpoint serviceEndpoint,  
            System.ServiceModel.Description.ClientCredentials clientCredentials);  
```  
  
 Você pode implementar esse método parcial e alterar a associação ou o ponto de extremidade em sua classe de proxy cliente da seguinte maneira:  
  
```csharp  
public partial class Service1Client : System.ServiceModel.ClientBase<MetroWcfClient.ServiceRefMultiEndpt.IService1>, MetroWcfClient.ServiceRefMultiEndpt.IService1  
    {   
        static partial void Configure(System.ServiceModel.Description.ServiceEndpoint serviceEndpoint,   
            System.ServiceModel.Description.ClientCredentials clientCredentials)  
        {  
            if (serviceEndpoint.Name ==   
                    ServiceRefMultiEndpt.Service1Client.EndpointConfiguration.BasicHttpBinding_IService1.ToString())  
            {  
                serviceEndpoint.Binding.SendTimeout = new System.TimeSpan(0, 1, 0);  
            }  
            else if (serviceEndpoint.Name ==   
                    ServiceRefMultiEndpt.Service1Client.EndpointConfiguration.BasicHttpBinding_IService11.ToString())  
            {  
                serviceEndpoint.Binding.SendTimeout = new System.TimeSpan(0, 1, 0);  
                clientCredentials.UserName.UserName = "username1";  
                clientCredentials.UserName.Password = "password";  
            }  
            else if (serviceEndpoint.Name ==   
                    ServiceRefMultiEndpt.Service1Client.EndpointConfiguration.NetTcpBinding_IService1.ToString())  
            {  
                serviceEndpoint.Binding.Name = "MyTcpBinding";  
                serviceEndpoint.Address = new System.ServiceModel.EndpointAddress("net.tcp://localhost/tcp");  
            }  
        }  
    }  
```  
  
### <a name="serialization"></a>Serialização  
 Os seguintes serializadores têm suporte em aplicativos da Windows Store:  
  
1. DataContractSerializer  
  
2. DataContractJsonSerializer  
  
3. XmlSerializer  
  
> [!WARNING]
> XmlDictionaryWriter.Write(DateTime) grava agora o objeto DateTime como uma cadeia de caracteres.  
  
### <a name="security"></a>Segurança  

Os seguintes modos de segurança têm suporte em aplicativos da Windows Store:
  
1. <xref:System.ServiceModel.SecurityMode.None>  
  
2. <xref:System.ServiceModel.SecurityMode.Transport>  
  
3. <xref:System.ServiceModel.SecurityMode.TransportWithMessageCredential>
  
4. <xref:System.ServiceModel.SecurityMode.Message>
  
Os seguintes tipos de credencial de cliente têm suporte em aplicativos da Windows Store:
  
1. Nenhum  
  
2. Basic  
  
3. Digest  
  
4. Negotiate  
  
5. NTLM  
  
6. Windows  
  
7. Username (Segurança de Mensagem)  
  
8. Windows (Segurança de Transporte)  
  
 Para que os aplicativos da Windows Store acessem e enviem credenciais padrão do Windows, você deverá habilitar essa funcionalidade dentro do arquivo Package.appmanifest. Abra esse arquivo e selecione a guia recursos e selecione "credenciais padrão do Windows". Isso permite que o aplicativo se conecte aos recursos de intranet que exigem credenciais de domínio.  
  
> [!IMPORTANT]
> Para que os aplicativos da Windows Store façam chamadas entre computadores, você deve habilitar outra funcionalidade chamada "rede doméstica/corporativa". Essa configuração também está no arquivo Package.appmanifest na guia Recursos. Marque a caixa de seleção Home/Work Networking. Isso concede o acesso de entrada e saída do aplicativo às redes locais confiáveis do usuário como casa e trabalho. As portas críticas de entrada são sempre bloqueadas. Para acessar serviços na Internet, é necessário habilitar o recurso Internet (Cliente).  
  
### <a name="misc"></a>Diversos  
 O uso das seguintes classes tem suporte para aplicativos da Windows Store:  
  
1. <xref:System.ServiceModel.ChannelFactory>  
  
2. <xref:System.ServiceModel.DuplexChannelFactory%601>
  
3. <xref:System.ServiceModel.CallbackBehaviorAttribute>  
  
### <a name="defining-service-contracts"></a>Definindo contratos de serviço  
 É recomendável somente a definição de operações de serviço assíncronas usando o padrão assíncrono baseado em tarefas. Isso garante que os aplicativos da Windows Store permaneçam respondendo ao chamar uma operação de serviço.  
  
> [!WARNING]
> Embora nenhuma exceção seja gerada se você definir uma operação síncrona, é altamente recomendável definir apenas operações assíncronas.  
  
### <a name="calling-wcf-services-from-windows-store-applications"></a>Chamando serviços WCF de aplicativos da Windows Store  
 Como mencionado acima, todas as configurações devem ser feitas no código no método GetBindingForEndpoint na classe proxy gerada. Chamar uma operação de serviço é feito da mesma maneira que chamar qualquer método assíncrono baseado em tarefas conforme mostrado no seguinte snippet de código.  
  
```csharp  
void async SomeMethod()  
{  
    ServiceClient proxy = new ServiceClient();  
    Task<T> results = await proxy.CallAsync(param1, param2);  
    T result = results.Result;  
    if (result.Success)  
    {  
       // Do something with result  
    }  
}  
```  
  
 Observe o uso da palavra-chave async no método que faz a chamada assíncrona e da palavra-chave await ao chamar o método assíncrono.  
  
## <a name="see-also"></a>Consulte também

- [Blog do WCF no aplicativos da Windows Store](https://blogs.msdn.com/b/piyushjo/archive/2011/09/22/wcf-in-win8-metro-styled-apps-absolutely-supported.aspx)
- [Segurança e clientes da Windows Store do WCF](https://blogs.msdn.com/b/piyushjo/archive/2011/10/11/calling-a-wcf-service-from-a-metro-application-adding-security.aspx)
- [Aplicativos da Windows Store e chamadas entre computadores](https://blogs.msdn.com/b/piyushjo/archive/2011/10/22/calling-a-wcf-service-from-a-metro-application-cross-machine-scenario.aspx)
- [Chamando um serviço WCF implantado no Azure de um aplicativo da Windows Store](https://blogs.msdn.com/b/piyushjo/archive/2011/10/22/calling-a-wcf-service-from-a-metro-application-cross-machine-scenario.aspx)
- [Programação de segurança do WCF](../../../../docs/framework/wcf/feature-details/programming-wcf-security.md)
- [Associações](../../../../docs/framework/wcf/bindings.md)
