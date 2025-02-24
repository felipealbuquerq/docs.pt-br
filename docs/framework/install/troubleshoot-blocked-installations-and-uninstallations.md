---
title: Solucionar problemas de instalações e desinstalações bloqueadas do .NET Framework
ms.date: 04/18/2019
ms.custom: updateeachrelease
helpviewer_keywords:
- .NET Framework, troubleshooting blocked installations
- blocked .NET Framework installations, troubleshooting
ms.assetid: c3fdfbc1-ed99-4202-a2b0-8c4f1646385d
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: d96ca26ab3733999810dd48611cf10a577381f40
ms.sourcegitcommit: 83ecdf731dc1920bca31f017b1556c917aafd7a0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67859522"
---
# <a name="troubleshoot-blocked-net-framework-installations-and-uninstallations"></a>Solucionar problemas de instalações e desinstalações bloqueadas do .NET Framework

Ao executar o [instalador da Web ou offline](../../../docs/framework/install/guide-for-developers.md) do .NET Framework 4.5 ou versões posteriores, talvez você encontre um problema que bloqueie ou impeça a instalação do .NET Framework. A tabela a seguir lista os possíveis problemas de bloqueio e fornece links para informações de solução de problemas.

No Windows 8 e posterior, o .NET Framework é um componente do sistema operacional e não pode ser desinstalado independentemente. As atualizações para o .NET Framework são exibidas na guia **Atualizações Instaladas** do aplicativo **Programas e Recursos** do Painel de Controle. Para sistemas operacionais nos quais o .NET Framework não vem pré-instalado, ele aparece na guia **Desinstalar ou alterar um programa** (ou na guia **Adicionar ou Remover Programas**) do aplicativo **Programas e Recursos** no Painel de Controle. Para saber mais sobre as versões do Windows nas quais o .NET Framework vem pré-instalado, confira [Requisitos do sistema](../../../docs/framework/get-started/system-requirements.md).

> [!IMPORTANT]
> Como as versões 4.x do .NET Framework são atualizações in-loco, não é possível instalar uma versão anterior do .NET Framework 4.x em um sistema que já tem uma versão mais recente instalada. Por exemplo, em um sistema com o Windows 10 Fall Creators Update, não é possível instalar o .NET Framework 4.6.2, pois o .NET Framework 4.7.1 já vem pré-instalado com o sistema operacional.

Você pode saber quais versões do .NET Framework estão instaladas em um sistema. Confira [Como Determinar quais versões do .NET Framework estão instaladas](../../../docs/framework/migration-guide/how-to-determine-which-versions-are-installed.md) para obter mais informações.

Nesta tabela, 4.5.x refere-se ao .NET Framework 4.5 e às suas versões de ponto, 4.5.1 e 4.5.2; 4.6.x refere-se ao .NET Framework 4.6 e às suas versões de ponto, 4.6.1 e 4.6.2, 4.7, e 4.7.x refere-se ao .NET Framework 4.7 e às suas versão de ponto, 4.7.1 e 4.7.2, e 4.8 se refere ao .NET Framework 4.8.

