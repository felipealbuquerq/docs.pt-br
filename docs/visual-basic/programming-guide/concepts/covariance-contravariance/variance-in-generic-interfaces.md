---
title: Variação em Interfaces genéricas (Visual Basic)
ms.date: 07/20/2015
ms.assetid: cf4096d0-4bb3-45a9-9a6b-f01e29a60333
ms.openlocfilehash: 93e169f658a855e3b271e3e3affeadac5d7e5032
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64642393"
---
# <a name="variance-in-generic-interfaces-visual-basic"></a>Variação em Interfaces genéricas (Visual Basic)
O .NET Framework 4 introduziu o suporte à variação para diversas interfaces genéricas existentes. O suporte à variação possibilita a conversão implícita de classes que implementam essas interfaces. As seguintes interfaces agora são variantes:  
  
- <xref:System.Collections.Generic.IEnumerable%601> (T é covariante)  
  
- <xref:System.Collections.Generic.IEnumerator%601> (T é covariante)  
  
- <xref:System.Linq.IQueryable%601> (T é covariante)  
  
- <xref:System.Linq.IGrouping%602> (`TKey` e `TElement` são covariantes)  
  
- <xref:System.Collections.Generic.IComparer%601> (T é contravariante)  
  
- <xref:System.Collections.Generic.IEqualityComparer%601> (T é contravariante)  
  
- <xref:System.IComparable%601> (T é contravariante)  
  
 A covariância permite que um método tenha um tipo de retorno mais derivados que aquele definidos pelo parâmetro de tipo genérico da interface. Para ilustrar o recurso de covariância, considere estas interfaces genéricas: `IEnumerable(Of Object)` e `IEnumerable(Of String)`. A interface `IEnumerable(Of String)` não herda a interface `IEnumerable(Of Object)`. No entanto, o tipo `String` herda o tipo `Object` e, em alguns casos, talvez você queira atribuir objetos dessas interfaces uns aos outros. Isso será mostrado no exemplo de código a seguir.  
  
```vb  
Dim strings As IEnumerable(Of String) = New List(Of String)  
Dim objects As IEnumerable(Of Object) = strings  
```  
  
 Em versões anteriores do .NET Framework, esse código causa um erro de compilação no Visual Basic com `Option Strict On`. Mas agora você pode usar `strings` em vez de `objects`, conforme mostrado no exemplo anterior, porque a interface <xref:System.Collections.Generic.IEnumerable%601> é covariante.  
  
 A contravariância permite que um método tenha tipos de argumentos menos derivados que aquele especificado pelo parâmetro genérico da interface. Para ilustrar a contravariância, suponha que você tenha criado uma classe `BaseComparer` para comparar instâncias da classe `BaseClass`. A classe `BaseComparer` implementa a interface `IEqualityComparer(Of BaseClass)`. Como a interface <xref:System.Collections.Generic.IEqualityComparer%601> agora é contravariante, você pode usar `BaseComparer` para comparar instâncias de classes que herdam a classe `BaseClass`. Isso será mostrado no exemplo de código a seguir.  
  
```vb  
' Simple hierarchy of classes.  
Class BaseClass  
End Class  
  
Class DerivedClass  
    Inherits BaseClass  
End Class  
  
' Comparer class.  
Class BaseComparer  
    Implements IEqualityComparer(Of BaseClass)  
  
    Public Function Equals1(ByVal x As BaseClass,  
                            ByVal y As BaseClass) As Boolean _  
                            Implements IEqualityComparer(Of BaseClass).Equals  
        Return (x.Equals(y))  
    End Function  
  
    Public Function GetHashCode1(ByVal obj As BaseClass) As Integer _  
        Implements IEqualityComparer(Of BaseClass).GetHashCode  
        Return obj.GetHashCode  
    End Function  
End Class  
Sub Test()  
    Dim baseComparer As IEqualityComparer(Of BaseClass) = New BaseComparer  
    ' Implicit conversion of IEqualityComparer(Of BaseClass) to   
    ' IEqualityComparer(Of DerivedClass).  
    Dim childComparer As IEqualityComparer(Of DerivedClass) = baseComparer  
End Sub  
```  
  
 Para obter mais exemplos, consulte [usando variação em Interfaces para coleções genéricas (Visual Basic)](../../../../visual-basic/programming-guide/concepts/covariance-contravariance/using-variance-in-interfaces-for-generic-collections.md).  
  
 A variação em interfaces genéricas tem suporte somente para tipos de referência. Tipos de valor não dão suporte à variação. Por exemplo, `IEnumerable(Of Integer)` não pode ser convertido implicitamente em `IEnumerable(Of Object)`, porque inteiros são representados por um tipo de valor.  
  
```vb  
Dim integers As IEnumerable(Of Integer) = New List(Of Integer)  
' The following statement generates a compiler error  
' with Option Strict On, because Integer is a value type.  
' Dim objects As IEnumerable(Of Object) = integers  
```  
  
 Também é importante lembrar que as classes que implementam interfaces variantes ainda são invariantes. Por exemplo, embora <xref:System.Collections.Generic.List%601> implemente a interface covariante <xref:System.Collections.Generic.IEnumerable%601>, você não pode converter implicitamente `List(Of Object)` para `List(Of String)`. Isso é ilustrado no exemplo de código a seguir.  
  
```vb  
' The following statement generates a compiler error  
' because classes are invariant.  
' Dim list As List(Of Object) = New List(Of String)  
  
' You can use the interface object instead.  
Dim listObjects As IEnumerable(Of Object) = New List(Of String)  
```  
  
## <a name="see-also"></a>Consulte também

- [Usando variação em interfaces para coleções genéricas (Visual Basic)](../../../../visual-basic/programming-guide/concepts/covariance-contravariance/using-variance-in-interfaces-for-generic-collections.md)
- [Criando interfaces genéricas variantes (Visual Basic)](../../../../visual-basic/programming-guide/concepts/covariance-contravariance/creating-variant-generic-interfaces.md)
- [Interfaces genéricas](../../../../standard/generics/interfaces.md)
- [Variação em delegados (Visual Basic)](../../../../visual-basic/programming-guide/concepts/covariance-contravariance/variance-in-delegates.md)
