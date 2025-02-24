---
title: Mapeamento externo
ms.date: 03/30/2017
ms.assetid: 076606b8-d889-4ba0-b5da-ae577b146f23
ms.openlocfilehash: 39cdd7b23bd90ff8938dda9eee630149ce6ddbea
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/07/2019
ms.locfileid: "70793993"
---
# <a name="external-mapping"></a>Mapeamento externo
[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)]dá suporte ao *mapeamento externo*, um processo pelo qual você usa um arquivo XML separado para especificar o mapeamento entre o modelo de dados do banco de dado e o modelo de objeto. As vantagens de usar um arquivo de mapeamento externo incluem o seguinte:  
  
- Você pode manter seu código de mapeamento fora de seu código do aplicativo. Essa abordagem reduz a confusão no código do aplicativo.  
  
- Você pode manipular um arquivo de mapeamento externo algo como um arquivo de configuração. Por exemplo, você pode atualizar como o aplicativo se comporta após enviar os binários apenas alternando para fora o arquivo de mapeamento externo.  
  
## <a name="requirements"></a>Requisitos  
 O arquivo de mapeamento deve ser um arquivo XML e o arquivo deve ser validado [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] em relação a um arquivo de definição de esquema (. xsd).  
  
 As seguintes regras se aplicam:  
  
- O arquivo de mapeamento deve ser um arquivo XML.  
  
- O arquivo de mapeamento de XML deve ser válido no arquivo de definição de esquema XML. Para obter mais informações, confira [Como: Valide os arquivos](how-to-validate-dbml-and-external-mapping-files.md)dbml e de mapeamento externo.  
  
- As substituições externos de mapeamento atributos com o mapeamento. Ou seja quando você usa uma fonte externa de mapeamento para criar <xref:System.Data.Linq.DataContext>, <xref:System.Data.Linq.DataContext> ignora todos os atributos de mapeamento que você criou em classes. Esse comportamento é verdadeiro se a classe está incluída no arquivo de mapeamento externo.  
  
- [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] não oferece suporte ao uso híbrido das duas abordagens mapeando (com base em atributos e externos).  
  
## <a name="xml-schema-definition-file"></a>Arquivo de Definição de Esquema XML  
 O mapeamento externa em [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] deve ser válido com a seguinte definição de esquema XML.  
  
 Distinguir este arquivo de definição do esquema do arquivo de definição do esquema que é usado para validar um arquivo DBML. Para obter mais informações, consulte [geração de código no LINQ to SQL](code-generation-in-linq-to-sql.md)).  
  
> [!NOTE]
> Os usuários do Visual Studio também encontrarão esse arquivo XSD na caixa de diálogo esquemas XML como "LinqToSqlMapping. xsd". Para usar esse arquivo corretamente para validar um arquivo de mapeamento externo, [consulte Como: Valide os arquivos](how-to-validate-dbml-and-external-mapping-files.md)dbml e de mapeamento externo.  
  
