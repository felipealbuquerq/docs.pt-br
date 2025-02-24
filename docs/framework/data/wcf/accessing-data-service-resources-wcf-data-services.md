---
title: Acessando recursos do serviço de dados (WCF Data Services)
ms.date: 03/30/2017
helpviewer_keywords:
- WCF Data Services, querying
- getting started, WCF Data Services
- querying the data service [WCF Data Service]
- WCF Data Services, getting started
- WCF Data Services, accessing data
ms.assetid: 9665ff5b-3e3a-495d-bf83-d531d5d060ed
ms.openlocfilehash: eff8d682004bf437a9b5470a4eb91c9bd52bfad5
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/07/2019
ms.locfileid: "70791328"
---
# <a name="accessing-data-service-resources-wcf-data-services"></a>Acessando recursos do serviço de dados (WCF Data Services)
[!INCLUDE[ssAstoria](../../../../includes/ssastoria-md.md)]dá suporte ao para expor seus dados como um feed com recursos que podem ser endereçáveis por URIs. [!INCLUDE[ssODataFull](../../../../includes/ssodatafull-md.md)] Esses recursos são representados de acordo com as convenções de relacionamento de entidade do [modelo de dados de entidade](../adonet/entity-data-model.md). Nesse modelo, as entidades representam unidades operacionais de dados que são tipos de dados em um domínio de aplicativo, como clientes, pedidos, itens e classes. Os dados de entidades são acessados e alterados usando-se a semântica REST (transferência de estado representativo), especificamente os verbos HTTP padrão GET, PUT, POST e DELETE.  
  
## <a name="addressing-resources"></a>Direcionando recursos  
 No [!INCLUDE[ssODataShort](../../../../includes/ssodatashort-md.md)], você direciona quaisquer dados expostos pelo modelo de dados usando um URI. Por exemplo, o URI a seguir retorna um feed que é o conjunto de entidades Customers, que contém entradas para todas as instâncias do tipo de entidade Customer:  
  
```  
http://services.odata.org/Northwind/Northwind.svc/Customers  
```  
  
 As entidades têm propriedades especiais chamadas chaves de entidade. Uma chave de entidade é usada para identificar exclusivamente uma única entidade em um conjunto de entidades. Isso permite que você direcione uma instância específica de um tipo de entidade no conjunto de entidades. Por exemplo, o URI a seguir retorna uma entrada para uma instância específica de tipo de entidade Customer que tem um valor de chave `ALFKI`:  
  
```  
http://services.odata.org/Northwind/Northwind.svc/Customers('ALFKI')  
```  
  
 As propriedades primitivas e complexas de uma instância de entidade também podem ser direcionadas individualmente. Por exemplo, o seguinte URI retorna um elemento XML que contém o valor da propriedade `ContactName` para um cliente específico:  
  
```  
http://services.odata.org/Northwind/Northwind.svc/Customers('ALFKI')/ContactName  
```  
  
 Quando você inclui o ponto de extremidade `$value` no URI anterior, somente o valor da propriedade primitiva é retornado na mensagem de resposta. O exemplo a seguir retorna apenas a cadeia de caracteres “Maria Anders” sem o elemento XML:  
  
```  
http://services.odata.org/Northwind/Northwind.svc/Customers('ALFKI')/ContactName/$value  
```  
  
 As relações entre as entidades são definidas no modelo de dados por associações. Essas associações permitem que você direcione entidades relacionadas usando propriedades de navegação de uma instância de entidade. Uma propriedade de navegação pode retornar uma única entidade relacionada, no caso de uma relação muitos-para-um, ou um conjunto de entidades relacionadas, no caso de uma relação um-para-muitos. Por exemplo, o seguinte URI retorna um feed que é o conjunto de todos os pedidos relacionados a um cliente específico:  
  
