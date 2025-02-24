---
title: Mapear restrições de esquema XML (XSD) chave para restrições de DataSet
ms.date: 03/30/2017
ms.assetid: 22664196-f270-4ebc-a169-70e16a83dfa1
ms.openlocfilehash: 8543f5b34ee2a80ff0154897cf7678b244a8d357
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/07/2019
ms.locfileid: "70786096"
---
# <a name="map-key-xml-schema-xsd-constraints-to-dataset-constraints"></a>Mapear restrições de esquema XML (XSD) chave para restrições de DataSet
Em um esquema, você pode especificar uma restrição de chave em um elemento ou atributo usando o elemento de **chave** . O elemento ou atributo no qual uma restrição de chave é especificada deve ter valores exclusivos em qualquer instância de esquema e não pode ter valores nulos.  
  
 A restrição de chave é semelhante à restrição UNIQUE, exceto que a coluna na qual uma restrição de chave é definida não pode ter valores nulos.  
  
 A tabela a seguir descreve os atributos **MSDATA** que você pode especificar no elemento **Key** .  
  
|Nome do atributo|Descrição|  
|--------------------|-----------------|  
|**msdata:ConstraintName**|Se esse atributo for especificado, seu valor será usado como o nome da restrição. Caso contrário, o atributo **Name** fornecerá o valor do nome da restrição.|  
|**msdata:PrimaryKey**|Se `PrimaryKey="true"` estiver presente, a propriedade **IsPrimaryKey** de restrição será definida como **true**, tornando-a uma chave primária. A propriedade de coluna **AllowDBNull** está definida como **false**, pois as chaves primárias não podem ter valores nulos.|  
  
 Na conversão do esquema no qual uma restrição de chave é especificada, o processo de mapeamento cria uma restrição UNIQUE na tabela com a propriedade de coluna **AllowDBNull** definida como **false** para cada coluna na restrição. A propriedade **IsPrimaryKey** da restrição UNIQUE também é definida como **false** , a menos que você `msdata:PrimaryKey="true"` tenha especificado no elemento **Key** . Isso é idêntico a uma restrição UNIQUE no esquema no qual `PrimaryKey="true"`.  
  
 No exemplo de esquema a seguir, o elemento **Key** especifica a restrição de chave no elemento **CustomerID** .  
  
```xml  
<xs:schema id="cod"  
            xmlns:xs="http://www.w3.org/2001/XMLSchema"   
            xmlns:msdata="urn:schemas-microsoft-com:xml-msdata">  
  <xs:element name="Customers">  
    <xs:complexType>  
      <xs:sequence>  
        <xs:element name="CustomerID" type="xs:string" minOccurs="0" />  
        <xs:element name="CompanyName" type="xs:string" minOccurs="0" />  
       <xs:element name="Phone" type="xs:string" />  
     </xs:sequence>  
   </xs:complexType>  
 </xs:element>  
<xs:element name="MyDataSet" msdata:IsDataSet="true">  
  <xs:complexType>  
    <xs:choice maxOccurs="unbounded">  
      <xs:element ref="Customers" />  
    </xs:choice>  
  </xs:complexType>  
   <xs:key  msdata:PrimaryKey="true"  
       msdata:ConstraintName="KeyCustID"  
          name="KeyConstCustomerID" >  
     <xs:selector xpath=".//Customers" />  
     <xs:field xpath="CustomerID" />  
    </xs:key>  
 </xs:element>  
</xs:schema>   
```  
  
 O elemento **Key** especifica que os valores do elemento filho **CustomerID** do elemento **Customers** devem ter valores exclusivos e não podem ter valores nulos. Ao traduzir o esquema XSD (linguagem de definição de esquema XML), o processo de mapeamento cria a seguinte tabela:  
  
```  
Customers(CustomerID, CompanyName, Phone)  
```  
  
 O mapeamento de esquema XML também cria um **UniqueConstraint** na coluna **CustomerID** , conforme mostrado a seguir <xref:System.Data.DataSet>. (Para simplificar, apenas as propriedades relevantes são mostradas.)  
  
```  
      DataSetName: MyDataSet  
TableName: customers  
  ColumnName: CustomerID  
      AllowDBNull: False  
      Unique: True  
  ConstraintName: KeyCustID  
      Table: customers  
      Columns: CustomerID   
      IsPrimaryKey: True  
```  
  
 No **conjunto** de um que é gerado, a propriedade **IsPrimaryKey** de **UniqueConstraint** é definida como **true** porque o esquema especifica `msdata:PrimaryKey="true"` no elemento **Key** .  
  
 O valor da propriedade **ConstraintName** do **UniqueConstraint** no **conjunto** de valores é o valor do atributo **MSDATA: ConstraintName** especificado no elemento **Key** no esquema.  
  
## <a name="see-also"></a>Consulte também

- [Mapeamento de restrições de esquema XML (XSD) exclusivos para restrições de conjunto de dados](mapping-xml-schema-xsd-constraints-to-dataset-constraints.md)
- [Gerando relações de conjunto de dados do esquema XML (XSD)](generating-dataset-relations-from-xml-schema-xsd.md)
- [ADO.NET Overview](../ado-net-overview.md) (Visão geral do ADO.NET)
