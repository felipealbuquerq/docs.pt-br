---
title: Elemento <add> para webRequestModules (Configurações de Rede)
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.net/webRequestModules/add
- http://schemas.microsoft.com/.NetConfiguration/v2.0#add
helpviewer_keywords:
- <webRequestModules>, add element
- webRequestModules, add element
- add element, webRequestModules
- <add> element, webRequestModules
ms.assetid: 47ec4adc-f39f-4bcd-8680-1ec21fd26890
ms.openlocfilehash: f99c5b0dc7eab57d4e3e86f49dbbb3228c7b7d8b
ms.sourcegitcommit: cdf67135a98a5a51913dacddb58e004a3c867802
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69664213"
---
# <a name="add-element-for-webrequestmodules-network-settings"></a>\<Adicionar > elemento para webRequestModules (configurações de rede)
Adiciona um módulo de solicitação da Web personalizado ao aplicativo.  
  
 \<configuration>  
\<system.net>  
\<webRequestModules>  
\<add>  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<add   
  prefix="URI prefix"   
  type="type_fullname, assembly_fullname"   
/>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|**Atributo**|**Descrição**|  
|-------------------|---------------------|  
|`prefix`|O prefixo de URI para solicitações tratadas por este módulo de solicitação da Web.|  
|`type`|O nome do tipo totalmente qualificado (indicado pela <xref:System.Type.FullName%2A> Propriedade) e o nome do assembly (indicado <xref:System.Reflection.Assembly.FullName%2A> pela propriedade), separados por uma vírgula, que implementa esse módulo de solicitação da Web.|  
  
### <a name="child-elements"></a>Elementos filho  
 nenhuma.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|**Elemento**|**Descrição**|  
|-----------------|---------------------|  
|[webRequestModules](webrequestmodules-element-network-settings.md)|Especifica os módulos a serem usados para solicitar informações de hosts de rede.|  
  
## <a name="remarks"></a>Comentários  
 O `prefix` atributo define o prefixo de URI que usa o módulo de solicitação da Web especificado. Os módulos de solicitação da Web normalmente são registrados para lidar com um protocolo específico, como HTTP ou FTP, mas podem ser registrados para lidar com uma solicitação para um servidor ou caminho específico em um servidor.  
  
 O módulo de solicitação da Web é criado quando um prefixo de correspondência de URI <xref:System.Net.WebRequest.Create%2A?displayProperty=nameWithType> é passado para o método.  
  
 O valor `prefix` do atributo deve ser os caracteres à esquerda de um URI válido. Por exemplo `http` ou `http://www.contoso.com`.
  
 O valor do `type` atributo deve ser um nome de tipo válido e um nome de assembly correspondente, separados por uma vírgula.
  
## <a name="configuration-files"></a>Arquivos de Configuração  
 Esse elemento pode ser usado no arquivo de configuração do aplicativo ou no arquivo de configuração do computador (Machine. config).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir registra um módulo de solicitação da Web personalizado para HTTP. Você deve substituir os valores de Version e PublicKeyToken pelos valores corretos para o módulo especificado.  
  
```xml  
<configuration>  
  <system.net>  
    <webRequestModules>  
      <add prefix="http"  
           type="System.Net.HttpRequestCreator, System, Version=2.0.3600.0,  
           Culture=neutral, PublicKeyToken=b77a5c561934e089"  
      />  
    </webRequestModules>  
  </system.net>  
</configuration>  
```  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Net.WebRequest>
- [Esquema de configurações de rede](index.md)