```  
http://services.odata.org/Northwind/Northwind.svc/Customers('ALFKI')/Orders  
```  
  
 As relações, que são geralmente bidirecionais, são representadas por um par de propriedades de navegação. Como o inverso da relação mostrada no exemplo anterior, o seguinte URI retorna uma referência à entidade Customer à qual uma entidade Order específica pertence:  
  
```  
http://services.odata.org/Northwind/Northwind.svc/Orders(10643)/Customer  
```  
  
 [!INCLUDE[ssODataShort](../../../../includes/ssodatashort-md.md)]também permite que você resolva recursos com base nos resultados de expressões de consulta. Isso possibilita filtrar conjuntos de recursos com base em uma expressão avaliada. Por exemplo, o seguinte URI filtra os recursos para retornar somente os pedidos para o cliente especificado que foram remetidos desde 22 de setembro de 1997:  
  
```  
http://services.odata.org/Northwind/Northwind.svc/Customers('ALFKI')/Orders?$filter=ShippedDate gt datetime'1997-09-22T00:00:00'  
```  
  
 Para obter mais informações, [consulte OData: Convenções](https://go.microsoft.com/fwlink/?LinkId=185564)de URI.  
  
## <a name="system-query-options"></a>Opções de consulta de sistema  
 [!INCLUDE[ssODataShort](../../../../includes/ssodatashort-md.md)]define um conjunto de opções de consulta do sistema que você pode usar para executar operações de consulta tradicionais em recursos, como filtragem, classificação e paginação. Por exemplo, o URI a seguir retorna o conjunto de todas `Order` as entidades, juntamente com `Order_Detail` as entidades relacionadas, os CEPs que não terminam em `100`:  
  
```  
http://services.odata.org/Northwind/Northwind.svc/Orders?$filter=not endswith(ShipPostalCode,'100')&$expand=Order_Details&$orderby=ShipCity  
```  
  
 As entradas no feed retornado também são ordenadas pelo valor da propriedade ShipCity dos pedidos.  
  
 [!INCLUDE[ssAstoria](../../../../includes/ssastoria-md.md)]o oferece suporte [!INCLUDE[ssODataShort](../../../../includes/ssodatashort-md.md)] às seguintes opções de consulta do sistema:  
  
|Opção de consulta|Descrição|  
|------------------|-----------------|  
|`$orderby`|Define uma ordem de classificação padrão para entidades no feed retornado. A consulta a seguir ordena o feed de clientes retornado por região e cidade:<br /><br /> `http://services.odata.org/Northwind/Northwind.svc/Customers?$orderby=Country,City`<br /><br /> Para obter mais informações, [consulte OData: Opção de consulta do sistema OrderBy (](https://go.microsoft.com/fwlink/?LinkId=186968)$OrderBy).|  
|`$top`|Especifica o número de entidades a serem incluídas no feed retornado. O exemplo a seguir ignora os 10 primeiros clientes e retorna os 10 seguintes:<br /><br /> `http://services.odata.org/Northwind/Northwind.svc/Customers?$skip=10&$top=10`<br /><br /> Para obter mais informações, [consulte OData: Opção de consulta de sistema superior (](https://go.microsoft.com/fwlink/?LinkId=186969)$Top).|  
|`$skip`|Especifica o número de entidades a serem ignoradas antes de começar a retornar entidades no feed. O exemplo a seguir ignora os 10 primeiros clientes e retorna os 10 seguintes:<br /><br /> `http://services.odata.org/Northwind/Northwind.svc/Customers?$skip=10&$top=10`<br /><br /> Para obter mais informações, [consulte OData: Opção ignorar consulta do sistema ($skip](https://go.microsoft.com/fwlink/?LinkId=186971)).|  
|`$filter`|Define uma expressão que filtra as entidades retornadas no feed com base em critérios específicos. Esta opção de consulta oferece suporte a um conjunto de operadores de comparação lógicos, operadores aritméticos e funções de consulta predefinidas que são usadas para avaliar a expressão de filtro. O exemplo a seguir retorna todos os pedidos cujos códigos postais não terminam em 100:<br /><br /> `http://services.odata.org/Northwind/Northwind.svc/Orders?$filter=not endswith(ShipPostalCode,'100')`<br /><br /> Para obter mais informações, [consulte OData: Opção de consulta do sistema de filtro](https://go.microsoft.com/fwlink/?LinkId=186972)($Filter).|  
|`$expand`|Especifica quais entidades relacionadas são retornadas pela consulta. As entidades relacionadas são incluídas como um feed ou uma entrada embutida na entidade retornada pela consulta. O exemplo a seguir retorna o pedido do cliente 'ALFKI' junto com os detalhes dos itens de cada ordem:<br /><br /> `http://services.odata.org/Northwind/Northwind.svc/Customers('ALFKI')/Orders?$expand=Order_Details`<br /><br /> Para obter mais informações, [consulte OData: Opção expandir consulta do sistema ($expand](https://go.microsoft.com/fwlink/?LinkId=186973)).|  
|`$select`|Especifica uma projeção que define as propriedades da entidade que são retornadas na projeção. Por padrão, todas as propriedades de uma entidade são retornadas em um feed. A seguinte consulta retorna apenas três propriedades da entidade `Customer`:<br /><br /> `http://services.odata.org/Northwind/Northwind.svc/Customers?$select=CustomerID,CompanyName,City`<br /><br /> Para obter mais informações, [consulte OData: Selecione a opção de consulta do sistema](https://go.microsoft.com/fwlink/?LinkID=186076)($SELECT).|  
|`$inlinecount`|Solicitações de que uma contagem do número de entidades retornadas no feed seja incluída no feed. Para obter mais informações, [consulte OData: Opção de consulta do sistema inlinecount (](https://go.microsoft.com/fwlink/?LinkId=186975)$inlinecount).|  
  
## <a name="addressing-relationships"></a>Direcionando relações  
 Além de tratar conjuntos de entidades e instâncias de entidade [!INCLUDE[ssODataShort](../../../../includes/ssodatashort-md.md)] , o também permite que você resolva as associações que representam as relações entre entidades. Essa funcionalidade é necessária para que seja possível criar ou alterar uma relação entre duas instâncias de entidade, como o remetente que está relacionado a um determinado pedido no banco de dados de exemplo Northwind. [!INCLUDE[ssODataShort](../../../../includes/ssodatashort-md.md)]dá suporte `$link` a um operador para atender especificamente às associações entre entidades. Por exemplo, o URI a seguir é especificado em uma mensagem de solicitação PUT HTTP para alterar o remetente do pedido especificado para um novo remetente.  
  
```  
http://services.odata.org/Northwind/Northwind.svc/Orders(10643)/$links/Shipper  
```  
  
 Para obter mais informações, [consulte OData: Endereçando links entre](https://go.microsoft.com/fwlink/?LinkId=187351)entradas.  
  
## <a name="consuming-the-returned-feed"></a>Consumindo o feed retornado  
 O URI de um [!INCLUDE[ssODataShort](../../../../includes/ssodatashort-md.md)] recurso permite que você resolva os dados de entidade expostos pelo serviço. Quando você insere um URI no campo endereço de um navegador da Web, uma [!INCLUDE[ssODataShort](../../../../includes/ssodatashort-md.md)] representação de feed do recurso solicitado é retornada. Para obter mais informações, consulte o guia de [início rápido do WCF Data Services](quickstart-wcf-data-services.md). Embora um navegador da Web possa ser útil para testar se um recurso de serviço de dados retorna os dados esperados, os serviços de dados de produção que também podem criar, atualizar e excluir dados são geralmente acessados pelo código do aplicativo ou por linguagens de script em uma página da Web. Para obter mais informações, consulte [usando um serviço de dados em um aplicativo cliente](using-a-data-service-in-a-client-application-wcf-data-services.md).  
  
## <a name="see-also"></a>Consulte também

- [Site do Protocolo Open Data](https://go.microsoft.com/fwlink/?LinkID=182204)
