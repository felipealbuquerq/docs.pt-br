---
title: Modificando dados de valores grandes (max) no ADO.NET
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 8aca5f00-d80e-4320-81b3-016d0466f7ee
ms.openlocfilehash: 34f0a61329667a42aa42693e93169a5b6fb0aa5e
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/07/2019
ms.locfileid: "70792044"
---
# <a name="modifying-large-value-max-data-in-adonet"></a>Modificando dados de valores grandes (max) no ADO.NET
Os tipos de dados de objetos grandes (LOB) são os que excedem o tamanho de linha máximo de 8 quilobytes (KB). O SQL Server fornece um especificador `max` para os tipos de dados `varchar`, `nvarchar` e `varbinary` para permitir o armazenamento de valores grandes como 2^32 bytes. As colunas da tabela e variáveis Transact-SQL podem especificar os tipos de dados `varchar(max)`, `nvarchar(max)` ou `varbinary(max)`. No ADO.NET, os tipos de dados `max` podem ser encontrados por `DataReader` e também podem ser especificados como valores de parâmetro de entrada e saída sem nenhuma manipulação especial. Para tipos de dados `varchar`, os dados podem ser recuperados e atualizados incrementalmente.  
  
 Os tipos de dados `max` podem ser usados para comparações, como variáveis Transact-SQL, e para concatenação. Eles também podem ser usados em cláusulas DISTINCT, ORDER BY e GROUP BY de uma instrução SELECT bem como em agregações, junções e subconsultas.  
  
 A tabela a seguir fornece links para a documentação nos Manuais Online do SQL Server.  
  
 **SQL Server Books Online** (Guias online do SQL Server)  
  
