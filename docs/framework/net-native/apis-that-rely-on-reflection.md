---
title: APIs que dependem de reflexão
ms.date: 03/30/2017
ms.assetid: f9532629-6594-4a41-909f-d083f30a42f3
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: b86775f78b02b09dd8fb7925a13625783520bce1
ms.sourcegitcommit: 7e129d879ddb42a8b4334eee35727afe3d437952
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66052662"
---
# <a name="apis-that-rely-on-reflection"></a>APIs que dependem de reflexão
Em alguns casos, o uso da reflexão em código não é óbvio e, portanto, a cadeia de ferramentas .NET Native não preserva os metadados necessários no tempo de execução. Este tópico abrange algumas APIs ou padrões de programação comuns que não são consideradas como parte da API de reflexão, mas que dependem de reflexão para serem executados com êxito. Se você usá-los no código-fonte, poderá adicionar informações sobre eles no arquivo de diretivas de tempo de execução (.rd.xml), de modo que as chamadas a essas APIs não gerem uma exceção [MissingMetadataException](../../../docs/framework/net-native/missingmetadataexception-class-net-native.md) ou outras exceções em tempo de execução.  
  
## <a name="typemakegenerictype-method"></a>Método Type.MakeGenericType  
 Você pode instanciar dinamicamente um tipo genérico `AppClass<T>` chamando o método <xref:System.Type.MakeGenericType%2A?displayProperty=nameWithType> usando um código da seguinte forma:  
  
 [!code-csharp[ProjectN#1](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn/cs/type_makegenerictype1.cs#1)]  
  
 Para que esse código seja bem-sucedido no tempo de execução, vários itens de metadados são necessários. O primeiro é metadados `Browse` para o tipo genérico não instanciado, `AppClass<T>`:  
  
```xml  
<Type Name="App1.AppClass`1" Browse="Required PublicAndInternal" />  
```  
  
 Isso permite que a chamada de método <xref:System.Type.GetType%28System.String%2CSystem.Boolean%29?displayProperty=nameWithType> seja bem-sucedida e retorne um objeto <xref:System.Type> válido.  
  
 Mesmo quando você adiciona metadados ao tipo genérico não instanciado, uma chamada ao método <xref:System.Type.MakeGenericType%2A?displayProperty=nameWithType> gera uma exceção [MissingMetadataException](../../../docs/framework/net-native/missingmetadataexception-class-net-native.md):  
  
```  
This operation cannot be carried out as metadata for the following type was removed for performance reasons:  
  
App1.AppClass`1<System.Int32>.  
```  
  
 Você pode adicionar a seguinte diretiva de tempo de execução ao arquivo de diretivas de tempo de execução para adicionar metadados `Activate` à instanciação específica sobre `AppClass<T>` de <xref:System.Int32?displayProperty=nameWithType>:  
  
```xml  
<TypeInstantiation Name="App1.AppClass" Arguments="System.Int32"   
                   Activate="Required Public" />  
```  
  
 Cada instanciação diferente em `AppClass<T>` exigirá uma diretiva separada se estiver sendo criada com o método <xref:System.Type.MakeGenericType%2A?displayProperty=nameWithType> e não for usada estaticamente.  
  
## <a name="methodinfomakegenericmethod-method"></a>Método MethodInfo.MakeGenericMethod  
 Dada a classe `Class1` com um método genérico `GetMethod<T>(T t)`, `GetMethod` pode ser invocado por meio de reflexão usando um código da seguinte forma:  
  
 [!code-csharp[ProjectN#2](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn/cs/makegenericmethod1.cs#2)]  
  
 Para ser executado com êxito, esse código requer vários itens de metadados:  
  
- Metadados `Browse` para o tipo cujo método você deseja chamar.  
  
- Metadados `Browse` para o método que você deseja chamar.  Se for um método público, adicionar metadados `Browse` públicos ao tipo recipiente também inclui o método.  
  
- Metadados dinâmicos para o método que você deseja chamar, para que o delegado de invocação de reflexão não é removido pela cadeia de ferramentas .NET Native. Se os metadados dinâmicos não estiverem presentes para o método, a exceção a seguir é acionada quando o método <xref:System.Reflection.MethodInfo.MakeGenericMethod%2A?displayProperty=nameWithType> é chamado:  
  
    ```  
    MakeGenericMethod() cannot create this generic method instantiation because the instantiation was not metadata-enabled: 'App1.Class1.GenMethod<Int32>(Int32)'.  
    ```  
  
 As seguintes diretivas de tempo de execução garantem que todos os metadados estão disponíveis:  
  
```xml  
<Type Name="App1.Class1" Browse="Required PublicAndInternal">  
   <MethodInstantiation Name="GenMethod" Arguments="System.Int32" Dynamic="Required"/>  
</Type>  
```  
  
 A diretiva `MethodInstantiation` é necessária para cada instanciação diferente do método é invocada dinamicamente, e o elemento `Arguments` é atualizado para refletir cada argumento de instanciação diferente.  
  
## <a name="arraycreateinstance-and-typemaketypearray-methods"></a>Métodos Array.CreateInstance e Type.MakeTypeArray  
 A exemplo a seguir chama os métodos <xref:System.Type.MakeArrayType%2A?displayProperty=nameWithType> e <xref:System.Array.CreateInstance%2A?displayProperty=nameWithType> em um tipo `Class1`.  
  
 [!code-csharp[ProjectN#3](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn/cs/array1.cs#3)]  
  
 Se não houver nenhum metadado matriz, é apresentado o seguinte erro:  
  
```  
This operation cannot be carried out as metadata for the following type was removed for performance reasons:  
  
App1.Class1[]  
  
Unfortunately, no further information is available.  
```  
  
 Metadados `Browse` para o tipo de matriz são necessários para instanciá-lo dinamicamente.  A diretiva de tempo de execução a seguir permite a instanciação dinâmica do `Class1[]`.  
  
```xml  
<Type Name="App1.Class1[]" Browse="Required Public" />  
```  
  
## <a name="see-also"></a>Consulte também

- [Introdução](../../../docs/framework/net-native/getting-started-with-net-native.md)
- [Referência do arquivo de configuração das diretivas de tempo de execução (rd.xml)](../../../docs/framework/net-native/runtime-directives-rd-xml-configuration-file-reference.md)
