---
title: <filter>Elemento para <add> para<sharedListeners>
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.diagnostics/sharedListeners/add/filter
helpviewer_keywords:
- filter element for <add> for <sharedListeners>
- initializeData attribute
- <filter> element for <add> for <sharedListeners>
- filters, trace listeners
- trace listeners, filters
ms.assetid: 7d4e7faa-2e4e-4379-ac76-f6cd7f2f8fac
ms.openlocfilehash: 571a3add232f3e4f9747040dc104b85e8cc3085e
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69920506"
---
# <a name="filter-element-for-add-for-sharedlisteners"></a>\<filtrar > elemento para \<Adicionar > para \<o sharedListeners >
Adiciona um filtro a um ouvinte na coleção `sharedListeners`.  
  
 \<configuration>  
\<System. Diagnostics >  
\<Elemento de > de sharedListeners  
\<add>  
\<filter>  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<filter type="System.Diagnostics.EventTypeFilter"   
  initializeData="Warning" />  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|**type**|Atributo obrigatório.<br /><br /> Especifica o tipo do filtro. Você pode usar apenas o nome completo do tipo (no formato da <xref:System.Type.FullName%2A?displayProperty=nameWithType> Propriedade) ou pode usar o nome do tipo totalmente qualificado, incluindo as informações do assembly (no formato <xref:System.Type.AssemblyQualifiedName%2A?displayProperty=nameWithType> da propriedade). Para obter informações sobre como criar um nome de tipo totalmente qualificado, consulte [especificando nomes de tipo totalmente qualificados](../../../reflection-and-codedom/specifying-fully-qualified-type-names.md).|  
|**initializeData**|Atributo opcional.<br /><br /> A cadeia de caracteres passada para o construtor para a classe especificada.|  
  
### <a name="child-elements"></a>Elementos filho  
 nenhuma.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|`configuration`|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|  
|`system.diagnostics`|Especifica os ouvintes de rastreamento que coletam, armazenam e roteiam mensagens e o nível em que uma opção de rastreamento é definida.|  
|`sharedListeners`|Uma coleção de ouvintes que qualquer elemento de origem ou de rastreamento pode referenciar.|  
|`add`|Adiciona um ouvinte à coleção **sharedListeners** .|  
  
## <a name="remarks"></a>Comentários  
 Se um ouvinte for definido em `<add>` um elemento `<sharedListeners>` do elemento, o filtro desse ouvinte deverá ser definido em um `<filter>` elemento que seja filho do `<add>` elemento.  
  
 Esse elemento pode ser usado no arquivo de configuração da máquina (Machine. config) e no arquivo de configuração do aplicativo.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como usar o `<filter>` elemento para adicionar um filtro ao ouvinte `console` de rastreamento na `sharedListeners` coleção.  
  
```xml  
<configuration>  
  <system.diagnostics>  
    <sources>  
      <source name="myTraceSource" >  
        <listeners>  
          <add name="console" />  
          <remove name="Default" />  
        </listeners>  
      </source>  
    </sources>  
    <sharedListeners>  
      <add name="console"   
        type="System.Diagnostics.ConsoleTraceListener" >  
        <filter type="System.Diagnostics.EventTypeFilter"   
          initializeData="Error" />  
      </add>  
    </sharedListeners>  
  </system.diagnostics>  
</configuration>  
```  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Diagnostics.TraceFilter>
- <xref:System.Diagnostics.TraceListener>
- <xref:System.Diagnostics.TraceSource>
- [Esquema de configurações de rastreamento e depuração](index.md)
