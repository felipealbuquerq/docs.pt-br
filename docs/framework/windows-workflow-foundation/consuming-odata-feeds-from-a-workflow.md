---
title: Consumindo feeds do OData de um fluxo de trabalho - WF
ms.date: 03/30/2017
ms.assetid: 1b26617c-53e9-476a-81af-675c36d95919
ms.openlocfilehash: e7d5230bb15474d63b2381d3906e07e48ac0134d
ms.sourcegitcommit: c7a7e1468bf0fa7f7065de951d60dfc8d5ba89f5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65592986"
---
# <a name="consuming-odata-feeds-from-a-workflow"></a>Consumindo feeds do OData de um fluxo de trabalho

WCF Data Services é um componente do .NET Framework que permite que você crie serviços que usam o Open Data Protocol (OData) para expor e consumir dados na Web ou intranet usando a semântica de transferência de estado representacional (REST). OData expõem dados como recursos que são endereçáveis por URIs. Qualquer aplicativo pode interagir com um serviço de dados com base OData- se pode enviar uma solicitação HTTP e processar o avanço de OData que um serviço de dados retorna. Além disso, o WCF Data Services inclui bibliotecas de cliente que fornecem uma experiência mais rica de programação quando você consome feeds OData de aplicativos do .NET Framework. Este tópico fornece uma visão geral de consumir um avanço de OData em um fluxo de trabalho com e sem o uso das bibliotecas de cliente.

## <a name="using-the-sample-northwind-odata-service"></a>Usando o serviço Northwind OData de exemplo

