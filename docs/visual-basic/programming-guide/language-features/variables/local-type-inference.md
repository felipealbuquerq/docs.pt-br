---
title: Inferência de tipo local (Visual Basic)
ms.date: 07/20/2015
f1_keywords:
- local type inference
- vb.TypeInfer
helpviewer_keywords:
- Option Infer statement [Visual Basic]
- local type inference [Visual Basic]
- implicitly-typed local variables [Visual Basic]
- variables [Visual Basic], type inference
- inference [Visual Basic]
- type inference [Visual Basic]
ms.assetid: b8307f18-2e56-4ab3-a45a-826873f400f6
ms.openlocfilehash: 59559f8775a5fd66a567897b009272df1727b1e8
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69953324"
---
# <a name="local-type-inference-visual-basic"></a>Inferência de tipo local (Visual Basic)
O compilador de Visual Basic usa a inferência de *tipos* para determinar os tipos de dados de `As` variáveis locais declarados sem uma cláusula. O compilador infere o tipo da variável do tipo da expressão de inicialização. Isso permite que você declare variáveis sem explicitamente indicar um tipo, conforme mostrado no exemplo a seguir. Como resultado das declarações, ambos `num1` e `num2` são fortemente tipados como inteiros.  
  
 [!code-vb[VbVbalrTypeInference#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrTypeInference/VB/Class1.vb#1)]  
 
> [!NOTE]
> Se `num2` você não quiser que o exemplo anterior seja digitado como um `Integer`, você pode especificar outro tipo usando uma declaração como `Dim num3 As Object = 3` ou `Dim num4 As Double = 3`.  

> [!NOTE]
> A inferência de tipos pode ser usada somente para variáveis locais não estáticas; Ele não pode ser usado para determinar o tipo de campos de classe, propriedades ou funções.
 
 A inferência de tipo local se aplica ao nível de procedimento. Ele não pode ser usado para declarar variáveis no nível do módulo (dentro de uma classe, estrutura, módulo ou interface, mas não dentro de um procedimento ou bloco). Se `num2` , no exemplo anterior, fosse um campo de uma classe em vez de uma variável local em um procedimento, a declaração causaria `Option Strict` um erro com on `Object` e `num2` classificaria como `Option Strict` off. Da mesma forma, a inferência de tipo local não se aplica a `Static`variáveis de nível de procedimento declaradas como.  
  
## <a name="type-inference-vs-late-binding"></a>Inferência de tipos vs. Associação tardia  
 O código que usa a inferência de tipos assemelha-se ao código que depende da ligação tardia. No entanto, a inferência de tipos digita fortemente a variável `Object`em vez de deixá-la como. O compilador usa o inicializador de uma variável para determinar o tipo da variável em tempo de compilação para produzir código de ligação antecipada. No exemplo anterior, `num2`, like `num1`, é digitado como um `Integer`.  
  
 O comportamento das variáveis de ligação antecipada difere do que as variáveis de associação tardia, para as quais o tipo é conhecido somente em tempo de execução. Saber que o tipo inicialmente habilita o compilador a identificar problemas antes da execução, alocar a memória com precisão e executar outras otimizações. A ligação antecipada também permite que o Visual Basic IDE (ambiente de desenvolvimento integrado) forneça ajuda IntelliSense sobre os membros de um objeto. A ligação antecipada também é preferida para o desempenho. Isso ocorre porque todos os dados armazenados em uma variável de associação tardia devem ser encapsulados `Object`como tipo e o acesso a membros do tipo em tempo de execução torna o programa mais lento.  
  
## <a name="examples"></a>Exemplos  
 A inferência de tipos ocorre quando uma variável local é `As` declarada sem uma cláusula e inicializada. O compilador usa o tipo de valor inicial atribuído como o tipo da variável. Por exemplo, cada uma das linhas de código a seguir declara uma variável do tipo `String`.  
  
 [!code-vb[VbVbalrTypeInference#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrTypeInference/VB/Class1.vb#2)]  
  
 O código a seguir demonstra duas maneiras equivalentes de criar uma matriz de inteiros.  
  
 [!code-vb[VbVbalrTypeInference#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrTypeInference/VB/Class1.vb#3)]  
  
 É conveniente usar a inferência de tipos para determinar o tipo de uma variável de controle de loop. No código a seguir, o compilador infere que `number` é um `Integer` porque `someNumbers2` do exemplo anterior é uma matriz de inteiros.  
  
 [!code-vb[VbVbalrTypeInference#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrTypeInference/VB/Class1.vb#4)]  
  
 A inferência de tipo local pode `Using` ser usada em instruções para estabelecer o tipo do nome do recurso, como demonstra o exemplo a seguir.  
  
 [!code-vb[VbVbalrTypeInference#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrTypeInference/VB/Class1.vb#7)]  
  
 O tipo de uma variável também pode ser deduzido dos valores retornados das funções, como demonstra o exemplo a seguir. Ambos `pList1` `Process.GetProcesses` e `pList2` são matrizes de processos porque retorna uma matriz de processos.  
  
 [!code-vb[VbVbalrTypeInference#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrTypeInference/VB/Class1.vb#5)]  
  
## <a name="option-infer"></a>Opção Infer  
 `Option Infer`permite que você especifique se a inferência de tipo local é permitida em um arquivo específico. Para habilitar ou bloquear a opção, digite uma das instruções a seguir no início do arquivo.  
  
 `Option Infer On`  
  
 `Option Infer Off`  
  
 Se você não especificar um valor para `Option Infer` em seu código, o padrão do compilador será. `Option Infer On` 
  
 Se o valor definido para `Option Infer` em um arquivo entrar em conflito com o valor definido no IDE ou na linha de comando, o valor no arquivo possui precedência.  
  
 Para obter mais informações, consulte [instrução Option Infer](../../../../visual-basic/language-reference/statements/option-infer-statement.md) e a [página de compilação, designer de projeto (Visual Basic)](/visualstudio/ide/reference/compile-page-project-designer-visual-basic).  
  
## <a name="see-also"></a>Consulte também

- [Tipos Anônimos](../../../../visual-basic/programming-guide/language-features/objects-and-classes/anonymous-types.md)
- [Associação Antecipada e Tardia](../../../../visual-basic/programming-guide/language-features/early-late-binding/index.md)
- [Instrução For Each...Next](../../../../visual-basic/language-reference/statements/for-each-next-statement.md)
- [Instrução For...Next](../../../../visual-basic/language-reference/statements/for-next-statement.md)
- [Instrução Option Infer](../../../../visual-basic/language-reference/statements/option-infer-statement.md)
- [/optioninfer](../../../../visual-basic/reference/command-line-compiler/optioninfer.md)
- [Introdução ao LINQ no Visual Basic](../../../../visual-basic/programming-guide/language-features/linq/introduction-to-linq.md)
