---
ms.openlocfilehash: a58ea1a16824cf4a1407221dfb0123efa79e48f9
ms.sourcegitcommit: 4d8efe00f2e5ab42e598aff298d13b8c052d9593
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68235672"
---
### <a name="aspnet-accessibility-improvements-in-net-framework-471"></a>Melhorias de acessibilidade do ASP.NET no .NET Framework 4.7.1

|   |   |
|---|---|
|Detalhes|A partir do .NET Framework 4.7.1, o ASP.NET melhorou o funcionamento dos controles da Web ASP.NET com a tecnologia de acessibilidade no Visual Studio para dar melhor suporte a clientes do ASP.NET.  Isso inclui as seguintes alterações:<ul><li>Alterações para implementar padrões de acessibilidade de interface do usuário ausentes nos controles, como a caixa de diálogo Adicionar Campo no assistente de Exibição de Detalhes ou a caixa de diálogo Configurar ListView do assistente ListView.</li><li>Alterações para melhorar a exibição no modo de Alto Contraste, como o Editor de Campos do Pager de Dados.</li><li>Alterações para melhorar as experiências de navegação por teclado, como a caixa de diálogo Campos no assistente Editar Campos do Pager do controle DataPager, a caixa de diálogo Configurar ObjectContext ou a caixa de diálogo Configurar Seleção de Dados do assistente Configurar Fonte de Dados.</li></ul>|
|Sugestão|<strong>Como aceitar ou recusar essas alterações</strong>Para se beneficiar dessas alterações, o Designer do Visual Studio deverá ser executado no .NET Framework 4.7.1 ou posterior. O aplicativo Web pode se beneficiar dessas alterações de uma das seguintes maneiras:<ul><li>Instalar o Visual Studio 2017 15.3 ou posterior, que dá suporte a novas funcionalidades de acessibilidade com a seguinte opção de AppContext por padrão.</li><li>Recusar os comportamentos de acessibilidade herdados adicionando a opção de AppContext <code>Switch.UseLegacyAccessibilityFeatures</code> à seção <code>&lt;runtime&gt;</code> do arquivo devenv.exe.config e configurando-a como <code>false</code>, como mostra o exemplo a seguir.</li></ul><pre><code class="lang-xml">&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;&#13;&#10;&lt;configuration&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;...&#13;&#10;&lt;!-- AppContextSwitchOverrides value attribute is in the form of &#39;key1=true/false;key2=true/false&#39;  --&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.UseLegacyAccessibilityFeatures=false&quot; /&gt;&#13;&#10;...&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>Aplicativos destinados ao .NET Framework 4.7.1 ou posterior que desejam preservar o comportamento de acessibilidade herdado pode aceitar o uso das funcionalidades de acessibilidade herdadas definindo explicitamente essa opção de AppContext como <code>true</code>.|
|Escopo|Secundário|
|Versão|4.7.1|
|Tipo|Redirecionando|
