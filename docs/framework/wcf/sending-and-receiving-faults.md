---
title: Enviando e recebendo falhas
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- handling faults [WCF], sending
ms.assetid: 7be6fb96-ce2a-450b-aebe-f932c6a4bc5d
ms.openlocfilehash: 447928ec3e6c0eaab997f839ab1aab49c677ad44
ms.sourcegitcommit: 581ab03291e91983459e56e40ea8d97b5189227e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2019
ms.locfileid: "70044462"
---
# <a name="sending-and-receiving-faults"></a>Enviando e recebendo falhas

Falhas de SOAP transmitem informações de condição de erro de um serviço para um cliente e no caso duplex de um cliente para um serviço de forma interoperável. Normalmente, um serviço define o conteúdo de falha personalizado e especifica quais operações podem retorná-los. (Para obter mais informações, consulte [definindo e especificando falhas](../../../docs/framework/wcf/defining-and-specifying-faults.md).) Este tópico discute como um serviço ou cliente duplex pode enviar essas falhas quando a condição de erro correspondente tiver ocorrido e como um aplicativo cliente ou de serviço lida com essas falhas. Para obter uma visão geral do tratamento de erros em aplicativos Windows Communication Foundation (WCF), consulte [especificando e manipulando falhas em contratos e serviços](../../../docs/framework/wcf/specifying-and-handling-faults-in-contracts-and-services.md).

## <a name="sending-soap-faults"></a>Enviando falhas de SOAP

As falhas de SOAP declaradas são aquelas em que uma <xref:System.ServiceModel.FaultContractAttribute?displayProperty=nameWithType> operação tem um que especifica um tipo de falha SOAP personalizado. As falhas de SOAP não declaradas são aquelas que não são especificadas no contrato para uma operação.

### <a name="sending-declared-faults"></a>Enviando falhas declaradas

Para enviar uma falha de SOAP declarada, detecte a condição de erro para a qual a falha <xref:System.ServiceModel.FaultException%601?displayProperty=nameWithType> SOAP é apropriada e lance um novo onde o parâmetro de tipo é um <xref:System.ServiceModel.FaultContractAttribute> novo objeto do tipo especificado no para essa operação. O exemplo de código a seguir mostra o <xref:System.ServiceModel.FaultContractAttribute> uso de para especificar `SampleMethod` que a operação pode retornar uma falha de SOAP com o `GreetingFault`tipo de detalhe de.

