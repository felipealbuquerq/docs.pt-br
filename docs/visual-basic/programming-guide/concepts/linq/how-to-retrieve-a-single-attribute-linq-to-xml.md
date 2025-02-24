---
title: 'Como: Recuperar um único atributo (LINQ to XML) (Visual Basic)'
ms.date: 07/20/2015
ms.assetid: 11b938d7-c011-4048-900e-8b9183c41c94
ms.openlocfilehash: 635644783153765d61aff3c00fe16860642c29f1
ms.sourcegitcommit: eb9ff6f364cde6f11322e03800d8f5ce302f3c73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68710521"
---
# <a name="how-to-retrieve-a-single-attribute-linq-to-xml-visual-basic"></a>Como: Recuperar um único atributo (LINQ to XML) (Visual Basic)
Este tópico explica como recuperar um único atributo de um elemento, dado o nome do atributo. Isso é útil para gravar as expressões de consulta onde você deseja localizar um elemento que possui um atributo específico.  
  
 O método de <xref:System.Xml.Linq.XElement.Attribute%2A> da classe de <xref:System.Xml.Linq.XElement> retorna <xref:System.Xml.Linq.XAttribute> com o nome especificado.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa o método <xref:System.Xml.Linq.XElement.Attribute%2A>.  
  
```vb  
Dim cust As XElement = <PhoneNumbers>  
                           <Phone type="home">555-555-5555</Phone>  
                           <Phone type="work">555-555-6666</Phone>  
                       </PhoneNumbers>  
Dim elList = From el In cust...<Phone>  
For Each e As XElement In elList  
    Console.WriteLine(e.@type)  
Next  
```  
  
 Este exemplo localiza os descendentes na árvore chamada `Phone`, e localiza o atributo chamado `type`.  
  
 Esse código gera a seguinte saída:  
  
```  
home  
work  
```  
  
## <a name="example"></a>Exemplo  
 Se você deseja recuperar o valor do atributo, você pode convertê-lo, exatamente como você faz para com objetos de <xref:System.Xml.Linq.XElement> . O exemplo a seguir demonstra isso.  
  
```vb  
Dim cust As XElement = <PhoneNumbers>  
                           <Phone type="home">555-555-5555</Phone>  
                           <Phone type="work">555-555-6666</Phone>  
                       </PhoneNumbers>  
Dim elList As IEnumerable(Of XElement) = _  
    From el In cust...<Phone> _  
    Select el  
For Each el As XElement In elList  
    Console.WriteLine(el.@type)  
Next  
```  
  
 Esse código gera a seguinte saída:  
  
```  
home  
work  
```  
  
 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] fornece operadores cast explícitos para a classe <xref:System.Xml.Linq.XAttribute> para `string`, `bool`, `bool?`, `int`, `int?`, `uint`, `uint?`, `long`, `long?`, `ulong`, `ulong?`, `float`, `float?`, `double`, `double?`, `decimal`, `decimal?`, `DateTime`, `DateTime?`, `TimeSpan`, `TimeSpan?`, `GUID` e `GUID?`.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra o mesmo código para um atributo que está em um namespace. Para obter mais informações, consulte [visão geral de namespaces (LINQ to XML) (Visual Basic)](namespaces-overview-linq-to-xml.md).  
  
```vb  
Imports <xmlns:aw="http://www.adventure-works.com">  
  
Module Module1  
    Sub Main()  
        Dim cust As XElement = _  
            <aw:PhoneNumbers>  
                <aw:Phone aw:type="home">555-555-5555</aw:Phone>  
                <aw:Phone aw:type="work">555-555-6666</aw:Phone>  
            </aw:PhoneNumbers>  
        Dim elList As IEnumerable(Of XElement) = _  
            From el In cust...<aw:Phone> _  
            Select el  
        For Each el As XElement In elList  
            Console.WriteLine(el.@aw:type)  
        Next  
    End Sub  
End Module  
```  
  
 Esse código gera a seguinte saída:  
  
```  
home  
work  
```  
  
## <a name="see-also"></a>Consulte também

- [Eixos LINQ to XML (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/linq-to-xml-axes.md)
