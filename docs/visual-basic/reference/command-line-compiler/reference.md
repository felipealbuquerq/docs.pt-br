---
title: -referência (Visual Basic)
ms.date: 03/13/2018
helpviewer_keywords:
- /reference compiler option [Visual Basic]
- r compiler option [Visual Basic]
- -reference compiler option [Visual Basic]
- /r compiler option [Visual Basic]
- reference compiler option [Visual Basic]
- -r compiler option [Visual Basic]
ms.assetid: 66bdfced-bbf6-43d1-a554-bc0990315737
ms.openlocfilehash: 2394a23ddd59d09ce53c78fc4486fc5bae9e8516
ms.sourcegitcommit: c7a7e1468bf0fa7f7065de951d60dfc8d5ba89f5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65583365"
---
# <a name="-reference-visual-basic"></a>-referência (Visual Basic)
Faz com que o compilador disponibilizar informações de tipo nos assemblies especificados para o projeto que você está compilando.  
  
## <a name="syntax"></a>Sintaxe  
  
```  
-reference:fileList  
' -or-  
-r:fileList  
```  
  
## <a name="arguments"></a>Arguments  
  
|Termo|Definição|  
|---|---|  
|`fileList`|Necessário. Lista delimitada por vírgulas de nomes de arquivo do assembly. Se o nome do arquivo contém um espaço, coloque o nome entre aspas.|  
  
## <a name="remarks"></a>Comentários  
 Os arquivos que você importa devem conter metadados do assembly. Somente os tipos públicos são visíveis fora do assembly. O [/addmodule](../../../visual-basic/reference/command-line-compiler/addmodule.md) opção importa os metadados de um módulo.  
  
 Se você fizer referência a um assembly (Assembly A) que se faz referência a outro assembly (Assembly B), você precisa referenciar o Assembly B se:  
  
- Um tipo do Assembly A herda de um tipo ou implementa uma interface do Assembly B.  
  
- Um campo, propriedade, evento ou método que tem um tipo de retorno ou de parâmetro do Assembly B é invocado.  
  
 Use [- libpath](../../../visual-basic/reference/command-line-compiler/libpath.md) para especificar o diretório em que uma ou mais das suas referências do assembly estão localizado.  
  
 Para o compilador reconheça um tipo em um assembly (não um módulo), ele deve ser forçado a resolver o tipo. Um exemplo de como você pode fazer isso é definir uma instância do tipo. Outras maneiras de estão disponíveis para resolver nomes de tipo em um assembly para o compilador. Por exemplo, se você herdar de um tipo em um assembly, o nome do tipo, em seguida, se tornam conhecido para o compilador.  
  
 O arquivo de resposta Vbc, que referencia assemblies .NET Framework comumente usados, é usado por padrão. Use `-noconfig` se você não quiser que o compilador use Vbc.  
  
 A forma abreviada de `-reference` é `/r`.  
  
## <a name="example"></a>Exemplo  
 O comando a seguir compila o arquivo de origem `Input.vb` e fazer referência a assemblies `Metad1.dll` e `Metad2.dll` para produzir `Out.exe`.  
  
```console
vbc -reference:metad1.dll,metad2.dll -out:out.exe input.vb  
```  
  
## <a name="see-also"></a>Consulte também

- [Compilador de linha de comando do Visual Basic](../../../visual-basic/reference/command-line-compiler/index.md)
- [-noconfig](../../../visual-basic/reference/command-line-compiler/noconfig.md)
- [-target (Visual Basic)](../../../visual-basic/reference/command-line-compiler/target.md)
- [Público](../../../visual-basic/language-reference/modifiers/public.md)
- [Linhas de Comando de Compilação de Exemplo](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