1. [Usando tipos de dados de valor grande](https://go.microsoft.com/fwlink/?LinkId=120498)  
  
## <a name="large-value-type-restrictions"></a>Restrições de tipo de valores grandes  
 As seguintes restrições aplicam-se a tipos de dados `max`, que não existem para tipos de dados menores:  
  
- Um `sql_variant` não pode conter um tipo de dados `varchar`.  
  
- As colunas `varchar` grandes não podem ser especificadas como uma coluna de chave em um índice. Elas são permitidas em uma coluna incluída em um índice não clusterizado.  
  
- As colunas `varchar` não podem ser usadas como colunas-chave de particionamento.  
  
## <a name="working-with-large-value-types-in-transact-sql"></a>Trabalhando com tipos de valores grandes no Transact-SQL  
 A função Transact-SQL `OPENROWSET` é um método único de conexão e acesso a dados remotos. Ela inclui todas as informações de conexão necessárias para acessar dados remotos de uma fonte de dados OLE DB. `OPENROWSET` pode ser referenciada na cláusula FROM de uma consulta como se fosse um nome de tabela. Ela também pode ser referenciada como a tabela de destino de uma instrução INSERT, UPDATE ou DELETE, sujeita aos recursos do provedor OLE DB.  
  
 A função `OPENROWSET` inclui o provedor de conjunto de linhas `BULK`, que permite que você leia dados diretamente de um arquivo sem carregar os dados em uma tabela de destino. Isso permite usar `OPENROWSET` em uma instrução INSERT SELECT simples.  
  
 Os `OPENROWSET BULK` argumentos de opção fornecem um controle significativo sobre onde começar e terminar a leitura de dados, como lidar com erros e como os dados são interpretados. Por exemplo, você pode especificar que o arquivo de dados seja lido como uma única linha, um conjunto de linhas de coluna única do tipo `varbinary`, `varchar` ou `nvarchar`. Para a sintaxe e as opções completas, consulte os Manuais Online do SQL Server.  
  
 O exemplo a seguir insere uma foto na tabela ProductPhoto no banco de dados de exemplo AdventureWorks. Ao usar o `BULK OPENROWSET` provedor, você deve fornecer a lista nomeada de colunas mesmo se não estiver inserindo valores em todas as colunas. A chave primária nesse caso é definida como uma coluna de identidade e pode ser omitida da lista de colunas. Observe que você também deve fornecer um nome de correlação no final da instrução `OPENROWSET`, que, nesse caso, é ThumbnailPhoto. Isso correlaciona-se com a coluna na tabela `ProductPhoto` em que o arquivo está sendo carregado.  
  
```  
INSERT Production.ProductPhoto (  
    ThumbnailPhoto,   
    ThumbnailPhotoFilePath,   
    LargePhoto,   
    LargePhotoFilePath)  
SELECT ThumbnailPhoto.*, null, null, N'tricycle_pink.gif'  
FROM OPENROWSET   
    (BULK 'c:\images\tricycle.jpg', SINGLE_BLOB) ThumbnailPhoto  
```  
  
## <a name="updating-data-using-update-write"></a>Atualizando dados usando UPDATE .WRITE  
 A instrução UPDATE do Transact-SQL tem a nova sintaxe WRITE para modificar o conteúdo das colunas `varchar(max)`, `nvarchar(max)` ou `varbinary(max)`. Isso permite que você execute atualizações parciais dos dados. A sintaxe da instrução UPDATE .WRITE é mostrada aqui na forma abreviada:  
  
 UPDATE  
  
 *{\<objeto >* }  
  
 SET  
  
 { *column_name* = { .WRITE ( *expression* , @Offset , @Length ) }  
  
 O método WRITE especifica que uma seção do valor de *column_name* será modificada. A expressão é o valor que será copiado para o *column_name*, o `@Offset` é o ponto inicial no qual a expressão será gravada e o `@Length` argumento é o comprimento da seção na coluna.  
  
|If|Then|  
|--------|----------|  
|A expressão é definida como NULL|`@Length`é ignorado e o valor em *column_name* é truncado no especificado `@Offset`.|  
|`@Offset`é nulo|A operação de atualização acrescenta a expressão no final do valor de *column_name* existente e `@Length` é ignorada.|  
|`@Offset` é maior do que o comprimento do valor column_name|O SQL Server retornará um erro.|  
|`@Length`é nulo|A operação de atualização remove todos os dados de `@Offset` para o final do valor `column_name`.|  
  
> [!NOTE]
> Nem `@Offset` ou `@Length` pode ser um número negativo.  
  
## <a name="example"></a>Exemplo  
 Este exemplo de Transact-SQL atualiza um valor parcial em DocumentSummary, uma coluna `nvarchar(max)` na tabela Document no banco de dados AdventureWorks. A palavra 'components' é substituída pela palavra 'features' especificando a palavra de substituição, o local de início (deslocamento) da palavra a ser substituída nos dados existentes e o número de caracteres a serem substituídos (comprimento). O exemplo inclui instruções SELECT antes e depois da instrução UPDATE para comparar resultados.  
  
```  
USE AdventureWorks;  
GO  
--View the existing value.  
SELECT DocumentSummary  
FROM Production.Document  
WHERE DocumentID = 3;  
GO  
-- The first sentence of the results will be:  
-- Reflectors are vital safety components of your bicycle.  
  
--Modify a single word in the DocumentSummary column  
UPDATE Production.Document  
SET DocumentSummary .WRITE (N'features',28,10)  
WHERE DocumentID = 3 ;  
GO   
--View the modified value.  
SELECT DocumentSummary  
FROM Production.Document  
WHERE DocumentID = 3;  
GO  
-- The first sentence of the results will be:  
-- Reflectors are vital safety features of your bicycle.  
```  
  
## <a name="working-with-large-value-types-in-adonet"></a>Trabalhando com tipos de valores grandes no ADO.NET  
 Você pode trabalhar com tipos de valor grande no ADO.net especificando tipos de valor grande <xref:System.Data.SqlClient.SqlParameter> como objetos em <xref:System.Data.SqlClient.SqlDataReader> um para retornar um conjunto de resultados ou usando um <xref:System.Data.SqlClient.SqlDataAdapter> para preencher um `DataSet` / `DataTable`. Não há nenhuma diferença entre o modo como você trabalha com um tipo de valor grande e o tipo de dados relacionado de menor valor.  
  
### <a name="using-getsqlbytes-to-retrieve-data"></a>Usando GetSqlBytes para recuperar dados  
 O método `GetSqlBytes` do <xref:System.Data.SqlClient.SqlDataReader> pode ser usado para recuperar o conteúdo de uma coluna `varbinary(max)`. O fragmento de código a seguir supõe um objeto <xref:System.Data.SqlClient.SqlCommand> chamado `cmd` que seleciona dados `varbinary(max)` de uma tabela e um objeto <xref:System.Data.SqlClient.SqlDataReader> nomeados `reader` que recuperam os dados como <xref:System.Data.SqlTypes.SqlBytes>.  
  
```vb  
reader = cmd.ExecuteReader(CommandBehavior.CloseConnection)  
While reader.Read()  
    Dim bytes As SqlBytes = reader.GetSqlBytes(0)  
End While  
```  
  
```csharp  
reader = cmd.ExecuteReader(CommandBehavior.CloseConnection);  
while (reader.Read())  
    {  
        SqlBytes bytes = reader.GetSqlBytes(0);  
    }  
```  
  
### <a name="using-getsqlchars-to-retrieve-data"></a>Usando GetSqlChars para recuperar dados  
 O método `GetSqlChars` do <xref:System.Data.SqlClient.SqlDataReader> pode ser usado para recuperar o conteúdo de uma coluna `varchar(max)` ou `nvarchar(max)`. O fragmento de código a seguir supõe um objeto <xref:System.Data.SqlClient.SqlCommand> chamado `cmd` que seleciona dados `nvarchar(max)` de uma tabela e um objeto <xref:System.Data.SqlClient.SqlDataReader> nomeados `reader` que recuperam os dados.  
  
```vb  
reader = cmd.ExecuteReader(CommandBehavior.CloseConnection)  
While reader.Read()  
    Dim buffer As SqlChars = reader.GetSqlChars(0)  
End While  
```  
  
```csharp  
reader = cmd.ExecuteReader(CommandBehavior.CloseConnection);  
while (reader.Read())  
{  
    SqlChars buffer = reader.GetSqlChars(0);  
}  
```  
  
### <a name="using-getsqlbinary-to-retrieve-data"></a>Usando GetSqlBinary para recuperar dados  
 O método `GetSqlBinary` de um <xref:System.Data.SqlClient.SqlDataReader> pode ser usado para recuperar o conteúdo de uma coluna `varbinary(max)`. O fragmento de código a seguir supõe um objeto <xref:System.Data.SqlClient.SqlCommand> chamado `cmd` que seleciona dados `varbinary(max)` de uma tabela e um objeto <xref:System.Data.SqlClient.SqlDataReader> nomeados `reader` que recuperam os dados como um fluxo <xref:System.Data.SqlTypes.SqlBinary>.  
  
```vb  
reader = cmd.ExecuteReader(CommandBehavior.CloseConnection)  
While reader.Read()  
    Dim binaryStream As SqlBinary = reader.GetSqlBinary(0)  
End While  
```  
  
```csharp  
reader = cmd.ExecuteReader(CommandBehavior.CloseConnection);  
while (reader.Read())  
    {  
        SqlBinary binaryStream = reader.GetSqlBinary(0);  
    }  
```  
  
### <a name="using-getbytes-to-retrieve-data"></a>Usando GetBytes para recuperar dados  
 O método `GetBytes` de um <xref:System.Data.SqlClient.SqlDataReader> lê um fluxo de bytes do deslocamento da coluna especificada em uma matriz de bytes que inicia no deslocamento da matriz especificada. O fragmento de código a seguir supõe um objeto <xref:System.Data.SqlClient.SqlDataReader> chamado `reader` que recupera bytes em uma matriz de bytes. Observe que, ao contrário de `GetSqlBytes`, `GetBytes` exige um tamanho para o buffer da matriz.  
  
```vb  
While reader.Read()  
    Dim buffer(4000) As Byte  
    Dim byteCount As Integer = _  
    CInt(reader.GetBytes(1, 0, buffer, 0, 4000))  
End While  
```  
  
```csharp  
while (reader.Read())  
{  
    byte[] buffer = new byte[4000];  
    long byteCount = reader.GetBytes(1, 0, buffer, 0, 4000);  
}  
```  
  
### <a name="using-getvalue-to-retrieve-data"></a>Usando GetValue para recuperar dados  
 O método `GetValue` de um <xref:System.Data.SqlClient.SqlDataReader> lê o valor de deslocamento da coluna especificada em uma matriz. O seguinte fragmento de código supõe um objeto <xref:System.Data.SqlClient.SqlDataReader> chamado `reader` que recupera os dados binários do deslocamento da primeira coluna e, em seguida, os dados de cadeia de caracteres do deslocamento da segunda coluna.  
  
```vb  
While reader.Read()  
    ' Read the data from varbinary(max) column  
    Dim binaryData() As Byte = CByte(reader.GetValue(0))  
  
    ' Read the data from varchar(max) or nvarchar(max) column  
    Dim stringData() As String = Cstr((reader.GetValue(1))  
End While  
```  
  
```csharp  
while (reader.Read())  
{  
    // Read the data from varbinary(max) column  
    byte[] binaryData = (byte[])reader.GetValue(0);  
  
    // Read the data from varchar(max) or nvarchar(max) column  
    String stringData = (String)reader.GetValue(1);  
}  
```  
  
## <a name="converting-from-large-value-types-to-clr-types"></a>Convertendo de tipos de valores grandes para tipos CLR  
 Você pode converter o conteúdo de uma coluna `varchar(max)` ou `nvarchar(max)` usando qualquer um dos métodos de conversão de cadeia de caracteres, como `ToString`. O fragmento de código a seguir supõe um objeto <xref:System.Data.SqlClient.SqlDataReader> chamado `reader` que recupera os dados.  
  
```vb  
While reader.Read()  
    Dim str as String = reader(0).ToString()  
    Console.WriteLine(str)  
End While  
```  
  
```csharp  
while (reader.Read())  
{  
     string str = reader[0].ToString();  
     Console.WriteLine(str);  
}  
```  
  
### <a name="example"></a>Exemplo  
 O código a seguir recupera o nome e o objeto `LargePhoto` da tabela `ProductPhoto` no banco de dados `AdventureWorks` e o salva em um arquivo. O assembly precisa ser compilado com uma referência ao namespace <xref:System.Drawing>.  O método <xref:System.Data.SqlClient.SqlDataReader.GetSqlBytes%2A> do <xref:System.Data.SqlClient.SqlDataReader> retorna um objeto <xref:System.Data.SqlTypes.SqlBytes> que expõe uma propriedade `Stream`. O código usa isso para criar um novo `Bitmap` objeto e, em seguida, salva-o `ImageFormat`no GIF.  
  
 [!code-csharp[DataWorks LargeValueType.Photo#1](../../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks LargeValueType.Photo/CS/source.cs#1)]
 [!code-vb[DataWorks LargeValueType.Photo#1](../../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks LargeValueType.Photo/VB/source.vb#1)]  
  
## <a name="using-large-value-type-parameters"></a>Usando parâmetros de tipo de valores grandes  
 Os tipos de valores grandes podem ser usados em objetos <xref:System.Data.SqlClient.SqlParameter> da mesma maneira que você usa tipos de valores menores em objetos <xref:System.Data.SqlClient.SqlParameter>. Você pode recuperar tipos de valor grande <xref:System.Data.SqlClient.SqlParameter> como valores, conforme mostrado no exemplo a seguir. O código presume que o seguinte procedimento armazenado GetDocumentSummary exista no banco de dados de exemplo AdventureWorks. O procedimento armazenado usa um parâmetro de entrada @DocumentID chamado e retorna o conteúdo da coluna DocumentSummary no parâmetro @DocumentSummary de saída.  
  
```  
CREATE PROCEDURE GetDocumentSummary   
(  
    @DocumentID int,  
    @DocumentSummary nvarchar(MAX) OUTPUT  
)  
AS  
SET NOCOUNT ON  
SELECT  @DocumentSummary=Convert(nvarchar(MAX), DocumentSummary)  
FROM    Production.Document  
WHERE   DocumentID=@DocumentID  
```  
  
### <a name="example"></a>Exemplo  
 O código do ADO.NET cria objetos <xref:System.Data.SqlClient.SqlConnection> e <xref:System.Data.SqlClient.SqlCommand> para executar o procedimento armazenado GetDocumentSummary e recuperar o resumo do documento, que está armazenado como um tipo de valor grande. O código passa um valor para o @DocumentID parâmetro de entrada e exibe os resultados passados @DocumentSummary no parâmetro output na janela do console.  
  
 [!code-csharp[DataWorks LargeValueType.Param#1](../../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks LargeValueType.Param/CS/source.cs#1)]
 [!code-vb[DataWorks LargeValueType.Param#1](../../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks LargeValueType.Param/VB/source.vb#1)]  
  
## <a name="see-also"></a>Consulte também

- [SQL Server Binary and Large-Value Data](sql-server-binary-and-large-value-data.md) (Dados binários e de valor grande do SQL Server)
- [Mapeamentos de tipo de dados do SQL Server](../sql-server-data-type-mappings.md)
- [SQL Server Data Operations in ADO.NET](sql-server-data-operations.md) (Operações de dados do SQL Server no ADO.NET)
- [ADO.NET Overview](../ado-net-overview.md) (Visão geral do ADO.NET)