[!code-csharp[FaultContractAttribute#4](../../../samples/snippets/csharp/VS_Snippets_CFX/faultcontractattribute/cs/services.cs#4)]
[!code-vb[FaultContractAttribute#4](../../../samples/snippets/visualbasic/VS_Snippets_CFX/faultcontractattribute/vb/services.vb#4)]

Para transmitir as `GreetingFault` informações de erro para o cliente, pegue a condição de erro apropriada e acione <xref:System.ServiceModel.FaultException%601?displayProperty=nameWithType> um novo `GreetingFault` tipo com um `GreetingFault` novo objeto como o argumento, como no exemplo de código a seguir. Se o cliente for um aplicativo cliente do WCF, ele o experimentará como uma exceção gerenciada em <xref:System.ServiceModel.FaultException%601?displayProperty=nameWithType> que o `GreetingFault`tipo é do tipo.

[!code-csharp[FaultContractAttribute#5](../../../samples/snippets/csharp/VS_Snippets_CFX/faultcontractattribute/cs/services.cs#5)]
[!code-vb[FaultContractAttribute#5](../../../samples/snippets/visualbasic/VS_Snippets_CFX/faultcontractattribute/vb/services.vb#5)]

### <a name="sending-undeclared-faults"></a>Enviando falhas não declaradas

O envio de falhas não declaradas pode ser muito útil para diagnosticar e depurar problemas rapidamente em aplicativos WCF, mas sua utilidade como ferramenta de depuração é limitada. Em geral, ao depurar, é recomendável que você use <xref:System.ServiceModel.Description.ServiceDebugBehavior.IncludeExceptionDetailInFaults%2A?displayProperty=nameWithType> a propriedade. Quando você define esse valor como true, os clientes experimentam tais falhas <xref:System.ServiceModel.FaultException%601> como exceções do <xref:System.ServiceModel.ExceptionDetail>tipo.

> [!IMPORTANT]
> Como as exceções gerenciadas podem expor informações internas do <xref:System.ServiceModel.ServiceBehaviorAttribute.IncludeExceptionDetailInFaults%2A?displayProperty=nameWithType> aplicativo <xref:System.ServiceModel.Description.ServiceDebugBehavior.IncludeExceptionDetailInFaults%2A?displayProperty=nameWithType> , `true` a configuração ou para permitir que os clientes do WCF obtenham informações sobre exceções de operação de serviço interno, incluindo pessoal identificável ou outras informações confidenciais.
>
> Portanto, a <xref:System.ServiceModel.ServiceBehaviorAttribute.IncludeExceptionDetailInFaults%2A?displayProperty=nameWithType> configuração <xref:System.ServiceModel.Description.ServiceDebugBehavior.IncludeExceptionDetailInFaults%2A?displayProperty=nameWithType> ou `true` para é recomendada apenas como uma maneira de depurar temporariamente um aplicativo de serviço. Além disso, o WSDL para um método que retorna exceções gerenciadas sem tratamento dessa maneira não contém o contrato para o <xref:System.ServiceModel.FaultException%601> do tipo. <xref:System.ServiceModel.ExceptionDetail> Os clientes devem esperar a possibilidade de uma falha SOAP desconhecida (retornada aos clientes WCF <xref:System.ServiceModel.FaultException?displayProperty=nameWithType> como objetos) para obter as informações de depuração corretamente.

Para enviar uma falha SOAP não declarada, <xref:System.ServiceModel.FaultException?displayProperty=nameWithType> lance um objeto (ou seja, não o <xref:System.ServiceModel.FaultException%601>tipo genérico) e passe a cadeia de caracteres para o construtor. Isso é exposto aos aplicativos cliente do WCF como uma exceção <xref:System.ServiceModel.FaultException?displayProperty=nameWithType> gerada em que a cadeia de caracteres está disponível <xref:System.ServiceModel.FaultException%601.ToString%2A?displayProperty=nameWithType> chamando o método.

> [!NOTE]
> Se você declarar uma falha SOAP do tipo cadeia de caracteres e, em seguida, lançar isso em <xref:System.ServiceModel.FaultException%601> seu serviço como um local em <xref:System.String?displayProperty=nameWithType> que o parâmetro de tipo é um <xref:System.ServiceModel.FaultException%601.Detail%2A?displayProperty=nameWithType> valor de cadeia de caracteres é atribuído <xref:System.ServiceModel.FaultException%601.ToString%2A?displayProperty=nameWithType>à propriedade e não está disponível no.

## <a name="handling-faults"></a>Tratamento de falhas

Em clientes WCF, as falhas de SOAP que ocorrem durante a comunicação que são de interesse para aplicativos cliente são geradas como exceções gerenciadas. Embora existam muitas exceções que podem ocorrer durante a execução de qualquer programa, os aplicativos que usam o modelo de programação de cliente do WCF podem esperar manipular exceções dos dois tipos a seguir como resultado da comunicação.

- <xref:System.TimeoutException>

- <xref:System.ServiceModel.CommunicationException>

<xref:System.TimeoutException>os objetos são emitidos quando uma operação excede o período de tempo limite especificado.

<xref:System.ServiceModel.CommunicationException>os objetos são lançados quando há alguma condição de erro de comunicação recuperável no serviço ou no cliente.

A <xref:System.ServiceModel.CommunicationException> classe tem dois <xref:System.ServiceModel.FaultException> tipos derivados importantes e o tipo genérico <xref:System.ServiceModel.FaultException%601> .

<xref:System.ServiceModel.FaultException>as exceções são geradas quando um ouvinte recebe uma falha que não é esperada ou especificada no contrato de operação; Geralmente isso ocorre quando o aplicativo está sendo depurado e o serviço tem a <xref:System.ServiceModel.Description.ServiceDebugBehavior.IncludeExceptionDetailInFaults%2A?displayProperty=nameWithType> propriedade definida como `true`.

<xref:System.ServiceModel.FaultException%601>as exceções são geradas no cliente quando uma falha especificada no contrato de operação é recebida em resposta a uma operação bidirecional (ou seja, um método com um <xref:System.ServiceModel.OperationContractAttribute> atributo com <xref:System.ServiceModel.OperationContractAttribute.IsOneWay%2A> definido como `false`).

> [!NOTE]
> Quando um serviço WCF tem a <xref:System.ServiceModel.ServiceBehaviorAttribute.IncludeExceptionDetailInFaults%2A?displayProperty=nameWithType> propriedade <xref:System.ServiceModel.Description.ServiceDebugBehavior.IncludeExceptionDetailInFaults%2A?displayProperty=nameWithType> ou definida `true` como o cliente experimenta isso como um tipo <xref:System.ServiceModel.ExceptionDetail>não <xref:System.ServiceModel.FaultException%601> declarado. Os clientes podem capturar essa falha específica ou lidar com a falha em um bloco Catch <xref:System.ServiceModel.FaultException>para o.

Normalmente, apenas <xref:System.ServiceModel.FaultException%601> <xref:System.TimeoutException>as exceçõessãodeinteresseparaclienteseserviços.<xref:System.ServiceModel.CommunicationException>

> [!NOTE]
> Outras exceções, é claro, ocorrem. Exceções inesperadas incluem falhas <xref:System.OutOfMemoryException?displayProperty=nameWithType>catastróficas como; normalmente, os aplicativos não devem capturar esses métodos.

### <a name="catch-fault-exceptions-in-the-correct-order"></a>Capturar exceções de falha na ordem correta

Como <xref:System.ServiceModel.FaultException%601> deriva de <xref:System.ServiceModel.FaultException>, e <xref:System.ServiceModel.FaultException> deriva de <xref:System.ServiceModel.CommunicationException>, é importante capturar essas exceções na ordem correta. Se, por exemplo, você tiver um bloco try/catch no qual você captura <xref:System.ServiceModel.CommunicationException>primeiro, todas as falhas de SOAP especificadas e não especificadas serão tratadas lá; quaisquer blocos catch subsequentes para manipular uma exceção personalizada <xref:System.ServiceModel.FaultException%601> nunca serão invocados.

Lembre-se de que uma operação pode retornar qualquer número de falhas especificadas. Cada falha é um tipo exclusivo e deve ser manipulada separadamente.

### <a name="handle-exceptions-when-closing-the-channel"></a>Tratar exceções ao fechar o canal

A maior parte da discussão anterior tem a ver com falhas enviadas no decorrer do processamento de mensagens de aplicativo, ou seja, mensagens explicitamente enviadas pelo cliente quando o aplicativo cliente chama operações no objeto cliente do WCF.

Mesmo com objetos locais descartando o objeto, é possível aumentar ou mascarar as exceções que ocorrem durante o processo de reciclagem. Algo semelhante pode ocorrer quando você usa objetos de cliente WCF. Ao chamar operações, você está enviando mensagens por uma conexão estabelecida. Fechar o canal pode gerar exceções se a conexão não puder ser fechada corretamente ou já estiver fechada, mesmo que todas as operações retornassem adequadamente.

Normalmente, os canais de objeto de cliente são fechados de uma das seguintes maneiras:

- Quando o objeto do cliente WCF é reciclado.

- Quando o aplicativo cliente chama <xref:System.ServiceModel.ClientBase%601.Close%2A?displayProperty=nameWithType>.

- Quando o aplicativo cliente chama <xref:System.ServiceModel.ICommunicationObject.Close%2A?displayProperty=nameWithType>.

- Quando o aplicativo cliente chama uma operação que é uma operação de encerramento para uma sessão.

Em todos os casos, o fechamento do canal instrui o canal a começar a fechar todos os canais subjacentes que podem estar enviando mensagens para oferecer suporte à funcionalidade complexa no nível do aplicativo. Por exemplo, quando um contrato requer sessões, uma associação tenta estabelecer uma sessão trocando mensagens com o canal de serviço até que uma sessão seja estabelecida. Quando o canal é fechado, o canal de sessão subjacente notifica o serviço de que a sessão foi encerrada. Nesse caso, se o canal já tiver sido anulado, fechado ou, de outra forma, inutilizável (por exemplo, quando um cabo de rede estiver desconectado), o canal do cliente não poderá informar ao canal de serviço que a sessão foi encerrada e uma exceção poderá resultar.

### <a name="abort-the-channel-if-necessary"></a>Anular o canal, se necessário

Como o fechamento do canal também pode gerar exceções, é recomendável que, além de capturar as exceções de falha na ordem correta, seja importante anular o canal que foi usado para fazer a chamada no bloco catch.

Se a falha transmitir informações de erro específicas a uma operação e se ainda for possível que outras pessoas possam usá-las, não haverá necessidade de anular o canal (embora esses casos sejam raros). Em todos os outros casos, é recomendável anular o canal. Para obter um exemplo que demonstra todos esses pontos, consulte [exceções esperadas](../../../docs/framework/wcf/samples/expected-exceptions.md).

O exemplo de código a seguir mostra como lidar com exceções de falha de SOAP em um aplicativo cliente básico, incluindo uma falha declarada e uma falha não declarada.

> [!NOTE]
> Este código de exemplo não usa a `using` construção. Como os canais de fechamento podem gerar exceções, é recomendável que os aplicativos criem primeiro um cliente WCF e, em seguida, abram, utilizem e fechem o cliente WCF no mesmo bloco try. Para obter detalhes, consulte [visão geral do cliente WCF](../../../docs/framework/wcf/wcf-client-overview.md) e [use fechar e anular para liberar os recursos do cliente WCF](../../../docs/framework/wcf/samples/use-close-abort-release-wcf-client-resources.md).

[!code-csharp[FaultContractAttribute#3](../../../samples/snippets/csharp/VS_Snippets_CFX/faultcontractattribute/cs/client.cs#3)]
[!code-vb[FaultContractAttribute#3](../../../samples/snippets/visualbasic/VS_Snippets_CFX/faultcontractattribute/vb/client.vb#3)]

## <a name="see-also"></a>Consulte também

- <xref:System.ServiceModel.FaultException>
- <xref:System.ServiceModel.FaultException%601>
- <xref:System.ServiceModel.CommunicationException?displayProperty=nameWithType>
- [Exceções esperadas](../../../docs/framework/wcf/samples/expected-exceptions.md)
- [Usar fechar e anular para liberar os recursos do cliente do WCF](../../../docs/framework/wcf/samples/use-close-abort-release-wcf-client-resources.md)
