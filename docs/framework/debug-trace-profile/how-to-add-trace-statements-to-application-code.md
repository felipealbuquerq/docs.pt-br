---
title: 'Como: adicionar instruções de rastreamento ao código de um aplicativo'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- tracing [.NET Framework], conditional writes based on switches
- trace statements
- WriteLineIf method
- tracing [.NET Framework], adding trace statements
- Assert method, tracing code
- trace switches, conditional writes based on switches
- WriteIf method
ms.assetid: f3a93fa7-1717-467d-aaff-393e5c9828b4
author: mairaw
ms.author: mairaw
ms.openlocfilehash: 626e9823bbf7d379a21ae353a9189485259f3c42
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69948018"
---
# <a name="how-to-add-trace-statements-to-application-code"></a>Como: adicionar instruções de rastreamento ao código de um aplicativo
Os métodos usados com mais frequência para rastreamento são os métodos para gravar a saída em ouvintes: **Write**, **WriteIf**, **WriteLine**, **WriteLineIf**, **Assert**e **Fail**. Esses métodos podem ser divididos em duas categorias: **Grave**, **WriteLine**e reprovam toda a saída de emissão incondicionalmente, enquanto **WriteIf**, **WriteLineIf**e **Assert** testam uma condição booliana e gravam ou não gravam com base no valor da condição. **WriteIf** e **WriteLineIf** emitirão a saída se a condição for `true` e **Assert** emitirá a saída se a condição for `false`.  
  
 Ao criar sua estratégia de rastreamento e depuração, pense em como você deseja que a saída se assemelhe. Várias instruções **Write** preenchidas com informações não relacionadas criarão um log que é difícil de ser lido. Por outro lado, o uso de **WriteLine** para colocar instruções relacionadas em linhas separadas pode tornar difícil distinguir quais informações pertencem juntas. Em geral, use várias instruções **Write** quando desejar combinar informações de várias fontes para criar uma única mensagem informativa e use a instrução **WriteLine** quando desejar criar uma única mensagem completa.  
  
### <a name="to-write-a-complete-line"></a>Para gravar uma linha completa  
  
1. Chame o método <xref:System.Diagnostics.Trace.WriteLine%2A> or <xref:System.Diagnostics.Trace.WriteLineIf%2A>.  
  
     Um retorno de carro é acrescentado ao final da mensagem retornada por esse método, de modo que a próxima mensagem retornada por **Write**, **WriteIf**, **WriteLine** ou **WriteLineIf** comece na seguinte linha:  
  
    ```vb  
    Dim errorFlag As Boolean = False  
    Trace.WriteLine("Error in AppendData procedure.")  
    Trace.WriteLineIf(errorFlag, "Error in AppendData procedure.")  
    ```  
  
    ```csharp  
    bool errorFlag = false;  
    System.Diagnostics.Trace.WriteLine ("Error in AppendData procedure.");  
    System.Diagnostics.Trace.WriteLineIf(errorFlag,   
       "Error in AppendData procedure.");  
    ```  
  
### <a name="to-write-a-partial-line"></a>Para gravar uma linha parcial  
  
1. Chame o método <xref:System.Diagnostics.Trace.Write%2A> or <xref:System.Diagnostics.Trace.WriteIf%2A>.  
  
     A próxima mensagem colocada por uma instrução **Write**, **WriteIf**, **WriteLine** ou **WriteLineIf** começará na mesma linha da mensagem colocada pela instrução **Write** ou **WriteIf**:  
  
    ```vb  
    Dim errorFlag As Boolean = False  
    Trace.WriteIf(errorFlag, "Error in AppendData procedure.")  
    Debug.WriteIf(errorFlag, "Transaction abandoned.")  
    Trace.Write("Invalid value for data request")  
    ```  
  
    ```csharp  
    bool errorFlag = false;  
    System.Diagnostics.Trace.WriteIf(errorFlag,   
       "Error in AppendData procedure.");  
    System.Diagnostics.Debug.WriteIf(errorFlag, "Transaction abandoned.");  
    Trace.Write("Invalid value for data request");  
    ```  
  
### <a name="to-verify-that-certain-conditions-exist-either-before-or-after-you-execute-a-method"></a>Para verificar se certas condições existem antes ou depois de executar um método  
  
1. Chame o método <xref:System.Diagnostics.Trace.Assert%2A>.  
  
    ```vb  
    Dim i As Integer = 4  
    Trace.Assert(i = 5, "i is not equal to 5.")  
    ```  
  
    ```csharp  
    int i = 4;  
    System.Diagnostics.Trace.Assert(i == 5, "i is not equal to 5.");  
    ```  
  
    > [!NOTE]
    > Use **Assert** com o rastreamento e a depuração. Este exemplo gera a pilha de chamadas para qualquer ouvinte da coleção **Listeners**. Para obter mais informações, consulte [Declarações em código gerenciado](/visualstudio/debugger/assertions-in-managed-code) e <xref:System.Diagnostics.Debug.Assert%2A?displayProperty=nameWithType>.  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Diagnostics.Debug.WriteIf%2A?displayProperty=nameWithType>
- <xref:System.Diagnostics.Debug.WriteLineIf%2A?displayProperty=nameWithType>
- <xref:System.Diagnostics.Trace.WriteIf%2A?displayProperty=nameWithType>
- <xref:System.Diagnostics.Trace.WriteLineIf%2A?displayProperty=nameWithType>
- [Rastreando e instrumentando aplicativos](../../../docs/framework/debug-trace-profile/tracing-and-instrumenting-applications.md)
- [Como: Criar, inicializar e configurar opções de rastreamento](../../../docs/framework/debug-trace-profile/how-to-create-initialize-and-configure-trace-switches.md)
- [Opções de rastreamento](../../../docs/framework/debug-trace-profile/trace-switches.md)
- [Ouvintes de rastreamento](../../../docs/framework/debug-trace-profile/trace-listeners.md)