Os exemplos neste tópico usam o exemplo Northwind, serviço de dados localizado em [ http://services.odata.org/Northwind/Northwind.svc/ ](https://go.microsoft.com/fwlink/?LinkID=187426). Esse serviço é fornecido como parte do [OData SDK](https://go.microsoft.com/fwlink/?LinkID=185248) e fornece acesso somente leitura aos dados de exemplo Northwind. Se o acesso de gravação é desejado, ou se um WCF Data Service local for desejado, você pode seguir as etapas da [WCF Services Data Quickstart](https://go.microsoft.com/fwlink/?LinkID=131076) para criar um serviço OData local que fornece acesso ao banco de dados Northwind. Se você segue o quickstart, substitua o URI local para aquele fornecido no exemplo de código neste tópico.

## <a name="consuming-an-odata-feed-using-the-client-libraries"></a>Consumir um OData feed usando as bibliotecas de cliente

WCF Data Services inclui bibliotecas de cliente que permitem que você mais facilmente consumir um feed OData em aplicativos cliente e do .NET Framework. Essas bibliotecas simplificam o envio e o recebimento de mensagens HTTP. Elas também convertem a carga da mensagem em objetos CLR que representam dados de entidade. As bibliotecas de cliente apresentam as duas classes principais <xref:System.Data.Services.Client.DataServiceContext> e <xref:System.Data.Services.Client.DataServiceQuery%601>. Essas classes permitem consultar um serviço de dados e, em seguida, trabalhar com os dados de entidade retornados como objetos CLR. Esta seção aborda duas abordagens para criar as atividades que usam as bibliotecas de cliente.

### <a name="adding-a-service-reference-to-the-wcf-data-service"></a>Adicionando uma referência de serviço para o WCF Data service

Para gerar as bibliotecas de cliente Northwind, você pode usar o **adicionar referência de serviço** caixa de diálogo no Visual Studio 2012 para adicionar uma referência para o serviço Northwind OData.

![Captura de tela que mostra a caixa de diálogo Adicionar referência de serviço.](./media/consuming-odata-feeds-from-a-workflow/add-service-reference-dialog.gif)

Observe que não há nenhuma operação de serviço expostas pelo serviço e, na **Services** lista há itens que representam as entidades expostas pelo serviço de dados Northwind. Quando a referência de serviço é adicionada, as classes serão geradas para essas entidades e podem ser usadas no código do cliente. Os exemplos neste tópico usam essas classes e a classe de `NorthwindEntities` para executar consultas.

> [!NOTE]
> Para obter mais informações, consulte [gerando a biblioteca de cliente do serviço de dados (WCF Data Services)](../data/wcf/generating-the-data-service-client-library-wcf-data-services.md).

### <a name="using-asynchronous-methods"></a>Usar métodos assíncronos

Para corrigir os problemas possíveis de latência que podem ocorrer ao acessar recursos sobre a Web, nós seja recomendável acessar WCF Services Data de forma assíncrona. As bibliotecas de cliente do WCF Data Services incluem métodos assíncronos para chamar consultas, e o Windows Workflow Foundation (WF) fornece o <xref:System.Activities.AsyncCodeActivity> classe para criar atividades assíncronas. <xref:System.Activities.AsyncCodeActivity> derivado atividades podem ser escritas para tirar proveito das classes do .NET Framework que possuem métodos assíncronos, ou o código a ser executado de forma assíncrona pode ser colocado em um método e invocado por meio de um delegado. Esta seção fornece dois exemplos de uma atividade derivada <xref:System.Activities.AsyncCodeActivity> ; um que usa os métodos assíncronos das bibliotecas de cliente de Data WCF Services e um que usa um representante.

> [!NOTE]
> Para obter mais informações, consulte [operações assíncronas (WCF Data Services)](../data/wcf/asynchronous-operations-wcf-data-services.md) e [criando atividades assíncronas](creating-asynchronous-activities-in-wf.md).

### <a name="using-client-library-asynchronous-methods"></a>Usar métodos assíncronos de biblioteca de cliente

A classe de <xref:System.Data.Services.Client.DataServiceQuery%601> fornece <xref:System.Data.Services.Client.DataServiceQuery%601.BeginExecute%2A> e métodos de <xref:System.Data.Services.Client.DataServiceQuery%601.EndExecute%2A> para ver um serviço de OData de forma assíncrona. Esses métodos podem ser chamados de <xref:System.Activities.AsyncCodeActivity.BeginExecute%2A> e as substituições de <xref:System.Activities.AsyncCodeActivity.EndExecute%2A> de uma classe derivada de <xref:System.Activities.AsyncCodeActivity> . Quando o <xref:System.Activities.AsyncCodeActivity> <xref:System.Activities.AsyncCodeActivity.BeginExecute%2A> substituição retorna, o fluxo de trabalho pode ir ocioso (mas não para persistir), e quando o trabalho assíncrono é concluído, <xref:System.Activities.AsyncCodeActivity.EndExecute%2A> é invocado pelo tempo de execução.

No exemplo a seguir, uma atividade de `OrdersByCustomer` é definida que possui dois argumentos conectados. O argumento de `CustomerId` representa o cliente que identifica qual as regras para retornar, e o argumento de `ServiceUri` representa o URI de serviço de OData a ser consultado. Porque a atividade deriva de `AsyncCodeActivity<IEnumerable<Order>>` há também um argumento de saída de <xref:System.Activities.Activity%601.Result%2A> que é usado para retornar os resultados da consulta. A substituição de <xref:System.Activities.AsyncCodeActivity.BeginExecute%2A> cria uma consulta LINQ que selecionar todos os pedidos do cliente especificado. Esta consulta é especificado como <xref:System.Activities.AsyncCodeActivityContext.UserState%2A> de <xref:System.Activities.AsyncCodeActivityContext>passado, e o método de <xref:System.Data.Services.Client.DataServiceQuery%601.BeginExecute%2A> de consulta é então chamado. Observe que o retorno de chamada e indica que é passado em <xref:System.Data.Services.Client.DataServiceQuery%601.BeginExecute%2A> de consulta são aqueles que são passados ao método de <xref:System.Activities.AsyncCodeActivity.BeginExecute%2A> de atividade. Quando a consulta terminou de executar, o método de <xref:System.Activities.AsyncCodeActivity.EndExecute%2A> da atividade é chamado. A consulta é recuperada de <xref:System.Activities.AsyncCodeActivityContext.UserState%2A>, e o método de <xref:System.Data.Services.Client.DataServiceQuery%601.EndExecute%2A> de consulta é então chamado. Esse método retorna <xref:System.Collections.Generic.IEnumerable%601> do tipo de entidade especificado; nesse caso `Order`. Uma vez que `IEnumerable<Order>` é o tipo genérico do <xref:System.Activities.AsyncCodeActivity%601>, esse <xref:System.Collections.IEnumerable> é definido como o <xref:System.Activities.Activity%601.Result%2A> <xref:System.Activities.OutArgument%601> da atividade.

[!code-csharp[CFX_WCFDataServicesActivityExample#100](~/samples/snippets/csharp/VS_Snippets_CFX/CFX_WCFDataServicesActivityExample/cs/Program.cs#100)]

No exemplo a seguir, a atividade de `OrdersByCustomer` recuperar uma lista de pedidos para o cliente especificado, e então uma atividade de <xref:System.Activities.Statements.ForEach%601> enumera os pedidos retornados e grava a data de cada pedido no console.

[!code-csharp[CFX_WCFDataServicesActivityExample#10](~/samples/snippets/csharp/VS_Snippets_CFX/CFX_WCFDataServicesActivityExample/cs/Program.cs#10)]

Quando esse fluxo de trabalho é chamado, os seguintes dados são gravados no console:

```console
Calling WCF Data Service...
8/25/1997
10/3/1997
10/13/1997
1/15/1998
3/16/1998
4/9/1998
```

> [!NOTE]
> Se uma conexão com o servidor de OData não pode ser estabelecida, você receberá uma exceção similar à seguinte exceção:
>
> Exceção sem tratamento: System.InvalidOperationException: Ocorreu um erro ao processar esta solicitação. ---> System.NET. WebException: Não é possível conectar ao servidor remoto---> System.Net.Sockets.SocketException: Uma tentativa de conexão falhou porque a parte conectada não respondeu corretamente após um período de tempo ou a conexão estabelecida falhou porque o host conectado não respondeu.

Se qualquer processamento adicional dos dados retornados pela consulta é necessário, pode ser feito em uma substituição de <xref:System.Activities.AsyncCodeActivity%601.EndExecute%2A> de atividade. O <xref:System.Activities.AsyncCodeActivity%601.BeginExecute%2A> e o <xref:System.Activities.AsyncCodeActivity%601.EndExecute%2A> são chamados usando o segmento de fluxo de trabalho, e qualquer código nessas alternativas não funciona de forma assíncrona. Se o processamento adicional é abrangente ou longo, ou os resultados da consulta são paginados, você deve considerar a abordagem discutido na seção a seguir, que usa um representante para executar a consulta e executar processamento adicional de forma assíncrona.

### <a name="using-a-delegate"></a>Usando um delegado

Além de chamar o método assíncrono de uma classe do .NET Framework, um <xref:System.Activities.AsyncCodeActivity>-atividade com base também pode definir a lógica assíncrona em um de seus métodos. Este método é especificado usando um delegado em uma substituição de <xref:System.Activities.AsyncCodeActivity.BeginExecute%2A> de atividade. Quando o método retorna, o tempo de execução chama a substituição de <xref:System.Activities.AsyncCodeActivity.EndExecute%2A> de atividade. Para chamar um serviço de OData de um fluxo de trabalho, esse método pode ser usado para ver o serviço e fornecer qualquer processamento adicional.

No exemplo a seguir, uma atividade de `ListCustomers` é definida. Esta atividade consulta o serviço de dados de exemplo Northwind e retorna `List<Customer>` que contém todos os clientes na base de dados Northwind. Assíncrono o trabalho é executado pelo método de `GetCustomers` . Este método consulta o serviço para todos os clientes, e copiar em `List<Customer>`. Então verifica para ver se os resultados são paginados. Em caso afirmativo, consulte o serviço para a próxima página de resultados, adicioná-los à lista, e continua-o até que todos os dados do cliente sejam recuperados.

> [!NOTE]
> Para obter mais informações sobre a paginação no WCF Data Services, consulte [como: Carregar resultados (WCF Data Services) paginados](../data/wcf/how-to-load-paged-results-wcf-data-services.md).

Depois que todos os clientes são adicionados, a lista será retornada. O método de `GetCustomers` é especificado em uma substituição de <xref:System.Activities.AsyncCodeActivity.BeginExecute%2A> de atividade. Desde que o método possui um valor de retorno, `Func<string, List<Customer>>` é criado para especificar o método.

> [!NOTE]
> Se o método que executa o trabalho assíncrona não tem um valor de retorno, <xref:System.Action> é usado em vez de <xref:System.Func%601>. Para obter exemplos de criação de um exemplo assíncrona usando as duas abordagens, consulte [criando atividades assíncronas](creating-asynchronous-activities-in-wf.md).

Este <xref:System.Func%601> é atribuído a <xref:System.Activities.AsyncCodeActivityContext.UserState%2A>, e `BeginInvoke` é então chamado. Desde que o método a ser chamado não tem acesso ao ambiente de atividade de argumentos, o valor do argumento de `ServiceUri` é passado como o primeiro parâmetro, juntamente com o retorno de chamada e indica que foram passados em <xref:System.Activities.AsyncCodeActivity.BeginExecute%2A>. Quando `GetCustomers` retorna, o tempo de execução chama <xref:System.Activities.AsyncCodeActivity.EndExecute%2A>. O código em <xref:System.Activities.AsyncCodeActivity.EndExecute%2A> recupera o representante de <xref:System.Activities.AsyncCodeActivityContext.UserState%2A>, chama `EndInvoke`, e retorna o resultado, que é a lista de clientes retornados pelo método de `GetCustomers` .

[!code-csharp[CFX_WCFDataServicesActivityExample#200](~/samples/snippets/csharp/VS_Snippets_CFX/CFX_WCFDataServicesActivityExample/cs/Program.cs#200)]

No exemplo a seguir, a atividade de `ListCustomers` recuperar uma lista de clientes, e então uma atividade de <xref:System.Activities.Statements.ForEach%601> enumera-os e escreve-ao nome do nome da empresa e de contatos de cada cliente no console.

[!code-csharp[CFX_WCFDataServicesActivityExample#20](~/samples/snippets/csharp/VS_Snippets_CFX/CFX_WCFDataServicesActivityExample/cs/Program.cs#20)]

Quando esse fluxo de trabalho é chamado, os seguintes dados são gravados no console. Como esta consulta retorna vários clientes, somente a parte de saída é mostrada aqui.

```console
Calling WCF Data Service...
Alfreds Futterkiste, Contact: Maria Anders
Ana Trujillo Emparedados y helados, Contact: Ana Trujillo
Antonio Moreno Taquería, Contact: Antonio Moreno
Around the Horn, Contact: Thomas Hardy
Berglunds snabbköp, Contact: Christina Berglund
...
```

## <a name="consuming-an-odata-feed-without-using-the-client-libraries"></a>Consumir um OData feed sem usar as bibliotecas de cliente

OData expõem dados como recursos que são endereçáveis por URIs. Quando você usa bibliotecas de cliente essas URIs são criados para você, mas você não tem que usar as bibliotecas de cliente. Se desejado, serviços de OData podem ser acessados diretamente sem usar as bibliotecas de cliente. Para não usar as bibliotecas de cliente o local do serviço e os dados desejados são especificados por um URI e os resultados são retornados em resposta a solicitação HTTP. Esses dados brutos podem ser processados ou manipulado na forma desejada. Uma maneira para recuperar os resultados de uma consulta de OData é usando a classe de <xref:System.Net.WebClient> . Nesse exemplo, o nome de contato para o cliente representado pela chave ALFKI é recuperado.

[!code-csharp[CFX_WCFDataServicesActivityExample#2](~/samples/snippets/csharp/VS_Snippets_CFX/CFX_WCFDataServicesActivityExample/cs/Program.cs#2)]

Quando esse código é executado, a saída a seguir são exibidas no console:

```console
Raw data returned:
<?xml version="1.0" encoding="utf-8" standalone="yes"?>
<ContactName xmlns="http://schemas.microsoft.com/ado/2007/08/dataservices">Maria Anders</ContactName>
```

Em um fluxo de trabalho, o código deste exemplo pode ser inserido na substituição de <xref:System.Activities.CodeActivity.Execute%2A> de uma atividade personalizado com base em <xref:System.Activities.CodeActivity>, mas a mesma funcionalidade também pode ser realizada usando a atividade de <xref:System.Activities.Expressions.InvokeMethod%601> . A atividade de <xref:System.Activities.Expressions.InvokeMethod%601> permite que autores de fluxo de trabalho para chamar métodos estáticos e de instância de uma classe, e também tem uma opção chamar o método de forma assíncrona especificado. No exemplo a seguir, uma atividade de <xref:System.Activities.Expressions.InvokeMethod%601> é configurada para chamar o método de <xref:System.Net.WebClient.DownloadString%2A> da classe de <xref:System.Net.WebClient> e retornar uma lista de clientes.

[!code-csharp[CFX_WCFDataServicesActivityExample#3](~/samples/snippets/csharp/VS_Snippets_CFX/CFX_WCFDataServicesActivityExample/cs/Program.cs#3)]

<xref:System.Activities.Expressions.InvokeMethod%601> pode chamar métodos estáticos e de instância de uma classe. Desde que <xref:System.Net.WebClient.DownloadString%2A> é um método de instância da classe de <xref:System.Net.WebClient> , uma nova instância da classe <xref:System.Net.WebClient> é especificada para <xref:System.Activities.Expressions.InvokeMethod%601.TargetObject%2A>. `DownloadString` é especificado como <xref:System.Activities.Expressions.InvokeMethod%601.MethodName%2A>, o URI que contém a consulta é especificado na coleção de <xref:System.Activities.Expressions.InvokeMethod%601.Parameters%2A> , e o valor de retorno é atribuído ao valor de <xref:System.Activities.Activity%601.Result%2A> . O valor de <xref:System.Activities.Expressions.InvokeMethod%601.RunAsynchronously%2A> é definido como `true`, o que significa que a chamada ao método executará de forma assíncrona com relação ao fluxo de trabalho. No exemplo a seguir, um fluxo de trabalho é construído que usa a atividade de <xref:System.Activities.Expressions.InvokeMethod%601> para ver o serviço de dados de exemplo Northwind para uma lista de pedidos para um cliente específico, e os dados retornados são gravados no console.

[!code-csharp[CFX_WCFDataServicesActivityExample#1](~/samples/snippets/csharp/VS_Snippets_CFX/CFX_WCFDataServicesActivityExample/cs/Program.cs#1)]

Quando esse fluxo de trabalho é chamado, a seguinte saída é exibida no console. Como esta consulta retorna vários pedidos, somente a parte de saída é mostrada aqui.

```console
Calling WCF Data Service...
Raw data returned:

<?xml version="1.0" encoding="utf-8" standalone="yes"?>*
<feed
xml:base="http://services.odata.org/Northwind/Northwind.svc/"
xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices"
xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata"
xmlns="http://www.w3.org/2005/Atom">
<title type="text">Orders\</title>
<id>http://services.odata.org/Northwind/Northwind.svc/Customers('ALFKI')/Orders\</id>
<updated>2010-05-19T19:37:07Z\</updated>
<link rel="self" title="Orders" href="Orders" />
<entry>
<id>http://services.odata.org/Northwind/Northwind.svc/Orders(10643)\</id>
<title type="text">\</title>
<updated>2010-05-19T19:37:07Z\</updated>
<author>
<name />
</author>
<link rel="edit" title="Order" href="Orders(10643)" />
<link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/Customer" type="application/atom+xml;type=entry" title="Customer" href="Orders(10643)/Customer" />
...
```

Este exemplo fornece um método que os autores de aplicativo de fluxo de trabalho podem usar para receber os dados brutos retornados de um serviço de OData. Para obter mais informações sobre como acessar WCF Services Data usando URIs, consulte [acessando recursos do serviço (WCF Data Services)](../data/wcf/accessing-data-service-resources-wcf-data-services.md) e [OData: Convenções de URI](https://go.microsoft.com/fwlink/?LinkId=185564).
