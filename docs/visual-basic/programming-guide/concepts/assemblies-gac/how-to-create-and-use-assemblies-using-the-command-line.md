---
title: 'Como: Criar e usar Assemblies usando a linha de comando (Visual Basic)'
ms.date: 03/14/2018
ms.assetid: 229ff9fb-1bd1-403b-946b-526104864c60
ms.openlocfilehash: a30d4b3ea203a8b4d3ba621fc7b0310477ddf10d
ms.sourcegitcommit: c7a7e1468bf0fa7f7065de951d60dfc8d5ba89f5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65592689"
---
# <a name="how-to-create-and-use-assemblies-using-the-command-line-visual-basic"></a>Como: Criar e usar Assemblies usando a linha de comando (Visual Basic)
Um assembly ou uma DLL (biblioteca de vínculo dinâmico), está vinculada ao seu programa em tempo de execução. Para demonstrar a compilação e uso de uma DLL, considere o seguinte cenário:  
  
- `MathLibrary.DLL`: O arquivo de biblioteca que contém os métodos a serem chamados em tempo de execução. Neste exemplo, a DLL contém dois métodos, `Add` e `Multiply`.  
  
- `Add`: O arquivo de origem que contém o método `Add`. Ele retorna a soma de seus parâmetros. A classe `AddClass` que contém o método `Add` é um membro do namespace `UtilityMethods`.  
  
- `Mult`: O código-fonte que contém o método `Multiply`. Ele retorna o produto de seus parâmetros. A classe `MultiplyClass` que contém o método `Multiply` também é um membro do namespace `UtilityMethods`.  
  
- `TestCode`: O arquivo que contém o método `Main`. Ele usa os métodos no arquivo DLL para calcular a soma e o produto dos argumentos em tempo de execução.  
  
## <a name="example"></a>Exemplo  
  
```vb  
' File: Add.vb   
Namespace UtilityMethods  
    Public Class AddClass  
        Public Shared Function Add(ByVal i As Long, ByVal j As Long) As Long  
            Return i + j  
        End Function  
    End Class  
End Namespace  
```  
  
```vb  
' File: Mult.vb  
Namespace UtilityMethods  
    Public Class MultiplyClass  
        Public Shared Function Multiply(ByVal x As Long, ByVal y As Long) As Long  
            Return x * y  
        End Function  
    End Class  
End Namespace  
```  
  
```vb  
' File: TestCode.vb  
  
Imports UtilityMethods  
  
Module Test  
  
    Sub Main(ByVal args As String())  
  
        System.Console.WriteLine("Calling methods from MathLibrary.DLL:")  
  
        If args.Length <> 2 Then  
            System.Console.WriteLine("Usage: TestCode <num1> <num2>")  
            Return  
        End If  
  
        Dim num1 As Long = Long.Parse(args(0))  
        Dim num2 As Long = Long.Parse(args(1))  
  
        Dim sum As Long = AddClass.Add(num1, num2)  
        Dim product As Long = MultiplyClass.Multiply(num1, num2)  
  
        System.Console.WriteLine("{0} + {1} = {2}", num1, num2, sum)  
        System.Console.WriteLine("{0} * {1} = {2}", num1, num2, product)  
  
    End Sub  
  
End Module  
  
' Output (assuming 1234 and 5678 are entered as command-line arguments):  
' Calling methods from MathLibrary.DLL:  
' 1234 + 5678 = 6912  
' 1234 * 5678 = 7006652  
```  
  
 Esse arquivo contém o algoritmo que usa os métodos DLL `Add` e `Multiply`. Ele começa com a análise dos argumentos `num1` e `num2`, inseridos na linha de comando. Em seguida, ele calcula a soma, usando o método `Add` na classe `AddClass` e o produto, usando o método `Multiply` na classe `MultiplyClass`.  
  
 Observe que o `Imports` instrução no início do arquivo permite que você use os nomes de classe não qualificados para fazer referência a métodos de DLL em tempo de compilação, da seguinte maneira:  
  
```vb  
MultiplyClass.Multiply(num1, num2)  
```  
  
 Caso contrário, você deve usar nomes totalmente qualificados, da seguinte maneira:  
  
```vb  
UtilityMethods.MultiplyClass.Multiply(num1, num2)  
```  
  
## <a name="execution"></a>Execução  
 Para executar o programa, digite o nome do arquivo EXE, seguido de dois números, da seguinte maneira:  
  
 `TestCode 1234 5678`  
  
## <a name="see-also"></a>Consulte também

- [Conceitos de Programação](../../../../visual-basic/programming-guide/concepts/index.md)
- [Assemblies no .NET](../../../../standard/assembly/index.md)
- [Criando uma classe para conter funções de DLL](../../../../framework/interop/creating-a-class-to-hold-dll-functions.md)
