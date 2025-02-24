---
title: Programação de rede no .NET Framework
ms.date: 03/30/2017
helpviewer_keywords:
- Networking
- Internet
- Internet, .NET Framework Internet services
- Network Resources
ms.assetid: 8d455610-67a0-4fa8-a62f-7747064a9256
ms.openlocfilehash: 245c843d13e70a807807f4e462dd1a5357c25665
ms.sourcegitcommit: 093571de904fc7979e85ef3c048547d0accb1d8a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70398528"
---
# <a name="network-programming-in-the-net-framework"></a>Programação de rede no .NET Framework
O Microsoft .NET Framework fornece uma implementação dos serviços de Internet em camadas, extensível e gerenciada que pode ser rápida e facilmente integrada aos aplicativos. Os aplicativos de rede podem compilar em protocolos conectáveis para usufruir automaticamente de novos protocolos da Internet ou podem usar uma implementação gerenciada da interface de soquete do Windows para trabalhar com a rede a nível de soquete.  
  
## <a name="in-this-section"></a>Nesta seção  

 [Apresentando protocolos conectáveis](../../../docs/framework/network-programming/introducing-pluggable-protocols.md)  
 Descreve como acessar um recurso da Internet sem considerar o protocolo de acesso necessário.  
  
 [Solicitando dados](../../../docs/framework/network-programming/requesting-data.md)  
 Explica como usar protocolos conectáveis para carregar e baixar dados dos recursos da Internet.  
  
 [Programando protocolos conectáveis](../../../docs/framework/network-programming/programming-pluggable-protocols.md)  
 Explica como derivar classes específicas de protocolo para implementar protocolos conectáveis.  
  
 [Usando protocolos de aplicativo](../../../docs/framework/network-programming/using-application-protocols.md)  
 Descreve aplicativos de programação que se beneficiam de protocolos de rede como o TCP, UDP e HTTP.  
  
 [Protocolo da Internet Versão 6](../../../docs/framework/network-programming/internet-protocol-version-6.md)  
 Descreve as vantagens do Protocolo de Internet versão 6 (IPv6) sob a versão atual do pacote do Protocolo de Internet (IPv4), descreve o endereçamento do IPv6, o roteamento e a configuração automática e também como habilitar e desabilitar o IPv6.  
  
 [Configurando aplicativos da Internet](../../../docs/framework/network-programming/configuring-internet-applications.md)  
 Explica como usar os arquivos de configuração do .NET Framework para configurar aplicativos da Internet.  
  
 [Rastreamento de rede no .NET Framework](../../../docs/framework/network-programming/network-tracing.md)  
 Explica como usar o rastreamento de rede para obter informações sobre invocações de método e tráfego de rede geradas por um aplicativo gerenciado.  
  
 [Gerenciamento de cache para aplicativos de rede](../../../docs/framework/network-programming/cache-management-for-network-applications.md)  
 Descreve como usar o cachê para aplicativos que usam as classes <xref:System.Net.WebClient?displayProperty=nameWithType>, <xref:System.Net.WebRequest?displayProperty=nameWithType> e <xref:System.Net.HttpWebRequest?displayProperty=nameWithType>.  
  
 [Segurança na programação de rede](../../../docs/framework/network-programming/security-in-network-programming.md)  
 Descreve como usar técnicas padrão de segurança e autenticação da Internet.  
  
 [Melhores práticas para classes System.Net](../../../docs/framework/network-programming/best-practices-for-system-net-classes.md)  
 Fornece dicas e truques para obter o máximo dos aplicativos da Internet.  
  
 [Acessando a Internet por meio de um proxy](../../../docs/framework/network-programming/accessing-the-internet-through-a-proxy.md)  
 Descreve como configurar proxies.  
  
 [NetworkInformation](../../../docs/framework/network-programming/networkinformation.md)  
 Descreve como coletar informações sobre eventos, alterações, estatísticas e propriedades de rede e também explica como determinar se um host remoto é acessível usando a classe <xref:System.Net.NetworkInformation.Ping?displayProperty=nameWithType>.  
  
 [Alterações no namespace System.Uri na versão 2.0](../../../docs/framework/network-programming/changes-to-the-system-uri-namespace-in-version-2-0.md)  
 Descreve várias alterações feitas na classe <xref:System.Uri?displayProperty=nameWithType> na versão 2.0 para comportamento fixo incorreto; aprimora a usabilidade e a segurança.  
  
 [Suporte ao International Resource Identifier em System.Uri](../../../docs/framework/network-programming/international-resource-identifier-support-in-system-uri.md)  
 Descreve aprimoramentos na classe <xref:System.Uri?displayProperty=nameWithType> na versão 3.5, 3.0 SP1 e 2.0 SP1 para o Identificador Internacional de Recursos (IRI) e o suporte de Nome de Domínio Internacionalizado (IDN).  
  
 [Melhorias do desempenho de soquete na versão 3.5](../../../docs/framework/network-programming/socket-performance-enhancements-in-version-3-5.md)  
 Descreve um conjunto de aprimoramentos na classe <xref:System.Net.Sockets.Socket?displayProperty=nameWithType> na versão 3.5, 3.0 SP1 e 2.0 SP1, que fornece um padrão assíncrono alternativo, o qual pode ser usado por aplicativos de alto desempenho especializados em soquete.  
  
 [Protocolo PNRP](../../../docs/framework/network-programming/peer-name-resolution-protocol.md)  
 Descreve o suporte adicionado à versão 3.5 para oferecer suporte ao protocolo PNRP, um protocolo de resolução de nome e um registro de nome dinâmico sem servidor. Esses novos recursos têm suporte pelo namespace <xref:System.Net.PeerToPeer?displayProperty=nameWithType>.  
  
 [Colaboração ponto a ponto](../../../docs/framework/network-programming/peer-to-peer-collaboration.md)  
 Descreve o suporte adicionado à versão 3.5 para dar suporte à Colaboração Ponto a Ponto que compila no PNRP. Esses novos recursos têm suporte pelo namespace <xref:System.Net.PeerToPeer.Collaboration?displayProperty=nameWithType>.  
  
 [Alterações na autenticação NTLM para HttpWebRequest na versão 3.5 SP1](../../../docs/framework/network-programming/changes-to-ntlm-authentication-for-httpwebrequest-in-version-3-5-sp1.md)  
 Descreve as alterações de segurança feitas na versão 3.5 SP1 que afetam a maneira como a autenticação integrada do Windows é controlada por <xref:System.Net.HttpWebRequest?displayProperty=nameWithType>, <xref:System.Net.HttpListener?displayProperty=nameWithType>, <xref:System.Net.Security.NegotiateStream?displayProperty=nameWithType> e por classes relacionadas no namespace do System.Net.  
  
 [Autenticação Integrada do Windows com proteção estendida](../../../docs/framework/network-programming/integrated-windows-authentication-with-extended-protection.md)  
 Descreve aprimoramentos para a proteção estendida que afetam a maneira como a autenticação integrada do Windows é controlada por <xref:System.Net.HttpWebRequest?displayProperty=nameWithType>, <xref:System.Net.HttpListener?displayProperty=nameWithType>, <xref:System.Net.Mail.SmtpClient?displayProperty=nameWithType>, <xref:System.Net.Security.SslStream?displayProperty=nameWithType>, <xref:System.Net.Security.NegotiateStream?displayProperty=nameWithType> e por classes relacionadas no <xref:System.Net?displayProperty=nameWithType> e por namespaces relacionados.  
  
 [Passagem de NAT usando IPv6 e Teredo](../../../docs/framework/network-programming/nat-traversal-using-ipv6-and-teredo.md)  
 Descreve os aprimoramentos adicionados ao <xref:System.Net?displayProperty=nameWithType>, <xref:System.Net.NetworkInformation?displayProperty=nameWithType> e namespaces <xref:System.Net.Sockets?displayProperty=nameWithType> para oferecer suporte à NAT transversal usando o IPv6 e o Teredo.  
  
 [Isolamento de rede para Aplicativos da Windows Store](../../../docs/framework/network-programming/network-isolation-for-windows-store-apps.md)  
 Descreve o impacto do isolamento de rede quando as classes nos namespaces <xref:System.Net>, <xref:System.Net.Http> e <xref:System.Net.Http.Headers> são usadas em aplicativos [!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)].  
  
 [Amostras de programação de rede](../../../docs/framework/network-programming/network-programming-samples.md)  
 Vincula-se a amostras para download de programação de rede que usam classes nos namespaces <xref:System.Net>, <xref:System.Net.Cache>, <xref:System.Net.Configuration>, <xref:System.Net.Mail>, <xref:System.Net.Mime>, <xref:System.Net.NetworkInformation>, <xref:System.Net.PeerToPeer>, <xref:System.Net.Security>, <xref:System.Net.Sockets>.  
  
