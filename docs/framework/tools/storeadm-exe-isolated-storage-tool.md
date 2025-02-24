---
title: Storeadm.exe (Ferramenta de Armazenamento Isolado)
ms.date: 03/30/2017
helpviewer_keywords:
- Storeadm.exe
- listing stores for current user
- Isolated Storage tool
- stores, current user
- removing stores
ms.assetid: b81202b8-d91d-4b23-9c53-4a112f74a44a
author: mairaw
ms.author: mairaw
ms.openlocfilehash: 4b947ea2bfabe1c3fa9afb43cb5ecc41ab92be89
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69929900"
---
# <a name="storeadmexe-isolated-storage-tool"></a>Storeadm.exe (Ferramenta de Armazenamento Isolado)
A ferramenta Armazenamento Isolado lista ou remove todos os repositórios existentes para o usuário atual.  
  
 Essa ferramenta é instalada automaticamente com o Visual Studio. Para executar a ferramenta, use o Prompt de Comando do Desenvolvedor para Visual Studio (ou o Prompt de Comando do Visual Studio no Windows 7). Para obter mais informações, consulte [Prompts de Comando](../../../docs/framework/tools/developer-command-prompt-for-vs.md).  
  
 No prompt de comando, digite o seguinte:  
  
## <a name="syntax"></a>Sintaxe  
  
```  
storeadm [/list][/machine][/remove][/roaming][/quiet]  
```  
  
## <a name="parameters"></a>Parâmetros  
  
|Opção|DESCRIÇÃO|  
|------------|-----------------|  
|**/h**[**elp**]|Exibe sintaxe de comando e opções para a ferramenta.|  
|**/list**|Exibe todos os repositórios existentes para o usuário atual. Isso inclui os repositórios de todos os aplicativos ou assemblies executados por esse usuário.|  
|**/machine**|Seleciona o repositório do computador. Use essa opção com as opções **/list** ou **/remove** para especificar que a ação deve ser aplicada ao repositório do computador.<br /><br /> Novidades no .NET Framework 2.0|  
|**/quiet**|Especifica o modo silencioso; suprime a saída informativa de forma que apenas as mensagens de erro sejam exibidas.|  
|**/remove**|Remove permanentemente todos os repositórios existentes para o usuário atual.|  
|**/roaming**|Seleciona o repositório móvel. Use essa opção com as opções **/list** ou **/remove** para especificar que a ação deve ser aplicada ao repositório móvel.|  
|**/?**|Exibe sintaxe de comando e opções para a ferramenta.|  
  
## <a name="remarks"></a>Comentários  
 A execução de Storeadm.exe na linha de comando sem especificar nenhuma opção exibe a sintaxe e as opções da ferramenta.  
  
 As opções **/list** e **/remove** costumam ser usadas uma de cada vez; no entanto, se duas ou mais opções forem especificadas, elas serão realizadas na ordem em que são exibidas na linha de comando.  
  
 Os aplicativos têm uma opção de gravação em um dos dois repositórios para um usuário ou no repositório do computador:  
  
- O repositório local existe em um local que tem garantia de não ser compatível com roaming (no Windows 2000 e posteriores), mesmo se o roaming de dados do usuário estiver habilitado para o usuário.  
  
- O repositório móvel existe em um local compatível com roaming, mas só poderá fazer isso se o roaming estiver habilitado para o usuário por meio da administração do Windows NT.  
  
- O repositório do computador é comum a todos os usuários em um computador e é armazenado em um diretório comum nesse computador.  
  
    > [!NOTE]
    > O repositório do computador é novo na versão 2.0 do .NET Framework.  
  
 Independentemente do roaming estar efetivamente habilitado para o usuário, isso não afeta a administração de Storeadm.exe. A execução da ferramenta sem opções se aplica a todas as ações no repositório local. A execução da ferramenta com a opção **/roaming** se aplica a todas as ações no repositório compatível com roaming. A execução da ferramenta com a opção **/machine** se aplica a todas as ações no repositório do computador.  
  
## <a name="see-also"></a>Consulte também

- [Ferramentas](../../../docs/framework/tools/index.md)
- [Armazenamentos isolado](../../standard/io/isolated-storage.md)
- [Prompts de Comando](../../../docs/framework/tools/developer-command-prompt-for-vs.md)
