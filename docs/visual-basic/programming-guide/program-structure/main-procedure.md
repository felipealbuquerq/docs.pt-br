---
title: Procedimento principal no Visual Basic
ms.date: 07/20/2015
f1_keywords:
- vb.Main
helpviewer_keywords:
- Main procedure
- Main method [Visual Basic]
- main function
ms.assetid: f0db283e-f283-4464-b521-b90858cc1b44
ms.openlocfilehash: 19c6fcb04a373d782db3deafc732f69bf20e7f0e
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69962767"
---
# <a name="main-procedure-in-visual-basic"></a>Procedimento principal no Visual Basic
Cada aplicativo de Visual Basic deve conter um procedimento `Main`chamado. Esse procedimento serve como o ponto de partida e o controle geral para seu aplicativo. O .NET Framework chama o `Main` procedimento quando ele carregou seu aplicativo e está pronto para passar o controle para ele. A menos que você esteja criando um aplicativo Windows Forms, você deve `Main` escrever o procedimento para aplicativos que são executados por conta própria.

 `Main`contém o código que é executado primeiro. No `Main`, você pode determinar qual formulário deve ser carregado primeiro quando o programa for iniciado, descobrir se uma cópia do seu aplicativo já está em execução no sistema, estabelecer um conjunto de variáveis para seu aplicativo ou abrir um banco de dados que o aplicativo requer.

## <a name="requirements-for-the-main-procedure"></a>Requisitos para o procedimento principal
 Um arquivo que é executado por conta própria (normalmente com extensão. exe) deve conter `Main` um procedimento. Uma biblioteca (por exemplo, com extensão. dll) não é executada por conta própria e não requer um `Main` procedimento. Os requisitos para os diferentes tipos de projetos que você pode criar são os seguintes:

- Os aplicativos de console são executados por conta própria e você deve fornecer pelo `Main` menos um procedimento.

- Windows Forms aplicativos são executados por conta própria. No entanto, o compilador de Visual Basic `Main` gera automaticamente um procedimento em tal aplicativo, e você não precisa escrever um.

- As bibliotecas de classes não exigem `Main` um procedimento. Isso inclui bibliotecas de controle do Windows e bibliotecas de controle da Web. Os aplicativos Web são implantados como bibliotecas de classes.

## <a name="declaring-the-main-procedure"></a>Declarando o procedimento principal
 Há quatro maneiras de declarar o `Main` procedimento. Ele pode usar argumentos ou não, e pode retornar um valor ou não.

> [!NOTE]
> Se você declarar `Main` em uma classe, deverá usar a `Shared` palavra-chave. Em um módulo, `Main` não precisa ser. `Shared`

- A maneira mais simples é declarar um `Sub` procedimento que não use argumentos nem retornar um valor.

    ```vb
    Module mainModule
        Sub Main()
            MsgBox("The Main procedure is starting the application.")
            ' Insert call to appropriate starting place in your code.
            MsgBox("The application is terminating.")
        End Sub
    End Module
    ```

- `Main`também pode retornar um `Integer` valor, que o sistema operacional usa como o código de saída para seu programa. Outros programas podem testar esse código examinando o valor ERRORLEVEL do Windows. Para retornar um código de saída, você deve `Main` declarar como `Function` um procedimento em vez `Sub` de um procedimento.

    ```vb
    Module mainModule
        Function Main() As Integer
            MsgBox("The Main procedure is starting the application.")
            Dim returnValue As Integer = 0
            ' Insert call to appropriate starting place in your code.
            ' On return, assign appropriate value to returnValue.
            ' 0 usually means successful completion.
            MsgBox("The application is terminating with error level " &
                 CStr(returnValue) & ".")
            Return returnValue
        End Function
    End Module
    ```

- `Main`também pode pegar uma `String` matriz como um argumento. Cada cadeia de caracteres na matriz contém um dos argumentos de linha de comando usados para invocar seu programa. Você pode executar ações diferentes dependendo de seus valores.

    ```vb
    Module mainModule
        Function Main(ByVal cmdArgs() As String) As Integer
            MsgBox("The Main procedure is starting the application.")
            Dim returnValue As Integer = 0
            ' See if there are any arguments.
            If cmdArgs.Length > 0 Then
                For argNum As Integer = 0 To UBound(cmdArgs, 1)
                    ' Insert code to examine cmdArgs(argNum) and take
                    ' appropriate action based on its value.
                Next
            End If
            ' Insert call to appropriate starting place in your code.
            ' On return, assign appropriate value to returnValue.
            ' 0 usually means successful completion.
            MsgBox("The application is terminating with error level " &
                 CStr(returnValue) & ".")
            Return returnValue
        End Function
    End Module
    ```

- Você pode declarar `Main` para examinar os argumentos de linha de comando, mas não retornar um código de saída, da seguinte maneira.

    ```vb
    Module mainModule
        Sub Main(ByVal cmdArgs() As String)
            MsgBox("The Main procedure is starting the application.")
            Dim returnValue As Integer = 0
            ' See if there are any arguments.
            If cmdArgs.Length > 0 Then
                For argNum As Integer = 0 To UBound(cmdArgs, 1)
                    ' Insert code to examine cmdArgs(argNum) and take
                    ' appropriate action based on its value.
                Next
            End If
            ' Insert call to appropriate starting place in your code.
            MsgBox("The application is terminating.")
        End Sub
    End Module
    ```
  
## <a name="see-also"></a>Consulte também

- <xref:Microsoft.VisualBasic.Interaction.MsgBox%2A>
- <xref:System.Array.Length%2A>
- <xref:Microsoft.VisualBasic.Information.UBound%2A>
- [Estrutura de um programa de Visual Basic](../../../visual-basic/programming-guide/program-structure/structure-of-a-visual-basic-program.md)
- [/main](../../../visual-basic/reference/command-line-compiler/main.md)
- [Compartilhado](../../../visual-basic/language-reference/modifiers/shared.md)
- [Instrução Sub](../../../visual-basic/language-reference/statements/sub-statement.md)
- [Instrução Function](../../../visual-basic/language-reference/statements/function-statement.md)
- [Tipo de Dados Integer](../../../visual-basic/language-reference/data-types/integer-data-type.md)
- [Tipo de Dados String](../../../visual-basic/language-reference/data-types/string-data-type.md)
