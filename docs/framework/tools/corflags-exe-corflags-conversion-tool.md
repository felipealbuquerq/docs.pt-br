---
title: CorFlags.exe (Ferramenta de Conversão de CorFlags)
ms.date: 03/30/2017
helpviewer_keywords:
- CorFlags conversion tool
- CorFlags.exe
- portable executable files, CorFlags section
ms.assetid: ef900f8f-71ca-4dde-9b8c-95ddb0d7d89c
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 4228da6efe22091c86de95d846c14f504d51457f
ms.sourcegitcommit: 205b9a204742e9c77256d43ac9d94c3f82909808
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70851293"
---
# <a name="corflagsexe-corflags-conversion-tool"></a>CorFlags.exe (Ferramenta de Conversão de CorFlags)
A ferramenta Conversão CorFlags permite configurar a seção CorFlags do cabeçalho de uma imagem PE (Portable Executable).  
  
 Essa ferramenta é instalada automaticamente com o Visual Studio. Para executar a ferramenta, use o Prompt de Comando do Desenvolvedor para Visual Studio (ou o Prompt de Comando do Visual Studio no Windows 7). Para obter mais informações, consulte [Prompts de Comando](../../../docs/framework/tools/developer-command-prompt-for-vs.md).  
  
 No prompt de comando, digite o seguinte:  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
CorFlags.exe assembly [options]  
```  
  
## <a name="parameters"></a>Parâmetros  
  
|Parâmetro obrigatório|Descrição|  
|------------------------|-----------------|  
|`assembly`|O nome do assembly para o qual configurar o CorFlags.|  
  
|Opção|Descrição|  
|------------|-----------------|  
|**/32BIT[REQ]+**|Define o sinalizador 32BITREQUIRED.|  
|**/32BIT[REQ]-**|Limpa o sinalizador 32BITREQUIRED.|  
|**/32BITPREF+**|Define o sinalizador 32BITPREFERRED. O aplicativo é executado como um processo 32 bits, mesmo em plataformas 64 bits. Defina esse sinalizador apenas em arquivos EXE. Se o sinalizador for definido em uma DLL, a DLL não será carregada em processos 64 bits, e uma exceção <xref:System.BadImageFormatException> será acionada. Um arquivo EXE com esse sinalizador pode ser carregado em um processo 64 bits.<br /><br /> Novidades no .NET Framework 4.5.|  
|**/32BITPREF-**|Limpa o sinalizador 32BITPREFERRED.<br /><br /> Novidades no .NET Framework 4.5.|  
|**/?**|Exibe sintaxe de comando e opções para a ferramenta.|  
|**/Force**|Força uma atualização, mesmo que o assembly tenha nome forte. **Importante:**  Se atualizar um assembly de nome forte, você deverá assiná-lo novamente antes de executar o código.|  
|**/help**|Exibe sintaxe de comando e opções para a ferramenta.|  
|**/ILONLY+**|Define o sinalizador ILONLY.|  
|**/ILONLY-**|Limpa o sinalizador ILONLY.|  
|**/nologo**|Suprime a exibição do banner de inicialização da Microsoft.|  
|**/RevertCLRHeader**|Reverte a versão do cabeçalho do CLR para 2.0.|  
|**/UpgradeCLRHeader**|Atualiza a versão do cabeçalho do CLR para 2.5. **Observação:**  Os assemblies devem ter uma versão do cabeçalho do CLR 2.5 ou posterior para serem executados nativamente.|  
  
## <a name="remarks"></a>Comentários  
 Se nenhuma opção estiver especificada, a ferramenta Conversão CorFlags exibirá os sinalizadores para o assembly especificado.  
  
## <a name="see-also"></a>Consulte também

- [Ferramentas](../../../docs/framework/tools/index.md)
- [Aplicativos de 64 bits](../../../docs/framework/64-bit-apps.md)
- [Prompts de Comando](../../../docs/framework/tools/developer-command-prompt-for-vs.md)
