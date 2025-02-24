---
title: Instrução Option Strict (Visual Basic)
ms.date: 07/20/2015
f1_keywords:
- vb.Strict
- vb.OptionStrict
helpviewer_keywords:
- Strict keyword [Visual Basic]
- Option Strict statement [Visual Basic]
- conversions [Visual Basic], implicit
- late binding [Visual Basic]
- implicit conversions [Visual Basic]
ms.assetid: 5883e0c1-a920-4274-8e46-b0ff047eaee5
ms.openlocfilehash: a8466cb0672ccf26717d012bdebb7363f31ebb08
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69912266"
---
# <a name="option-strict-statement"></a>Instrução Option Strict
Restringe conversões de tipo de dados implícitos apenas a conversões de alargamento, não permite a associação tardia e não permite a digitação implícita que resulta em `Object` um tipo.  
  
## <a name="syntax"></a>Sintaxe  
  
```  
Option Strict { On | Off }  
```  
  
## <a name="parts"></a>Partes  
  
|Termo|Definição|  
|---|---|  
|`On`|Opcional. Habilita `Option Strict` a verificação.|  
|`Off`|Opcional. Desabilita `Option Strict` a verificação.|  
  
## <a name="remarks"></a>Comentários  
 Quando `Option Strict On` ou`Option Strict` aparece em um arquivo, as seguintes condições causam um erro de tempo de compilação:  
  
- Conversões de estreitamento implícitas  
  
- Associação tardia  
  
- Digitação implícita que resulta em um tipo `Object`  
  
