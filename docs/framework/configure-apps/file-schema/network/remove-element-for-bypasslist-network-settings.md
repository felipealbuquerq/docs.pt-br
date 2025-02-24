---
title: Elemento <remove> para bypasslist (Configurações de Rede)
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.net/defaultProxy/bypasslist/remove
- http://schemas.microsoft.com/.NetConfiguration/v2.0#remove
helpviewer_keywords:
- <bypasslist>, remove element
- remove element, bypasslist
- bypasslist, remove element
- remove element, bypasslist
ms.assetid: 61dcfb4a-e3d9-4abf-a2cd-7d685fe2f64b
ms.openlocfilehash: 99c18bd5b779845d52831b4a9591eaf4d5e5530b
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69920956"
---
# <a name="remove-element-for-bypasslist-network-settings"></a>\<remover o elemento > para BypassList (configurações de rede)

Remove um endereço IP ou nome DNS da lista de bypass de proxy.

\<> de configuração \
\<system.net>\
\<defaultProxy>\
\<bypasslist>\
\<remove>

## <a name="syntax"></a>Sintaxe

```xml
<remove
  address="regular expression"
/>
```

## <a name="attributes-and-elements"></a>Atributos e elementos

As seções a seguir descrevem atributos, elementos filho e elementos pai.

### <a name="attributes"></a>Atributos

|**Atributo**|**Descrição**|
|-------------------|---------------------|
|`address`|Uma expressão regular que descreve um endereço IP ou nome DNS.|

### <a name="child-elements"></a>Elementos filho

nenhuma.

### <a name="parent-elements"></a>Elementos pai

|**Elemento**|**Descrição**|
|-----------------|---------------------|
|[bypasslist](bypasslist-element-network-settings.md)|Fornece um conjunto de expressões regulares que descrevem endereços que não usam um proxy.|

## <a name="remarks"></a>Comentários

O `remove` elemento remove expressões regulares que descrevem endereços IP ou nomes de servidores DNS da lista de endereços que ignoram um servidor proxy. Os endereços foram definidos anteriormente no arquivo de configuração ou em um nível superior na hierarquia de configuração.

O valor do `address` atributo deve ser uma expressão regular que descreve um conjunto de endereços IP ou nomes de host.

Para obter mais informações sobre expressões regulares, consulte. [.NET Framework expressões regulares](../../../../standard/base-types/regular-expressions.md).

## <a name="configuration-files"></a>Arquivos de Configuração

Esse elemento pode ser usado no arquivo de configuração do aplicativo ou no arquivo de configuração do computador (Machine. config).

## <a name="example"></a>Exemplo

O exemplo a seguir remove qualquer definição anterior para o domínio adventure-works.com e, em seguida, adiciona o domínio contoso.com à lista de bypass.

```xml
<configuration>
  <system.net>
    <defaultProxy>
      <bypasslist>
        <remove address="[a-z]+\.adventure-works\.com$" />
        <add address="[a-z]+\.contoso\.com$" />
      </bypasslist>
    </defaultProxy>
  </system.net>
</configuration>
```

## <a name="see-also"></a>Consulte também

- <xref:System.Net.WebProxy?displayProperty=nameWithType>
- [Esquema de configurações de rede](index.md)
