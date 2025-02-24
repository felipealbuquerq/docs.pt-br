---
title: Reflexão no .NET
ms.date: 03/30/2017
helpviewer_keywords:
- assemblies [.NET], reflection
- EventInfo class, reflection
- common language runtime, reflection
- FieldInfo class, reflection
- ParameterInfo class, reflection
- ConstructorInfo class, reflection
- Assembly class, reflection
- value types, reflection
- reflection, about reflection
- modules, reflection
- runtime, reflection
- Module class, reflection
- MethodInfo class, reflection
- PropertyInfo class, reflection
- type browsers
- reflection
- discovering type information at run time
- type system, reflection
ms.assetid: d1a58e7f-fb39-4d50-bf84-e3b8f9bf9775
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 56c3b536954118b80db2ae7f2f0d0ffdc74bac68
ms.sourcegitcommit: 77e33b682db39955e331b8e8eda4ef1925a24e78
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2019
ms.locfileid: "70133751"
---
# <a name="reflection-in-net"></a>Reflexão no .NET
As classes no namespace <xref:System.Reflection>, juntamente com <xref:System.Type?displayProperty=nameWithType> permitem que você obtenha informações sobre os [assemblies](../app-domains/assemblies-in-the-common-language-runtime.md) carregados e os tipos definidos neles, como [classes](../../standard/base-types/common-type-system.md#classes), [interfaces](../../standard/base-types/common-type-system.md#interfaces) e [tipos de valor](../../csharp/language-reference/keywords/value-types.md). Você também pode usar a reflexão para criar instâncias de tipo em tempo de execução e para invocá-los e acessá-los. Para tópicos sobre aspectos específicos da reflexão, consulte [Tópicos relacionados](#related_topics) no final dessa visão geral.
  
 O carregador [Common Language Runtime](../../standard/clr.md) gerencia os [domínios do aplicativo](../../../docs/framework/app-domains/application-domains.md), que constituem limites definidos em torno de objetos que têm o mesmo escopo de aplicativo. Esse gerenciamento inclui carregar cada assembly no domínio do aplicativo apropriado e controlar o layout da memória da hierarquia de tipo em cada assembly.  
  
 Os [assemblies](../../../docs/framework/app-domains/assemblies-in-the-common-language-runtime.md) contêm módulos, os módulos contêm tipos e tipos contêm membros. A reflexão fornece objetos que encapsulam assemblies, módulos e tipos. É possível usar a reflexão para criar dinamicamente uma instância de um tipo, associar o tipo a um objeto existente ou obter o tipo de um objeto existente. Você pode então invocar os métodos do tipo ou acessar suas propriedades e campos. Usos típicos da reflexão incluem o seguinte:  
  
- Use <xref:System.Reflection.Assembly> para definir e carregar assemblies, carregar módulos que estão listados no manifesto do assembly e localizar um tipo nesse assembly e criar uma instância dela.  
  
- Use <xref:System.Reflection.Module> para descobrir informações como o assembly que contém o módulo e as classes no módulo. Você também pode obter todos os métodos globais ou outros métodos específicos e não globais definidos no módulo.  
  
- Use <xref:System.Reflection.ConstructorInfo> para descobrir informações como o nome, os parâmetros, os modificadores de acesso (como `public` ou `private`) e os detalhes de implementação (como `abstract` ou `virtual`) de um construtor. Use o método <xref:System.Type.GetConstructors%2A> ou o <xref:System.Type.GetConstructor%2A> de um <xref:System.Type> para invocar um construtor específico.  
  
- Use <xref:System.Reflection.MethodInfo> para descobrir informações como o nome, o tipo de retorno, os parâmetros, os modificadores de acesso (como `public` ou `private`) e os detalhes de implementação (como `abstract` ou `virtual`) de um método. Use o método <xref:System.Type.GetMethods%2A> ou o <xref:System.Type.GetMethod%2A> de um <xref:System.Type> para invocar um método específico.  
  
- Use <xref:System.Reflection.FieldInfo> para descobrir informações como o nome, os modificadores de acesso (como `public` ou `private`) e detalhes de implementação (como `static`) de um campo e para obter ou definir os valores de campo.  
  
- Use <xref:System.Reflection.EventInfo> para descobrir informações como o nome, o tipo de dados do manipulador de eventos, os atributos personalizados, o tipo de declaração e o tipo refletido de um evento e para adicionar ou remover manipuladores de evento.  
  
- Use <xref:System.Reflection.PropertyInfo> para descobrir informações como o nome, o tipo de dados, o tipo de declaração, o tipo refletido e o status somente leitura ou gravável de uma propriedade e para obter ou definir os valores da propriedade.  
  
- Use <xref:System.Reflection.ParameterInfo> para descobrir informações como o nome do parâmetro, o tipo de dados, se um parâmetro é um parâmetro de entrada ou de saída e a posição do parâmetro em uma assinatura de método.  
  
- Use <xref:System.Reflection.CustomAttributeData> para descobrir informações sobre atributos personalizados ao trabalhar no contexto de somente reflexão de um domínio do aplicativo. <xref:System.Reflection.CustomAttributeData> permite que você examine atributos sem criar instâncias deles.  
  
 As classes do namespace <xref:System.Reflection.Emit> fornecem uma forma especializada de reflexão que permite criar tipos em tempo de execução.  
  
 A reflexão também pode ser usada para criar aplicativos chamados navegadores de tipo, que permitem aos usuários selecionar tipos e, em seguida, exibir as informações sobre esses tipos.  
  
 Há outros usos para reflexão. Os compiladores de linguagens como o JScript usam reflexão para criar tabelas de símbolos. As classes no namespace <xref:System.Runtime.Serialization> usam reflexão para acessar dados e para determinar quais campos persistir. As classes do namespace <xref:System.Runtime.Remoting> usa a reflexão indiretamente por meio da serialização.  
  
## <a name="runtime-types-in-reflection"></a>Tipos do tempo de execução na reflexão  
 A reflexão fornece classes, como <xref:System.Type> e <xref:System.Reflection.MethodInfo>, para representar tipos, membros, parâmetros e outras entidades de código. No entanto, ao usar a reflexão, você não trabalha diretamente com essas classes, a maioria das quais é abstrata (`MustInherit` no Visual Basic). Em vez disso, você trabalha com tipos fornecidos pelo CLR (Common Language Runtime).  
  
 Por exemplo, quando você usa o operador `typeof` C# (`GetType` no Visual Basic) para obter um objeto <xref:System.Type>, o objeto é na verdade um `RuntimeType`. `RuntimeType` deriva de <xref:System.Type> e fornece implementações de todos os métodos abstratos.  
  
 Essas classes de tempo de execução são `internal` (`Friend` no Visual Basic). Elas não são documentadas separadamente de suas classes base porque seu comportamento é descrito na documentação da classe base.  
  
<a name="related_topics"></a>   
## <a name="related-topics"></a>Tópicos relacionados  
  
|Título|DESCRIÇÃO|  
|-----------|-----------------|  
|[Exibindo informações de tipo](../../../docs/framework/reflection-and-codedom/viewing-type-information.md)|Descreve a classe <xref:System.Type> e fornece exemplos de código que ilustram como usar <xref:System.Type> com várias classes de reflexão para obter informações sobre construtores, métodos, campos, propriedades e eventos.|  
|[Reflexão e tipos genéricos](../../../docs/framework/reflection-and-codedom/reflection-and-generic-types.md)|Explica como a reflexão trata os parâmetros de tipo e argumentos de tipo de tipos genéricos e métodos genéricos.|  
|[Security Considerations for Reflection](../../../docs/framework/reflection-and-codedom/security-considerations-for-reflection.md) (Considerações sobre segurança relacionadas à reflexão)|Descreve as regras que determinam até que nível a reflexão pode ser usada para descobrir informações de tipo e tipos de acesso.|  
|[Dynamically Loading and Using Types](../../../docs/framework/reflection-and-codedom/dynamically-loading-and-using-types.md) (Carregando e usando tipos dinamicamente)|Descreve a interface de associação personalizada de reflexão que dá suporte à associação tardia.|  
|[Como: Carregar assemblies no contexto somente de reflexão](../../../docs/framework/reflection-and-codedom/how-to-load-assemblies-into-the-reflection-only-context.md)|Descreve o contexto de carregamento de somente reflexão. Mostra como carregar um assembly, como testar o contexto e como examinar os atributos aplicados a um assembly no contexto de somente reflexão.|  
|[Acessando atributos personalizados](../../../docs/framework/reflection-and-codedom/accessing-custom-attributes.md)|Demonstra o uso de reflexão para consultar a existência do atributo e os valores.|  
|[Especificando nomes de tipo totalmente qualificados](../../../docs/framework/reflection-and-codedom/specifying-fully-qualified-type-names.md)|Descreve o formato dos nomes de tipo totalmente qualificados nos termos da BNF (forma de Backus-Naur) e a sintaxe necessária para especificar os caracteres especiais, os nomes de assembly, os ponteiros, as referências e as matrizes.|  
|[Como: Conectar um delegado usando a reflexão](../../../docs/framework/reflection-and-codedom/how-to-hook-up-a-delegate-using-reflection.md)|Explica como criar um delegado para um método e conectar o delegado a um evento. Explica como criar um método de manipulação de eventos em tempo de execução usando <xref:System.Reflection.Emit.DynamicMethod>.|  
|[Emissão de métodos e assemblies dinâmicos](../../../docs/framework/reflection-and-codedom/emitting-dynamic-methods-and-assemblies.md)|Explica como gerar métodos dinâmicos e assemblies dinâmicos.|  
  
## <a name="reference"></a>Referência  
 <xref:System.Type?displayProperty=nameWithType>  
  
 <xref:System.Reflection>  
  
 <xref:System.Reflection.Emit>  