```  
?<?xml version="1.0" encoding="utf-16"?>  
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" targetNamespace="http://schemas.microsoft.com/linqtosql/mapping/2007" xmlns="http://schemas.microsoft.com/linqtosql/mapping/2007"  
elementFormDefault="qualified" >  
  <xs:element name="Database" type="Database" />  
  <xs:complexType name="Database">  
    <xs:sequence>  
      <xs:element name="Table" type="Table" minOccurs="0" maxOccurs="unbounded" />  
      <xs:element name="Function" type="Function" minOccurs="0" maxOccurs="unbounded" />  
    </xs:sequence>  
    <xs:attribute name="Name" type="xs:string" use="optional" />  
    <xs:attribute name="Provider" type="xs:string" use="optional" />  
  </xs:complexType>  
  <xs:complexType name="Table">  
    <xs:sequence>  
      <xs:element name="Type" type="Type" minOccurs="1" maxOccurs="1" />  
    </xs:sequence>  
    <xs:attribute name="Name" type="xs:string" use="optional" />  
    <xs:attribute name="Member" type="xs:string" use="optional" />  
  </xs:complexType>  
  <xs:complexType name="Type">  
    <xs:sequence>  
      <xs:choice minOccurs="0" maxOccurs="unbounded">  
        <xs:element name="Column" type="Column" minOccurs="0" maxOccurs="unbounded" />  
        <xs:element name="Association" type="Association" minOccurs="0" maxOccurs="unbounded" />  
      </xs:choice>  
      <xs:element name="Type" type="Type" minOccurs="0" maxOccurs="unbounded" />  
    </xs:sequence>  
    <xs:attribute name="Name" type="xs:string" use="required" />  
    <xs:attribute name="InheritanceCode" type="xs:string" use="optional" />  
    <xs:attribute name="IsInheritanceDefault" type="xs:boolean" use="optional" />  
  </xs:complexType>  
  <xs:complexType name="Column">  
    <xs:attribute name="Name" type="xs:string" use="optional" />  
    <xs:attribute name="Member" type="xs:string" use="required" />  
    <xs:attribute name="Storage" type="xs:string" use="optional" />  
    <xs:attribute name="DbType" type="xs:string" use="optional" />  
    <xs:attribute name="IsPrimaryKey" type="xs:boolean" use="optional" />  
    <xs:attribute name="IsDbGenerated" type="xs:boolean" use="optional" />  
    <xs:attribute name="CanBeNull" type="xs:boolean" use="optional" />  
    <xs:attribute name="UpdateCheck" type="UpdateCheck" use="optional" />  
    <xs:attribute name="IsDiscriminator" type="xs:boolean" use="optional" />  
    <xs:attribute name="Expression" type="xs:string" use="optional" />  
    <xs:attribute name="IsVersion" type="xs:boolean" use="optional" />  
    <xs:attribute name="AutoSync" type="AutoSync" use="optional" />  
  </xs:complexType>  
  <xs:complexType name="Association">  
    <xs:attribute name="Name" type="xs:string" use="optional" />  
    <xs:attribute name="Member" type="xs:string" use="required" />  
    <xs:attribute name="Storage" type="xs:string" use="optional" />  
    <xs:attribute name="ThisKey" type="xs:string" use="optional" />  
    <xs:attribute name="OtherKey" type="xs:string" use="optional" />  
    <xs:attribute name="IsForeignKey" type="xs:boolean" use="optional" />  
    <xs:attribute name="IsUnique" type="xs:boolean" use="optional" />  
    <xs:attribute name="DeleteRule" type="xs:string" use="optional" />  
    <xs:attribute name="DeleteOnNull" type="xs:boolean" use="optional" />  
  </xs:complexType>  
  <xs:complexType name="Function">  
    <xs:sequence>  
      <xs:element name="Parameter" type="Parameter" minOccurs="0" maxOccurs="unbounded" />  
      <xs:choice>  
        <xs:element name="ElementType" type="Type" minOccurs="0" maxOccurs="unbounded" />  
        <xs:element name="Return" type="Return" minOccurs="0" maxOccurs="1" />  
      </xs:choice>  
    </xs:sequence>  
    <xs:attribute name="Name" type="xs:string" use="optional" />  
    <xs:attribute name="Method" type="xs:string" use="required" />  
    <xs:attribute name="IsComposable" type="xs:boolean" use="optional" />  
  </xs:complexType>  
  <xs:complexType name="Parameter">  
    <xs:attribute name="Name" type="xs:string" use="optional" />  
    <xs:attribute name="Parameter" type="xs:string" use="required" />  
    <xs:attribute name="DbType" type="xs:string" use="optional" />  
    <xs:attribute name="Direction" type="ParameterDirection" use="optional" />  
  </xs:complexType>  
  <xs:complexType name="Return">  
    <xs:attribute name="DbType" type="xs:string" use="optional" />  
  </xs:complexType>  
  <xs:simpleType name="UpdateCheck">  
    <xs:restriction base="xs:string">  
      <xs:enumeration value="Always" />  
      <xs:enumeration value="Never" />  
      <xs:enumeration value="WhenChanged" />  
    </xs:restriction>  
  </xs:simpleType>  
  <xs:simpleType name="ParameterDirection">  
    <xs:restriction base="xs:string">  
      <xs:enumeration value="In" />  
      <xs:enumeration value="Out" />  
      <xs:enumeration value="InOut" />  
    </xs:restriction>  
  </xs:simpleType>  
  <xs:simpleType name="AutoSync">  
    <xs:restriction base="xs:string">  
      <xs:enumeration value="Never" />  
      <xs:enumeration value="OnInsert" />  
      <xs:enumeration value="OnUpdate" />  
      <xs:enumeration value="Always" />  
      <xs:enumeration value="Default" />  
    </xs:restriction>  
  </xs:simpleType>  
</xs:schema>  
```  
  
## <a name="see-also"></a>Consulte também

- [Geração de código em LINQ to SQL](code-generation-in-linq-to-sql.md)
- [Referência](reference.md)
- [Como: Gerar o modelo de objeto como um arquivo externo](how-to-generate-the-object-model-as-an-external-file.md)