|Mensagem de bloqueio|Para obter mais informações ou para resolver o problema|  
|----------------------|--------------------------------------------------|  
|A desinstalação do Microsoft .NET Framework pode fazer com que alguns aplicativos deixem de funcionar.|Em geral, você não deve desinstalar quaisquer versões do .NET Framework que estão instaladas em seu computador, pois um aplicativo que você usa pode depender de uma versão específica do .NET Framework. Para saber mais, confira [O .NET Framework para usuários](../../../docs/framework/get-started/index.md#ForUsers) no guia de *Introdução*.|  
|O .NET Framework 4.5.x/4.6.x/4.7.x (ENU) ou uma versão posterior já está instalado neste computador.|Nenhuma ação é necessária.<br /><br /> Para determinar quais versões do .NET Framework estão instaladas em um sistema, confira [Como: Determinar quais versões do .NET Framework estão instaladas](../../../docs/framework/migration-guide/how-to-determine-which-versions-are-installed.md).|  
|O .NET Framework 4.5.x/4.6.x/4.7.x/4.8 (*linguagem*) requer o .NET Framework 4.5.x/4.6.x/4.7.x/4.8. Instale o .NET Framework 4.5.x/4.6.x/4.7.x/4.8 no Centro de Download e execute a Instalação novamente.|É necessário instalar a versão em inglês do .NET Framework especificado antes de instalar um pacote de idiomas. Para saber mais, veja a seção sobre [Como instalar pacotes de idiomas](../../../docs/framework/install/guide-for-developers.md#to-install-language-packs) no guia de instalação.|  
|Não é possível instalar o .NET Framework 4.5.x/4.6.x/4.7.x/4.8. Outros aplicativos do computador não são compatíveis com esse programa.<br /><br /> -ou-<br /><br /> Outros aplicativos do computador não são compatíveis com esse programa.|A causa mais provável desta mensagem é que uma versão de avaliação ou RC do .NET Framework foi instalada. Desinstale a versão de avaliação ou RC e execute a instalação novamente.|  
|O .NET Framework 4.5.x/4.6.x/4.7.x/4.8 não pode ser desinstalado usando este pacote. Para desinstalar o .NET Framework 4.5.x/4.6.x/4.7.x/4.8 do computador, acesse o **Painel de Controle**, escolha **Programas e Recursos**, escolha **Exibir atualizações instaladas**, selecione Atualização para o Microsoft Windows (KB2828152) e, em seguida, escolha **Desinstalar**.|O pacote que você está instalando não desinstala as versões de avaliação ou RC do .NET Framework.<br /><br /> Desinstale a versão de avaliação ou RC no Painel de Controle.|  
|Não é possível desinstalar o .NET Framework 4.5.x/4.6.x/4.7.x/4.8. Outros aplicativos do computador dependem desse programa.|Em geral, não é recomendável desinstalar quaisquer versões do .NET Framework que estão instaladas no computador, pois algum outro aplicativo pode depender de uma versão específica do .NET Framework. Para saber mais, confira [O .NET Framework para usuários](../../../docs/framework/get-started/index.md#ForUsers) no guia de *Introdução*.|  
|O .NET Framework 4.5.x/4.6.x/4.7.x/4.8 redistribuível não é aplicável a este sistema operacional. Baixe o NET Framework 4.5.x/4.6.x/4.7.x/4.8 para seu sistema operacional no Centro de Download da Microsoft.|Você pode estar tentando instalar o .NET Framework 4.5.1, 4.5.2, 4.6, 4.6.1, 4.6.2, 4.7, 4.7.1, 4.7.2 ou 4.8 em uma plataforma não compatível ou escolheu o pacote de instalação que não inclui componentes para todos os sistemas operacionais compatíveis. Execute a instalação novamente usando o instalador offline ([para 4.5.1](https://go.microsoft.com/fwlink/p/?LinkId=309493), [para 4.5.2](https://go.microsoft.com/fwlink/p/?LinkId=397706), [para 4.6](https://go.microsoft.com/fwlink/p/?LinkId=528233), [para 4.6.1](https://go.microsoft.com/fwlink/p/?LinkId=671744), para [4.6.2](https://go.microsoft.com/fwlink/p/?LinkId=780604), para [4.7](https://go.microsoft.com/fwlink/p/?LinkId=825306)), para [4.7.1](https://go.microsoft.com/fwlink/p/?LinkId=852090), para [4.7.2](https://go.microsoft.com/fwlink/p/?LinkId=863265) ou para [4.8](https://go.microsoft.com/fwlink/?linkid=2088631). Para saber mais, veja o [Guia de instalação](../../../docs/framework/install/guide-for-developers.md) e [requisitos do sistema](../../../docs/framework/get-started/system-requirements.md) para saber quais são os sistemas operacionais compatíveis.|  
|A atualização correspondente a KB\<*número*> precisa ser instalado antes de instalar este produto.|Instalação do .NET Framework exige a instalação de uma atualização de KB antes de instalar o .NET Framework. Instale a atualização e, depois, comece novamente a instalação do .NET Framework.<br /><br /> Por exemplo, a instalação de versões atualizadas do .NET Framework no Windows 8.1, Windows RT 8.1 e Windows Server 2012 R2 exige que a atualização correspondente a [KB 2919355](https://support.microsoft.com/kb/2919355) seja instalada.|  
|O computador está executando uma instalação Núcleo do Servidor do sistema operacional Windows Server 2008. O .NET Framework 4.5.x requer uma versão mais recente do sistema operacional. Instale o Windows Server 2008 R2 SP1 ou posterior e execute novamente a instalação do .NET Framework 4.5.x.|O .NET Framework 4.5.1 e o 4.5.2 são compatíveis com a função Server Core no Windows Server 2008 R2 SP1 ou posterior. Confira [Requisitos de sistema](../../../docs/framework/get-started/system-requirements.md).|  
|Você não tem privilégios suficientes para concluir a operação para todos os usuários do computador. Faça logon como administrador e execute novamente a **Instalação**.|Você deve ser um administrador no computador para instalar o .NET Framework.|  
|A Instalação não pode continuar, pois uma instalação anterior requer que o computador seja reiniciado. Reinicie o computador e execute novamente a Instalação.|Às vezes, uma reinicialização é necessária para concluir uma instalação. Siga as instruções para reiniciar o computador e executar novamente a Instalação.<br /><br /> Em casos raros, você poderá receber uma solicitação para reiniciar o sistema mais de uma vez se o Windows detectar uma quantidade de atualizações ausentes e estiver reiniciando para instalar a próxima atualização na fila.|  
|A instalação do .NET Framework não pode ser executada no Modo de Compatibilidade de Programa.|Confira a seção [Problemas de compatibilidade de programas](#compat) mais adiante neste artigo.|  
|O .NET Framework 4.5.x/4.6.x/4.7.x/4.8 não foi instalado porque o armazenamento dos componentes foi corrompido.|Confira [Corrigir erros do Windows Update usando a ferramenta DISM ou de Preparação para Atualização do Sistema](https://support.microsoft.com/kb/947821) para saber mais.|  
|Não é possível executar a instalação porque o Serviço Windows Installer não está disponível neste computador.|Confira [Erro do Serviço Windows Installer ao instalar ou atualizar programas](https://go.microsoft.com/fwlink/p/?LinkId=248684) no site de suporte da Microsoft.|  
|A instalação pode não executar corretamente porque o Serviço Windows Update não está disponível neste computador.|O computador pode estar configurado para usar Windows Server Update Services (WSUS) em vez do Microsoft Windows Update. Para saber mais, veja a seção correspondente ao código de erro 0x800F0906 em [Códigos de erro quando você tenta instalar o .NET Framework 3.5 no Windows 8 ou no Windows Server 2012](https://support.microsoft.com/kb/2734782).<br /><br /> Veja também [Como obter a versão mais recente do Windows Update Agent para ajudar a gerenciar atualizações em um computador](https://go.microsoft.com/fwlink/p/?LinkId=248437) no site do Suporte da Microsoft.|  
|A instalação pode não executar corretamente porque o BITS (Serviço de Transferência Inteligente em Segundo Plano) não está disponível neste computador.|Confira [Uma atualização para evitar uma falha no BITS (Serviço de Transferência Inteligente em Segundo Plano) em um computador com Windows Vista](https://go.microsoft.com/fwlink/p/?LinkId=248680) no site do Suporte da Microsoft.|  
|Talvez a instalação não execute corretamente porque o Windows Update encontrou um erro e exibiu o código de erro 0x80070643 ou 0x643.|Confira [Erro de instalação de atualização do .NET Framework: "0x80070643" ou "0x643"](https://support.microsoft.com/kb/976982) no site do Suporte da Microsoft.|  
|O .NET Framework 4.5.x/4.6.x/4.7.x/4.8 já faz parte do sistema operacional. Não é necessário instalar o .NET Framework 4.5.x/4.6.x/4.7.x/4.8 redistribuível.|Nenhuma ação.<br /><br /> Para determinar quais versões do .NET Framework estão instaladas em um sistema, confira [Como: Determinar quais versões do .NET Framework estão instaladas](../../../docs/framework/migration-guide/how-to-determine-which-versions-are-installed.md). Confira [Requisitos de sistema](../../../docs/framework/get-started/system-requirements.md) para conhecer os sistemas operacionais com suporte.|  
|O .NET Framework 4.5.x/4.6.x/4.7.x/4.8 não é compatível com este sistema operacional.|Confira [Requisitos de sistema](../../../docs/framework/get-started/system-requirements.md) para conhecer os sistemas operacionais com suporte.<br /><br /> Para instalações com falha do .NET Framework no Windows 7, essa mensagem geralmente indica que o Windows 7 SP1 não está instalado. Em sistemas com Windows 7, o .NET Framework exige o Windows 7 SP1. Se você estiver no Windows 7 e ainda não tiver instalado o Service Pack 1, faça isso antes de instalar o .NET Framework. Para saber mais sobre como instalar o Windows 7 SP1, veja [Saiba como instalar o Windows 7 Service Pack 1 (SP1)](https://windows.microsoft.com/windows7/install-windows-7-service-pack-1).|  
|O computador está executando uma instalação Server Core do sistema operacional Windows Server 2008. O .NET Framework 4.5.x exige uma versão completa do sistema operacional ou Server Core 2008 R2 SP1. Instale a versão completa do Windows Server 2008 SP2, Windows Server 2008 R2 SP1 ou Server Core 2008 R2 SP1 e execute a instalação do .NET Framework 4.5.x novamente.|O .NET Framework é suportado na função Server Core no Windows Server 2008 R2 SP1 ou posterior. Confira [Requisitos de sistema](../../../docs/framework/get-started/system-requirements.md).|  
|O .NET Framework 4.5.x já faz parte do sistema operacional, mas está desativado no momento (apenas [!INCLUDE[winserver8](../../../includes/winserver8-md.md)]).|Confira [Ativar ou desativar recursos do Windows](https://go.microsoft.com/fwlink/p/?LinkId=248438) no site do Windows.|  
|Este programa de instalação requer um computador x86. Ele não pode ser instalado em computadores x64 ou IA64.|Confira [Requisitos de sistema](../../../docs/framework/get-started/system-requirements.md).|  
|Este programa de instalação requer um computador x64 ou x86. Ele não pode ser instalado em computadores IA64.|Confira [Requisitos de sistema](../../../docs/framework/get-started/system-requirements.md).|  

<a name="compat"></a>
### <a name="program-compatibility-issues"></a>Problemas de compatibilidade de programas

A instalação do .NET Framework 4.5 ou dos lançamentos pontuais falha com um código de erro 1603 ou é bloqueada quando executada no Modo de Compatibilidade de Programa do Windows. O **Auxiliar de Compatibilidade de Programas** indica que o .NET Framework pode não ter sido instalado corretamente e sugere que ele seja reinstalado usando a configuração recomendada (Modo de Compatibilidade do Programa). O Modo de Compatibilidade de Programa também pode ter sido definido pelo Auxiliar de Compatibilidade de Programas em tentativas anteriores fracassadas ou canceladas de executar o programa de instalação do .NET Framework.

O instalador do .NET Framework não pode ser executado no Modo de Compatibilidade de Programa. Para resolver esse problema de bloqueio, você deve usar o Editor do Registro para garantir que a definição do modo de compatibilidade não esteja ativada para todo o sistema:

1. Escolha o botão **Iniciar** e, em seguida, escolha **Executar**.

1. Na caixa de diálogo **Executar**, digite “regedit” e selecione **OK**.

1. No Editor do Registro, navegue para as seguintes subchaves:

   - HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\Compatibility Assistant\Persisted

   - HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\Layers

1. Na coluna Nome, procure os nomes de download do .NET Framework 4.5, 4.5.1, 4.5.2, 4.6, 4.6.1, 4.6.2, 4.7, 4.7.1 ou 4.7.2 (dependendo da versão que estiver instalando) e exclua essas entradas. Para obter os nomes de downloads, consulte o artigo [Instalar o .NET Framework para desenvolvedores](../../../docs/framework/install/guide-for-developers.md).

1. Execute novamente o instalador do .NET Framework para as versões 4.5, 4.5.1, 4.5.2, ou 4.6, 4.6.1, 4.6.2, 4.7, 4.7.1 ou 4.7.2.

## <a name="see-also"></a>Consulte também

- [Instalar o .NET Framework para desenvolvedores](../../../docs/framework/install/guide-for-developers.md)
- [Como: Determinar quais versões do .NET Framework estão instaladas](../../../docs/framework/migration-guide/how-to-determine-which-versions-are-installed.md)
- [Versões e dependências](../../../docs/framework/migration-guide/versions-and-dependencies.md)
