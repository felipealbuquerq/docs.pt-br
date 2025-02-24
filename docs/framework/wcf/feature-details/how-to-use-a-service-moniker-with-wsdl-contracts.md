---
title: 'Como: usar um moniker de serviço com contratos WSDL'
ms.date: 03/30/2017
ms.assetid: a88d9650-bb50-4f48-8c85-12f5ce98a83a
ms.openlocfilehash: 80b0d034b92123862d0500106f81d4a566cac467
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69968775"
---
# <a name="how-to-use-a-service-moniker-with-wsdl-contracts"></a>Como: usar um moniker de serviço com contratos WSDL
Há situações em que você talvez queira ter um cliente de interoperabilidade COM totalmente independente. O serviço que você deseja chamar pode não expor um ponto de extremidade MEX e a DLL do cliente do WCF pode não estar registrada para interoperabilidade COM. Nesses casos, você pode criar um arquivo WSDL que descreve o serviço e passá-lo para o moniker do serviço WCF. Este tópico descreve como chamar o Introdução exemplo do WCF usando um moniker WSDL do WCF.  
  
### <a name="using-the-wsdl-service-moniker"></a>Usando o moniker do serviço WSDL  
  
1. Abra e crie a solução de exemplo GettingStarted.  
  
2. Abra o Internet Explorer e navegue `http://localhost/ServiceModelSamples/Service.svc` até para certificar-se de que o serviço está funcionando.  
  
3. No arquivo Service.cs, adicione o seguinte atributo na classe CalculatorService:  
  
     [!code-csharp[S_WSDL_Client#0](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_wsdl_client/cs/service.cs#0)]  
  
4. Adicione um namespace de associação ao aplicativo de serviço. config:  

5. Crie um arquivo WSDL para que o aplicativo Leia. Como os namespaces foram adicionados nas etapas 3 e 4, você pode usar o IE para consultar toda a descrição WSDL do serviço navegando até `http://localhost/ServiceModelSamples/Service.svc?wsdl`. Em seguida, você pode salvar o arquivo do Internet Explorer como arquivo. xml. Se você não especificar os namespaces nas etapas 3 e 4, o documento WSDL retornado da consulta da URL acima não será o WSDL completo. O documento WSDL retornado incluirá várias instruções de importação que importam outros documentos WSDL. Você precisará percorrer cada instrução de importação e criar o documento WSDL completo, combinando o WSDL retornado do serviço com o WSDL importado.  
  
6. Abra Visual Basic 6,0 e crie um novo arquivo Standard. exe. Adicione um botão ao formulário e clique duas vezes no botão para adicionar o seguinte código ao manipulador de cliques:  
  
    ```  
    ' Open the WSDL contract file and read it all into the wsdlContract string.  
    Const ForReading = 1  
    Set objFSO = CreateObject("Scripting.FileSystemObject")  
    Set objFile = objFSO.OpenTextFile("c:\serviceWsdl.xml", ForReading)  
    wsdlContract = objFile.ReadAll  
    objFile.Close  
  
    ' Create a string for the service moniker including the content of the WSDL contract file.  
    wsdlMonikerString = "service4:address='http://localhost/ServiceModelSamples/service.svc'"  
    wsdlMonikerString = wsdlMonikerString + ", wsdl='" & wsdlContract & "'"  
    wsdlMonikerString = wsdlMonikerString + ", binding=WSHttpBinding_ICalculator, bindingNamespace='http://Microsoft.ServiceModel.Samples'"  
    wsdlMonikerString = wsdlMonikerString + ", contract=ICalculator, contractNamespace='http://Microsoft.ServiceModel.Samples'"  
  
    ' Create the service moniker object.  
    Set wsdlServiceMoniker = GetObject(wsdlMonikerString)  
  
    ' Call the service operations using the moniker object.  
    MsgBox "WSDL service moniker: 145 - 76.54 = " & wsdlServiceMoniker.Subtract(145, 76.54)  
    ```  
  
    > [!NOTE]
    > Se o moniker estiver malformado ou se o serviço não estiver disponível, `GetObject` a chamada para retornará um erro dizendo "sintaxe inválida".  Se você receber esse erro, verifique se o moniker que você está usando está correto e se o serviço está disponível.  
  
7. Execute o aplicativo Visual Basic. Uma caixa de mensagem será exibida com os resultados da chamada de subtração (145, 76,54).  
  
## <a name="see-also"></a>Consulte também

- [Introdução](../../../../docs/framework/wcf/samples/getting-started-sample.md)
- [Visão geral da integração de aplicativos COM](../../../../docs/framework/wcf/feature-details/integrating-with-com-applications-overview.md)
