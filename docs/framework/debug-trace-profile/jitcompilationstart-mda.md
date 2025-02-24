---
title: MDA jitCompilationStart
ms.date: 03/30/2017
helpviewer_keywords:
- JIT compilation
- MDAs (managed debugging assistants), JIT compilation
- JitCompilationStart MDA
- managed debugging assistants (MDAs), JIT compilation
ms.assetid: 5ffd2857-d0ba-4342-9824-9ffe04ec135d
author: mairaw
ms.author: mairaw
ms.openlocfilehash: 80473e01581a372c193c4b816a37166b73d57824
ms.sourcegitcommit: 205b9a204742e9c77256d43ac9d94c3f82909808
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70854152"
---
# <a name="jitcompilationstart-mda"></a>MDA jitCompilationStart
O MDA (Assistente de Depuração Gerenciado) de `jitCompilationStart` é ativado para relatar quando o compilador JIT (Just-In-Time) começa a compilar uma função.  
  
## <a name="symptoms"></a>Sintomas  
 O tamanho do conjunto de trabalho aumenta para um programa que já está no formato de imagem nativo porque mscorjit é carregado no processo.  
  
## <a name="cause"></a>Causa  
 Nem todos os assemblies dos quais o programa depende foram gerados em formato nativo ou aqueles que foram não tiveram seu registro feito corretamente.  
  
## <a name="resolution"></a>Resolução  
 Habilitar esse MDA permite que você determine qual função está sendo compilada via JIT. Determine se o assembly que contém a função é gerado no formato nativo e registrado corretamente.  
  
## <a name="effect-on-the-runtime"></a>Efeito sobre o tempo de execução  
 Esse MDA registra uma mensagem logo antes de um método ser compilado via JIT, então habilitar esse MDA tem um impacto significativo no desempenho. Observe que, se um método é embutido, esse MDA não gerará uma mensagem separada.  
  
## <a name="output"></a>Saída  
 O exemplo de código a seguir mostra uma saída de exemplo. Nesse caso, a saída mostra que, no teste de assembly, o método "m" na classe "ns2.CO" foi compilado via JIT.  
  
```output
method name="Test!ns2.C0::m"  
```  
  
## <a name="configuration"></a>Configuração  
 O arquivo de configuração a seguir mostra uma variedade de filtros que podem ser utilizados para filtrar quais métodos são relatados quando eles são compilados via JIT pela primeira vez. Você pode especificar que todos os métodos sejam relatados definindo o valor do atributo name \*como.  
  
```xml  
<mdaConfig>  
  <assistants>  
    <jitCompilationStart>  
      <methods>  
        <match name="C0::m" />  
        <match name="MyMethod" />  
        <match name="C2::*" />  
        <match name="ns0::*" />  
        <match name="ns1.C0::*" />  
        <match name="ns2.C0::m" />  
        <match name="ns2.C0+N0::m" />  
      </methods>  
    </jitCompilationStart >  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir destina-se a ser usado com o arquivo de configuração anterior.  
  
```csharp
using System;  
using System.Reflection;  
using System.Runtime.CompilerServices;  
using System.Runtime.InteropServices;  
  
public class Entry  
{  
    public static void Main(string[] args)  
    {  
        C0.m();  
        C1.MyMethod();  
        C2.m();  
  
        ns0.C0.m();  
        ns0.C0.N0.m();  
        ns0.C1.m();  
  
        ns1.C0.m();  
        ns1.C0.N0.m();  
  
        ns2.C0.m();  
        ns2.C0.N0.m();  
    }  
}  
  
public class C0  
{  
    [MethodImpl(MethodImplOptions.NoInlining)]  
    public static void m() { }  
}  
  
public class C1  
{  
    [MethodImpl(MethodImplOptions.NoInlining)]  
    public static void MyMethod() { }  
}  
  
public class C2  
{  
    [MethodImpl(MethodImplOptions.NoInlining)]  
    public static void m() { }  
}  
  
namespace ns0  
{  
    public class C0  
    {  
        [MethodImpl(MethodImplOptions.NoInlining)]  
        public static void m() { }  
  
        public class N0  
        {  
            [MethodImpl(MethodImplOptions.NoInlining)]  
            public static void m() { }  
        }  
    }  
  
    public class C1  
    {  
        [MethodImpl(MethodImplOptions.NoInlining)]  
        public static void m() { }  
    }  
}  
  
namespace ns1  
{  
    public class C0  
    {  
        [MethodImpl(MethodImplOptions.NoInlining)]  
        public static void m() { }  
        public class N0  
        {  
            [MethodImpl(MethodImplOptions.NoInlining)]  
            public static void m() { }  
        }  
    }  
}  
  
namespace ns2  
{  
    public class C0  
    {  
        [MethodImpl(MethodImplOptions.NoInlining)]  
        public static void m() { }  
  
        public class N0  
        {  
            [MethodImpl(MethodImplOptions.NoInlining)]  
            public static void m() { }  
        }  
    }  
}  
```  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- [Diagnosticando erros com Assistentes de Depuração Gerenciados](../../../docs/framework/debug-trace-profile/diagnosing-errors-with-managed-debugging-assistants.md)
- [Marshaling de interoperabilidade](../../../docs/framework/interop/interop-marshaling.md)