## <a name="reference"></a>Referência  
 <xref:System.Net?displayProperty=nameWithType>  
 Fornece uma interface de programação simples para muitos dos protocolos usados nas redes de hoje. As classes <xref:System.Net.WebRequest?displayProperty=nameWithType> e <xref:System.Net.WebResponse?displayProperty=nameWithType> nesse namespace são a base para protocolos conectáveis.  
  
 <xref:System.Net.Cache?displayProperty=nameWithType>  
 Define tipos e enumerações usados para definir políticas de cache para os recursos obtidos usando as classes <xref:System.Net.WebRequest?displayProperty=nameWithType> e <xref:System.Net.HttpWebRequest?displayProperty=nameWithType>.  
  
 <xref:System.Net.Configuration?displayProperty=nameWithType>  
 Classes que os aplicativos usam para acessar e atualizar programaticamente definições de configuração dos namespaces do System.Net.  
  
 <xref:System.Net.Http?displayProperty=nameWithType>  
 Classes que fornecem uma interface de programação para aplicativos HTTP modernos.  
  
 <xref:System.Net.Http.Headers?displayProperty=nameWithType>  
 Fornece suporte para coleções de cabeçalhos HTTP usados pelo namespace <xref:System.Net.Http?displayProperty=nameWithType>  
  
 <xref:System.Net.Mail?displayProperty=nameWithType>  
 Classes para compor e enviar email usando o protocolo SMTP.  
  
 <xref:System.Net.Mime?displayProperty=nameWithType>  
 Define os tipos usados para representar os cabeçalhos MIME (Multipurpose Internet Mail Exchange) usado por classes no namespace <xref:System.Net.Mail?displayProperty=nameWithType>.  
  
 <xref:System.Net.NetworkInformation?displayProperty=nameWithType>  
 Classes para coletar programaticamente informações sobre eventos, alterações, estatísticas e propriedades de rede.  
  
 <xref:System.Net.PeerToPeer?displayProperty=nameWithType>  
 Fornece uma implementação gerenciada do protocolo PNRP para desenvolvedores.  
  
 <xref:System.Net.PeerToPeer.Collaboration?displayProperty=nameWithType>  
 Fornece uma implementação gerenciada da interface de Colaboração Ponto a Ponto para desenvolvedores.  
  
 <xref:System.Net.Security?displayProperty=nameWithType>  
 Classes para fornecer fluxos de rede para proteger comunicações entre hosts.  
  
 <xref:System.Net.Sockets?displayProperty=nameWithType>  
 Fornece uma implementação gerenciada da interface dos Soquetes do Windows (Winsock) para desenvolvedores que precisam ajudar a controlar o acesso à rede.  
  
 <xref:System.Net.WebSockets?displayProperty=nameWithType>  
 Fornece uma implementação gerenciada da interface de WebSocket para desenvolvedores.  
  
 <xref:System.Uri?displayProperty=nameWithType>  
 Fornece uma representação de objeto de um URI (Uniform Resource Identifier) e fácil acesso às partes do URI.  
  
 <xref:System.Security.Authentication.ExtendedProtection?displayProperty=nameWithType>  
 Fornece suporte à autenticação usando proteção estendida para aplicativos.  
  
 <xref:System.Security.Authentication.ExtendedProtection.Configuration?displayProperty=nameWithType>  
 Fornece suporte à configuração da autenticação usando proteção estendida para aplicativos.  
  
## <a name="see-also"></a>Consulte também

- [Práticas recomendadas do protocolo TLS com o .NET Framework](../../../docs/framework/network-programming/tls.md)
- [Tópicos de instruções de programação de rede](../../../docs/framework/network-programming/network-programming-how-to-topics.md)
- [Amostras de programação de rede](../../../docs/framework/network-programming/network-programming-samples.md)
- [Amostra de HttpClient](https://code.msdn.microsoft.com/windowsapps/HttpClient-sample-55700664)
