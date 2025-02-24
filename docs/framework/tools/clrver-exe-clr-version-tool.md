---
title: Clrver.exe (Ferramenta de Versão do CLR)
ms.date: 03/30/2017
helpviewer_keywords:
- Clrver.exe
- CLR Version tool
ms.assetid: cbc2ee86-bdc8-4a65-a8f1-ba23bce3a699
author: mairaw
ms.author: mairaw
ms.openlocfilehash: 2f4fc74a270cc171efa166bf54ac52a1b7acfdc5
ms.sourcegitcommit: 205b9a204742e9c77256d43ac9d94c3f82909808
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70851317"
---
# <a name="clrverexe-clr-version-tool"></a>Clrver.exe (Ferramenta de Versão do CLR)
A ferramenta Versão do CLR (Clrver.exe) relata todas as versões instaladas do CLR (Common Language Runtime) no computador.  
  
 Essa ferramenta é instalada automaticamente com o Visual Studio. Para executar a ferramenta, use o Prompt de Comando do Desenvolvedor para Visual Studio (ou o Prompt de Comando do Visual Studio no Windows 7). Para obter mais informações, consulte [Prompts de Comando](../../../docs/framework/tools/developer-command-prompt-for-vs.md).  
  
 No prompt de comando, digite o seguinte:  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
clrver [option]  
```  
  
## <a name="options"></a>Opções  
  
|Opção|Descrição|  
|------------|-----------------|  
|`-all`|Exibe todos os processos no computador que estão usando o CLR.|  
|*pid*|Exibe as versões do CLR usado pelo processo com a PID (ID de processo especificado).|  
|`-?`|Exibe sintaxe de comando e opções para a ferramenta.|  
  
## <a name="remarks"></a>Comentários  
 Se você chamar Clrver.exe sem opções, ele exibirá todas as versões do CLR instaladas. Se especificar uma PID para outro usuário, você deverá ter permissões administrativas para obter as informações da versão.  
  
> [!NOTE]
> No Windows Vista e posterior, UAC (Controle de Conta de Usuário) determina os privilégios de um usuário. Se for um membro do grupo Administradores Internos, você receberá dois tokens de acesso do tempo de execução: um token de acesso do usuário padrão e um token de acesso do administrador. Por padrão, você está na função de usuário padrão. Para executar o código que exige a permissão administrativa, você deve primeiro elevar os privilégios do usuário padrão para o administrador. Você pode fazer isso ao iniciar o prompt de comando clicando com o botão direito do mouse no ícone do prompt de comando e indicando que você deseja executar como administrador.  
  
 A tentativa de determinar a versão do CLR dos processos SYSTEM, LOCAL SERVICE e NETWORK SERVICE resulta em uma mensagem indicando que a PID não existe.  
  
## <a name="examples"></a>Exemplos  
 O comando a seguir exibe todas as versões do CLR instaladas no computador.  
  
 `clrver`  
  
 O comando a seguir exibe a versão do CLR usada pelo processo 128.  
  
 `clrver 128`  
  
 O comando a seguir exibe todos os processos gerenciados e a versão do CLR que eles estão usando.  
  
 `Clrver -all`  
  
## <a name="see-also"></a>Consulte também

- [Ferramentas](../../../docs/framework/tools/index.md)
- [Prompts de Comando](../../../docs/framework/tools/developer-command-prompt-for-vs.md)
