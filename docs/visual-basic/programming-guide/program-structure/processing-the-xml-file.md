---
title: Processando o arquivo XML (Visual Basic)
ms.date: 07/20/2015
helpviewer_keywords:
- XML comments [Visual Basic], parsing [Visual Basic]
ms.assetid: 78a15cd0-7708-4e79-85d1-c154b7a14a8c
ms.openlocfilehash: ab05db770f312a362e26f17df684f6f4f49c0eb3
ms.sourcegitcommit: c7a7e1468bf0fa7f7065de951d60dfc8d5ba89f5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65586744"
---
# <a name="processing-the-xml-file-visual-basic"></a>Processando o arquivo XML (Visual Basic)
O compilador gera uma cadeia de identificação para cada constructo no seu código marcado para gerar a documentação. (Para obter informações sobre como marcar seu código, consulte [marcas de comentário XML](../../../visual-basic/language-reference/xmldoc/index.md).) A cadeia de identificação identifica exclusivamente o constructo. Programas que processam o arquivo XML podem usar a cadeia de caracteres de ID para identificar o item de metadados/reflexão do .NET Framework correspondente.  
  
 O arquivo XML não é uma representação hierárquica de seu código; é uma lista simples com uma ID gerada para cada elemento.  
  
 O compilador observa as seguintes regras quando gera as cadeias de identificação:  
  
- Nenhum espaço em branco é colocado na cadeia de caracteres.  
  
- A primeira parte da cadeia de identificação identifica o tipo de membro que está sendo identificado, com um único caractere seguido por dois-pontos. Os seguintes tipos de membro são usados.  
  
|Caractere|Descrição|  
|---|---|  
|N|namespace<br /><br /> Não é possível adicionar comentários de documentação a um namespace, mas você pode fazer referências CREF a eles, onde houver suporte.|  
|T|type: `Class`, `Module`, `Interface`, `Structure`, `Enum`, `Delegate`|  
|F|field: `Dim`|  
|P|propriedade: `Property` (incluindo propriedades padrão)|  
|M|método: `Sub`, `Function`, `Declare`, `Operator`|  
|E|event: `Event`|  
|!|cadeia de caracteres de erro<br /><br /> O restante da cadeia de caracteres fornece informações sobre o erro. O compilador do Visual Basic gera informações de erro para links que não pode ser resolvido.|  
  
- A segunda parte do `String` é o nome totalmente qualificado do item, iniciando na raiz do namespace. O nome do item, seu tipo delimitador (s) e o namespace são separados por pontos. Se o nome do próprio item contiver períodos, eles serão substituídos pelo sinal numérico (#). Supõe-se que nenhum item tem um sinal de número diretamente em seu nome. Por exemplo, o nome totalmente qualificado do `String` construtor seria `System.String.#ctor`.  
  
- Para propriedades e métodos, se houver argumentos para o método, seguirá a lista de argumentos entre parênteses. Se não houver nenhum argumento, não haverá parênteses. Os argumentos são separados por vírgulas. A codificação de cada argumento segue diretamente como ela é codificada em uma assinatura do .NET Framework.  
  
## <a name="example"></a>Exemplo  
 O código a seguir mostra como as cadeias de identificação de uma classe e seus membros são gerados.  
  
 [!code-vb[VbVbcnXmlDocComments#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnXmlDocComments/VB/Class1.vb#10)]  
  
## <a name="see-also"></a>Consulte também

- [/doc](../../../visual-basic/reference/command-line-compiler/doc.md)
- [Como: Criar documentação XML](../../../visual-basic/programming-guide/program-structure/how-to-create-xml-documentation.md)