> [!NOTE]
> Nas configurações de aviso que você pode definir na [página compilar, o designer de projeto (Visual Basic)](/visualstudio/ide/reference/compile-page-project-designer-visual-basic), há três configurações que correspondem às três condições que causam um erro de tempo de compilação. Para obter informações sobre como usar essas configurações, consulte [para definir configurações de aviso no IDE](../../../visual-basic/language-reference/statements/option-strict-statement.md#conditions) mais adiante neste tópico.  
  
 A `Option Strict Off` instrução desativa a verificação de erros e avisos para todas as três condições, mesmo que as configurações do IDE associadas especifiquem para ativar esses erros ou avisos. A `Option Strict On` instrução ativa a verificação de erros e avisos para todas as três condições, mesmo que as configurações do IDE associadas especifiquem para desativar esses erros ou avisos.  
  
 Se usado, a `Option Strict` instrução deve aparecer antes de qualquer outra instrução de código em um arquivo.  
  
 Quando você define `Option Strict` como `On`, Visual Basic verifica se os tipos de dados são especificados para todos os elementos de programação. Os tipos de dados podem ser especificados explicitamente ou especificados usando a inferência de tipo local. É recomendável especificar tipos de dados para todos os elementos de programação, pelos seguintes motivos:  
  
- Ele habilita o suporte IntelliSense para suas variáveis e parâmetros. Isso permite que você veja suas propriedades e outros membros à medida que digita o código.  
  
- Ele permite que o compilador execute a verificação de tipo. A verificação de tipo ajuda a encontrar instruções que podem falhar em tempo de execução devido a erros de conversão de tipo. Ele também identifica chamadas para métodos em objetos que não dão suporte a esses métodos.  
  
- Ele acelera a execução do código. Um motivo para isso é que se você não especificar um tipo de dados para um elemento de programação, o compilador Visual Basic atribuirá a ele `Object` o tipo. O código compilado pode precisar fazer a conversão entre `Object` e outros tipos de dados, o que reduz o desempenho.  
  
## <a name="implicit-narrowing-conversion-errors"></a>Erros de conversão de estreitamento implícito  
 Erros de conversão de redução implícita ocorrerem quando há uma conversão de tipo de dados implícita que é uma conversão de redução.  
  
 Visual Basic pode converter muitos tipos de dados em outros tipos de dados. A perda de dados pode ocorrer quando o valor de um tipo de dados é convertido em um tipo de dados que tem menos precisão ou uma capacidade menor. Um erro em tempo de execução ocorrerá se uma conversão de restrição desse tipo falhar. `Option Strict`garante a notificação em tempo de compilação dessas conversões redutoras para que você possa evitá-las. Para obter mais informações, consulte [conversões implícitas e explícitas](../../../visual-basic/programming-guide/language-features/data-types/implicit-and-explicit-conversions.md) e conversões de [alargamento e estreitamento](../../../visual-basic/programming-guide/language-features/data-types/widening-and-narrowing-conversions.md).  
  
 Conversões que podem causar erros incluem conversões implícitas que ocorrem em expressões. Para mais informações, consulte os seguintes tópicos:  
  
- [Operador +](../../../visual-basic/language-reference/operators/addition-operator.md)  
  
- [Operador +=](../../../visual-basic/language-reference/operators/addition-assignment-operator.md)  
  
- [Operador \ (Visual Basic)](../../../visual-basic/language-reference/operators/integer-division-operator.md)  
  
- [Operador/= (Visual Basic)](../../../visual-basic/language-reference/operators/floating-point-division-assignment-operator.md)  
  
- [Tipo de Dados de Caractere](../../../visual-basic/language-reference/data-types/char-data-type.md)  
  
 Quando você concatena cadeias de caracteres usando o [operador &](../../../visual-basic/language-reference/operators/concatenation-operator.md), todas as conversões para as cadeias de caracteres são consideradas como sendo ampliadas. Portanto, essas conversões não geram um erro de conversão de estreitamento implícito, `Option Strict` mesmo se houver.  
  
 Quando você chamar um método que tem um argumento que tem um tipo de dados diferente do parâmetro correspondente, uma conversão de restrição causará um erro em tempo `Option Strict` de compilação se estiver ativada. Você pode evitar o erro de tempo de compilação usando uma conversão de ampliação ou uma conversão explícita.  
  
 Os erros de conversão implícita de estreitamento são suprimidos no tempo de compilação para conversões dos elementos em `For Each…Next` uma coleção para a variável de controle de loop. Isso ocorre mesmo se `Option Strict` o estiver ativado. Para obter mais informações, consulte a seção "conversões estreitas" em [para cada... Próxima instrução](../../../visual-basic/language-reference/statements/for-each-next-statement.md).  
  
## <a name="late-binding-errors"></a>Erros de associação tardia  
 Um objeto tem associação tardia quando é atribuído a uma propriedade ou a um método de uma variável declarada como sendo do tipo `Object`. Para obter mais informações, consulte [ligação antecipada e tardia](../../../visual-basic/programming-guide/language-features/early-late-binding/index.md).  
  
## <a name="implicit-object-type-errors"></a>Erros de tipo de objeto implícito  
 Erros de tipo de objeto implícitos ocorrem quando um tipo apropriado não pode ser inferido de uma variável declarada, portanto, um tipo de `Object` é inferido. Isso ocorre principalmente quando você usa uma instrução `Dim` para declarar uma variável sem usar uma cláusula `As` e `Option Infer` está desativado. Para obter mais informações, consulte [instrução Option Infer](../../../visual-basic/language-reference/statements/option-infer-statement.md) e a [especificação da linguagem Visual Basic](../../../visual-basic/reference/language-specification/index.md).  
  
 Para parâmetros de método, `As` a cláusula será opcional `Option Strict` se for off. No entanto, se qualquer parâmetro usar `As` uma cláusula, todos eles deverão usá-lo. Se `Option Strict` for on, a `As` cláusula será necessária para cada definição de parâmetro.  
  
 Se você declarar uma variável sem usar uma `As` cláusula e defini-la `Nothing`como, a variável terá um tipo `Object`de. Nenhum erro de tempo de compilação ocorre nesse caso quando `Option Strict` o está ativado `Option Infer` e está ligado. Um exemplo disso é `Dim something = Nothing`.  
  
### <a name="default-data-types-and-values"></a>Tipos de Dados e Valores Padrão  
 A tabela a seguir descreve os resultados de várias combinações de especificando o tipo de dados e o inicializador em uma [instrução Dim](../../../visual-basic/language-reference/statements/dim-statement.md).  
  
|Tipo de dados especificado?|Inicializador especificado?|Exemplo|Resultado|  
|---|---|---|---|  
|Não|Não|`Dim qty`|Se o `Option Strict` estiver desativado (padrão), a variável é definida como `Nothing`.<br /><br /> Se `Option Strict` estiver ativado, ocorre um erro de tempo de compilação.|  
|Não|Sim|`Dim qty = 5`|Se `Option Infer` estiver ativado (padrão), a variável usa o tipo de dados do inicializador. Consulte [inferência de tipo local](../../../visual-basic/programming-guide/language-features/variables/local-type-inference.md).<br /><br /> Se `Option Infer` estiver desativado e `Option Strict` estiver desativado, a variável usa o tipo de dados do `Object`.<br /><br /> Se `Option Infer` estiver desativado e `Option Strict` estiver ativado, ocorre um erro de tempo de compilação.|  
|Sim|Não|`Dim qty As Integer`|A variável é inicializada para o valor padrão para o tipo de dados. Para obter mais informações, consulte [instrução Dim](../../../visual-basic/language-reference/statements/dim-statement.md).|  
|Sim|Sim|`Dim qty  As Integer = 5`|Se o tipo de dados do inicializador não for conversível para o tipo de dados especificado, ocorrerá um erro de tempo de compilação.|  
  
## <a name="when-an-option-strict-statement-is-not-present"></a>Quando uma instrução Option Strict não está presente  
 Se o código-fonte não contiver `Option Strict` uma instrução, a opção de configuração **estrita** na [página compilar, designer de projeto (Visual Basic)](/visualstudio/ide/reference/compile-page-project-designer-visual-basic) será usada. A **página de compilação** tem configurações que fornecem controle adicional sobre as condições que geram um erro.  
  
 Se você estiver usando o compilador de linha de comando, poderá usar a opção de compilador [/optionstrict](../../../visual-basic/reference/command-line-compiler/optionstrict.md) para especificar uma configuração `Option Strict`para.  
  
### <a name="to-set-option-strict-in-the-ide"></a>Para definir Option Strict no IDE  
[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]  
  
1. No **Gerenciador de Soluções**, selecione um projeto. No menu **Projeto**, clique em **Propriedades**.  
  
2. Na guia **Compilar** , defina o valor na caixa **opção estrita** .  
  
### <a name="conditions"></a>Para definir as configurações de aviso no IDE  
 Quando você usa a [página compilar, o designer de projeto (Visual Basic)](/visualstudio/ide/reference/compile-page-project-designer-visual-basic) em `Option Strict` vez de uma instrução, você tem controle adicional sobre as condições que geram erros. A seção **configurações de aviso** da **página compilar** tem configurações que correspondem às três condições que causam um erro de tempo de compilação quando `Option Strict` o está ativado. A seguir estão estas configurações:  
  
- **Conversão implícita**  
  
- **Associação tardia; a chamada poderia falhar no tempo de execução**  
  
- **Tipo implícito; objeto assumido**  
  
 Quando você define **Opção Estrita** como **Ativada**, todas estas três definições de configuração de aviso são definidas como **Erro**. Quando você define **Opção Estrita** como **Desativada**, todas as três configurações são definidas como **Nenhum**.  
  
 Você pode alterar individualmente cada definição de configuração de aviso como **Nenhum**, **Aviso** ou **Erro**. Se todas as três definições de configuração de aviso estiverem definidas como **Erro**, `On` aparecerá na caixa `Option strict`. Se todas as três estiverem definidas como **Nenhum**, `Off` será exibido nessa caixa. Para qualquer outra combinação dessas configurações, **(personalizado)** será exibido.  
  
### <a name="to-set-the-option-strict-default-setting-for-new-projects"></a>Para definir a opção de configuração padrão estrita para novos projetos  
 Quando você cria um projeto, a **opção configuração estrita** na guia **Compilar** é definida como a **opção configuração estrita** na caixa de diálogo **Opções** .  
  
 Para definir `Option Strict` nessa caixa de diálogo, no menu **ferramentas** , clique em **Opções**. Na caixa de diálogo **Opções**, expanda **Projetos e Soluções** e, em seguida, clique em **Padrões de VB**. A configuração padrão inicial em **padrões do VB** é `Off`.  
  
### <a name="to-set-option-strict-on-the-command-line"></a>Para definir Option Strict na linha de comando  
 Inclua a opção de compilador [/optionstrict](../../../visual-basic/reference/command-line-compiler/optionstrict.md) no comando **Vbc** .  
  
## <a name="example"></a>Exemplo  
 Os exemplos a seguir demonstram erros de tempo de compilação causados por conversões implícitas de tipo que são conversões redutoras. Essa categoria de erros corresponde à condição de **conversão implícita** na **página de compilação**.  
  
 [!code-vb[VbVbalrStatements#161](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class13.vb#161)]  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir demonstra um erro de tempo de compilação causado pela associação tardia. Essa categoria de erros corresponde à **associação tardia; a chamada pode falhar em** condição de tempo de execução na **página de compilação**.  
  
 [!code-vb[VbVbalrStatements#162](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class13.vb#162)]  
  
## <a name="example"></a>Exemplo  
 Os exemplos a seguir demonstram erros causados por variáveis que são declaradas com um tipo implícito de `Object`. Essa categoria de erros corresponde ao **tipo implícito; objeto assumido** condição na **página de compilação**.  
  
 [!code-vb[VbVbalrStatements#163](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class13.vb#163)]  
  
 [!code-vb[VbVbalrStatements#164](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class13.vb#164)]  
  
## <a name="see-also"></a>Consulte também

- [Conversões de Widening e Narrowing](../../../visual-basic/programming-guide/language-features/data-types/widening-and-narrowing-conversions.md)
- [Conversões Implícitas e Explícitas](../../../visual-basic/programming-guide/language-features/data-types/implicit-and-explicit-conversions.md)
- [Página de Compilação, Designer de Projeto (Visual Basic)](/visualstudio/ide/reference/compile-page-project-designer-visual-basic)
- [Instrução Option Explicit](../../../visual-basic/language-reference/statements/option-explicit-statement.md)
- [Funções de Conversão do Tipo](../../../visual-basic/language-reference/functions/type-conversion-functions.md)
- [Como: Acessar membros de um objeto](../../../visual-basic/programming-guide/language-features/variables/how-to-access-members-of-an-object.md)
- [Expressões Inseridas no XML](../../../visual-basic/programming-guide/language-features/xml/embedded-expressions-in-xml.md)
- [Conversão de Delegado Reduzida](../../../visual-basic/programming-guide/language-features/delegates/relaxed-delegate-conversion.md)
- [Vinculação tardia em soluções do Office](/visualstudio/vsto/late-binding-in-office-solutions)
- [/optionstrict](../../../visual-basic/reference/command-line-compiler/optionstrict.md)
- [Caixa de diálogo Padrões do Visual Basic, Projetos, Opções](/visualstudio/ide/reference/visual-basic-defaults-projects-options-dialog-box)
